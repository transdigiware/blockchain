---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-12"

keywords: pricing model, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

subcollection: blockchain-sw-251

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
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
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Pricing
{: #ibp-sw-pricing}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-ibp-sw-pricing">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-sw-pricing">2.1.3</a>,
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-sw-pricing">2.5</a>
    </p>
</div>


This guide helps you understand the pricing model for {{site.data.keyword.blockchainfull}} Platform 2.5.1, and how much you will pay when you develop and grow your blockchain network of Certificate Authorities, peers, and ordering service components, that are based on Hyperledger Fabric v1.4.9 and v2.2.1.
{:shortdesc}

## License
{: #ibp-software-pricing-license}

{{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 delivers the components that are needed to run a blockchain network on your own infrastructure using Kubernetes. You can access your [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering which is required in order to deploy the release. With this offering, you are responsible for procuring and provisioning your own Kubernetes cluster on one of the [Supported Platforms](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-prerequisites). Technical support from IBM Blockchain is included in the offering.

## Pricing
{: #ibp-software-pricing-pricing}

The pricing of {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 is based on the number of Virtual Processor Cores (VPCs) used. A VPC, or vCPU, can be either a virtual core that is assigned to a virtual server, or a physical processor core in a non-partitioned server. You must obtain a licensed entitlement for each VPC made available to the {{site.data.keyword.blockchainfull_notm}} Platform. The number of vCPU or CPUs that are required can vary depending on your infrastructure, network design and performance requirements. You are only charged for the VPCs used by your Certificate Authority (CA), peer, and ordering nodes. Specifically, for the peer, you are only charged for the VPCs used by the peer container itself. You are not charged for the VPCs used by the CouchDB, chaincode launcher (and associated chaincode pod), and gRPC proxy containers. Similarly, for an ordering service node, you are not charged for the gRPC proxy container.

If you are using the Red Hat OpenShift Container Platform, you can read more about  VPCs (CPUs) in that platform, see  [Allocating Node Resources](https://docs.openshift.com/container-platform/4.4/nodes/nodes/nodes-nodes-resources-configuring.html){: external}.
Note that 1 `core` in OpenShift is equivalent to 1 vCPU in {{site.data.keyword.blockchainfull_notm}} Platform.

The {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 offering also includes the ability to download and run just the peer, orderer, CA, and smart contract container images without the console. This is an advanced option for customers who are experienced with the Hyperledger Fabric processes for deploying and running the nodes and includes limited support for those images.

Various licensing options are available. For more information, please [Contact us](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Free trial
{: #ibp-software-pricing-free}

OpenShift customers can preview the {{site.data.keyword.blockchainfull_notm}} Platform at no charge for 30 days when you select the free trial version available from the Red Hat Marketplace.  Simply follow instructions to [deploy the blockchain console](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp-rhm) to get started. The free trial version is fully functional, meaning the product features available in the free trial version are identical to the paid version.

### Free trial limitations
{: #ibp-software-pricing-free-limits}

- Customers must have an existing OpenShift cluster with adequate resources available.
- Only one free trial is permitted per account.
- The free trial is available for 30 days, after which the product and all deployment artifacts are no longer available.
- There is no migration or upgrade to the paid version.
- Support is limited to documentation and self-help instructions such as community support.
