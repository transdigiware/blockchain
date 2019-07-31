---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain offerings, Linux Foundation, Hyperledger Fabric, open source, community contribution

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

# 免责声明
{: #disclaimer}

**注意：**在使用任何 {{site.data.keyword.blockchainfull}} 套餐之前，必须先查看以下信息。

## {{site.data.keyword.IBM_notm}} 支持声明
{: #disclaimer-support-statement}

{{site.data.keyword.IBM_notm}} 在引领创新方面有着悠久的历史，{{site.data.keyword.cloud_notm}} 上的 {{site.data.keyword.blockchainfull_notm}} Platform 产品延续了这一趋势。区块链是一项快速发展的技术，预计将颠覆金融行业、本地和全球供应链，以及对任意数量的业务空间的物流支持。{{site.data.keyword.IBM_notm}} 客户和业务合作伙伴通过各种早期采用程序，一直积极推动区块链作为业界的解决方案。{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 就是这样一种程序。**就像任何新技术一样，{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 用户应该意识到有可能发生快速、根本性的变化**。
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} 背后的体系结构是 Linux Foundation 的 Hyperledger 项目。每一个开放式源代码社区贡献都会改进 Hyperledger Fabric，但同时也带来了采用方面的挑战。**{{site.data.keyword.IBM_notm}} 提醒不要直接在任何 Hyperledger Fabric 区块链网络上定义或交换金融资产<!--, or any assets of value,-->**。

{{site.data.keyword.IBM_notm}} 不支持将 Hyperledger Composer 用于生产的网络，包括 Composer CLI、JavaScript API、REST 服务器和 Web Playground。
{:note}

## 开放式源代码声明
{: #disclaimer-open-source-statement}

{{site.data.keyword.cloud_notm}} 上的 {{site.data.keyword.blockchainfull_notm}} 产品套餐以 Linux Foundation 的 Hyperledger Fabric 堆栈为基础构建。Hyperledger 项目成员（包括 {{site.data.keyword.IBM_notm}}）将持续向 Hyperledger 伞下的各种子项目添加内容。所有添加项都由社区复查、审查和测试。

## 链代码支持声明
{: #disclaimer-chaincode-support-statement}

{{site.data.keyword.blockchainfull_notm}} 网络不支持以下编码实践：

1. 使用具有迭代的相关联数组（顺序在 Go 中为随机的）。
2. 从 KVS 表中读取项目列表（不保证顺序）。
3. 编写线程不安全链代码（可能会并行调用查询和调用）。
4. 在链代码中将全局内存或高速缓存存储器替换为分类帐状态变量。
5. 从链代码中直接访问外部服务，如数据库。
6. 使用可能引入非确定性的库或全局变量（如使用“random”或“time”）。

此外，建议不要编写不确定的链代码，这会导致发生数据一致性和完整性风险。值得注意的是，Hyperledger Fabric 体系结构旨在通过一系列支持和验证检查来反对不确定的链代码。然而，对于不依赖于非静态全局变量（例如时间）的确定性函数，仍然强烈建议您进行编码。
