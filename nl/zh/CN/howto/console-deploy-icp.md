---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-21"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# 部署 {{site.data.keyword.blockchainfull_notm}} 控制台
{: #console-deploy-icp}

安装了 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart 后，请使用以下指示信息在本地 {{site.data.keyword.cloud_notm}} Private 集群上部署 {{site.data.keyword.blockchainfull}} Platform 控制台。
{:shortdesc}

## 需要的资源
{: #console-deploy-icp-resources-required}

确保 {{site.data.keyword.cloud_notm}} Private 系统满足控制台以及创建的组件的最低硬件资源需求。根据您的基础架构、网络设计和性能需求，需要的 vCPU/CPU 数可能会有所不同。可以通过检查 {{site.data.keyword.cloud_notm}} 的[缺省资源分配表](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default)来大致估计 vCPU/CPU 需求，不过 {{site.data.keyword.cloud_notm}} Private 中的分配会略有不同。部署节点时，实际资源分配会显示在区块链控制台中。

**注：**  

- vCPU 是分配给虚拟机的虚拟核心，或者如果服务器没有针对虚拟机进行分区，那么 vCPU 是物理处理器核心。决定用于 {{site.data.keyword.cloud_notm}} Private 中您的部署的虚拟处理器核心 (VPC) 时，需要考虑 vCPU 需求。VPC 是确定 {{site.data.keyword.IBM_notm}} 产品许可成本的计量单位。有关决定 VPC 的场景的更多信息，请参阅 [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}。
- vCPU 相当于一个 CPU，后者相当于一个 VPC。

## 存储
{: #console-deploy-icp-storage}

{{site.data.keyword.blockchainfull_notm}} Helm chart 使用动态供应来供应将由控制台使用的存储器以及将创建的区块链组件。在部署控制台之前，需要创建具有足够备用存储量可用于控制台和组件的 storageClass。您需要提供在配置期间创建的 storageClass 的名称。

- 如果使用缺省设置，Helm chart 将使用 Helm 发布名称创建一个新的持久卷声明，以用于存储控制台数据。


## 部署控制台的先决条件
{: #console-deploy-icp-prerequisites}

1. 必须先[安装 {{site.data.keyword.cloud_notm}}Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup) 并[安装 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console_helm-install)，然后才能部署 {{site.data.keyword.blockchainfull_notm}} Platform 控制台。

2. 您应该为 {{site.data.keyword.blockchainfull_notm}} Platform 部署创建一个新的定制名称空间。名称空间需要使用[必需的 PodSecurityPolicy](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-prereqs-pod-security-requirements)。如果计划创建多个区块链网络，例如为开发、编译打包和生产创建不同的环境，那么应该为每个环境创建唯一的名称空间。请注意，每个名称空间只能部署一个 Helm chart，因此如果希望控制台的多个实例在同一集群上运行，请确保分别使用单独的名称空间。

3. 在 {{site.data.keyword.cloud_notm}} Private 控制台中检索 CA 的集群代理 IP 地址的值。**注：**您需要是[集群管理员](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external}才能访问代理 IP。登录到 {{site.data.keyword.cloud_notm}} Private 集群。在左侧导航面板中，单击**平台**，然后单击**节点**以查看在集群中定义的节点。单击具有角色`代理`的节点，然后从表中复制`主机 IP` 的值。**重要信息：**请保存此值，配置 Helm chart 的`代理 IP` 字段时会使用此值。

4. 创建[映像安全策略](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-image-policy)，以允许部署从集群 Docker 注册表下载必需的映像。

5. 创建将用于首次登录到控制台的密码，并将其存储在 {{site.data.keyword.cloud_notm}} Private 的私钥对象中。您可以在[下一部分](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)中找到创建私钥的步骤。


## 集群映像策略需求
{: #console-deploy-icp-image-policy}

缺省情况下，ICP 3.2+ 中启用了[容器映像安全性](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_images/image_security.html)。因此，需要将安装 Helm chart 时指定的 Docker Hub 容器注册表添加到可信注册表列表。否则，控制台部署无法访问该注册表中的映像。执行以下步骤来创建新的映像策略：

1. 登录到 {{site.data.keyword.cloud_notm}} Private 控制台。在左侧导航面板中，单击**管理**，然后单击**资源安全性**。在**资源安全性**菜单下，导航至**映像策略**，然后单击**创建映像策略**。

2. 在**策略详细信息**部分中，填写以下字段：
  - 输入新映像策略的**名称**。例如：`ibp-imagepolicy`。
  - 在**作用域**中，选择`名称空间`。
  - 在**名称空间**中，选择在其中安装了 Helm chart 的名称空间。   

3. 在**策略详细信息**部分中，单击**添加注册表**，然后提供[安装 Helm chart](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-importing) 时指定的映像注册表，后跟 `/*`。
  - 例如，可以提供注册表 `<cluster_CA_domain>:8500/*` 或 `<cluster_CA_domain>:8500/ibp/*` 和 `<cluster_CA_domain>:8500/op-tools/*`。

还可以使用 YAML 文件和 kubectl 命令行工具来添加新的集群映像策略。您可以在下面找到示例 YAML 文件：

```
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ClusterImagePolicy
metadata:
  name: ibp-imagepolicy
spec:
  repositories:
  - name: <cluster_CA_domain>:8500/ibp/*
  - name: <cluster_CA_domain>:8500/op-tools/*
```

## 创建控制台密码私钥
{: #console-deploy-icp-password-secret}

您需要创建将用于首次登录到控制台的缺省密码，然后才能访问控制台。在部署控制台之前，请创建 [Kubernetes 私钥](https://kubernetes.io/docs/concepts/configuration/secret/){: external}以用于存储密码。Kubernetes 私钥支持您保护和共享信息，而不必将其直接传递到部署。

1. 创建密码，并将其编码为 Base64 格式。在终端窗口中运行以下命令，并将 `password` 的值替换为要使用的值。**保存此命令的输出**。
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. 登录到 {{site.data.keyword.cloud_notm}} Private 控制台。在左侧导航面板中，单击**配置**，然后单击**私钥**。单击**创建私钥**按钮以打开一个弹出面板，在其中允许您生成新的私钥对象。

3. 在**常规**选项卡上，填写以下字段：
  - **名称：**为私钥提供在集群中唯一的名称。部署控制台时将使用此名称。名称必须全部为小写。
  - **名称空间：**要添加私钥的名称空间。选择要将控制台部署到的`名称空间`。
  - **类型：**输入值`通用`。

4. 使**注释**选项卡保留为空白。

5. 在**数据**选项卡中，添加用户名和密码作为键值对。
  1. 在第一个**名称**字段中，输入值 `password`。
  2. 在第一个**值**字段中，输入步骤 1 中的结果 `echo -n 'password' | base64`。
  3. 单击**创建**以构成新的 Secret 对象。

部署控制台时，为配置页面的`控制台管理员密码私钥名称`字段提供私钥名称。首次登录到控制台时，您需要使用在步骤 1 中编码的值。除非控制台管理员更改了此值，否则这将成为控制台的缺省密码。有关更多信息，请参阅[通过控制台管理用户](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)。

## 创建 TLS 私钥（可选）
{: #console-deploy-icp-tls-secret}

控制台使用 TLS 来保护控制台与区块链节点之间的通信。缺省情况下，控制台会生成自己的 TLS 证书。但是，您可以选择提供自己的 TLS 证书和密钥。使用以下步骤将证书存储在 [Kubernetes 私钥](https://kubernetes.io/docs/concepts/configuration/secret/)中。然后，可以通过在配置期间向“TLS 私钥”字段提供私钥名称，将这些证书提供给控制台。

1. 将 TLS 证书和专用密钥转换为 Base64 格式。可以通过在本地计算机上运行以下命令，将证书或密钥文件的内容转换为 Base64 字符串：

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  对 TLS 证书和附带的 TLS 密钥运行此命令。**保存**输出。

2. 登录到 {{site.data.keyword.cloud_notm}} Private 控制台。在左侧导航面板中，单击**配置**，然后单击**私钥**。单击**创建私钥**按钮以打开一个弹出面板，在其中允许您生成新的私钥对象。

3. 在**常规**选项卡上，填写以下字段：
  - **名称：**为私钥提供在集群中唯一的名称。配置 CA 时将使用此名称。名称必须全部为小写。
  - **名称空间：**要添加私钥的名称空间。选择要将 CA 部署到的`名称空间`。
  - **类型：**输入值 `kubernetes.io/tls`。

4. 使**注释**选项卡保留为空白。

5. 在**数据**选项卡中，添加用户名和密码作为键值对。
  1. 在第一个**名称**字段中，输入值 `tls.crt`。
  2. 在第一个**值**字段中，输入步骤 1 中 Base64 编码的 TLS 证书的字符串。
  3. 单击**添加数据**按钮以添加第二个键值对。
  4. 在第一个**名称**字段中，输入值 `tls.key`。
  5. 在第一个**值**字段中，输入步骤 1 中 Base64 编码的 TLS 密钥的字符串。
  6. 单击**创建**以构成新的 Secret 对象。

部署控制台时，为配置页面的“所有参数”部分中的 `TLS 私钥`字段提供所创建私钥的名称。

删除 Helm 发布时，不会从 {{site.data.keyword.cloud_notm}} Private 集群中除去私钥。您应负责管理 {{site.data.keyword.cloud_notm}} Private 集群中的密码和 TLS 私钥。如果您计划未来部署其他控制台，那么可以复用私钥。否则，您应负责将其从 {{site.data.keyword.cloud_notm}} Private 集群中删除。
{:note}

## 配置
{: #console-deploy-icp-configuration}

创建 TLS 私钥对象后，可以使用以下步骤来部署控制台。

1. 登录到 {{site.data.keyword.cloud_notm}} Private 控制台，然后单击右上角的**目录**。
2. 单击左侧导航面板中的`区块链`，并找到标注为 `ibm-blockchain-platform-prod` 的磁贴。单击该磁贴以将其打开。您应该会看到包含有关安装和配置 Helm chart 的信息的自述文件。
3. 单击面板顶部的**配置**选项卡，或单击右下角的**配置**按钮。
4. 指定[配置和 pod 安全性参数](#icp-peer-deploy-configuration-parms)的值并接受许可协议。
5. 导航至**参数**部分：
- 可以仅使用[快速入门参数](#icp-peer-deploy-quickstart-parms)来部署控制台。如果您是在试验或是新手，请使用此选项。
- 可以使用[所有参数](#icp-peer-deploy-quickstart-parms)部分来定制控制台使用的网络访问权、资源和存储器。建议仅更富有经验的 Kubernetes 用户使用**所有参数**部分。
6. 单击**安装**。

以下各表描述了配置参数字段及其缺省值。

### 配置和 pod 安全性参数
{: #icp-peer-deploy-configuration-parms}

|参数|描述|缺省值|必需|
| --------------|-----------------|-------|------- |
| `Helm 发布名称`|用于标识 Helm 发布部署的名称。以小写字母开头并以任意字母数字字符结尾，并且只能包含连字符和小写字母数字字符。|无|是|
|`目标名称空间`|指定为控制台和组件创建的名称空间。名称空间必须包含 `ibm-privileged-psp` 策略。否则，[绑定 PodSecurityPolicy](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) 至名称空间。|无|是|
|`目标集群`|指定要部署资源的集群。可用的集群按所选名称空间和 Kubernetes 版本需求进行过滤。|无|是|
|`目标名称空间策略`|已预配置为使用您的目标名称空间。值必须包含 `ibm-privileged-psp` 或 `ibm-blockchain-platform-psp` 策略。|无|是|

### 快速入门参数和所有参数
{: #icp-peer-deploy-quickstart-parms}

如果您是在试验或是新手，请使用“快速入门”部分。通过“所有参数”部分，可以定制网络访问权、资源和存储器，建议仅更富有经验的 Kubernetes 用户使用此部分。

**单击“所有参数”选项卡以查看所有配置参数。**

|参数|描述|缺省值|必需|
| --------------|-----------------|-------|------- |
|**控制台设置**|**管理和部署控制台。**| | |
|`代理 IP`|集群的代理节点的 IP 地址。|无|是|
|`控制台管理员电子邮件`|用于登录到控制台的电子邮件。|无|是|
|`控制台管理员密码私钥名称`|[创建用于存储密码](#console-deploy-icp-password-secret)（登录到控制台时使用）的私钥的名称。|无|是|
|**网络设置**|**对控制台的网络访问权**| | |
|`控制台主机名`|输入与“代理 IP”相同的值。|无|是|
|`控制台端口`|输入 31210 - 31220 范围内要使用的端口。此端口不能由其他应用程序使用。|无|是|
|`代理主机名`|代理服务器主机名。输入与“代理 IP”相同的值。|无|是|
|`代理端口`|输入 31210 - 31220 范围内要使用的任何端口。此端口不能由其他应用程序或控制台使用。|无|是|
|**存储设置**|**控制台要使用的存储器**| | |
|`存储类名称`|输入控制台和创建的组件要使用的存储类的名称。|无|是|
{: caption="表 1. 快速入门参数" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|参数|描述|缺省值|必需|
| --------------|-----------------|-------|------- |
|`许可证`|设置为“接受”以指示您接受许可证条款|不接受|是|
|`体系结构`|选择云平台体系结构。（AMD64 或 S390x）|AMD64|否|
|**控制台设置**|**管理和部署控制台。**| | |
|`服务帐户名称`|操作员要使用的服务帐户。|无|否|
|`代理 IP`|集群中代理节点的 IP 地址。|无|是|
|`控制台管理员电子邮件`|用于登录到控制台的电子邮件。|无|是|
|`控制台管理员密码私钥名称`|[创建用于存储密码](#console-deploy-icp-password-secret)（登录到控制台时使用）的私钥的名称。|无|是|
|**Docker 映像设置**|**使用这些设置来定制要由控制台拉取的 Fabric 映像**| | |
|`imagePullSecret 名称`|要用于下载映像的 imagePullSecret。|`ibp-ibmregistry`|否|
|**网络设置**|**对控制台的网络访问权**| | |
|`控制台主机名`|输入与“代理 IP”相同的值。|无|是|
|`控制台端口`|输入 31210 - 31220 范围内要使用的任何端口。|无|是|
|`代理主机名`|代理服务器主机名。输入与“代理 IP”相同的值。|无|是|
|`代理端口`|输入 31210 - 31220 范围内要使用的任何端口。此端口不能由其他应用程序或控制台使用。|无|是|
|`TLS 私钥`|[创建用于存储 TLS 证书](#console-deploy-icp-tls-secret)（将由控制台使用）的私钥的名称。|无|否|
|**存储设置**|**为控制台和工具供应存储器**| | |
|`卷声明大小`|要供应的持久卷声明的大小。|10Gi|否|
|`存储类名称`|控制台和创建的组件要使用的存储类的名称。|无|是|
|`存储器访问方式`|指定用于 PVC 的存储器[访问方式](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes)。|ReadWriteMany|否|
|**分配资源**|**为控制台分配资源**| | |
|`opstools CPU 限制`|分配给 opstools 组件的最大 CPU 数。|500m|否|
|`opstools 内存限制`|分配给 opstools 组件的最大内存量。|1000Mi|否|
|`opstools CPU 请求`|分配给 opstools 组件的最小 CPU 数。|500m|否|
|`opstools 内存请求`|分配给 opstools 组件的最小内存量。|1000Mi|否|
|`configtxlator CPU 限制`|分配给 configtxlator 工具的最大 CPU 数。|25m|否|
|`configtxlator 内存限制`|分配给 configtxlator 工具的最大内存量。|50Mi|否|
|`configtxlator CPU 请求`|分配给 configtxlator 工具的最小 CPU 数。|25m|否|
|`configtxlator 内存请求`|分配给 configtxlator 工具的最小内存量。|50Mi|否|
|`CouchDB CPU 限制`|分配给 CouchDB 的最大 CPU 数。|500m|否|
|`CouchDB 内存限制`|分配给 CouchDB 的最大内存量。|1000Mi|否|
|`CouchDB CPU 请求`|分配给 CouchDB 的最小 CPU 数。|500m|否|
|`CouchDB 内存请求`|分配给 CouchDB 的最小内存量。|1000Mi|否|
|`operator CPU 限制`|分配给 operator 组件的最大 CPU 数。|100m|否|
|`operator 内存限制`|分配给 operator 组件的最大内存量。|200Mi|否|
|`operator CPU 请求`|分配给 operator 组件的最小 CPU 数。|100m|否|
|`operator 内存请求`|分配给 operator 组件的最小内存量。|200Mi|否|
|`deployer CPU 限制`|分配给 deployer 组件的最大 CPU 数。|100m|否|
|`deployer 内存限制`|分配给 deployer 组件的最大内存量。|200Mi|否|
|`deployer CPU 请求`|分配给 deployer 组件的最小 CPU 数。|100m|否|
|`deployer 内存请求`|分配给 deployer 组件的最小内存量。|200Mi|否|
{: caption="表 2. 所有参数" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### 使用 Helm 命令行安装 Helm 发布
{: #console-deploy-icp-helm-cli}

或者，可以使用 `helm` CLI 来安装 Helm 发布。运行 `helm install` 命令之前，请确保[将集群的 Helm 存储库添加到 Helm CLI 环境](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}。

您可以通过创建 `yaml` 文件并将其传递到以下 `helm install` 命令来设置安装所需的参数。
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

其中：
- `<helm_release name>` 表示您要为 Helm 发布提供的名称。
- `<helm_chart>` 表示导入到目录中的 Helm chart 的名称。
- `<helm_chart_version>` 表示导入到目录中的 Helm chart 的版本。
- `<customvalues.yaml>` 是包含配置参数的 yaml 文件的名称。

例如：
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

您可以通过编辑已下载归档文件中包含的 `values.yaml` 来创建新的 `yaml` 文件，values.yaml 文件包含所有必需参数及其缺省设置。

## 验证安装

完成配置参数并单击**安装**按钮后，单击**查看 Helm 发布**按钮可查看部署。如果部署成功，您应该会在“部署”表中看到 `DESIRED`、`CURRENT`、`UP TO DATE` 和 `AVAILABLE` 字段中的值为 1。您可能需要单击“刷新”并等待该表更新。

通过导航至**部署**概述屏幕，然后单击已创建的 pod，可以查看部署的详细信息。Helm chart 部署在集群上创建了五个容器：
- **optools**：控制台 UI。
- **deployer**：允许控制台与部署进行通信的工具。
- **configtxlator**：控制台用于读取和创建通道更新的工具。
- **couchdb**：用于存储控制台数据（包括您的授权信息）的 CouchDB 实例。
- **operator**：用于管理组件部署的 Kubernetes operator。

## 登录到控制台

安装控制台后，可以使用浏览器来访问控制台。您可以在部署后打开的 Helm 发布概述屏幕的**注：**部分中找到该 URL。检查以确保使用的不是 ESR 版本的 Firefox。如果使用的是这种版本，请切换到其他浏览器（如 Chrome），然后重试。

在浏览器中，您应该能够看到控制台登录屏幕：
- 对于**用户标识**，请使用在配置期间为`控制台管理员电子邮件`字段提供的值。
- 对于**密码**，请使用已编码并存储在[密码私钥](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)中，然后在配置期间传递给控制台的值。此密码将成为所有新用户用于登录到控制台的控制台缺省密码。首次登录后，系统会要求您提供可用于登录到控制台的新密码。

供应 Helm chart 的管理员可以授予其他用户对控制台的访问权，并指定他们可以执行哪些操作。有关更多信息，请参阅[通过控制台管理用户](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)。

## 后续步骤
{: #console-deploy-icp-next-steps}

访问控制台后，可以查看控制台 UI 的**节点**选项卡。可以使用此屏幕在本地集群中部署组件。请访问[构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)，以开始使用控制台。还可以使用此选项卡来操作已在其他云上创建的节点。有关更多信息，请参阅[导入节点](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes)。

要了解如何管理可以访问控制台的用户，使用 {{site.data.keyword.blockchainfull_notm}} Platform API，以及查看控制台和区块链组件的日志，请访问[在 {{site.data.keyword.cloud_notm}} Private 上管理控制台](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage)。
