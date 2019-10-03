---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-03"

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

{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components in environments of your choice. The environment can be {{site.data.keyword.cloud_notm}}, on-premises through {{site.data.keyword.cloud_notm}} Private, and third-party clouds, such as Amazon Web Services (AWS). In this tutorial, we'll take you through the general process to set up a basic blockchain network with {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

Before you use an {{site.data.keyword.blockchainfull_notm}} Platform offering, read the technical and support information in the [Disclaimer](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer) section.
{: important}

## Before you begin
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings to help all types of users get started on their blockchain journey and move their applications into production. You need to decide on the environment and offering that you will use.

### Which {{site.data.keyword.blockchainfull_notm}} Platform offering is right for your business?
{: #get-started-console-ocp-which-ibp}

You can run the {{site.data.keyword.blockchainfull_notm}} Platform on a Kubernetes Cluster on {{site.data.keyword.cloud_notm}} or on the OpenShift Container Platform.

If you are looking for {{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.0) which is deployed on Red Hat OpenShift,  see [Getting started with IBM Blockchain Platform v2.1.0](/docs/services/blockchain-rhos?topic=blockchain-rhos-get-started-console-ocp)
{: note}

| | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} | {{site.data.keyword.blockchainfull_notm}} Platform for anywhere (v2.1.0) |
|----|---|----|
| Where do you want to deploy the platform?| An {{site.data.keyword.IBM_notm}} Kubernetes Service cluster on {{site.data.keyword.cloud_notm}} | Any x86_64 Red Hat OpenShift Container Platform 3.11 Kubernetes cluster -– private, public or hybrid multicloud |
| How is it billed? | [$0.29 USD per allocated CPU hour](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing) | Contact us for pricing |
| Can I connect with Peers in other clouds? | Yes | Yes |
| Can my data center be on-premises and behind a firewall? | No | Yes|
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes|
| Are APIs available for node management? | Yes | Yes|
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes|
| Where can I learn more? | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-overview) | See [About {{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/services/blockchain-rhos?topic=blockchain-rhos-console-ocp-about#console-ocp-about-offers) |
*Figure 1. {{site.data.keyword.blockchainfull_notm}} Platform offerings*

### Developer Tools

- [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode)
  Developers can start with a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly.

### {{site.data.keyword.blockchainfull_notm}} on {{site.data.keyword.cloud_notm}} Private

- [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about)
  {{site.data.keyword.blockchainfull_notm}} Platform console deployed on an {{site.data.keyword.cloud_notm}} Private cluster using a Kubernetes Helm chart and APIs for provisioning and managing blockchain components.

{: #get-started-ibp-step1}

Ensure that you have the cloud account or PPA license to get an {{site.data.keyword.blockchainfull_notm}} Platform offering.

* **{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**

  This VS Code extension is available for free in the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} and can be used to develop, debug, and test smart contracts for eventual deployment into {{site.data.keyword.blockchainfull_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  This offering is available in the [{{site.data.keyword.cloud_notm}} Catalog dashboard](https://cloud.ibm.com/catalog){: external} of {{site.data.keyword.cloud_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  This offering is delivered as a deployable Helm chart through [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html).

## Step two: Deploy {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Log in to {{site.data.keyword.cloud_notm}} and create a service instance with the offering. Follow the wizard to complete initial configuration for your network. For more information, see [Getting started {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Before you deploy a network, you need to install the Helm chart on an {{site.data.keyword.cloud_notm}} Private cluster. Then, you can deploy the {{site.data.keyword.blockchainfull_notm}} Platform console and use it to deploy and operate blockchain components on your local cluster. For more information, see [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp).

## Next steps
{: #get-started-ibp-next-steps}

After you deploy {{site.data.keyword.blockchainfull_notm}} Platform in the environment of your choice, you can set up the network with consortium, nodes, channels, smart contracts, and start transactions on it. For more information, see the task topics under the **HOW TO** section in the left navigation of this documentation.

## Getting support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offers various support options on {{site.data.keyword.IBM_notm}}-implemented blockchain solutions. For more information about {{site.data.keyword.blockchainfull_notm}} Platform support, see [Getting support](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
