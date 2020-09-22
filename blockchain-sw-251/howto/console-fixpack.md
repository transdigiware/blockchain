---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: Kubernetes, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters, fix pack, multicloud

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

# Installing the 2.5.1 fix pack
{: #install-fixpack}

Use these instructions if you have already installed or upgraded to the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 before September 9, 2020 and want to apply the latest 2.5.1 fix pack. This fix pack contains important bug fixes and should be applied to your network as soon as possible.  If you install the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1 after September 9, 2020, the platform will contain all the bug fixes and improvements included in this fix pack, and you do not need to apply it.
{:shortdesc}

You can install the fix pack by updating the {{site.data.keyword.blockchainfull_notm}} Platform deployment on your Kubernetes cluster to pull the latest images from the {{site.data.keyword.IBM_notm}} entitlement registry. You can apply the fix pack by using the following steps:

1. [Update the {{site.data.keyword.blockchainfull_notm}} Platform operator](#install-fixpack-operator)
2. [Update the {{site.data.keyword.blockchainfull_notm}} console](#install-fixpack-console)
3. [Update your blockchain nodes](#install-fixpack-nodes)

You can use these steps if you deployed the platform on the OpenShift Container Platform, open source Kubernetes, or distributions such as Rancher. You will need to apply the fix pack for each 2.5.1 network that runs on a separate namespace. If you experience any problems, see the instructions for [rolling back the fix pack installation](#install-fixpack-rollback).  If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Installing the 2.5.1 fix pack behind a firewall](#install-fixpack-firewall). You can install the fix pack without disrupting a running network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the process.

## What this fix pack contains
{: #install-fixpack-contents}

This fix pack contains security patches for the Fabric images and miscellaneous bug fixes.

## Before you begin
{: #install-fixpack-begin}

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8#deploy-k8-entitlement-key) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

## Step one: Update the {{site.data.keyword.blockchainfull_notm}} operator
{: #install-fixpack-operator}

You can start applying the fix pack to your network by updating the {{site.data.keyword.blockchainfull_notm}} operator. Log in to your cluster by using the kubectl CLI. You will need to provide the name of the Kubernetes namespace that you created to deploy your {{site.data.keyword.blockchainfull_notm}} network. If you deployed your network on Kubernetes, you can use the `kubectl get namespace` command to find the name of your namespace. If you deployed the platform on the OpenShift Container Platform, log in to your cluster using the oc CLI. You can find the name of your OpenShift project using the `oc get project` command. Use the project name as the value for `<namespace>`.

Run the following command to download the operator deployment spec to your local file system. The default name of the operator deployment is `ibp-operator`. If you changed the name during the deployment process, you can use the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl get deployment ibp-operator -n <namespace> -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-fixpack.yaml`. You need to update the `image:` field with the new operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibp-operator:2.5.0-20200825-amd64
```
{:codeblock}

Save the file on your local system. You can then issue the following command to apply the fix to the {{site.data.keyword.blockchainfull_notm}} operator:
```
kubectl apply -f operator-fixpack.yaml
```
{:codeblock}

You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that new image was added to your deployment.

After you apply the `operator-fixpack.yaml` operator spec to your namespace, the operator will restart and pull the latest operator image. While the operator is restarting, you can still access your console UI. However, you cannot use the console to install and instantiate smart contracts, or use the console or the APIs to create or remove a node.

You can check that the fix was applied successfully by running the `kubectl get deployment ibp-operator` command. If the upgrade is successful, then you can see the following table with four ones displayed.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           1m
```

If you experience a problem while you are updating the operator, go to this [troubleshooting topic](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

## Step two: Update the {{site.data.keyword.blockchainfull_notm}} console
{: #install-fixpack-console}

After you update to the {{site.data.keyword.blockchainfull_notm}} operator, you need to apply the fix pack to your console. You can update your console by removing the original ConfigMap and deployment spec that was created when the console was deployed. Removing these artifacts will allow your console to pull the latest configuration and images from the updated operator.

You can start by running the following command to delete the ConfigMap used by the console deployer. The default name of the ConfigMap is `ibpconsole-deployer`. If you changed the name of the console during the deployment process, you can use the `kubectl get configmap -n <namespace>` command to get the list of ConfigMaps on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl delete configmap -n <namespace> ibpconsole-deployer
```
{:codeblock}

You can then delete the console deployment. The default name of the console deployment is `ibpconsole`, unless you changed the name during the deployment process. You can find the name of your console deployment by using the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Delete the deployment using the following command:
```
kubectl delete deployment -n <namespace> ibpconsole
```
{:codeblock}

After you delete the console deployment and ConfigMap, the console will restart and download the new images and configuration settings provided by the 2.5.1 fix pack from the updated operator. You can use the following commands to confirm that the console has been updated with the latest images and configuration. The new images used by the console and your blockchain nodes will have the tags with the date `20200825`.
```
kubectl get deployment -n <namespace> ibpconsole -o yaml
kubectl get configmap -n <namespace> ibpconsole-deployer -o yaml
```
{:codeblock}

You can check that the upgrade is complete by running `kubectl get deployment ibpconsole`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibpconsole     1/1       1            1           1m
```

## Step three: Apply fixes to your blockchain nodes
{: #install-fixpack-nodes}

After you apply the fix to your console, you need to use your console UI to update the nodes of your blockchain network. Browse to the console UI and open the nodes overview tab. To apply a patch to a node, open the node tile and click the **Install patch** button. You cannot patch nodes that you imported to the console.

Apply patches to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, you need to ensure that another node of the same type is available to process requests whenever possible. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:important}

## Rolling back the fix pack installation
{: #install-fixpack-rollback}

When you apply the fix pack to your operator, it saves the secrets, deployment spec, and network information of your deployment before it restarts and attempts to apply the fixes to the console. If the fix pack installation fails for any reason, {{site.data.keyword.IBM_notm}} Support can roll back your upgrade and restore your previous deployment by using the information on your cluster. If you need to roll back your upgrade, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/node/1072956){: external} page.

You can roll back an upgrade after you use the console to operate your network. However, after you use the console to upgrade your blockchain nodes, you can no longer roll back your console to a previous version of the platform.

## Installing the 2.5.1 fix pack behind a firewall
{: #install-fixpack-firewall}

If you deployed the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without access to the external internet, you can install the  2.5.1 fix pack by using the following steps:

1. [Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images](#install-fixpack-firewall)
2. [Update the {{site.data.keyword.blockchainfull_notm}} Platform operator](#install-fixpack-operator-firewall)
3. [Update the {{site.data.keyword.blockchainfull_notm}} console](#install-fixpack-console-firewall)
4. [Update your blockchain nodes](#install-fixpack-nodes-firewall)

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

### Before you begin
{: #install-fixpack-begin-firewall}

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8-firewall#deploy-k8-entitlement-key-firewall) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

### Step one: Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images
{: #install-fixpack-images-firewall}

To apply the fix to your network, you need to download the latest set of {{site.data.keyword.blockchainfull_notm}} Platform images and push them to a Docker registry that you can access from behind your firewall.

Use the following command to log in to the {{site.data.keyword.IBM_notm}} entitlement registry:
```
docker login --username cp --password <KEY> cp.icr.io
```
{:codeblock}

- Replace `<KEY>` with your entitlement key.

After you log in, use the following command to pull the images for {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1.

If you are deploying on OpenShift Container Platform on LinuxONE, you need to replace `amd64` with `s390x` throughout these instructions.
{: important}

```
docker pull cp.icr.io/cp/ibp-operator:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-init:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-console:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-grpcweb:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-deployer:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-fluentd:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-couchdb:2.3.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-peer:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-orderer:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-ca:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-dind:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-utilities:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-peer:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-orderer:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-chaincode-launcher:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-utilities:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-ccenv:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-goenv:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-nodeenv:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-javaenv:2.1.1-20200825-amd64
docker pull cp.icr.io/cp/ibp-crdwebhook:2.5.0-20200825-amd64
docker pull cp.icr.io/cp/ibp-ccenv:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-goenv:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-nodeenv:1.4.7-20200825-amd64
docker pull cp.icr.io/cp/ibp-javaenv:1.4.7-20200825-amd64
```
{:codeblock}

After you download the images, you must change the image tags to refer to your Docker registry. Replace `<LOCAL_REGISTRY>` with the URL of your local registry and run the following commands:
```
docker tag cp.icr.io/cp/ibp-operator:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-operator:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-init:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-init:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-console:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-console:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-grpcweb:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-grpcweb:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-deployer:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-deployer:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-fluentd:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-fluentd:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-couchdb:2.3.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-couchdb:2.3.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-peer:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-peer:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-orderer:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-orderer:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-ca:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-ca:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-dind:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-dind:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-utilities:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-utilities:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-peer:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-peer:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-orderer:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-orderer:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-chaincode-launcher:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-chaincode-launcher:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-utilities:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-utilities:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-ccenv:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-ccenv:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-goenv:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-goenv:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-nodeenv:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-nodeenv:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-javaenv:2.1.1-20200825-amd64 <LOCAL_REGISTRY>/ibp-javaenv:2.1.1-20200825-amd64
docker tag cp.icr.io/cp/ibp-crdwebhook:2.5.0-20200825-amd64 <LOCAL_REGISTRY>/ibp-crdwebhook:2.5.0-20200825-amd64
docker tag cp.icr.io/cp/ibp-ccenv:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-ccenv:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-goenv:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-goenv:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-nodeenv:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-nodeenv:1.4.7-20200825-amd64
docker tag cp.icr.io/cp/ibp-javaenv:1.4.7-20200825-amd64 <LOCAL_REGISTRY>/ibp-javaenv:1.4.7-20200825-amd64
```
{:codeblock}

You can use the `docker images` command to check that the new tags were added. You can then push the images with the new tags to your Docker registry. Log in to your registry by using the following command:
```
docker login --username <USER> --password <LOCAL_REGISTRY_PASSWORD> <LOCAL_REGISTRY>
```
{:codeblock}

- Replace `<USER>` with your username
- Replace `<LOCAL_REGISTRY_PASSWORD>` with the password to your registry.
- Replace `<LOCAL_REGISTRY>` with the URL of your local registry.

Then, run the following command to push the images. Replace `<LOCAL_REGISTRY>` with the URL of your local registry.
```
docker push <LOCAL_REGISTRY>/ibp-operator:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-init:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-console:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-grpcweb:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-deployer:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-fluentd:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-couchdb:2.3.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-peer:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-orderer:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-ca:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-dind:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-utilities:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-peer:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-orderer:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-chaincode-launcher:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-utilities:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-ccenv:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-goenv:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-nodeenv:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-javaenv:2.1.1-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-crdwebhook:2.5.0-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-ccenv:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-goenv:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-nodeenv:1.4.7-20200825-amd64
docker push <LOCAL_REGISTRY>/ibp-javaenv:1.4.7-20200825-amd64
```
{:codeblock}

After you complete these steps, you can use the following instructions to deploy the {{site.data.keyword.blockchainfull_notm}} Platform with the images in your registry.

### Step two: Update the {{site.data.keyword.blockchainfull_notm}} operator
{: #install-fixpack-operator-firewall}

You can start applying the fix pack to your network by updating the {{site.data.keyword.blockchainfull_notm}} operator. Log in to your cluster by using the kubectl CLI. You will need to provide the name of the Kubernetes namespace that you created to deploy your {{site.data.keyword.blockchainfull_notm}} network. If you deployed your network on Kubernetes, you can use the `kubectl get namespace` command to find the name of your namespace. If you deployed the platform on the OpenShift Container Platform, log in to your cluster using the `oc` Cli. You can find the name of your OpenShift project using the `oc get project` command. Use the project name as the value for `<namespace>`.

Run the following command to download the operator deployment spec to your local file system. The default name of the operator deployment is `ibp-operator`. If you changed the name during the deployment process, you can use the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl get deployment ibp-operator -n <namespace> -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-fixpack.yaml`. You need to update the `image:` field with the new operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibp-operator:2.5.0-20200825-amd64
```
{:codeblock}

Save the file on your local system. You can then issue the following command to apply the fix to the {{site.data.keyword.blockchainfull_notm}} operator:
```
kubectl apply -f operator-fixpack.yaml
```
{:codeblock}

You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that new image was added to your deployment.

After you apply the `operator-fixpack.yaml` operator spec to your namespace, the operator will restart and pull the latest operator image. While the operator is restarting, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the fix was applied successfully by running the `kubectl get deployment ibp-operator` command. If the upgrade is successful, then you can see the following table with four ones displayed.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1/1       1            1           1m
```

If you experience a problem while you are updating the operator, go to this [troubleshooting topic](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

### Step three: Update the {{site.data.keyword.blockchainfull_notm}} console
{: #install-fixpack-console-firewall}

After you update to the {{site.data.keyword.blockchainfull_notm}} operator, you need to apply the fix pack to your console. You can update your console by removing the original ConfigMap and deployment spec that was created when the console was deployed. Removing these artifacts will allow your console to pull the latest configuration and images from the updated operator.

You can start by running the following command to delete the ConfigMap used by the console deployer. The default name of the deployer ConfigMap is `ibpconsole-deployer`. If you changed the name of the console during the deployment process, you can use the `kubectl get configmap -n <namespace>` command to get the list of ConfigMaps on your namespace. Replace `<namespace>` with the name of your namespace or OpenShift project:
```
kubectl delete configmap -n <namespace> ibpconsole-deployer
```
{:codeblock}

You can then delete the console deployment. The default name of the console deployment is `ibpconsole`, unless you changed the name during the deployment process. You can find the name of your console deployment by using the `kubectl get deployment -n <namespace>` command to get the name of the deployments on your namespace. Delete the deployment using the following command:
```
kubectl delete deployment -n <namespace> ibpconsole
```
{:codeblock}

After you delete the console deployment and ConfigMap, the console will restart and download the new images and configuration settings provided by the 2.5.1 fix pack from the updated operator. You can use the following commands to confirm that the console has been updated with the latest images and configuration. The new images used by the console and your blockchain nodes will have the tags with the date `20200825`.
```
kubectl get deployment -n <namespace> ibpconsole -o yaml
kubectl get configmap -n <namespace> ibpconsole-deployer -o yaml
```
{:codeblock}

You can check that the upgrade is complete by running `kubectl get deployment ibpconsole`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           READY     UP-TO-DATE   AVAILABLE   AGE
ibpconsole     1/1       1            1           1m
```

If your console experiences an image pull error, you may need to update the console deployment spec with local registry that you used to download the images. Run the following command to download the deployment spec of the console:
```
kubectl get ibpconsole ibpconsole -o yaml > console.yaml
```
{:codeblock}

Then add the URL of your local registry to the `spec:` section of `console.yaml`. Replace `<LOCAL_REGISTRY>` with the URL of your local registry:
```
spec:
  registryURL: <LOCAL_REGISTRY>
```
{:codeblock}

Save the updated file as `console-upgrade.yaml` on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-fixpack.yaml
```
{:codeblock}

### Step four: Update your blockchain nodes
{: #install-fixpack-nodes-firewall}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#install-fixpack-nodes).
