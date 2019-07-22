---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: Fabric SDKs, client application, enroll, register, chaincode

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# 使用 Fabric SDK 开发应用程序
{: #dev-app}

{{site.data.keyword.blockchainfull}} Platform 提供了可用于将应用程序连接到区块链网络的 API。可以使用连接概要文件中的网络 API 端点来调用链代码，并更新或查询同级上特定于通道的分类帐。此外，还可以在 [Swagger UI](/docs/services/blockchain/howto?topic=blockchain-ibp-swagger#ibp-swagger) 中使用 API 来管理节点、通道和网络的成员。
{:shortdesc}

您可以使用本教程来学习如何访问 {{site.data.keyword.blockchainfull_notm}} Platform API，并使用这些 API 向网络注册应用程序。您还将了解如何与网络交互以及从应用程序发出事务。本教程基于 Hyperledger Fabric 文档中的 [Writing Your First Application](https://hyperledger-fabric.readthedocs.io/en/release-1.2/write_first_app.html){: external} 教程。您将用到**编写第一个应用程序**教程中的许多相同文件和命令，但这些文件和命令将用于与 {{site.data.keyword.blockchainfull_notm}} Platform 上的网络进行交互。本教程描述了使用 Hyperledger Fabric Node SDK 进行应用程序开发的每个步骤。您还将了解如何使用 Fabric CA 客户机作为使用 SDK 的替代方法注册和登记用户。

除了本教程外，在创建自己的业务解决方案时，还可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 提供的样本应用程序和链代码作为模板。有关更多信息，请参阅[部署样本应用程序](/docs/services/blockchain/howto?topic=blockchain-deploying-sample-applications#deploying-sample-applications)。

准备好缩放应用程序后，请访问[应用程序开发最佳实践](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app)。

## 先决条件
{: #dev-app-prerequisites}

您需要满足以下先决条件，才能在 {{site.data.keyword.blockchainfull_notm}} Platform 上使用**编写第一个应用程序**教程。

- 如果 {{site.data.keyword.cloud_notm}} 上没有区块链网络，那么需要使用入门套餐或企业成员资格套餐创建区块链网络。有关更多信息，请参阅[创建入门套餐网络](/docs/services/blockchain?topic=blockchain-getting-started-with-starter-plan#getting-started-with-starter-plan-creating-a-network)或[创建企业套餐网络](/docs/services/blockchain?topic=blockchain-getting-started-with-enterprise-plan)。

  在进入网络的“网络监视器”后，在“概述”屏幕上至少为组织添加一个同级。然后，在网络中至少创建一个通道。有关更多信息，请参阅[创建通道](/docs/services/blockchain/howto?topic=blockchain-ibp-create-channel#ibp-create-channel-creating-a-channel)。**注**：如果使用的是入门套餐网络，那么网络已具有一个名称为 `defaultchannel` 的通道，您可以使用该通道来部署链代码。

- 安装必需的工具以下载 Hyperledger Fabric 样本并使用 Node SDK。
  * [Curl](https://hyperledger-fabric.readthedocs.io/en/release-1.2/prereqs.html#install-curl){: external} 或 [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git){: external}
  * [Node.js](https://hyperledger-fabric.readthedocs.io/en/release-1.2/prereqs.html#node-js-runtime-and-npm){: external}

- 通过下载 `fabric-samples` 目录来安装 Hyperledger Fabric 样本。您可以遵循 Hyperledger Fabric 文档中的[入门指南](https://hyperledger-fabric.readthedocs.io/en/release-1.2/install.html){: external}。

- 导航到本地计算机上的 `fabric-samples` 目录。
  * 使用 `git checkout` 命令来使用与网络 Hyperledger Fabric 版本匹配的分支。可以通过打开“网络监视器”中的[“网络首选项”窗口](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-network-preferences)来找到 Fabric 版本。
    - 如果您的网络位于 Fabric V1.2 上，您可以使用主要分支。
    - 如果您的网络位于 Fabric V1.1 上，请运行 `git checkout v1.1.0`。
    - 如果您的网络位于 Fabric V1.0 上，请运行 `git checkout v1.0.6`。

  * 导航到 `fabric-samples/fabcar`。您可以复制此目录并对其重命名，以便您可以在干净的目录中尝试测试样本应用程序。

  * 在 `fabcar` 目录中，运行 `npm install` 命令以安装使用 Fabric SDK 所必需的软件包，包括 `fabric-client` 和 `fabric-ca-client`。

- 使用[网络监视器](/docs/services/blockchain/howto?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode-install-cc)在通道上安装和实例化 fabcar 链代码。可以在 `fabric-samples > chaincode > fabcar > go` 文件夹下的 `fabric-samples` 文件夹中找到 fabcar 链代码。

- 在“网络监视器”的“概述”屏幕上检索网络的连接概要文件。将连接概要文件保存到 `fabcar` 目录中，并将其重命名为 `creds.json`。

## 使用 Fabric SDK
{: #dev-app-fabric-sdks}

Hyperledger Fabric SDK 提供了一组功能强大的 API，支持应用程序与区块链网络进行交互。您可以在 [Hyperledger Fabric SDK 社区文档](https://hyperledger-fabric.readthedocs.io/en/release-1.2/getting_started.html#hyperledger-fabric-sdks){: external}中找到受支持语言的最新列表。建议将 Node SDK 或 Java SDK 用于 {{site.data.keyword.blockchainfull_notm}} Platform。您可以了解有关这些 SDK 在其各自存储库中提供的 API 的更多信息。

本教程使用 [Node SDK ](https://fabric-sdk-node.github.io/){: external} 来注册和登记应用程序，然后使用应用程序通过调用和查询链代码来发出事务。本教程描述了需要向 SDK 提供的信息，以便应用程序可以连接到区块链网络。本教程还介绍了一些可以使用的 API，以及 SDK 如何与区块链网络进行交互并向区块链网络提交事务。

## 向应用程序添加网络 API 端点
{: #dev-app-api-endpoints}

您需要在 {{site.data.keyword.cloud_notm}} 上的区块链网络中，向应用程序提供特定网络资源的 API 端点，包括排序节点、CA 和同级节点。应用程序可以通过这些 API 端点与网络进行交互。您可以在网络的连接概要文件中找到 API 端点。连接概要文件为 JSON 格式，包含用于网络资源的 API 端点信息和注册标识/私钥。

1. 使用下列其中一种方法从“网络监视器”中检索网络资源的 API 端点信息：
  * 在“概述”屏幕中，单击**连接概要文件**。连接概要文件包含所有网络资源的一组完整 API 端点的信息。![“网络监视器”中的连接概要文件](images/service_credentials.png "“网络监视器”中的连接概要文件")

  * 如果已在网络中运行链代码，那么可以获取特定于链代码的 API 端点信息。在“通道”屏幕上，单击正在运行链代码的通道行以打开特定通道屏幕。然后找到链代码并单击 **JSON** 按钮。
    ![每个链代码的 API 端点](images/channel_chaincode.png "每个链代码的 API 端点")

2. 找到网络资源的 API 端点信息，这类似于以下示例中 `peer1-org1` 行的 URL：
  ```
"peers": {
            "org1-peer1": {
            "url": "grpcs://n7413e3b503174a58b112d30f3af55016-org1-peer1.us3.blockchain.ibm.com:31002",
            "eventUrl": "grpcs://n7413e3b503174a58b112d30f3af55016-org1-peer1.us3.blockchain.ibm.com:31003",
                  ...
  ```

您可能希望将组织外部的网络资源设定为应用程序的目标。例如，如果链代码[背书策略](/docs/services/blockchain/howto?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode-endorsement-policy)需要来自通道上其他组织的背书，那么您需要将事务发送到足够多的这些组织的同级，以便符合该策略。入门套餐或企业套餐不支持 Hyperledger Fabric 中的[服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.2/discovery-overview.html){: external}。您需要使用连接概要文件的“peers”部分，获取同级的端点信息以及其他组织的附带 TLS 证书。您可以联系其他组织的管理员，了解他们将哪些同级加入到特定通道。
{:note}

3. 将 API 端点信息插入应用程序的配置文件，如以下示例中所示：
  ```
  grpcs://n7413e3b503174a58b112d30f3af55016-orderer.us3.blockchain.ibm.com:31001
  ```

  您还可以将 [HEAD 请求](/docs/services/blockchain/howto?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network-monitor-nodes)发送到这些端点，以检查网络资源的可用性。

  如果使用的是 Fabric SDK，那么还可以使用连接概要文件来连接到网络。本教程提供了手动将 SDK 连接到网络的端点信息。但是，您可以在后面的部分中找到有关[将连接概要文件与 SDK 配合使用](#dev-app-connection-profile)的教程和指南。

## 注册应用程序
{: #dev-app-enroll}

在将应用程序连接到 {{site.data.keyword.blockchainfull_notm}} Platform 上的网络之前，需要向网络证明应用程序的真实性。我们不会深入探讨 x509 证书和公共密钥基础架构的详细信息，但您可以通过
访问[管理 {{site.data.keyword.blockchainfull_notm}} Platform 上的证书](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates)教程来了解更多相关信息。简单来说，Fabric 中的通信流在每个接触点都使用签名/验证操作。因此，任何将调用（例如，分类帐查询或更新）发送到网络的应用程序都需要使用自己的专用密钥对有效内容签名，并附加正确签名的 x509 证书，以用于验证目的。**注册**是从相应的认证中心生成必需的密钥和证书的过程。注册之后，应用程序就可以随时与网络通信了。

本部分说明了如何通过样本代码（**编写第一个应用程序**教程中的一部分内容）检索 Fabric Node SDK 的密钥和证书。您只能使用向认证中心注册的身份来生成证书。以下教程首先使用已向您的 CA 注册的管理员身份进行登记。然后，使用这些证书注册新客户机身份。此教程使用新身份再次登记，并使用这些证书将事务提交到网络<!---You can find an illustration of how the developing applications tutorial interacts with your organization CA in the diagram below.--->

您也可以使用“网络监视器”的“认证中心”屏幕来生成证书，并使用这些证书与网络交互。要了解其方式，请访问[使用网络监视器生成证书](#dev-app-enroll-panel)。通过[管理证书](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates)教程，您还可以了解如何从命令行使用 [Fabric CA 客户机](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates-enroll-register-caclient)以生成证书并注册用户。

### 使用 Fabric SDK 进行注册
{: #dev-app-enroll-sdk}

在文本编辑器中，打开 `fabric-samples` 文件夹中 `fabcar` 目录下的 `enrollAdmin.js` 文件。

1. 该文件会首先创建 Fabric 客户机的实例。
  ```
  var fabric_client = new Fabric_Client();
  ```
  {:codeblock}

2. 然后，该文件将创建键值存储 (KVS) 来管理证书。SDK 使用 [KeyValueStore](https://fabric-sdk-node.github.io/module-api.KeyValueStore.html){: external} 类来创建键值存储，并使用 [CryptoSuite](https://fabric-sdk-node.github.io/module-api.CryptoSuite.html){: external} 类来执行加密计算。您可以在下面看到相关代码块。
  ```
  # 创建键值存储，如 fabric-client/config/default.json 的“key-value-store”设置中所定义
  Fabric_Client.newDefaultKeyValueStore({ path: store_path
    }).then((state_store) => {
   // 将该存储分配给 Fabric 客户机
   fabric_client.setStateStore(state_store);
   var crypto_suite = Fabric_Client.newCryptoSuite();
   // 对状态存储（保存用户证书的位置）和
   // 加密存储（保存用户密钥的位置）使用相同的位置
   var crypto_store = Fabric_Client.newCryptoKeyStore({path: store_path});
   crypto_suite.setCryptoKeyStore(crypto_store);
   fabric_client.setCryptoSuite(crypto_suite);
   var	tlsOptions = {
     trustedRoots: [],
     verify: false
   };
  ```
  {:codeblock}

3. 在定义 KVS 之后，可以使用 [Fabric Client](https://fabric-sdk-node.github.io/Client.html){: external} 类和 Fabric-CA-Client API <!---[FabricCAServices](https://fabric-sdk-node.github.io/FabricCAServices.html){: external} --->中的一些方法与 CA 服务器通信。您将需要向 SDK 提供认证中心的名称和 URL。在“网络监视器”的**概述**屏幕中打开**连接概要文件** JSON 文件，并在 `certificateAuthorites` 部分下找到以下变量：
  * CA 的 URL：``certificateAuthorities`` 下的 ``url``
  * 管理用户标识：``enrollId``
  * 管理密码：``enrollSecret``
  * CA 名称：`caName`

  通过以下方式，使用这些信息来**编辑** `enrollAdmin.js` 文件中的相关行：
  ```
  fabric_ca_client = new Fabric_CA_Client('https://<enrollID>:<enrollSecret>@<ca_url_with_port>', null ,<caName>, crypto_suite);
  ```
  {:codeblock}

  例如：
  ```
  fabric_ca_client = new Fabric_CA_Client('https://admin:dda0c53f7b@n7413e3b503174a58b112d30f3af55016-org1-ca.us3.blockchain.ibm.com:31011', null ,'org1CA', crypto_suite);
  ```

  然后，Fabric 客户机将检查应用程序是否已注册。使用连接概要文件中的 `enrollID` 来**编辑**以下行：
  ```
  return fabric_client.getUserContext('<enrollID>', true);
  ```
  {:codeblock}

4. 您需要向 CA 服务器发送“enroll”调用。您的 `admin` 已向网络进行注册。enroll 调用会检索包装在 x509 证书中并由目标 CA 签名的专用密钥和公用密钥。此包装并签名的证书称为 signCert。signCert 允许网络成员验证源自客户机的调用。您需要通过凭证文件提供组织名称、密码和 MSP 标识。在网络凭证的 `certificateAuthorities` 部分中，找到 `enrollID`、`enrollSecret` 和 `x-mspid`。使用这些值**编辑**下面的代码块，然后替换文件中的相关部分。
  ```
  return fabric_ca_client.enroll({
    enrollmentID: '<enrollID>',
    enrollmentSecret: '<enrollSecret>'
      }).then((enrollment) => {
    console.log('Successfully enrolled admin user');
    return fabric_client.createUser(
         username: 'admin',
            mspid: '<x-mspid>',
            cryptoContent: { privateKeyPEM: enrollment.key.toBytes(), signedCertPEM: enrollment.certificate }
        });
  ```
  {:codeblock}

5. 保存 `enrollAdmin.js` 文件。

在 `fabcar` 目录中，通过发出以下命令来注册 admin：
```
node enrollAdmin.js
```
{:codeblock}

注册命令会生成 signCert，并将其导出到名为 `hfc-key-store` 的文件夹。本教程中的未来文件将在此文件夹中查找证书。如果可以在 `hfc-key-store` 文件夹中找到 admin 证书，说明 enroll 命令正常运行。

如果要[使用 SDK 操作网络](#dev-app-operate-sdk)，那么需要将 admin signCert 上传到 {{site.data.keyword.blockchainfull_notm}} Platform。您可以在 `hfc-key-store` 文件夹中找到 admin signCert。打开 `admin` 文件，然后复制 `certificate` 字段后的引号内的证书。使用工具或文本编辑器将证书转换为 PEM 格式。然后，您可以从“网络监视器”将管理证书上传到区块链网络。有关添加证书的更多信息，请参阅“网络监视器”中[“成员”屏幕的“证书”选项卡](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members)。如果要仅使用 SDK 来调用或查询链代码，那么不必执行此操作。

## 注册应用程序
{: #dev-app-register}

生成客户机端证书之后，需要向网络认证中心注册应用程序。注册会将应用程序添加到网络可以识别的组件的列表中。最佳做法是以单独的身份注册应用程序，而不使用 `admin` 来对请求签名。

### 使用 SDK 进行注册
{: #dev-app-register-sdk}

可以使用 `registerUser.js` 文件通过 `admin` signCert 将应用程序注册为 `user1`。在文本编辑器中打开 `registerUser.js`。

1. 向 Fabric CA 客户机的新实例提供 CA URL 和名称。

2. 向 [Fabric Client 类](https://fabric-sdk-node.github.io/Client.html){: external}中的 `getUserContext` 方法提供 enrollID，以检查您的 `admin` 是否已注册并被允许发出此请求。基于下面的样本，在文件中**编辑**相关代码块：
  ```
  // CA 在启用了 TLS 的情况下运行时，请确保将 http 更改为 https
  fabric_ca_client = new Fabric_CA_Client('https://admin:dda0c53f7b@n7413e3b503174a58b112d30f3af55016-org1-ca.us3.blockchain.ibm.com:31011', null , '<caName>', crypto_suite);

  // 首先检查 admin 是否已注册
  return fabric_client.getUserContext('admin', true);
  }).then((user_from_store) => {
  if (user_from_store && user_from_store.isEnrolled()) {
      console.log('Successfully loaded admin from persistence');
      admin_user = user_from_store;
  } else {
      throw new Error('Failed to get admin.... run enrollAdmin.js');
  }
  ```
  {:codeblock}

3. 使用 **Fabric CA 客户机**向 CA 注册和登记用户，然后使用 **Fabric 客户机**创建新的 signCert。使用 MSP 标识和组织亲缘关系来**编辑**以下块。您可以在网络凭证的 certificate authorities 部分中找到 `x-affiliations`，并使用列出的任何亲缘关系。添加要创建的用户的名称。缺省情况下，fabcar 样本使用 `user1`。
  ```
  return fabric_ca_client.register({enrollmentID: 'user1', affiliation: '<x-affiliations>',role: 'client'}, admin_user)
    ...
  return fabric_ca_client.enroll({enrollmentID: 'user1', enrollmentSecret: secret});
  }).then((enrollment) => {
  console.log('Successfully enrolled member user "user1" ');
  return fabric_client.createUser(
   {username: 'user1',
   mspid: '<x-mspid>',
   cryptoContent: { privateKeyPEM: enrollment.key.toBytes(), signedCertPEM: enrollment.certificate }
   });
  ```
  {:codeblock}

4. 保存 `registerUser.js` 文件。

运行 `node registerUser.js` 命令以注册 `user1`。如果可以在 `hfc-key-store` 文件夹中找到 `user1` 证书，说明该命令正常运行。您只能注册一次身份。如果遇到问题，请尝试使用新的用户名来运行 `registerUser.js`。

### 使用网络监视器进行注册

或者，可以使用“网络监视器”的**认证中心**选项卡来注册客户机应用程序。有关更多指示信息，请参阅此[信息](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-ca)。

## 通过调用和查询链代码来发出事务
{: #dev-app-invoke-query}

应用程序需要与整个区块链网络交互以提交事务。

1. 应用程序发送要由通道上的同级背书的事务建议。
2. 背书同级将已背书事务返回给应用程序。
3. 应用程序将已背书事务发送给排序服务，以用于将该事务添加到分类帐。

有关完整事务处理流程的更多信息，请参阅 Hyperledger Fabric 文档中的 [Transaction Flow](https://hyperledger-fabric.readthedocs.io/en/release-1.2/txflow.html){: external}。开始学习本教程后，请访问[应用程序连接和可用性](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-connectivity-availability)部分，以获取有关管理 SDK 如何与网络进行交互的提示。

以下样本演示了 Node SDK 如何设置网络拓扑，定义事务处理建议，然后将事务提交到网络。可以使用 `invoke.js` 文件来调用 `fabcar` 链代码中的函数。通过这些函数，可以在区块链分类帐上创建和转移资产。本教程使用 `initLedger` 函数向通道添加新数据，然后使用 `query.js` 文件来查询数据。

### 调用链代码
{: #dev-app-invoke}

在文本编辑器中打开 `invoke.js` 文件。

1. 将 `var creds = require('./creds.json')` 添加到该文件的顶部。此代码行允许 `invoke.js` 文件从 `creds.json` 凭证文件中读取信息。

2. 使用 [Fabric Client](https://fabric-sdk-node.github.io/Client.html){: external} 类通过网络资源的 API 端点来设置 Fabric 网络。此步骤定义客户机将向其提交建议的通道和同级，以及 SDK 随后将向其发送已背书事务的排序服务。**编辑**下面的相关代码块。`creds.peers["org1-peer1"].url` 行从凭证文件导入同级 URL。
  ```
  # 设置 Fabric 网络
  var channel = fabric_client.newChannel('defaultchannel');
  var peer = fabric_client.newPeer(creds.peers["org1-peer1"].url, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null});
  channel.addPeer(peer);
  var order = fabric_client.newOrderer(creds.orderers.orderer.url, { pem: creds.orderers.orderer.tlsCACerts.pem , 'ssl-target-name-override': null})
  channel.addOrderer(order);
  ```
  {:codeblock}

  新的同级和排序节点变量会打开与区块链网络的 gRPC 连接。有关管理这些连接的更多信息，请参阅[打开和关闭网络连接](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-connections)。

  将同级 URL 添加到 `fabric_client.newPeer` 方法时，还可使用下面的代码片段从连接概要文件导入相关的 TLS 证书。在添加排序服务 URL 时，执行了相同的操作。您需要使用这些 TLS 证书来认证与网络的通信。
  ```
  { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null}
  ```
  {:codeblock}

  如果背书策略要求事务由通道中的其他组织进行背书，那么需要在设置网络时使用 `newPeer()` 和 `channel.addPeer()` 方法来添加这些同级组织。组织会向您发送他们已加入特定通道的同级的列表。连接概要文件中将提供端点信息和 TLS 证书。然后，SDK 会将事务发送给添加到通道的所有同级。

  在[使应用程序高度可用](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-ha-app)中，还可以添加属于您的组织并已加入通道的其他同级。这将为 SDK 提供其中一个同级中断时执行故障转移的功能。

3. 设置 Fabric 网络并在注册步骤中导入应用程序身份和 signCert 之后，`invoke.js` 文件会定义将提交给网络的建议。可以使用 `fabcar` 链代码中的 `initLedger` 函数向分类帐添加一些初始数据。还可以编辑代码块以调用可在 `fabcar` 链代码中找到的其他函数。
  ```
  var request = {
    //targets: let default to the peer assigned to the client
    chaincodeId: 'fabcar',
    fcn: 'initLedger',
    args: [''],
    chainId: 'defaultchannel',
    txId: tx_id
  };
  ```
  {:codeblock}

  定义请求后，可以将[事务处理建议](https://fabric-sdk-node.github.io/Channel.html#sendTransactionProposal){: external}发送给通道上的同级。从同级返回建议后，可以向排序服务[发送事务](https://fabric-sdk-node.github.io/Channel.html#sendTransaction){: external}。

4. 可以添加事件服务来提高事务流的效率。请**编辑**以下部分：
  ```
  let event_hub = fabric_client.newEventHub();
  event_hub.setPeerAddr(creds.peers["org1-peer1"].eventUrl, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null});
  ```
  {:codeblock}

  虽然样本使用的是基于同级的事件服务，但您应该使用基于通道的侦听器。您可以在[管理事务](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-managing-transactions)部分和 [Node SDK 文档](https://fabric-sdk-node.github.io/tutorial-channel-events.html){: external}中了解更多信息。

5. 缺省情况下，`invoke.js` 以 `user1` 身份提交事务。如果注册了其他名称，那么可以编辑 `invoke.js` 文件。

完成文件编辑后，发出 `node invokke.js` 命令以将事务提交到网络。您应该会看到以下内容，这是对成功调用的确认。
```
Successfully sent Proposal and received ProposalResponse: Status - 200, message - "OK"
```

这指示应用程序已成功调用链代码并将数据添加到分类帐。

### 查询链代码
{: #dev-app-query}

现在，您可以使用 `query.js` 来读取分类帐。在文本编辑器中打开 `query.js` 文件。

1. 将 `var creds = require('./creds.json')` 添加到该文件的顶部。
2. 使用同级的通道名称和端点信息更新该文件。由于此操作仅读取存储在同级上的数据，因此无需添加排序服务的端点信息。`query.js` 还假定您以 `user1` 身份发送建议。
  ```
  var channel = fabric_client.newChannel('defaultchannel');
  var peer = fabric_client.newPeer(creds.peers["org1-peer1"].url, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null});
  channel.addPeer(peer);
  ```
  {:codeblock}

完成编辑文件后，发出 `node query.js` 命令，您应该会看到分类帐上的汽车列表，这类似于以下示例。
```
Query has completed, checking results
Response is  
  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},
  {"Key":"CAR1", "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},
  ...
```
{:codeblock}

有关 fabcar 应用程序及其使用的函数的更多信息，可以访问 Hyperledger Fabric 文档中的完整教程 [Writing Your First Application](https://hyperledger-fabric.readthedocs.io/en/release-1.2/write_first_app.html){: external}。

## 将连接概要文件与 SDK 配合使用
{: #dev-app-connection-profile}

您可以使用“网络监视器”的**概述**屏幕中的**连接概要文件**使 SDK 连接到网络，而不手动导入网络的端点信息。这简化了连接到认证中心以进行注册的过程。此外，也无需在提交事务之前定义 Fabric 网络。SDK 将直接在连接概要文件中找到相关通道上的同级和排序节点。您可以在 [Node SDK 文档](https://fabric-sdk-node.github.io/tutorial-network-config.html){: external}中找到有关如何使用连接概要文件的更多信息。

我们将 `invoke.js` 文件用作示例来查看使用连接概要文件（而不是手动端点）的效率。可以使用 `loadFromConfig` 类来建立 Fabric 客户机的新实例。将 `var fabric_client = new Fabric_Client();` 替换为以下代码。
```
var fabric_client = Fabric_Client.loadFromConfig(path.join(__dirname, './connection-profile.json'));
```
{:codeblock}

可以使用以下行来创建新的通道，而不通过创建同级和排序节点，然后将它们添加到通道来设置 Fabric 网络。

```
var channel = fabric_client.newChannel('defaultchannel');
```
{:codeblock}

然后，SDK 将添加使用连接概要文件在通道上定义的同级和排序服务。这将提高编写应用程序的效率，让您在网络成员连接、离开和启动新通道时，更轻松地更新应用程序。请参阅 Node SDK 文档中的[连接概要文件教程](https://fabric-sdk-node.github.io/tutorial-network-config.html){: external}，以了解有关所涉及的其他步骤的信息。可以使用此 [fabcar 教程版本](https://developer.ibm.com/tutorials/cl-deploy-fabcar-sample-application-ibm-blockchain-starter-plan/){: external}，该教程使用的是连接概要文件，而不是手动端点连接。

入门套餐或企业套餐不支持 Hyperledger Fabric 中对背书策略的[服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.2/discovery-overview.html){: external}。但是，通过编辑连接概要文件，可以将事务发送给组织外部的同级进行背书。连接概要文件已包含来自 {{site.data.keyword.blockchainfull_notm}} Platform 网络上其他组织中同级的端点信息和 TLS 证书。将同级的名称添加到概要文件的“channels”部分中的相关通道，以将同级添加到通道。您需要联系其他组织的管理员，了解他们将哪些同级加入到特定通道。


## 使用网络监视器生成证书
{: #dev-app-enroll-panel}

您可以通过“网络监视器”使用管理员身份生成证书，然后将这些证书直接传递到 SDK。这意味着您可以快速开始与网络交互，而不必使用 SDK 生成证书。

浏览至“网络监视器”的“认证中心”面板。单击您的管理员身份旁边的**生成证书**按钮，以从您的 CA 获取新的 signCert 和专用密钥。**证书**字段包含 signCert，刚好位于**专用密钥**上方。您可以单击每个字段末尾的复制图标以复制值。将这些证书保存在您可以将其提供给应用程序的位置。**请注意**，{{site.data.keyword.blockchainfull_notm}} Platform 不存储这些证书。您需要安全地保存并存储它们。

signCert 和专用密钥足以构成用户上下文，该上下文可以对 Node SDK 中的请求进行签名。使用 Client 类的 [createUser](https://fabric-sdk-node.github.io/Client.html#createUser__anchor){: external} 方法来创建用户上下文对象。在 `creatUser` 方法中，将身份名称和 mspid 传递给 [user](https://fabric-sdk-node.github.io/global.html#UserOpts){: external} 对象，同时将专用密钥和 signCert 的路径传递给 [cryptoContent](https://fabric-sdk-node.github.io/global.html#CryptoContent){: external} 对象。

作为示例，我们可以使用“认证中心”面板和 `createUser` 类作为开发应用程序教程的一部分。如果您已完成本教程，并且随后已安装并实例化 `fabcar` 链代码，并将一些数据添加到分类帐中。我们可以使用这些证书以 `admin` 用户身份查询分类帐。如果尚未使用上述“网络监视器”生成证书，请遵循指示信息完成此操作。

将专用密钥保存为 privateKey.pem，将 signCert 保存为 certificate.pem，并存储在 `query.js` 所在的相同目录中。在文本编辑器中打开 `query.js`。将以下行添加到文件的顶部：
```
var fs = require('fs');
```
将用于从持久状态导入用户上下文的以下行
```
return fabric_client.getUserContext('user1', true);
```
替换为使用 signCert 和专用密钥创建新用户上下文的以下代码。
```
return fabric_client.createUser({
		username: 'admin',
		mspid:  'org1',
		cryptoContent: {
			privateKeyPEM: fs.readFileSync(path.join(__dirname,'./privateKey.pem')),
			signedCertPEM: fs.readFileSync(path.join(__dirname,'./certificate.pem'))
		}});
```
以上片段将证书作为 PEM 文件直接读取到 `cryptoContent` 类。由于证书是使用 `admin` 身份生成的，因此用户名将为 `admin`。您可以在连接概要文件的 `certificateAuthorites` 部分中找到 mspid。保存文件并发出命令 `node query.js`。如果成功，查询将返回与之前相同的结果。

## （可选）使用 SDK 操作网络
{: #dev-app-operate-sdk}

您还可以使用 SDK 来操作区块链网络。本教程说明了如何使用 SDK 将同级加入通道，在同级上安装链代码以及在通道上实例化链代码。这些步骤是可选的，因为如果所有同级都正在 {{site.data.keyword.blockchainfull_notm}} Platform 上运行，那么还可以在 [Swagger UI](/docs/services/blockchain/howto?topic=blockchain-ibp-swagger#ibp-swagger) 中使用“网络监视器”或 API 来执行这些操作。

您需要将 admin signCert 上传到 {{site.data.keyword.blockchainfull_notm}} Platform 来完成这些步骤。您可以在[注册部分](#dev-app-enroll-sdk)末尾找到有关如何上传 signCert 的指示信息。

### 加入通道
{: #dev-app-join-channel-sdk}

组织使用“网络监视器”或 API 创建或加入通道后，您可以使用 SDK 将同级加入该通道。

1. 通过排序服务[访存通道的起源区块](https://fabric-sdk-node.github.io/Channel.html#getGenesisBlock){: external}。
2. 将起源区块传递到 [joinChannel](https://fabric-sdk-node.github.io/Channel.html#joinChannel){: external} 方法，以将同级加入通道。

要使用 `fabcar` 样本来加入通道，请使用 `invoke.js` 文件作为起始点。您需要以 admin 身份（而非应用程序用户）发送此请求，因此请将 `getUserContext` 方法中的 `user1` 替换为 `admin`。从 `var request = {` 处定义链代码调用请求的位置开始，根据 [Node SDK 文档](https://fabric-sdk-node.github.io/tutorial-channel-create.html){: external}中的以下代码片段，将事务处理流程替换为通道加入请求。
  ```
  let g_request = {
    txId :     tx_id
  };

  // 从排序节点那里获取起源区块
  channel.getGenesisBlock(g_request).then((block) =>{
    genesis_block = block;
    tx_id = client.newTransactionID();
    let j_request = {
      targets : targets,
      block : genesis_block,
      txId : tx_id
    };
    // 向同级发送起源区块
    return channel.joinChannel(j_request);
  }).then((results) =>{
    if(results && results[0].response && results[0].response.status == 200) {
    // 良好
    } else {
      // 不佳
    }
  });
  ```

需要先将 signCert 添加到通道，才能访存起源区块。如果在组织加入通道后生成了证书，那么需要将您的 signCert 上传到平台，然后单击“通道”屏幕中的**同步证书**按钮。在发出加入通道的命令之前，您可能需要等待几分钟，以便通道完成同步。有关更多信息，请参阅[管理证书](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates)教程中的[将签名证书上传到 {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates-upload-certs)。

### 安装链代码
{: #dev-app-install-cc-sdk}

可以使用 [Fabric Client](https://fabric-sdk-node.github.io/Client.html){: external} 类中的 [installChaincode](https://fabric-sdk-node.github.io/Client.html#installChaincode){: external} 方法在同级上安装链代码。

要使用 `fabcar` 样本在同级上安装 `fabcar` 链代码，请使用 `query.js` 文件作为基线并对其进行编辑。您需要以 admin 身份（而非应用程序用户）发送此请求，因此请将 `getUserContext` 方法中的 `user1` 替换为 `admin`。使用以下示例将事务建议对象替换为安装链代码请求：
```
var request = {
    targets: peer,
    chaincodePath: chaincode_path,
    chaincodeId: 'fabcar',
    chaincodeType: 'golang',
    chaincodeVersion: 'v1',
    channelNames: 'defaultchannel'
	 };
```
{:codeblock}

将此对象发送到 `return fabric_client.installChaincode(request);`，而不是当前该文件中的 `return channel.queryByChaincode(request);` 行。

### 实例化链代码
{: #dev-app-instantiate-cc-sdk}

要实例化链代码，您需要向同级发送[实例化建议](https://fabric-sdk-node.github.io/Channel.html#sendInstantiateProposal){: external}，然后向排序服务发送[事务处理请求](https://fabric-sdk-node.github.io/Channel.html#sendTransaction){: external}。

要使用 `fabcar` 样本来实例化链代码，请使用 `invoke.js` 文件作为起始点。您需要以 admin 身份（而非应用程序用户）发送此请求，因此请将 `getUserContext` 方法中的 `user1` 替换为 `admin`。使用以下示例将事务建议对象替换为安装链代码请求：
```
var request = {
    targets: peer,
    chaincodePath: chaincode_path,
    chaincodeId: 'fabcar',
    chaincodeType: 'golang',
    chaincodeVersion: 'v1',
    channelNames: 'defaultchannel',
    txId : tx_id
};
```
{:codeblock}

将此请求发送到 `return channel.sendInstantiateProposal(request);`，而不是当前该文件中的 `return channel.sendTransactionProposal(request);`。将实例化请求发送到通道后，您需要将已背书建议作为事务发送给排序服务。此操作使用的方法与发送事务的方法相同，因此您可以使文件其余部分保持不变。您可能需要在实例化建议中[增大超时值](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-set-timeout-in-sdk)。否则，在平台可以启动链代码容器之前，该请求可能会超时。

需要先将 signCert 添加到通道，才能实例化链代码。如果在您加入通道后生成了证书，那么需要将您的 signCert 上传到平台，然后单击“通道”屏幕中的**同步证书**按钮。在发出实例化链代码的命令之前，您可能需要等待几分钟，以便通道完成同步。要了解更多信息，请参阅[管理证书](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates)教程中的[将签名证书上传到 {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates-upload-certs)。

## 断开应用程序与网络的连接
{: #dev-app-disconnect-app}

完成以下步骤以除去应用程序与 {{site.data.keyword.cloud_notm}} 上的区块链网络之间的连接。
1. 从应用程序配置文件中除去 API 端点信息。有关参考信息，请参阅[将网络 API 端点添加到应用程序](#dev-app-api-endpoints)。
2. 删除链代码容器。
  1. 在“网络监视器”的“通道”屏幕中，找到已安装链代码的通道。
  2. 在特定通道屏幕中，找到要禁用的链代码。
  3. 单击**删除**按钮，然后单击链代码删除面板中的**提交**。此时将除去链代码容器。
	![删除链代码](images/channel_chaincode.png "删除链代码")
