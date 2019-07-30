---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-21"

keywords: IBM Blockchain Platform, release, new features

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# 新增内容
{: #whats-new}

## 2019 年 6 月 18 日
{: #whats-new-6-18-2019}

{{site.data.keyword.blockchainfull}} Platform for Multicloud 已一般可用，它支持在您自己的基础架构和您选择的 Kubernetes 云提供者中使用第二代 {{site.data.keyword.blockchainfull_notm}} Platform。通过此发行版，可使用用户界面控制台借助 {{site.data.keyword.cloud_notm}} Private 在您自己的环境中部署和管理区块链组件。{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 使用 Hyperledger Fabric V1.4.1 代码库，并支持在 {{site.data.keyword.cloud_notm}} Private V3.2 上进行部署。

此 {{site.data.keyword.blockchainfull_notm}} Platform 发行版包含以下主要功能：

**构建 - 综合开发者体验**
- **轻松编码**，通过 Node.js、Golang 或 Java 对智能合同轻松编码，使用新的 {{site.data.keyword.blockchainfull_notm}} VS Code 扩展来编写客户机应用程序，利用与用户界面控制台的 **SDK 集成**，并通过我们丰富的教程和样本进行学习。
- **简化的 DevOps** 允许您通过扩展 Kubernetes 资源来添加更多组件，在单个环境中从开发移至测试，再移至生产。
- **Kubernetes 服务集成**。利用 Grafana 和 Prometheus 等服务进行日志记录，并利用 Kibana 进行监视。
- **最新的 Fabric 主要功能**。利用 Hyperledger Fabric V1.4.1 的最新功能：
  - [Raft 排序服务](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [专用数据集合](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)，用于通过 Gossip 协议确保仅与授权同级共享分类帐数据，以提高数据隐私性。
  - [服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}，允许您动态发现和更新应用程序与网络的交互方式。
  - [通道访问控制表](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}，允许您对通道和智能合同的管理进行额外控制。

**操作 - 全面控制部署**
- **仅部署所需的组件**。将一个同级连接到多个通道和网络，或者托管业务合作伙伴可以连接到的排序服务。
- **保持对身份的完全控制权**。存储和管理用于在您自己的安全环境中管理节点的密钥。
- **统一操作**。通过 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，可以在**一个控制台**中部署和管理所有组织和节点，而不必依赖 {{site.data.keyword.IBM_notm}} 或其他供应商来管理排序节点或认证中心。此外，还可以通过控制台在区块链联盟中添加或除去成员，创建和加入通道，以及安装和实例化智能合同。
- **托管或加入网络**。将集群中托管的同级部署到多个云上的多个通道，或邀请其他组织加入您的联盟或通道，同时组织可跨基础架构独立管理其节点。
- **管理访问权**，即管理可以管理或监视节点的用户的访问权。
- **直接访问日志**，即通过 Kubernetes 服务直接访问节点的日志。使用任何支持的第三方服务来抽取和分析日志。
- **与 pod 直接交互**，使用 Kubernetes 服务来执行。
- **动态签名集合**，允许通过通道配置对协作管理进行更好的控制。

**增长 - 可伸缩性和灵活性**
- **选择计算**。您可以灵活地决定要在 Kubernetes 集群中供应的 CPU、内存和存储器的数量。有关更多信息，请参阅[控制台如何与 Kubernetes 集群进行交互](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction)。
- **缩放**，即扩展和缩减 Kubernetes 集群中的资源，仅为所需资源付费。有关更多信息，请参阅[定价](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing)。
- **灾难恢复和多专区高可用性**。此功能可在不同专区中复制 Kubernetes 部署，从而支持组件高可用性 (HA) 以及灾难恢复 (DR)。
- **在任意位置运行**。依托 {{site.data.keyword.blockchainfull_notm}} Platform 控制台的**统一代码库**，可以在 {{site.data.keyword.cloud_notm}} Private 支持的任何环境上运行组件。


**有关部署的指示信息位于 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 入门](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)中。**

- 可以在[关于 {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) 中找到更多信息。
- 有关使用 {{site.data.keyword.blockchainfull_notm}} Platform 的已更新教程在**操作指南**类别下的 **{{site.data.keyword.blockchainfull_notm}} Platform 控制台**子部分中提供。
  * [构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)将指导您完成通过创建 CA、排序服务和同级来托管网络的过程。
  * [加入网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network)说明了如何通过创建同级并将其加入通道来加入现有网络。
  * [在网络上部署智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts)提供了有关如何编写智能合同并将其部署在网络上的信息。
- {{site.data.keyword.blockchainfull_notm}} Platform 产品基于 Hyperledger Fabric V1.4.1，并支持同级对同级 Gossip、服务发现和专用数据。请访问此[主题](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)以了解如何在网络上配置专用数据。

有关 Hyperledger Fabric V1.4.1 的更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}。有关 {{site.data.keyword.cloud_notm}} Private 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private V3.2 文档](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 前发行版的文档已移至新位置。如果仍使用的是 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private V1.0.1 或 V1.0.2，请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 文档](/docs/services/blockchain-icp-102?topic=blockchain-icp-102-get-started-icp)。

## 2019 年 5 月 31 日
{: #whats-new-5-31-2019}

第二代 {{site.data.keyword.blockchainfull_notm}} Platform 已一般可用，它支持部署、操作和监视区块链网络。此发行版包含新的用户界面控制台，可用于在 {{site.data.keyword.cloud_notm}} 上您自己的 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群中部署和管理区块链组件。

此 {{site.data.keyword.blockchainfull_notm}} Platform 发行版包含以下主要功能：

**构建 - 综合开发者体验**
- **轻松编码**，通过 Node.js、Golang 或 Java 对智能合同轻松编码，使用新的 {{site.data.keyword.blockchainfull_notm}} VS Code 扩展来编写客户机应用程序，利用与控制台的 **SDK 集成**，并通过我们丰富的教程和样本进行学习。
- **简化的 DevOps** 允许您通过扩展 Kubernetes 资源来添加更多组件，在单个环境中从开发移至测试，再移至生产。
- **最新的 Fabric 主要功能**。利用 Hyperledger Fabric V1.4.1 的最新功能：
  -  [Raft 排序服务](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - **{{site.data.keyword.cloud_notm}} 服务集成**。利用内置 {{site.data.keyword.cloud_notm}} 服务，例如 {{site.data.keyword.cloud_notm}} Kubernetes Service 仪表板、{{site.data.keyword.IBM_notm}} Log Analysis with LogDNA 和 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)。
  - [**专用数据**集合](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)，用于通过 Gossip 协议确保仅与授权同级共享分类帐数据，以提高数据隐私性。
  - [服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}，允许您动态发现和更新应用程序与网络的交互方式。
  - [通道访问控制表](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}，允许您对通道和智能合同的管理进行额外控制。

**操作 - 全面控制部署**
- **仅部署所需的组件**。将一个同级连接到多个通道和网络，或者托管业务合作伙伴可以连接到的排序服务。
- **保持对身份的完全控制权**。存储和管理用于管理节点的密钥，而无需在 {{site.data.keyword.cloud_notm}} 上存储专用密钥。
- **集中化操作**。通过 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，可以在**一个中央控制台**中部署和管理所有组织和节点，而不必依赖 {{site.data.keyword.IBM_notm}} 或其他供应商来管理排序节点或认证中心。此外，还可以通过控制台在区块链联盟中添加或除去成员，创建和加入通道，以及安装和实例化智能合同。
- **托管或加入网络**。将集群中托管的同级部署到多个云上的多个通道，或邀请其他组织加入您的联盟或通道，同时组织可跨基础架构独立管理其节点。
- **管理访问权**，即管理可以管理或监视节点的用户的访问权。
- **直接访问日志**，即通过 {{site.data.keyword.IBM_notm}} Kubernetes 服务直接访问节点的日志。使用 {{site.data.keyword.cloud_notm}} Log Analysis 服务或第三方服务来抽取和分析日志。
- **与 pod 直接交互**，使用 Kubernetes 仪表板来执行。通过命令行执行到 pod 和容器中以运行命令和更新证书。
- **动态签名集合**，允许通过通道配置对协作管理进行更好的控制。

**增长 - 可伸缩性和灵活性**
- **选择计算**。您可以灵活地决定要在 Kubernetes 集群中供应的 CPU、内存和存储器的数量。有关更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Kubernetes Service 如何与控制台进行交互](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction)。
- **缩放**，即扩展和缩减 Kubernetes 集群中的资源，仅为所需资源付费。有关更多信息，请参阅[定价](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing)。
- **灾难恢复和多专区高可用性**。此选项可在不同专区中复制 Kubernetes 部署，从而支持组件高可用性 (HA) 以及灾难恢复 (DR)。
- **在任意位置运行**（相关指示信息即将推出）。依托 {{site.data.keyword.blockchainfull_notm}} Platform 控制台的**统一代码库**，可以在 {{site.data.keyword.cloud_notm}}、{{site.data.keyword.cloud_notm}} Private 和第三方公共云上运行组件。

- 有关 {{site.data.keyword.blockchainfull_notm}} Platform 的更多信息在[关于 {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) 中提供。
- 有关如何将发行版部署到 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群的指示信息在 [{{site.data.keyword.blockchainfull_notm}} Platform 2.0 入门](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)中提供。
- 有关使用 {{site.data.keyword.blockchainfull_notm}} Platform 的新教程在**操作指南**类别下的 **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** 子部分中提供。
  * [构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)将指导您完成通过创建排序节点和同级来托管网络的过程。
  * [加入网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network)说明了如何通过创建同级并将其加入通道来加入现有网络。
  * [在网络上部署智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts)提供了有关如何编写智能合同并将其部署在网络上的信息。
- {{site.data.keyword.blockchainfull_notm}} Platform 产品基于 Hyperledger Fabric V1.4.1，并支持同级对同级 Gossip、服务发现和专用数据。请访问此[主题](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)以了解如何在网络上配置专用数据。

## 2019 年 5 月 9 日
{: #whats-new-5-09-2019}

{{site.data.keyword.blockchainfull_notm}} 发布了 {{site.data.keyword.blockchainfull_notm}} Platform Visual Studio (VS) Code Extension V1.0.0。此开发者工具箱包含以下主要功能：

**重新配置的用户界面，导航更简单**

**规范的详细教程，涵盖以下方面的操作指南：**
- 开发智能合同
- 在 {{site.data.keyword.cloud_notm}} 上供应 {{site.data.keyword.blockchainfull_notm}} 服务的实例
- 部署 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例并与服务实例之间进行事务处理

**前发行版中的所有常用功能，包括：**
- 调试智能合同的功能
- 样本代码
- 更新了电子钱包功能

有关更多信息，请参阅[使用 Visual Studio Code 扩展开发智能合同](/docs/services/blockchain?topic=blockchain-develop-vscode "使用 Visual Studio Code 扩展开发智能合同")。

## 2019 年 4 月 23 日
{: #whats-new-4-23-2019}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 发布了 V1.0.2，此版本使用 Hyperledger Fabric V1.4.0 代码库，并支持在 {{site.data.keyword.cloud_notm}} Private V3.1.2 上进行部署。

如果要将现有 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 升级到 V1.0.2，请参阅[在 {{site.data.keyword.cloud_notm}} Private 上升级 Helm chart](/docs/services/blockchain-icp-102/howto?topic=blockchain-icp-102-helm-install#helm-install-upgrading)。

有关 Hyperledger Fabric V1.4.0 的更多信息，请参阅 [Hyperledger Fabric 文档](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}。有关 {{site.data.keyword.cloud_notm}} Private 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private V3.1.2 文档](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.2/kc_welcome_containers.html){: external}。

## 2019 年 2 月 8 日
{: #whats-new-2-08-2019}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了免费 2.0 Beta，这是下一代 {{site.data.keyword.blockchainfull_notm}} Platform 解决方案，支持部署、操作和监视区块链网络。{{site.data.keyword.blockchainfull_notm}} Platform 免费 2.0 Beta 包含新的用户界面控制台，可用于在 {{site.data.keyword.cloud_notm}} 上您自己的 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群中部署和管理区块链组件。{{site.data.keyword.blockchainfull_notm}} Platform 免费 2.0 Beta 支持：

**通过无缝体验更快、更轻松地构建网络**

*	智能合同开发 (VS Code) 与网络管理之间的平稳集成
*	简化的 DevOps 允许通过单个控制台从开发移至测试，再移至生产
*	支持使用 Javascript、Java 和 Go 语言编写智能合同

**全面控制网络操作和管理**

*	仅部署所需的区块链组件（同级、排序服务和认证中心）
*	通过重新设计的控制台，可以在一个位置管理网络组件，无论组件部署在何处
*	保持对身份、分类帐和智能合同的完全控制权

**通过新启用的多云灵活性，轻松扩展分布式网络**

*	连接到在任何环境（内部部署、公共云和混合云）中运行的节点
*	轻松将单个同级连接到多个行业网络
*	从小规模开始，随着使用量的增长付费，无需前期投资，并可通过 Kubernetes 轻松升级

- 有关 {{site.data.keyword.blockchainfull_notm}} Platform 免费 2.0 Beta 的更多信息在[关于 {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) 中提供。
- 有关如何将免费 2.0 Beta 发行版部署到 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群的指示信息在 [{{site.data.keyword.blockchainfull_notm}} Platform 2.0 入门](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)中提供。
- 有关使用 {{site.data.keyword.blockchainfull_notm}} Platform 免费 2.0 Beta 的新教程在**操作指南**类别下的 **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** 子部分中提供。
  * [构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)将指导您完成通过创建排序节点和同级来托管网络的过程。
  * [加入网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network)说明了如何通过创建同级并将其加入通道来加入现有网络。
  * [在网络上部署智能合同](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts)提供了有关如何编写智能合同并将其部署在网络上的信息。
- {{site.data.keyword.blockchainfull_notm}} Platform 免费 2.0 Beta 产品基于 Hyperledger Fabric V1.4，并支持同级对同级 Gossip、服务发现和专用数据。请访问此[主题](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)以了解如何在网络上配置专用数据。

- {{site.data.keyword.blockchainfull_notm}} Visual Studio Code 扩展可从 Visual Studio Code Marketplace 获取。开发者可以使用该扩展来创建和测试智能合同，并将其部署到 Hyperledger Fabric 的实例。该扩展与 Hyperledger Fabric 1.3 及更高版本兼容。有关使用 VS Code 扩展的信息，请参阅[用于智能合同的工具](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode)。  

通过将所有入门主题分组到名为**入门教程**的部分中，增强了目录功能。此外，原先在**关于 {{site.data.keyword.blockchainfull_notm}} Platform** 子部分中描述的每个 {{site.data.keyword.blockchainfull_notm}} Platform 产品现在包含在**学习**部分下。

**注：**免费云信用值对于入门套餐用户不再可用。

## 2018 年 12 月 7 日
{: #whats-new-12-07-2018}

*操作入门套餐网络*和*操作企业套餐网络*主题组合并替换为关于[使用网络监视器](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard)的单个教程。

## 2018 年 11 月 27 日
{: #whats-new-11-27-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private。{{site.data.keyword.cloud_notm}} Private 是一个应用程序平台，用于开发和管理基于 Kubernetes 的容器化应用程序，允许用户在 x86、LinuxONE 和 {{site.data.keyword.IBM_notm}} Z 上部署认证中心 (CA)、排序节点和同级。{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 基于 Hyperledger Fabric V1.2.1，并使用 Kubernetes Helm chart 进行部署。安装 Helm chart 后，您可在 {{site.data.keyword.cloud_notm}} Private 目录中找到作为捆绑服务的 Helm chart。请注意，[此产品](/docs/services/blockchain-icp-102?topic=blockchain-icp-102-ibp-icp-about#ibp-icp-about)更适合经验丰富的 Fabric 用户。

有关 {{site.data.keyword.cloud_notm}} Private 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private V3.1.0 文档](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/kc_welcome_containers.html){: external}。

此外，{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 发布之后，{{site.data.keyword.blockchainfull_notm}} Platform Remote Peer (Beta) 程序即宣告终结。您仍可以在 {{site.data.keyword.cloud_notm}} Private 或 AWS 中部署同级，并将其连接到入门套餐或企业套餐网络。有关更多信息，请参阅[针对入门套餐或企业套餐部署同级](/docs/services/blockchain-icp-102/howto?topic=blockchain-icp-102-ibp-peer-deploy#ibp-peer-deploy)以及[在 AWS 中部署同级](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws)。这些同级被视为**分布式**同级而不是远程同级，因为部署由客户进行管理，因此没有一个中央位置可作为远程位置的基础。

此发布还首次对文档目录进行了一些改进。如果您是入门套餐或企业套餐用户，那么您的内容仍可在**了解**、**方法**、**参考**和**帮助**部分中找到，并且链接仍然相同。内容可能位于目录的不同子节中。

## 2018 年 11 月 13 日
{: #whats-new-11-13-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了 {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services (AWS)。

{{site.data.keyword.blockchainfull_notm}} Platform for AWS 允许您在 AWS Cloud 中运行同级，并将同级连接到 {{site.data.keyword.cloud_notm}} 中的现有区块链网络。AWS 中的同级可以使用现有网络中的连接概要文件和其他区块链组件，同时使您可以更灵活地将同级与 {{site.data.keyword.cloud_notm}} 之外的其他应用程序并置在一起。有关更多信息，请参阅[关于 {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws)。

## 2018 年 10 月 4 日
{: #whats-new-10-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 将入门套餐从 Hyperledger Fabric V1.1.0 升级到 V1.2.1。有关更多信息，请参阅[关于入门套餐](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about)。

**重要信息：**Fabric V1.2.1 当前与使用 V1.1.0 同级的 Remote Peer beta 版不兼容。入门套餐网络（在 2018 年 10 月 4 日之前创建，并基于 Fabric V1.1.0）不受影响，仍然可以与 Remote Peer beta 版配合使用。{{site.data.keyword.blockchainfull_notm}} Platform 将更新 Remote Peer beta 版以使用 V1.2.1 同级，该同级将与新的入门套餐网络（Fabric V1.2.1 级别）和企业套餐网络（Fabric V1.1.0 级别）兼容。在 Remote Peer beta 版更新到 Fabric V1.2.1 之前，不要尝试使用新的 V1.2.1 入门套餐网络部署 V1.1.0 远程同级。

## 2018 年 9 月 4 日
{: #whats-new-9-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了 Remote Peer 产品 beta 版。Remote Peer 产品基于 Hyperledger Fabric V1.1.0。通过使用 Remote Peer，您可以在自己的 {{site.data.keyword.cloud_notm}} Private (ICP) 或 Amazon Web Services (AWS) 云环境中运行 {{site.data.keyword.blockchainfull_notm}} Platform 同级节点。有关更多信息，请参阅[关于远程同级](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws)。

## 2018 年 6 月 15 日
{: #whats-new-6-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了入门套餐 GA。有关更多信息，请参阅[关于入门套餐](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about)。

## 2018 年 5 月 15 日
{: #whats-new-5-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 将企业套餐从 Hyperledger Fabric V1.0.0 升级到 V1.1.0。有关更多信息，请参阅[关于企业套餐](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about)。

## 2018 年 3 月 18 日
{: #whats-new-3-18-2018}

{{site.data.keyword.blockchainfull_notm}} Platform 发布了企业套餐 beta 版。企业套餐为您提供了开发和测试环境，以在 {{site.data.keyword.blockchainfull_notm}} Platform 网络中进行学习和实践。有关更多信息，请参阅[关于入门套餐](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about)。

## 2017 年 8 月 11 日
{: #whats-new-8-11-2017}

{{site.data.keyword.blockchainfull_notm}} Platform 替换了 {{site.data.keyword.blockchainfull_notm}} on Cloud，并发布了企业套餐。有关更多信息，请参阅[关于企业套餐](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about)。
