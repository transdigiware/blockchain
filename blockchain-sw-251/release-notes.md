---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"


keywords: release note, latest changes, Hyperledger Fabric, multicloud

subcollection: blockchain-sw-25

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# Release notes
{: #release-notes-saas-20}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-release-notes-saas-20">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-release-notes-saas-20">2.1.3</a>
    </p>
</div>


Use these release notes that are grouped by date to learn about the latest changes to {{site.data.keyword.blockchainfull}} Platform 2.5.
{:shortdesc}

See [Installing patches](/docs/blockchain-sw-25?topic=blockchain-sw-25-console-icp-manage#ibp-console-manage-patch) for instructions on how to apply patches to your existing nodes. Patches are cumulative. This means that if multiple patches, for example `1.4.7-0` and `1.4.7-1`, are available for a node, you should always select the latest patch, `1.4.7-1` in this case, wherever possible because it includes the fixes from the previous patches as well.

## 25 Aug 2020
{: #08-25-2020}





**CA, Peer, and ordering node patch 1.4.7-2, 2.1.1-2**

Miscellaneous bug fixes and security patches.  

Certificate expiration dates have been added throughout the component details, making it easier to monitor and track certificate expiration dates. See [Certificate Management](/docs/blockchain?topic=blockchain-cert-mgmt) to learn more about your responsibilities. In addition, it is now possible to enable Node OU support for your MSPs and channels through the console. Read more about [Node OU support](/docs/blockchain?topic=blockchain-cert-mgmt#cert-mgmt-nodeou), why this is important, and how to simplify certificate renewal for the MSPs on your network.


## 14 July 2020
{: #07-14-2020}

**CA, Peer, and ordering node patch 1.4.7-1, 2.1.1-1**  

Miscellaneous bug fixes and security patches.




## 18 June 2020
{: #06-18-2020}

**Peer and ordering node patch 1.4.7-0**

**{{site.data.keyword.IBM_notm}} considers this a critical patch that you should apply at your nearest opportunity.** Not applying this patch risks being susceptible to a data integrity issue if you experience a CouchDB or underlying storage crash on your peer. This patch updates the CouchDB `delayed_commits` configuration property to false. The prior setting could cause the peer's CouchDB database to be in an inconsistent state in the event of a CouchDB or underlying storage system crash. The new setting ensures that a peer will recover from crashes in a consistent state. As always, it is recommended to utilize an endorsement policy that requires multiple peers to endorse a transaction, to avoid an inconsistency from a single peer from impacting the overall blockchain network state.  

**To be certain that your peers have no database corruption**, you should reprovision your peers.

Miscellaneous bug fixes and security patches. 

If you are running **OpenShift Container Platform 3.11** in {{site.data.keyword.cloud_notm}}, it is recommended that you [upgrade your cluster to 4.3](https://docs.openshift.com/container-platform/4.3/migration/migrating_3_4/planning-migration-3-to-4.html){: external} now in order to fully take advantage of the new features. After you upgrade your cluster, follow instructions to [refresh your blockchain console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-refresh) to experience the latest functionality in this release.
{: important}


### Fabric peer and ordering node images
{: #06-18-2020-images}

The platform introduces the capability to deploy new peer and ordering nodes based on either Hyperledger Fabric v1.4 or v2.x. Deploying peer and ordering nodes with the latest Fabric images is recommended to ensure that you have access to current Fabric fixes and features. It is not currently possible to migrate existing nodes to Fabric v2 images.

### Elimination of Docker daemon dependency
{: #06-18-2020-docker}

Leveraging the Fabric v2 **external chaincode launcher** capability, when you deploy a peer based on the Fabric v2.1.1 image, smart contracts are deployed into their own pod rather than inside a container on the peer pod.

### Multizone-capable storage
{: #06-18-2020-Multizone}

If your Kubernetes cluster is configured to use multizone-capable storage, new peer and ordering nodes can be deployed that leverage multizone storage, effectively extending their high availability across cluster zones. See [Multizone-capable Storage](/docs/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage-multizone) for more information.

### Kubernetes version upgrade
{: #06-18-2020-k8s}

{{site.data.keyword.blockchainfull_notm}} Platform requires **Kubernetes v1.15 - v1.18**. If your existing Kubernetes cluster is running v1.14 or lower, you need to upgrade your cluster before you can update your existing blockchain components to this latest release.

