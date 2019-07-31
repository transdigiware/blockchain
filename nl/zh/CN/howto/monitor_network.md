---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: view Logs, IBM Cloud Private, logs of a specific network component, monitor blockchain network

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 监视区块链网络
{: #monitor-blockchain-network}

本教程显示如何在 {{site.data.keyword.cloud_notm}} 上查看和监视 {{site.data.keyword.blockchain}} 网络的状态信息。
{:shortdesc}


## 监视同级、排序节点和 CA
{: #monitor-blockchain-network-monitor-nodes}

可以针对某个网络节点发出 HTTP **HEAD** 请求，以检查该节点的状态。网络节点可以是区块链网络中的同级、排序节点或 CA。**HEAD** 请求类似于 GET 请求，但仅发送不含主体的头。如果节点工作正常，您会获得 200 响应。

1. 在“网络监视器”的“概述”屏幕中，单击**连接概要文件**。然后，可以单击**原始 JSON** 以在 Web 浏览器中查看连接概要文件，或者单击**下载**以在本地保存连接概要文件。
2. 在连接概要文件中，找到要检查的网络节点的 URL 信息。例如，`fabric-orderer-20190b` 排序节点的 URL 为 `grpcs://fft-zbc02b.4.secure.blockchain.ibm.com:20190`。![排序节点 URL 示例](../images/orderer_url.png "排序节点 URL 示例")
3. 将 URL 中的 **grpcs** 替换为 **https**。以上示例中的 URL 会变成 `https://fft-zbc02b.4.secure.blockchain.ibm.com:20190`。
4. 使用 curl 或 Chrome Postman 应用程序等工具对该 URL 发出 **HEAD** 请求。
    - 如果获得 200 状态响应，说明网络节点工作正常。
    - 如果 **HEAD** 请求失败并返回连接错误，说明网络节点可能未在运行、节点 URL 不正确，或者防火墙阻止您访问该节点。您必须解决此错误；否则，应用程序无法连接该节点。

以下示例显示 curl 中的 **HEAD** 请求及 200 响应。请注意，您可以忽略 grpc 错误，因为 HTTP **HEAD** 请求会检查该节点是否可访问。如果可访问，那么对该节点的 grpc 请求在应用程序中也会正常工作。

```
C:\>curl -i --head https://fft-zbc02b.4.secure.blockchain.ibm.com:20190
HTTP/2 200
contnent-type: application/grpc
grpc-status: 8
grpc-message: malformed method name: "/"
```

以下示例显示 curl 中的 **HEAD** 请求及连接错误。

```
C:\>curl -i --head https://fft-zbc02b.4.secure.blockchain.ibm.com:20190
curl: (7) Failed to connect to fft-zbc02b.4.secure.blockchain.ibm.com:20190: Connection refused
```

下图显示 Chrome Postman 应用程序中的 **HEAD** 请求及 200 响应。

  ![Postman 中的 HEAD 请求示例](../images/orderer_head_postman.png "Postman 中的 HEAD 请求示例")

## 使用网络日志
{: #monitor-blockchain-network-using-logs}

“网络监视器”的“概述”屏幕显示认证中心、排序服务和同级的状态。单击**操作**标题下方下拉列表中的**查看日志**以查看特定网络组件的日志。如果使用企业套餐网络，那么可以使用文本文件格式查看组件日志。如果使用入门套餐网络，那么 [{{site.data.keyword.cloud_notm}} Log Analysis 服务](https://cloud.ibm.com/catalog/services/log-analysis){: external}会收集组件日志，并且您可以在 [Kibana](/docs/services/blockchain/howto?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network-viewing-kibana-logs) 中查看这些日志。

每个组件都从不同的活动生成日志。这是因为每个组件在 Hyperledger Fabric [网络体系结构](https://hyperledger-fabric.readthedocs.io/en/release-1.2/network/network.html){: external}和[事务处理流程](https://hyperledger-fabric.readthedocs.io/en/release-1.2/txflow.html){: external}中扮演不同的角色。

- **认证中心日志**  
认证中心管理网络中参与者的身份。在认证中心日志中，您可以找到参与者生成公用和专用密钥以与网络通信（注册）时或者新成员、同级或应用程序向认证中心进行注册时的日志。如果证书验证存在任何问题，那么还可使用 CA 日志来进行调试。

- **排序服务日志**  
  排序服务是区块链网络的常见绑定组件。来自同级、通道更新或网络成员资格更新的所有背书的事务处理建议都将发送到排序服务以进行验证。因此，排序服务中包含从启动网络时开始记录的日志。它还包含因为没有得到正确组织的支持而被拒绝的事务的日志。您还可以找到创建或更新通道时或者通道更新失败时的日志。

- **同级日志**  
同级日志包含安装、实例化和调用链代码的结果。您可以搜索链代码名称和版本以查找特定链代码的日志。您还可以从[通道监视器的链代码部分](/docs/services/blockchain/howto?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network-monitor-channel-cc)查看来自特定链代码的日志。可在同级日志中找到事务建议生成的消息或建议请求的任何超时问题。同级日志还包含因未能满足[链代码支持策略](/docs/services/blockchain/howto?topic=blockchain-install-instantiate-chaincode#install-instantiate-chaincode-endorsement-policy)而被拒绝的事务处理的错误。您还可以找到通道加入请求的结果。

Hyperledger Fabric 根据消息的严重性提供不同的[日志记录级别](https://hyperledger-fabric.readthedocs.io/en/release-1.2/logging-control.html){: external}。{{site.data.keyword.blockchainfull_notm}} Platform 上的缺省日志记录级别为 `INFO`。要查看其他日志，您可以开具[支持凭单](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases)以将日志记录级别设置为更详细的 `DEBUG`。请注意，`DEBUG` 级别日志会显示大量 Gossip 消息，可能需要进行过滤。搜索消息中的 `warning` 或 `error` 以检测来自 Hyperledger Fabric 组件的问题。要检测组件容器是否失败或已被终止，请搜索 {{site.data.keyword.cloud_notm}} 发送的 `panic` 或 `killed` 消息。

## 在入门套餐的 Kibana 中查看日志
{: #monitor-blockchain-network-viewing-kibana-logs}

[{{site.data.keyword.cloud_notm}} Log Analysis 服务](https://cloud.ibm.com/catalog/services/log-analysis){: external}收集入门套餐网络的日志。缺省情况下，会通过 Log Analysis 服务的轻量套餐来收集日志。此套餐免费并且**存储日志 3 天**，然后废弃这些日志。还允许**每天仅搜索日志的前 500 MB**。如果网络日志超过 500 MB，那么无法在 Kibana 中查看新日志。如果网络生成的日志超过 500 MB，或者您想要保留日志超过 3 天，您可以升级到 Log Analysis 服务的付费版本。

在“网络监视器”的“概述”屏幕中，单击**操作**标题下方下拉列表中的**查看日志**，以在 Kibana 界面中打开每个网络组件的日志。在 Kibana 打开时，它按顶部搜索栏的过滤结果显示日志。例如，在单击以查看同级日志时，按网络标识和同级标识过滤搜索：`NETWORK_ID_str:"nf8389d520c243004bb21ff5d70fc8939" && NODE_NAME_str:"org1-peer1"`。如果想要查看更具体的日志，可以在搜索栏中输入其他字段。例如，您可以添加 `&& "marbles"` 以显示来自 `"marbles"` 链代码的日志。删除特定组件术语并仅搜索网络标识（例如，`NETWORK_ID_str:"nf8389d520c243004bb21ff5d70fc8939"`）将显示来自所有网络组件的日志。

您可以使用右上角中的时间范围按钮来更改显示哪个时间段的日志。您还可以使用屏幕左侧的选项卡以向搜索添加和除去搜索中的字段。要显示的最重要字段是消息字段。不带时间戳记搜索消息可能有助于查找此消息日志的所有实例。单击**保存**按钮以保存当前搜索并返回到特定视图。有关在 Kibana 中显示数据的更多信息，请参阅 [Kibana User Guide](https://www.elastic.co/guide/en/kibana/6.2/index.html){: external}。您还可以使用 Log Analysis CLI [下载日志](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-downloading_logs#downloading_logs){: external}至本地文件系统。

**注：**缺省情况下，Kibana 预先配置为显示最近 30 天的活动的日志。如果最近 30 天内没有任何活动，那么您将看到消息：*找不到结果*。要查看其他日志，您可以单击右上角用户名下面的计时器图标，并设置较大的时间范围，例如，*今年至今*。

## 监视通道
{: #monitor-blockchain-network-monitor-channnels}

进入“网络监视器”并在“通道”屏幕中查找要查看和监视的通道。在特定通道屏幕中，您可以在三个选项卡中查看此通道的数据状态信息、成员和已实例化的链代码：

### 通道概述
{: #monitor-blockchain-network-monitor-channel-overview}

“通道概述”选项卡显示有关此通道的区块信息：
  * 一系列数据点，包括已创建的总块数、自上次事务处理以来的时间间隔、链代码实例化数和链代码调用数。
  * 列出此通道上所有块的表。展开区块，您可以查看有关该区块的详细信息。

  ![通道概述](../images/channel_overview_detail.png "通道概述")

### 成员
{: #monitor-blockchain-network-monitor-channel-members}

“成员”选项卡显示此通道上的成员的信息，包括组织操作员的电子邮件地址。

  ![通道成员](../images/channel_members.png "通道成员")

### 链代码 (Chaincode)
{: #monitor-blockchain-network-monitor-channel-cc}

“链代码”选项卡列出在此通道上实例化的所有链代码，其中包括代码标识、版本和运行链代码的同级数。

展开链代码行，以获取有关链代码的详细信息：
  * 您可以单击 **JSON** 以查看链代码的 JSON 文件。
  * 您可以单击**日志**以查看链代码日志。此视图显示已安装链代码的同级的日志，并使用链代码名称和版本进行了过滤。

建议在每个链代码函数后添加唯一成功或错误消息，以帮助监视和调试链代码。如果具有使用多个不同文件的复杂链代码，那么可在链代码日志中添加唯一关键字，这可帮助您查找来自不同事务编译打包的消息。
   * 您可以单击**删除**以除去正在运行的链代码容器。请注意，删除运行中的链代码容器不会实际删除链代码。无法删除区块链网络上已实例化的链代码。

  ![通道链代码](../images/channel_chaincode.png "通道链代码")


## 监视链代码
{: #monitor-blockchain-network-monitor-chaincode}

进入“网络监视器”并打开“安装代码”屏幕。如果具有正在运行的链代码，那么可以在表中看到链代码以及链代码标识和版本。从下拉列表中选择同级，您可以在表中查看此同级的所有链代码。您可以在特定“通道”屏幕的[“链代码”选项卡](/docs/services/blockchain/howto?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network-monitor-channel-cc)上查看链代码的日志。

  ![链代码](../images/installed_cc.png "链代码")

<!----
## Monitoring sample applications
{: #monitor-apps}

In a Starter Plan network, you can view and access sample applications in the "Try Samples" screen of the Network Monitor.  After you deploy a sample application, you can click the **Launch** button to enter your application interface, or the **View on GitHub** link to visit the code repository.  For more information, see [Deploying sample applications](/docs/services/blockchain/prebuilt_samples.html#deploying-sample-applications).

  ![Sample applications](../images/sampleappflow0.png "Sample applications")
--->
