# Name
IBM&reg; Blockchain Platform

# Introduction

## Details

The IBM Blockchain Platform provides advanced tools to help build, operate, govern, and grow blockchain solutions with flexible deployment options. The platform is based on Hyperledger Fabric, the leading open source distributed ledger technology for enterprises, and is optimized to run on Red Hat OpenShift.

Supported versions: OpenShift Container Platform 4.3+

## Features

- **Build**

  Easily code your smart contracts in Node.js, Golang (Go), Java, or JavaScript. Use the IBM Blockchain Platform Developer Tools to develop smart contracts locally or use Red Hat CodeReady Workspaces to develop them in the cloud. Leverage **SDK integration** with the console, and learn from  rich tutorials and samples.

- **Operate**

  Customers have total control of their deployments where they can host or join networks while maintaining complete control of their identities,  including support for HSM to generate and store the private key of your nodes. The IBM Blockchain Platform console allows you to deploy and manage all of your organizations and nodes in **one console**. You can also add or remove members from a blockchain consortium, create and join channels, and install and instantiate smart contracts from your console. The console includes **Dynamic signature collection** that allows better control over collaborative governance of channel configurations. The platform includes the latest Hyperledger Fabric features, such as support for a Raft ordering service, private data collections, Fabric Node OUs, service discovery, channel access control, and more.

- **Grow**

  For scalability and flexibility, customers can choose the amount of CPU, memory, and storage they want to provision in their OpenShift cluster, and scale the resources up and down to pay for only what they need. Interoperability of the platform allows customers to easily connect to other Fabric networks by joining their peers to any network running Hyperledger Fabric components. Similarly, you can invite Fabric peers to join channels hosted on an ordering service deployed on the IBM Blockchain Platform.

# Details

You can install the console operator and then use the console  to deploy Certificate Authority (CA), orderer, and peer nodes that are based on Hyperledger Fabric. See the [Build a network tutorial](https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-console-build-network) to easily get started building a blockchain network.

Use the instructions in this README if you plan to deploy the IBM Blockchain Platform from the **Red Hat Marketplace**. Otherwise, refer to the CASE instructions:
- [Deploying the IBM Blockchain Platform Operator](./inventory/ibmBlockchainPlatformOperatorSetup/README.md)
- [Deploying the IBM Blockchain Platform Console](./inventory/ibmBlockchainPlatformConsole/README.md)

## Prerequisites

