---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 入门
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform 可以在公共云和私有云中进行部署，例如 {{site.data.keyword.cloud_notm}}、您自己的数据中心以及第三方公共云。{{site.data.keyword.cloud_notm}} Private 是 {{site.data.keyword.IBM_notm}} 的基于 Kubernetes 的容器编排平台，通过此平台可以进行此多云部署。您可以使用此产品在 {{site.data.keyword.cloud_notm}} Private 的部署上安装 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，然后使用该控制台在本地集群上创建 {{site.data.keyword.blockchainfull_notm}} 组件。您还可以使用控制台通过导入在其他 {{site.data.keyword.cloud_notm}} Private 集群上或 {{site.data.keyword.cloud_notm}} 上部署的节点，以操作分布式多云网络。
{:shortdesc}

**目标受众**：本主题适用于负责在 {{site.data.keyword.cloud_notm}} Private 上部署 {{site.data.keyword.blockchainfull_notm}} Platform 的系统管理员。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 是 {{site.data.keyword.cloud_notm}} Private 的捆绑产品。有关 {{site.data.keyword.cloud_notm}} Private 的更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private V3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} 的文档。

## 步骤 1：在 {{site.data.keyword.cloud_notm}} Platform 上设置 Kubernetes 集群
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

第一步是在您选择的云平台中的 {{site.data.keyword.cloud_notm}} Private 上设置 Kubernetes 集群。您可以访问[设置 {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup) 以获取建议和指导信息。

如果要构建将在生产中使用的网络，那么需要设置集群部署和区块链网络，以实现高可用性：

- 有关配置集群的建议，请访问 [Implement high availability on {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}。
- 准备好开始构建网络后，请访问[同级的高可用性注意事项](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-peers)和[排序服务的高可用性注意事项](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-ordering-service)。

## 步骤 2：安装 {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 作为 Helm chart 交付，可从 Passport Advantage (PPA) 进行下载。要了解如何在本地集群上安装 Helm chart，请访问[安装 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install)。

## 步骤 3：部署 {{site.data.keyword.blockchainfull_notm}} Platform 控制台
{: #get-started-console-icp-step-three-deploy-console}

安装 Helm chart 后，可以单击“目录”页面上的 {{site.data.keyword.blockchainfull_notm}} Platform 应用程序磁贴，以在本地集群上安装 {{site.data.keyword.blockchainfull_notm}} Platform 控制台。要了解如何配置控制台以及区块链组件所需的资源，请参阅[部署 {{site.data.keyword.blockchainfull_notm}} Platform 控制台](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp)。

## 步骤 4：将用户作为管理员添加到控制台
{: #get-started-console-icp-step-four-add-console-admin}

控制台管理员可以使用部署期间提供的电子邮件地址和密码登录到控制台。提供的密码将成为控制台的缺省密码，并且所有新用户首次登录到控制台时都会使用该密码。然后，管理员可以将新用户添加到控制台，以允许其他人登录并开始使用 {{site.data.keyword.blockchainfull_notm}} 节点。管理员还可以设置新的缺省密码。要了解更多信息，请参阅[通过控制台管理用户](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)。

## 步骤 5：使用控制台创建组件
{: #get-started-console-icp-build-network}

部署控制台后，可以使用控制台在本地集群上创建 {{site.data.keyword.blockchainfull_notm}} 组件，并对这些组件进行操作和管理。要开始使用控制台 UI，请访问[构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)。

## 步骤 6：连接不同云上的网络
{: #get-started-console-icp-import-nodes}

可以使用控制台来操作在其他 {{site.data.keyword.cloud_notm}} Private 集群上或在 {{site.data.keyword.cloud_notm}} 上运行的组件。首先，需要通过最初部署组件的控制台将组件信息导出到 JSON 文件。接着，可以将节点 JSON 文件导入到在本地集群上部署的控制台，并管理不同云上的组件。有关更多信息，请参阅[导入节点](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes)。
