---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: client application, Commercial Paper, SDK, wallet, generate a certificate, generate a private key, fabric gateway, APIs, smart contract

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

# 创建应用程序
{: #ibp-console-app}

安装智能合同并部署节点后，可以使用客户机应用程序与网络中的其他成员之间进行事务处理。应用程序可以调用智能合同中包含的业务逻辑，以在区块链分类帐上创建、转让或更新资产。使用本教程可了解如何使用客户机应用程序与通过 {{site.data.keyword.blockchainfull}} Platform 控制台管理的网络进行交互。
{:shortdesc}

**目标受众：**本主题适用于有兴趣进一步了解如何创建与区块链网络进行交互的客户机应用程序的应用程序开发者。

## 学习资源
{: #ibp-console-app-learning-resources}

您可以在商业票据样本中了解有关应用程序和智能合同如何协同工作的更多信息。请访问有关如何[在 {{site.data.keyword.blockchainfull_notm}} Platform 上运行商业票据样本](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-commercial-paper)的主题，在其中可以学习部署和调用商业票据合同。

开发应用程序可能需要网络中的两个不同用户（即，网络操作员和应用程序开发者）之间进行协调：
- **网络操作员**是使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台部署组织节点并在网络上安装智能合同的管理员。
- **应用程序开发者**构建将由最终用户使用的客户机应用程序。开发者使用 [Hyperledger Fabric SDK](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html#hyperledger-fabric-sdks){: external} 来调用智能合同中编写的事务。

如果您是**网络操作员**，那么您需要先完成以下步骤，然后应用程序开发者才能与您的网络进行交互：
1. 使用组织 CA 来[注册应用程序身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities)。
2. 通过“智能合同”面板[下载连接概要文件](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile)。
3. 向应用程序开发者发送以下对象和信息：
  - 应用程序身份的注册标识和私钥。
  - 连接概要文件。
  - 智能合同名称。
  - 在其中实例化智能合同的通道的名称。  

如果您是**应用程序开发者**，请使用网络操作员提供的信息来完成以下步骤：
1. 使用应用程序身份的注册标识和私钥以及连接概要文件中的 CA 端点信息来生成证书和专用密钥。
2. 使用连接概要文件、通道名称、智能合同名称和应用程序密钥来调用智能合同。  

通过 {{site.data.keyword.blockchainfull_notm}} Platform 控制台下载的连接概要文件只能用于通过 Node.js（JavaScript 和 TypeScript）和 Java Fabric SDK 来连接到网络。
{: note}

应用程序开发者可以使用两个编程模型与网络进行交互：

**高级别 Fabric SDK API**

从 Fabric V1.4 开始，用户可以利用经过简化的应用程序和智能合同编程模型。新模型减少了提交事务所需的步骤数和代码量。只有使用 **Node.js** 编写的应用程序支持此模型。如果要利用新模型，可以使用此教程在 {{site.data.keyword.blockchainfull_notm}} Platform 网络上完成以下操作：

- 使用 SDK [为应用程序生成证书](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-enroll)。
- [通过 SDK 调用智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-invoke)。
- 通过将[商业票据教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-commercial-paper)部署到通过控制台管理的节点，了解有关应用程序开发的信息。此教程将提供有关如何使用 Fabric 电子钱包和网关的更多背景信息。

**低级别 Fabric SDK API**

如果要继续使用现有的智能合同和应用程序代码，或使用 Hyperledger 社区提供的其他 Fabric SDK 语言，那么可以使用[低级别 Fabric SDK API](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-low-level) 来连接到网络。

## 注册应用程序身份
{: #ibp-console-app-identities}

应用程序需要对它们提交到 {{site.data.keyword.blockchainfull_notm}} 节点的事务进行签名，并附加签名证书，以供节点用于验证这些事务是否由正确的当事方发送。这可确保事务是有权参与的组织提交的。

网络操作员需要使用组织的 CA 来[注册应用程序身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-register)，然后应用程序开发者可以使用此身份来生成证书和专用密钥。操作员可以提供身份的注册标识和私钥以及 CA 端点信息，以供 SDK 用于生成证书。通过在客户机端进行注册，应用程序开发者可确保没有其他当事方有权访问应用程序的专用密钥。在注册期间，网络操作员可以将注册限制设置为 1 以实现额外的安全性。应用程序开发者注册后，该注册标识和私钥即不能用于生成其他专用密钥。

如果您不是很担心安全性，那么网络操作员可以使用 [CA 选项卡](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)来注册应用程序身份。然后，操作员可以下载身份，或将其导出到控制台电子钱包。要通过 SDK 使用证书，密钥需要从 Base64 解码为 PEM 格式。可以通过在本地计算机上运行以下命令来解码证书：

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
echo <base64_string> | base64 --decode $FLAG > <key>.pem
```
{:codeblock}

## 下载连接概要文件
{: #ibp-console-app-profile}

应用程序只能向已在通道上实例化的智能合同提交事务。因此，可以在控制台中已实例化的智能合同的列表中找到进行连接以与智能合同交互所需要的信息。这意味着您必须已经安装并实例化智能合同。

通过 {{site.data.keyword.blockchainfull_notm}} Platform 控制台下载的连接概要文件只能用于通过 Node.js（JavaScript 和 TypeScript）和 Java Fabric SDK 来连接到网络。
{: note}

Hyperledger Fabric [事务处理流程](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}跨多个组件，其中客户机应用程序收集同级的背书，并将已背书的事务发送到排序服务。连接概要文件为应用程序提供了提交事务所需的同级和排序节点的端点。其中还包含有关您组织的信息，例如认证中心和 MSP 标识。Fabric SDK 可以直接读取连接概要文件，而无需编写管理事务处理和背书流程的代码。

为了利用 Hyperledger Fabric 的[服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}功能，必须配置锚点同级。通过服务发现，应用程序可了解组织外部通道上哪些同级需要对事务进行背书。若不使用服务发现，您需要通过频带外操作从其他组织获取这些同级的端点信息，并将其添加到连接概要文件。有关更多信息，请参阅[配置锚点同级](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers)。

导航至 Platform 控制台中的**智能合同**选项卡。导航至每个已实例化的智能合同旁边的溢出菜单。单击名为**使用 SDK 连接**的按钮。这将打开一个侧面板，允许您构建和下载连接概要文件。首先，需要选择用于注册应用程序身份的组织 CA。此外，还需要选择组织 MSP 定义。然后，就能够下载可用于生成证书并调用智能合同的连接概要文件。

如果节点是部署在 {{site.data.keyword.cloud_notm}} Private 上，那么需要确保连接概要文件中的认证中心、同级和排序节点使用的端口在外部公开给客户机应用程序。
{: note}


## 使用 SDK 进行注册
{: #ibp-console-app-enroll}

网络操作员提供应用程序身份的注册标识和私钥以及网络连接概要文件后，应用程序开发者就可以使用 Fabric SDK 或 Fabric CA 客户机来生成客户机端证书。可以使用以下步骤通过 [Fabric SDK for Node.js](https://fabric-sdk-node.github.io/){: external} 来注册应用程序身份。

1. 将连接概要文件保存到本地计算机，并将其重命名为 `connection.json`。
2. 在连接概要文件所在的目录中，将以下代码块保存为 `enrollUser.js`：

    ```
    'use strict';

    const FabricCAServices = require('fabric-ca-client');
    const { FileSystemWallet, X509WalletMixin } = require('fabric-network');
    const fs = require('fs');
    const path = require('path');

    const ccpPath = path.resolve(__dirname, 'connection.json');
    const ccpJSON = fs.readFileSync(ccpPath, 'utf8');
    const ccp = JSON.parse(ccpJSON);

    async function main() {
      try {

      // Create a new CA client for interacting with the CA.
      const caURL = ccp.certificateAuthorities['<CA_Name>'].url;
      const ca = new FabricCAServices(caURL);

      // Create a new file system based wallet for managing identities.
      const walletPath = path.join(process.cwd(), 'wallet');
      const wallet = new FileSystemWallet(walletPath);
      console.log(`Wallet path: ${walletPath}`);

      // Check to see if we've already enrolled the admin user.
      const userExists = await wallet.exists('user1');
      if (userExists) {
        console.log('An identity for "user1" already exists in the wallet');
        return;
      }

      // Enroll the admin user, and import the new identity into the wallet.
      const enrollment = await ca.enroll({ enrollmentID: '<app_enroll_id>', enrollmentSecret: '<app_enroll_secret>' });
      const identity = X509WalletMixin.createIdentity('<msp_id>', enrollment.certificate, enrollment.key.toBytes());
      await wallet.import('user1', identity);
      console.log('Successfully enrolled client "user1" and imported it into the wallet');

      } catch (error) {
        console.error(`Failed to enroll "user1": ${error}`);
        process.exit(1);
      }
    }

    main();
    ```
    {:codeblock}

3. 编辑 `enrollUser.js` 以替换以下值：
  - 将 ``<CA_Name>`` 替换为组织 CA 的名称。您可以在连接概要文件的 "organizations" 部分中的 "Certificate Authorities" 下找到 CA 名称。不要使用 "Certificate Authorities" 部分中的 "caName"。
  - 将 ``<app_enroll_id>`` 替换为网络操作员提供的应用程序注册标识。
  - 将 ``<app_enroll_secret>`` 替换为网络操作员提供的应用程序注册私钥。
  - 将 ``<msp_id>`` 替换为组织的 MSP 标识。您可以在连接概要文件的 "organizations" 部分下找到 MSP 标识。
4. 使用终端导航至 `enrollUser.js`，并运行 `node enrollUser.js`。如果命令成功运行，您应该会看到以下输出：

  ```
  Successfully enrolled client "user1" and imported it into the wallet
  ```
  SDK 将在该命令创建的 `wallet/user1/` 目录中创建并存储证书。此目录是用于提交未来事务的文件系统电子钱包。

Fabric SDK 使用的电子钱包与 {{site.data.keyword.blockchainfull_notm}} Platform 控制台中的电子钱包不同。SDK 无法直接使用存储在控制台电子钱包中的身份。
{: note}

## 使用 SDK 调用智能合同
{: #ibp-console-app-invoke}

生成应用程序签名证书和专用密钥并将其存储在电子钱包中后，即已准备好提交事务。您需要知道智能合同的名称以及在其中实例化该合同的通道的名称。可以使用以下步骤通过 [Fabric SDK for Node.js](https://fabric-sdk-node.github.io/){: external} 来调用智能合同。


1. 在本地计算机上将以下文件保存为 `invoke.js`。请将该文件保存在 `enrollUser.js` 所在的目录中。

    ```
    'use strict';

    const { FileSystemWallet, Gateway } = require('fabric-network');
    const fs = require('fs');
    const path = require('path');

    async function main() {
      try {

        // Parse the connection profile. This would be the path to the file downloaded
        // from the IBM Blockchain Platform operational console.
        const ccpPath = path.resolve(__dirname, 'connection.json');
        const ccp = JSON.parse(fs.readFileSync(ccpPath, 'utf8'));

        // Configure a wallet. This wallet must already be primed with an identity that
        // the application can use to interact with the peer node.
        const walletPath = path.resolve(__dirname, 'wallet');
        const wallet = new FileSystemWallet(walletPath);

        // Create a new gateway, and connect to the gateway peer node(s). The identity
        // specified must already exist in the specified wallet.
        const gateway = new Gateway();
        await gateway.connect(ccp, { wallet, identity: 'user1' , discovery: {enabled: true, asLocalhost:false }});

        // Get the network channel that the smart contract is deployed to.
        const network = await gateway.getNetwork('<channel_name>');

        // Get the smart contract from the network channel.
        const contract = network.getContract('<smart_contract_name>');

        // Submit the 'createCar' transaction to the smart contract, and wait for it
        // to be committed to the ledger.
        await contract.submitTransaction('createCar', 'CAR12', 'Honda', 'Accord', 'Black', 'Tom');
        console.log('Transaction has been submitted');

        await gateway.disconnect();

        } catch (error) {
          console.error(`Failed to submit transaction: ${error}`);
          process.exit(1);
        }
      }
    main();
    ```
    {:codeblock}

2. 编辑 `invoke.js` 以替换以下值：
  - 将 ``<channel_name>`` 替换为在其中实例化智能合同的通道的名称。您可以在连接概要文件的 "Certificate Authorities" 部分下找到 CA 名称。
  - 将 ``<smart_contract_name>`` 替换为已安装智能合同的名称。可以从网络操作员那里获取此值。
  - 编辑 `submitTransaction` 的内容以调用智能合同中的函数。编写 `invoke.js` 文件是为了调用 [fabcar 智能合同](https://github.com/hyperledger/fabric-samples/tree/release-1.4/chaincode/fabcar){: external}。如果要运行以下文件来提交事务，请安装 fabcar 并在其中一个通道上实例化智能合同。

3. 使用终端导航至 `invoke.js`，并运行 `node invoke.js`。如果命令成功运行，您应该会看到以下输出：

  ```
  Transaction has been submitted
  ```
  {:codeblock}
  如果使用控制台导航至通道，您将能够看到该事务添加的另一个区块。

## 运行商业票据样本
{: #ibp-console-app-commercial-paper}

Hyperledger Fabric 文档中的 [Commercial paper tutorial](https://hyperledger-fabric.readthedocs.io/en/release-1.4/tutorial/commercial_paper.html){: external} 将引导开发者完成多个当事方购买、销售和兑换商业票据的用例。该教程通过提供样本智能合同和应用程序代码以允许在 Fabric 的本地实例上创建和交易资产，扩展了 [Developing Applications](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} 主题。

此外，还可以将商业票据教程样本代码部署到 {{site.data.keyword.blockchainfull_notm}} Platform 网络上。这将允许您快速开始与网络进行交互，并使用样本来下载必要的依赖项。样本代码还包括如何将证书导入到[电子钱包](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/wallet.html){: external}，并使用连接概要文件构建 [Fabric 网关](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/gateway.html){: external}的示例。两个不同的组织可以使用该教程来完成与单个资产之间的不同事务处理。如果使用[构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)来部署连接到通道的两个同级组织，那么可以使用这两个组织与该教程进行交互。

执行以下步骤在网络上部署样本。可以查看 Fabric 文档 [Commercial paper tutorial](https://hyperledger-fabric.readthedocs.io/en/release-1.4/tutorial/commercial_paper.html){: external} 中的教程，以了解有关智能合同和应用程序结构的更多详细信息。

### 先决条件

需要先在本地计算机上安装必需的工具，然后才能部署商业票据样本：
  * [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git){: external}
  * [Node.js](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html#node-js-runtime-and-npm){: external}

您还需要使用文本编辑器来编辑和保存样本中的文件。可以使用许多免费提供的高质量编辑器，例如 [Visual Studio Code](https://code.visualstudio.com/){: external}、[Atom](https://atom.io/){: external}、[Sublime Text](http://www.sublimetext.com/){: external} 或 [Brackets](http://brackets.io/){: external}。

### 步骤 1：下载样本

可以通过克隆 [Fabric 样本存储库](https://github.com/hyperledger/fabric-samples){: external}来下载商业票据样本：

```
git clone https://github.com/hyperledger/fabric-samples.git
```
{:codeblock}

下载 Fabric 样本后，运行以下命令以确保使用与 Fabric V1.4 兼容的样本版本。

```
cd fabric-samples
git checkout v1.4.1
```
{:codeblock}

可以在 `fabric-samples` 的 `commercial paper` 文件夹中找到该样本。

```
cd $HOME/fabric-samples/commercial-paper
```
{:codeblock}

教程包含两个样本组织：**magnetocorp** 和 **digibank**。每个组织都有自己的样本应用程序代码，存储在 `organization` 目录下的两个不同文件夹中。

```
cd organization && ls
digibank	magnetocorp
```
{:codeblock}

在使用教程的过程中，将首先使用 magnetocorp 应用程序代码来发行商业票据。然后，可以使用 digibank 应用程序代码来购买和兑换票据。两个目录包含相同的智能合同。

导航至 magnetocorp 目录中的应用程序代码。

```
cd magnetocorp/application
```
{:codeblock}

位于该目录后，可以通过运行以下命令来下载应用程序依赖项：

```
  npm install
  ```
{:codeblock}

### 步骤 2：安装和实例化智能合同

您可以在 `digibank` 和 `magnetocorp` 目录的 `contract` 文件夹中找到商业票据智能合同。需要使用该教程在组织的所有同级上安装此智能合同。然后，需要在通道上实例化商业票据合同。智能合同需要打包成 [.cds 格式](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external}，才能使用控制台进行安装。

可以使用 [{{site.data.keyword.blockchainfull_notm}} VS Code 扩展](/docs/services/blockchain?topic=blockchain-develop-vscode)来打包智能合同。安装该扩展后，使用 Visual Studio Code 打开工作空间中的 `contracts` 文件夹。打开 _{{site.data.keyword.blockchainfull_notm}} Platform_ 选项卡。在 _{{site.data.keyword.blockchainfull_notm}} Platform_ 窗格中，导航至“智能合同包”部分，然后单击**打包智能合同项目**。VS Code 扩展将使用 `contracts` 文件夹中的文件来创建名为 `papernet-js@.0.0.1.cds` 的新包。右键单击此包以将其导出到本地文件系统。接着，可以使用控制台[在同级上安装智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install)，然后[在通道上实例化智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate)。

### 步骤 3：为电子钱包生成证书

应用程序需要对它们发送给 Fabric 组件的请求签名。如果组件无法识别提交事务的组织，那么会拒绝事务并返回错误。商业票据样本将创建一个文件系统电子钱包，用于存储证书并对事务签名。有关应用程序如何使用电子钱包的更多信息，请参阅 Fabric 文档中的 [Wallet](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/wallet.html){: external} 主题。Fabric SDK 使用的电子钱包与 {{site.data.keyword.blockchainfull_notm}} Platform 控制台中的电子钱包不同。SDK 无法直接使用存储在控制台电子钱包中的身份。

原始样本使用 `addToWallet.js` 文件通过 fabric samples 文件夹中的证书来创建文件系统电子钱包。我们将创建一个新文件，此文件使用 SDK 来生成客户机端证书，并将其直接存储在新的电子钱包中。

选择要用于以 magnetocorp 身份操作教程的组织的 CA。例如，如果已完成“构建网络”教程，那么可以使用 Org1。使用 CA 来[创建应用程序身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities)。**保存**注册标识和私钥。

使用控制台来[下载连接概要文件](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile)。将连接概要文件保存到本地文件系统，并将其重命名为 `connection.json`。然后使用以下命令将连接概要文件移至未来命令可在其中引用此概要文件的目录。

  ```
  mv $HOME/<path_to_creds>/connection.json /gateway/connection.json
  ```
  {:codeblock}

导航至 `/magnetocorp/application` 目录，并将以下代码块保存为 `enrollUser.js`。

    ```
    'use strict';

    const FabricCAServices = require('fabric-ca-client');
    const { FileSystemWallet, X509WalletMixin } = require('fabric-network');
    const fs = require('fs');
    const path = require('path');

    const ccpPath = path.resolve(__dirname, '../gateway/connection.json');
    const ccpJSON = fs.readFileSync(ccpPath, 'utf8');
    const ccp = JSON.parse(ccpJSON);

    async function main() {
      try {

        // Create a new CA client for interacting with the CA.
        const caURL = ccp.certificateAuthorities['<CA_Name>'].url;
        const ca = new FabricCAServices(caURL);

        // Create a new file system based wallet for managing identities.
        const wallet = new FileSystemWallet('../identity/user/isabella/wallet');

        // Check to see if we've already enrolled the admin user.
        const userExists = await wallet.exists('user1');
        if (userExists) {
          console.log('An identity for "user1" already exists in the wallet');
          return;
        }

        // Enroll the admin user, and import the new identity into the wallet.
        const enrollment = await ca.enroll({ enrollmentID: '<app_enroll_id>', enrollmentSecret: '<app_enroll_secret>' });
        const identity = X509WalletMixin.createIdentity('<msp_id>', enrollment.certificate, enrollment.key.toBytes());
        await wallet.import('user1', identity);
        console.log('Successfully enrolled client "user1" and imported it into the wallet');

        } catch (error) {
          console.error(`Failed to enroll "user1": ${error}`);
          process.exit(1);
        }
    }
    main();
    ```
    {:codeblock}

在编辑此文件之前，应该花些时间来研究此文件的运作方式。首先，`enrollUser.js` 会从 `fabric-network` 库中导入 `FileSystemWallet` 和 `X509WalletMixin` 类。

```
const FabricCAServices = require('fabric-ca-client');
const { FileSystemWallet, X509WalletMixin } = require('fabric-network');
```
{:codeblock}

然后，该文件使用 `FileSystemWallet` 类在本地文件系统上构建电子钱包。可以编辑以下行，以将电子钱包存储在其他位置。

```
const wallet = new FileSystemWallet('../identity/user/isabella/wallet')
```
{:codeblock}

创建电子钱包后，代码片段使用注册标识和私钥通过组织 CA 进行注册。然后，它会为签名证书和专用密钥创建身份，并将其导入到电子钱包中。此外，请注意此文件如何将组织 MSP 标识传递到电子钱包中。

```
// Enroll the admin user, and import the new identity into the wallet.
const enrollment = await ca.enroll({ enrollmentID: '<app_enroll_id>', enrollmentSecret: '<app_enroll_secret>' });
const identity = X509WalletMixin.createIdentity('<msp_id>', enrollment.certificate, enrollment.key.toBytes());
wallet.import('user1', identity);
console.log('Successfully enrolled client "user1" and imported it into the wallet');
```
{:codeblock}

**编辑** `enrollUser.js` 以替换以下值：
- 将 `'<CA_Name>'` 替换为组织 CA 的名称。您可以在连接概要文件的 "organizations" 部分中的 "Certificate Authorities" 下找到 CA 名称。不要使用 "Certificate Authorities" 部分中的 "caName"。
- 将 `'<app_enroll_id>'` 替换为网络操作员提供的应用程序注册标识。
- 将 `'<app_enroll_secret>'` 替换为网络操作员提供的应用程序注册私钥。
- 将 `'<msp_id>'` 替换为组织的 MSP 标识。您可以在连接概要文件的 "organizations" 部分下找到此 MSP 标识。

保存 `enrollUser.js` 并将其关闭。在 `magnetocorp` 目录中，运行以下命令：
```
node enrollUser.js
```
{:codeblock}

命令成功完成后，您应该会看到以下输出：

```
Successfully enrolled client "user1" and imported it into the wallet
```
{:codeblock}

您可以找到在 `magnetocorp` 目录的 `identity` 文件夹中创建的电子钱包。

### 步骤 4：使用连接概要文件构建 Fabric 网关

Hyperledger Fabric [事务处理流程](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}跨多个组件，其中客户机应用程序扮演唯一角色。应用程序需要连接到需背书事务的同级，还需要连接到将对事务排序并将其添加到区块中的排序服务。可以使用连接概要文件来构造 Fabric 网关，以向应用程序提供这些节点的端点。然后，网关会与 Fabric 网络进行低级别交互。要了解更多信息，请访问 Fabric 文档中的 [Fabric 网关](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/gateway.html){: external}主题。

您已经下载了连接概要文件，并将其用于连接到组织的认证中心。现在，将使用连接概要文件来构建网关。

在文本编辑器中打开 `issue.js` 文件。在编辑此文件之前，请注意此文件将从 fabric-network 库中导入 `FileSystemWallet` 和 `Gateway` 类。

```
const { FileSystemWallet, Gateway } = require('fabric-network')
```
{:codeblock}

`Gateway` 类用于构造提交事务所使用的网关。

```
const gateway = new Gateway()
```
{:codeblock}

`FileSystemWallet` 类用于装入在上一步中创建的电子钱包。如果更改了电子钱包在文件系统上的位置，请**编辑**以下行。

```
const wallet = new FileSystemWallet('../identity/user/isabella/wallet');
```
{:codeblock}

导入电子钱包后，使用以下代码将连接概要文件和电子钱包传递到新网关。您需要对代码进行以下**编辑**，以使其类似于以下代码片段。为简洁起见，除去了输出日志的行。
- 更新 `userName` 以匹配在 `enrollUser.js` 中为 `identityLabel` 选择的值。
- 更新发现选项，以利用网络上的服务发现。设置 `discovery: { enabled: true, asLocalhost: false }`。  
- 更新用于导入连接概要文件的部分。控制台连接概要文件为 JSON 格式，而不是样本使用的 YAML 文件。  

```
const userName = 'user1';

// Load connection profile; will be used to locate a gateway
const ccpPath = path.resolve(__dirname, '../gateway/connection.json');
const ccpJSON = fs.readFileSync(ccpPath, 'utf8');
const connectionProfile = JSON.parse(ccpJSON);

// Set connection options; identity and wallet
let connectionOptions = {
  identity: userName,
  wallet: wallet,
  discovery: { enabled: true, asLocalhost: false }
};

await gateway.connect(connectionProfile, connectionOptions);
```
{:codeblock}

此代码片段使用网关来打开与同级节点和排序节点的 gRPC 连接，并与网络进行交互。

### 步骤 5：调用智能合同

配置网关以连接到控制台管理的网络后，我们将编辑 `issue.js` 文件中用于连接到商业票据智能合同的部分。您需要为网关提供合同名称以及在其中实例化智能合同的通道。

**编辑**以下行，以将 `mychannel` 替换为通道名称。

```
const network = await gateway.getNetwork('mychannel');
```
{:codeblock}

在用于向控制台输出消息的一行代码后面，可以找到用于为网关提供智能合同名称的一行。**编辑**以下行，以将名称 `papercontract` 更改为安装的合同的名称。

```
const contract = await network.getContract('papercontract-js', 'org.papernet.commercialpaper');
```
{:codeblock}

现在，网关已拥有提交事务所需的所有信息。以下行将使用定义新的商业票据资产的自变量，调用商业票据合同中的 `issue` 函数。

```
const issueResponse = await contract.submitTransaction('issue', 'MagnetoCorp', '00001', '2020-05-31', '2020-11-30', '5000000');
```
{:codeblock}

使用网关提交事务后，`issue.js` 文件会断开网关连接。

```
gateway.disconnect();
```
{:codeblock}

此命令会关闭网关打开的 gRPC 连接。关闭连接将节省网络资源并提高性能。

完成此步骤和**步骤 4** 中的编辑后，保存 `issue.js`，然后将其关闭。使用以下命令提交创建新商业票据的事务：

```
node issue.js
```
{:codeblock}

如果事务成功，您应该能够在终端中看到以下输出：

```
Transaction complete.
Disconnect from Fabric gateway.
Issue program complete.
```

### 步骤 6：以 digibank 身份操作样本

通过以 magnetocorp 身份操作来创建商业票据后，可以通过以 digibank 身份操作教程来购买和兑换商业票据。可以使用相同的组织 magnetocorp 来使用 digibank 应用程序代码，也可以使用不同组织的 CA、同级和连接概要文件。如果已完成[构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)，那么这是一个以 Org2 身份操作教程的好机会。

导航至 `digibank/application` 目录。可以遵循**步骤 3** 中提供的指导信息，创建和生成证书与电子钱包，用于以 digibank 身份对事务签名。接着，可以使用 `buy.js` 文件向 magnetocorp 购买商业票据，然后使用 `redeem.js` 兑换票据。可以遵循**步骤 4** 和**步骤 5** 来编辑这些文件，使其指向正确的连接概要文件、通道和智能合同。

## 使用低级别 Fabric SDK API 连接到网络
{: #ibp-console-app-low-level}

如果您希望保留现有的应用程序代码，或者将 Fabric SDK 用于除 Node.js 以外的语言，那么仍然可以使用较低级别的 Fabric SDK API 来连接到网络。使用控制台来[下载连接概要文件](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile)。然后，可以直接从连接概要文件导入通道的同级和排序节点的端点，或者使用节点端点信息来手动添加同级和排序节点对象。此外，还需要使用 CA 来[创建应用程序身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities)，然后在客户机端使用 CA 端点信息注册，或使用控制台生成证书。

[Fabric Node SDK](https://fabric-sdk-node.github.io){: external} 文档提供了有关如何[使用连接概要文件连接到网络](https://fabric-sdk-node.github.io/tutorial-network-config.html){: external}的教程。本教程使用连接概要文件中的 CA 端点信息通过 SDK 生成密钥。还可以使用控制台来生成签名证书和专用密钥，并将这些密钥转换为 PEM 格式。然后，可以使用以下代码将密钥直接传递到 SDK 的 [Fabric Client 类](https://fabric-sdk-node.github.io/Client.html){: external}来设置用户上下文：

```
fabric_client.createUser({
		username: 'admin',
		mspid:  'org1',
		cryptoContent: {
			privateKeyPEM: fs.readFileSync(path.join(__dirname,'./privateKey.pem')),
			signedCertPEM: fs.readFileSync(path.join(__dirname,'./certificate.pem'))
		}});
```
{:codeblock}

如果是使用低级别 SDK API 连接到网络，那么可以执行额外的步骤来管理应用程序的性能和可用性。有关更多信息，请参阅[应用程序连接和可用性的最佳实践](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-connectivity-availability)。


## 将索引用于 CouchDB
{: #console-app-couchdb}

如果将 CouchDB 用作状态数据库，那么可以根据通道的状态数据，从智能合同执行 JSON 数据查询。强烈建议您为 JSON 查询创建索引，并在智能合同中使用这些索引。索引允许应用程序在网络添加更多全局状态的事务和条目区块时高效地检索数据。要了解如何将索引用于智能合同和应用程序，请参阅[使用 CouchDB 时的最佳实践](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-couchdb-indices)。
