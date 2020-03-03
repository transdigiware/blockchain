---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-03"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:term: .term}
{:pre: .pre}
{:external: target="_blank" .external}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components in environments of your choice. You can deploy the {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}. You can also deploy the platform on your own premises or on any Kubernetes v1.14 - v1.16 container platform on x86_64 hardware. You need to decide on the environment and offering that you will use.
{:shortdesc}

Before you use an {{site.data.keyword.blockchainfull_notm}} Platform offering, read the technical and support information in the [Disclaimer](/docs/blockchain?topic=blockchain-disclaimer#disclaimer) section.
{: important}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings to help all types of users get started on their blockchain journey and move their applications into production. You can run the {{site.data.keyword.blockchainfull_notm}} Platform on a Kubernetes Cluster on {{site.data.keyword.cloud_notm}} or on your Kubernetes v1.14 - v1.16 container platform on x86_64.

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.2) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public or hybrid multicloud | An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} |  
| What are my deployment options? | <ul><li> Full platform </li> <li> Only {{site.data.keyword.blockchainfull_notm}} images  ** </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-sw-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-premises and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw-213?topic=blockchain-sw-213-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}

** {{site.data.keyword.blockchainfull_notm}} images - Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 includes an entitlement to images for peer, [Certificate Authority](#x2016383){: term}, [ordering service](#x9826021){: term}, and smart contract containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}}  images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [Using the IBM Blockchain images](/docs/blockchain-sw-213?topic=blockchain-sw-213-blockchain-images).


### Developer Tools

- [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/blockchain?topic=blockchain-develop-vscode#develop-vscode)
  Developers can start with a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly.

### {{site.data.keyword.blockchainfull_notm}} on {{site.data.keyword.cloud_notm}} Private

- [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/blockchain?topic=blockchain-console-icp-about#console-icp-about)
  {{site.data.keyword.blockchainfull_notm}} Platform console deployed on an {{site.data.keyword.cloud_notm}} Private cluster using a Kubernetes Helm chart and APIs for provisioning and managing blockchain components.

## Next steps
{: #get-started-ibp-next-steps}

After you deploy {{site.data.keyword.blockchainfull_notm}} Platform in the environment of your choice, you can set up the network with consortium, nodes, channels, smart contracts, and start transactions on it. For more information, see the task topics under the **HOW TO** section in the left navigation of this documentation.

## Getting support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offers various support options on {{site.data.keyword.IBM_notm}}-implemented blockchain solutions. For more information about {{site.data.keyword.blockchainfull_notm}} Platform support, see [Getting support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support).
