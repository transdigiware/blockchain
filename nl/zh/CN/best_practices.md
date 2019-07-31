---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: best practices, develop applications, connectivity, availability, mutual TLS, CouchDB

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# 应用程序开发的最佳实践
{: #best-practices-app}

本指南适用于已经学习了应用程序开发基础知识并准备好缩放其解决方案的用户。遵循这些最佳实践可最大限度地提高网络性能，并避免应用程序产生停机时间。
{:shortdesc}

## 应用程序连接和可用性
{: #best-practices-app-connectivity-availability}

Hyperledger Fabric [事务处理流程](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}跨多个组件，其中客户机应用程序扮演唯一角色。SDK 会将事务建议提交给同级进行背书。然后，SDK 会收集已背书建议以发送给排序服务，然后将事务区块发送给同级以添加到通道分类帐。生产应用程序的开发者应该做好准备来管理 SDK 与其网络之间的交互，以实现效率和可用性。

### 管理事务
{: #best-practices-app-managing-transactions}

应用程序客户机必须确保验证其事务处理建议，并确保建议成功完成。建议可能会由于多种原因（例如，网络中断或组件故障）而延迟或丢失。对应用程序编码时应实现[高可用性](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-ha-app)，以便处理组件故障。此外，还可以在应用程序中[增大超时值](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-set-timeout-in-sdk)，以防止建议在网络可以响应之前超时。

如果链代码未运行，那么发送到此链代码的第一个事务处理建议将启动链代码。链代码正在启动时，将拒绝其他所有建议，并返回指示链代码当前正在启动的错误。这不同于事务失效。如果在启动链代码期间拒绝了任何建议，那么应用程序客户机需要在链代码启动后重新发送被拒绝的建议。应用程序客户机可以使用消息队列来避免丢失事务建议。

可以使用基于通道的事件服务来监视事务并构建消息队列。[channelEventHub](https://fabric-sdk-node.github.io/ChannelEventHub.html){: external} 类可以根据事务、区块和链代码事件来注册侦听器。来自通道 eventhub 的基于通道的侦听器可以缩放到多个通道，并区分不同通道上的流量。

建议您使用 channelEventHub，而不是旧的 EventHub 类。eventHub 采用单个线程，包含所有通道的事件，可以减缓甚至挂起各通道中的侦听器。eventHub 类也不保证会传递事件，不提供从特定点（例如块号）检索事件以跟踪缺失事件的方式。

**注：**在未来的 Fabric SDK 发行版中，不推荐使用同级 EventHub 类。如果您的现有应用程序使用同级 EventHub，请更新应用程序以改为使用通道 EventHub 类。有关更多信息，请参阅 Node SDK 文档中的 [How to use the channel-based event service](https://fabric-sdk-node.github.io/tutorial-channel-events.html){: external}。

### 打开和关闭网络连接
{: #best-practices-app-connections}

在提交事务建议之前使用 SDK 创建同级和排序节点对象时，您将打开应用程序和网络组件之间的 gRPC 连接。例如，以下命令打开与 `org1-peer1` 的连接。应用程序正在运行时，此连接继续保持活动状态。

```
var peer = fabric_client.newPeer(creds.peers["org1-peer1"].url, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null});
```
{:codeblock}

管理应用程序和网络之间的连接时，您可能考虑以下建议。

- 与网络交互时，请复用同级和排序节点对象，而不是打开新连接来提交事务。复用同级和排序节点对象可以节省资源并提高性能。  
- 要维护与网络组件的持续连接，请使用 [gRPC 保持活动](https://github.com/grpc/grpc/blob/master/doc/keepalive.md){: external}。保持活动让 gRPC 连接保持活动状态，并阻止关闭“未使用的”连接。以下同级连接示例将 gRPC 选项添加到 [ConnectionOpts](https://fabric-sdk-node.github.io/global.html#ConnectionOpts){: external} 对象。gRPC 选项设置为 {{site.data.keyword.blockchainfull_notm}}Platform 建议的值。  
  ```
  var peer = fabric_client.newPeer(creds.peers["org1-peer1"].url, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null},
  "grpcOptions": {
    "grpc.keepalive_time_ms": 120000,
    "grpc.http2.min_time_between_pings_ms": 120000,
    "grpc.keepalive_timeout_ms": 20000,
    "grpc.http2.max_pings_without_data": 0,
    "grpc.keepalive_permit_without_calls": 1
    }
  );
  ```
  {:codeblock}

  您还可以在网络连接概要文件的 `"peers"` 部分中找到带有所建议设置的这些变量。如果通过 SDK 使用连接概要文件连接到网络端点，那么建议的选项将自动导入到应用程序中。您可以在 [Node SDK 文档](https://fabric-sdk-node.github.io/tutorial-network-config.html){: external}中找到有关如何使用连接概要文件的更多信息。

- 不再需要连接时，请使用 `peer.close()` 和 `order.close()` 命令来释放资源并防止性能下降。有关更多信息，请参阅 Node SDK 文档中的 [peer close](https://fabric-sdk-node.github.io/Peer.html#close__anchor){: external} 和 [orderer close](https://fabric-sdk-node.github.io/Orderer.html#close__anchor){: external} 类。如果您已使用连接概要文件将同级和排序节点添加到通道对象，那么可以使用 `channel.close()` 命令来关闭分配给该通道的所有连接。

### 高可用性应用程序
{: #best-practices-app-ha-app}

作为高可用性最佳实践，强烈建议您每个组织至少部署两个同级以实现故障转移。您还需要调整应用程序以获得高可用性。在两个同级上安装链代码，并将这两个同级添加到通道。然后，在设置网络并构建同级目标列表时，请准备好向这两个同级端点提交事务建议。企业套餐网络有多个排序节点以用于故障转移，允许您的客户机应用程序在一个排序节点不可用时将背书的事务发送到其他排序节点。如果改为使用连接概要文件来手动添加网络端点，请确保概要文件是最新的，并且其他同级和排序节点已添加到概要文件的 `channels` 部分中的相关通道。然后，SDK 可以使用连接概要文件添加通道上加入的组件。

## 启用双向 TLS
{: #best-practices-app-mutual-tls}

如果运行的是 Fabric V1.1 级别的企业套餐网络，那么可以选择为应用程序[启用双向 TLS](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-network-preferences)。如果启用双向 TLS，那么需要更新应用程序以支持此功能。否则，应用程序无法与网络通信。

在连接概要文件中，找到 `certificateAuthorities` 部分，您可在其中找到注册以及获取证书以使用双向 TLS 与网络进行通信所需的以下属性。

- `url`：用于连接到可提供双向 TLS 证书的 CA 的 URL
- `enrollId`：用于获取证书的注册标识
- `enrollSecret`：用于获取证书的注册密钥
- `x-tlsCAName`：用于获取将允许应用程序使用双向 TLS 进行通信的证书的 CA 名称。

有关更新应用程序以支持双向 TLS 的更多信息，请参阅 [How to configure mutual TLS](https://fabric-sdk-node.github.io/tutorial-mutual-tls.html){: external}。


## （可选）在 Fabric SDK 中设置超时值
{: #best-practices-app-set-timeout-in-sdk}

Fabric SDK 针对区块链网络中的事件在客户机应用程序中设置缺省超时值。请参阅以下有关 Fabric Java SDK 中缺省超时设置的示例。文件路径为 `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`。

```
    /**
     * Timeout settings
     **/
    public static final String PROPOSAL_WAIT_TIME = "org.hyperledger.fabric.sdk.proposal.wait.time";
    public static final String CHANNEL_CONFIG_WAIT_TIME = "org.hyperledger.fabric.sdk.channelconfig.wait_time";
    public static final String TRANSACTION_CLEANUP_UP_TIMEOUT_WAIT_TIME = "org.hyperledger.fabric.sdk.client.transaction_cleanup_up_timeout_wait_time";
    public static final String ORDERER_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer_retry.wait_time";
    public static final String ORDERER_WAIT_TIME = "org.hyperledger.fabric.sdk.orderer.ordererWaitTimeMilliSecs";
    public static final String PEER_EVENT_REGISTRATION_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.eventRegistration.wait_time";
    public static final String PEER_EVENT_RETRY_WAIT_TIME = "org.hyperledger.fabric.sdk.peer.retry_wait_time";
    public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
    public static final String EVENTHUB_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.eventhub.reconnection_warning_rate";
    public static final String PEER_EVENT_RECONNECTION_WARNING_RATE = "org.hyperledger.fabric.sdk.peer.reconnection_warning_rate";
    public static final String GENESISBLOCK_WAIT_TIME = "org.hyperledger.fabric.sdk.channel.genesisblock_wait_time";

    ...

    // Default values
    /**
     * Timeout settings
     **/
    defaultProperty(PROPOSAL_WAIT_TIME, "20000");
    defaultProperty(CHANNEL_CONFIG_WAIT_TIME, "15000");
    defaultProperty(ORDERER_RETRY_WAIT_TIME, "200");
    defaultProperty(ORDERER_WAIT_TIME, "10000");
    defaultProperty(PEER_EVENT_REGISTRATION_WAIT_TIME, "5000");
    defaultProperty(PEER_EVENT_RETRY_WAIT_TIME, "500");
    defaultProperty(EVENTHUB_CONNECTION_WAIT_TIME, "5000");
    defaultProperty(GENESISBLOCK_WAIT_TIME, "5000");
    /**
     * This will NOT complete any transaction futures time out and must be kept WELL above any expected future timeout
     * for transactions sent to the Orderer. For internal cleanup only.
     */
    defaultProperty(TRANSACTION_CLEANUP_UP_TIMEOUT_WAIT_TIME, "600000"); //10 min.
```
{:codeblock}

但是，您可能需要更改自己应用程序中的缺省超时值。例如，在应用程序调用响应所需时间超过 5000 毫秒（Event Hub 连接的缺省超时值）的事务时，可能会收到失败错误，因为调用事件在 5000 毫秒时结束，而此时事务未完成。您可以设置系统属性以覆盖客户机应用程序的缺省值。因为在设置系统属性前初始化缺省值，所以系统属性可能不会生效。因此，需要在客户机应用程序的静态结构中设置超时系统属性。请参阅以下示例，以了解如何将 Fabric Java SDK 中 Event Hub 连接的超时值更改为 15000 毫秒。文件路径为 `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`。

```
 public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
 private static final long EVENTHUB_CONNECTION_WAIT_TIME_VALUE = 15000;

 static {
     System.setProperty(EVENTHUB_CONNECTION_WAIT_TIME, EVENTHUB_CONNECTION_WAIT_TIME_VALUE);
 }
```
{:codeblock}

如果使用的是 Node SDK，那么可以直接在调用的方法中指定超时值。例如，可以使用下面的一行将[实例化链代码](https://fabric-sdk-node.github.io/Channel.html#sendInstantiateProposal){: external}的超时值增加到 5 分钟。
```
channel.sendInstantiateProposal(request, 300000);
```
{:codeblock}

## 使用 CouchDB 时的最佳实践
{: #best-practices-app-couchdb-indices}

如果将 CouchDB 用作状态数据库，那么可以根据通道的状态数据，从链代码执行 JSON 数据查询。强烈建议您为 JSON 查询创建索引，并在链代码中使用这些索引。索引允许应用程序在网络添加更多全局状态的事务和条目区块时高效地检索数据。

有关 CouchDB 以及如何设置索引的更多信息，请参阅 Hyperledger Fabric 文档中的 [CouchDB 作为状态数据库](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html){: external}。您还可以在 [Fabric CouchDB 教程](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_tutorial.html){: external}中找到将索引与链代码配合使用的示例。

避免对将导致扫描整个 CouchDB 数据库的查询使用链代码。全面的数据库扫描将导致很长的响应时间，并且会降低网络的性能。您可以执行以下某些步骤来避免和管理大型查询：
- 使用链代码设置索引。
- 索引中的所有字段还必须位于查询的选择器或排序部分，才能使用索引。
- 查询越复杂，查询性能越低，使用索引的可能性越小。
- 应该设法避免将导致全表扫描或全索引扫描的运算符，例如 `$or`、`$in` 和 `$regex`。

在 [Fabric CouchDB 教程](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_tutorial.html#use-best-practices-for-queries-and-indexes){: external}中可以找到演示查询如何使用索引以及哪种类型的查询具有最佳性能的示例。

{{site.data.keyword.blockchainfull_notm}} Platform 上的同级设置有 queryLimit，因此将仅从状态数据库返回 10,000 个条目。如果查询达到 queryLimit，您可以使用多个查询来获取剩余的结果。如果需要来自某个范围查询的更多结果，请使用上一个查询返回的最后一个键来启动后续查询。如果需要来自 JSON 查询的更多结果，请使用数据中的其中一个变量对查询进行排序，然后在“大于”过滤器中将上一个查询中的最后一个值用于下一个查询。

不要为了汇总或报告目的而查询整个数据库。如果要构建仪表板或收集大量数据以作为应用程序的一部分，那么可以查询从区块链网络复制数据的链外数据库。这将允许您了解区块链上的数据，而不会降低网络性能或中断事务。

可以使用 Fabric SDK 提供的基于通道的事件服务客户机来构建链外数据存储。例如，您可以使用块侦听器获取添加到通道分类帐的最新事务。然后，可以使用来自有效事务处理的事务读集和写集来更新已存储在单独数据库中的全局状态的副本。有关更多信息，请参阅 Node SDK 文档中的 [How to use the channel-based event service](https://fabric-sdk-node.github.io/tutorial-channel-events.html){: external}。
