---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}

# 常见问题
{: #ibp-v2-faq}

**常规**   

- [通过本机 Hyperledger Fabric 使用 {{site.data.keyword.blockchainfull_notm}} Platform 有何价值？](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [哪个版本的 Hyperledger Fabric 用于 {{site.data.keyword.blockchainfull_notm}} Platform？](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [同级将哪个数据库用于其分类帐？](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [智能合同支持哪些编程语言？](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [支持使用来自非 IBM 认证中心 (CA) 的证书吗？](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [可以从 V1.0 升级到新的 {{site.data.keyword.blockchainfull_notm}} Platform 吗？](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [删除 {{site.data.keyword.blockchainfull_notm}} Platform 服务会发生什么情况？](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [哪些区域可用于在 {{site.data.keyword.cloud_notm}} 上运行的区块链服务？](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [可以使用现有 {{site.data.keyword.cloud_notm}} Kubernetes Service 集群吗？](#ibp-v2-faq-v2-Infrastructure-4-2)
- [我有权访问日志记录服务吗？我可以使用哪些日志？](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的优点是什么？](#ibp-v2-faq-icp-benefits)
- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 支持哪些环境？](#ibp-v2-faq-icp-environments)
- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的定价模型是什么？](#ibp-v2-faq-icp-pricing)
- [为了使用 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud，我需要安装哪些服务？](#ibp-v2-faq-icp-services)
- [如何针对开发、测试和生产环境估算 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的大小设置需求？](#ibp-v2-faq-icp-sizing)
- [删除 Helm 发布时，区块链组件会发生什么情况？](#ibp-v2-faq-icp-delete)

## 通过本机 Hyperledger Fabric 使用 {{site.data.keyword.blockchainfull_notm}} Platform 有何价值？
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform 支持客户机轻松部署定制区块链网络。您可以使用直观的控制台 UI 来快速部署网络，轻松安装和实例化智能合同以及监视事务。

## 哪个版本的 Hyperledger Fabric 用于 {{site.data.keyword.blockchainfull_notm}} Platform？
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 和 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 使用 Hyperledger Fabric V1.4.1。

## 同级将哪个数据库用于其分类帐？
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

通过 {{site.data.keyword.blockchainfull_notm}} Platform 部署的所有同级都使用 CouchDB 作为分类帐的数据库。

## 智能合同支持哪些语言？
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform 支持使用 Go 和 Node.js 编写的智能合同。新的 Hyperledger Fabric 编程模型目前仅支持 Node.js，但即将支持更多语言。如果您希望保留现有的应用程序代码，或者将 Fabric SDK 用于除 Node.js 以外的语言，那么仍然可以使用较低级别的 Fabric SDK API 来连接到 {{site.data.keyword.blockchainfull_notm}} Platform 网络。

## 支持使用来自非 IBM 认证中心的证书吗？
{: #ibp-v2-faq-v2-external-certs}
{: faq}

支持，您可以自带证书，只要证书是由符合 X.509 的 CA 签发的即可。

## 可以从 V1.0 升级到新的 {{site.data.keyword.blockchainfull_notm}} Platform 吗？
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

入门套餐不能。无法通过入门套餐升级到新的 {{site.data.keyword.blockchainfull_notm}} Platform。对于企业套餐，未来能够升级到新的 {{site.data.keyword.blockchainfull_notm}} Platform。您将收到 {{site.data.keyword.blockchainfull_notm}} Platform 团队发送给帐户所有者的电子邮件，用于提供相关帮助。

## 删除 {{site.data.keyword.blockchainfull_notm}} Platform 服务会发生什么情况？
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

删除 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例时，将删除所有区块链 CA、同级和排序节点及其关联的存储器。

## 哪些区域可用于在 {{site.data.keyword.cloud_notm}} 上运行的区块链服务？
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform 的可用区域在 [{{site.data.keyword.blockchainfull_notm}} Platform 位置](/docs/services/blockchain?topic=blockchain-ibp-regions-locations)中列出。请注意，必须在区块链服务所在的区域中创建 {{site.data.keyword.cloud_notm}} Kubernetes Service 集群，才能识别到该集群。识别在其他区域中所创建集群的功能即将可用。

## 可以使用现有 {{site.data.keyword.cloud_notm}} Kubernetes Service 集群吗？
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

只要现有 Kubernetes 集群满足以下条件，就能用于 {{site.data.keyword.blockchainfull_notm}} Platform：
- 在 Kubernetes V1.11 或更高的稳定版本上运行。
- 集群中有足够的可用资源。

## 我有权访问日志记录服务吗？我可以使用哪些日志？
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

现在，通过 {{site.data.keyword.blockchainfull_notm}} Platform，您可以直接在 Kubernetes 仪表板中访问同级、CA 和排序节点日志。建议您利用 {{site.data.keyword.cloud_notm}} LogDNA 服务，此服务支持您轻松地实时解析日志。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的优点是什么？
{: #ibp-v2-faq-icp-benefits}
{: faq}

请参阅有关 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 提供的功能](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers)的主题。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 支持哪些环境？
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 支持 {{site.data.keyword.cloud_notm}} Private V3.2 支持的所有环境。请参阅 [Supported operating systems and platforms](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external}，以获取更多信息。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的定价模型是什么？
{: #ibp-v2-faq-icp-pricing}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [许可](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)基于产品可用的 CPU (VPC) 数量。有关详细信息，请[联系 IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}。

## 为了使用 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud，我需要安装哪些服务？
{: #ibp-v2-faq-icp-services}
{: faq}

您只需要安装 {{site.data.keyword.cloud_notm}} Private V3.2。

## 如何针对开发、测试和生产环境估算 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 的大小设置需求？
{: #ibp-v2-faq-icp-sizing}
{: faq}

了解了需要多少 CA、同级和排序节点后，就可以检查[缺省资源分配表](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources)，以获取网络所需的 CPU (VPC) 大致估算值。

## 删除 Helm 发布时，区块链组件会发生什么情况？
{: #ibp-v2-faq-icp-delete}
{: faq}

从 {{site.data.keyword.cloud_notm}} Private 集群中删除 Helm 发布时，不会删除关联的区块链组件。要从集群中正确删除 Helm 发布，应首先使用区块链控制台或区块链 API 来删除所有组件。然后可以删除 Helm chart。  
