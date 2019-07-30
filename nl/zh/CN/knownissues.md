---

copyright:
  years: 2017, 2019

lastupdated: "2019-06-18"

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

# 已知问题
{: #known-issues}

此页面描述了在使用入门套餐或企业套餐时可能遇到的已知问题。
{:shortdesc}

已报告以下问题：
- **尚不支持配置外部 CA**。或者，您可以通过“网络监视器”来生成和上传管理证书。有关更多信息，请参阅“网络监视器”中[“成员”屏幕的“证书”选项卡](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members)上的描述。
- 在入门套餐网络的“网络监视器”中，在单击“概述”屏幕上列出的节点的**查看日志**时，将打开 {{site.data.keyword.cloud}} 日志记录 Kibana 界面。**缺省情况下，Kibana 预先配置为显示最近 30 天的活动的日志**。如果最近 30 天内没有任何活动，那么您将看到消息：*找不到结果*。要查看其他日志，您可以单击右上角您的用户名下面的计时器图标，并设置较大的时间范围，例如*年初至今*。
- [{{site.data.keyword.cloud_notm}} Log Analysis 服务](https://cloud.ibm.com/catalog/services/log-analysis){: external}收集入门套餐网络的日志。缺省情况下，会通过 Log Analysis 服务的轻量套餐来收集日志。此套餐是免费的，并且**只允许搜索每天前 500 MB 的日志**。如果网络日志超过 500 MB，那么无法在 Kibana 中查看新日志。如果网络生成的日志超过 500 MB，您可以升级到 Log Analysis 服务的付费版本。
- 由于入门套餐不是生产环境，所以**应用程序可能无法立即访问网络资源**。
  - 如果发生这种情况，建议首先增大 Fabric SDK 中的缺省超时值。有关设置超时值的更多信息，请参阅[在 Fabric SDK 中设置超时值](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-set-timeout-in-sdk)。
  - 您还可以在应用程序级别重试请求。
- 因后台网络问题，**链代码容器有时可能会停止**，并且可能需要在用户调用链代码后对其重建并重新启动。如果发生这种情况，您的链代码可能需要几分钟时间才能响应。
- 由于入门套餐网络中的资源限制（即，每个同级 1 个 CPU 和 4 Gi RAM），所以**在链代码实例化期间可能会遇到 `REQUEST_TIMEOUT` 错误**。如果发生这种情况，请重试实例化步骤。如果错误继续存在，可以增大链代码实例化超时。在连接概要文件中，链代码实例化超时设置为 300 秒。
  - 如果要在 SDK 中使用此缺省超时值，请如下所示复制连接概要文件中的 **client** 部分（用于将超时设置为 300 秒），并确保 SDK 对其进行读取。请注意，对于 Node SDK，连接概要文件中的此超时设置会影响所有调用，例如 `invoke` 和 `queries`。
    ```
    "client": {
       "organization": "Org1",
       "connection": {
           "timeout": {
               "peer": {
                   "endorser": "300"
               }
           }
       }
    },
    ```
    {:codeblock}
  - 如果要覆盖 SDK 中链代码实例化命令的超时设置，请将其设置回缺省值或将其更改为 300 秒或更长时间。
    - 如果使用的是 Node SDK，那么可以更改 `sendInstantiateProposal(request, timeout)` 方法的超时设置。有关更多信息，请参阅 [sendInstantiateProposal(request, timeout)](https://fabric-sdk-node.github.io/Channel.html#sendInstantiateProposal){: external}。
    - 如果使用的是 Java SDK，那么可以更改 `instantiateProposalRequest.setProposalWaitTime(DEPLOYWAITTIME)` 命令的超时设置，该命令位于 `src/test/java/org/hyperledger/fabric/sdkintegration/End2endIT.java` 文件中。

有关 {{site.data.keyword.cloud_notm}} 上 {{site.data.keyword.blockchainfull_notm}} Platform 网络的支持和帮助，请参阅[获取支持](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)。
