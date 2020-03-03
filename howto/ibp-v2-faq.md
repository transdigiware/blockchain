---

copyright:
  years: 2019, 2020
lastupdated: "2020-03-03"

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
{:support: data-reuse='support'}
{:pre: .pre}


# FAQs
{: #ibp-v2-faq}

**General**   

- [What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [What database do the peers use for their ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [What languages are supported for smart contracts?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Do you support using certificates from non-IBM Certificate Authorities (CAs)?](#ibp-v2-faq-v2-external-certs)
- [I am currently using Hyperledger Fabric v1.4 and want to move to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} or Multicloud. Can I continue to use Raft?](#ibp-v2-faq-migrate-raft)
- [Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?](#ibp-v2-faq-multicloud)
- [How can I find the examples and tutorials within the VSCode extension?](#ibp-v2-faq-vscode-tutorials)
- [Is there a best practice for monitoring my blockchain resources?](#ibp-v2-faq-mon-res)
- [If service discovery is on, will an endorsement request be routed to any peer on the network?](#ibp-v2-faq-service-discovery)
- [Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?](#ibp-v2-faq-raft-tls)


**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [How does pricing work on {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-cloud-pricing)
- [What are the limitations of the free {{site.data.keyword.blockchainfull_notm}} Platform using the  {{site.data.keyword.cloud_notm}} Kubernetes Service free cluster?](#ibp-v2-faq-free-cluster)
- [How can I see the price breakdown for {{site.data.keyword.cloud_notm}} Kubernetes Service, Storage, and Blockchain in my monthly invoice?](#ibp-v2-faq-cloud-invoice)
- [Can I upgrade from V1.0 to the new {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [What happens when I delete my {{site.data.keyword.blockchainfull_notm}} Platform service?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [What regions are available for the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Can I use my existing {{site.data.keyword.cloud_notm}} Kubernetes Service cluster?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Do we have access to logging services and what logs are available to me?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  
- [What persistent file storage does {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} use by default?](#ibp-v2-faq-cloud-storage)

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [What are the benefits of {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [Which environments does {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud support?](#ibp-v2-faq-icp-environments)
- [What is the pricing model for {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-pricing)
- [What services do I need to install to use {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-services)
- [How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud sizing requirements for my dev, test, and production environments?](#ibp-v2-faq-icp-sizing)
- [What happens to my blockchain components when I delete my Helm release?](#ibp-v2-faq-icp-delete)


## What is the value of using {{site.data.keyword.blockchainfull_notm}} Platform over native Hyperledger Fabric?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

Hyperledger Fabric is a powerful, versatile, pluggable, open source, distributed ledger technology capable of addressing a wide variety of use cases across many industries. {{site.data.keyword.blockchainfull_notm}} Platform is built on top of Fabric and includes integrated tools that provide end to end features for developers and network operators to develop, test, operate, monitor, and govern Hyperledger Fabric components by using an intuitive console UI. Quickly deploy an instance and use the streamlined console UI to [build a network](/docs/blockchain?topic=blockchain-ibp-console-build-network), easily [install and instantiate smart contracts](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts), [govern your components](/docs/blockchain?topic=blockchain-ibp-console-govern-components), and [govern your channel](/docs/blockchain?topic=blockchain-ibp-console-govern). Interested in APIs? See the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain){: external}. With the {{site.data.keyword.blockchainfull_notm}} Platform, it is easy to extend a basic network, work with multicloud solutions, and receive {{site.data.keyword.IBM_notm}} worldwide support when needed. Finally, the {{site.data.keyword.blockchainfull_notm}} Platform provides additional security benefits that are essential for running an enterprise-grade production network.



## What version of Hyperledger Fabric is being used with {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}


{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses Hyperledger Fabric v1.4.3.

## What database do the peers use for their ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}
{: support}

You have the choice of either CouchDB or LevelDB when you configure your peer database. Because data is modeled differently in a Couch database than in a Level database, the peers in a channel must all use the same database type. See [LevelDB versus CouchDB](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-components-level-couch) to decide what is best for your business needs.

## What languages are supported for smart contracts?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}
{: support}

The {{site.data.keyword.blockchainfull_notm}} Platform supports smart contracts that are written in  Node.js, Golang, or JavaScript, with future support for Java. The new Hyperledger Fabric programming model currently supports JavaScript, TypeScript, Java, and Go. If you are interested in preserving your existing application code, or by using Fabric SDKs for *Go*, you can still connect to your {{site.data.keyword.blockchainfull_notm}} Platform network by using the lower-level Fabric SDK APIs.

## Do you support using certificates from non-IBM Certificate Authorities?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Yes, you can bring your own certificates if they are issued by a CA that is X.509 compliant. The CA should sign by using ECDSA and the defaults should be set to use P256 curve. See this topic about [Using certificates from an external CA with your peer or ordering node](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-third-party-ca).

## I am currently using Hyperledger Fabric v1.4.x and want to move to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} or Multicloud. Can I continue to use Raft?
{: #ibp-v2-faq-migrate-raft}
{: faq}

Yes. The {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} or Multicloud uses Raft consensus. All of the applications and smart contracts that you are using on Fabric 1.4.x are able to work on your {{site.data.keyword.blockchainfull_notm}} Platform network. However, no mechanism exists to migrate your ledger data from one network to another. Instead, you can reinstall your smart contract packages on your {{site.data.keyword.blockchainfull_notm}} Platform network. See also [Can IBM Blockchain Platform components interoperate with Hyperledger Fabric components on the same network?](/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-v2-faq#ibp-v2-faq-interoperability).

## Is it possible to deploy blockchain nodes to multiple clouds from a single blockchain console?
{: #ibp-v2-faq-multicloud}
{: faq}

You can not currently deploy blockchain nodes to multiple hosted cloud providers. However, you can use your console to operate a distributed multicloud network by importing nodes deployed by using consoles on other clouds.

## How can I find the examples and tutorials within the VSCode extension?
{: #ibp-v2-faq-vscode-tutorials}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform extension provides guided tutorials within VS Code to help you get started. You can find these tutorials on the extension home page when the extension is first installed. However, you can return to the tutorials and the home page by going to the extensions tab. For more information, see [Guided tutorials in VS Code](/docs/blockchain?topic=blockchain-develop-vscode#develop-vscode-guided-tutorials).

## Is there a best practice for monitoring my blockchain resources?
{: #ibp-v2-faq-mon-res}
{: faq}
{: support}

You are responsible for the health monitoring and resource allocation of the blockchain nodes in your Kubernetes cluster. While requests against the nodes are being actively processed, you should be monitoring for spikes in resource consumption to avoid problems.  {{site.data.keyword.IBM_notm}} recommends that you configure Sysdig and setup alerts to track when blockchain nodes are reaching their limits. See the tutorial on [{{site.data.keyword.mon_full_notm}}](/docs/blockchain?topic=blockchain-ibp-sysdig) for more details.

You should be aware that JavaScript and TypeScript smart contracts require more resources than contracts written in Go. Therefore, when you are allocating resources to your peer node, it is important to allocate sufficient resources and monitor the peer containers to ensure adequate resources are available to the peer when smart contracts are instantiated on a channel and during transaction processing.
{: tip}

## If service discovery is on, will an endorsement request be routed to any peer on the network?
{: #ibp-v2-faq-service-discovery}
{: faq}

Yes, if you have the endorsement policy set to “ANY”. However, you do have the opportunity to bind the policy directly to an organization's peers. The service discovery information provided by the peer supplies two pieces of information, `Layouts` and `EndorsersByGroup`. With these two pieces of data, the SDK has the ability to send requests to peers in different organizations that meet the endorsement policy requirements. The node.js SDK provides default code that uses the `Layouts` and `EndoresersByGroup` and sends the requests to the appropriate peers to meet the endorsement policy requirements. This existing logic can be customized to meet the business needs.

## Do ordering service Raft nodes use Transport Layer Security (TLS) for communication?
{: #ibp-v2-faq-raft-tls}
{: faq}

Yes. The Raft ordering service nodes are configured to use TLS communication. TLS is embedded in the trust model of Hyperledger Fabric. By default, server-side TLS is enabled for all communications using TLS certificates. TLS is used to encrypt the communication between your nodes and as well as between your nodes and your applications. TLS prevents man-in-the-middle and session hijacking attacks. All {{site.data.keyword.blockchainfull_notm}} Platform components use TLS to communicate with each other.


## How does pricing work on the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-cloud-pricing)
{: faq}
{: support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} is priced based on the VPCs that you allocate to your blockchain nodes on the {{site.data.keyword.IBM_notm}} Kubernetes Service. For more information, see [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing).

## What are the limitations of the free {{site.data.keyword.blockchainfull_notm}} Platform using the  {{site.data.keyword.cloud_notm}} Kubernetes Service free cluster?
{: #ibp-v2-faq-free-cluster}
{: faq}

* Preview the {{site.data.keyword.blockchainfull_notm}} Platform at no charge for 30 days when you link your {{site.data.keyword.blockchainfull_notm}} Platform service instance to an {{site.data.keyword.cloud_notm}} Kubernetes free cluster.
* Performance will be limited by throughput, storage, and functionality. Read more about the [limitations of free clusters](/docs/containers?topic=containers-cs_ov#cluster_types).
* {{site.data.keyword.cloud_notm}} will delete your Kubernetes cluster after 30 days.
* Only one blockchain console can be connected to a free cluster at a time.
* You cannot migrate any nodes or data from a free cluster to a paid cluster.
* The free offering only supports a single node Raft ordering service.    
* Custom resource allocation is not available on a free cluster.


## How can I see the price breakdown for {{site.data.keyword.cloud_notm}} Kubernetes Service, Storage, and Blockchain in my monthly invoice?
{: #ibp-v2-faq-cloud-invoice}

Actual cost breakdowns are visible from your Invoices in the {{site.data.keyword.cloud_notm}} Dashboard. For detailed steps, see the [Billing](/docs/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-billing) section in the Pricing topic.

## Can I upgrade from V1.0 to the new {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Starter Plan was always designed as a "starting" test network platform and explicitly was not for production. As a result, you cannot upgrade from Starter Plan to the new {{site.data.keyword.blockchainfull_notm}} Platform. Enterprise Plan customers will be able to upgrade to the new {{site.data.keyword.blockchainfull_notm}} Platform in the future. You will receive an email to the account owner from the {{site.data.keyword.blockchainfull_notm}} Platform team to assist.

## What happens when I delete my {{site.data.keyword.blockchainfull_notm}} Platform service?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

When you delete an {{site.data.keyword.blockchainfull_notm}} Platform service instance, all of the blockchain CAs, peers, and ordering nodes are deleted along with their associated storage. If you have exported any of these nodes to other consoles, make sure to reach out to the administrators of those consoles to let them know that those nodes are no longer functioning, because deleting them in your console does not automatically delete them in theirs.

## What regions are available for the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

The available regions for {{site.data.keyword.blockchainfull_notm}} Platform are listed in [{{site.data.keyword.blockchainfull_notm}} Platform locations](/docs/blockchain?topic=blockchain-ibp-regions-locations). Note that you must create an {{site.data.keyword.cloud_notm}} Kubernetes Service cluster in the same region as the blockchain service to recognize the cluster. Additional regions will be available soon.

## Can I use my existing {{site.data.keyword.cloud_notm}} Kubernetes Service cluster?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}
{: support}

Your existing Kubernetes cluster works with the {{site.data.keyword.blockchainfull_notm}} Platform if it satisfies the following conditions:
- It is running Kubernetes version 1.13, 1.14, or 1.15.
- There are enough available resources in the cluster.

## Do we have access to logging services and what logs are available to me?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

With {{site.data.keyword.blockchainfull_notm}} Platform, you can now directly access your peer, CA, and ordering node logs from your Kubernetes dashboard. It is recommend that you take advantage of the {{site.data.keyword.cloud_notm}} LogDNA service that allows you to easily parse the logs in real time.

## What persistent file storage does {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} use by default?
{: #ibp-v2-faq-cloud-storage}
{: faq}

By default {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} uses Classic file storage. You can find more information on the [{{site.data.keyword.cloud_notm}} File storage page](/docs/containers?topic=containers-file_storage#file_storage){: external}. For a complete list of storage options, see [Persistent storage considerations](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage)

## What are the benefits of {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

See [What the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud offers](/docs/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## Which environments does {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud support?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud supports all environments that {{site.data.keyword.cloud_notm}} Private v3.2 supports. See [Supported operating systems and platforms](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external} for more information.

## What is the pricing model for {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-pricing}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [licensing](/docs/blockchain?topic=blockchain-ibp-software-pricing) is based on the amount of CPU (VPC) made available to the product. For details, [contact IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## What services do I need to install to use {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-services}
{: faq}

You need to install only {{site.data.keyword.cloud_notm}} Private v3.2.

## How can I estimate the {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud sizing requirements for my dev, test, and production environments?
{: #ibp-v2-faq-icp-sizing}
{: faq}

After you understand how many CAs, peers, and ordering nodes are required, you can examine the [default resource allocations table](/docs/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) to get an approximate estimate of the CPUs (VPCs) required for your network.

## What happens to my blockchain components when I delete my Helm release?
{: #ibp-v2-faq-icp-delete}
{: faq}

When you delete a helm release from your {{site.data.keyword.cloud_notm}} Private cluster, the associated blockchain components are not deleted. To properly remove a helm release from your cluster, delete all of your components by using the blockchain console or the blockchain APIs first. Then, you can delete the helm chart.


