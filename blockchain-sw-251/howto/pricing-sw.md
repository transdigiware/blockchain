---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: pricing model, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

subcollection: blockchain-sw-25

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}


# Pricing
{: #ibp-sw-pricing}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-ibp-sw-pricing">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-sw-pricing">2.1.3</a>
    </p>
</div>


This guide helps you understand the pricing model for {{site.data.keyword.blockchainfull}} Platform 2.5, and how much you will pay when you develop and grow your blockchain network of Certificate Authorities, peers, and ordering service components, that are based on Hyperledger Fabric v1.4.7 and v2.x.
{:shortdesc}

## License
{: #ibp-software-pricing-license}

{{site.data.keyword.blockchainfull_notm}} Platform 2.5 delivers the components that are needed to run a blockchain network on your own infrastructure using Kubernetes. You can access your [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering which is required in order to deploy the release. With this offering, you are responsible for procuring and provisioning your own Kubernetes cluster on one of the [Supported Platforms](/docs/blockchain-sw-25?topic=blockchain-sw-25-console-ocp-about#console-ocp-about-prerequisites). Technical support from IBM Blockchain is included in the offering.

## Pricing
{: #ibp-software-pricing-pricing}

The pricing of {{site.data.keyword.blockchainfull_notm}} Platform 2.5 is based on the number of Virtual Processor Cores (VPCs) used. A VPC, or vCPU, can be either a virtual core that is assigned to a virtual server, or a physical processor core in a non-partitioned server. You must obtain a licensed entitlement for each VPC made available to the {{site.data.keyword.blockchainfull_notm}} Platform. The number of vCPU or CPUs that are required can vary depending on your infrastructure, network design and performance requirements.

If you are using the Red Hat OpenShift Container Platform, you can read more about  VPCs (CPUs) in that platform, see  [Allocating Node Resources](https://docs.openshift.com/container-platform/4.3/nodes/nodes/nodes-nodes-resources-configuring.html){: external}.
Note that 1 `core` in OpenShift is equivalent to 1 vCPU in {{site.data.keyword.blockchainfull_notm}} Platform.

The {{site.data.keyword.blockchainfull_notm}} Platform 2.5 offering also includes the ability to download and run just the peer, orderer, CA, and smart contract container images without the console. This is an advanced option for customers who are experienced with the Hyperledger Fabric processes for deploying and running the nodes and includes limited support for those images.

Various licensing options are available. For more information, please [Contact us](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Free trial
{: #ibp-software-pricing-free}

OpenShift customers can preview the {{site.data.keyword.blockchainfull_notm}} Platform at no charge for 30 days when you select the free trial version available from the Red Hat Marketplace.  Simply follow instructions to [deploy the blockchain console](/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-ocp-rhm) to get started. The free trial version is fully functional, meaning the product features available in the free trial version are identical to the paid version.

### Free trial limitations
{: #ibp-software-pricing-free-limits}

- Customers must have an existing OpenShift cluster with adequate resources available.
- Only one free trial is permitted per account.
- The free trial is available for 30 days, after which the product and all deployment artifacts are no longer available.
- There is no migration or upgrade to the paid version.
- Support is limited to documentation and self-help instructions such as community support.
