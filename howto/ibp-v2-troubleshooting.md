---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-13"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

subcollection: blockchain
content-type: troubleshoot

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:support: data-reuse='support'}
{:troubleshoot: data-hd-content-type='troubleshoot'}


# Troubleshooting
{: #ibp-v2-troubleshooting}

<blockchain-sw-251><div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-ibp-v2-troubleshooting">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-ibp-v2-troubleshooting">2.1.3</a>,
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-v2-troubleshooting">2.5</a>
    </p>
</div></blockchain-sw-251>

General problems can occur when you use the console to manage nodes, channels, or smart contracts. In many cases, you can recover from these problems by following a few easy steps.
{:shortdesc}

This topic describes common issues that can occur when you use the {{site.data.keyword.blockchainfull_notm}} Platform console.  <blockchain-sw-251>

**Issues during Deployment**
- [My deployment fails when I try apply the security and access policies to my namespace](#ibp-v2-troubleshooting-deployment-policies)
- [My deployment fails when I try apply the custom resource definition of the console or operator](#ibp-v2-troubleshooting-deployment-cr)
- [Extracting the TLS certificate from the Kubernetes webhook fails](#ibp-v2-troubleshooting-wh-extract)</blockchain-sw-251>

**Issues with the Console**<blockchain-sw-251>
- [Why is my console upgrade from 2.5 to 2.5.1 failing?](#ibp-v2-troubleshootingconsole-upgrade-fails)</blockchain-sw-251>
- [Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?](#ibp-v2-troubleshooting-chrome-v77)<blockchain-sw-251>
- [Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?](#ibp-v2-troubleshooting-console-catalina)
- [Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?](#ibp-v2-troubleshooting-accept-tls)</blockchain-sw-251>
- [When I hover over my node, the status is `Status unavailable`, what does this mean?](#ibp-v2-troubleshooting-status-unavailable)
- [When I hover over my node, the status is `Status undetectable`, what does this mean?](#ibp-v2-troubleshooting-status-undetectable)
- [Why am I getting the error `Unable to get system channel` when I open my ordering service?](#ibp-troubleshoot-ordering-service)
- [Why did my smart contract installation, instantiation or upgrade fail?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Why is my smart contract installation failing with an error on my peer?](#ibp-v2-troubleshooting-sc-install)<blockchain-sw-251>
- [Why is my Node.js smart contract instantiation failing?](#ibp-v2-troubleshooting-nodejs-instantiate)</blockchain-sw-251>
- [Why is my Node.js smart contract endorsement failing?](#ibp-v2-troubleshooting-nodejs-endorsement)
- [Why is the smart contract that I installed on the peer not listed in the UI?](#ibp-console-build-network-troubleshoot-missing-sc)
- [My channel, smart contracts, and identities have disappeared from the console. How can I get them back?](/docs/blockchain?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?](#ibp-v2-troubleshooting-create-msp)
- [Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?](#ibp-v2-troubleshooting-update-channel)
- [When I log in to my console, why am I getting a 401 unauthorized error?](#ibp-v2-troubleshooting-console-401)
- [Why am I getting a `Cluster linking is taking too long` error when I try to link my Kubernetes cluster in {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance?](#ibp-v2-troubleshooting-console-helm-reset)
- [Why am I getting an error “all SubConns are in TransientFailure” on the console?](#ibp-console-transientfailure)

**Issues with your Nodes**  

- [Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?](#ibp-v2-troubleshooting-smart-contract-anchor-peers)
- [Why are my node operations failing after I create my peer or ordering service?](#ibp-console-build-network-troubleshoot-entry1)
- [Why does my peer or ordering node fail to start?](#ibp-console-build-network-troubleshoot-entry2)
- [Why does my blockchain node fail to restart?](#ibp-v2-troubleshooting-restart)
- [What is the proper way to clean up a failed node deployment?](#ibp-v2-troubleshooting-cleanup)
- [How can I view my smart contract container logs?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Why is my CA, peer, or ordering node that is configured to use HSM not working?](#ibp-v2-troubleshooting-hsm-proxy)<blockchain-sw-251>
- [My CA failed to upgrade, how can I fix it?](#ibp-v2-troubleshooting-ca-upgrade-fails)</blockchain-sw-251>
- [Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?](#ibp-v2-troubleshooting-endorsement-sig-failure)
- [Why are the transactions I submit from VS Code failing with a No endorsement plan available error?](#ibp-v2-troubleshooting-anchor-peer)
- [Why are the transactions I submit from VS Code failing with an endorsement failure?](#ibp-v2-troubleshooting-endorsement)
- [How do I delete a peer pod?](#ibp-troubleshooting-delete-peer)
- [How can I recover a contract after a failed upgrade of the smart contract container?](#ibp-troubleshooting-contract-fail)

**Issues on {{site.data.keyword.cloud_notm}}**  

- [My Kubernetes cluster on {{site.data.keyword.cloud_notm}} expired. What does this mean?](#ibp-v2-troubleshooting-cluster-expired)
- [After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has unbound immediate persistent volume claims. Is this an error?](#ibp-v2-troubleshooting-unbound-persistent-volume-claim)
- [After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff. Is this an error?](#ibp-v2-troubleshooting-crash-loop-backoff)

<blockchain-sw-251>
## My deployment fails when I try apply the security and access policies to my namespace
{: #ibp-v2-troubleshooting-deployment-policies}
{: troubleshoot}

When I try to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.2 and apply the Security Context Constraint, clusterRole, or ClusterRoleBinding to my namespace, I encounter one of the following errors.

When I apply the file, I receive a user forbidden error:
{: tsSymptoms}

```
securitycontextconstraints.security.openshift.io "e-block2" is forbidden: User cannot get securitycontextconstraints.security.openshift.io at the cluster scope: no RBAC policy matched
```

This problem occurs when you are not a cluster administrator. You must have a cluster administrator role and apply the security and access policies to your namespace.
{: tsCauses}

When I try to apply resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing ibp-scc.yaml: error converting YAML to JSON: yaml: line 10: mapping values are not allowed in this context
```

This error happens when a problem exists with the indents in your YAML file. Refer to the documentation for the correct format for the security and access files.
{: tsCauses}


## My deployment fails when I try apply the custom resource definition of the console or operator
{: #ibp-v2-troubleshooting-deployment-cr}
{: troubleshoot}

When I try to deploy the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.2 and apply the custom resource definition of the operator or the console, I encounter one of the following errors:

When I apply the custom resource file, I receive an image pull or image pull back-off error:
{: tsSymptoms}

```
NAME                            READY     STATUS             RESTARTS   AGE
ibp-operator-769d94ffbc-w52n6   0/1       ImagePullBackOff   0          32s
```

This problem occurs when your deployment cannot pull the {{site.data.keyword.blockchainfull_notm}} Platform images from the {{site.data.keyword.IBM_notm}} Entitlement registry. This can happen because you provided an incorrect name of your secret to the `imagePullSecrets:` field, or if there was a problem with the URL or the key that you provided to the entitlement key secret. This error can also occur if you supplied the wrong image tag to the file.
{: tsCauses}

When I try to apply the custom resource file, I receive a parsing error:
{: tsSymptoms}

```
error: error parsing console.yaml: error converting YAML to JSON: yaml: line 11: mapping values are not allowed in this context
```

This error happens when a problem exists with the indents in your file. Refer to the documentation for the correct format for the custom resource files of the console and operator.
{: tsCauses}

## Extracting the TLS certificate from the Kubernetes webhook fails
{: #ibp-v2-troubleshooting-wh-extract}
{: troubleshoot}

When deploying or upgrading to the {{site.data.keyword.blockchainfull_notm}} Platform, after I deploy the webhook, I am unable to successfully extract the extract the TLS certificate that was generated by the webhook deployment. The following command fails:
```
kubectl get secret webhook-tls-cert -n ibpinfra -o json | jq -r .data.\"cert.pem\"
```
{: tsSymptoms}

This problem can be occur when the entitlement `key`, that you specified in the `docker-key-secret` for the `ibpinfra` namespace or project, is invalid. Because only one entitlement key can be used per deployment, you  need to refresh the key if it was already used for a different deployment.
{: tsCauses}

To resolve this problem, repeat the steps you followed to [Get your entitlement key](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp#deploy-ocp-entitlement-key) and then delete the kubernetes `docker-key-secret` in the `ibpinfra` namespace or project and recreate it using the refreshed key value. Then rerun the command to extract the TLS certificate that was generated by the webhook deployment.
{: tsResolve}</blockchain-sw-251>

<blockchain-sw-251>
## Why is my console upgrade from 2.5 to 2.5.1 failing?
{: #ibp-v2-troubleshootingconsole-upgrade-fails}
{: troubleshoot}

The console upgrade fails with
`Unable to attach or mount volumes: unmounted volumes=[couchdb], unattached volumes` and the console pod is stuck in the `init` state.
{: tsSymptoms}

This problem can occur when there is more than one console pod running in your cluster.
{: tsCauses}

{: tsResolve}
To resolve this problem, delete the console deployment by running the command:
```
kubectl delete deploy ibpconsole
```
{: codeblock}

This command deletes the console replicas and the operator then starts only one console replica which starts successfully.


</blockchain-sw-251>


## Why are my console actions failing in my Chrome browser Version 77.0.3865.90 (Official Build) (64-bit)?
{: #ibp-v2-troubleshooting-chrome-v77}
{: troubleshoot}

The console has been working successfully, but requests have started to fail. For example, after I create an ordering service and open it I see the error: `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details.`
{: tsSymptoms}

This problem can be caused by a [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=1006243){: external} introduced by the Chrome browser `Version 77.0.3865.90 (Official Build) (64-bit)` that causes actions from the browser to fail.
{: tsCauses}

To resolve this problem, open the console in a new browser tab in Chrome. Any identities that you saved in your console wallet will  persist in the new browser tab. To avoid this problem you can upgrade your Chrome browser version. Ensure you have downloaded all of your wallet identities to your local machine before closing your browser. <blockchain-sw-251>If this solution does not resolve your problem see [Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?](#ibp-v2-troubleshooting-accept-tls).</blockchain-sw-251>
{: tsResolve}

<blockchain-sw-251>
## Why am I not able to log in to the console from my Chrome browser on Mac OS Catalina?
{: #ibp-v2-troubleshooting-console-catalina}
{: troubleshoot}

The console has been working successfully, but after I upgraded my Mac OS to Catalina, I can no longer log in to the console.
{: tsSymptoms}

There are three ways to resolve this problem:
{: tsResolve}
1.  Use a different [supported browser](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp-getting-started#deploy-ocp-browsers) with Catalina.
2. Use your own [TLS certificates when deploying on OpenShift Contain Platform](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp#console-deploy-ocp-use-your-own-tls-certificates-optional) or [TLS certificates when deploying on Kubernetes](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-k8#deploy-k8-tls).
3. Run the following commands to generate a new key and certificate pair for the console that will fix the problem.
      - Run the following command to get the pod that corresponds to the console:
        ```
        kubectl get po
        ```
        {: codeblock}
      - Exec into the pod by running the command:
        ```
        kubectl get po <pod-name> -c optools bash
        ```
        {: codeblock}
      - Delete the console key and certificate by running the command:
        ```
        rm -f /certs/tls.key rm -f /certs/tls.crt
        ```
        {: codeblock}
      - Delete the console pod which causes it to restart by running the command:
        ```
        kubectl delete po <pod-name>
        ```
        {: codeblock}
   When the pod restart completes, you should now be able to log in to your console URL from a Chrome Browser.

## Why is my channel creation failing or I am unable to add a new organization to my ordering service with the error "Unable to get system channel"?
{: #ibp-v2-troubleshooting-accept-tls}
{: troubleshoot}

If you are not using your own TLS certificates to secure communications in your blockchain network, you need to accept the self-signed certificate that was generated for you. Otherwise, when you try to create a channel or add an organization to an ordering service the action fails. Channel creation fails with the error `An error occurred when creating channel. submit config update failed: grpc code ???= response contains no code or message`. Or, when you click on your ordering service, you see `Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node, you will not be able to view or manage ordering service details`.
{: tsSymptoms}

This problem occurs when the blockchain console is deployed without the advanced deployment option to [use your own TLS certificates](/docs/blockchain-sw-251?topic=blockchain-sw-251-deploy-ocp#console-deploy-ocp-use-your-own-tls-certificates-optional).
{: tsCauses}

To resolve this problem, you need to accept the self-signed certificate in your browser.
{: tsResolve}

1. Copy your console URL, for example `https://<PROJECT-NAME>-ibpconsole-console.abc.xyz.com:443`, where the value of `abc.xyz.com` depends on your cloud provider.
2. Replace `-console` with `-proxy`. The new URL looks similar to: `https://<PROJECT-NAME>-ibpconsole-proxy.abc.xyz.com:443`.
3. Open a new tab in your browser and paste in the new URL.
4. Accept the certificate.

You need to accept the certificate from this URL to communicate with your nodes from your console and then log in as usual. When you switch to a new machine or a new browser, you need to repeat these steps.
</blockchain-sw-251>

## When I hover over my node, the status is `Status unavailable` or `Status unknown`, what does this mean?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

A CA, peer, or ordering node has a gray status box, meaning the status of the node is not available. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This problem can occur if the node is newly created and the deployment process has not completed. If the node is a CA and the status has been gray for more than a few minutes, then it is likely that the deployment process has failed. Peers and ordering nodes take longer to deploy, but this condition can also occur when the health checker that runs against the peer or ordering nodes cannot contact the node. The request for status can fail with a timeout error because the node did not respond within a specific time period, the node could be down, or network connectivity is down.
{: tsCauses}

If this is a new node, wait a few more minutes for the deployment to complete. You can try reloading the page in your browser to refresh the status. If the node is not new,
[examine the associated node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) <blockchain-sw-251>[examine the associated node logs](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-icp-manage#console-icp-manage-console-logs)</blockchain-sw-251> for errors to determine the cause.
{: tsResolve}

## When I hover over my node, the status is `Status undetectable`, what does this mean?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}
{: support}

The node status in the tile for the peer or ordering node is yellow, meaning the status of the node cannot be detected. Ideally, when you hover over any node, the node status should be `Running`.
{: tsSymptoms}

This condition can occur on peer and ordering nodes that were *imported* to the console and the health checker cannot run against the node. This status happens because an `operations_url` was not specified when the node was imported. An operations URL is required for the node health checker to run. The node itself is likely `Running`, but because the operations URL was not specified, its status cannot be determined. This problem can also occur if you migrated your {{site.data.keyword.cloud_notm}} Kubernetes cluster Ingress from {{site.data.keyword.containerlong_notm}} Ingress to the community Kubernetes Ingress image, or vis versa.
{: tsCauses}

{: tsResolve}
**If you imported the node:**  
You can resolve this problem by performing the following steps:
 1. Click the node tile to open it.
 2. Click the **Settings** icon.
 3. Click **Associate identity**, view and make note of the identity that is associated with this node. Click **Cancel** to close this panel.
 4. Click **Export** to generate a `JSON` file for the node.
 5. Edit the generated `JSON` file and enter the `operations_url` for the node. The value of the `operations_url` depends on how it was configured as well as various network settings. This value needs to be provided by the network administrator who deployed the node.
 6. Click **Delete**. This step removes the imported node from the console, but does not delete the actual node.
 7. From the **Nodes** tab, click **Add Peer** or **Add ordering service** followed by **Import an existing peer** or **Import an existing Ordering service**.
 8. Click **Upload JSON** and browse to the JSON file you just edited. Click **Next**.
 9. Associate the same identity you noted in step three.
 10. Click **Add peer** or **Add ordering service**.
The health checker can now run against the node and report the status of the node.

**If you migrated your cluster Ingress:**
You need to [refresh your blockchain console](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-refresh).


## Why am I getting the error `Unable to get system channel` when I open my ordering service?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

After you create an ordering service, the status is `Running`. But when you open the ordering service you see the error:
{: tsSymptoms}

```
Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node,
you will not be able to view or manage ordering service details.
```

This problem can occur when the console has lost contact with your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.blockchainfull_notm}} platform.
{: tsCauses}

You can renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster on {{site.data.keyword.cloud_notm}} by clicking on the service instance of your console in your **Resource list** and clicking the **Refresh cluster** button. When the link between your cluster and your console has been refreshed, click the **Launch the IBM Blockchain Platform** button.
{: tsResolve}


## Why did my smart contract installation, instantiation or upgrade fail?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}
{: support}

It is possible you may experience an error when installing, instantiating or upgrading a smart contract. For example, when you try to install a smart contract on a peer, it fails with the error `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

You may receive this error if this version of the smart contract already exists on the peer, or if your peer is out of resources.
{: tsCauses}

- Open your Kubernetes dashboard and ensure the peer status is `Running`.
- Open the peer node and ensure the smart contract version does not already exist on the peer and try again with the proper version.
- If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) <blockchain-sw-251>[check your node logs](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-icp-manage#console-icp-manage-node-logs)</blockchain-sw-251> for errors.
{: tsResolve}

## Why is my smart contract installation failing with an error on my peer?
{: #ibp-v2-troubleshooting-sc-install}
{: troubleshoot}
{: support}

{: tsSymptoms}
Installing a smart contract on a peer fails with an error similar to the following:

```
An error occurred when installing smart contract on peer.
error in simulation: failed to execute transaction
421fac...2fda: error sending: timeout expired while executing transaction.
```

{: tsCauses}
When running the {{site.data.keyword.blockchainfull_notm}} Platform on s390x architecture, or in an environment with constrained resources, it is possible that smart contract installation can fail on a peer if the default timeout is too short on the peer that is running the Fabric v2x image.

In some cases, if you simply wait several minutes and then refresh the **Smart contracts** tab in the console, you can see that the smart contract was successfully installed. You can customize how long the console waits for the installation to complete by changing the console settings with the [APIs](/apidocs/blockchain#edit-settings). See the `fabric_lc_install_cc_timeout_ms` setting. In some cases, it is the peer itself that is timing out and you need to override the peer configuration to extend its time out.
{: tsResolve}

1. From the console, open the peer tile and click the **Settings** icon.
2. Click **Edit configuration JSON (Advanced)**.
3. In the **Configuration updates** box paste the following text and click **Update peer**.
  ```
  {
   "chaincode":{
      "startuptimeout":"300s",
      "installTimeout":"600s",
      "executetimeout":"30s"
    }
  }
  ```
  {: codeblock}

The peer restarts and then you can retry the smart contract installation. Because the original installation failed you need to specify a new smart contract name and version. <blockchain-sw-251>

## Why is my Node.js smart contract instantiation failing?
{: #ibp-v2-troubleshooting-nodejs-instantiate}
{: troubleshoot}

{: tsSymptoms}
Instantiating a Node.js smart contract fails with the timeout error:
```
[endorser] SimulateProposal -> ERRO 0ba [channel2][37876c5f] failed to invoke chaincode name:"lscc" , error: timeout expired while starting chaincode myassetc:0.0.1 for transaction
github.com/hyperledger/fabric/core/chaincode.(*RuntimeLauncher).Launch
	/go/src/github.com/hyperledger/fabric/core/chaincode/runtime_launcher.go:75
```

{: tsCauses}
When running the {{site.data.keyword.blockchainfull_notm}} Platform on s390x architecture, it is possible that Node.js smart contract instantiation can fail if the default timeout is too short.

{: tsResolve}
Customers should wait for five minutes after the failure occurs and then retry the instantiation again. It will then work successfully on the subsequent attempt.</blockchain-sw-251>

## Why is my Node.js smart contract endorsement failing?
{: #ibp-v2-troubleshooting-nodejs-endorsement}
{: troubleshoot}
{: support}

{: tsSymptoms}
My Node.js smart contract endorsement fails with the error:
```
Error: endorsement failure during query. response: status:500 message:"error in simulation: failed to execute transaction 6cbcf9f94bdef3fc68abba5604e46293ae: could not launch chaincode nodecc:1.1: error building chaincode: error building image: external builder failed: external builder failed to build: external builder 'ibp-builder' failed: exit status 3"
```

{: tsCauses}
By default, a Fabric v1.4 peer creates a Node v8 runtime, and a Fabric v2.x peer creates a Node v12 runtime. In order for the smart contract to work with Node 12 runtime, the `fabric-contract-api` and `fabric-shim` node modules must be at v1.4.5 or greater.

{: tsResolve}
If you are using a smart contract that was originally written to work with Fabric 1.4, update the Node modules by running the following command before deploying the smart contract on a Fabric v2.x peer.  See [Support and Compatibility for fabric-chaincode-node](https://github.com/hyperledger/fabric-chaincode-node/blob/master/COMPATIBILITY.md) for more information.
```
npm install --save fabric-contract-api@latest-1.4 fabric-shim@latest-1.4
```
{: codeblock}


## Why is the smart contract that I installed on the peer not listed in the UI?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

A smart contract was installed on a peer but when you click on the **Smart contracts** tab, it is not listed.
{: tsSymptoms}

This issue can occur when another user or application installs the smart contract onto the peer and you do not have the peer admin identity in your browser wallet.
{: tsCauses}

In order to view the smart contracts installed on a peer, you need to be a peer admin. Ensure that the peer admin identity exists in your browser wallet. If it does not, you need to import it into your console wallet.
{: tsResolve}

## My nodes, channels, smart contracts, and identities have disappeared from the console. How can I get them back?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}
{: support}

Nodes, identities, channels, or smart contracts (or some combination of these) that I successfully deployed into the console are no longer available.
{: tsSymptoms}

One of the features of {{site.data.keyword.blockchainfull_notm}} Platform is that you are now responsible for securing and managing your certificates. Therefore, they are only persisted in the browser local storage to allow you to manage the component. If you are using a private browser window and then switch to another browser or non-private browser window, the identities that you created will be gone from your wallet in the new browser session. Therefore, it is required that you export the identities from the wallet in your private browser session to your file system. You can then import them into your non-private browser session if they are needed. Otherwise, there is no way to recover them.
{: tsCauses}

- Whenever you create a new organization MSP definition, you generate keys for an identity that is allowed to administer the organization. Therefore, during that process you must click the **Generate** and then **Export** buttons to store the generated identity in your wallet and then save it to your file system as a JSON file.
- To resolve this problem in your browser, you need to import those identities and associate them with the corresponding node:
  - In the browser where you are experiencing the problem, click the **Wallet** tab followed by **Add identity** to import the JSON file into your wallet.
  - Click **Upload JSON** and browse to the JSON file you exported using the **Add files** button.
  - Click **Submit**.
  - Now open the peer or ordering service node that this identity was originally associated to, and click on the **Settings** icon.
  - Click the **Associate identity** button.
  - Select the identity you just imported to your console wallet from the drop-down list.
  - Click **Associate**.
- Repeat this process for each identity that was in the wallet of the original browser.
{: tsResolve}

This problem can also occur when the console has lost contact with your Kubernetes cluster on {{site.data.keyword.cloud_notm}}. This can happen if your console has not recently communicated with your cluster, or if you have made changes to your cluster that have overridden the settings used by the {{site.data.keyword.blockchainfull_notm}} platform.
{: tsCauses}

You can renew the communication between your console and the {{site.data.keyword.IBM_notm}} Kubernetes service cluster on {{site.data.keyword.cloud_notm}} by clicking on the service instance of your console in your **Resource list** and clicking the **Refresh cluster** button. When the link between your cluster and your console has been refreshed, click the **Launch the IBM Blockchain Platform** button.
{: tsResolve}

## Why am I getting the error `Unable to authenticate with the enroll ID and secret you provided` when I create a new organization MSP definition?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}
{: support}

When you attempt to create a new organization MSP definition from the Organizations tab, you experience the error `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

This error occurs when the value that you specified for the enroll secret is not valid for the enroll ID that you selected in the `Generate organization admin certificate` section of the panel.
{: tsCauses}

Verify that you have selected the correct organization admin enroll ID from enroll ID drop down list and enter the correct value for the enroll secret.
{: tsResolve}

## Why am I getting the error `An error occurred when updating channel` when I try to add an organization to my channel?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

When you attempt to add another organization to a channel, the update fails with the message `An error occurred when updating channel`.
{: tsSymptoms}

This error occurs when the selected **Channel Updater MSP ID** on the **Update channel** panel is not an admin of the channel.
{: tsCauses}

On the **Update channel** panel, scroll down to the **Channel Updater MSP ID** and select the MSP ID that was specified when the channel was created or specify the MSP ID that is the admin of the channel.
{: tsResolve}

## When I log in to my console, why am I getting a 401 Unauthorized error?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}
{: support}

When I try to log in to my console, I am unable to access the console from my browser. If I check the browser logs, I can find the error 401 unauthorized.
{: tsSymptoms}

Your browser console session times out after **8 hours** of inactivity. If a session becomes inactive, the console will prevent the inactive user from performing any actions.
{: tsCauses}

If your session has become inactive, you can try simply refreshing your browser. If that does not work, close the browser including **all** tabs and windows. Open the URL again. You will be required to log in.

As a best practice, you should have already stored your certificates and identities on your file system. If you happen to be using an incognito window, all the certificates are deleted from the browser local storage when you close the browser. After you log in again you will need to re-import your identities and certificates.
{: note}

## Why am I getting a `Cluster linking is taking too long` error when I try to link my Kubernetes cluster in {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance?
{: #ibp-v2-troubleshooting-console-helm-reset}
{: troubleshoot}
{: support}

After attempting to link my Kubernetes cluster on {{site.data.keyword.cloud_notm}} to my {{site.data.keyword.blockchainfull_notm}} Platform service instance, it fails with the error `Cluster linking is taking too long`.
{: tsSymptoms}

This issue can occur when your cluster is busy processing other requests and does not respond to the linking request in a timely matter.
{: tsCauses}

To resolve this problem you can run the `helm reset` command to delete the tiller and then try to link your cluster again. The tiller is the helm mechanism that the blockchain deployer uses to set up components on your cluster.
{: tsResolve}
Run the following command from your {{site.data.keyword.cloud_notm}} CLI terminal:

```
bx api cloud.ibm.com
bx login
bx cs clusters
$(bx cs cluster-config <cluster_name> --export)
kubectl get deployments #test that the connection is working
helm reset
```
{: codeblock}

Attempt to link your cluster again. If it continues to fail, you should [Contact Support](/docs/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).



## Why am I getting an error “all SubConns are in TransientFailure” on the console?
{: #ibp-console-transientfailure}
{: troubleshoot}

The following error is shown on the console: "All SubConns are in TransientFailutre."
{: tsSymptoms}

An Out of Memory (OOM) situation can cause this error.
{: tsCauses}

To resolve this problem, you need to resize the peers and CouchDB containers to add more memory, such as 2000 MB memory each. After resizing the memory, [delete the peer pods](#ibp-troubleshooting-delete-peer) so they will be re-created. Then try the scenario again. See [Considerations when reallocating resources](/docs/blockchain?topic=blockchain-ibp-console-govern-components#ibp-console-govern-components-reallocate-resources) for more information.
{: tsResolve}

## Why is my first invoke of a smart contract returning the following error: no suitable peers available to initialize from?
{: #ibp-v2-troubleshooting-smart-contract-anchor-peers}
{: troubleshoot}
{: support}

When I try to invoke a smart contract from the Fabric SDK, the transaction fails and returns the following error:
{: tsSymptoms}

```
error: [Network]: _initializeInternalChannel: no suitable peers available to initialize from Failed to submit transaction: Error: no suitable peers available to initialize from
```

This error occurs if you have not configured an anchor peer on your channel. Unless you have manually updated your connection profile, your application needs to use the [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-2.2/discovery-overview.html){: external} feature to learn about the peers it needs to submit the transaction to.
{: tsCauses}

Use the following steps to [configure anchor peers on your channel](/docs/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers).
Also, you should verify that you are submitting the transactions against the correct channel and organization MSP id.
{: tsResolve}

## Why are my node operations failing after I create my peer or ordering service?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

It is possible you may experience an error when managing an existing node. When that occurs, it is often useful to consult the node logs.

For example, when you try to operate the node, the action might fail.
{: tsSymptoms}

After creating a new peer or ordering service, depending on your cluster storage configuration, it may take a few minutes for the nodes to be ready for operation.
{: tsCauses}


Check your Kubernetes dashboard and ensure the peer or node status is `Running`. Then try your action again. If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) for errors.  
{: tsResolve}


<blockchain-sw-251>
Check your Kubernetes dashboard and ensure the peer or node status is `Running`. Then try your action again. If you are still experiencing problems after the node is up, [check your node logs](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-icp-manage#console-icp-manage-node-logs) for errors.
{: tsResolve}
</blockchain-sw-251>

## Why does my peer or ordering node fail to start?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

It is possible to experience this error under a variety of conditions.

The peer log includes:
{: tsSymptoms}
```
[main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
```
or the ordering node log contains:

```
Failed to initialize local MSP: admin 0 is invalid [The identity does not contain OU [CLIENT], MSP: [orderermsp],The identity does not contain OU [ADMIN], MSP: [orderermsp]]
```

- This error can occur under the following conditions:
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that corresponds to an identity of type `peer` and not `client` or `admin`. It must be of type `client` or `admin`.
  - When you created the peer or ordering service organization MSP definition, you specified an enroll ID and secret that does not match the enroll ID or secret of the corresponding organization admin identity.
  - When you created the peer or ordering service, you specified the enroll ID and secret of an identity that is not type 'peer' or 'orderer'.
  - When you created the peer or ordering service, you associated an identity that does not have the `admin` role.

{: tsResolve}
- Open your peer or ordering service CA node and view the registered identities listed in the **Registered Users** table.
- Delete the peer or ordering service and recreate it, being careful to specify the correct enroll ID and secret of a user that has the `peer` or `orderer` role and associate an identity that has a role of `admin` with the node.
- Note that before you create the peer or ordering service, you need to register an organization admin user, with a type `admin`. Be sure to specify that same id as the enroll ID when you create the organization MSP definition. See these instructions for [registering peer identities](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) and these instructions for [registering orderer identities](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).

## Why does my blockchain node fail to restart?
{: #ibp-v2-troubleshooting-restart}
{: troubleshoot}

When you change the resources allocated to a CA, peer, or ordering node, or if you update the admin certificates for a node, the pod has to restart in order for the changes to take effect. Or when a worker node is updated for maintenance and needs to restart, the associated blockchain pods also need to be restarted. Sometimes, due to conditions on your Kubernetes cluster, the pod restart can fail.
{: tsSymptoms}

{: tsCauses}
A component can fail to restart for a variety of reasons:
- If another application in a cluster with constrained resources is in a pending state, and is prioritized higher than the blockchain component, the blockchain pod cannot restart because the resources are consumed by the pending application.
- If the blockchain component needs to restart, but is on a worker node that has been "cordoned", meaning no new components can be brought to that node, it cannot restart. A blockchain pod becomes unschedulable when the Kubernetes scheduler is unable to find a node that can accommodate the resource requirements of the pod.
- If there is a timeout between the kubelet, the node agent that runs on each node, and the Kubernetes master node, a pod restart can fail.
- The pod restart itself can time out when its worker node restarts and takes too long to mount the persistent volume claim (PVC).

{: tsResolve}
To enable the blockchain pod to restart successfully, you need to address the underlying cluster issues:
- Ensure there are adequate resources available on worker nodes in the cluster. If not, deploy a new worker node with increased resources.
- After you resolve the cluster issues, delete the failing blockchain pod. When Kubernetes restarts the pod, it will attempt to move the pod workload to another node with sufficient resources.
- If the timeout between a kubelet and the master node cannot be resolved, open a support ticket with your cluster provider.

## What is the proper way to clean up a failed node deployment?
{: #ibp-v2-troubleshooting-cleanup}
{: troubleshoot}

Sometimes a node can fail to deploy, for example, due to lack of resources in your Kubernetes cluster. After you understand the cause of the node deployment failure, you need to clean up the failed node in your cluster.
{: tsSymptoms}

Do not attempt to use Kubernetes commands to remove the node. Instead, it is extremely important that you use the {{site.data.keyword.blockchainfull_notm}} Platform console or the APIs to remove the failed node to ensure that the associated metadata and storage are also cleaned up.
{: tsResolve}

## How can I view my smart contract container logs?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

You may need to view your smart contract, or chaincode, container logs to debug a smart contract issue.
{: tsSymptoms}

<blockchain-sw-251>
Follow these [instructions](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-icp-manage#console-icp-manage-container-logs) to view your smart contract container logs.
{: tsResolve}
</blockchain-sw-251>


Follow these instructions to view your smart contract container logs on [{{site.data.keyword.cloud_notm}}](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
{: tsResolve}


## Why is my CA, peer, or ordering node that is configured to use HSM not working?
{: #ibp-v2-troubleshooting-hsm-proxy}
{: troubleshoot}

This problem can happen when you try to deploy a CA, peer, or ordering node that is configured with HSM and the deployment fails. Or it can surface when a node that is configured for HSM stops working. The node, client, or SDK logs contain the following error:
{: tsSymptoms}
```
{"code":1000,"message":"Private key not found [pkcs11: 0x30: CKR_DEVICE_ERROR]"}"
```
or
```
{"code":1000,"message":"Private key not found [pkcs11: 0xB3: CKR_SESSION_HANDLE_INVALID]"}"
```


This problem happens when the PKCS #11 proxy that is associated with the HSM is unreachable due to a network problem or if the proxy restarts after the node has connected to it.
{: tsCauses}

To re-establish communications between the node and the proxy, restart the failing node by deleting the pod associated with the node. A new pod will be created and the connection with the PKCS #11 proxy is restored. Use the following steps to restart the failing node:
{: tsResolve}
- List the pods: `kubectl get pods -n <NAMESPACE>`
- Delete the pod: `kubectl delete pod -n <NAMESPACE> <PODNAME>`  

Replace:
- `<NAMESPACE>` with the namespace where the associated pods are deployed.
To find the namespace, open any CA node in your console and click the Settings icon. View the value of the Certificate Authority endpoint URL. For example: https://n2734d0-paorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054.
The namespace is the first part of the URL beginning with the letter n and followed by a random string of six alphanumeric characters. So in this example, the value of the namespace is `n2734d0`.<blockchain-sw-251>or project, if using OpenShift Container Platform, where the {{site.data.keyword.blockchainfull_notm}} Platform was deployed in your Kubernetes cluster.</blockchain-sw-251>
- `<PODNAME>` with the **Name** of the failing pod that is visible in the list of pods returned by the previous command.<blockchain-sw-251>

## My CA failed to upgrade, how can I fix it?
{: #ibp-v2-troubleshooting-ca-upgrade-fails}
{: troubleshoot}

After upgrading from {{site.data.keyword.blockchainfull_notm}} Platform v2.1.2 to v2.1.3, when I try to update my CA by clicking **Update version**, it fails with the error `ECONNRESET`. The CA logs include an error similar to the following text `"error":"CA instance 'org1ca' encountered error: Code: 23 - failed to migrate ca: no matches for kind \"IBPCA\" in version \"ibp.com/v212\`.
{: tsSymptoms}

To resolve this problem, you need to restart the Operator pod in your cluster.
{: tsResolve}
  - Run the following command to get the name of the pod that corresponds to the operator:

    ```
    kubectl get po | grep ibp-operator
    ```
    {: codeblock}

    The output would look similar to:

    ```
    ibp-operator-5794799cff-pbm2h   1/1     Running   0          21d
  ```

  - In the following command, replace `<OPERATOR-POD>` with the name of the operator pod from the previous command, for example `ibp-operator-5794799cff-pbm2h`.
    ```
    kubectl delete po <CONSOLE-POD>
    ```
    {: codeblock}
After the operator pod restarts, the CA node is successfully upgraded.</blockchain-sw-251>

## Why are my transactions returning an endorsement policy error: signature set did not satisfy policy?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

When I invoke a smart contract to submit a transaction, the transaction returns the following endorsement policy failure:
{: tsSymptoms}

```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```

If you have recently joined a channel and installed the smart contract, this error occurs if you have not added your organization to the endorsement policy. Because your organization is not on the list of organizations who can endorse a transaction from the smart contract, the endorsement from your peers is rejected by the channel. If you encounter this problem, you can change the endorsement policy by upgrading the smart contract. For more information, see [Specifying an endorsement policy](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2#ibp-console-smart-contracts-v2-install-propose) and [Versioning a smart contract](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2#ibp-console-smart-contracts-v2-versioning).
{: tsCauses}

## Why are the transactions I submit from VS Code failing with a No endorsement plan available error?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Transactions submitted from VS Code fail with an error similar to:
{: tsSymptoms}

```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```

This error occurs if you are using the Fabric Service Discovery feature but did not configure any anchor peers on your channel.
{: tsCauses}

Follow step three of the [private data topic](/docs/blockchain?topic=blockchain-ibp-console-smart-contracts-v2#ibp-console-smart-contracts-v2-private-data) in the Deploy a smart contract tutorial to configure your anchor peers.
{: tsResolve}

## Why are the transactions I submit from VS Code failing with an endorsement failure?
{: #ibp-v2-troubleshooting-endorsement}
{: troubleshoot}

My smart contract endorsement proposals from my peer are failing from my client application with the endorsement error `error: [Channel.js]: Channel:<channel_name> received discovery error:failed constructing descriptor for chaincodes:<name:"chaincode-name">` or `[ERROR] Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"MyAssetContract"}]}`
{: tsSymptoms}

Also in the endorsing peer logs I can see the error:
```
UTC [discovery] chaincodeQuery -> ERRO 23c Failed constructing descriptor for chaincode chaincodes:<name:"chaincode-name">,: cannot satisfy any principal combination
```

This error occurs when the peer's enroll id type does not match the smart contract endorsement policy that was configured when the smart contract was instantiated on the channel.
{: tsCauses}

The only way to resolve this error is to delete the peer and create a new one with an enroll id that has the correct type `peer`. You can use the enroll id and secret from an existing user of type `peer` from the peer's CA or register a new user with type `peer`. Follow the instructions in the [Build a network tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peer-org1) to create a new peer identity with the correct type and peer.
{: tsResolve}

## How do I delete a peer pod?
{: #ibp-troubleshooting-delete-peer}
{: troubleshoot}

You are looking for the commands to delete a peer.
{: tsSymptoms}

Use the following CLI command to identify and then delete and restart the failing peer:
{: tsResolve}

* List the pods: `kubectl get pods --all-namespaces`
* Delete the pod: `kubectl delete pod -n <NAMESPACE> <PODNAME>`  

Replace `<NAMESPACE>` with the name of your Kubernetes namespace or your OpenShift project.

## How can I recover a contract after a failed upgrade of the smart contract container?
{: #ibp-troubleshooting-contract-fail}
{: troubleshoot}

All contracts were lost after the procedure to upgrade the smart contract container crashed.
{: tsSymptoms}

[Delete all the peer pods](#ibp-troubleshooting-delete-peer). This deletion triggers the peer to be created again and restarts the proxy.
{: tsResolve}



## My Kubernetes cluster on {{site.data.keyword.cloud_notm}} expired. What does this mean?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

I received an e-mail that my {{site.data.keyword.IBM_notm}} Kubernetes service cluster is about to expire or its status is `Expired`. Or, you are not able to access the console after 30 days.
{: tsSymptoms}

Free Kubernetes clusters are only valid for 30 days.
{: tsCauses}

It is not possible to migrate from a free cluster to a paid cluster. After 30 days you cannot access the console and all of your nodes and certificates are deleted. See this topic on [Kubernetes cluster expiration](/docs/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration) for information on what is happening and what you can do.
{: tsResolve}

## After I deploy a node in the console, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has unbound immediate persistent volume claims. Is this an error?
{: #ibp-v2-troubleshooting-unbound-persistent-volume-claim}
{: troubleshoot}
{: support}

I see what looks like an error in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that a pod has unbound immediate persistent volume claims and am worried that my node has failed to deploy.
{: tsSymptoms}

More time is needed for the node to come up or you've possibly exceeded the maximum number of storage instances for your {{site.data.keyword.cloud_notm}} account.
{: tsCauses}

Unbound immediate persistent volume claims are a normal part of the deployment process for any node, reflecting that the persistent volume claim has been made but that the node itself has not yet deployed. Wait a few minutes for the node to deploy. If it is still not resolved, run the following kubectl command to describe the persistent volume claim for your namespace:
{: tsResolve}
```
kubectl describe pvc <pvc_name> -n <namespace>
```
{: codeblock}

If you see the following error, it is likely that you have reached your limit of storage volumes:  

`Backend Error:Your order will exceed the maximum number of storage volumes allowed. Please contact Sales. Type:StorageOrderFailed, RC:500, Recommended Action(s):Wait a few minutes, then try re-creating your PVC. If the problem persists, open an {{site.data.keyword.cloud_notm}} support case.`

See the troubleshooting topic on [File storage and block storage: PVC remains in a pending state](/docs/containers?topic=containers-cs_troubleshoot_storage#file_pvc_pending) for more details.

## After I deploy a node, I'm seeing a message in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff. Is this an error?
{: #ibp-v2-troubleshooting-crash-loop-backoff}
{: troubleshoot}

I see an error in my Kubernetes cluster on {{site.data.keyword.cloud_notm}} reporting that the pod has hit a crash loop backoff and am worried that my node has failed to deploy.

This node has failed to deploy.
{: tsCauses}

The node has failed to deploy. There can be several reasons for this, but you must go to your console, delete the node, and attempt to redeploy it. Make sure you are using the correct MSP, enroll ID, and secret.
{: tsResolve}


