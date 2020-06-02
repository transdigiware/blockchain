---

copyright:
  years: 2017, 2020

lastupdated: "2019-09-24"

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

# Known issues
{: #known-issues}

This page describes known issues that you might encounter when you use Enterprise Plan.
{:shortdesc}

The following issues are already reported:
- **Configuring an external CA in not supported on Enterprise Plan**. As an alternative, you can generate and upload admin certificates via the Network Monitor. For more information, see the description on the ["Certificates" tab of "Member" screen](/docs/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) in the Network Monitor.
- **Chaincode containers can sometimes be stopped** by a background network issue and might need to be rebuilt and restarted after a user invokes the chaincode. If this happens it may take your chaincode a few minutes to respond.

For support and help with your {{site.data.keyword.blockchainfull_notm}} Platform network on {{site.data.keyword.cloud_notm}}, see [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).

## Enterprise Plan upgrade

Networks on Enterprise Plan will soon be upgraded to Fabric v1.4.3. This will allow your network to be migrated to a RAFT based ordering service from Kafka.

This upgrade could cause breaking changes in your applications if they still use the peer based EventHub class of the Fabric SDKs. The peer based EventHub has been removed in Fabric v1.4.3. You must update your applications to use the new channel based event service. Because you can use channel events on Fabric v1.1, you can migrate your application at any time. For more information, see [How to use the channel-based event service](https://hyperledger.github.io/fabric-sdk-node/release-1.4/tutorial-channel-events.html){: external} in the Node SDK documentation.

Fabric v1.4 is the first long term support release of Hyperledger Fabric. You can learn more by visiting the [Whats new in v1.4](https://hyperledger-fabric.readthedocs.io/en/release-1.4/whatsnew.html){: external} in the Fabric documentation.
