---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

keywords: blockchain network, Starter Plan, getting started

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# 关于入门套餐
{: #starter-plan-about}

{{site.data.keyword.blockchainfull}} Platform 入门套餐是一个入门级选项，支持组织模拟多组织区块链网络，快速开发应用程序以及使用样本智能合同和业务网络。它还拥有与其他成员资格选项相同的 UI 体验，有助于消除任何学习曲线。
2018 年 10 月 4 日之后创建的新入门套餐网络基于 Hyperledger Fabric V1.2.1 进行构建。较旧的入门套餐网络仍使用 Fabric V1.1.0 级别。
{:shortdesc}

现在不推荐使用入门套餐，因此目前无法创建新的入门套餐网络。** 请访问 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks)，以使用第二代区块链技术中目前提供的最新用户界面和功能。
{: important}  

**现有网络不会受到影响，您可继续使用到 2020 年 6 月 4 日。


**入门套餐**是一种环境，适用于要开始开发区块链网络或开始学习有关区块链技术的用户。可以将入门套餐用作开发或测试环境，这将允许您在移至生产环境之前，从小规模网络成员资格或事务处理量开始，随后进行缩放。

 要部署入门套餐网络，请立即在 {{site.data.keyword.cloud_notm}} 上注册[入门套餐成员资格](https://cloud.ibm.com/catalog/services/ibm-blockchain-5-prod){: external}。准备好开始开发网络后，请访问[入门套餐入门](/docs/services/blockchain?topic=blockchain-getting-started-with-starter-plan#getting-started-with-starter-plan)。


**注：**
- {{site.data.keyword.blockchainfull_notm}} Platform 入门套餐是一个开发与测试环境，不适合生产工作负载。如果需要生产环境，请参阅[关于企业套餐](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about)。
- {{site.data.keyword.blockchainfull_notm}} Platform 是 {{site.data.keyword.cloud_notm}} 上的平台服务，所有成员资格产品均遵循服务级别协议 (SLA) 上的 [{{site.data.keyword.cloud_notm}} 服务条款](http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm){: external}。入门套餐在单个地理位置的数据中心进行供应。有关可用地理位置的列表，请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform 位置](/docs/services/blockchain?topic=blockchain-ibp-regions-locations#ibp-regions-locations)。

## 入门套餐提供的内容
{: #starter-plan-about-what-starter-plan-offers}

- **_一次单击就绪型网络_**
    入门套餐供应的是实时区块链网络，只需单击一下就可使用。每个网络随附一个 CA、一个排序节点、两个组织（每个组织一个同级）以及一个用于处理和部署链代码的缺省通道。{{site.data.keyword.blockchainfull_notm}} Platform 会处理此网络的创建和配置（在网络上线后您可对其进行更新），以便您可专注于开发工作。入门套餐网络是基于 Fabric V1.2 构建的，并将 CouchDB 用作状态数据库。
- **_成本效益_**
    入门套餐成员资格选项提供了许多与企业套餐成员资格选项相同的区块链功能，但成本更低。
    有关更多信息，请参阅[入门套餐定价](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing-starter-pricing)。
- **_多组织网络模拟_**
    可以使用入门套餐来模拟使用多个组织构建网络。您不需要实际邀请其他组织加入网络，而是可以自己充当其他组织。通过这种机制，您可了解新组织能如何加入网络，多个组织如何在网络中一起工作，等等。可以通过“网络监视器”切换组织，以从不同组织的角度查看和管理网络。

{{site.data.keyword.IBM_notm}} 不支持将 Hyperledger Composer 用于生产的网络，包括 Composer CLI、JavaScript API、REST 服务器和 Web Playground。
{:note}

## 入门套餐是否适合您
{: #starter-plan-about-is-starter-suitable-for-you}

如果您符合下列其中一种情况，那么入门套餐适合您开始区块链之旅。
- **_了解 {{site.data.keyword.blockchainfull_notm}} Platform。_**
        如果您对 {{site.data.keyword.blockchainfull_notm}} Platform 感兴趣，甚或完全不熟悉区块链，那么通过入门套餐，可以很好地了解如何开发和管理真正的区块链网络。您可以了解组成网络的组件，管理成员资格，了解如何部署和管理链代码（也称为“智能合同”），如何添加通道（并管理通道许可权）以及跟踪何时生成新区块，就像在生产网络中一样。
- **_在实时网络中构建演示解决方案。_**    入门套餐为演示区块链应用程序提供了强大的环境。随时可用的区块链网络支持通过“网络监视器”提供的操作和管理工具来快速向同事、管理层和合作伙伴进行演示。
- **_突破单个组织开发。_**通过入门套餐，您可充当多个组织，以了解 {{site.data.keyword.blockchainfull_notm}} Platform 如何管理协作任务（如通道创建和链代码实例化），以及如何测试应用程序和调用事务处理。您还可以邀请其他人在入门套餐网络中进行协作（与在企业套餐中一样）。这有助于您在有多个参与方对链代码和事务进行背书的更现实环境中进行构建。
- **_以迭代方式开发和测试区块链应用程序。_**    入门套餐为您提供了一个编译打包区域，可用于在区块链网络上持续开发和测试代码。通过移至云，开发者和测试者能专注于功能，并轻松地从单元测试移至功能测试。入门套餐提供了与企业套餐相同的区块链网络功能以及操作和管理工具。在准备好将项目推送到其中一个企业套餐后，您可以按照入门套餐中的相同方式进行操作，但有更多机会使网络增长。

## 入门套餐注意事项
{: #starter-plan-about-considerations}

入门套餐是 {{site.data.keyword.blockchainfull_notm}} Platform 的起点，用于开发和测试目的。在使用入门套餐之前，请检查以下各项。

- **{{site.data.keyword.cloud_notm}} 帐户需求**
    您必须具有付费 {{site.data.keyword.cloud_notm}} 帐户，例如**现收现付**类型。{{site.data.keyword.blockchainfull_notm}} Platform 提供的所有成员资格套餐均需要 {{site.data.keyword.cloud_notm}} 付费帐户。要将帐户升级为“现收现付”类型，请转至 {{site.data.keyword.cloud_notm}} 控制台中的**管理** > **计费和使用情况** > **计费**，然后单击**添加信用卡**。
- **与企业套餐的差异**
    - [CA](/docs/services/blockchain?topic=blockchain-glossary#glossary-CA) 和排序服务不具有容错性，因为每个组织只有一个 CA，并且一个网络只有一个[排序节点](/docs/services/blockchain?topic=blockchain-glossary#glossary-orderer)。
    - 排序服务仅使用 [SOLO](/docs/services/blockchain?topic=blockchain-glossary#glossary-solo) [共识](/docs/services/blockchain?topic=blockchain-glossary#glossary-consensus)。入门套餐网络只包含一个排序节点，此排序节点对所有同级执行共识。
    - [Hardware Security Module (HSM)](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) 不可用于保护和管理用于高强度认证和加密处理的数字密钥。
- **入门套餐版本和升级**
    - 2018 年 10 月 4 日之后创建的新入门套餐网络基于 Hyperledger Fabric V1.2.1 进行构建。较旧的入门套餐网络仍使用 Fabric V1.1.0 级别。
    - 添加到较旧的入门套餐网络的新同级将基于 Fabric v1.2.1 进行构建。由于向后兼容，网络的性能不受影响。
    - 创建或更新通道时，您可以选择使用更高级的[通道配置](https://hyperledger-fabric.readthedocs.io/en/release-1.2/config_update.html){: external}设置和[访问控制表](https://hyperledger-fabric.readthedocs.io/en/release-1.2/access_control.html){: external}。
    - 入门套餐上不支持 Hyperledger Fabric V1.2 的[服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.2/discovery-overview.html){: external}和[专用数据](https://hyperledger-fabric.readthedocs.io/en/release-1.2/private-data/private-data.html){: external}功能。
    - 如果您[重置](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-reset-network)较旧的入门套餐网络 Fabric V1.1.0，那么您的新网络将为 Fabric V1.2 级别。如果重置网络，那么需要在新网络中安装链代码或 .bna 文件，以及重新邀请旧网络的成员。
- **网络资源限制**
    入门套餐为每个同级分配 1 个 CPU 和 4 Gi RAM，为每个 {{site.data.keyword.cloud_notm}} 服务实例（包括同级）分配 20 Gi 存储器。链代码容器和分类帐区块是消耗资源最多的网络组件。如果用户在网络上具有多个同级，或者生成大量区块，或者使用大型链代码文件，就可能会遇到资源限制影响性能的问题。您还可以在[“网络监视器”的“概述”屏幕](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-storage)中查看网络存储使用情况。
- **维护和升级**
    入门套餐维护和网络更新会定期执行。在维护期间，不能供应新网络，并且您可能会注意到有短暂的网络中断。
- **数据保留**
    入门套餐不保证在发行版升级时保留数据。
- **迁移注意事项**
    目前不支持将数据从入门套餐网络迁移到其他成员资格套餐。但是，可以迁移在入门套餐上经过测试的 `.bna` 文件、链代码和应用程序。有关更多信息，请参阅[从入门套餐迁移到企业套餐](/docs/services/blockchain/howto?topic=blockchain-migrate_starter_to_enterprise#migrate_starter_to_enterprise)。
