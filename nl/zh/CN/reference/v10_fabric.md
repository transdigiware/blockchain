---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: Hyperledger Fabric, confidential channels, Membership Service Provider, Linux Foundation, SDKs, modular architecture, permissioned network

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Hyperledger Fabric
{: #hyperledger-fabric}

{{site.data.keyword.blockchainfull}} 网络以 Hyperledger Fabric 堆栈为基础构建，后者是 Linux Foundation Hyperledger 项目中的区块链项目之一。它是一个“许可”网络，其中所有用户和组件都具有已知标识。在每个通信接触点实施签名/验证逻辑，并通过一系列支持和验证检查来同意事务处理。在此意义上，它与传统的区块链实现有很大差异，因为传统区块链实现可提升匿名性，并强制依赖于加密货币和大量计算责任来验证事务处理。
{:shortdesc}

Hyperledger Fabric 提供模块化体系结构来提高可伸缩性和性能。本主题介绍了 Hyperledger Fabric 中的一些关键组件。有关 Hyperledger Fabric 的完整介绍，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}。

## 同级
{: #hyperledger-fabric-peer}

在物理级别，区块链网络主要由同级节点（或者简称为同级）构成。同级是网络的基本元素，因为同级用于托管分类帐和智能合同（这些对象包含在[“链代码”](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html){: external}中）。更准确地说，同级托管分类帐的**实例**以及智能合同的**实例**。由于智能合同和分类帐分别用于封装网络中的共享过程和共享信息，因此同级的这些方面使其成为了解 Fabric 实际所执行操作的合适起点。

要了解有关同级的更多具体信息，请查看 Fabric 社区文档中[仅关注同级的此文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html){: external}。

## 认证中心
{: #hyperledger-fabric-certificate-authority}

作为**许可**区块链网络的平台，Hyperledger Fabric 包含模块化**认证中心 (CA)** 组件，用于管理所有成员组织及其用户的网络标识。每个用户的许可标识要求对网络活动启用基于 ACL 的控制，并保证每个事务处理最终都可跟踪到注册用户。
* CA 向授权加入网络的每个**成员**（组织或个人）签发根证书 (**rootCert**)。
* CA 还向每个成员组件、服务器端应用程序和（偶尔）用户发出注册证书 (**eCert**)。
* 每个已注册的用户还会被授予事务处理证书 (**tCert**) 的分配。每个 **tCert** 授权一个网络事务处理。

此基于证书的网络成员资格和操作控制使成员能够通过特定用户身份，限制对专用和保密通道、应用程序和数据的访问。

有关 Hyperledger Fabric 认证中心组件的更多信息，请参阅 [Fabric CA User’s Guide](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/){: external}。

## 成员资格服务提供者
{: #hyperledger-fabric-membership-service-provider}

Hyperledger Fabric 包含**成员资格服务提供者 (MSP)** 组件，以提供对发出和验证证书背后的所有加密机制和协议的抽象，以及用户认证。MSP 会安装在每个通道同级上，以确保向同级发出的事务处理请求源自已认证和已授权的用户身份。

有关 Hyperledger Fabric 成员资格服务提供者组件的更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}中的 [Membership](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external}。

## 排序服务
{: #hyperledger-fabric-ordering-service}

在其他分布式区块链（例如以太坊和比特币）中，没有中央管理机构来对事务排序并将其发送给同级。Hyperledger Fabric 是 {{site.data.keyword.blockchainfull_notm}} Platform 所基于的区块链，其工作方式有所不同。它有一种称为**排序节点**的节点。

排序节点是网络中的关键组件，因为排序节点会执行一些基本功能：

- 顾名思义，排序节点用于对发送给同级以写入其分类帐的事务处理区块**排序**，此过程称为“排序”。如果这些事务处理改为在同级本身进行捆绑并排序，那么将更有可能出现一个同级将一个事务处理写入其分类账，而另一个同级没有写入的情况，从而产生状态分叉。
- 排序节点维护**排序节点系统通道**，这是**联盟**（即，允许创建通道的同级组织的列表）所在的位置。
- 排序节点执行重要的身份验证检查。例如，如果不属于排序节点联盟的组织尝试创建通道，系统会拒绝该请求。排序节点还会对事务通道中的行为（例如，用于更改通道配置的许可权）进行验证。

Hyperledger Fabric 目前支持 SOLO（一个排序节点）和基于 Kafka 的排序服务实现。有关 Hyperledger Fabric 排序服务的更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}中的 [Bringing up a Kafka-based Ordering Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/kafka.html){: external}。

## Fabric SDK
{: #hyperledger-fabric-fabric-sdks}

Hyperledger Fabric SDK 支持应用程序开发者构建与区块链网络进行交互的应用程序。这些 SDK 帮助简化应用程序对通道和链代码生命周期的管理。

Hyperledger Fabric 提供 Node.js SDK 和 Java SDK，并提供以下功能来与区块链网络进行交互：

* 注册和登记用户
* 创建通道
* 将同级加入通道
* 更新系统通道或应用程序通道配置
* 在同级上安装链代码
* 在通道上实例化链代码
* 在通道上升级链代码
* 调用链代码函数以更新分类帐
* 查询分类帐以获取特定事务处理、块或密钥
* 在通道上监视事件（例如，成功落实事务处理）

有关 Fabric SDK 的更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}中的 [Hyperledger Fabric SDKs](https://hyperledger-fabric.readthedocs.io/en/release-1.4/fabric-sdks.html){: external}。

## 事务处理流程
{: #hyperledger-fabric-transaction-flow}

为了确保数据的一致性和完整性，Hyperledger Fabric 在整个事务处理流程中实现多个检查点，包括客户机认证、支持、排序和落实到分类帐。

**图 1** 描述 Hyperledger Fabric 区块链网络上的事务处理流程：![事务处理流程](../images/v10_txflow.svg "Hyperledger Fabric 网络上的事务处理流程")

在 Hyperledger Fabric 网络上，用于查询和事务处理的数据流由客户机端应用程序通过向通道上的同级提交事务处理请求来启动。跨网络的初始数据流对于查询和事务处理来说是通用的：

1. 使用 SDK 中提供的 API，客户机应用程序会对事务处理建议签名并将该建议提交给指定通道上的相应背书同级。此初始事务处理建议是用于支持的**请求**。
2. 通道上的每个同级会验证提交客户机的身份和权限，如果有效，同级会根据提供的输入运行指定的链代码。根据所调用链代码的事务处理结果和背书策略，每个同级会向应用程序返回已签名的“是”或“否”响应。每个已签署的“是”响应都是事务处理的**支持**。

	此时在事务处理流程中，流程与查询和事务处理不同。如果建议调用链代码中的查询函数，那么应用程序会将数据返回给客户机。如果建议调用链代码中的某个函数来更新分类帐，那么应用程序将继续执行以下步骤：
3. 应用程序将事务处理（包括读/写集和背书）转发到**排序服务**。
4. 然后，事务处理会中继到排序服务。所有通道同级通过应用“链代码特定验证策略”并运行“并行控制版本检查”来验证该区块中的每个事务处理。
	* 未通过验证过程的任何事务处理都会在该区块中标记为无效，并且该区块将附加到通道的分类帐。
	* 所有有效的事务处理都将根据修改后的键/值对相应地更新状态数据库。

**Gossip 数据传播协议**将连续在通道上广播分类帐数据，以确保同级之间的分类帐同步。有关更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}中的 [Gossip data dissemination protocol](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external}。

有关事务处理流程的逐步介绍，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}中的 [Transaction Flow](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}。
