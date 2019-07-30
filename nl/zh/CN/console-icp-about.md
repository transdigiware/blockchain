---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# 关于 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform 可以在公共云和私有云中进行部署，例如 {{site.data.keyword.cloud_notm}}、您自己的数据中心以及第三方公共云。{{site.data.keyword.cloud_notm}} Private 是 IBM 的基于 Kubernetes 的容器编排平台，通过此平台可以进行此多云部署。此发行版提供了可用来在 {{site.data.keyword.cloud_notm}} Private 集群上部署和管理区块链组件的区块链控制台用户界面。{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private 使用 Hyperledger Fabric V1.4.1 代码库，并支持在 {{site.data.keyword.cloud_notm}} Private V3.2.0 上进行部署。
{:shortdesc}

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 提供的功能
{: #console-icp-about-offers}

您可以使用此产品在 {{site.data.keyword.cloud_notm}} Private 的部署上安装 {{site.data.keyword.blockchainfull_notm}} Platform 控制台。然后可以使用该控制台在本地集群上创建 Hyperledger Fabric 区块链的所有基础组件：认证中心、排序服务和同级。有关 Hyperledger Fabric 网络构建块的更多信息，请参阅[区块链组件概述](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview)。您还可以使用控制台通过导入在其他 {{site.data.keyword.cloud_notm}} Private 集群上或 {{site.data.keyword.cloud_notm}} 上部署的节点，以操作分布式多云网络。


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

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 是 {{site.data.keyword.cloud_notm}} Private 的捆绑产品，作为 Kubernetes Helm chart 交付。有关 {{site.data.keyword.cloud_notm}} Private 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private V3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} 的文档。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 是否适用于您
{: #console-icp-about-suitable}

通过在 {{site.data.keyword.cloud_notm}} 之外运行 {{site.data.keyword.blockchainfull_notm}} Platform，可以更灵活地扩展或加入区块链网络。这也有助于网络发起者允许新成员通过使用其所选平台加入网络，从而扩展其网络。此外，还允许有兴趣加入区块链网络的组织将其同级与现有应用程序并置，或者与其记录系统集成。

此产品的用户应负责管理其自己的安全性和基础架构。{{site.data.keyword.cloud_notm}} 不提供这些服务。开始之前，请查看下一部分中的[注意事项和限制](#console-icp-about-considerations)。

## 注意事项和限制
{: #console-icp-about-considerations}

开始之前，请确保了解以下限制：

- 您负责区块链组件的运行状况监视、安全性、日志记录以及资源使用情况管理。
- 控制台只能用于创建和管理基于 Hyperledger Fabric V1.4.1 或更高版本的组件。
- 每个 Kubernetes 名称空间只能部署一个控制台。如果计划创建多个区块链网络，例如为开发、编译打包和生产创建不同的环境，那么应该为每个环境创建唯一的名称空间。
- 只能导入已从其他 {{site.data.keyword.blockchainfull_notm}} Platform 控制台导出的节点。为了能够通过控制台操作导入的同级节点或排序节点，还需要将关联节点的组织 MSP 定义和管理员身份导入到控制台。
- 仅 {{site.data.keyword.cloud_notm}} Private V3.2 Enterprise Edition 上支持 {{site.data.keyword.blockchainfull_notm}} Platform。不支持使用 {{site.data.keyword.cloud_notm}} Private V3.2 Community Edition。
- 不能使用 {{site.data.keyword.IBM_notm}} Multicloud Manager 来安装 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart。

有关 {{site.data.keyword.cloud_notm}} Private 支持的环境的概述，请参阅[支持的环境](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}。
{:important}

## 系统先决条件
{: #console-icp-about-prerequisites}

请参阅[系统需求](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external}，以获取 {{site.data.keyword.cloud_notm}} Private 的硬件和软件先决条件的列表。但请注意，Linux on Power (ppc64le) POWER8 系统上不支持 {{site.data.keyword.blockchainfull_notm}} Platform。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm chart 已经过验证，可以使用以下工作程序节点和备用存储器在 Ubuntu Linux 上的 {{site.data.keyword.cloud_notm}} Private V3.2 集群中运行：

- **LinuxONE 和 {{site.data.keyword.IBM_notm}} Z**：z/VM 和 KVM（使用 NFS）。
- **x86**：Linux 64 位（使用 GlusterFS）。

## 许可
{: #console-icp-about-license}

请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 定价](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)，以获取有关许可和定价的更多信息。

## 安装 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 作为 Helm chart 文件交付，可安装在本地 {{site.data.keyword.cloud_notm}} Private 集群中。安装 Helm chart 之后，您可在 {{site.data.keyword.cloud_notm}} Private 目录中找到作为应用程序的 {{site.data.keyword.blockchainfull_notm}} Platform。有关如何安装 Helm chart 和必需的必备软件的指示信息，请参阅[安装 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install)。

如果您是 {{site.data.keyword.cloud_notm}} Private 的新用户，并且希望了解有关安装和部署 {{site.data.keyword.cloud_notm}} Private 的信息和技巧，请参阅[设置 {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)。

您可以在防火墙后部署 {{site.data.keyword.blockchainfull_notm}} Platform，而无需访问公用因特网。下载的 Helm chart 包中包含 {{site.data.keyword.blockchainfull_notm}} Platform 使用的所有 Fabric 组件 Docker 映像，无需在部署期间从 DockerHub 进行拉取。

## 安全注意事项
{: #console-icp-about-security}

由于这些组件是在您自己的基础架构上部署的，因此您负责管理其安全性。这包括重要安全性领域，例如密钥管理和数据加密。考虑组件的安全性时，请查看以下主题。

### 数据安全性
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 使用的是基于[对称密钥加密](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external}的全盘加密来保护每个服务实例创建的所有数据。您必须在自己的环境中执行类似的步骤来保护同级数据。

状态数据库（无论使用的是 LevelDB 还是 CouchDB）中的数据未加密。可以使用应用程序级别加密来保护状态数据库中的静态数据。

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### 密钥管理
{: #console-icp-about-security-key-management}

密钥管理是安全性中至关重要的一个方面。如果专用密钥泄漏或丢失，那么恶意参与者可能有能力访问您的数据和功能。{{site.data.keyword.IBM_notm}} 使用称为 [Hardware Security Module](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) 的物理设备来存储 {{site.data.keyword.blockchainfull_notm}} Platform 企业套餐网络的专用密钥。

您负责管理专用密钥。虽然可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台 UI 来生成专用密钥，但这些密钥不会由控制台存储，也不会存储在 {{site.data.keyword.cloud_notm}} Private 中。因此，必须安全地存储密钥，避免密钥泄露。

您可以使用“密钥代管”来恢复丢失的专用密钥。这需要在丢失任何密钥之前执行。如果无法恢复专用密钥，那么需要通过向认证中心注册新的身份来获取新的专用密钥。您还应该从加入的任何通道中除去并替换 signCert。

### TLS
{: #console-icp-about-security-tls}

[传输层安全性](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) 嵌入在 Hyperledger Fabric 的信任模型中。所有 {{site.data.keyword.blockchainfull_notm}} Platform 组件都使用 TLS 进行认证并相互通信。因此，{{site.data.keyword.cloud_notm}} Private 上的节点需要能够完成与其他组件和应用程序的 TLS 握手。这样做的其中一个影响是，您需要在从客户机应用程序到节点之间的防火墙中启用传递（例如，使用白名单来实现）。

### 应用程序安全性
{: #console-icp-about-security-appl}

由于所有链代码调用都已签名，因此 Fabric 会管理应用程序安全性。此外，Fabric 还包含基于 ACL 的应用程序级别检查。

## 获取支持
{: #console-icp-about-support}

有关如何获取有关 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 的支持的更多信息，以及可用于对问题进行故障诊断的免费区块链开发者资源和支持论坛的更多信息，请参阅[获取支持](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)。

如果您已购买 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 许可证并且要与客户支持人员联系，请参阅有关[访问 {{site.data.keyword.IBM_notm}} 支持社区并开具支持凭单](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}的信息。