1. These instructions assume you already have a Kubernetes cluster available in OpenShift Container Platform v4.3+.
2. Browse to the [Red Hat Marketplace](https://marketplace.redhat.com/en-us){: external} and log in or create a new account.
3. Follow the [instructions](https://marketplace.redhat.com/en-us/documentation/clusters#register-openshift-cluster-with-red-hat-marketplace) to register your OpenShift cluster with the Red Hat Marketplace.
4. When prompted `Would you like to go back to the Red Hat Marketplace now? [Y/n]`, type `Y` and the Red Hat Marketplace page is opened in your browser.
5. Click **My software** > **Visit the Marketplace**.
6. In the search bar type "blockchain" to load the blockchain tile.
7. Click **Purchase** or **Free trial** to get started. Do not install the operator yet. Continue to [Step one: Log in to your OpenShift cluster](https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-ocp-rhm#deploy-ocp-rhm-login).

You must have the cluster administrator role to install the operators from the Red Hat Marketplace.
{: note}

## Resources Required

Ensure that your OpenShift cluster has sufficient resources for the IBM Blockchain console and for the blockchain nodes that you create. The amount of resources that are required can vary depending on your infrastructure, network design, and performance requirements. To help you plan your resource requirements, the default CPU, memory, and storage requirements for each component type are provided in this table. Your actual resource allocations are visible in your blockchain console when you deploy a node and can be adjusted at deployment time or after deployment according to your business needs.

| **Component** (all containers) | CPU**  | Memory (GB) | Storage (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer (Hyperledger Fabric v1.4)**                       | 1.1           | 2.8                   | 200 (includes 100GB for peer and 100GB for state database)|
| **Peer (Hyperledger Fabric v2.x)**                       | 0.7           | 2.0                   | 200 (includes 100GB for peer and 100GB for state database)|
| **CA**                         | 0.1           | 0.2                   | 20                     |
| **Ordering node**              | 0.35          | 0.7                   | 100                    |
| **Operator**                   | 0.1           | 0.2                   | 0                      |
| **Console**                    | 1.2           | 2.4                   | 10                                       |
** These values can vary slightly. Actual VPC allocations are visible in the blockchain console when a node is deployed.

# Installing

To get started, see [Step one: Log in to your OpenShift cluster](https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-ocp-rhm#deploy-ocp-rhm-login).

## Configuration

**IBM Blockchain Platform Console specification**

### Operator

| **Parameter** | Description | Default value | Required |
|---------------|-------------|---------------|----------|
| imagePullSecrets | Name of the Kubernetes secret that contains the entitlement key | docker-key-secret| Yes|

### Console

| **Parameter** | Description | Default value | Required |
|---------------|-------------|---------------|----------|
| email | Email address of the console administrator. | none provided | Yes|
| password | Initial password for the console administrator. | none provided | Yes|
| domain | Name of your Kubernetes cluster domain. You can find this value by using the OpenShift web console. Use the dropdown menu next to **OpenShift Container Platform** at the top of the page to switch from **Service Catalog** to **Cluster Console**. Examine the url for that page. It will be similar to `console.xyz.abc.com/k8s/cluster/projects`. The value of the domain then would be `xyz.abc.com`, after removing `console` and `/k8s/cluster/projects`. | none provided | Yes |

## Storage

In addition to a small amount of storage (10GB) required by the IBM Blockchain console, persistent storage is required for each CA, peer, and ordering node that you deploy. Because blockchain components do not use the Kubernetes node local storage, network-attached remote storage is required so that blockchain nodes can fail over to a different Kubernetes worker node in the event of a node outage. And because you cannot change your storage type after deploying peer, CA, or ordering nodes, you need to decide the type of persistent storage that you want to use before you deploy any blockchain nodes.

The IBM Blockchain Platform console uses dynamic provisioning to allocate storage for each blockchain node that you deploy by using a pre-defined storage class. You can choose your persistent storage from the available Kubernetes storage options.

Before you deploy the IBM Blockchain Platform, you must create a storage class with enough backing storage for the IBM Blockchain console and the nodes that you create. You can set this storage class to use the default storage class of your Kubernetes cluster or create a new class that is used by the IBM Blockchain Platform console. If you are using a multizone cluster in IBM Cloud and you change the default storage class definition, then you must configure the default storage class for each zone. After you create the storage class, run the following command to set the storage class of the multizone region to be the default storage class:
```
kubectl patch storageclass
```
**Note:** If you prefer not to choose a persistent storage option, the default storage class of your OpenShift project is used. Host-local volume storage is not supported.

## Limitations

Before you begin, ensure that you understand the following limitations:

- IBM Blockchain Platform 2.5.0 is only supported on Red Hat OpenShift 4.3.
- You are responsible for the management of health monitoring, logging, and resource usage of your blockchain components.
- You must have the cluster admin role in order to deploy the product.
- You can deploy only one IBM Blockchain Platform console per OpenShift project. If you plan to create multiple blockchain networks, for example to create different environments for development, staging, and production, you should create a unique project for each environment.
- IBM Blockchain Platform is not supported on OpenShift Online.
- This option is not available for OpenShift Container Platform on LinuxONE.
- Mutual TLS is not supported between your applications and your blockchain nodes.
- You can not use the ESR version of Firefox to log in to the IBM Blockchain Platform console.

## Documentation
To learn more see [Getting started with IBM Blockchain Platform 2.5](https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-get-started-console-ocp).

When you are ready to use the IBM Blockchain console to deploy and operate your blockchain network, see the [Build a network tutorial](https://cloud.ibm.com/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-console-build-network).
