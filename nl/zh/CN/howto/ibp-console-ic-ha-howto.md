---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# 设置多区域高可用性 (HA) 部署
{: #ibp-console-hadr-mr}

通过多区域 HA 配置，可提供尽可能最大程度的 HA 覆盖范围。在多个地理区域中部署同级可确保如果任何一个区域变得不可用，其他区域中的同级可以继续进行事务处理。请注意，目前多区域 HA 支持不可用于 CA 和排序服务。

## 概述
{: #ibp-console-hadr-overview}

为同级设置多区域 HA 支持时，将执行以下任务：
- 在 {{site.data.keyword.cloud}} 中创建多个服务实例，每个实例绑定到不同区域中的 Kubernetes 集群。
- 在不同区域中创建区块链节点。
- 使用节点导出/导入功能通过单个控制台来管理节点。

## 配置步骤
{: #ibp-console-hadr-config}

要通过为每个组织创建冗余同级来配置多区域 HA，请在配置区块链网络时完成以下步骤：

1. 在您偏好使用的区域中创建三个 {{site.data.keyword.cloud_notm}} Kubernetes 集群。这些集群可以位于您所需的任何区域中，但要实现高性能，集群应该彼此靠得相对近一些。例如，“美国东海岸”、“美国西海岸”和“加拿大”区域优于“美国西海岸”、“伦敦”和“东京”区域。
2. 部署新的 {{site.data.keyword.blockchainfull_notm}} Platform 实例，并将其链接到第一个区域中的集群。然后，部署另一个 {{site.data.keyword.blockchainfull_notm}} Platform 实例，并将其链接到第二个区域中的集群。重复这些步骤将第三个服务实例链接到第三个区域中的集群。完成后，您将有三个单独的 {{site.data.keyword.blockchainfull_notm}} Platform 实例链接到三个单独的集群，每个集群分别位于不同的区域中，还有三个单独的控制台。

本教程假定存在一个排序服务，其中定义了同级可以加入的通道。
{: important}

### 步骤 1：在集群 1 中创建同级组织的 CA 和元数据
{: #ibp-console-hadr-peerCA}

1. 通过遵循“构建网络”教程中有关[创建同级组织的 CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA) 的指示信息，在第一个集群中部署 CA。记下 CA **注册标识**和**私钥**的值，因为在将 CA 导入到其他集群时需要提供这些值。
2. CA 磁贴状态指示器变为绿色（表示`正在运行`）后，请遵循指示信息[向 CA 注册身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1)。在这些指示信息中，您将创建两个身份，一个用于同级，另一个用于同级的组织管理员身份。
3. 针对第一个集群中同级的组织，[创建同级的组织 MSP 定义](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1)。
4. 在第一个集群中[创建同级](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create)。
5. 在同级上[安装智能合同](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install)。

您需要执行以下步骤将同级加入排序服务上的通道，然后才能在通道上实例化智能合同：
- [导出同级的组织定义](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote)，并与排序服务管理员共享该定义。
- 排序服务管理员需要执行[导入同级的组织](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp)的步骤，并将其添加到联盟。管理员还需要完成这些指示信息中的相应步骤以将排序服务导出到 JSON 文件。
- [导入排序服务 JSON 文件](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer)至控制台。
- [将同级加入通道](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)。
- 最后，如果使用服务发现、专用数据和同级 Gossip，那么排序服务管理员需要在通道上[配置锚点同级](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers)。对于 HA，建议将每个冗余同级都添加为锚点同级。这样一来，如果其中一个同级不可用，组织之间的 Gossip 仍可以继续。   

既然已将同级加入通道，接下来可以在通道上[实例化智能合同](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)。

### 步骤 2：从集群 1 中导出元数据和身份
{: #ibp-console-hadr-export-meta1}

1. 将 CA 定义导出到 JSON 文件。
   - 在**节点**选项卡中打开 CA。
   - 单击“下载”图标以通过浏览器会话生成 CA JSON 文件。
2. 将同级的组织 MSP 定义导出到 JSON 文件。
   - 在**组织**选项卡中导航至 MSP 定义。
   - 单击磁贴上的“下载”图标。
3. 从电子钱包中导出同级的组织管理员身份。
   - 导航至**电子钱包**选项卡。
   - 单击同级的组织管理员身份，然后单击**导出身份**。
   - 这将创建包含组织管理员证书的 JSON 文件。请记下文件名并妥善保管该文件，因为在其他集群中创建其他同级时需要该文件。

### 步骤 3：将元数据和身份导入到集群 2 和 3
{: #ibp-console-hadr-import-meta23}

1. 在集群 2 和 3 中，[导入 CA JSON 文件](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca)，即从集群 1 中导出的 CA JSON 文件。  
2. 上传 JSON 文件后，需要输入 CA 管理员注册标识和私钥，以及在集群 1 上部署 CA 时使用的 TLS CA 管理员注册标识和私钥。
2. 导入从集群 1 中导出的同级组织 MSP 定义 JSON 文件。
   - 在**组织**选项卡中，单击**导入 MSP 定义**。
   - 单击**添加文件**以上传 JSON 文件。
   - 单击**我具有 MSP 定义的管理员身份**复选框，以允许您在其他专区中创建新同级时使用此 MSP 定义。
   - 单击**导入 MSP 定义**。
3. 将从集群 1 中导出的组织管理员身份 JSON 文件导入到集群 2 和 3 上的控制台电子钱包。

### 步骤 4：在集群 2 和 3 中创建新的同级并加入通道
{: #ibp-console-hadr-create-new-peers}

1. 在集群 2 和 3 中，通过执行在集群 1 中注册同级身份时所采用的相同步骤，使用 CA 来注册新的同级用户。只需要创建同级用户，无需重新创建组织管理员身份，因为您会在下一步中将其导入。
2. 通过使用从集群 1 导入的 CA 作为同级的 CA，并使用从集群 1 中导入的同级组织 MSP 作为同级组织 MSP 定义，创建新的同级。
3. 在集群 2 和 3 中，现在可以重复先前运行的步骤，以将这些同级加入集群 1 中同级所在的通道。
4. 在这些冗余同级上安装智能合同后，分类帐将自动在所有同级之间同步。
5. 同样，如果计划使用服务发现、专用数据和同级 Gossip，那么需要使每个冗余同级都成为锚点同级。  

现在，网络已配置为可确保任何单个区域中的故障都不会影响同级工作负载。  

要进一步最大限度提高 HA，请考虑以下其他选项：
- 导出在集群 2 和 3 中创建的同级，并将这些同级导入到集群 1 中的控制台。然后，可以通过单个集群来管理所有内容。
- 但是，集群 1 可能会发生故障。因此，为了进一步应对集群故障，可以将所有同级导入到三个控制台中的每个控制台。在此之后，如果包含任一控制台的集群发生故障，仍可以通过其他两个集群中的控制台来管理所有内容。
