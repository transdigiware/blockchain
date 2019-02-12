---

copyright:
  years: 2016, 2017
lastupdated: "2017-07-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 区块链基础
{: #ibmblockchain_overview}

区块链是一种分布式分类帐技术 (DLT)，通过建立新一代事务性应用程序的新信任度、可计帐性和透明度，来简化业务流程。区块链网络首次引入到了比特币兑换市场，但其实际使用范围远远超出了加密数字货币事务处理。{{site.data.keyword.blockchainfull}} 与 Linux Foundation 的 Hyperledger 项目一起，将使人们重新构想最基本的业务交流，从而开启新的数字互动世界之门。

{{site.data.keyword.blockchain}} 通过创建高效、高度安全的网络来降低跨企业事务处理的成本和复杂性，在这种网络中，几乎可以跟踪和交易任何价值，而无需依靠集中的控制点。在金融方面，区块链网络可容许证券交易在数分钟而非数天内结算。在贸易世界，这些网络可以促进供应链管理，并允许实时跟踪和记录货物和付款的流向。 

## 区块链网络概述

在 {{site.data.keyword.blockchain}} 网络中，网络事务处理的记录被保留在跨所有或部分网络成员复制的共享分类帐上（分类帐存在于通道作用域中），因此，如果成员的同级未预订通道，那么他们将不具有该通道的事务处理。所有事务处理的记录（有效和无效）都记录在块中，并附加到每个通道的散列链（即区块链）。有效事务处理将更新全局状态数据库，而无效事务处理将不会更新。链代码（也称为“智能合同”）是包含一组允许对分类帐进行读写的函数的软件。客户机端应用程序利用 SDK 来与一个或多个同级进行交互，并最终调用特定链代码上的函数。有两个关键 Fabric API，它们允许链代码读或写：`getState` 和 `putState`。

**图 1** 描述了一个许可区块链网络的示例，它具有分布式分散对等体系结构，以及管理用户角色和许可权的认证中心：![区块链网络](images/Architecture_network_and_application.png "许可区块链网络的示例")
*图 1. 许可区块链网络：数据流和网络访问权由成员角色管理*

以下描述对应于图 1 中显示的体系结构和流程（注：这些描述不表示顺序进程）：

**A：**区块链用户向区块链网络提交事务处理。事务处理可以是部署、调用或查询，通过利用 SDK 的客户端应用程序或直接通过 REST API 发出。  

**B：**值得信赖的业务网络提供对监管者和审计员的访问权（例如，美国股票市场中的 SEC）。  

**C：**区块链网络操作员管理成员许可权，例如，将监管者 (B) 注册为“审计员”，将区块链用户 (A) 注册为“客户”。审计者仅限于查询分类帐，而客户可以获得授权部署、调用和查询特定类型的链代码。 

**D：**区块链开发者编写链代码和客户机端应用程序。区块链开发者可以通过 REST 接口将链代码直接部署到网络。要在链代码中包含来自传统数据源的凭证，开发者可以使用频带外连接来访问数据 (G)。 

**E：**区块链用户通过同级节点 (A) 连接到网络。继续任何事务处理之前，节点会先从认证中心检索用户的注册和事务处理证书。用户必须拥有这些数字证书，才能在许可网络上进行事务处理。

**F：**尝试驱动链代码的用户可能需要在传统数据源 (G) 上验证其凭证。要确认用户的授权，链代码可以通过传统处理平台，使用频带外连接到此数据。