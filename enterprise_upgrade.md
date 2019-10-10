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

# Upgrading to the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}
{: #enterprise-upgrade}

The {{site.data.keyword.blockchainfull}} Platform upgrade tool is not yet available. Enterprise Plan users will able to use {{site.data.keyword.blockchainfull_notm}} Platform upgrade tool to migrate their networks to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} starting 15 November 2019.
{: note}

You can use the {{site.data.keyword.blockchainfull_notm}} Platform upgrade tool to migrate your Enterprise Plan network to {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} (referred to as {{site.data.keyword.blockchainfull_notm}} Platform 2.0). The upgrade tool allows you to create new nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 side by side with your Enterprise Plan network. Your new nodes are joined to your exiting channels and contain the same ledger data as your old network. You can continue to use your Enterprise Plan network throughout the upgrade process, providing you the ability to migrate your applications without any downtime. When your applications are successfully submitting transactions to your nodes on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0, you can safely delete your network on Enterprise Plan.

Review [Before you begin](#enterprise-upgrade-considerations) before you start by using the upgrade tool. You can then review the [Upgrade overview](#enterprise-upgrade-overview) to learn the steps of the upgrade process and the recommended path for migrating a blockchain consortium to the new platform.

## Before you begin
{: #enterprise-upgrade-considerations}

If possible, Enterprise Plan customers are encouraged to migrate to the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 right away. If your network on Enterprise Plan does contain production data that is critical to your applications, you are encouraged to skip the upgrade process and create a new network on the {{site.data.keyword.blockchainfull_notm}} for {{site.data.keyword.cloud_notm}}. If you do not need to preserve your ledger data on Enterprise Plan, go to [Getting Started with {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

It is helpful to become familiar with the {{site.data.keyword.blockchainfull_notm}} Platform 2.0 console before you start the upgrade process. See the [getting started guide](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) to learn how to deploy an instance of {{site.data.keyword.blockchainfull_notm}} Platform 2.0. You can then use the [Build a network tutorial](/docs/services/blockchain/reference?topic=blockchain-ibp-console-build-network#ibp-console-build-network) to learn how to use the console to operate your new network.

You can only use the upgrade tool to migrate your network to the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. You cannot use the tool to migrate your network to {{site.data.keyword.blockchainfull_notm}} Platform 2.1.0.

Hyperledger Composer is not supported on the {{site.data.keyword.blockchainfull_notm}} Platform 2.0. If you want assistance for upgrading a network that is using Composer on the new platform, engage [{{site.data.keyword.blockchainfull_notm}} services](https://www.ibm.com/blockchain/services).

## Upgrade Overview
{: #enterprise-upgrade-overview}

The migration process involves steps that need to be completed by {{site.data.keyword.IBM_notm}} and the organizations participating in your network.

- **Done by IBM:** [Upgrade your Enterprise Plan network to Fabric v1.4.3](#enterprise-upgrade-overview-one)
- **Done by all network members:** [Update their applications to use service discovery](#enterprise-upgrade-overview-two)
- **Done by all network members:** [Use the upgrade tool to create new nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0](#enterprise-upgrade-overview-three)

The members of your blockchain network can use these steps to migrate to the new platform without experiencing network downtime or making extensive changes to their client applications.

## Step one: Have your Enterprise Plan network upgraded to Fabric v1.4
{: #enterprise-upgrade-overview-one}

If you choose upgrade your Enterprise Plan nework to {{site.data.keyword.blockchainfull_notm}} Platform 2.0, {{site.data.keyword.IBM_notm}} will upgrade your Enterprise Plan network from Fabric v1.1 to Fabric v1.4.3. {{site.data.keyword.IBM_notm}} can then migrate your ordering service from Kafka to RAFT consensus. Upgrading to RAFT will allow you to create new ordering nodes on {{site.data.keyword.blockchainfull_notm}} Platform 2.0.

The upgrade of your network to Fabric 1.4.3 might cause breaking changes that will require updates to your application. For more information, see [Prepare for the breaking changes from Fabric v1.4](/docs/services/blockchain/reference?topic=blockchain-enterprise-upgrade-applications#enterprise-upgrade-applications-one).

## Step two: Update your applications to use service discovery
{: #enterprise-upgrade-overview-two}

When your Enterprise Plan network is running on Fabric 1.4.3, you can update your client application to take advantage of the [Service Discovery Feature](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} of Hyperledger Fabric. Using service discovery makes it easier to upgrade to {{site.data.keyword.blockchainfull_notm}} Platform 2.0:
- Applications can find all of the peers and ordering nodes that are created on the new platform, eliminating the need coordinate the creation of new nodes between the members of your network.
- All of the needed application updates can be completed and tested before you start using the upgrade tool.
- Using service discovery prepares your application for long-term use of the new platform.

As a result, it is recommended that all of the organizations in your network update their applications to use service discovery before they start using the upgrade tool. For guidance on how you an update your applications, see [Updating your applications](/docs/services/blockchain/reference?topic=blockchain-enterprise-upgrade-applications#enterprise-upgrade-applications).

## Step three: Get started with the upgrade tool
{: #enterprise-upgrade-overview-three}

After your Enterprise Plan network has been upgraded to use Fabric v1.4.3 and a RAFT ordering service, you can start using the upgrade tool to create nodes on the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. For more information on how to use the tool and how the tool works, see [Getting started with upgrade tool](/docs/services/blockchain/reference?topic=blockchain-enterprise-upgrade-tool#enterprise-upgrade-tool).

If you need time to update your applications, you can start using the tool to create and test new nodes as soon as the tool is available. However, all of the applications that interact with your network should be updated to use service discovery before you start submitting transactions to the new platform. Once all of the applications have been updated, each organization in your network can use the upgrade tool at their own pace.

## Support
{: #enterprise-upgrade-support}

If you encounter an issue during the upgrade process, open a support ticket by using the {{site.data.keyword.cloud_notm}} Service Portal. For more information, see [Submitting support cases](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).
