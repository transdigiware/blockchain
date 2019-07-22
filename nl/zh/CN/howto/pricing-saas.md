---

copyright:
  years: 2019
lastupdated: "2019-06-20"

keywords: pricing model, hourly, per hour, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost, billing

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:gif: data-image-type='gif'}
{:pre: .pre}

# {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 定价
{: #ibp-saas-pricing}

本指南可帮助您了解 {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} 的定价模型，以及在开发和扩展同级、排序节点、认证中心组件（这些组件基于 Hyperledger Fabric V1.4.1）构成的区块链网络时将支付的费用。
{:shortdesc}

_此定价模型仅适用于 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}。如果使用的是入门套餐或企业套餐并有定价方面的疑问，请参阅入门套餐和企业套餐[定价](/docs/services/blockchain?topic=blockchain-ibp-pricing)。_

## 定价模型
{: #ibp-saas-pricing-model}

{{site.data.keyword.blockchainfull_notm}} Platform 引入了基于虚拟处理器核心 (VPC) 使用量的全新按小时定价模型。此简化的模型基于 {{site.data.keyword.blockchainfull_notm}} Platform 节点每小时使用的 CPU（或 VPC）数量，统一费率为 **0.29 美元/VPC-小时**。

VPC 是用于确定 {{site.data.keyword.IBM_notm}} 产品许可成本的计量单位。它基于可用于该产品的虚拟核心 (vCPU) 的数量。vCPU 是分配给虚拟机的虚拟核心，或者是物理处理器核心。出于 {{site.data.keyword.blockchainfull_notm}} Platform 成本估算目的，**1 个 VPC = 1 个 CPU = 1 个 vCPU = 1 个核心**。
{:note}

对于总成本估算，请记住区块链网络由 {{site.data.keyword.cloud_notm}} Kubernetes 集群组成，Kubernetes 集群包含 {{site.data.keyword.blockchainfull_notm}} Platform 组件并使用您选择的存储器。对于 {{site.data.keyword.cloud_notm}} Kubernetes 集群以及您选择的存储器，会单独进行收费。操作工具实例（也称为控制台）运行所在的集群是免费的。请参阅[体系结构参考](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview-architecture)主题，以获取相关说明。有关如何计算费用的更多详细信息如下所述。

### 您是开发者吗？
{: #ibp-saas-pricing-developer}

开发者一开始可以使用免费的 [VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external}。使用此集成开发环境可在本地编写、测试、调试和打包智能合同，对于 {{site.data.keyword.blockchainfull_notm}} Platform 部署，还可以编写客户机应用程序。请从头开始或访问教程和样本来学习区块链基础知识。然后，返回此处并将 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例链接到 Kubernetes 集群，以便可以使用控制台来构建区块链网络。

## 新定价模型的优点
{: #ibp-saas-pricing-benefits}

- **无成员资格费用**：免除成员资格费用意味着可以直接对区块链组件投资。
- **估算清晰**：使用 {{site.data.keyword.cloud_notm}} 仪表板中提供的成本估算工具，并利用简单的按小时定价模型，成本估算简单明了。
- **无需起步价**：仅需按使用量付费，无需使用按小时计费的最低 VPC 套餐，因此入门价格非常低。
- **计算资源可伸缩性**：可以选择在峰值使用时间段扩展计算资源，或者在不需要计算资源时缩减至容量的很小一部分以尽可能降低费用。  

总之，通过这些功能，避免了考虑成员资格限制或购买超出需求的计算资源的复杂性。

## 成本的关键要素
{: #ibp-saas-pricing-elements}

由于区块链网络由 {{site.data.keyword.cloud_notm}} Kubernetes 集群组成，Kubernetes 集群包含 {{site.data.keyword.blockchainfull_notm}} Platform 组件并使用您选择的存储器，因此总成本由以下各个要素构成：

- **{{site.data.keyword.blockchainfull_notm}} Platform** 统一费率 0.29 美元/VPC-小时。此费用表示对 Kubernetes 集群中区块链组件的收费。
- **{{site.data.keyword.cloud_notm}} Kubernetes Service** 集群的分层定价，供应付费集群时，此定价会显示在 {{site.data.keyword.cloud_notm}} 中。此定价包含对计算资源（即 CPU 和内存）的收费。{{site.data.keyword.cloud_notm}} Kubernetes Services 按分层模型定价，该模型基于每月使用小时数。因此，在检查价格套餐时，请注意，全天候使用量相当于每月 720 小时。请参阅 [Kubernetes Service 目录页面](https://cloud.ibm.com/kubernetes/catalog/cluster){: external}上的表，以获取有关集群定价的更多详细信息。
- 根据需要选择**存储器**套餐。请参阅[了解 Kubernetes 存储器基础知识](/docs/containers?topic=containers-kube_concepts#kube_concepts)主题，以了解有关存储类选项及其[成本](https://www.ibm.com/cloud/file-storage/pricing){: external}的更多信息。{{site.data.keyword.blockchainfull_notm}} Platform 节点使用集群的缺省存储类。在 {{site.data.keyword.cloud_notm}} 中供应 Kubernetes 集群时，它已预配置[铜牌级文件存储器](/docs/containers?topic=containers-file_storage#file_predefined_storageclass)作为持久性存储器插件。

## 定价示例
{: #ibp-saas-pricing-scenarios}

下表提供了使用[缺省资源分配]( #ibp-saas-pricing-default)（除非另有说明）进行定价的两个示例。
- **测试网络**场景适用于入门和测试智能合同。
- **加入生产网络**场景包含两个同级（建议使用两个同级以实现高可用性）和一个需要用于组织成员资格的认证中心 (CA)。
   - 这两个同级可以加入在其他位置托管的生产 {{site.data.keyword.blockchainfull_notm}} Platform 网络。
   - 在不使用节点时，可以随时将节点回调至最低利用率状态（0.001 个 CPU）以[降低成本](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources)。
   - 由于此场景旨在用于**生产**环境，因此：
     - 缺省计算资源已翻倍，用于提供更大容量。
     - 选择了[银牌级](/docs/containers?topic=containers-file_storage#file_silver){: external}存储类，以支持更快性能。

|定价选项**（1 个 VPC = 1 个 CPU）|**测试网络**|**加入生产网络**|
|-|------------|-----------------------------|
|**CPU 分配**|1.85 个 CPU<br> 包括：<br> - 1 个同级<br> - 2 个 CA<br> - 1 个排序节点|4.9 个 CPU<br> 包括：<br> - 2 个同级（用于 HA）<br> **（2 倍缺省计算资源）**<br>- 1 个 CA<br>  |
|**每小时成本：{{site.data.keyword.blockchainfull_notm}} Platform**|0.54 美元<br> （1.85 个 CPU x 0.29 美元/VPC-小时）|1.42 美元<br> （4.9 个 CPU x 0.29 美元/VPC-小时）|
|**每小时成本：{{site.data.keyword.cloud_notm}} Kubernetes 集群**|0.12 美元<br> （计算资源：2 x 4 层）<br> （IP 分配：16 美元/月）|0.46 美元<br> （计算资源：8 x 32 层）<br> （IP 分配：16 美元/月）|
|**每小时成本：存储器**|0.07 美元<br> 340 GB<br> [铜牌级](https://www.ibm.com/cloud/file-storage/pricing){: external}<br>  2 IOPS/GB|0.13 美元<br> 420 GB<br> [银牌级](https://www.ibm.com/cloud/file-storage/pricing){: external} <br> 4 IOPS/GB|
|**每小时总成本**|**0.73 美元**|**2.01 美元**| |
** 将 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例链接到 {{site.data.keyword.cloud_notm}} Kubernetes 免费集群时，可免费预览 {{site.data.keyword.blockchainfull_notm}} Platform 30 天。但性能在吞吐量、存储器和功能方面会受到限制。{{site.data.keyword.cloud_notm}} 将在 30 天后删除 Kubernetes 集群，并且您无法将任何节点或数据从免费集群迁移到付费集群。  

您的实际成本将根据其他因素而变化，例如事务处理速率、需要的通道数、事务处理上的有效内容大小以及最大并发事务处理数。上述定价示例仅基于 {{site.data.keyword.cloud_notm}} Kubernetes 单专区集群。如果选择多专区集群，那么对于其他专区和必需的多专区负载均衡器，会收取额外费用。
{:note}

对于可以供应并关联到单个 Kubernetes 集群的服务实例数量没有限制，但您需要通过监视 CPU、内存和存储器使用情况来确保有足够的资源可用，以避免服务中断。{{site.data.keyword.blockchainfull_notm}} Platform 节点不必位于自己的集群中。可以在运行区块链组件的集群中运行其他 {{site.data.keyword.cloud_notm}} 服务，但同样需要确保有足够的计算资源和存储器来满足所有服务实例的所有需求。

## 缺省资源分配
{: #ibp-saas-pricing-default}

下表中的值可用于基于 CPU、计算资源和存储器来估算定制网络的每小时成本。这些最低建议值足以满足入门需求。在监视网络使用情况时，您可能会发现实际的资源需求和成本会因用例以及安全性和可用性需求而变化。  

|**组件**（所有容器）|CPU|内存 (GB)|存储器 (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
|**同级**|1.2|2.4|200（包括 100 GB 用于同级，100 GB 用于 CouchDB）|
|**CA**|0.1|0.2|20|
|**排序节点**|0.45|0.9|100|


## 帐单
{: #ibp-saas-pricing-billing}

**现收现付**帐户的计费和使用情况信息在 {{site.data.keyword.cloud_notm}} 仪表板的[使用情况](https://cloud.ibm.com/billing/usage)磁贴中提供。测量服务会每小时生成一次 {{site.data.keyword.blockchainfull_notm}} Platform VPC 使用总量快照，这样每月累积用量会反映在**使用情况**磁贴中。

创建新节点时，可能需要一小时才能在 {{site.data.keyword.cloud_notm}} 仪表板的**使用情况**磁贴中反映出 VPC 使用情况。
{:note}

### 监视使用情况
{: #ibp-saas-pricing-usage}

在获取帐单之前，可以在 {{site.data.keyword.cloud_notm}} 仪表板的**使用情况**磁贴中监视 {{site.data.keyword.blockchainfull_notm}} Platform 和 Kubernetes 集群成本。{{site.data.keyword.blockchainfull_notm}} Platform VPC 使用情况每小时评估一次。**这些成本仅为估算值。**实际成本反映在每月帐单上。

#### {{site.data.keyword.blockchainfull_notm}} Platform 和 Kubernetes Service 使用情况

以下剪辑提供了如何针对包含单个 CA 节点的 {{site.data.keyword.blockchainfull_notm}} Platform 查看相应费用的简单示例。

![监视使用情况](../images/usage_monitoring.gif){: gif}

导航至 {{site.data.keyword.cloud_notm}} 仪表板顶部的**管理**，单击**计费和使用情况**，然后单击左侧菜单中的**使用情况**。**服务**子部分下的饼图按您本月使用和消耗的服务产品类型对总成本进行细分。使用此图表可了解 {{site.data.keyword.blockchainfull_notm}} Platform、Kubernetes Service 和存储器占总成本的比例。

向下滚动时，可以在列表视图中按**类型**和**成本**查看类似的细分。您可看到列出了“Kubernetes Service”和“Blockchain Platform”以及已在集群中供应的其他服务。单击其中每项旁边的**查看套餐**，可了解按度量值划分的成本细分。例如，`VIRTUAL_PROCESSOR_CORE_HOURS` 表示到目前为止已评估的 VPC 小时总数。使用此值可了解根据 **0.29 美元/VPC-小时**的定价度量值，将向您收取多少费用。

#### IP 分配费用

在 {{site.data.keyword.cloud_notm}} 中供应 Kubernetes 集群时，将针对 IP 分配评估统一的每月费用。此费用按专区收取，因此如果在集群中供应三个专区，那么可以将此费用乘以三。以下示例显示的是单个专区的费用。

![IP 分配费用](../images/ip_allocation_charge.png "Kubernetes 集群 IP 分配费用")

此费用显示在“使用情况”磁贴的**发票**选项卡上。单击**下一个经常性发票**下的链接，可查看 IP 分配的费用。

#### 存储器使用情况

如果使用的是 {{site.data.keyword.cloud_notm}} 文件存储器，那么成本每月评估一次，因此存储器成本估算要到月底才会显示。但是，在整个月份中供应的存储器会在“使用情况”磁贴中的**销售** > **订单**下作为行项列出。在**项**列中查找关于部署同级、CA 或排序节点时动态供应的存储器的描述。

## 优化节点成本
{: #ibp-saas-pricing-shutdown}

{{site.data.keyword.blockchainfull_notm}} Platform 定价模型的其中一个主要优点是能够在不需要资源时回调或删除资源。

- **将节点切换至最低利用率状态**  
  单个节点上的 CPU 可以缩减至 0.001 个 CPU，以彻底将费用降至最低。执行这些操作会导致节点无法正常运行。稍后需要计算资源时，可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台中的“重新分配”选项来扩展至所需的资源量。有关如何重新分配资源的更多信息，请参阅[重新分配资源](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources)。

- **根据需要删除未使用的同级和部署新同级**  
  由于分类帐存储在排序节点上，因此部署新同级并将其加入通道时，同级会收到分布式分类帐的副本。此方法的缺点是您需要生成新证书，并再次将同级加入通道。

  建议永远不要删除 CA 节点，因为该节点上的数据绝对无法恢复。同样，如果您只有一个排序节点，那么也绝不应将其删除。  
  {:important}

- **监视资源分配并根据需求进行调整**  
  监视资源使用情况一段时间后，您可能会决定缩减分配给节点的资源，同时仍确保节点有足够的性能。遵循控制台中[重新分配资源](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources)的指示信息进行操作时，会更新对节点总 VPC 的影响，并且此影响可用于估算修改后的每月成本。
