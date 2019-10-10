---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-09"

keywords: IBM Blockchain Platform, blockchain

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:javascript: data-hd-programlang="javascript"}
{:java: data-hd-programlang="java"}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Getting started with upgrade tool
{: #enterprise-upgrade-tool}

You can use the upgrade tool to create new components on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 side by side with your existing network. The new nodes that are created by the tool are joined to the same channels and store the same ledger data as your components on Enterprise Plan.

The upgrade tool user interface guides you through a series of independent steps. Although you need to follow the steps in order, you do not need to complete the upgrade process in a single session. You can migrate your network carefully and test the new nodes that are created by tool. Use the following steps to get started and learn more about how the upgrade process works.


## Before you begin

### Prerequisites
{: #enterprise-upgrade-tool-prerequisites}

- {{site.data.keyword.IBM_notm}} needs to upgrade your Enterprise Plan network to Fabric v1.4.3 and migrate your ordering service from Kafka to RAFT consensus before you can start using the upgrade tool. The upgrade tool will appear on your Network Monitor after those upgrades are complete.

### Considerations and limitations
{: #enterprise-upgrade-tool-considerations}

- You can use the upgrade tool to complete every step of the migration process without assistance from {{site.data.keyword.IBM_notm}}. If you encounter any problems while you are upgrading your network, you can open a support ticket. For more information, see [support](#enterprise-upgrade-support).
- Each organization that has a separate Enterprise Plan membership must use the upgrade tool to migrate their components. The founder of the network cannot upgrade all of the organizations and peers that are joined to your network.
- Review the [considerations for using {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks-considerations) before you upgrade your network. The next generation of the platform has a different user interface and provides you with more control over the nodes of your network. In particular, you are responsible for managing your certificates and private keys, which are not stored on {{site.data.keyword.cloud_notm}}.
- It is helpful to become familiar with the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console before you start the upgrade process. See [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) to learn how to deploy an instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You can then use the [Build a network tutorial](/docs/services/blockchain/reference?topic=blockchain-ibp-console-build-network#ibp-console-build-network) to learn how to use the console to operate your new network.
- You must create peers on {{site.data.keyword.blockchainfull_notm}} Platform 2.0 that use the same state database (LevelDB or CouchDB) as your peers on Enterprise Plan. You cannot migrate from peers that are running LevelDB to peers that use CouchDB.
- As part of having more control of your network on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you need to take steps to ensure high availability and disaster recovery. For more information, review [High Availability on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/reference?topic=blockchain-ibp-console-ha#ibp-console-ha).

## Opening the upgrade tool

You can find the upgrade tool on your Enterprise Plan Network Monitor. Go to the **Overview** page and click **Upgrade network**. The tool opens in a separate URL of your browser. You can use this URL to return to the tool without using the Network Monitor. You can exit the tool without losing your work as you go through the upgrade process.

If you are not able to see the **Upgrade network** button, your Enterprise Plan network needs to be upgraded to Fabric v1.4 or your ordering service needs to be migrated to RAFT.

## Create an instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0

You need to deploy an instance of the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 that contains sufficient resources for your Enterprise Plan network. The **Deploy {{site.data.keyword.blockchainfull_notm}} Platform 2.0** displays the CPU and memory that is used by your Enterprise Plan network. Use this information when you select the size of the Kubernetes cluster on {{site.data.keyword.cloud_notm}} that you must create to deploy the next generation of the platform. Deploying the platform is a two-step process:

1. Create a cluster on {{site.data.keyword.cloud_notm}} with the {{site.data.keyword.IBM_notm}} Kubernetes service. The upgrade tool provides an overview of the resources that are used by your Enterprise Plan network. It is recommended that you create a multizone cluster for high availability.

2. After you create a Kubernetes cluster, you can deploy a service instance of the {{site.data.keyword.blockchainfull_notm}} Platform and link it to that cluster.

After you review the resources that are required, click **Create {{site.data.keyword.blockchainfull_notm}} Platform 2.0 network**. You can also follow [Getting started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

## Upload connection information

You need to upload the connection information of your Enterprise Plan network and your {{site.data.keyword.blockchainfull_notm}} Platform 2.0 service instance to the upgrade tool.

- Copy the service credentials from your Enterprise Plan network and paste them to the window named **Enterprise Plan service credentials**.

- Provide the enrollID and Secret of your Certificate Authority on Enterprise Plan. You can find this information in your connection profile:

  Open the **Connection Profile** JSON file from the **Overview** screen of your Network Monitor. You can find required information under the `certificateAuthorites` section:
    * Enroll ID: ``enrollId``
    * Enroll Secret: ``enrollSecret``

- Copy the service credentials your new instance of {{site.data.keyword.blockchainfull_notm}} Platform and paste them into the **{{site.data.keyword.blockchainfull_notm}} Platform 2.0 service credentials** window. You can create the service credentials by using the following steps:

  1. In your [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources){: external}, open the blockchain service instance that you created.
  2. Click **Service credentials** from the left navigator.
  3. Click the **New credential** button on the **Service credentials** page to create a new credential.
    1. Give the credential a name, for example, *Upgrade*.
    2. You can leave the **Add inline configuration parameter** field blank.
    3. Click the **Add** button.
  4. After the new credential is created, click **View credentials** under the **ACTIONS** header of this credential. The contents of the credential looks similar to the following example:

    ```
    {
      "api_endpoint": "https://1088ac8563e44f5a92539d946733ad7e-optools.so01.blockchain.test.cloud.ibm.com"
      "apikey": "nvASKts6B6lMZGZUNTGVP7MLK2BujMnxz0plSPYaqc3R",
      "configtxlator": "https://1088ac8563e44f5a92539d946733ad7e-configtxlator.so01.blockchain.test.cloud.ibm.com",
      "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:blockchain:us-south:a/9d46037caee397faa45c55e087d26baa:1088ac85-63e4-4f5a-9253-9d946733ad7e::",
      "iam_apikey_name": "auto-generated-apikey-c1981f5c-cff0-464f-9a78-ed2f52e24d1a",
      "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
      "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/9d46037caee397faa45c55e087d26baa::serviceid:ServiceId-774190c5-9c37-4f68-8572-8d6e2aabbc7e",
    }
    ```

## Create a backup of your Enterprise Plan network

You can save a snapshot of your Enterprise Plan network as a protection against any problems that might occur during the upgrade process. This data is saved as a precaution, and is not used by the migration tool. The backup will complete after a few minutes.

## Migrating your Certificate Authorities

The first step of the upgrade is to create a new Certificate Authority (CA) on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. The identities that are created by the new CA are recognized as belonging to your organization on Enterprise Plan. You can use the new CA to deploy peer and ordering nodes on the {{site.data.keyword.blockchainfull_notm}} platform 2.0 and join those nodes to the channels created on Enterprise Plan. The application, node, and admin identities that you created on Enterprise Plan are registered with the new Certificate Authority.

In the page, you can find a list of the Certificate Authorities that you own on Enterprise Plan. If you are a network founder, the operator of the organization that is named peerOrg1 in your connection profile, you can also see the CA used by the ordering service. You must migrate both of these CAs to the new platform.

For each CA you want to migrate, enter a **Display Name** for the new CA. This name is used to identify the CA on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You also need to create the enrollID and Secret of the CA Admin. **Save these values**. You need to use the EnrollID and Secret to generate the CA admin identity and operate your CA.

You can use the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console to use your CA to register identities and generate certificates. For more information, see [Creating and managing identities](/docs/services/blockchain/reference?topic=blockchain-ibp-console-identities#ibp-console-identities).

## Migrating your Peers

After you migrate your Certificate Authorities, you can start migrating your peers. The peers that you create on {{site.data.keyword.blockchainfull_notm}} Platform 2.0 are automatically joined to the channels that were created on Enterprise Plan. Because they are joined to the same channels, the new peers can receive ledger data from your existing peers by using intra-organization gossip.

Before you can migrate your peers, you must create an organization administrator identity and organization MSP definition. You can use the organization administrator to operate your peers with the console UI or by using the Fabric CLI tools or Fabric SDKs. The organization MSP definition allows you to deploy nodes and join or create channels on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0.

Under **Create an organization admin and MSP**, select the organization CA (instead of the ordering service CA) that you created on the previous step. Then, provide the enrollID and secret of your organization admin. When you click **Create**, the upgrade tool completes the following steps:

- Register a new organization administrator with your CA with the enrollID and secret that you provide.
- Generate an administrator certificate and private key with the admin identity.
- Create an organization MSP definition that contains the administrator signing certificate. You can see the MSP with the console UI by going to the organizations tab.

After you create the administrator identity and the MSP, click the **Export Identity** button to download the administrator private key and certificate. Upload this identity to your console wallet to use console to operate your new nodes. For more information about your organization MSP and your organization administrator, see [Managing organizations](/docs/services/blockchain/reference?topic=blockchain-ibp-console-organizations#ibp-console-organizations).

You can then start migrating your Peers. For each Peer you want to migrate, enter a **Display Name** for the new peer on {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You also need to provide the enrollID and Secret for the peer identity. Your peer uses this identity to generate the certificate and keys it needs to operate on the network during deployment.

If you created a multizone cluster, you can use this page to deploy your peer to a specific zone and ensure that your peers are deployed in different zones. This ensures that your peers are available if there is a zone failure. While the upgrade tool uses an anti-affinity policy to deploy your peers to worker nodes with free resources, the anti-affinity policy cannot detect if the peers are being deployed to different zones. For more information about how to find the zones of your cluster, see [Creating a node within a specific zone](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-apis#ibp-v2-apis-zone).

## Migrating your ordering service

## Migrating your smart contracts

The Enterprise Plan Network Monitor UI and the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console use different formats to install chaincode on your peers. You can use the tool to migrate the chaincode that you installed on Enterprise plan to {{site.data.keyword.blockchainfull_notm}} Platform 2.0 without having to repackage your chaincode. Chaincode is referred to as smart contracts on on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console.

You can find a list of chaincode packages that are installed on the peers of your Enterprise Plan network. The chaincode packages are grouped by chaincode version. You can use this page to find the latest version of each smart contract that is installed. **However, you need to have the chaincode on your local file system to use this page**.

You can use the **Upload chaincode file** button to find the chaincode package on your local file system. Select the peers on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 that you would like to install the chainode on and then click **Migrate**. You can view this smart contract with the {{site.data.keyword.blockchainfull_notm}} Platform console UI by browsing to the smart contracts tab. You can also use this page to instantiate the smart contract on your channels and set the endorsement policy. For more information, see [Deploy a smart contract on the network tutorial](/docs/services/blockchain/reference?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts).

If you want to upgrade your smart contract with the console UI, you need to package your chaincode in a .cds file. You can use the {{site.data.keyword.blockchainfull_notm}} Platform Visual Studio Code extension to develop your chaincode project and package it in the correct format. For more information, see [Developing smart contracts with Visual Studio Code extension](/docs/services/blockchain/reference?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts).

## Complete upgrade

The upgrade tool stores the certificates and private keys that it used deploy your {{site.data.keyword.blockchainfull_notm}} Platform 2.0 network on your cluster inside Kubernetes secrets. When you finish upgrading your Enterprise Plan network, you can delete all the data that was created by the upgrade tool, including the certificates and keys, by clicking **Complete Upgrade**. **Note** that this action is final. After you complete the upgrade, you can no longer use the tool to interact with your network. You can see an overview of the nodes and chaincode that you migrated to the new platform to help you decide whether you still need to use the tool.

Ensure that you downloaded your organization administrator identities and stored them in a secure place you click **Complete Upgrade**. You also need to save the enrollID and secret of your Certificate Authority administrator. After you complete this step, only the network administrator with your org admin identity can operate your network on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0.
{: important}
