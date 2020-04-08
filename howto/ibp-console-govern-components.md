---

copyright:
  years: 2019, 2020
lastupdated: "2020-04-08"

keywords: network components, IBM Cloud Kubernetes Service, allocate resources, batch timeout, reallocate resources, LevelDB, CouchDB

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Reallocating resources
{: #ibp-console-govern-components}



After creating CAs, peers, and ordering nodes, you need to monitor the resource usage by the nodes and potentially reallocate resources.
{:shortdesc}

**Target audience:** This topic is designed for network operators who are responsible for creating, monitoring, and managing their components in the blockchain network.
## Considerations when reallocating resources
{: #ibp-console-govern-components-reallocate-resources}

Resizing a node requires the containers to be rebuilt, which can cause a delay in the functioning of the node.
{:important}

We recommend using the [{{site.data.keyword.cloud_notm}} Sysdig](https://www.ibm.com/cloud/sysdig){: external} tool in combination with your {{site.data.keyword.cloud_notm}} Kubernetes dashboard to monitor your Kubernetes resource usage. If you determine that a worker node is running out of resources, you can add a new larger worker node to your cluster and then delete the existing working node.
{:note}




While it takes less effort to deploy enough resources to your Kubernetes cluster from the start and therefore be able deploy and expand resources without having to increase the resources in your cluster, the bigger the deployment, the more it will cost. Users need to consider their options carefully and recognize the tradeoffs that they are making regardless of the option that they choose.


You can scale your cluster manually by monitoring your nodes and either adding more nodes or larger nodes. While this process can be labor intensive, it has the advantage of allowing the user to always be certain what is being charged to their cloud account.

The {{site.data.keyword.cloud_notm}} Kubernetes Service also gives users the ability to use the {{site.data.keyword.cloud_notm}} Kubernetes Service **autoscaler**. The autoscaler will scale your worker nodes up or down in response to your pod spec settings and resource requests. For more information about the {{site.data.keyword.cloud_notm}} Kubernetes Service autoscaler and how to set it up, see [Scaling clusters](/docs/containers?topic=containers-ca#ca){: external} in the {{site.data.keyword.cloud_notm}} documentation. Note that allowing the autoscaler to adjust your resources will result in charges to your {{site.data.keyword.cloud_notm}} Kubernetes Service account that will vary automatically with your usage.

To scale manually in the console, click the node that you want to adjust on the **Nodes** page and then click the **Usage** tab. You can see a button called **Reallocate**, which will launch a **Resource allocation** tab that is very similar to the one that you saw when you created the node. If you want to lower the amount of available resources, simply provide lower values and click **Reallocate resources** on that tab and the resulting **Summary** page.

If you want to increase the CPU and memory for a node, use the **Resource allocation** tab in the console to increase the values. The white box at the bottom of the page will add up the new values. After clicking **Reallocate resources**, the **Summary** page will translate this value into a **VPC** amount, which is used to calculate your bill. You'll then need to navigate to your Kubernetes cluster to make sure your cluster has sufficient resources for this reallocation. If it does, you can click **Reallocate resources**. If sufficient resources are not available, you will need to increase the size of your cluster.

The method you will use to increase storage will depend on the storage class you chose for your cluster.  Refer to the [storage options](/docs/containers?topic=containers-kube_concepts#kube_concepts){: external} topic in the {{site.data.keyword.cloud_notm}} documentation. If you are about to exhaust the storage on your peer or ordering node, you might need to deploy a new peer or ordering node with a larger file system and let it sync via your other components on the same channels.

In {{site.data.keyword.cloud_notm}}, CPU and memory can be increased using the console if you have resources available in your {{site.data.keyword.cloud_notm}} Kubernetes Service cluster. However, storage must be increased using the {{site.data.keyword.cloud_notm}} CLI. For a tutorial on how to do this, see [Changing the size and IOPS of your existing storage device](/docs/containers?topic=containers-file_storage#file_change_storage_configuration){: external}.





### Monitoring file storage
{: #ibp-console-govern-components-monitor-storage}

To view your consumption of file storage, navigate to your {{site.data.keyword.cloud_notm}} Kubernetes Service cluster. Click on the menu button in the upper left hand corner. Then click on **Classic infrastructure**, **Storage**, and then **File Storage**. This will display the capacity and usage for each persistent volume claim (PVC). This usage can be mapped to your {{site.data.keyword.blockchainfull_notm}} Platform nodes by clicking on the cell in the **Notes** column.

You will see something that looks like this:

```
PVC {"plugin":"ibm-file-plugin-77497497cc-5qm58","region":"us-south","cluster":"05ccfca248dc4c389978d074524e0a8e","type":"Endurance","ns":"n4c817f","pvc":"n4c817forg1ca-fabric-ca-pvc","pv":"pvc-63074ac8-8b9b-11e9-88db-222bd59fb4bc","storageclass":"default","reclaim":"Delete"}
```

To see the same information from the command line, login to your cluster and enter this command:

```
ibmcloud sl file volume-list --column id --column notes
```

This will allow you to map the output from the pods to the nodes you have deployed.

### Adding storage
{: #ibp-console-govern-components-add-storage}

As you monitor your pods and notice that more storage is needed, you can increase storage when additional storage is available. The following links provide more information about how to increase storage after your network is deployed.

- [IBM file storage](/docs/FileStorage?topic=FileStorage-expandCapacity)
- [Portworx](https://docs.portworx.com/portworx-install-with-kubernetes/storage-operations/create-pvcs/resize-pvc/)
- [Block storage](/docs/BlockStorage?topic=BlockStorage-expandingcapacity#expandingcapacity)

Between the ability to resize storage and the ability to add new ordering nodes, users have more flexibility in allocating storage to their network after it is deployed.


