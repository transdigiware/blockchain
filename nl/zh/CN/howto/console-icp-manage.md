---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

subcollection: blockchain

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

# 管理控制台
{: #console-icp-manage}

在 {{site.data.keyword.cloud}} Private 上部署控制台后，可以使用控制台来添加或除去控制台用户，以及访问允许您操作网络和管理控制台的 API。此外，还可以访问和定制控制台的日志。
{:shortdesc}

## 通过控制台管理用户
{: #console-icp-manage-users}

供应 {{site.data.keyword.blockchainfull_notm}} Platform 控制台的用户被视为控制台管理员。然后，该管理员可以使用控制台中的**用户**选项卡来添加其他用户并向其授予对控制台的访问权。对于访问控制台的每个用户，必须为其分配定义了用户角色的访问策略。策略确定用户可以在控制台中执行的操作。缺省情况下，将为控制台管理员授予控制台的**管理者**角色。控制台管理者将其他用户添加到控制台时，可以为这些用户分配**管理者**、**写入者**或**读取者**角色。请注意，还可以使用 [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis) 来管理用户。

|角色|功能|
|--------|----------|
|管理者|“管理者”拥有的许可权范围大于“写入者”角色。“管理者”除了可以执行“读取者”和“写入者”可以执行的所有操作外，还可以执行以下操作：<ul><li>使用控制台或 API 来供应新组件。</li><li>使用控制台或 API 删除已供应的组件。</li><li>添加/除去用户并更改用户访问策略。</li><li>使用控制台或 API 更改控制台日志记录级别。</li><li>使用 API 重新启动控制台。</li></ul> |
|写入者 |“写入者”拥有的许可权范围大于“读取者”角色，包括：<ul><li>使用控制台或 API 导入组件。</li><li>使用控制台或 API 除去导入的组件。</li><li>在 CA 上注册用户。</li><li> 使用控制台或 API 添加或除去通知。</li></ul>  |
|读取者 |“读取者”可以执行只读操作，包括：<ul><li>查看控制台 UI。</li><li>查看控制台日志。</li><li>导出组件。</li><li>发出任何 GET API。</li></ul> | |

许可权是累积的。如果选择**管理者**角色，那么用户还能够执行所有**写入者**和**读取者**操作，并且无需另外选中这些角色。同样，具有**写入者**角色的用户将能够执行**读取者**角色中的所有操作。

### 管理用户密码
{: #console-icp-manage-user-pw}

存储在[密码私钥](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)中，然后在配置期间传递给控制台的密码将成为控制台的缺省密码。所有用户首次登录到控制台时，都需要使用此密码。控制台管理者还可以指定新的缺省密码。在控制台的**用户**选项卡中，单击认证服务磁贴名称下的**更新配置**链接。在右侧面板中，可以查看或更新控制台新用户的缺省密码。

您需要与用户共享缺省密码或您重置的缺省密码，以便用户可以登录到控制台。系统会要求用户在首次登录时更改密码。
{:note}

具有“管理者”角色的用户可以重置其他用户的密码。在控制台的**用户**选项卡中，单击特定用户所在行末尾的三个垂直点，然后单击**重置密码**。控制台会将该用户的密码重置为缺省密码，您可以在右侧面板中检查并确认该密码。具有任何角色的用户都可以随时更改自己的密码。单击右上角的头像图标，然后单击**更改密码**。

将新用户添加到控制台后，这些用户可能无法查看其他用户部署的所有节点、通道或链代码。要使用这些组件，每个用户都需要将关联的身份导入到其自己的控制台电子钱包。有关更多信息，请参阅[在控制台电子钱包中存储身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet)。
{:important}

### 修改用户角色
{: #console-icp-manage-reset-user-pw}

具有“管理者”角色的用户可以更新其他控制台用户的角色，并为他们提供对更多或更少控制台功能的访问权。在控制台的**用户**选项卡中，单击特定用户行末尾的三个垂直点，然后单击**更新已认证的用户**。在右侧面板中为用户选择新角色。用户的角色将在其下次登录控制台时更新。

### 从控制台中删除用户
{: #console-icp-manage-icp-remove-user}

如果您是具有“管理者”角色的用户，那么可以除去用户对控制台的访问权。在控制台的**用户**选项卡中，单击特定用户行开头的复选框。然后，单击表顶部**添加新用户**按钮旁边的**废纸篓**图标。从表中除去用户后，该用户即无法再次登录到控制台。

## 使用 {{site.data.keyword.blockchainfull_notm}} API
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform 公开了 RESTful API，允许通过控制台导入、编辑和除去区块链节点。还可以使用 API 来管理控制台的设置、日志记录和通知。请参阅以下指示信息来使用部署在 {{site.data.keyword.cloud_notm}} Private 上的控制台提供的 API。

### 先决条件
{: #console-icp-manage-prereqs}

要使用 API，需要收集以下信息：

- 控制台 **API 端点**。这是用于通过浏览器访问控制台的 URL。您可以在 Helm 发布概述屏幕的**注：**部分中找到此 URL。
- 可用于访问控制台的用户名和密码。要创建 API 密钥，您必须拥有具有[管理者角色](#console-icp-manage-users)的帐户。

### 使用 API 密钥
{: #console-icp-manage-create-api-key}

每个控制台都会提供其自己的身份和访问权管理。可以使用控制台用户名和密码来生成可以授权 API 调用的 API 密钥和私钥。

每个 API 密钥都与一个角色相关联，该角色用于管理允许用户执行哪些功能。API 密钥使用的访问策略角色与使用用户名和密码登录到控制台的用户的访问策略角色相同。请参阅[管理用户](#console-icp-manage-users)部分中的表，以获取有关允许每个角色执行的操作的列表。由于只有“管理者”角色可以创建 API 密钥，因此控制台管理员需要为“读取者”和“写入者”用户创建 API 密钥。API 密钥永不到期。因此，控制台管理员应该负责删除未使用的 API 密钥。

有三个 API 可用于管理 API 密钥：
- [创建 API 密钥](#console-icp-manage-create-api-key)
- [查看 API 密钥](#console-icp-manage-view-api-keys)
- [删除 API 密钥](#console-icp-manage-delete-api-keys)

使用以下请求来创建 API 密钥和私钥。然后，可以使用此密钥和私钥来利用其他 API。控制台不会存储 API 私钥，这需要由用户保存。

#### 创建 API 密钥
{: #console-icp-manage-create-api-key-api}

|**请求**|  |
|-------------|-----------|
|路径|POST `<API_endpoint>`/ak/api/v1/permissions/keys|
|**请求主体字段**| |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 至少需要一个值</li><li>`string` 可选</li></ul>|
|**响应主体字段**| |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` 保存：不会存储密钥</li><li>`["<role>"]`</li></ul>|
|需要授权|管理者|

#### 示例 curl 请求：创建 API 密钥
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### 管理控制台 API 密钥
{: #console-icp-manage-api-keys}

创建 API 密钥和私钥后，可以使用 API 来查看或除去已创建的密钥。密钥和私钥不会到期，直到将其删除。

#### 查看 API 密钥
{: #console-icp-manage-view-api-keys}

|**请求**|  |
|-------------|-----------|
|路径|GET `<API_endpoint>`/ak/api/v1/permissions/keys|
|**响应主体字段**| |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` Unix 时间戳记（以毫秒为单位）</li><li>`string`</li></ul>|
|需要授权|读取者|

#### 示例 curl 请求：查看 API 密钥
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### 删除 API 密钥
{: #console-icp-manage-delete-api-keys}

|**请求**|  |
|-------------|-----------|
|路径|DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>`|
|需要授权|管理者|

#### 示例 curl 请求：删除 API 密钥
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### 使用 API 管理用户
{: #console-icp-manage-users-apis}


您还可以使用 API 来列出、添加或除去可以登录到控制台或访问控制台 API 的用户。还可以编辑用户角色以升级用户访问级别。以下 API 可用：

- [列出用户](#console-icp-manage-list-users-api)
- [编辑用户](#console-icp-manage-edit-users-api)
- [添加用户](#console-icp-manage-add-users-api)
- [除去用户](#console-icp-manage-remove-users-api)

对于管理控制台的用户和设置以及管理网络节点的 API，需要添加 ``-K`` 或 ``--insecure`` 标志以绕过对 TLS 证书的需求。还可以使用浏览器通过控制台来下载 TLS 证书。

#### 列出用户
{: #console-icp-manage-list-users-api}


|**请求**|  |
|-------------|-----------|
|路径|GET `<API_endpoint>`/ak/api/v1/permissions/users|
|**响应主体字段**| |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` 用户标识</li><li>`string` 电子邮件地址</li><li>`["<role>"]`</li><li>`number` Unix 时间戳记（以毫秒为单位）</li></ul>|
|需要授权|读取者|

#### 示例 curl 请求：列出用户
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### 编辑用户
{: #console-icp-manage-edit-users-api}

|**请求**|  |
|-------------|-----------|
|路径|PUT `<API_endpoint>`/ak/api/v1/permissions/users|
|**请求主体字段**| |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` 用户标识</li><li>`["reader", "writer", "manager"]` 至少需要一个值</li></ul> |
|**响应主体字段**| |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` 用户标识</li></ul>|
|授权|管理者|

#### 示例 curl 请求：编辑用户
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### 添加用户
{: #console-icp-manage-add-users-api}

|**请求**|  |
|-------------|-----------|
|路径|POST `<API_endpoint>`/ak/api/v1/permissions/users|
|**请求主体字段**| |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 至少需要一个值</li><li>`string` 可选</li></ul>|
|授权|管理者|

#### 示例 curl 请求：添加用户
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### 除去用户
{: #console-icp-manage-remove-users-api}

|**请求**|  |
|-------------|-----------|
|路径|DELETE `<API_endpoint>`/ak/api/v1/permissions/users|
|**请求主体字段**| |
| <ul><li>`users`</li></ul>| <ul><li>`string` 用户标识</li></ul>|
|**响应主体字段**| |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` 用户标识</li></ul>|
|授权|管理者|

#### 示例 curl 请求：除去用户
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### 使用 API 管理组件

可以查看 [{{site.data.keyword.blockchainfull_notm}} Platform API 参考](https://test.cloud.ibm.com/apidocs/blockchain)中提供的完整 API 集。

由于您要使用 API 与 {{site.data.keyword.cloud_notm}} Private 上的控制台进行通信，因此需要将 {{site.data.keyword.cloud_notm}} 提供的授权与控制台提供的认证配合使用。将 API 参考中的 ``Bearer Auth`` 替换为 ``-u <api_key>:<api_secret>``。此外，还需要向命令添加 ``-K`` 或 ``--insecure`` 标志，或者使用浏览器下载控制台 TLS 证书。不能使用**试用**选项卡对 {{site.data.keyword.cloud_notm}} Private 上的网络测试 API。

例如，以下 API 调用将返回有关在 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 服务实例上运行的所有组件的信息。
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
对于在 {{site.data.keyword.cloud_notm}} Private 上部署的控制台，相同的 API 调用将类似于以下请求。
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

可以使用 API 在部署控制台的集群上创建节点，也可以从其他集群或 {{site.data.keyword.cloud_notm}} 中导入节点。有关更多信息，请访问[使用 API 构建网络](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis)和[使用 API 导入网络](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis)。

## 查看日志
{: #icp-console-manage-logs}

使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台时，可能需要查看日志来调试问题。

### 查看控制台日志
{: #console-icp-manage-console-logs}

如果需要调试在使用控制台或操作节点时遇到的问题，可以轻松访问控制台日志。您还可以设置日志记录级别，以增大或减小控制台收集的日志量。控制台日志与[节点日志](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs)分开收集，节点日志由 {{site.data.keyword.cloud_notm}} Private 进行收集。

在控制台浏览器中，导航至**设置**选项卡以更改日志记录设置。控制台日志从两个不同的源进行收集：

  * **客户机日志记录：**通过浏览器向控制台发送命令时会收集这些日志。
  * **服务器日志记录：**控制台向节点发送命令时会收集这些日志，还会从控制台部署中收集这些日志。这些日志包含 Hyperledger Fabric 日志记录输出。

使用每种日志类型下的下拉列表来设置收集的日志量。例如，**错误**和**警告**收集的日志量最少，而**调试**和**所有**收集的日志量最多。

仅当以控制台管理员身份登录时，才能查看控制台日志。要在**设置**选项卡中查看日志，请将浏览器 URL 中的 `settings` 一词替换为 `api/v1/logs`。例如，如果控制台 URL 为 `localhost:3001/settings`，那么可以通过导航至 `localhost:3001/api/v1/logs` 来查看日志。客户机和服务器日志在不同的文件中进行收集。最近的日志位于页面顶部。

### 查看节点日志
{: #console-icp-manage-node-logs}

在命令行中使用 [kubectl CLI 命令](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}或通过 {{site.data.keyword.cloud_notm}} Private 集群中包含的 [Kibana](https://www.elastic.co/products/kibana){: external}，可以查看组件日志。

- 使用 `kubectl logs` 命令来查看 pod 内的容器日志。如果尚未[安装 kubectl cli](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}，请遵循指示信息进行安装。如果不确定 pod 名称，请运行以下命令来查看 pod 的列表。

  ```
  kubectl get pods
  ```
  {:codeblock}

  然后，运行以下命令检索位于 pod 内的节点容器的日志：

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  将 `<pod_name>` 替换为上面命令输出中 pod 的名称。  
  将 `<node>` 替换为 `ca`、`peer` 或 `orderer` 以查看您的节点的日志。  
  将 `<node>` 替换为 `fluentd` 以查看您的智能合同的日志。

  有关 `kubectl logs` 命令的更多信息，请参阅 [Kubernetes 文档](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}。

- 或者，可以通过 {{site.data.keyword.cloud_notm}} Private 控制台来[访问日志](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external}，这将在 Kibana 中打开日志。

  **注：**在 Kibana 中查看日志时，可能会收到响应 `No results found`。如果 {{site.data.keyword.cloud_notm}} Private 将工作程序节点 IP 地址用作其主机名，那么会发生此情况。要解决此问题，请在面板顶部除去以 `node.hostname.keyword` 开头的过滤器，随后相关日志即可显示。

### 查看智能合同容器日志
{: #console-icp-manage-container-logs}

如果遇到与智能合同相关的问题，可以查看智能合同（或链代码）容器日志来调试问题。可以运行以下命令来查看智能合同容器日志：

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

将 `<peer_pod>` 替换为正在运行链代码的同级 pod 的名称。使用 `kubectl get po` 命令来获取运行中 pod 的列表。

## 为节点安装补丁
{: #console-icp-manage-patch}

同级、CA 和排序节点的底层 {{site.data.keyword.IBM_notm}} Hyperledger Fabric Docker 映像可能需要在一段时间后进行更新，例如使用安全性更新进行更新，或者更新为新的 Fabric 单点版本。升级 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart 时，可以更新 Fabric 映像。

升级 Helm chart 后，如果组件更新可用，那么您能够在节点磁贴上找到**补丁可用**文本。此补丁可以随时在节点上进行安装。这些补丁是可选的，但建议进行安装。无法修补已导入到控制台的节点。

将补丁应用于节点时，一次只能应用一个。在应用补丁期间，节点不可用于处理请求或事务。因此，为了避免任何服务中断，应尽可能确保有相同类型的另一个节点可用于处理请求。在节点上安装补丁大约需要一分钟时间才能完成，更新完成后，节点即可处理请求。
{:note}

要将补丁应用于节点，请打开节点磁贴，然后单击**安装补丁**按钮。无法修补已导入到控制台的节点。
