---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: business network, Starter Plan, Enterprise Plan, developer environment, certificate authority card, admin business network card, BNA, business network archive

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:gif: data-image-type='gif'}

# 在入门套餐和企业套餐上部署业务网络
{: #deploying-a-business-network}

{{site.data.keyword.IBM}} 不支持将 Hyperledger Composer 用于生产的网络，包括 Composer CLI、JavaScript API、REST 服务器和 Web Playground。
{:note}

可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 开发者环境和 Hyperledger Composer 开发者工具集来开发[业务网络](/docs/services/blockchain?topic=blockchain-glossary#glossary-business-network)，并将其部署到入门套餐和企业套餐网络。
{:shortdesc}

通过使用开发者环境，可以快速对区块链业务网络进行建模和测试，然后将其部署到 {{site.data.keyword.blockchainfull_notm}} Platform 的入门套餐或企业套餐网络。

## 将业务网络部署到入门套餐
{: #deploying-a-business-network-starter-plan}

### 开始之前
{: #deploying-a-business-network-before-begin}

确保您已阅读[关于入门套餐](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about)，并已遵循[入门套餐入门](/docs/services/blockchain?topic=blockchain-getting-started-with-starter-plan#getting-started-with-starter-plan)中的指示信息创建了入门套餐网络。

确保您已安装 Node V8.9 或更高版本、npm V5.x 以及 Hyperledger Composer：

- 如果您的网络位于 Fabric V1.2 上，请使用 Hyperledger Composer V0.20.x。
- 如果您的网络位于 Fabric V1.1 上，请使用 Hyperledger Composer V0.19.x。

可以通过打开“网络监视器”中的[“网络首选项”窗口](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-network-preferences)来找到 Fabric 版本。

### 步骤 1：检索管理员私钥
{: #deploying-a-business-network-retrieve-admin-secret}

1. 在入门套餐的“概述”屏幕中，单击**连接概要文件**，然后进行下载。将此文件重命名为“connection-profile.json”。

2. 将此文件移至与 `.bna` 文件相同的目录中。

3. 在连接概要文件中，一直向下直至看到“registrar”。在“registrar”中的“enrollId”下方，有一个 **enrollSecret** 属性。检索到该私钥并保存其副本。

    ![检索管理密钥](images/get_enroll_secret.gif "检索管理密钥"){: gif}

### 步骤 2：创建认证中心卡
{: #deploying-a-business-network-CA-card}

在上一步中检索到的私钥将用于为认证中心 (CA) 创建业务网络卡。这将导入 CA 卡，并且该卡用于将 **enrollSecret** 交换为入门套餐认证中心的有效证书。

1. 使用第一步中记录的 **enrollSecret**，运行以下命令来创建 CA 业务网络卡：

   ```
   composer card create -f ca.card -p connection-profile.json -u admin -s enrollSecret
   ```
   {:pre}

  将以上命令中的 `enrollSecret` 替换为从连接概要文件检索到的管理私钥。

2. 使用以下命令来导入该卡：

   ```
   composer card import -f ca.card -c ca
   ```
   {:codeblock}

3. 现在，该卡已导入，可以用于将 **enrollSecret** 交换为 CA 的有效证书。运行以下命令向认证中心请求证书：

   ```
   composer identity request --card ca --path ./credentials -u admin -s enrollSecret
   ```
   {:codeblock}

  将以上命令中的 `enrollSecret` 替换为从连接概要文件检索到的管理私钥。`composer identity request` 命令会创建包含证书 `.pem` 文件的 `credentials` 目录。

### 步骤 3：向入门套餐实例添加证书
{: #deploying-a-business-network-add-certs-to-starter-plan}

必须向入门套餐网络添加证书。为了方便起见，可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 的“网络监视器”来添加证书。必须添加证书，随后必须重新启动同级，然后必须在通道上同步证书。所需证书是先前命令生成的 `admin-pub.pem` 文件，其位于 `credentials` 目录中。

1. 在入门套餐的“网络监视器”中，依次单击**成员**选项卡、**证书**和**添加证书**。转至 `credentials` 目录，复制 `admin-pub.pem` 文件的内容并粘贴到证书框中。提交证书，然后重新启动同级。注：重新启动同级需要 1 分钟。

    ![添加证书](images/add_cert.gif "添加证书"){: gif}

2. 接下来，必须在通道上同步证书。单击**通道**选项卡，随后单击**操作**按钮，然后是**同步证书**和**提交**。

    ![同步证书](images/sync_cert.gif "同步证书"){: gif}

### 步骤 4：创建管理业务网络卡
{: #deploying-a-business-network-create-admin-card}

现在，正确的证书已经与同级同步，因此可以创建有权安装 Hyperledger Composer 运行时并启动链代码的业务网络卡。

1. 使用以下命令来创建具有通道管理和同级管理角色的管理卡：

   ```
   composer card create -f adminCard.card -p connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin
   ```
   {:codeblock}

   此卡用于将业务网络部署到入门套餐。

2. 使用以下命令导入在上一步中创建的卡：

   ```
   composer card import -f adminCard.card -c adminCard
   ```
   {:codeblock}

### 步骤 5：安装并启动业务网络
{: #deploying-a-business-network-install-start-network}

接下来，可以使用上一步中创建的卡来安装并启动业务网络。对于本指南，我们将安装车辆制造网络样本，使用车辆制造网络样本，或者安装您自己的业务网络，但是确保更改命令中指定的业务网络名称。用于启动业务网络的命令也会创建一个卡。对于入门套餐，必须删除此卡；给定的示例命令将此卡命名为 `delete_me.card`，因此可以轻松认出该卡。

1. 使用以下命令来安装 Hyperledger Composer 运行时：

   ```
   composer network install -c adminCard -a vehicle-manufacture-network.bna
   ```
   {:codeblock}

   请记录运行此命令时返回的业务网络版本号。在下一步中将需要此项。

2. 使用以下命令启动业务网络。如果发生错误，请等待一分钟，然后重试。在 `-V` 选项后使用来自上一步的版本号。

    ```
composer network start -c adminCard -n vehicle-manufacture-network -V 0.0.1 -A admin -C ./credentials/admin-pub.pem -f delete_me.card
    ```
    {:codeblock}

3. 删除名为 `delete_me.card` 的业务网络卡。

4. 使用以下命令来创建新的业务网络卡，并引用早先时候检索到的证书：

   ```
   composer card create -n vehicle-manufacture-network -p connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem
   ```
   {:codeblock}

5. 使用以下命令来导入业务网络卡：

    ```
composer card import -f ./admin@vehicle-manufacture-network.card
    ```
    {:codeblock}

现在，业务网络已部署到入门套餐实例。

### 步骤 6：对业务网络执行 ping 操作以确保它正常运行
{: #deploying-a-business-network-ping-business-network}

运行以下命令以对业务网络执行 ping 操作：

   ```
   composer network ping -c admin@vehicle-manufacture-network
   ```
   {:codeblock}

要查看链代码日志，请单击**通道**，然后选择通道。<!-- Click the dropdown arrow to view the logs, or the Actions symbol to view in more detail. -->单击**链代码**选项卡。展开链代码行，然后单击 **JSON** 或**日志**按钮。

<!-- [fN-Yuj](https://i.makeagif.com/media/4-13-2018/fN-Yuj.gif) -->

现在，业务网络已成功部署到入门套餐实例。

## 将业务网络部署到企业套餐
{: #deploying-a-business-network-to-enterprise-plan}

{{site.data.keyword.blockchainfull_notm}} Platform 的开发者工具可帮助您创建**业务网络定义**，然后可将其打包成业务网络归档 (`.bna`)。开发者环境支持您将 `.bna` 文件部署到本地或云 {{site.data.keyword.blockchain}} 以进行开发和共享。

本教程将讨论业务网络生命周期的下一步，即通过将 `.bna` 部署到 {{site.data.keyword.blockchainfull_notm}} Platform 企业套餐来激活业务网络。

### 开始之前
{: #deploying-a-business-network-enterprise-plan-before-begin}

确保已安装 {{site.data.keyword.blockchainfull_notm}} 开发者环境，并已熟悉开发和部署业务网络。[Hyperledger Composer 文档](https://hyperledger.github.io/composer/latest/business-network/business-network-index)中提供了有关编写业务网络的指导信息。

您需要访问 {{site.data.keyword.blockchainfull_notm}} Platform 的企业套餐实例，并预先创建您的同级。有关 {{site.data.keyword.blockchainfull_notm}} Platform 企业套餐的更多信息，请参阅[企业套餐概述](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about)。

### 步骤 1：为 {{site.data.keyword.blockchainfull_notm}} Platform 创建连接概要文件
{: #deploying-a-business-network-create-connection-profile}

1. 创建用于存储连接详细信息的目录，例如：

    ```
    /Users/myUserId/.composer-connection-profiles/bmx-hlfv11
    ```
    {:codeblock}

    每个连接概要文件都应该包含 `connection.json` 文件。在 `.composer-connection-profiles` 下创建一个新目录，在本例中为 `bmx-hlfv11`。这将是您在使用 Hyperledger Composer 和 {{site.data.keyword.blockchainfull_notm}} Platform 时要使用的概要文件的名称。

    ```
    mkdir -p ~/.composer-connection-profiles/bmx-hlfv11
    cd ~/.composer-connection-profiles/bmx-hlfv11
    ```
    {:codeblock}

    现在，您应该具有以下目录结构：

    ```
    /Users/myUserId/.composer-connection-profiles/bmx-hlfv11
    ```

### 步骤 2：检索连接概要文件
{: #deploying-a-business-network-retrieve-connection-profile}

1. 从 {{site.data.keyword.blockchainfull_notm}} Platform 仪表板，选择**概述**，然后选择**连接概要文件**和**下载**按钮以检索连接概要文件。

2. 将连接概要文件保存到上一步中所创建的目录结构中。将其命名为 **connection.json**。

### 步骤 3：添加通道信息
{: #deploying-a-business-network-add-channel-information}

1. 在 `connection.json` 中添加通道值，以匹配您计划创建和部署业务网络的通道的名称。在提供的示例连接概要文件模板中，通道元素如下所示：`"channels": {},`。

### 步骤 4：准备同级
{: #deploying-a-business-network-prepare-peers}

**必须**在创建要将业务网络部署到的通道之前执行此步骤。如果在通道创建后执行此步骤，部署的业务网络**可能无法启动**。
{:note}

在“连接概要文件”文档中，**certificateAuthorities** 下是 **registrar** 属性，包含 **enrollId** 和 **enrollSecret** 属性，格式如下：

  ```
"registrar": [
            {
                "enrollId": "admin",
                "enrollSecret": "PA55W0RD12"
            }
        ],
  ```

1. 使用以下命令为 CA 创建一个业务网络卡：

    ```
    composer card create -f ca.card -p bmx-hlfv11 -u admin -s PA55W0RD12
    ```
    {:codeblock}

  确保您正在连接概要文件所在的同一目录中运行该命令。

2. 使用以下命令来导入业务网络卡：

    ```
   composer card import -f ca.card -c ca
   ```
    {:codeblock}

3. 导入卡后，可以将其用于通过以下命令从认证中心获取证书：

    ```
    composer identity request --card ca --path ./credentials -u admin -s PA55W0RD12
    ```
    {:codeblock}

    `composer identity request` 命令会创建包含相关证书文件的凭证目录。

4. 从导航菜单中选择**成员**，然后选择**证书**菜单选项，并单击**添加证书**按钮。

5. 在**名称**字段中输入此证书的唯一名称，此名称不能包含破折号或连字符。

6. 打开早些时候创建的 `admin-pub.pem` 文件，将此文件的内容复制到 **Key** 字段中，然后按**提交**按钮。

7. 使用用户界面停止同级，然后启动同级。

8. 现在，该证书应该显示在证书列表中。

### 步骤 5：创建通道
{: #deploying-a-business-network-create-your-channel}

1. 从左侧面板上的导航菜单中选择**通道**，然后单击**新建通道**按钮。

2. 输入“通道名称”和可选的描述，然后单击**下一步**。请注意，通道名称必须与您在连接概要文件中为 channel 属性指定的名称相匹配。

3. 根据需要授予许可权，然后单击**下一步**。

4. 选择需要接受通道更新并提交请求的操作员数量的策略。

5. 您现在应该转至**通知**部分，在其中应该有新的请求要复查。单击**复查请求**按钮。

6. 单击**接受**按钮后，您将重新转至**通知**部分，在其中可以看到现在可以提交该请求。单击**提交请求**按钮以打开提交对话框，然后单击**提交**按钮。新的通道已创建。

7. 从导航菜单中选择**通道**。新通道应该位于通道列表中，并且应该具有“尚未添加同级”。单击其旁边的“操作”菜单，并选择**加入同级**。选中要添加的同级旁边的复选框，然后按**添加所选项**。

### 步骤 6：创建新身份以管理业务网络
{: #deploying-a-business-network-add-new-identity-enterprise-plan}

使用所请求的证书创建业务网络卡。此业务网络卡将有权将链代码安装到具有已上传公共证书的同级，并且这些同级将是认证中心的签发者。

1. 要创建卡，请运行以下命令：

    ```
    composer card create -f adminCard.card -p bmx-hlfv11 -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin
    ```
    {:codeblock}

    其中，`bmx-hlfv11` 是先前创建的概要文件的名称。

2. 创建卡后，使用以下命令将其导入：

    ```
   composer card import -f adminCard.card -c adminCard
   ```
    {:codeblock}

    现在我们已准备好将 `.bna` 文件部署到 {{site.data.keyword.blockchainfull_notm}} Platform。


### 步骤 7：部署业务网络
{: #deploying-a-business-network-deploy-business-network-enterprise-plan}

现在，您可以将 `.bna` 文件部署到 {{site.data.keyword.blockchainfull_notm}} Platform。

1. 要使用先前步骤中创建的卡，您需要首先安装业务网络，然后将其启动。使用以下命令安装业务网络：

   ```
   composer network install -c adminCard -a myNetwork.bna
   ```
   {:codeblock}

2. 安装业务网络后，使用以下命令来启动业务网络：

    ```
    composer network start -c adminCard -n myNetwork -V networkVersion -A admin -C ./credentials/admin-pub.pem -f delete_me.card
    ```
    {:codeblock}

3. 要检查是否正确部署了业务网络，请创建可用于对网络执行 ping 操作的身份和关联的业务网络卡。

    ```
    composer card create -f admin.card -p bmx-hlfv11 -u admin -n myNetwork -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem
    ```
    {:codeblock}

4. 使用以下命令来导入业务网络卡：

    ```
    composer card import -f admin.card
    ```
    {:codeblock}

5. 对网络执行 ping 操作以检查网络是否正在运行：

    ```
    composer network ping -c admin@myNetwork
    ```
    {:codeblock}
