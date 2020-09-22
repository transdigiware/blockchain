---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

subcollection: blockchain-sw-25

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# Administering your console
{: #console-icp-manage}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-console-icp-manage">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-console-icp-manage">2.1.3</a>
    </p>
</div>


After you install the console on your cluster, you can use the console to add or remove console users and access APIs that allow you to operate your network and govern your console. You can also access and customize the logs of your console.
{:shortdesc}

## Managing users from the console
{: #console-icp-manage-users}

The user who provisions the {{site.data.keyword.blockchainfull_notm}} Platform console is considered the console administrator. The administrator can then add and grant other users access to the console from the **Users** tab. Every user that accesses the console must be assigned an access policy with a user role defined. The policy determines what actions the user can perform within the console. By default, the console administrator is given the **Manager** role for the console. Other users can be assigned with **Manager**, **Writer**, or **Reader** roles when a console manager adds them to the console. Note that the users can also be managed with [APIs](/docs/blockchain-sw-25?topic=blockchain-sw-25-ibp-v2-apis#console-icp-manage-users-apis)

### Role to permission mapping
{: #console-icp-manage-role-mapping}

| Role | Permissions |
|--------|----------|
| Manager | As a Manager, you have permissions beyond the Writer role. You can do everything a Reader and Writer can do as well as: <ul><li>Provision new components such as CAs, peers, and ordering services, by using the console or APIs.</li><li>Override configuration settings on components.</li><li>Delete provisioned components by using the console or APIs.</li><li>Add/remove users and change user access policies.</li><li>Change console logging levels by using the console or APIs.</li><li>Restart the console by using an API.</li></ul> |
| Writer | As a Writer, you have permissions beyond the Reader role, including: <ul><li>Import components by using the console or APIs.</li><li>Remove imported components by using the console or APIs.</li><li>Register users on a CA.</li><li> Add or remove notifications by using the console or APIs.</li></ul>  |
| Reader | As a reader, you can perform read-only actions including: <ul><li>View console UI.</li><li>View console log.</li><li>Export components.</li><li>Issue any GET API.</li></ul> | |
{: caption="Table 1. Role to permissions mapping table" caption-side="bottom"}

Permissions are cumulative. If you select a **Manager** role, the user will also be able to perform all **Writer** and **Reader** actions, and you are not required to additionally check those roles. Likewise, a user with the **Writer** role will be able to perform all of the actions in the **Reader** role.

### Managing a user's password
{: #console-icp-manage-user-pw}


The password that you provided to the console custom resource definition becomes the default password for the console. All users need to use this password when they log in to the console for the first time. The console manager can also specify a new default password. In the **Users** tab of the console, click the **Update configuration** link under your authentication service tile name. In the right panel, you can view or update the default password for new users of the console.

You need to share the default password, or the default password that you reset, with the users so that they can log in to the console. They will be required to change the password upon their first login.
{:note}

Users with the manager role can reset passwords for other users. In the **Users** tab of the console, click the three vertical dots at the end of the row for a specific user, and then click **Reset password**. The console resets that user's password to the default password, which you can check and confirm in the right panel. Users with any role can change their own password at any time. Click the avatar icon in the upper right corner, and then click **Change password**.

After you add new users to the console, the users might not be able to view all the nodes, channels, or chaincode, which other users deploy. To work with these components, each user needs to import the associated identities into their own console wallet. For more information, see [Storing identities in your console wallet](/docs/blockchain?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modifying a user's role
{: #console-icp-manage-reset-user-pw}

A user with a manager role can update the roles of other console users and provide them access to more or fewer console permissions. In the **Users** tab of the console, click the three vertical dots at the end of the specific user row, and then click **Update authenticated user**. Select the new role for the user in the right panel. The user's role will be updated when they log in to the console the next time.

### Deleting a user from the console
{: #console-icp-manage-icp-remove-user}

If you are a user with a manager role, you can remove a user's access to the console. In the **Users** tab of the console, click the checkbox at the beginning of the specific user row. Then, click the **Trash** can icon at the top of the table, next to the **Add new users** button. After you remove the user from the table, the user cannot log in to the console again.

## Using the {{site.data.keyword.blockchainfull_notm}} APIs
{: #console-icp-manage-apis}

The {{site.data.keyword.blockchainfull_notm}} Platform exposes RESTful APIs that allow you to import, edit, and remove blockchain nodes from your console. You can also use APIs to manage the settings, logging, and notifications of your console.

### Prerequisites
{: #console-icp-manage-prereqs}

To use the APIs, you need to gather the following information:

- The console **API Endpoint**. This is the URL that you use to access the console by using your browser. You can find this URL in the **Notes:** section of the Helm release overview screen.
- A username and password that you can use to access the console. To create an API key, you must have an account with a [manager role](#console-icp-manage-users).

### Using an API key
{: #console-icp-manage-api-key}

Each console provides its own identity and access management. You can use your console username and password to generate an API key and secret that can authorize your API calls.

Each API key is associated with a role that governs the user's permissions. The API keys use the same access policy roles as exist for users who log in to the console by using a username and password. Refer to the table in the [Managing users](#console-icp-manage-users) section for the list of actions each role is allowed to perform. Because only manager roles can create API keys, a console administrator needs to create API keys for reader and writer users. The API keys never expire. As a result, the console administrator should delete API keys that are not being used.

There are three APIs available to manage your API keys:
- [Create an API key](#console-icp-manage-create-api-key)
- [View API keys](#console-icp-manage-view-api-keys)
- [Delete API keys](#console-icp-manage-delete-api-keys)

Use the request below to create an API key and secret. You can then use this key and secret to use the other APIs. The API secret is not stored by the console and needs to be saved by the user.

For all of APIs provided by the console, you need to add a `-k` or ``--insecure`` flag to bypass the requirement for a TLS certificate. You can also download the TLS certificate from your console by using your browser.

#### Create an API key
{: #console-icp-manage-create-api-key}

| **Request** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Request body fields** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` At least one value is required </li><li>`string` optional</li></ul>|
| **Response body fields** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Save: the key is not stored</li><li>`["<role>"]`</li></ul>|
| Authorization required | manager |

#### Example curl request: Create API key
{: #console-icp-manage-create-api-key-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -k \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### Manage your console API keys
{: #console-icp-manage-api-keys}

Once you have created an API key and secret, you can use the APIs to view or remove the keys that you have created. The keys and secrets do not expire until they are deleted.

#### View API keys
{: #console-icp-manage-view-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/keys |
| **Response body fields** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` UNIX time stamp in milliseconds</li><li>`string`</li></ul>|
| Authorization required | reader |

#### Example curl request: view API keys
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys \
  -u <api_key>:<api_secret> \
  -k
```

#### Delete API keys
{: #console-icp-manage-delete-api-keys}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/keys/`<api_key>` |
| Authorization required| manager |

#### Example curl request: delete API key
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v2/permissions/keys/<api_key> \
  -u <api_key>:<api_secret> \
  -k
```

### Managing users using the APIs
{: #console-icp-manage-users-apis}


You can also use the APIs to list, add, or remove users who can log in to the console or access the console APIs. You can also edit the users roles to upgrade a users level of access. The following APIs are available:

- [List users](#console-icp-manage-list-users-api)
- [Edit users](#console-icp-manage-edit-users-api)
- [Add users](#console-icp-manage-add-users-api)
- [Remove users](#console-icp-manage-remove-users-api)

#### List users
{: #console-icp-manage-list-users-api}


| **Request** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v2/permissions/users |
| **Response body fields** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` user ID</li><li>`string` email address</li><li>`["<role>"]`</li><li>`number` UNIX time stamp in milliseconds</li></ul>|
| Authorization required | reader |

#### Example curl request: list users
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
  -u <api_key>:<api_secret> \
  -k
```

#### Edit users
{: #console-icp-manage-edit-users-api}

| **Request** |  |
|-------------|-----------|
| Path | PUT `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`uuids`</li><li>`roles`</li></ul> | <ul><li>`array of strings` user IDs </li><li>`["reader", "writer", "manager"]` At least one value is required</li></ul> |
| **Response body fields** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`array of strings` user IDs </li></ul>|
| Authorization| manager |

#### Example curl request: edit a user
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
  -u <api_key>:<api_secret> \
  -k \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### Add users
{: #console-icp-manage-add-users-api}

| **Request** |  |
|-------------|-----------|
| Path | POST `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` At least one value is required </li><li>`string` optional</li></ul>|
| Authorization | manager |

#### Example curl request: add a user
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
  -u <api_key>:<api_secret> \
  -k \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### Remove users
{: #console-icp-manage-remove-users-api}

| **Request** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v2/permissions/users |
| **Request body fields** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` user ID</li></ul>|
| **Response body fields** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` user ID</li></ul>|
| Authorization | manager |

#### Example curl request: remove a user
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v2/permissions/users \
  -u <api_key>:<api_secret> \
  -k \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### Use the APIs to manage your components

You can view the complete set of APIs that are available in the [{{site.data.keyword.blockchainfull_notm}} Platform API reference](https://cloud.ibm.com/apidocs/blockchain).

Because you are using the APIs to communicate with your console on your own cluster, you need to replace the authorization that is provided by {{site.data.keyword.cloud_notm}} with the authentication that is provided by your console. Replace the `Bearer Auth` in the API reference with  `-u <api_key>:<api_secret>`. You also need to add a `-k` or ``--insecure`` flag to the command, or download the console TLS certificate by using your browser.

As an example, the API call below returns information about all of your components running on a service instance of the {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
```
curl -X GET \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v2/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```

You can use the APIs to create nodes on the cluster where your console is deployed, and to import nodes from other clusters or {{site.data.keyword.cloud_notm}}. For more information, see [Build a network by using APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) and [Import a network by using APIs](/docs/blockchain?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Viewing your logs
{: #icp-console-manage-logs}

When you use the {{site.data.keyword.blockchainfull_notm}} Platform console, you might need to view logs to debug an issue.

### Viewing your console logs
{: #console-icp-manage-console-logs}

You can easily access the console logs if you need to debug problems that you encounter when you use the console or operate your nodes. You can also set the logging level to increase or decrease the amount of logs that the console collects. The console logs are collected separately from the [node logs](#console-icp-manage-node-logs), which are collected by your Kubernetes cluster.

Navigate to the **Settings** tab in the console browser to change the logging settings. The console logs are collected from two separate sources:

  * **Client logging:** These logs are collected when commands are sent from your browser to the console.
  * **Server logging:** These logs are collected when the console sends commands to your nodes and from the console deployment. These logs include the Hyperledger Fabric logging output.

Set the number of collected logs by using the drop-down list under each log type. For example, **Error** and **Warning** collect the least amount of logs, while **Debug** and **All** collect the most.

You can view only the console logs if you are logged in as a console administrator. To view the logs from the **Settings** tab, replace the word `settings` in the browser URL with `api/v2/logs`. For example, if your console URL is `localhost:3001/settings`, you can view your logs by navigating to `localhost:3001/api/v2/logs`. Client and server logs are collected in separate files. The most recent logs are at the top of the page.

### Viewing your node logs
{: #console-icp-manage-node-logs}

Component logs can be viewed from the command line by using the [kubectl CLI commands](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} or through [Kibana](https://www.elastic.co/kibana){: external} which is included in the OpenShift Container Platform.

- Use the `kubectl logs` command to view the container logs inside the pod. Follow the instructions to [Install the kubectl cli](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} if you have not already done so. If you are unsure of your pod name, run the following command to view your list of pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Then, run the following command to retrieve the logs for the node container that resides inside the pod:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Replace `<pod_name>` with the name of your pod from the command output above.  
  Replace `<node>` with `ca`, `peer`, or `orderer` to view the logs for your node.  
  Replace `<node>` with `chaincode-logs`to view the logs for your smart contracts.

  You can also run the command `kubectl logs -f <pod_name>` to get a list of all of the containers that are running inside a pod. The response to the command is an error message similar to the following:
  ```
  Error from server (BadRequest): a container name must be specified for pod peer1org1-7b4b6687dc-7n4fz, choose one of: [dind peer proxy chaincode-logs couchdb] or one of the init containers: [init]
  ```

  For more information about the `kubectl logs` command, see [Kubernetes documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- Alternatively, if you are running on OpenShift Container Platform, you can access the logs from the OpenShift Web Browser.

  **Note:** When you view your logs in Kibana, you might receive the response `No results found`. This condition can occur if your cluster uses your worker node IP address as its hostname. To resolve this problem, remove the filter that begins with `node.hostname.keyword` at the top of the panel and the logs will become visible.

### Viewing your smart contract container logs
{: #console-icp-manage-container-logs}

If you encounter issues with your smart contract, you can view the smart contract, or chaincode, logs to debug an issue.

<img src="../images/2-x_Pill.png" alt="version 2.x" width="30" style="width:30px; border-style: none"/> **Hyperleder Fabric v2.x peer image**  

If your peer is based on the Fabric v2.x image, you can run the following commands to view the smart contract container logs.

First get a list of all of the chaincode pods running in your cluster:

```
kubectl get po -n <NAMESPACE> | grep chaincode-execution | cut -d" " -f1 | xargs -I {} kubectl get po {} -n <NAMESPACE> --show-labels
```
{:codeblock}
Replacing `<NAMESPACE>` with the name of your cluster namespace or OpenShift project.  

You should see results similar to:
```
NAME                                                       READY   STATUS            RESTARTS   AGE   LABELS
chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4   1/1     Running   0          14s   chaincode-id=myjavacc-1.1,peer-id=org1peer1
NAME                                                       READY   STATUS    RESTARTS   AGE   LABELS
chaincode-execution-f3cc736f-94ef-454d-8da3-362a50c653d9   1/1     Running   0          4m    chaincode-id=mynodecc-1.1,peer-id=org1peer1
```

Your smart contract name and version is visible next to the `chaincode-id`.

Then, to view the logs for a specific smart contract pod, run the command:
```
kubectl logs -f <SMART_CONTRACT_POD> -n <NAMESPACE>
```
{:codeblock}

Replace
- `<SMART_CONTRACT_POD>` with the name of the pod where the chaincode is running.
- `<NAMESPACE>` with the name of your cluster namespace.

For example:
```
kubectl  logs -f chaincode-execution-0a8fb504-78e2-4d50-a614-e95fb7e7c8f4 -n na0513

```

<br><br>

<img src="../images/1-4_Pill.png" alt="version 1.4" width="30" style="width:30px; border-style: none"/> **Hyperledger Fabric v1.4 peer image**  

If your peer is based on the Fabric v1.4 image, you can run the following kubectl command to view the smart contract container logs.

```
kubectl  logs -f <PEER_POD> -c chaincode-logs -n <NAMESPACE>
```
{:codeblock}

Replace
- `<PEER_POD>` with the name of the peer pod where the chaincode is running. Use the command `kubectl get po` to get the list of running pods.
- `<NAMESPACE>` with the name of the cluster namespace or OpenShift project.

## Installing patches for your nodes
{: #ibp-console-manage-patch}

The underlying {{site.data.keyword.IBM_notm}} Hyperledger Fabric docker images for the peer, CA, and ordering nodes might need to be updated over time, for example, with security updates or to a new Fabric point release. The **Patch available** text on a node tile is the indicator that such a patch is available and can be installed on the node whenever you are ready. Unless otherwise noted in the [Release notes](/docs/blockchain?topic=blockchain-release-notes-saas-20), these patches are optional, but recommended.

Patches are applied to nodes one at a time. While the patch is being applied, the node is unavailable to process requests or transactions. Therefore, to avoid any disruption of service, whenever possible you should ensure another node of the same type is available to process requests. Installing patches on a node takes about a minute to complete and when the update is complete, the node is ready to process requests.
{:note}

To apply a patch to a node, open the node tile and click the **Install patch** button.

You cannot patch nodes that you imported to the console.
{: important}
