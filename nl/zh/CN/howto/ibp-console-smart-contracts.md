---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: smart contract, private data, private data collection, anchor peer

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

# 在网络上部署智能合同教程
{: #ibp-console-smart-contracts}

智能合同是允许您在区块链分类帐上读取和更新数据的代码，有时称为链代码。智能合同可以将业务逻辑转变为区块链网络的所有成员同意并验证的可执行程序。本教程是[样本网络教程系列](#ibp-console-smart-contracts-structure)的第三部分，描述了如何部署智能合同以在区块链网络中开始处理事务。
{:shortdesc}

如果使用的是 Beta 试用版 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}，那么控制台中的某些面板可能与当前的文档不一致，该文档对应于一般可用 (GA) 服务实例并保持最新。要利用所有最新的功能，目前建议您遵循 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 入门](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)中的指示信息来供应新的 GA 服务实例。
{: important}

**目标受众：**本主题适用于负责创建、监视和管理区块链网络的网络操作员。此外，应用程序开发者可能对说明如何创建智能合同的部分感兴趣。

## 样本网络教程系列
{: #ibp-console-smart-contracts-structure}

本教程系列分为三个部分，可指导您完成以下过程：使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台将网络部署到 Kubernetes 集群，并安装和实例化智能合同，以创建和互连相对简单的多节点 Hyperledger Fabric 网络。

* [构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)将指导您完成通过创建排序节点和同级来托管网络的过程。
* [加入网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network)将指导您完成通过创建同级并将其加入通道来加入现有网络的过程。
* **在网络上部署智能合同**（即本教程）提供了有关如何编写智能合同并将其部署在网络上的信息。

可以使用这些教程中的步骤在一个集群中构建具有多个组织的网络，以用于开发和测试目的。如果要通过创建排序节点和添加组织来构成区块链联盟，请使用**构建网络**教程。使用**加入网络**教程将同级连接到网络。通过遵循这些教程对其他联盟成员执行相应操作，可以创建真正的**分布式**区块链网络。  

这最后一个教程旨在说明如何创建和打包智能合同，如何在同级上安装智能合同，以及如何在通道上实例化智能合同。  

智能合同安装在同级上，然后在通道上进行实例化。**要使用智能合同提交事务或读取数据的所有成员都需要在其同级上安装该合同。**智能合同通过其名称和版本进行定义。因此，在通道中计划运行该智能合同的所有同级上，安装的合同的名称和版本需要保持一致。

在同级上安装智能合同后，单个网络成员可在通道上实例化该合同。网络成员需要加入通道才能执行此操作。实例化会使用智能合同所用的初始数据来更新分类帐，然后在已加入通道且安装了该合同的同级上启动智能合同容器。然后，同级可使用正在运行的容器以进行交易。  
- **仅一个网络成员需要实例化智能合同。**
- **如果已安装智能合同的同级加入的通道中已实例化相同的智能合同版本，那么智能合同容器将自动启动。**  

在本教程中，我们将完成使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台来管理网络上的智能合同部署的步骤：

- [在同级上安装智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install)。
- [在通道上实例化智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate)。
- [指定背书策略](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse)。
- [更新智能合同策略和代码](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade)。


## 开始之前

要能够安装智能合同，请先确保以下各项已准备就绪。

- 必须使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台来[构建网络](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)或[加入网络](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network)。
- 由于智能合同是安装在同级上，因此请确保向控制台[添加同级](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peer-org1)。  
- 在通道上实例化智能合同。因此，必须使用控制台来[创建通道](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel)或[加入通道](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)。

## 步骤 1：编写智能合同

{{site.data.keyword.blockchainfull_notm}} 控制台管理的是智能合同的*部署*，而不是开发。如果您感兴趣的是开发智能合同，可以从 Hyperledger Fabric 社区提供的教程和 {{site.data.keyword.IBM_notm}} 提供的工具入手。

- 要了解如何使用智能合同在多方之间进行事务处理，请参阅 Hyperledger Fabric 文档中的 [Developing Applications](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} 主题。
- 准备好开始构建智能合同时，可以使用 [{{site.data.keyword.blockchainfull_notm}} Visual Studio Code 扩展](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external}来开始构建您自己的智能合同项目。还可以使用该扩展[通过 Visual Studio Code 直接连接到网络](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-vscode)，并探索内联教程。
- 有关开发智能合同的快速教程，请参阅 [Develop a smart contract with the IBM Blockchain Platform VS Code extension](https://developer.ibm.com/tutorials/ibm-blockchain-platform-vscode-smart-contract/){: external}。
- 有关使用应用程序与智能合同进行交互的更深入端到端教程，请参阅 [Hyperledger Fabric Commercial paper tutorial](https://hyperledger-fabric.readthedocs.io/en/release-1.4/tutorial/commercial_paper.html){: external}。
- 要了解如何将访问控制机制合并到智能合同中，请参阅 [Chaincode for Developers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4ade.html#chaincode-access-control){: external}。
- 准备好安装时，必须将智能合同打包成 [.cds 格式](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external}，这样才能将其安装到同级上。有关更多信息，请参阅[打包智能合同](/docs/services/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract)。或者，可以使用 [peer cli 命令](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/peerchaincode.html#peer-chaincode-package){: external}来构建包。
<!-- Update the tutorial link to release1-4 when it is published -->


## 步骤 2：安装智能合同
{: #ibp-console-smart-contracts-install}

使用控制台来执行以下步骤：

1. 单击**智能合同**选项卡以安装一个或多个智能合同。
2. 单击**安装智能合同**以上传 [.cds 格式](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external}的智能合同包文件。_智能合同包文件的大小必须小于 4 MB。_可以使用 {{site.data.keyword.blockchainfull_notm}} Visual Studio Code 扩展来[创建智能合同包](/docs/services/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract)。在**智能合同**选项卡中安装包时，可以选择一个或多个要安装智能合同的同级节点。

如果控制台中只存在一个同级，那么智能合同会安装在该同级上。系统不会提示您选择要安装智能合同的同级。您可以导航至“节点”选项卡，然后单击由控制台管理的某个同级，以查看已在单个同级上安装的智能合同的列表。

以后可返回到**智能合同**选项卡以在其他同级上安装该智能合同，即使在通道上已实例化该智能合同之后也可执行此操作。如果安装的智能合同版本与实例化的版本相同，那么这些新增的同级可以像现有同级一样处理事务。
{:tip}

**此控制台不能用于安装 Hyperledger Composer `.bna` 文件。**

<!-- Instead, `.bna` files must be installed on a peer by using the [`Composer` CLI commands]("peer chaincode" "peer chaincode").  -->

## 步骤 3：实例化智能合同
{: #ibp-console-smart-contracts-instantiate}

在通道上实例化智能合同。具有加入通道的同级的任何控制台成员都可以实例化智能合同并指定关联的[背书策略](/docs/services/blockchain?topic=blockchain-glossary#glossary-endorsement-policy)。

使用控制台来执行以下步骤：

1. 在“智能合同”选项卡上，从同级上安装的智能合同列表中找到相应智能合同，然后单击行右侧溢出菜单中的**实例化**。
2. 在打开的侧面板上，选择要在其中实例化智能合同的通道。可以选择已创建的名为 `channel1` 的通道。然后，单击**下一步**。
3. 指定[智能合同的背书策略](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse)，如以下部分中所述。当多个组织是通道的成员时，您有机会选择需要多少个组织来对智能合同事务背书。
4. 还需要选择要包含在背书策略中的组织成员。如果您是按部就班地学习本教程，那么在完成了**构建网络**和**加入网络**教程后，组织成员将为 `org1msp`，也可能为 `org2msp`。
5. 如果智能合同包含 Fabric 专用数据集合，那么需要上传关联的集合配置 JSON 文件，否则可以跳过此步骤。请参阅有关使用[专用数据](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)的主题，以获取更多信息。
6. 在最后一个面板上，系统会提示您指定智能合同启动时要运行的智能合同函数，以及要传递给该函数的关联自变量。

通过单击左侧导航中的“通道”图标，从表中选择一个通道，然后单击**通道详细信息**选项卡，可以查看已在通道上实例化的所有智能合同。

请注意，如果使用的是免费 {{site.data.keyword.cloud_notm}} Kubernetes Service 集群，那么实例化所用时间可能比付费集群长得多。实例化可能需要几分钟时间，具体取决于集群中部署的同级数。

**安装和实例化**组合是一个强大的功能，因为它支持同级在多个通道上使用单个智能合同。同级可能希望加入使用同一智能合同，但能够访问数据的网络成员组不同的多个通道。同级可安装智能合同一次，然后在已实例化该合同的任何通道上使用相同的智能合同容器。此轻量级方法节省计算和存储空间，并帮助缩放网络。

## 步骤 4：使用客户机应用程序发送事务
{: #ibp-console-smart-contracts-connect-to-SDK}

智能合同在通道上实例化后，您可以使用客户机应用程序与网络中的其他成员之间进行事务处理。应用程序可以调用智能合同中包含的业务逻辑，以在区块链分类帐上创建、转让或更新资产。

### 使用 SDK 进行连接
{: #ibp-console-smart-contracts-connect-to-SDK-panel}
**智能合同**选项卡包含从客户机应用程序连接到已实例化智能合同所需的信息。导航至每个已实例化的智能合同旁边的溢出菜单。单击**使用 SDK 进行连接**按钮。这将打开一个侧面板，其中提供连接到此智能合同所需的信息：合同名称、通道名称和连接概要文件。有关更多信息，请参阅[创建应用程序](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app)。

## 指定背书策略
{: #ibp-console-smart-contracts-endorse}

每个智能合同都必须有一个背书策略，该策略在智能合同实例化期间指定。背书策略指定通道上可以执行该智能合同并独立验证事务输出的组织集，即同级。例如，背书策略可以指定仅当通道上的大多数成员对某个事务背书时，才会将该事务添加到分类帐。对智能合同进行实例化的组织可以从已安装智能合同的成员中选择成员，使其成为验证者，并设置用于所有通道成员的背书策略。通过执行[更新智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade)的步骤，可以更新背书策略。

执行[实例化智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate)的步骤时，在选择通道后，可以使用侧面板来设置合同的背书策略。使用此面板通过从已在通道上安装智能合同的同级列表中选择需要对事务背书的同级来指定简单策略。可以使用此方法来指定背书策略：所有通道成员、大多数通道成员、单个成员，或防止成员自签名的简单 +1 策略。缺省背书策略设置为 `1/x`，这意味着只需要一个成员来对智能合同事务背书。

如果要指定 JSON 格式的策略，请单击**高级**按钮。可以使用此方法来指定更复杂的背书策略，例如要求通道的某个成员必须与大多数其他成员一起验证事务。可以在 Hyperledger Fabric 文档中找到其他[高级背书策略示例](https://hyperledger-fabric.readthedocs.io/en/release-1.4/arch-deep-dive.html#example-endorsement-policies){: external}。有关使用 JSON 编写背书策略的更多信息，请参阅 [Hyperledger Fabric Node SDK 文档](https://fabric-sdk-node.github.io/global.html#ChaincodeInstantiateUpgradeRequest){: external}。

## 升级智能合同
{: #ibp-console-smart-contracts-upgrade}

您可以升级智能合同以更改其代码、背书策略或专用数据集合，同时保持它与分类帐上资产的关系。您可能希望升级已实例化智能合同的原因有多种。
1. 可以升级智能合同以在其代码中添加或除去功能，并迭代业务网络的逻辑。
2. 每当将新成员添加到通道时，都*必须*更新已实例化智能合同的背书策略，以包含新通道成员。为了使用新的通道成员，智能合同必须使用新的版本号重新打包并在通道上进行实例化，即使智能合同本身没有变化。否则，无法成功对事务背书。
3. 专用数据集合发生更改（例如添加或除去了组织）时，需要升级智能合同。或者，每当将新的专用数据集合添加到集合配置 JSON 文件时，都使用此操作。
4. 更改了智能合同初始化自变量。

**在升级已实例化的智能合同之前，必须在通道中运行先前级别智能合同的所有同级上安装智能合同的新版本。**

### 如何升级智能合同
{: #ibp-console-smart-contracts-upgrade-howto}

要升级智能合同，请通过指定相同的智能合同名称但使用新的版本号来安装更新的代码。如果已在通道中的任何同级上安装了更新版本的智能合同，请注意在**已实例化的智能合同**表中，现在原始版本旁边会有`升级可用`按钮。

{:important}
在将要运行智能合同的新成员加入通道时，该成员必须通过在所有同级上安装新版本，并使用修改后包含新成员的背书策略在通道上实例化智能合同，以更新智能合同。

- 导航至左侧的**智能合同**选项卡。
- 向下滚动到**已实例化的智能合同**表。
- 在**已实例化的智能合同**表中，找到要升级的智能合同版本和通道。该合同旁边必定有**升级可用**标签。
- 单击智能合同行右侧的**溢出菜单**，然后单击**升级**以执行以下步骤：  

 1. 从下拉列表中选择要在通道上升级的智能合同版本。
 2. 通过添加或除去通道成员来更新背书策略。还可以单击**高级**以粘贴新的 JSON 格式字符串，该字符串用于修改现有策略。
 3. 如果要将专用数据集合配置文件与智能合同相关联，可以上传 JSON 文件。或者，如果要更新现有集合配置，也可以上传 JSON 文件。   
如果智能合同先前已使用集合配置文件进行实例化，那么**必须**在此步骤中重新上传集合配置文件的先前版本或新版本。  
 4. （可选）如果参数已更改，请修改智能合同初始化自变量值。如果不确定参数是否更改，请咨询智能合同开发者。如果没有更改，那么可以将此字段保留为空白。

升级智能合同后，请更改在通道上实例化的合同版本，然后更改已安装新版本的所有同级的智能合同容器。如果要使用专用数据集合，请确保已在通道上配置了锚点同级。

### 升级智能合同时的注意事项
{: #ibp-console-smart-contracts-upgrade-considerations}

1. 需要在所有同级上安装智能合同的新版本吗？  

  在同级上调用智能合同时，它会尝试运行已在通道上实例化的版本。如果先前版本正在同级上运行，那么会导致错误。因此，在升级通道上的智能合同之前，*最佳做法是在运行先前版本的所有同级上安装智能合同的最新版本*。  

2. 后续智能合同版本仍可以处理先前版本智能合同中分类帐上的数据吗？  

  对。只要新的智能合同代码能以兼容的方式处理数据（通过添加新的 JSON 字段，而不更改现有字段的语义或类型），智能合同的任何后续版本都可以针对先前版本的数据运行。

3. 如果更新通道上的智能合同之前忘记将其升级到最新版本，同级会发生什么情况？  

  在更新通道以使用智能合同的新版本之后，如果通道上仍有同级在运行先前版本，那么这些同级无法再对智能合同的事务背书。此外，根据智能合同背书策略的定义方式，您会面临事务没有足够背书而无法提交给分类帐的风险。但是，以后可以在这些同级上安装智能合同的新版本，然后这些同级就能再次对事务背书，从而实际跟上其他同级的步伐。

4. 从专用数据集合中除去组织时会发生什么情况？

   该组织中的同级会继续将数据存储在专用数据集合中，直到其分类帐到达从该集合中除去其成员资格的区块为止。在此之后，同级就不会再接收任何未来事务中的专用数据，并且该组织的_客户机_无法再从任何同级通过链代码来查询专用数据。

## 专用数据
{: #ibp-console-smart-contracts-private-data}

专用数据是 Hyperledger Fabric 网络（V1.2 或更高版本）的一项功能，用于**在通道上**使来自其他组织成员的敏感信息保持专用。通过使用[专用数据集合](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "What is a private data collection?"){: external}可实现数据隐私。例如，多家批发商和一组农场主可能会加入一个通道。如果某个农场主和某个批发商希望私下进行交易，他们可以创建用于此目的的通道。但是，他们还可以决定在智能合同上创建专用数据集合，用于管理其业务交互，针对销售的敏感方面（例如，价格）保持隐私，而无需创建辅助通道。要了解有关何时在区块链中使用专用数据的更多信息，请访问 Fabric 文档中的[专用数据](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external}概念文章。

为了将专用数据用于 {{site.data.keyword.blockchainfull_notm}} Platform，必须满足以下三个条件：  
1. **定义专用数据集合。**可以将专用数据集合文件添加到智能合同。然后，在运行时，客户机应用程序可以使用特定于专用数据的链代码 API 在集合中输入和检索数据。有关如何在智能合同中使用专用数据集合的更多信息，请参阅 Fabric SDK 文档中有关[使用专用数据](https://fabric-sdk-node.github.io/tutorial-private-data.html){: external}的 Fabric SDK 教程。  

2. **安装和实例化智能合同。**定义了智能合同专用数据集合后，需要在作为通道成员的同级上安装智能合同。使用控制台在通道上实例化智能合同时，需要上传集合配置 JSON 文件。有关如何[创建集合定义 JSON 文件](https://fabric-sdk-node.github.io/tutorial-private-data.html){: external}的更多信息，请参阅 Fabric SDK 文档主题。

  除了使用控制台来安装智能合同并通过集合配置文件实例化智能合同，您还可以使用 Fabric SDK。这些指示信息还可在 Node SDK 文档中的 [How to use private data](https://fabric-sdk-node.github.io/release-1.4/tutorial-private-data.html){: external} 下找到。  

  **注：**客户机需要成为同级的管理员，才能使用 SDK 来安装或实例化智能合同。因此，需要从控制台电子钱包中下载同级管理员身份的证书，并将同级管理员的签名证书和专用密钥直接传递给 SDK，而不是创建应用程序身份。有关如何将密钥对传递给 SDK 的示例，请参阅[使用低级别 Fabric SDK API 连接到网络](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-low-level)。  


3. **配置锚点同级。**由于必须启用跨组织的 [Gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external}，专用数据才能起作用，因此在集合定义中每个组织必须存在一个锚点同级。请参阅有关在网络上[如何配置锚点同级](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers)的信息。

现在，通道已配置为使用专用数据。
