---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-26"


keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:term: .term}
{:pre: .pre}
{:external: target="_blank" .external}

# Release notes
{: #release-notes-saas-20}

Use these release notes that are grouped by date to learn about the latest changes to {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} which is built on Hyperledger Fabric v1.4.3.
{:shortdesc}

See [Installing patches](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) for instructions on how to apply patches to your existing nodes.

## 24 March 2020
{: #03-24-2020}





{{site.data.keyword.IBM_notm}} is in the process of migrating existing {{site.data.keyword.blockchainfull_notm}} Platform consoles to v2.1.3, therefore, the new features described in this list may not yet be available in your console. Unsure what version you are currently using? Click the question mark icon in the upper right corner of the console. The {{site.data.keyword.blockchainfull_notm}} Platform version is visible under the page heading. Existing customers will receive a Cloud notification with more details about when their console will be migrated.
{: note}




**Hyperledger Fabric v1.4.6**

All new nodes are deployed using Hyperledger Fabric v1.4.6. If you have an existing blockchain network, you should review the topic on [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities) to understand how this new Fabric version can impact your network.

**Hardware Security Module (HSM) support for node identities**   

Full cryptographic HSM support is now available for HSMs that implement the PKCS #11 standard. Using an HSM provides on-demand encryption, key management, and key storage. When you deploy a CA, peer, or ordering node, you now have the option to store the private key for the node identity in an HSM. See [Configuring a node to use a HSM](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-cfg-hsm) for more details.

**Support for adding and removing ordering nodes from an existing ordering service**  

Previously, an ordering service could only contain one or five ordering nodes and they all were contributed from the same organization. Now, the ordering service can be deployed across multiple organizations in a blockchain network, enabling individual organizations to add and remove individual ordering nodes as required. Multi-organizational transaction ordering improves the decentralized nature of a blockchain network.  Learn more about the process in the new [Adding and removing Raft ordering service nodes tutorial](/docs/blockchain?topic=blockchain-ibp-console-add-remove-orderer).

**Ability to override default CA, peer, ordering node configuration**  

Hyperledger Fabric includes many configuration options for a CA, peer, or ordering node. A subset of those options for the [CA](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-ca-customization), [peer](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-peer-create-json) and [ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-orderer-create-json) can now be overridden by using the console or the [APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis#ibp-v2-apis-custom).

**Full Java smart contract development support**

In addition to smart contracts written in JavaScript, TypeScript, and Go programming languages, it is now possible to install and instantiate Java smart contracts from the console. Moreover, Java can be freely mixed and matched with other application and smart contract programming languages, including JavaScript, TypeScript, and Go. This heterogeneous programming language support enables an organization to capitalize on the full range of its development skills.

In order to use Java chaincode, developers should be aware that:

- Java 11 is required to execute Java smart contracts.
- Gradle v4.x and Maven v3.x are used to build Java smart contracts.
- Custom Gradle versions can be used by using a Gradle wrapper.
- Java smart contracts require the fabric-chaincode-shim at v1.4.6 or later, as this version is the first version that includes support for Java 11.
- For an example of a Java smart contract, see the [FabCar Java smart contract](https://github.com/hyperledger/fabric-samples/tree/release-1.4/chaincode/fabcar/java){: external} from Fabric v1.4.

**v2 APIs available**

New {{site.data.keyword.blockchainfull_notm}} Platform console APIs using the route `/v2/` are now available. Use of the earlier `/v1/` APIs continues to be supported. See [{{site.data.keyword.blockchainfull_notm}} Platform APIs](https://cloud.ibm.com/apidocs/blockchain) for more information.


**High Availability Certificate Authority**

When a CA is deployed, replica sets can be configured to ensure High Availability of the node. Note that CA replica sets require a PostgreSQL database. Learn more in the [Building a high availability Certificate Authority (CA)](/docs/blockchain?topic=blockchain-ibp-console-build-ha-ca) tutorial.

## 6 Feburary 2020
{: #02-06-2020}

**Update default resource allocation**

The default _memory_ allocation for the peer container has been increased from 0.4GB to 1GB. When deploying a new peer, the peer container will now default to 1GB memory, but you can adjust this value according to your use case. Increasing the memory alleviates some resource contention that could occur during smart contract instantiation.     

Existing peer containers are not impacted.

**Peer and ordering node patch 1.4.3-1**

The Ingress timeout for the gRPC web proxy has been increased to avoid an error during smart contract instantiation. It is recommended that you apply this patch to your existing peer and ordering nodes.

## 11 December 2019
{: #12-11-2019}

**Miscellaneous bug fixes**

## 15 November 2019
{: #11-15-2019}

**Service availability in two new regions**  

The {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} is now available to be provisioned in two new {{site.data.keyword.cloud_notm}} regions: **US East**, and **AP South**. Refer to the [locations](/docs/blockchain?topic=blockchain-ibp-regions-locations) information to see the data centers that are available in those regions.

## 06 November 2019
{: #11-06-2019}

**Simplified component creation flows**  

Peer, CA and ordering service creation has been streamlined. Now, when you create a node, you have to select a checkbox to configure advanced deployment options.

## 02 October 2019
{: #10-02-2019}

**Peer, CA, and ordering node patch 1.4.3-0**

**Zone selection for ordering nodes**

When multiple zones are configured in your Kubernetes cluster, you can now choose which zone to deploy your ordering node to. Spreading nodes across zones is useful for ensuring high availability of your blockchain network.

**Ability to set peer to be an anchor peer when the peer joins the channel**

Users can now choose to designate a peer to be an anchor peer when the peer is being joined to the channel, rather than after the peer has joined the channel. An anchor peer must exist for each organization in order for cross organizational gossip to work. [Anchor peers](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) are also required for private data and service discovery to work. [Learn how](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-join-peer).

**Ability to add a peer to a channel from the Channels tab**

A peer can now be joined to a channel directly from the Channels tab. See [Join a peer to a channel](docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-join-peer) for more details.

**Export/Import all**

Allows for the bulk export and import of your blockchain network, including nodes, MSPs and identities, rather than having to export and import these components one at a time. See the topic on [Exporting and importing in bulk](/docs/blockchain?topic=blockchain-ibp-console-import-nodes#ibp-console-import-bulk-export-import) for more details.

**Hyperledger Fabric v1.4.3**

All new nodes are deployed using Hyperledger Fabric v1.4.3. If you have an existing blockchain network, you should review the topic on [Capabilities](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-capabilities) to understand how this new Fabric version can impact your network.

## 21 August 2019
{: #08-21-2019}

**Peer node patch `1.4.1-3`**

With this patch, you now have the ability to upload additional admin identities for a peer. See [Adding new peer admin certificates](/docs/blockchain?topic=blockchain-ibp-console-organizations#ibp-console-organizations-new-admins) for more information.

**Select peer zone** ({{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} only)  

If multiple zones are configured in your {{site.data.keyword.cloud_notm}} Kubernetes cluster, you can now use the console UI to select the zone where the peer is deployed. This capability is important if you are configuring [multizone HA for peers](/docs/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-multi-zone).

**Peer database support now includes LevelDB**

You now have the option to choose between CouchDB and LevelDB for your Peer database. For more information, see [LevelDB vs CouchDB](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-level-couch).

**Certificate Authority (CA) updates**   

The console UI no longer stores the admin enroll id and secret that you provide when you create a CA. Therefore, after you create a CA, you are responsible for generating the admin certificates by using the new **Associate identity** and passing in the enroll id and secret. This change improves CA security. See [Associating the identity of the CA admin](/docs/blockchain?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-identity) for more details.

## 12 August 2019
{: #08-12-2019}

**Beta trial of the {{site.data.keyword.blockchainfull_notm}} Platform ends**

The Beta trial of the {{site.data.keyword.blockchainfull_notm}} Platform, which began on February 12, 2019, has officially ended. All of the beta instances of your console and the components that you deployed into your Kubernetes cluster using the beta console will be deleted.

**Peer node patch `1.4.1-2`**

This patch contains a security fix for the peer couchDB container.

## 10 July 2019
{: #07-10-2019}

**Ordering node patch `1.4.1-2`**  

This patch fixes a problem that occurs when ordering nodes restart. The [genesis block](#x9076628){: term} is updated which prevents the ordering node from communicating with the other nodes in the ordering service. You can apply this patch to all existing ordering nodes in your ordering services by following these [instructions](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) to update each ordering node. All new ordering nodes that you create will automatically include this fix.

**Anchor peer removal**  

You now have the ability to remove anchor peers from a channel.

**Miscellaneous bug fixes**  

Bug fixes and translation updates.

## 24 May 2019
{: #05-24-2019}

**Raft consensus protocol**  

The five node Raft ordering service, recommended for production networks, is now available. In addition, for development and testing purposes, you can deploy a single node Raft ordering service.

## 9 May 2019
{: #05-09-2019}

**Introducing {{site.data.keyword.blockchainfull_notm}} Platform Console APIs**

APIs are now available to provision, edit, and delete peer, orderer, and CA nodes, making it possible to script the building of your blockchain network. Use the documentation in the [{{site.data.keyword.cloud_notm}} API documentation repository](/apidocs/blockchain#introduction){: external} to learn more about the APIs and try them out. In addition, see the topic on [Building a network with APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis) for instructions on how to use the APIs to build out your network.  

**Channel governance**  

Channel governance updates allow policies to be reconfigured. You can also control which channel members need to sign a channel update and you can orchestrate signature collection.

## 17 April 2019
{: #04-17-2019}

**Ability to size and scale node resources**  

When you deploy a node you now have the ability to specify the amount of CPU, memory, and storage to your containers, where applicable. You can later scale their resources up or down at a later time according to usage patterns. For more information, see [Allocating resources](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-allocate-resources).

**Use of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

IAM is used to control user access to the console UI as well as restricting the actions they can perform in the UI.  See this topic for information on how to [add and remove users from the console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).

## 3 April 2019
{: #04-03-2019}

**Support for external CA**

When you add a peer or orderer node, you have the option to use certificates from an external CA, one that is not hosted by {{site.data.keyword.IBM_notm}}.  See this topic on [Using certificates from an external CA with your peer or ordering node](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-third-party-ca) for more information.

**Tuning orderer performance**

New orderer tuning parameters are available in the console to give you more control over your orderer throughput and performance. See this topic on [Tuning your orderer](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning) for instructions on how to configure the parameters.
