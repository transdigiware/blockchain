---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-07"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components in environments of your choice. You can deploy the {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}. You can also deploy the platform on your own premises or on any x86_64 Kubernetes v1.11 or higher container platform. You need to decide on the environment and offering that you will use.
{:shortdesc}

Before you use an {{site.data.keyword.blockchainfull_notm}} Platform offering, read the technical and support information in the [Disclaimer](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer) section.
{: important}

## Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings to help all types of users get started on their blockchain journey and move their applications into production. You can run the {{site.data.keyword.blockchainfull_notm}} Platform on a Kubernetes Cluster on {{site.data.keyword.cloud_notm}} or on your x86_64 Kubernetes v1.11 or higher container platform.

| | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} | {{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.1) |
|----|---|----|
| Where do you want to deploy the platform?| An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} | Any x86_64 Kubernetes v1.11 or higher container platform -– private, public or hybrid multicloud |
| How is it billed? | [$0.29 USD per allocated CPU hour](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing) | Contact us for pricing |
| Can I connect with Peers in other clouds? | Yes | Yes |
| Can my data center be on-premises and behind a firewall? | No | Yes|
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| Where can I learn more? | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-overview) | See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-offers) |
| How do I get started? |  Go to [Getting started with the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks). | Go to [Getting started with the {{site.data.keyword.blockchainfull_notm}} Platform v2.1.1](/docs/services/blockchain-rhos?topic=blockchain-rhos-get-started-console-ocp). |
*Figure 1. {{site.data.keyword.blockchainfull_notm}} Platform offerings*

### Developer Tools

- [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode)
  Developers can start with a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly.

### {{site.data.keyword.blockchainfull_notm}} on {{site.data.keyword.cloud_notm}} Private

- [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about)
  {{site.data.keyword.blockchainfull_notm}} Platform console deployed on an {{site.data.keyword.cloud_notm}} Private cluster using a Kubernetes Helm chart and APIs for provisioning and managing blockchain components.

## Next steps
{: #get-started-ibp-next-steps}

After you deploy {{site.data.keyword.blockchainfull_notm}} Platform in the environment of your choice, you can set up the network with consortium, nodes, channels, smart contracts, and start transactions on it. For more information, see the task topics under the **HOW TO** section in the left navigation of this documentation.

## Getting support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offers various support options on {{site.data.keyword.IBM_notm}}-implemented blockchain solutions. For more information about {{site.data.keyword.blockchainfull_notm}} Platform support, see [Getting support](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
