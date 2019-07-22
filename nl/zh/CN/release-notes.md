---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# 发行说明
{: #release-notes-saas-20}

使用这些按日期分组的发行说明，了解基于 Hyperledger Fabric V1.4.1 构建的 {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} 的最新更改。
{:shortdesc}


## 2019 年 5 月 24 日
{: #05-24-2019}

**Raft 共识协议**：建议用于生产网络的五节点 Raft 排序服务现在可用。此外，出于开发和测试目的，还可以部署单节点 Raft 排序服务。

## 2019 年 5 月 9 日
{: #05-09-2019}

**引入 {{site.data.keyword.blockchainfull_notm}} Platform 控制台 API**

现在，API 可用于供应、编辑和删除同级、排序节点和 CA 节点，因此可以编制脚本来构建区块链网络。使用 [{{site.data.keyword.cloud_notm}} API 文档存储库](/apidocs/blockchain#introduction){: external}中的文档来了解有关 API 的更多信息并试用这些 API。此外，请参阅有关[使用 API 构建网络](/docs/services/blockchain?topic=blockchain-ibp-v2-apis)的主题，以获取有关如何使用 API 来构建网络的指示信息。  

**通道管控**  

通过通道管控更新，可以重新配置策略。您还可以控制哪些通道成员需要对通道更新签名，并且您可以编排签名集合。

## 2019 年 4 月 17 日
{: #04-17-2019}

**能够对节点资源进行大小设置和缩放**  

现在，部署节点时，可以指定容器的 CPU、内存和存储空间的数量（如果适用）。日后可以根据使用模式来扩展或缩减这些资源。有关更多信息，请参阅[分配资源](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources)。

**使用 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

IAM 用于控制用户对控制台 UI 的访问，以及限制用户可以在 UI 中执行的操作。请参阅有关[通过控制台添加和除去用户](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove)的主题，以相关信息。

## 2019 年 4 月 3 日
{: #04-03-2019}

**支持外部 CA**

添加同级或排序节点时，可以选择使用来自外部 CA 的证书，即非 {{site.data.keyword.IBM_notm}} 托管的证书。请参阅有关[将来自外部 CA 的证书用于同级或排序节点](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca)的主题，以获取更多信息。

**调整排序节点性能**

控制台中提供了新的排序节点调整参数，让您对排序节点的吞吐量和性能有更多控制权。请参阅有关[调整排序节点](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning)的主题，以获取有关如何配置参数的指示信息。
