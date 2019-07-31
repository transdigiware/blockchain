---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: IBM Blockchain Platform, IBM Cloud Private, AWS, Data residency, world state

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# 数据存储位置
{: #console-icp-about-data-residency}

由于区块链网络并不关注所处理的数据类型，因此有时必须采取额外的步骤来确保某些类型数据的安全。数据存储位置的最常见需求与特定国家或地区的法律相关，这要求在 IT 系统中处理和存储的所有数据必须保留在特定国家或地区境内。与此类似，受到高度监管的行业（例如，政府、医疗保健和金融服务）中的某些公司要求必须将数据完全存储在其防火墙之后。

区块链网络允许多个组织使用分布式分类帐，以可信且安全的方式对数据进行事务处理和共享。但是，这意味着可在网络的节点中以及这些节点所在的区域中分发数据。组织可使用多个选项将数据与网络的其余部分隔离并实现数据存储位置：
1. [共享通道上的专用数据集合](#console-icp-about-data-residency-fabric)
2. [单独通道上的专用数据集合](#console-icp-about-data-residency-use-case)
3. [单独通道，该通道上的所有节点都位于一个国家或地区内](#console-icp-about-data-residency-use-case-channel)

每种方法都为数据提供更高级别的隔离和保护，但需要执行额外工作来实现和管理。为帮助您了解如何使用每个选项来实现数据存储位置，我们提供了如何在 Hyperledger Fabric 网络中共享数据的概述。此外，我们还提供了一个示例用例，以说明区块链联盟中的组织会如何使用每个选项来隔离其数据并阻止数据离开其区域。

## 如何在 {{site.data.keyword.blockchainfull_notm}} Platform 网络中共享数据
{: #console-icp-about-data-residency-fabric}

作为 {{site.data.keyword.blockchainfull_notm}} Platform 基础的 Hyperledger Fabric 体系结构主要围绕着三个关键组件：排序服务（由排序节点组成）、认证中心 (CA) 和同级。此外，组织会使用 [Fabric SDK](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}，将事务从客户机应用程序发送到这些节点。在考虑数据存储位置时，重要的是了解这些组件如何与数据进行交互以及如何存储数据。

**同级**由联盟的成员用于存储区块链[分类帐](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ledger/ledger.html){: external}。区块链分类帐由两部分组成。第一部分是全局状态，用于以键值对的方式存储分类帐中所有数据的最新值。第二部分是每个事务的区块链记录。同级以新区块形式接收来自排序服务的状态更新。然后使用区块和全局状态来确认（或落实）事务，更新全局状态并在区块链上添加事务日志。排序服务为[联盟](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium)上所有同级的事务建立顺序，并为分类帐的区块链部分存储副本。

**通道**是用于在网络中传输数据的机制。如果不加入通道，将无法参与到区块链网络中。通道可以由网络成员用于在业务应用程序之间创建逻辑分隔，甚至通过限制流量来提升性能。联盟中组织的子集也可使用通道对数据进行私下事务处理并隔离数据。

同级为加入的每个通道维护单独的分类帐。只有作为通道成员的组织可将其同级加入该通道并接收来自排序服务的分类帐更新。因此，每个通道都与一个排序服务绑定，该服务会存储它所维护的每个通道分类帐的区块链部分。客户机应用程序会将事务提交至给定通道的同级和排序服务。这些事务会添加到区块链内的事务日志中，并包含[读/写集](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}，后者会添加到全局状态的键值对中。

如果要求国家或地区内的数据存储位置，那么您需要考虑同级、排序服务以及客户机应用程序的位置。您还需要了解您的通道上属于其他组织的同级的位置。如果使用的是 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}，您可以查找 [{{site.data.keyword.blockchainfull_notm}} Platform 区域和位置](/docs/services/blockchain/reference?topic=blockchain-ibp-regions-locations#ibp-regions-locations)列表，您和您联盟的成员可在其中部署您的组件。

## 数据存储位置的用例
{: #console-icp-about-data-residency-use-case}

我们可以使用示例联盟来说明如何在 {{site.data.keyword.blockchainfull_notm}} Platform 上分发数据，并探索成员可如何实现数据存储位置。下图包含的联盟具有一个排序服务和四个组织。每个组织具有一个同级节点。有两个组织（组织 A 和组织 B）以及排序服务位于美国。其他两个组织（组织 C 和组织 D）位于德国。所有四个组织都是通道 X 的成员并已将其同级加入其中。

![示例联盟](images/data_res_use_case.svg "示例联盟")

加入通道 X 的每个同级会存储通道分类帐的副本，该通道分类帐在**图 1** 中显示为分类帐 X。由于美国和德国的同级已加入该通道，因此通道分类帐上的数据同时保留在这两个地理位置中。分类帐的区块链部分也由位于美国的排序服务进行存储。

如果联盟中的两个组织创建第二个通道（通道 Y），那么将创建第二个分类帐并存储在通道成员的同级上。只有已加入该通道的组织才会具有该通道数据的副本。

![添加第二个通道](images/data_res_use_case_channel.svg "添加第二个通道")


在**图 2** 中，组织 B 和组织 D 已加入通道 Y。现在，组织 B 和组织 D 的同级除了分类帐 X 外，还会存储分类帐 Y 的副本。由于使用了相同的排序服务来创建通道 X 和通道 Y，排序服务现在具有这两个通道分类帐的区块链部分的副本。在**图 1** 和**图 2** 中，由德国的应用程序创建的数据存储在美国，如果需要数据存储位置，那么不应该发生这种情况。

我们可以使用上面的示例来探索组织可用于实现数据存储位置的选项。假定德国的某项法规要求组织 C 和组织 D 创建的某些数据保留在本国内。德国的组织可以使用所有三个选项来阻止数据存储到美国。

## 选项一：共享通道上的专用数据集合
{: #console-icp-about-data-residency-use-case-private-data}

组织 C 和组织 D 可以使用 Hyperledger Fabric 的[专用数据功能](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "什么是专用数据集合？"){: external}，以阻止数据分发到通道上的所有组织。专用数据集合允许组织将状态数据以同级对同级的方式（通过 Gossip 协议）与已授权读取集合的其他组织共享。数据存储在同级的专用单独数据库中。排序服务不会涉及其中，并且排序服务也不会看到专用数据。只有集合中数据的散列会添加到通道分类帐中，并存储于其他通道成员的同级以及排序服务上。这允许组织在需要将事务详细信息公开时验证专用数据。要了解更多信息，请访问 Fabric 文档中的[专用数据](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "专用数据"){: external}概念文章。

![专用数据](images/data_res_private_data.svg "文字说明三")

在**图 3** 中，组织 C 和组织 D 已创建专用数据集合（OrgC-OrgD 集合），可允许组织进行事务处理，而无需与组织 A、组织 B 或排序服务共享数据。此集合中的键值状态数据仅存储于组织 C 和组织 D 的同级上，并且不会离开德国。但是，集合中数据的散列存储于分类帐 X 中，并与更广泛的通道共享。这意味着 OrgC-OrgD 集合中数据的散列会存储于美国的同级以及排序服务上。

在考虑专用数据时，重要的是了解**散列数据**与**加密数据**之间的差异。加密使用双向函数来将数据变换为隐藏其原始值但可转换回原始状态的形式。例如，通过使用 TLS 保护的网络发送数据时，数据会使用 TLS 证书进行加密。然后数据会作为加密文本在网络中发送，之后再由接收方解密。加密文本包含所有原始数据，并且可使用专用密钥解密。但是，散列是一种单向函数，使用数据创建数字和字母的唯一字符串。散列的数据无法使用散列转换回原始形式。要验证创建了散列的数据，接收方需要使用相同的散列函数创建原始数据的新散列，并验证这些散列值是否匹配。如果没有原始数据的副本，第三方无法使用散列。  

对于此选项，请务必注意，虽然组织 A 和 B 看不到实际分类帐数据（因为数据已散列），但仍可以看到组织 C 和 D 正在进行事务处理，并且可以查看两者之间正在进行的事务处理量。

此外，请注意，专用数据集合中的数据可从存储该数据的同级中清除。当数据永久存储于通道上时，集合允许成员指定将多少个区块落实到通道之后[清除专用数据](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private_data_tutorial.html#pd-purge){: external}。从专用数据集合除去数据之后，无法再使用通道上的散列来验证创建该数据的事务。在**图 3** 的示例网络中，组织 C 和组织 D 可使用`阻止存在`策略来确保无需永久存储的任何数据将在指定时间段内从网络中完全除去。

## 选项二：单独通道上的专用数据集合
{: #console-icp-about-data-residency-use-case-private-data-channel}

在单独通道上下文中，组织 C 和组织 D 也可以使用专用数据集合，为其数据提供额外隔离。创建新通道（在本例中为通道 Y）可确保专用数据的散列仅与排序服务共享，而不会与联盟的其他成员共享，也不会存储在其同级上。

![在单独通道上使用专用数据](images/data_res_private_data_channel.svg "在单独通道上使用专用数据")

在**图 4** 中，组织 C 和组织 D 已构成新通道（通道 Y），其中不包含美国的任何成员。因此，OrgC-OrgD 集合中数据的散列会存储在分类帐 Y 而非分类帐 X 上，并且不会存储在美国的同级上。由于排序服务位于美国，因此在德国创建的数据的散列仍离开了德国。

创建单独通道可阻止事务详细信息与联盟中的其他组织共享。如果德国的组织使用共享通道，那么组织 A 和组织 B 将能够查看由专用数据集合落实到通道分类帐的事务散列数。这样一来，这些组织可能会发现组织 C 和组织 D 正在进行事务处理，并看到两者之间生成的事务处理量。不过，请注意，构成新通道需要额外的管理开销来创建和更新。构成新通道也可使组织 C 和组织 D 更难与组织 A 和组织 B 共享数据。

## 选项三：所有组件都位于一个国家或地区的通道
{: #console-icp-about-data-residency-use-case-channel}

组织 C 和组织 D 也可以创建一个通道，使其中所有基础架构都位于相同国家或地区。这要求加入通道的同级、应用程序和排序服务全部位于相同区域中。在此场景中，通道分类帐上存储的任何数据都不会离开该区域，也不会存储在该国家或地区之外。

![创建单独通道](images/data_res_separate_channel.svg "创建单独通道")

在**图 5** 中，组织 C 和组织 D 已为不得离开德国的数据创建新通道。这要求创建位于德国的新排序服务，以确保排序节点的通道分类帐副本存储在德国内。由于在本例中，排序服务、OrgC-peer 和 OrgD-peer 位于德国，组织 C 和组织 D 现在可选择将数据在通道上公开，或者仍可决定使用专用数据集合以防止将完整事务数据存储在排序服务上。

创建所有组件都位于一个国家或地区的通道可确保所有数据都位于一个区域内，包括键值对、区块链事务日志和任何专用数据的散列。但是，此选项需要一些开销来维护新通道，并且也会产生与维护排序服务关联的成本。

## 参考资料
{: #console-icp-about-data-residency-reference}

要更深刻地了解 {{site.data.keyword.blockchainfull_notm}} Platform 网络上的数据流，请参阅[有关事务流的 Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}。

未来，Zero Knowledge Proof 将提高在 Hyperledger Fabric 中实现进一步数据存储位置的能力。Zero-Knowledge Proof (ZKP) 允许“证明者”向“验证者”保证他们在不泄露自身秘密的情况下了解保密信息。这样就可以表明您知道满足特定条件的某个内容，而不会透露您所知道的具体内容。

您可以在有关 [Private and confidential transactions with Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external} 的白皮书中获取专用数据集合和 Zero Knowledge Proof 的更多相关信息。
