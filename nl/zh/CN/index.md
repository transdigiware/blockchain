---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

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

# {{site.data.keyword.blockchainfull_notm}} Platform 入门
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform 提高了一种受管的全堆栈区块链即服务 (BaaS) 产品，支持在您选择的环境中部署区块链组件。环境可以是 {{site.data.keyword.cloud_notm}}、通过 {{site.data.keyword.cloud_notm}} Private 实现的内部部署环境以及第三方云（例如，Amazon Web Services (AWS)）。在本教程中，我们将引导您逐步完成使用 {{site.data.keyword.blockchainfull_notm}} Platform 设置基本区块链网络的一般过程。
{:shortdesc}

在使用 {{site.data.keyword.blockchainfull_notm}} Platform 产品之前，请阅读[免责声明](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer)部分中的技术和支持信息。
{: important}


## 开始之前
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform 提供不同的产品，帮助所有类型的用户开启其区块链之旅并将其应用程序移动到生产环境。您需要确定将使用的环境和产品。图 1 列出了不同产品的功能、目标受众、定价和云平台。有关每种产品的更多信息，请单击下表中的相应产品。

|**产品**| **包含的内容** | **计费策略** |**云平台**|
| ------------------------- |-----------|-----------|-----------|-----------|
|[**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) |开发者一开始可以使用 IDE，IDE 提供了资源管理器以及可从命令选用板访问的命令，用于快速开发智能合同。|免费|在本地计算机上运行|
|[**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview)| {{site.data.keyword.blockchainfull_notm}} Platform 控制台和 API，可用于在 {{site.data.keyword.cloud_notm}} Kubernetes 集群中部署和管理区块链组件。|[VPC 定价：0.29 美元/VPC-小时](/docs/services/blockchain/howto?topic=blockchain-ibp-saas-pricing)| {{site.data.keyword.cloud_notm}} |
|[**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about)|部署在 {{site.data.keyword.cloud_notm}} Private 集群上的 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，使用 Kubernetes Helm chart 和 API 来供应和管理区块链组件。|[VPC 定价](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)|{{site.data.keyword.cloud_notm}} Private|
|[**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws-about#remote-peer-aws-about)|AWS 快速入门模板，用于部署 {{site.data.keyword.cloud_notm}} 外部的远程同级。|免费|AWS|

*图 1. {{site.data.keyword.blockchainfull_notm}} Platform 产品*


## 步骤 1：获取产品
{: #get-started-ibp-step1}

确保您拥有云帐户或 PPA 许可证以获取 {{site.data.keyword.blockchainfull_notm}} Platform 产品。

* **{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**

  此 VS 代码扩展在 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} 中免费提供，可用于开发、调试和测试智能合同，以便最终部署到 {{site.data.keyword.blockchainfull_notm}} 中。

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  此产品在 {{site.data.keyword.cloud_notm}} 的 [{{site.data.keyword.cloud_notm}}“目录”仪表板 ](https://cloud.ibm.com/catalog){: external}中提供。

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  此产品通过 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html) 作为可部署的 Helm chart 交付。

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  此产品在 AWS 中提供，可以使用[快速入门模板](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}在 AWS 中部署区块链同级。

## 步骤 2：部署 {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  登录到 {{site.data.keyword.cloud_notm}}，并使用本产品创建服务实例。遵循向导来完成网络初始配置。有关更多信息，请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service 入门](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)。

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  部署网络之前，需要先在 {{site.data.keyword.cloud_notm}} Private 集群上安装 Helm chart。然后，可以部署 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，并使用该控制台来部署和操作本地集群上的区块链组件。有关更多信息，请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 入门](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp)。

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  此产品在 AWS 中提供，可以使用[快速入门模板](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}在 AWS 中部署区块链同级。

## 后续步骤
{: #get-started-ibp-next-steps}

在您选择的环境中部署 {{site.data.keyword.blockchainfull_notm}} Platform 后，可以使用联盟、节点、通道和智能合同来设置网络，然后在网络上开始事务处理。有关更多信息，请参阅本文档左侧导航中的**操作指南**部分下的各任务主题。

## 获取支持
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} 提供有关 {{site.data.keyword.IBM_notm}} 实施的区块链解决方案的各种支持选项。有关 {{site.data.keyword.blockchainfull_notm}} Platform 支持的更多信息，请参阅[获取支持](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)。
