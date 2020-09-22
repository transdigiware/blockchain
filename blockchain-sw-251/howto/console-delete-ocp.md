---

copyright:
  years: 2018, 2020
lastupdated: "2020-09-22"

keywords: OpenShift, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

subcollection: blockchain-sw-251

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Removing your deployment
{: #Removing-ocp}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-Removing-ocp">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-Removing-ocp">2.1.3</a>
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-Removing-ocp">2.5</a>
    </p>
</div>


The {{site.data.keyword.blockchainfull}} Platform operator automatically restarts your blockchain nodes or your console if they stop or crash. As a result, you cannot manually remove your blockchain components by manually deleting their pods. Use the following steps to remove the {{site.data.keyword.blockchainfull_notm}} Platform from your OpenShift cluster. You must follow these steps for each OpenShift project that you create.

If your organization is participating in an active blockchain network, you should remove your organization from the network before you remove your deployment. See [Removing an organization](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-console-organizations#console-organizations-remove) for more details.
{: important}

## Step one: Use the console to delete your blockchain nodes
{: #Removing-ocp-step-one}

The best practice for deleting components is to delete them using the console. This will also delete all of the artifacts associated with a node including your ledger data in persistent storage and the keys that are stored as secrets. Deleting a peer will not, however, delete any smart contract pods associated with it. These must be deleted separately. Deleting a component is usually achieved by logging onto the console where a component was created or installed, clicking on the component and finding the related **trash can** icon. You will typically be prompted to type the name of the component and to confirm your decision. You can also delete nodes by using the {{site.data.keyword.blockchainfull_notm}} Platform APIs.

However, there are cases in which this type of deletion will not be successful. For example, occasionally when a node fails to deploy it will not be possible to delete it using the console. The same can be true if the console loses connection with the cluster for some reason.

In these cases, it will be necessary to use the Kubernetes dashboard or delete the node or relevant pods manually. If you attempt to delete pods that contain deployments such as peers, CAs, or ordering nodes, the pod will automatically restart and not be permanently deleted. <blockchain-sw-251>Your Kubernetes cluster on {{site.data.keyword.cloud_notm}} manager of choice might have a UI that allows you to delete pods. Check the documentation for your cluster for instructions.</blockchain-sw-251>

Because smart contracts installed on a 2.x peer are deployed into their own pods and not directly into the peer container, they will not be deleted when a peer is deleted. They will have to be deleted either using the UI of your cluster or by issuing kubectl commands. Smart contracts installed on a v1.4.x peer will be deleted when the peer is deleted.
{: important}

<blockchain-sw-251>
If you are using OpenShift, you have the option to use either the kubectl CLI (which is native to Kubernetes), or the OpenShift cluster (oc) CLI. The commands should be largely the same, except that OpenShift uses "projects" instead of "namespaces". If you are running any cluster type other than OpenShift, you will have to use the kubectl CLI. In this topic, we'll use both CLIs.
{: tip}
</blockchain-sw-251>


Because deployments are organized in Kubernetes by their "namespace", you need to know your Kubernetes cluster namespace before you can delete components using the kubectl CLI. From the console, open any CA node and click the **Info and Usage** icon. View the value of the API URL. For example: https://nf85a2a-soorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054. The namespace is the first part of the URL beginning with the letter `n` and followed by a random string of six alphanumeric characters. So in the example above the value of the namespace is `nf85a2a`.


If you want to delete all of your smart contract pods, you can issue this command:


```
kubectl get po -n <NAMESPACE> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl delete po {} -n <NAMESPACE>
```
{:codeblock}


<blockchain-sw-251>
```
kubectl get po -n <PROJECT_NAME> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl delete po {} -n <PROJECT_NAME>
```
{:codeblock}

Where `<PROJECT_NAME>` is the name of your OpenShift project.
</blockchain-sw-251>

If you want to delete a single smart contract pod, you will first have to figure out the name of your smart contract pod.


First, get a list of all of the smart contract pods running in your cluster:

```
kubectl get po -n <NAMESPACE> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl get po {} -n <NAMESPACE> --show-labels
```
{:codeblock}

Replacing `<NAMESPACE>` with the name of your cluster namespace.

You should see results similar to:

```
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4   1/1     Running   0          14s   chaincode-id=javacc-1.1,peer-id=org1peer1
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-f3cc736f-94ef-454d-8da3-362a50c653d9   1/1     Running   0          4m    chaincode-id=nodecc-1.1,peer-id=org1peer1
```

Your smart contract name and version is visible next to the chaincode-id.


<blockchain-sw-251>
First, get a list of all of the smart contract pods running in your cluster:

```
kubectl get po -n <PROJECT_NAME> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl get po {} -n <PROJECT_NAME> --show-labels
```

You should see results similar to:

```
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4   1/1     Running   0          14s   chaincode-id=javacc-1.1,peer-id=org1peer1
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-f3cc736f-94ef-454d-8da3-362a50c653d9   1/1     Running   0          4m    chaincode-id=nodecc-1.1,peer-id=org1peer1
```

Your smart contract name and version is visible next to the chaincode-id.
</blockchain-sw-251>


To delete a single pod, issue this command, substituting the `<POD_NAME>` for the name of your pod, for example the smart contract pod `chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4`, as well as your `<NAMESPACE>`:

```
kubectl delete pod <POD_NAME> -n <NAMESPACE>
```
{:codeblock}


<blockchain-sw-251>
To delete a single pod, issue this command, substituting the `<POD_NAME>` for the name of your pod, for example the smart contract pod `chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4`, as well as your `<PROJECT_NAME>`:

```
oc delete pod <POD_NAME> -n <PROJECT_NAME>
```
{:codeblock}
</blockchain-sw-251>


You can also use kubectl commands to delete all of the nodes in your cluster by issuing commands to delete each type of node. First, set the namespace where the nodes you want to delete are located:

```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{:codeblock}


<blockchain-sw-251>
If you cannot use your console or the APIs to remove your nodes, you can manually remove all of the nodes from your cluster by using the OpenShift CLI. Navigate to your OpenShift Project:

```
oc project <PROJECT_NAME>
```
{:codeblock}
</blockchain-sw-251>

Then run the following commands to delete all of your blockchain nodes:

```
kubectl delete ibpca --all
kubectl delete ibppeer --all
kubectl delete ibporderer --all
```
{:codeblock}

You may also choose to only delete all of a single type of node within a namespace, for example, by only issuing `kubectl delete ibppeer --all`.

Note that if you delete your entire <blockchain-sw-251>project</blockchain-sw-251>namespace, your smart contract pods will also be deleted. Your smart contract pods will also be deleted, along with your nodes, if you delete your service instance using the Kubernetes dashboard in {{site.data.keyword.cloud_notm}}.
{: tip}


## Step two: Delete the {{site.data.keyword.blockchainfull_notm}} Platform operator
{: #Removing-ocp-step-two}

You can use the OpenShift CLI to remove the {{site.data.keyword.blockchainfull_notm}} Platform operator. When the operator is no longer running on your cluster, you can delete the console and any remaining nodes without them being restarted.

1. Log in to your cluster by using the OpenShift CLI. If you are using the OpenShift web console, you can log in to your cluster by clicking the dropdown menu in the upper right corner and then clicking **Copy Login Command**. Paste the copied command in your command terminal.

2. Use the CLI to switch to the OpenShift project that you created for your blockchain network:

  ```
  oc project <PROJECT_NAME>
  ```
  {:codeblock}

3. You can remove the operator deployment using the OpenShift CLI:

  ```
  kubectl delete deployment ibp-operator
  ```
  {:codeblock}

## Step Three: Delete the {{site.data.keyword.blockchainfull_notm}} Platform console
{: #Removing-ocp-step-three}

After you remove the operator, you can then delete the console without it being restarted. When you are logged in to your cluster and are operating from your project, you can delete the console by issuing the following command:

```
kubectl delete ibpconsole --all
```
{:codeblock}

## Step Four: Remove policies and secrets
{: #Removing-ocp-step-four}

If you are done working with the {{site.data.keyword.blockchainfull_notm}} Platform on your OpenShift cluster, you can delete your OpenShift project.

```
oc delete project <PROJECT_NAME>
```
{:codeblock}

This command deletes any remaining blockchain nodes that are running on the project, in addition to your console and the {{site.data.keyword.blockchainfull_notm}} Platform operator.
{:important}

Deleting the OpenShift project removes the {{site.data.keyword.blockchainfull_notm}} Platform Security Context Constraint, clusterRole, and ClusterRoleBinding that you applied. It deletes the secrets that you created. You cannot undo this action. As a result, deleting your project is not recommended unless you are not going to deploy another instance of the platform.
