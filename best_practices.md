---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-17"

keywords: best practices, develop applications, connectivity, availability, mutual TLS, CouchDB

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:javascript: data-hd-programlang="javascript"}
{:java: data-hd-programlang="java"}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}
{:pre: .pre}


# Best practices for application development
{: #best-practices-app}
{: help}
{: support}



This guide is for users who understand the basics of application development and are ready to scale their solution. Follow these best practices to maximize the performance of your network, and avoid application downtime.
{:shortdesc}

For information about how to migrate your applications built using the Fabric v1.4 SDK to the v2.x SDK, check out [Migrating client applications from v1.4 to v2.0](https://hyperledger.github.io/fabric-sdk-node/release-2.2/tutorial-migration.html){: external}. The v1.4 SDK provides both the `fabric-network` and `fabric-client` APIs for developing client applications that interact with smart contracts deployed to a Hyperledger Fabric blockchain. The `fabric-network` implements the Fabric programming model, which provides consistency across programming languages, and is the preferred API. The `fabric-client` API is a lower-level, legacy API that is significantly more complex to use. Starting with v2.1, `fabric-network` is the only recommended API for developing client applications.
{: important}

## Application connectivity and availability
{: #best-practices-app-connectivity-availability}

The Hyperledger Fabric [Transaction Flow](https://hyperledger-fabric.readthedocs.io/en/release-2.2/txflow.html){: external} spans multiple components, with the client applications playing a unique role. The SDK submits transaction proposals to the peers for endorsement. It then collects the endorsed proposals to be sent to the ordering service, which then sends blocks of transactions to the peers to be added to channel ledgers. Developers of production applications should be prepared to manage their interactions between the SDK and their networks for efficiency and availability.

### Managing transactions
{: #best-practices-app-managing-transactions}

Application clients must ensure that their transaction proposals are validated and that the proposals complete successfully. A proposal can be delayed or lost for multiple reasons, such as a network outage or a component failure. You should code your application for [high availability](/docs/blockchain?topic=blockchain-ibp-console-app#console-app-ha) to handle component failure. You can also [increase the timeout values](/docs/blockchain?topic=blockchain-best-practices-app#best-practices-app-set-timeout-in-sdk) in your application to prevent proposals from timing out before the network can respond.

If a smart contract is not running, the first transaction proposal that is sent to the smart contract starts the smart contract. While the smart contract is starting, all other proposals are rejected with an error that indicates that the smart contract is starting. This is different from transaction invalidation. If any proposal is rejected while the smart contract is starting, application clients need to resend the rejected proposals after the smart contract starts. Application clients can use a message queue to avoid losing transaction proposals.

You can use a channel-based event service to monitor transactions and build message queues. The [Event Service](https://hyperledger.github.io/fabric-sdk-node/release-2.2/EventService.html){: external} class allows the user to register a listener to be notified when a new block is added to the ledger, when a new block is added that has a specific transaction ID, or to be notified when a transaction contains a chaincode event name of interest.

It is recommended that you use the channelEventHub rather than the old EventHub class. EventHub is single threaded and contains events from all channels that might slow down or even hang listeners across channels. The eventHub class also provides no guarantee that an event will be delivered, and provides no way of retrieving events from a certain point, such as a block number, to track events that were missed.

### Opening and closing network connections
{: #best-practices-app-connections}

When you create peer and orderer objects with the SDK before submitting transaction proposals, you are opening a gRPC connection between your application and the network component. For example, the following command opens a connection to `org1-peer1`. This connection continues to be active while your application is running.

```javascript
var peer = fabric_client.newPeer(creds.peers["org1-peer1"].url, { pem: creds.peers["org1-peer1"].tlsCACerts.pem , 'ssl-target-name-override': null});
```
{:codeblock}

When you manage the connections between your application and your network, you might consider the following recommendations.

- Reuse peer and orderer objects when you interact with your network, instead of opening new connections to submit transactions. Reusing peer and orderer objects can save resources and lead to better performance.
- To maintain a persistent connection to your network components, use [gRPC keepalives](https://github.com/grpc/grpc/blob/master/doc/keepalive.md){: external}. Keepalives keep the gRPC connection active and prevent an "unused" connection from being closed. Check out [How to set gRPC settings](https://hyperledger.github.io/fabric-sdk-node/release-2.2/tutorial-grpc-settings.html){: external} to learn the different ways of setting the gRPC settings used on connections to the Hyperledger Fabric network with a Hyperledger Fabric Node.js Client. The gRPC options are set to values that {{site.data.keyword.blockchainfull_notm}} Platform recommends.  

  ```javascript
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

  You can also find these variables with the recommended settings in the `"peers"` section of your network connection profile. The recommended options are imported into your application automatically if you use the connection profile with the SDK to connect to your network endpoints. You can find more information on how to use a Connection Profile in the [Node SDK documentation](https://hyperledger.github.io/fabric-sdk-node/release-2.2/tutorial-commonconnectionprofile.html){: external}. The [Fabric `Gateway` class](https://hyperledger.github.io/fabric-sdk-node/release-2.2/module-fabric-network.Gateway.html){: external} also provides a connection point for an application to access the Fabric network.

## (Optional) Setting timeout values in Fabric SDKs
{: #best-practices-app-set-timeout-in-sdk}

Fabric SDKs set default timeout values in client applications for events in the blockchain network. See the following example about default timeout settings in Fabric Java SDK. The file path is `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`.

```java
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

However, you might need to change the default timeout values in your own application. For example, when your application invokes a transaction that needs more than 5000 ms, which is the default timeout value for event hub connection to respond, you might get a failing error because the invoke event ends at 5000 ms before the transaction completes. You can set the system property to overwrite the default values from your client application. Because the default values are initialized before you set the system property, the system property might not take effect. Therefore, you need to set the system property for timeout in a static construct in your client application. See the following example on changing timeout value for event hub connection to 15000 ms in Fabric Java SDK. The file path is `src\main\java\org\hyperledger\fabric\sdk\helper\Config.java`.

```java
 public static final String EVENTHUB_CONNECTION_WAIT_TIME = "org.hyperledger.fabric.sdk.eventhub_connection.wait_time";
 private static final long EVENTHUB_CONNECTION_WAIT_TIME_VALUE = 15000;

 static {
     System.setProperty(EVENTHUB_CONNECTION_WAIT_TIME, EVENTHUB_CONNECTION_WAIT_TIME_VALUE);
 }
```
{:codeblock}

If you are using the Node SDK, you can specify the timeout values directly in the method called. As an example, you can use the following line to increase the timeout value for [instantiating a chaincode created using the v1.4 lifecycle](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Channel.html#sendInstantiateProposal){: external} to 5 minutes.
```javascript
channel.sendInstantiateProposal(request, 300000);
```
{:codeblock}

For the v.2x lifecycle, check out the documentation on how to [propose](https://hyperledger.github.io/fabric-sdk-node/release-2.2/Proposal.html){: external}, [endorse](https://hyperledger.github.io/fabric-sdk-node/release-2.2/Endorser.html){: external}, and [commit](https://hyperledger.github.io/fabric-sdk-node/release-2.2/Commit.html){: external} a chaincode.


## Resources
{: #best-practices-resources}

[{{site.data.keyword.IBM_notm}} Developer](https://developer.ibm.com/technologies/blockchain/)   
- For tutorials, code patterns, and videos that help developers get started and learn best practices for developing blockchain applications.

[Blockchain Design patterns](https://developer.ibm.com/technologies/blockchain/articles/getting-started-with-blockchain-design-patterns)  
- For application developers who want to learn about common patterns for interacting with blockchain networks.
