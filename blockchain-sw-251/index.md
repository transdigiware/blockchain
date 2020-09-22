---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: Kubernetes, IBM Cloud Private, OCP, OpenShift Container Platform, IBM Blockchain Platform, multicloud

subcollection: blockchain-sw-251

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Getting started with {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1
{: #get-started-console-ocp}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-get-started-console-ocp">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-get-started-console-ocp">2.1.3</a>
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-get-started-console-ocp">2.5</a>
    </p>
</div>


{{site.data.keyword.blockchainfull}} Platform provides a managed and full stack blockchain-as-a-service (BaaS) offering that allows you to deploy blockchain components on many platforms including open source Kubernetes, distributions such as Rancher, OpenShift Container Platform. For details of what is supported, see [Supported Platforms](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-prerequisites). This offering is ideal for the customers who want to deploy their components, store their data, or run their workloads on their own infrastructure or across public and private clouds for security, risk mitigation, preference, or compliance reasons. Clients can build, operate, and grow their blockchain networks with an offering that can be used from development through production.
{:shortdesc}

Your entitlement includes the flexible management console for deploying and managing your blockchain network. Use the console or {{site.data.keyword.blockchainfull_notm}} Platform APIs to build a consortium of organizations to easily transact on the same network, regardless of each client's cloud preference. By installing the {{site.data.keyword.blockchainfull_notm}} Platform console, users can create components in their clusters and connect them to components deployed on other clusters, forming a distributed, multi-organizational blockchain network.

Experienced Hyperledger Fabric customers who prefer to deploy and manage their containers can download and use the peer, CA, orderer, and smart contract container images without the management console.

The **{{site.data.keyword.blockchainfull_notm}} Platform 2.5.1** is purchased as a stand-alone entitlement. After purchase, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. This key is required to deploy the release or download the container images unless you are deploying from Red Hat Marketplace.

With this offering, you are responsible for purchasing and provisioning your own Kubernetes cluster. Additionally, you will need to [open ports in your firewall](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-security#ibp-security-ibp-ports) if you are using one.
{: important}

## Are you a Red Hat Marketplace customer?
{: #get-started-console-ocp-rhm}

Already using Red Hat Marketplace and ready to try out blockchain? Check out these [instructions](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp-rhm) to get started.

## Already have an {{site.data.keyword.blockchainfull_notm}} Platform v2.1.x network and want to upgrade?
{: #get-started-console-ocp-upgrade}

Check out the following topics for instructions on how to upgrade:
- [Upgrading your console and components on the OpenShift Container Platform](/docs/blockchain-sw-251?topic=blockchain-sw-251-upgrade-ocp)
- [Upgrading your console and components on Kubernetes](/docs/blockchain-sw-251?topic=blockchain-sw-251-upgrade-k8)
- [Upgrading the {{site.data.keyword.blockchainfull_notm}} images](/docs/blockchain-sw-251?topic=blockchain-sw-251-blockchain-images#blockchain-images-upgrade)


## Is {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 suitable for you?
{: #get-started-console-ocp-suitable}

{{site.data.keyword.blockchainfull_notm}} Platform provides different offerings that allow you to deploy your network in the environment of your choice. You need to decide if you want to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 or if you want to use the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

| |{{site.data.keyword.blockchainfull_notm}} Platform for anywhere (2.5.1) | {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} |
|----|---|----|
| Where do you want to deploy the platform?|  Multiple Kubernetes distributions on a private, public or hybrid **multicloud** <br><br> See [Supported Platforms](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-prerequisites) | A Kubernetes cluster on {{site.data.keyword.cloud_notm}} <br><br> See [Supported configuration](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-supported-cfg) |  
| What are my deployment options? | <ul><li> Full platform </li> <li> Only {{site.data.keyword.blockchainfull_notm}} images ** </li> </ul>| <ul><li> Full platform </li> </ul>
| How is it billed? |Contact us for [pricing](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-sw-pricing) |[$0.29 USD per allocated CPU hour](/docs/blockchain?topic=blockchain-ibp-saas-pricing)  |
| Can I connect with Peers in other clouds? |  Yes| Yes |
| Can my data center be on-prem and behind a firewall? | Yes| No |
| Can I use a console UI to deploy and manage my blockchain components? | Yes | Yes |
| Are APIs available for node management? | Yes | Yes |
| Does the product integrate with the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extension to develop and test my smart contracts?| Yes | Yes |
| I am an experienced Fabric customer. Are peer, CA, orderer, and smart contract images available and supported? | Yes | No |
| Where can I learn more? |See [About {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-offers)  | See [About {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-overview#ibp-console-overview-capabilities) |
| Ready to get started? | See [Step one: Install the IBM Blockchain Platform](/docs/blockchain-sw-251?topic=blockchain-sw-251-get-started-console-ocp#get-started-console-ocp-step-two-deploy-console) | See [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks) |
{: caption="Table 1. Which offering is right for your business?" caption-side="bottom"}
** {{site.data.keyword.blockchainfull_notm}} images - Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 includes an entitlement to images for peer, CA, ordering service, and smart contract containers that are signed by IBM. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}} images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [Using the IBM Blockchain images](/docs/blockchain-sw-251?topic=blockchain-sw-251-blockchain-images).  


Have questions and want to speak to an {{site.data.keyword.blockchainfull_notm}} Platform expert? [Schedule a consult](https://www.ibm.com/cloud/blockchain-platform/developer?schedulerform){: external} now to learn more about how blockchain can transform your business.

### Developer Tools
{: #get-started-console-ocp-dev-tools}

Are you a developer? Check out the [**{{site.data.keyword.blockchainfull_notm}} Platform Developer Tools**](/docs/blockchain-sw-251?topic=blockchain-sw-251-develop-vscode#develop-vscode), a free IDE that provides an explorer and commands accessible from the command palette for developing smart contracts quickly. Install it locally or run it from the cloud using Red Hat CodeReady Workspaces.

### {{site.data.keyword.blockchainfull_notm}} images
{: #get-started-console-ocp-images}

- Your purchase of {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 includes an entitlement to images for [peer](#x2033450){: term}, [Certificate Authority](#x2016383){: term}, [ordering service](#x9826021){: term}, and [smart contract](#x8888420){: term} containers that are signed by {{site.data.keyword.IBM_notm}}. The images are based on the open source Hyperledger Fabric code base and contain a number of enhancements for stability and serviceability. The images are bundled with support from IBM. The {{site.data.keyword.blockchainfull_notm}} images do not include the {{site.data.keyword.blockchainfull_notm}} management console or operator. This offering is intended for experienced Fabric users with existing Fabric deployments. For more information, see [{{site.data.keyword.blockchainfull_notm}} images for Hyperledger  Fabric](/docs/blockchain-sw-251?topic=blockchain-sw-251-blockchain-images#blockchain-images).

## Before you begin
{: #get-started-console-ocp-set-up-ocp}

1. Review the [Supported Platforms](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-prerequisites).

2. Install a Kubernetes cluster and log in to your cluster. If you are using OpenShift, see the [OpenShift Container Platform CLI](https://docs.openshift.com/container-platform/4.3/cli_reference/openshift_cli/getting-started-cli.html){: external} to deploy the CLI. If you are using an OpenShift cluster that was deployed with the {{site.data.keyword.IBM_notm}} Kubernetes Service, use these instructions to [Install the OpenShift Origin CLI](/docs/openshift?topic=openshift-openshift-cli#cli_oc).

3. If you are _not_ running the platform on Red Hat OpenShift Container Platform or Red Hat Open Kubernetes Distribution, you need to set up the NGINX Ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

4. If you are not deploying from Red Hat Marketplace, get the entitlement key from your MyIBM account in order to install the platform. For more information, see [Get your entitlement key](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp#deploy-ocp-entitlement-key).

5. Review the persistent storage considerations for [OpenShift](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp-getting-started#deploy-ocp-storage) or [Kubernetes](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8#deploy-k8-storage) depending on your platform.

## Step one: Install the {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-ocp-step-two-deploy-console}

The {{site.data.keyword.blockchainfull_notm}} Platform uses a [Kubernetes Operator](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform console on your cluster and manage the deployment and your blockchain nodes. A cluster administrator can use your entitlement key to deploy the platform on any Kubernetes Cluster

-  If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on the OpenShift Container Platform, you can deploy the platform by using the steps in [Deploying {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 on the OpenShift Container Platform](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp). Or, if you prefer to deploy from the Red Hat Marketplace see [Deploying {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 from Red Hat Marketplace](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp-rhm).

- If you are deploying the {{site.data.keyword.blockchainfull_notm}} Platform on open source Kubernetes or a distribution such as Rancher, use the steps described in the [Deploying {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 on Kubernetes](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8).

- If you are an experienced Hyperledger Fabric customer and prefer to only use the {{site.data.keyword.blockchainfull_notm}} images, see [Using the {{site.data.keyword.blockchainfull_notm}} images](/docs/blockchain-sw-251?topic=blockchain-sw-251-blockchain-images). Consult the Fabric documentation for further deployment, configuration, and management instructions. The rest of the steps in this topic do not apply when using the images.

## Step two: Grant console access to other users
{: #get-started-console-ocp-step-four-add-console-admin}

After the platform is deployed, the console administrator can log in to the console with the email address and password that was provided during console deployment. The password that was provided becomes the default password of the console, and is used by all new users to log in to the console for the first time. The administrator can then add new users to the console, allowing others to log in and start working with {{site.data.keyword.blockchainfull_notm}} nodes. The administrator can also set a new default password. To learn more, see [Managing users from the console](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-icp-manage#console-icp-manage-users).

## Step three: Use the console to create your components
{: #get-started-console-ocp-build-network}

After you deploy the console, you can use it to create, operate, and govern {{site.data.keyword.blockchainfull_notm}} components on your Kubernetes cluster. To get started with building a blockchain network by using the management console, go to the [Build a network tutorial](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-console-build-network#ibp-console-build-network).

## Step four: Connect networks across clouds
{: #get-started-console-ocp-import-nodes}

You can use your console to operate components that are running on other clusters. First, you need to export the component information to a JSON file from the console where the component was originally deployed. Then, you can import the node JSON file into the console that is deployed on your local cluster and manage the components across clouds. For more information, see [Importing nodes](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-console-import-nodes#ibp-console-import-nodes).
