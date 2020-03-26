---

copyright:
  years: 2017, 2020
lastupdated: "2019-08-27"

keywords: Log analysis, logDNA, viewing logs, monitoring

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:note: .note}
{:tip: .tip}
{:download: .download}

# IBM Cloud LogDNA
{: #ibp-LogDNA}
{: help}
{: support}

{{site.data.keyword.cloud_notm}} includes the {{site.data.keyword.la_full_notm}} service which is useful for viewing and parsing the logs of your {{site.data.keyword.blockchainfull}} Platform nodes and smart contract containers.
{: shortdesc}

If you are using Starter Plan, refer to these [instructions](/docs/blockchain?topic=blockchain-monitor-blockchain-network#monitor-blockchain-network-viewing-kibana-logs) for how to use {{site.data.keyword.la_full_notm}} to view your Starter Plan peer, Certificate Authority, and ordering node logs.
{: note}

This tutorial describes how to configure {{site.data.keyword.la_full_notm}} service to work with your {{site.data.keyword.blockchainfull_notm}} Platform service instance. First, you need to configure cluster-level logging for you Kubernetes cluster. Then, you can access the LogDNA dashboard from the **Logging** tab of the **Observability** dashboard in {{site.data.keyword.cloud_notm}}.

## Step one: Configure cluster-level logging
{: #ibp-LogDNA-kubernetes}

You need to deploy an instance of the {{site.data.keyword.la_full_notm}} service in your {{site.data.keyword.cloud_notm}} account. Complete the steps in the [Managing Kubernetes cluster logs with {{site.data.keyword.la_full_notm}}](/docs/Log-Analysis-with-LogDNA?topic=LogDNA-kube){: external} tutorial.

## Step two: View the logs for your {{site.data.keyword.blockchainfull_notm}} Platform nodes
{: #ibp-LogDNA-ibp}

Using the {{site.data.keyword.la_full_notm}} service makes viewing your {{site.data.keyword.blockchainfull_notm}} Platform node logs simple. The search input box located at the bottom of the LogDNA page can be used to filter the logs. See the following examples.

### View node logs
{: #ibp-LogDNA-ibp-nodes}

When you deploy a peer, Certificate Authority (CA), or ordering node,  a new pod is created in your Kubernetes cluster.  You can use the name of the pod to filter the logs by node. For example, to view the logs of a peer node simply add the text `pod:<pod_name_of_peer_node>` to the search input box, replacing `<pod_name_of_peer_node>` with the name of the peer node pod.  The pod name of your node can be obtained by using kubectl commands such as 'kubectl get pod', or it is visible in your Kubernetes dashboard. You need to filter on your namespace for the blockchain nodes to be visible In the Kubernetes dashboard. To find the namespace, open any certificate authority tile in your blockchain console and click the **Settings** icon. View the value of the **Certificate Authority endpoint URL**. For example: `https://n2734d0-paorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054`.

The namespace is the first part of the url beginning with the letter `n` and followed by a random string of six alphanumeric characters. So in the example above the value of the namespace is `n2734d0`.  

The resulting search string in the logDNA dashboard looks similar to:

```
pod:n4c817fpeer1org1-55885d5666-zq55t
```
{: codeblock}

In this case, `n4c817fpeer1org1-55885d5666-zq55t` represents the name of the ordering node pod. This process can be used to view the logs of ordering nodes, peers and CAs. Simply add the pod name of the node to the
search bar as shown in the following screenshot:

![Filtering on node](../images/logDNAPod.png "Filtering logs by node"){: caption="Figure 1.Filtering logs by node" caption-side="bottom"}  

### View smart contract logs
{: #ibp-LogDNA-ibp-smart-contract}

The process to view the logs for a smart contract is similar to how you view your node logs.  But additional strings are included the search input box.
- As above, you include the pod name, but it should be the pod name of a peer where the smart contract is running.
- Since smart contracts run in a `fluentd` container in Kubernetes, we also append the string `app:fluentd` in the search input box.
- Finally, you can append the name and optionally the version of the smart contract, `marbles 1.0.0`.

Thus the full search string would be similar to the following example:

```
pod:n4c817fpeer1org1-55885d5666-zq55t app:fluentd marbles 1.0.0
```
{: codeblock}

![Filtering on smart contract](../images/logDNAsc.png "Filtering logs by smart contract"){: caption="Figure 2.Filtering logs by smart contract" caption-side="bottom"}  
