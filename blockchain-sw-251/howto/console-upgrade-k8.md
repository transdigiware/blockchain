---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: Kubernetes, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Upgrading your console and components
{: #upgrade-k8}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-upgrade-k8">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-upgrade-k8">2.1.3</a>
    </p>
</div>


You can upgrade the {{site.data.keyword.blockchainfull}} Platform without disrupting a running network. Because the platform is deployed by using a Kubernetes operator, you can pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images from the {{site.data.keyword.IBM_notm}} Entitlement registry without having to reinstall the platform. You can use these instructions to upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform 2.5.
{:shortdesc}

## {{site.data.keyword.blockchainfull_notm}} Platform overview
{: #upgrade-k8-platform-overview}

Use these instructions to upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform 2.5 from v2.1.3, v2.1.2, v2.1.1, and v2.1.0. The table provides an overview of the current and past releases.

| Version | Release date | Image tags | New features |
|----|----|----|----|
| [{{site.data.keyword.blockchainfull_notm}} Platform 2.5](/docs/blockchain-sw-25?topic=blockchain-sw-25-release-notes-saas-20#08-25-2020) | 9 Sept 2020| **Console and tools** <ul><li>2.5.0-20200825-amd64</li><li>2.5.0-20200714-amd64</li><li>2.5.0-20200618-amd64</li></ul> **Fabric nodes** <ul><li>1.4.7-20200825-amd64</li><li>1.4.7-20200714-amd64</li><li>1.4.7-20200618-amd64</li><li>2.1.1-20200825-amd64</li><li>2.1.1-20200714-amd64</li><li>2.1.1-20200618-amd64</li></ul> **CouchDB** <ul><li>2.3.1-20200825-amd64</li><li>2.3.1-20200714-amd64</li><li>2.3.1-20200618-amd64</li></ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.7 and 2.1.1</ul> **Improvements to the Console UI** <ul><li>Ability to select Fabric version when you deploy a new peer or ordering node.</li></ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.3](/docs/blockchain-sw-213?topic=blockchain-sw-213-whats-new#whats-new-03-24-2020) | 24 March 2020| **Console and tools** <ul><li>2.1.3-20200520-amd64</li><li>2.1.3-20200416-amd64</li><li>2.1.3-20200324-amd64</li></ul> **Fabric nodes** <ul><li>1.4.6-20200520-amd64</li><li>1.4.6-20200416-amd64</li><li>1.4.6-20200324-amd64</li></ul> **CouchDB** <ul><li>2.3.1-20200520-amd64</li><li>2.3.1-20200416-amd64</li><li>2.3.1-20200324-amd64</li></ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.6</ul> **Improvements to the Console UI** <ul><li>Hardware Security Module (HSM) support for node identities</li><li>Ability to override CA, peer, and ordering node configuration</li><li>Ability to add and remove Raft ordering nodes</li><li>Java smart contract instantiation</li><li>Updated create channel and create organization panels</ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.2](/docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-12-17-2019) | 17 December 2019 | **Console and tools** <ul><li>2.1.2-20191217-amd64</li><li>2.1.2-20200213-amd64</li></ul> **Fabric nodes** <ul><li>1.4.4-20191217-amd64</li><li>1.4.4-20200213-amd64</li></ul> **CouchDB** <ul><li>2.3.1-20191217-amd64</li><li>2.3.1-20200213-amd64</li></ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.4</ul> **Additional platforms** <ul><li>Platform can be deployed on the OpenShift Container Platform 4.1 and 4.2</ul> **Improvements to the Console UI** <ul><li>Simplified component creation flows</li><li>Zone selection for ordering nodes</li><li>Add peer to a channel from Channels tab</li><li>Anchor peer during join</li><li>Export/Import all</ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.1]( /docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-11-08-2019)| 8 November 2019 | **Console and tools** <ul><li>2.1.1-20191108-amd64</ul> **Fabric nodes** <ul><li>1.4.3-20191108-amd64</ul> **CouchDB** <ul><li>2.3.1-20191108-amd64</ul> | **Additional platforms** <ul><li>Platform can be deployed on Kubernetes v1.14 - v1.16</li><li>Platform can be deployed on {{site.data.keyword.cloud_notm}} Private 3.2.1</li></ul> |
| [{{site.data.keyword.blockchainfull_notm}} Platform v2.1.0](/docs/blockchain-sw?topic=blockchain-sw-whats-new#whats-new-9-24-2019) | 24 September 2019 | **Console and tools** <ul><li>2.1.0-20190918-amd64</ul> **Fabric nodes** <ul><li>1.4.3-20190918-amd64</ul> **CouchDB** <ul><li>2.3.1-20190918-amd64</ul> | **Fabric Version Upgrade** <ul><li>Fabric version 1.4.3</ul> **Additional platforms** <ul><li>Platform can be deployed on the OpenShift Container Platform 3.11</ul> |
{: caption="Table 1. {{site.data.keyword.blockchainfull_notm}} Platform versions" caption-side="bottom"}



## Upgrade to the {{site.data.keyword.blockchainfull_notm}} Platform 2.5
{: #upgrade-k8-steps}

You can upgrade an {{site.data.keyword.blockchainfull_notm}} Platform network by using the following steps:

1. [Create the `ibpinfra` project for the webhook](#upgrade-k8s-ibpinfra)
2. [Create a secret for your entitlement key](#upgrade-k8s-secret-ibpinfra)
3. [Deploy the webhook and custom resource definitions to your Kubernetes cluster](#upgrade-k8s-webhook-crd)
4. [Update the ClusterRole](#upgrade-k8-clusterrole)
5. [Upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator](#upgrade-k8-operator)
6. [Use your console to upgrade your running blockchain nodes](#upgrade-k8-nodes)

After you upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator, the operator will automatically upgrade the console that is deployed on your namespace. You can then use the upgraded console to upgrade your blockchain nodes.

You need to complete these steps 4-6 for each network that that runs on a separate namespace. If you experience any problems, see the instructions for [rolling back an upgrade](#upgrade-k8-rollback). If you deployed your network behind a firewall, without access to the external internet, see the separate set of instructions for [Upgrading the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall](#upgrade-k8-firewall).

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

It is a best practice to upgrade your SDK to the latest version as part of a general upgrade of your network. While the SDK will always be compatible with equivalent releases of Fabric and lower, it might be necessary to upgrade to the latest SDK to leverage the latest Fabric features. Also, after upgrading, it's possible your client application may experience errors. Consult the your Fabric SDK documentation for information about how to upgrade.
{: tip}

### Roll back an upgrade
{: #upgrade-k8-rollback}

When you upgrade your operator, it saves the secrets, deployment spec, and network information of your console before it the operator attempts to upgrade the console. If your upgrade fails for any reason, {{site.data.keyword.IBM_notm}} Support can roll back your upgrade and restore your previous deployment by using the information on your cluster. If you need to roll back your upgrade, you can submit a support case from the [mysupport](https://www.ibm.com/support/pages/node/1072956){: external} page.

You can roll back an upgrade after you use the console to operate your network. However, after you use the console to upgrade your blockchain nodes, you can no longer roll back your console to a previous version of the platform.

## Before you begin
{: #upgrade-k8-before}

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-k8#deploy-k8-entitlement-key) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

Occasionally, a five node ordering service that was deployed using v2.1.2 will be deleted by the Kubernetes garbage collector because it considers the nodes a resource that needs to be cleaned up. This process is both random and unrecoverable --- if the ordering service is deleted, all of the channels hosted on it are permanently lost. To prevent this, the `ownerReferences` field in the configuration of each ordering node must be removed **before upgrading to 2.5**. For the steps about how to pull the configuration file, remove `ordererReferences`, and apply the change, see [Known issues](/docs/blockchain-sw?topic=blockchain-sw-sw-known-issues#sw-known-issues-ordering-service-delete) in the v2.1.2 documentation.
{:important}

Updating the Operator triggers a restart of all components managed by this installation of the {{site.data.keyword.blockchainfull_notm}} Platform including Fabric nodes. To avoid disruption of service, a multiregion setup is recommended.
{: note}

## Step one: Create the `ibpinfra` namespace for the webhook
{: #upgrade-k8s-ibpinfra}

Because the platform has updated the internal apiversion from `v1alpha1` in previous versions to `v1alpha2` in 2.5, a Kubernetes conversion webhook is required to update the CA, peer, operator, and console to the new API version. This webhook will continue to be used in the future, so new deployments of the platform are required to deploy it as well.  The webhook is deployed to its own namespace, referred to as  `ibpinfra` throughout these instructions.

After you log in to your cluster, you can create the new `ibpinfra` namespace for the Kubernetes conversion webhook using the kubectl CLI. The new namespace needs to be created by a cluster administrator.

Run the following command to create the namespace:
```
kubectl create namespace ibpinfra
```
{:codeblock}

## Step two: Create a secret for your entitlement key
{: #upgrade-k8s-secret-ibpinfra}

After you purchase the {{site.data.keyword.blockchainfull_notm}} Platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You need to store the entitlement key on your cluster by creating a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external}. Kubernetes secrets are used to securely store the key on your cluster and pass it to the operator and the console deployments.

Run the following command to create the secret and add it to your `ibpinfra` namespace or project:
```
kubectl create secret docker-registry docker-key-secret --docker-server=cp.icr.io --docker-username=cp --docker-password=<KEY> --docker-email=<EMAIL> -n ibpinfra
```
{:codeblock}
- Replace `<KEY>` with your entitlement key.
- Replace `<EMAIL>` with your email address.

The name of the secret that you are creating is `docker-key-secret`. It is required by the webhook that you will deploy later. You can only use the key once per deployment. You can refresh the key before you attempt another deployment and use that value here.
{: note}


## Step three: Deploy the webhook and custom resource definitions to your Kubernetes cluster
{: #upgrade-k8s-webhook-crd}

Before you can upgrade an existing network to 2.5, or deploy a new instance of the platform to your Kubernetes cluster, you need to create the conversion webhook by completing the steps in this section. The webhook is deployed to its own namespace or project, referred to `ibpinfra` throughout these instructions.

The first three steps are for deployment of the webhook. The last five steps are for the custom resource definitions for the CA, peer, orderer, and console components that the {{site.data.keyword.blockchainfull_notm}} Platform requires. You only have to deploy the webhook and custom resource definitions **once per cluster**. If you have already deployed this webhook and custom resource definitions to your cluster, you can skip these eight steps below.
{: important}

### 1. Configure role-based access control (RBAC) for the webhook
{: #webhook-rbac}

First, copy the following text to a file on your local system and save the file as `rbac.yaml`. This step allows the webhook to read and create a TLS secret in its own project.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: webhook
  namespace: ibpinfra
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: webhook
rules:
- apiGroups:
  - "*"
  resources:
  - secrets
  verbs:
  - "*"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: ibpinfra
subjects:
- kind: ServiceAccount
  name: webhook
  namespace: ibpinfra
roleRef:
  kind: Role
  name: webhook
  apiGroup: rbac.authorization.k8s.io
```
{: codeblock}

Run the following command to add the file to your cluster definition:
```
kubectl apply -f rbac.yaml -n ibpinfra
```
{:codeblock}

When the command completes successfully, you should see something similar to:
```
serviceaccount/webhook created
role.rbac.authorization.k8s.io/webhook created
rolebinding.rbac.authorization.k8s.io/ibpinfra created
```
### 2. (OpenShift cluster only) Apply the Security Context Constraint
{: #webhook-scc}

The {{site.data.keyword.blockchainfull_notm}} Platform requires specific security and access policies to be added to the `ibpinfra` project. Copy the security context constraint object below and save it to your local system as `ibpinfra-scc.yaml`.

```yaml
allowHostDirVolumePlugin: true
allowHostIPC: true
allowHostNetwork: true
allowHostPID: true
allowHostPorts: true
allowPrivilegeEscalation: true
allowPrivilegedContainer: true
allowedCapabilities:
- NET_BIND_SERVICE
- CHOWN
- DAC_OVERRIDE
- SETGID
- SETUID
- FOWNER
apiVersion: security.openshift.io/v1
defaultAddCapabilities: []
fsGroup:
  type: RunAsAny
groups:
- system:cluster-admins
- system:authenticated
kind: SecurityContextConstraints
metadata:
  name: ibpinfra
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
supplementalGroups:
  type: RunAsAny
volumes:
- "*"
priority: 1
```
{:codeblock}


After you save the file, run the following commands to add the file to your cluster and add the policy to your project.

```
oc apply -f ibpinfra-scc.yaml -n ibpinfra
oc adm policy add-scc-to-user ibpinfra system:serviceaccounts:ibpinfra
```
{:codeblock}

If the commands are successful, you can see a response that is similar to the following example:
```
securitycontextconstraints.security.openshift.io/ibpinfra created
scc "ibpinfra" added to: ["system:serviceaccounts:ibpinfra"]
```

### 3. Deploy the webhook
{: #webhook-deploy}

In order to deploy the webhook, you need to create two `.yaml` files and apply them to your Kubernetes cluster.

#### deployment.yaml
{: #webhook-deployment-yaml}

Copy the following text to a file on your local system and save the file as `deployment.yaml`. If you are deploying on OpenShift Container Platform 4.3 on LinuxONE, you need to replace `amd64` with `s390x`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: "ibp-webhook"
  labels:
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp-webhook"
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: "ibp-webhook"
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        helm.sh/chart: "ibm-ibp"
        app.kubernetes.io/name: "ibp"
        app.kubernetes.io/instance: "ibp-webhook"
      annotations:
        productName: "IBM Blockchain Platform"
        productID: "54283fa24f1a4e8589964e6e92626ec4"
        productVersion: "2.5.0"
    spec:
      serviceAccountName: webhook
      imagePullSecrets:
        - name: docker-key-secret
      hostIPC: false
      hostNetwork: false
      hostPID: false
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      containers:
        - name: "ibp-webhook"
          image: "cp.icr.io/cp/ibp-crdwebhook:2.5.0-20200825-amd64"
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 1000
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
          env:
            - name: "LICENSE"
              value: "accept"
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: server
              containerPort: 3000
          livenessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 30
            timeoutSeconds: 5
            failureThreshold: 6
          readinessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 26
            timeoutSeconds: 5
            periodSeconds: 5
          resources:
            requests:
              cpu: 0.1
              memory: "100Mi"
```
{: codeblock}


Run the following command to add the file to your cluster definition:
```
kubectl apply -n ibpinfra -f deployment.yaml
```
{: codeblock}

When the command completes successfully, you should see something similar to:
```
deployment.apps/ibp-webhook created
```

#### service.yaml
{: #webhook-service-yaml}

Second, copy the following text to a file on your local system and save the file as `service.yaml`.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: "ibp-webhook"
  labels:
    type: "webhook"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp-webhook"
    helm.sh/chart: "ibm-ibp"
spec:
  type: ClusterIP
  ports:
    - name: server
      port: 443
      targetPort: server
      protocol: TCP
  selector:
    app.kubernetes.io/instance: "ibp-webhook"
```
{: codeblock}

Run the following command to add the file to your cluster definition:
```
kubectl apply -n ibpinfra -f service.yaml
```
{: codeblock}

When the command completes successfully, you should see something similar to:
```
service/ibp-webhook created
```

### 4. Extract the certificate

Next, we need to extract the TLS certificate that was generated by the webhook deployment so that it can be used in the custom resource definitions in the next steps. Run the following command to extract the secret to a base64 encoded string:

```
kubectl get secret webhook-tls-cert -n ibpinfra -o json | jq -r .data.\"cert.pem\"
```
{: codeblock}

The output of this command is a base64 encoded string and looks similar to:
```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJoRENDQVNtZ0F3SUJBZ0lRZDNadkhZalN0KytKdTJXbFMvVDFzakFLQmdncWhrak9QUVFEQWpBU01SQXcKRGdZRFZRUUtFd2RKUWswZ1NVSlFNQjRYRFRJd01EWXdPVEUxTkRrME5sFORGsxTVZvdwpFakVRTUE0R0ExVUVDaE1IU1VKTklFbENVREJaTUJGcVRyV0Z4WFBhTU5mSUkrYUJ2RG9DQVFlTW3SUZvREFUQmdOVkhTVUVEREFLQmdncgpCZ0VGQlFjREFUQU1CZ05WSFJNQkFmOEVBakFBTUNvR0ExVWRFUVFqTUNHQ0gyTnlaQzEzWldKb2IyOXJMWE5sCmNuWnBZMlV1ZDJWaWFHOXZheTV6ZG1Nd0NnWUlLb1pJemowRUF3SURTUUF3UmdJaEFNb29kLy9zNGxYaTB2Y28KVjBOMTUrL0h6TkI1cTErSTJDdU9lb1c1RnR4MUFpRUEzOEFlVktPZnZSa0paN0R2THpCRFh6VmhJN2lBQVV3ZAo3ZStrOTA3TGFlTT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
```

Save the base64 encoded string that is returned by this command to be used in the next steps when you create the custom resource definitions.
{: important}

### 5. Create the CA custom resource definition
{: #deploy-crd-ca}

Copy the following text to a file on your local system and save the file as `ibpca-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  labels:
    app.kubernetes.io/instance: ibpca
    app.kubernetes.io/managed-by: ibp-operator
    app.kubernetes.io/name: ibp
    helm.sh/chart: ibm-ibp
    release: operator
  name: ibpcas.ibp.com
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v210
    served: false
    storage: false
  - name: v212
    served: false
    storage: false
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}


Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  

Then, use the `kubectl` CLI to add the custom resource definition to your project.

```
kubectl apply -f ibpca-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com created
```

### 6. Create the peer custom resource definition
{: #deploy-crd-peer}

Copy the following text to a file on your local system and save the file as `ibppeer-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibppeers.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibppeer"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}


Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  

Then, use the `kubectl` CLI to add the custom resource definition to your project.

```
kubectl apply -f ibppeer-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com created
```

### 7. Create the orderer custom resource definition
{: #deploy-crd-orderer}

Copy the following text to a file on your local system and save the file as `ibporderer-crd.yaml`.

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibporderers.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibporderer"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}


Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.

Then, use the `kubectl` CLI to add the custom resource definition to your project.

```
kubectl apply -f ibporderer-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com created
```

### 8. Create the console custom resource definition
{: #deploy-crd-console}

Copy the following text to a file on your local system and save the file as `ibpconsole-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibpconsoles.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibpconsole"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true
  group: ibp.com
  names:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}


Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  
Then, use the `kubectl` CLI to add the custom resource definition to your project.

```
kubectl apply -f ibpconsole-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com created
```


## Step four: Update the ClusterRole
{: #upgrade-k8-clusterrole}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibp-clusterrole.yaml`. Edit the file and replace `<NAMESPACE>` with the name of your project.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <NAMESPACE>
rules:
- apiGroups:
  - extensions
  resourceNames:
  - ibm-blockchain-platform-psp
  resources:
  - podsecuritypolicies
  verbs:
  - use
- apiGroups:
  - "*"
  resources:
  - pods
  - pod/logs
  - services
  - endpoints
  - persistentvolumeclaims
  - persistentvolumes
  - events
  - configmaps
  - secrets
  - ingresses
  - roles
  - rolebindings
  - serviceaccounts
  - nodes
  verbs:
  - '*'
- apiGroups:
  - ""
  resources:
  - namespaces
  - nodes
  verbs:
  - 'get'
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - customresourcedefinitions
  verbs:
  - get
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  - ibpservices
  - ibpcas
  - ibppeers
  - ibpfabproxies
  - ibporderers
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  verbs:
  - '*'
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - '*'
```
{:codeblock}

After you save and edit the file, run the following commands. Replace `<NAMESPACE>` with your Kubernetes namespace.
```
kubectl apply -f ibp-clusterrole.yaml -n <NAMESPACE>
```
{:codeblock}
Replace `<NAMESPACE>` with the name of your {{site.data.keyword.blockchainfull_notm}} Platform deployment namespace.


## Step five: Upgrade the {{site.data.keyword.blockchainfull_notm}} operator
{: #upgrade-k8-operator}

You can upgrade the {{site.data.keyword.blockchainfull_notm}} operator by fetching the operator deployment spec from your cluster. When the upgraded operator is running, the new operator will upgrade your console and download the latest images for your blockchain nodes.

Log in to your cluster by using the kubectl CLI. Because each {{site.data.keyword.blockchainfull_notm}} network runs in a different namespace, you must switch to each namespace and upgrade each network separately. use the following command to set the context to the namespace of the network that you want to upgrade. Replace `<NAMESPACE>` with your namespace.
```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{:codeblock}

When you are operating from your namespace, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibp-operator -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. You need to update the `image:` field with the updated version of the operator image. You can find the name and tag of the latest operator image below:
```
cp.icr.io/cp/ibp-operator:2.5.0-20200825-amd64
```
{:codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```
- name: ISOPENSHIFT
  value: "false"
```
{:codeblock}

Replace the values above with the following lines at the same indentation:
```
- name: CLUSTERTYPE
  value: <CLUSTER_TYPE>
```
{:codeblock}

- Replace `<CLUSTER_TYPE>` with `K8S` if you are deploying the platform on open source Kubernetes or Rancher.

When you are finished editing the file, the `env:` section would look similar to the following:
```
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibp-operator
- name: CLUSTERTYPE
  value: K8S
```
{:codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml -n <NAMESPACE>
```
{:codeblock}
Replace `<NAMESPACE>` with the name of your {{site.data.keyword.blockchainfull_notm}} Platform deployment namespace.

You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` operator spec to your namespace, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment ibp-operator`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1         1         1            1           1m
ibpconsole     1         1         1            1           8m
```

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems. You can run the command to apply the original operator file, `kubectl apply -f operator.yaml` to restore your original operator deployment.

## Step six: Upgrade your blockchain nodes
{: #upgrade-k8-nodes}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. Browse to the console UI and open the nodes overview tab. You can find the **Patch available** text on a node tile if there is an update available for the component. You can install this patch whenever you are ready. These patches are optional, but they are recommended. You cannot patch nodes that were imported into the console.

Apply patches to nodes one at a time. Your nodes are unavailable to process requests or transactions while the patch is being applied. Therefore, to avoid any disruption of service, you need to ensure that another node of the same type is available to process requests whenever possible. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:important}

To apply a patch to a node, open the node tile and click the **Install patch** button. You cannot patch nodes that you imported to the console.

## Upgrading the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall
{: #upgrade-k8-firewall}

If you deployed the {{site.data.keyword.blockchainfull_notm}} Platform behind a firewall, without access to the external internet, you can upgrade your network by using the following steps:

1. [Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images](#upgrade-k8-images-firewall)
2. [Create the `ibpinfra` project for the webhook](#upgrade-k8s-ibpinfra-fw)
3. [Create a secret for your entitlement key](#upgrade-k8s-secret-ibpinfra-fw)
4. [Deploy the webhook and custom resource definitions to your Kubernetes cluster](#upgrade-webhook-fw)
5. [Update the ClusterRole](#upgrade-k8-clusterrole-firewall)
6. [Upgrade the {{site.data.keyword.blockchainfull_notm}} Platform operator](#upgrade-k8-operator-firewall)
7. [Use your console to upgrade your running blockchain nodes](#upgrade-k8-nodes-firewall)

You can continue to submit transactions to your network while you are upgrading your network. However, you cannot use the console to deploy new nodes, install or instantiate smart contracts, or create new channels during the upgrade process.

### Before you begin
{: #upgrade-k8-begin-firewall}

To upgrade your network, you need to [retrieve your entitlement key](/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-k8-firewall#deploy-k8-entitlement-key-firewall) from the My {{site.data.keyword.IBM_notm}} Dashboard, and [create a Kubernetes secret](/docs/blockchain-sw-25?topic=blockchain-sw-25-deploy-k8#deploy-k8-docker-registry-secret) to store the key on your namespace. If the Entitlement key secret was removed from your cluster, or if your key is expired, then you need to download another key and create a new secret.

### Step one: Pull the latest {{site.data.keyword.blockchainfull_notm}} Platform images
{: #upgrade-k8-images-firewall}

To upgrade your network, download the latest set of {{site.data.keyword.blockchainfull_notm}} Platform images and push them to a docker registry that you can access from behind your firewall.

Use the following command to log in to the {{site.data.keyword.IBM_notm}} Entitlement Registry:
```
docker login --username cp --password <KEY> cp.icr.io
```
{:codeblock}

- Replace `<KEY>` with your entitlement key.

After you log in, use the following command to pull the images for {{site.data.keyword.blockchainfull_notm}} Platform v2.1.3:
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

After you download the images, you must change the image tags to refer to your docker registry. Replace `<LOCAL_REGISTRY>` with the URL of your local registry and run the following commands:
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

You can use the `docker images` command to check that the new tags were added. You can then push the images with the new tags to your docker registry. Log in to your registry by using the following command:
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

### Step two: Create the `ibpinfra` namespace for the webhook
{: #upgrade-k8s-ibpinfra-fw}

ABecause the platform has updated the internal apiversion from `v1alpha1` in previous versions to `v1alpha2` in 2.5, a Kubernetes conversion webhook is required to update the CA, peer, operator, and console to the new API version. This webhook will continue to be used in the future, so new deployments of the platform are required to deploy it as well.  The webhook is deployed to its own namespace, referred to as  `ibpinfra` throughout these instructions.

After you log in to your cluster, you can create the new `ibpinfra` namespace for the Kubernetes conversion webhook using the kubectl CLI. The new namespace needs to be created by a cluster administrator.

Run the following command to create the namespace:
```
kubectl create namespace ibpinfra
```
{:codeblock}

### Step three: Create a secret for your entitlement key
{: #upgrade-k8s-secret-ibpinfra-fw}

After you purchase the {{site.data.keyword.blockchainfull_notm}} Platform, you can access the [My IBM dashboard](https://myibm.ibm.com/dashboard/){: external} to obtain your entitlement key for the offering. You need to store the entitlement key on your cluster by creating a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/){: external}. Kubernetes secrets are used to securely store the key on your cluster and pass it to the operator and the console deployments.

Run the following command to create the secret and add it to your `ibpinfra` namespace or project:
```
kubectl create secret docker-registry docker-key-secret --docker-server=cp.icr.io --docker-username=cp --docker-password=<KEY> --docker-email=<EMAIL> -n ibpinfra
```
{:codeblock}
- Replace `<KEY>` with your entitlement key.
- Replace `<EMAIL>` with your email address.

The name of the secret that you are creating is `docker-key-secret`. It is required by the webhook that you will deploy later. You can only use the key once per deployment. You can refresh the key before you attempt another deployment and use that value here.
{: note}


### Step four: Deploy the webhook and custom resource definitions to your Kubernetes cluster
{: #upgrade-webhook-fw}

Before you can upgrade an existing network to 2.5, or deploy a new instance of the platform to your Kubernetes cluster, you need to create the conversion webhook by completing the steps in this section. The webhook is deployed to its own namespace or project, referred to `ibpinfra` throughout these instructions.

The first three steps are for deployment of the webhook. The last five steps are for creation of the custom resource definitions for the CA, peer, orderer and console components that the {{site.data.keyword.blockchainfull_notm}} Platform requires. You only have to deploy the webhook and custom resource definitions **once per cluster**. If you have already deployed this webhook and custom resource definitions to your cluster, you can skip these eight steps below.
{: important}

#### 1. Configure role-based access control (RBAC) for the webhook
{: #upgrade-webhook-rbac}

Copy the following text to a file on your local system and save the file as `rbac.yaml`. This step allows the webhook to read and create a TLS secret in its own project.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: webhook
  namespace: ibpinfra
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: webhook
rules:
- apiGroups:
  - "*"
  resources:
  - secrets
  verbs:
  - "*"
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: ibpinfra
subjects:
- kind: ServiceAccount
  name: webhook
  namespace: ibpinfra
roleRef:
  kind: Role
  name: webhook
  apiGroup: rbac.authorization.k8s.io
```
{: codeblock}

Run the following command to add the file to your cluster definition:
```
kubectl apply -f rbac.yaml -n ibpinfra
```
{:codeblock}

When the command completes successfully, you should see something similar to:
```
serviceaccount/webhook created
role.rbac.authorization.k8s.io/webhook created
rolebinding.rbac.authorization.k8s.io/ibpinfra created
```
#### 2.(OpenShift cluster only) Apply the Security Context Constraint
{: #upgrade-webhook-scc}

The {{site.data.keyword.blockchainfull_notm}} Platform requires specific security and access policies to be added to the `ibpinfra` project. Copy the security context constraint object below and save it to your local system as `ibpinfra-scc.yaml`.

```yaml
allowHostDirVolumePlugin: true
allowHostIPC: true
allowHostNetwork: true
allowHostPID: true
allowHostPorts: true
allowPrivilegeEscalation: true
allowPrivilegedContainer: true
allowedCapabilities:
- NET_BIND_SERVICE
- CHOWN
- DAC_OVERRIDE
- SETGID
- SETUID
- FOWNER
apiVersion: security.openshift.io/v1
defaultAddCapabilities: []
fsGroup:
  type: RunAsAny
groups:
- system:cluster-admins
- system:authenticated
kind: SecurityContextConstraints
metadata:
  name: ibpinfra
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
supplementalGroups:
  type: RunAsAny
volumes:
- "*"
priority: 1
```
{:codeblock}


After you save the file, run the following commands to add the file to your cluster and add the policy to your project.

```
oc apply -f ibpinfra-scc.yaml -n ibpinfra
oc adm policy add-scc-to-user ibpinfra system:serviceaccounts:ibpinfra
```
{:codeblock}

If the commands are successful, you can see a response that is similar to the following example:
```
securitycontextconstraints.security.openshift.io/ibpinfra created
scc "ibpinfra" added to: ["system:serviceaccounts:ibpinfra"]
```

#### 3. Deploy the webhook
{: #upgrade-webhook-deploy}

In order to deploy the webhook, you need to create two `.yaml` files and apply them to your Kubernetes cluster.

##### deployment.yaml
{: #upgrade-webhook-deployment-yaml}

Copy the following text to a file on your local system and save the file as `deployment.yaml`. If you are deploying on OpenShift Container Platform 4.3 on LinuxONE, you need to replace `amd64` with `s390x`.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: "ibp-webhook"
  labels:
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp-webhook"
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/instance: "ibp-webhook"
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        helm.sh/chart: "ibm-ibp"
        app.kubernetes.io/name: "ibp"
        app.kubernetes.io/instance: "ibp-webhook"
      annotations:
        productName: "IBM Blockchain Platform"
        productID: "54283fa24f1a4e8589964e6e92626ec4"
        productVersion: "2.5.0"
    spec:
      serviceAccountName: webhook
      imagePullSecrets:
        - name: docker-key-secret
      hostIPC: false
      hostNetwork: false
      hostPID: false
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      containers:
        - name: "ibp-webhook"
          image: "cp.icr.io/cp/ibp-crdwebhook:2.5.0-20200825-amd64"
          imagePullPolicy: Always
          securityContext:
            privileged: false
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            runAsUser: 1000
            capabilities:
              drop:
              - ALL
              add:
              - NET_BIND_SERVICE
          env:
            - name: "LICENSE"
              value: "accept"
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: server
              containerPort: 3000
          livenessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 30
            timeoutSeconds: 5
            failureThreshold: 6
          readinessProbe:
            httpGet:
              path: /healthz
              port: server
              scheme: HTTPS
            initialDelaySeconds: 26
            timeoutSeconds: 5
            periodSeconds: 5
          resources:
            requests:
              cpu: 0.1
              memory: "100Mi"
```
{: codeblock}


Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibpinfra -f deployment.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
deployment.apps/ibp-webhook created
```

##### service.yaml
{: #upgrade-webhook-service-yaml}

Secondly, copy the following text to a file on your local system and save the file as `service.yaml`.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: "ibp-webhook"
  labels:
    type: "webhook"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibp-webhook"
    helm.sh/chart: "ibm-ibp"
spec:
  type: ClusterIP
  ports:
    - name: server
      port: 443
      targetPort: server
      protocol: TCP
  selector:
    app.kubernetes.io/instance: "ibp-webhook"
```
{: codeblock}

Run the following command to add the configuration to your cluster definition:
```
kubectl apply -n ibpinfra -f service.yaml
```
{: codeblock}

When the command completes successfully you should see something similar to:
```
service/ibp-webhook created
```

#### 4. Extract the certificate
{: #upgrade-webhook-extract-cert}

Next, we need to extract the TLS certificate that was generated by the webhook deployment so that it can be used in the next steps. Run the following command to extract the secret to a base64 encoded string:

```
kubectl get secret webhook-tls-cert -n ibpinfra -o json | jq -r .data.\"cert.pem\"
```
{: codeblock}

The output of this command is a base64 encoded string and looks similar to:
```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJoRENDQVNtZ0F3SUJBZ0lRZDNadkhZalN0KytKdTJXbFMvVDFzakFLQmdncWhrak9QUVFEQWpBU01SQXcKRGdZRFZRUUtFd2RKUWswZ1NVSlFNQjRYRFRJd01EWXdPVEUxTkRrME5sFORGsxTVZvdwpFakVRTUE0R0ExVUVDaE1IU1VKTklFbENVREJaTUJGcVRyV0Z4WFBhTU5mSUkrYUJ2RG9DQVFlTW3SUZvREFUQmdOVkhTVUVEREFLQmdncgpCZ0VGQlFjREFUQU1CZ05WSFJNQkFmOEVBakFBTUNvR0ExVWRFUVFqTUNHQ0gyTnlaQzEzWldKb2IyOXJMWE5sCmNuWnBZMlV1ZDJWaWFHOXZheTV6ZG1Nd0NnWUlLb1pJemowRUF3SURTUUF3UmdJaEFNb29kLy9zNGxYaTB2Y28KVjBOMTUrL0h6TkI1cTErSTJDdU9lb1c1RnR4MUFpRUEzOEFlVktPZnZSa0paN0R2THpCRFh6VmhJN2lBQVV3ZAo3ZStrOTA3TGFlTT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
```

Save the base64 encoded string that is returned by this command to be used in the next steps when you create the custom resource definitions.
{: important}

#### 5. Create the CA custom resource definition
{: #upgrade-crd-ca}

Copy the following text to a file on your local system and save the file as `ibpca-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  labels:
    app.kubernetes.io/instance: ibpca
    app.kubernetes.io/managed-by: ibp-operator
    app.kubernetes.io/name: ibp
    helm.sh/chart: ibm-ibp
    release: operator
  name: ibpcas.ibp.com
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPCA
    listKind: IBPCAList
    plural: ibpcas
    singular: ibpca
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v210
    served: false
    storage: false
  - name: v212
    served: false
    storage: false
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}


Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  

Then, use the `kubectl` CLI to add the custom resource definition to your namespace or project.

```
kubectl apply -f ibpca-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibpcas.ibp.com created
```

#### 6. Create the peer custom resource definition
{: #upgrade-crd-peer}

Copy the following text to a file on your local system and save the file as `ibppeer-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibppeers.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibppeer"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPPeer
    listKind: IBPPeerList
    plural: ibppeers
    singular: ibppeer
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}



Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  

Then, use the `kubectl` CLI to add the custom resource definition to your namespace or project.

```
kubectl apply -f ibppeer-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibppeers.ibp.com created
```

#### 7. Create the orderer custom resource definition
{: #upgrade-crd-orderer}

Copy the following text to a file on your local system and save the file as `ibporderer-crd.yaml`.

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibporderers.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibporderer"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true    
  group: ibp.com
  names:
    kind: IBPOrderer
    listKind: IBPOrdererList
    plural: ibporderers
    singular: ibporderer
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}

{:codeblock}

Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.

Then, use the `kubectl` CLI to add the custom resource definition to your namespace or project.

```
kubectl apply -f ibporderer-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibporderers.ibp.com created
```

#### 8. Create the console custom resource definition
{: #upgrade-crd-console}

Copy the following text to a file on your local system and save the file as `ibpconsole-crd.yaml`.
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ibpconsoles.ibp.com
  labels:
    release: "operator"
    helm.sh/chart: "ibm-ibp"
    app.kubernetes.io/name: "ibp"
    app.kubernetes.io/instance: "ibpconsole"
    app.kubernetes.io/managed-by: "ibp-operator"
spec:
  preserveUnknownFields: false
  conversion:
    strategy: Webhook
    webhookClientConfig:
      service:
        namespace: ibpinfra
        name: ibp-webhook
        path: /crdconvert
      caBundle: "<CABUNDLE>"
  validation:
    openAPIV3Schema:
      x-kubernetes-preserve-unknown-fields: true
  group: ibp.com
  names:
    kind: IBPConsole
    listKind: IBPConsoleList
    plural: ibpconsoles
    singular: ibpconsole
  scope: Namespaced
  subresources:
    status: {}
  version: v1alpha2
  versions:
  - name: v1alpha2
    served: true
    storage: true
  - name: v1alpha1
    served: true
    storage: false
```
{:codeblock}

{:codeblock}

Replace the value of `<CABUNDLE>` with the base64 encoded string that you extracted in step three after the webhook deployment.  
Then, use the `kubectl` CLI to add the custom resource definition to your namespace or project.

```
kubectl apply -f ibpconsole-crd.yaml
```
{:codeblock}

You should see the following output when it is successful:
```
customresourcedefinition.apiextensions.k8s.io/ibpconsoles.ibp.com created
```


### Step five: Update the ClusterRole
{: #upgrade-k8-clusterrole-firewall}

This step is only required if you are upgrading from v2.1.0 or v2.1.1. If you are running v2.1.2 you can skip to [Step three](#upgrade-k8-operator-firewall).
{: note}

You need to update the ClusterRole that is applied to your project. Copy the following text to a file on your local system and save the file as `ibp-clusterrole.yaml`. Edit the file and replace `<NAMESPACE>` with the name of your project.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: <NAMESPACE>
rules:
- apiGroups:
  - extensions
  resourceNames:
  - ibm-blockchain-platform-psp
  resources:
  - podsecuritypolicies
  verbs:
  - use
- apiGroups:
  - "*"
  resources:
  - pods
  - pod/logs
  - services
  - endpoints
  - persistentvolumeclaims
  - persistentvolumes
  - events
  - configmaps
  - secrets
  - ingresses
  - roles
  - rolebindings
  - serviceaccounts
  - nodes
  verbs:
  - '*'
- apiGroups:
  - ""
  resources:
  - namespaces
  - nodes
  verbs:
  - 'get'
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - customresourcedefinitions
  verbs:
  - get
- apiGroups:
  - apiextensions.k8s.io
  resources:
  - persistentvolumeclaims
  - persistentvolumes
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  - ibpservices
  - ibpcas
  - ibppeers
  - ibpfabproxies
  - ibporderers
  verbs:
  - '*'
- apiGroups:
  - ibp.com
  resources:
  - '*'
  verbs:
  - '*'
- apiGroups:
  - apps
  resources:
  - deployments
  - daemonsets
  - replicasets
  - statefulsets
  verbs:
  - '*'
```
{:codeblock}

After you save and edit the file, run the following commands. Replace `<NAMESPACE>` with your Kubernetes namespace.
```
kubectl apply -f ibp-clusterrole.yaml -n <NAMESPACE>
```
{:codeblock}
Replace `<NAMESPACE>` with the name of your {{site.data.keyword.blockchainfull_notm}} Platform deployment namespace.


### Step six: Upgrade the {{site.data.keyword.blockchainfull_notm}} operator
{: #upgrade-k8-operator-firewall}

You can upgrade the {{site.data.keyword.blockchainfull_notm}} operator by fetching the operator deployment spec from your cluster. You can then update the spec with the latest operator image that you pushed to your local registry. When the upgraded operator is running, the new operator will download the images that you pushed to your local registry and upgrade your console.

Log in to your cluster by using the kubectl CLI. Because each {{site.data.keyword.blockchainfull_notm}} network runs in a different namespace, you must switch to each namespace and upgrade each network separately. use the following command to set the context to the namespace of the network that you want to upgrade. Replace `<NAMESPACE>` with your namespace.
```
kubectl config set-context --current --namespace=<NAMESPACE>
```
{:codeblock}
Replace `<NAMESPACE>` with the name of your {{site.data.keyword.blockchainfull_notm}} Platform deployment namespace.

When you are operating from your namespace, run the following command to download the operator deployment spec to your local file system:
```
kubectl get deployment ibp-operator -o yaml > operator.yaml
```
{:codeblock}

Open `operator.yaml` in a text editor and save a new copy of the file as `operator-upgrade.yaml`. You need to update the `image:` field with the updated version of the operator image:
```yaml
<LOCAL_REGISTRY>/ibp-operator:2.5.0-20200825-amd64
```
{:codeblock}

You also need to edit the `env:` section of the file. Find the following lines in `operator-upgrade.yaml`:
```yaml
- name: ISOPENSHIFT
  value: "false"
```
{:codeblock}

Replace the values above with the following lines at the same indentation:
```yaml
- name: CLUSTERTYPE
  value: <CLUSTER_TYPE>
```
{:codeblock}

- Replace `<CLUSTER_TYPE>` with `K8S` if you are deploying the platform on open source Kubernetes or Rancher.

When you are finished editing the file, the `env:` section would look similar to the following:
```yaml
env:
- name: WATCH_NAMESPACE
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.namespace
- name: POD_NAME
  valueFrom:
    fieldRef:
      apiVersion: v1
      fieldPath: metadata.name
- name: OPERATOR_NAME
  value: ibp-operator
- name: CLUSTERTYPE
  value: K8S
```
{:codeblock}

Save the file on your local system. You can then issue the following command upgrade your operator:
```
kubectl apply -f operator-upgrade.yaml
```
{:codeblock}


You can use the `kubectl get deployment ibp-operator -o yaml` command to confirm that the command updated the operator spec.

After you apply the `operator-upgrade.yaml` deployment spec to your namespace, the operator will restart and pull the latest image. The upgrade takes about a minute. While the upgrade is taking place, you can still access your console UI. However, you cannot use the console to install and instantiate chaincode, or use the console or the APIs to create or remove a node.

You can check that the upgrade is complete by running `kubectl get deployment ibp-operator`. If the upgrade is successful, then you can see the following tables with four ones displayed for your operator and your console.
```
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
ibp-operator   1         1         1            1           1m
ibpconsole     1         1         1            1           8m
```

If you experience a problem while you are upgrading the operator, go to this [troubleshooting topic](/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-v2-troubleshooting#ibp-v2-troubleshooting-deployment-cr) for a list of commonly encountered problems.

If your console experiences an image pull error, you may need to update the console deployment spec with local registry that you used to download the images. Run the following command to download the deployment spec of the console:
```
kubectl get ibpconsole ibpconsole -o yaml > console.yaml
```
{:codeblock}

Then add the URL of your local registry to the `spec:` section of `console.yaml`. Replace `<LOCAL_REGISTRY>` with the URL of your local registry:
```yaml
spec:
  registryURL: <LOCAL_REGISTRY>
```
{:codeblock}

Save the updated file as `console-upgrade.yaml` on your local system. You can then issue the following command upgrade your console:
```
kubectl apply -f console-upgrade.yaml
```
{:codeblock}

### Step seven: Upgrade your blockchain nodes
{: #upgrade-k8-nodes-firewall}

After you upgrade your console, you can use the console UI to upgrade the nodes of your blockchain network. For more information, see [Upgrade your blockchain nodes](#upgrade-k8-nodes).
