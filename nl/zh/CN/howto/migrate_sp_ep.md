---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: Starter Plan network, Starter Plan, Enterprise Plan network, Enterprise Plan, migration

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 从入门套餐迁移到企业套餐
{: #migrate_starter_to_enterprise}

{{site.data.keyword.blockchainfull}} Platform [入门套餐](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about)提供一个测试和开发环境以运行 PoC、演示并试用链代码和应用程序。在准备好移至生产环境时，可从入门套餐网络迁移至[企业套餐](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about)网络。
{:shortdesc}

企业套餐网络提供以下生产就绪功能以支持您的生产工作负载：

- 在入门套餐网络之外的其他全球位置中提供的区块链网络。
- 无存储限制，入门套餐为 20 GB。
- 增强的 CPU 和 RAM 管理，确保所有网络平稳运行。
- 增强安全性，包括以下功能：
  - 安全服务容器 (SSC) 确保在任何指定的时刻区块链映像不会被篡改和装入，并且设备代码和数据在动态和静态情况下均受到保密保护。
  - [硬件安全模块 (HSM)](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm)，用于密钥管理和密钥存储。
  - 普遍磁盘加密。
- 高可用性、灾难恢复、崩溃故障容错和卷动升级。
- 可选的高级支持。

## 注意事项
{: #migrate_starter_to_enterprise_considerations}

在从入门套餐网络迁移至企业套餐网络时，可能会看到以下注意事项。

- **定价：**组织使用企业套餐网络的每月费用包括每个实例 1000 美元成员资格费用和每个同级 1000 美元同级费用。有关更多信息，请参阅[企业套餐定价](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing-enterprise-plan)。
- **Hyperledger Fabric 版本：**企业套餐网络在 Hyperledger Fabric v1.1 上运行。入门套餐网络在 Fabric v1.2 上运行。

- **受影响的资源：**链代码（智能合同）和客户机应用程序。同样，请注意您的链代码是否正在利用与 Fabric V1.1 网络不兼容的 V1.2 组件或功能。
- **所需时间：**至少用半天时间以将基本网络从入门套餐迁移至企业套餐。
- **现有分类帐数据**无法从入门套餐网络迁移至企业套餐网络，因为生产环境中不适合存在测试数据。
- **Hyperledger Composer：**{{site.data.keyword.IBM_notm}} 不支持将 Hyperledger Composer 用于生产的网络，包括 Composer CLI、JavaScript API、REST 服务器和 Web Playground。

**注：***基本*网络包含两个组织，其中有两个同级、单个通道和单个链代码文件。实际迁移时间可能根据企业套餐网络中所需的网络组件的大小和复杂性而有所不同。

## 迁移核对表
{: #migrate_starter_to_enterprise_checklist}

准备从入门套餐网络移至企业套餐网络需要完成大量任务。您可以在后续部分中找到详细指示信息，以下是摘要：

- 创建企业套餐网络。
- 从测试入门套餐网络重新创建期望的组织和同级配置。
- 标识将在企业套餐网络中运行的链代码（用 Go 或 Node 编写的）。
- 使用企业套餐网络的新 API 端点信息更新客户机应用程序。

### 创建企业套餐网络
{: #migrate_starter_to_enterprise_create_network}

迁移之前，需要创建企业套餐网络。有关更多信息，请参阅[创建企业套餐网络](/docs/services/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-create-network)。

### 重新创建网络配置
{: #migrate_starter_to_enterprise_config_network}

您可以在企业套餐网络中重新创建入门套餐网络的组织（成员）、通道和同级配置。您可以使用“网络监视器 UI”，通过邀请相应的组织（请注意，您无法与在入门套餐中一样**切换**组织）来重新创建这些网络资源、创建通道和创建同级（此外，受邀请的组织必须创建自己的同级）。

1. 登录到 {{site.data.keyword.cloud_notm}} 上的企业套餐网络并进入“网络监视器”。
2. 在“成员”屏幕上重新创建组织（成员），在“通道”屏幕上重新创建通道，并在“概述”屏幕上重新创建同级。有关创建网络资源的更多信息，请参阅[使用网络监视器](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-overview)。
3. 通过使用入门套餐网络中的相同方式，添加成员并设置通道策略来配置通道。

**注：**要实现高可用性，必须为组织至少创建两个同级，将他们加入到相同通道，并适当地编码客户机应用程序以在需要时从一个同级切换到另一个。

### 迁移链代码
{: #migrate_starter_to_enterprise_cc}

链代码是在本地环境中在外部开发的并且由客户机应用程序调用。要将在入门套餐网络中测试过的链代码安装到企业套餐网络上选中的同级并进行实例化，请遵循[安装、实例化和更新链代码](/docs/services/blockchain/howto?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode-install-cc)中的指示信息进行操作。

### 更新客户机应用程序
{: #migrate_starter_to_enterprise_app}

您需要使用企业套餐网络的新 API 端点信息更新现有客户机应用程序，此程序已针对入门套餐网络进行测试。有关更多信息，请参阅[检索网络凭证和连接概要文件](/docs/services/blockchain?topic=blockchain-getting-started-with-enterprise-plan#getting-started-with-enterprise-plan-retrieve-credentials)。

有关高可用性，客户机应用程序负责检测何时同级未响应并将事务路由到不同的同级。

## 如何处理现有的入门套餐网络
{: #migrate_starter_to_enterprise_existing_network}

您可以继续使用现有入门套餐网络作为沙箱环境以孵化新项目和测试对现有链代码的更改。此外，您可以在入门套餐网络中保留未迁移到企业套餐网络的测试分类帐数据。

在完成所有测试并验证一切都正常工作之前，请勿删除入门套餐网络。但是，一旦不再需要入门套餐网络以及其中的分类帐数据，即可安全地删除网络。有关更多信息，请参阅[删除或离开网络](/docs/services/blockchain?topic=blockchain-getting-started-with-starter-plan#getting-started-with-starter-plan-delete-network)。
