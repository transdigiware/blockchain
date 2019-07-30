---

copyright:
  years: 2019
lastupdated: "2019-06-21"

keywords: APIs, build a network, authentication, service credentials, API key, API endpoint, IAM access token, Fabric CA client, import a network, generate certificates

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

# 使用 API 构建网络
{: #ibp-v2-apis}

{{site.data.keyword.blockchainfull}} Platform 会公开 RESTful API，供您创建、导入、编辑和删除区块链组件，以及管理日志记录、通知和控制台设置。您可以使用 API 和相应的 SDK 来开发与区块链网络交互的应用程序。
{: shortdesc}

本教程介绍了使用 {{site.data.keyword.blockchainfull_notm}} Platform API 构建区块链网络的通用流程。有关每个 API 的更多信息，请参阅 [{{site.data.keyword.blockchainfull_notm}} Platform API 参考文档](/apidocs/blockchain){: external}。

这些 API 仅与 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} V0.1.77 或更高版本兼容。要检查版本，请浏览至 `https://[your_console_url]/version.txt`，其中 *`[your_console_url]`* 是 {{site.data.keyword.blockchainfull_notm}} Platform 控制台的 URL。例如：https://1ee1869ffa6496d6bc1ce4b-optools.bp01.blockchain.cloud.ibm.com/version.txt
{:note}

## 开始之前
{: #ibp-v2-apis-prereq}

您必须在 {{site.data.keyword.cloud_notm}} 中有 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例，这样才能发出 API 调用与网络进行交互。如果您还没有服务实例，请遵循[入门指示信息](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)进行创建。

## 认证
{: #ibp-v2-apis-authentication}

为了使用 API 来访问通过 {{site.data.keyword.blockchainfull_notm}} Platform 创建的区块链网络，应用程序需要能够向 {{site.data.keyword.cloud_notm}} 进行认证，然后连接到服务实例。

### 检索服务凭证
{: #ibp-v2-apis-retrieve-service-credentials}

您需要基本认证凭证来确保您有权访问 {{site.data.keyword.cloud_notm}} 上的 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例。

1. 在 [{{site.data.keyword.cloud_notm}} 资源列表](https://cloud.ibm.com/resources){: external}中，打开创建的区块链服务实例。
2. 单击左侧导航器中的**服务凭证**。
3. 单击**服务凭证**页面上的**新建凭证**按钮以创建新凭证。
  1. 为凭证提供名称，例如 *UseAPIs*。
  2. 可以将**添加内联配置参数**保留为空白。
  3. 单击**添加**按钮。
4. 创建新凭证后，单击此凭证的**操作**标题下方的**查看凭证**。凭证的内容类似于以下示例：

  ```
  {
    "api_endpoint": "https://1088ac8563e44f5a92539d946733ad7e-optools.so01.blockchain.test.cloud.ibm.com"
    "apikey": "nvASKts6B6lMZGZUNTGVP7MLK2BujMnxz0plSPYaqc3R",
    "configtxlator": "https://1088ac8563e44f5a92539d946733ad7e-configtxlator.so01.blockchain.test.cloud.ibm.com",
    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:blockchain:us-south:a/9d46037caee397faa45c55e087d26baa:1088ac85-63e4-4f5a-9253-9d946733ad7e::",
    "iam_apikey_name": "auto-generated-apikey-c1981f5c-cff0-464f-9a78-ed2f52e24d1a",
    "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
    "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/9d46037caee397faa45c55e087d26baa::serviceid:ServiceId-774190c5-9c37-4f68-8572-8d6e2aabbc7e",
  }
  ```

在服务凭证中，可以找到检索 {{site.data.keyword.iamshort}} (IAM) 访问令牌所需的 **API 密钥** (`apikey`) 和 **API 端点** (`api_endpoint`)。

### 检索访问令牌
{: #ibp-v2-apis-retrieve-token}

可以通过从 IAM 检索访问令牌来向 {{site.data.keyword.blockchainfull_notm}} Platform 进行认证。通过访问令牌，您可以代表 {{site.data.keyword.cloud_notm}} 上或其外部的服务或应用程序来使用 {{site.data.keyword.blockchainfull_notm}} Platform API，而无需共享您的个人用户凭证。  

调用 {{site.data.keyword.iamshort}} API 来检索访问令牌。

```cURL
curl -X POST \
  "https://iam.cloud.ibm.com/identity/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \
```
{: codeblock}

在请求中，将 `<API_KEY>` 替换为服务凭证中的 `apikey` 值。以下截断的示例显示了令牌输出：

```
{
"access_token": "eyJraWQiOiIyM...",
"refresh_token": "...",
"expires_in":3600,
"expiration":1555558683,
"scope":"ibm openid"}
```
{: screen}

使用 API 通过以 `Bearer` 令牌类型为前缀的完整 `access_token` 值，以编程方式使用区块链网络。

访问令牌有效期为一小时，但可以根据需要重新生成访问令牌。要保持对服务的访问权，请通过调用 {{site.data.keyword.iamshort}} API 来定期刷新 API 密钥的访问令牌。   
{: tip}

## 构成 API 请求
{: #ibp-v2-apis-form-api-request}

在对服务进行 API 调用时，请根据最初供应 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例的方式来构造 API 请求。

要构建请求，请将 API 端点与以下格式的相应认证凭证配对使用：

```cURL
curl -X <API method> \
    <API_endpoint>/<API> \
    -H 'authorization: Bearer <access_token>' \
    -H 'Content-Type: application/json' \
    -d '<request body>' \
```
{: codeblock}

在 [{{site.data.keyword.blockchainfull_notm}} Platform API 参考文档](/apidocs/blockchain){: external}中，为每个 API 提供了示例 curl 命令。

此外，可以使用“API 参考”文档中的**试用**功能来测试 API 调用，然后再将其添加到应用程序中。您需要登录到 {{site.data.keyword.cloud_notm}} 才能使用**试用**功能。可以从下拉列表中选择任何服务实例。所有 API 请求都会发送到 API 端点中指定的网络。

## 限制
{: #ibp-v2-apis-limitations}

您只能从其他 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} 网络中导入现有 CA、同级和排序节点。

## 使用 API 构建网络
{: #ibp-v2-apis-build-with-apis}

可以使用 API 在 {{site.data.keyword.blockchainfull_notm}} Platform 的实例中创建区块链组件。请使用以下步骤通过 {{site.data.keyword.blockchainfull_notm}} API 来构建区块链网络。


1. 通过调用 [`POST /ak/api/v1/kubernetes/components/ca`](/apidocs/blockchain?code=try#create-a-ca) 来创建认证中心 (CA)。

  请记住您的输入和响应，稍后您将需要这些信息。
  {: tip}

  您需要等待 CA 启动。这可能需要几分钟时间，具体取决于环境。可以调用 `GET <ca_url>/cainfo` API 来检查 CA 状态。您将收到重复的错误，然后会收到 `200` 状态代码，这意味着您可以继续执行下一步。请注意，此 API 调用会在一分钟后超时。

2. 使用 CA 来注册组件和管理员身份，然后生成必需的证书。可以使用 Fabric CA 客户机来完成以下步骤：

  - [设置 Fabric CA 客户机](#ibp-v2-apis-config-fabric-ca-client)。
  - [使用 CA 管理员生成证书](#ibp-v2-apis-enroll-ca-admin)。
  - [向 CA 注册新组件](#ibp-v2-apis-config-register-component)。
  - 您还需要[注册组织管理员](#ibp-v2-apis-config-register-admin)，然后在 MSP 文件夹中[为管理员生成证书](#ibp-v2-apis-config-enroll-admin)。如果您已经注册了管理员身份，那么无需完成此步骤。
  - [向 TLS CA 注册新组件](#ibp-v2-apis-config-register-component-tls)。

  您还可以使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台来完成这些步骤。有关更多信息，请参阅[创建和管理身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities)。

3. 通过调用 [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?#import-a-membership-service-provide-msp) [为组织创建 MSP 定义](#ibp-v2-apis-msp)。

4. [构建配置文件](#ibp-v2-apis-config)，创建排序服务或同级时需要配置文件。您必须为要创建的每个排序服务或同级构建唯一的配置文件。如果要部署多个排序节点，那么需要为要创建的每个节点提供一个配置文件。

5. 通过调用 [`POST /ak/api/v1/kubernetes/components/orderer`](/apidocs/blockchain?code=try#create-an-ordering-service) 来创建排序服务。

6. 通过调用 [`POST /ak/api/v1/kubernetes/components/peer`](/apidocs/blockchain?code=try#create-a-peer) 来创建同级。

7. 如果要使用控制台来操作区块链组件，那么必须将管理员身份导入到控制台电子钱包。使用“电子钱包”选项卡将节点管理员的证书和专用密钥导入到控制台，然后创建身份。接着，需要使用控制台将此身份与创建的组件相关联。有关更多信息，请参阅[将管理员身份导入到 {{site.data.keyword.blockchainfull_notm}} Platform 控制台](#ibp-v2-apis-admin-console)。

8. 部署网络后，可以使用 Fabric SDK、同级 CLI 或控制台 UI 来创建通道，然后安装或实例化智能合同。

用于 API 认证的服务凭证必须在 IAM 中具有`管理者`角色才能创建组件。请参阅有关[用户角色](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove)的主题中的表，以获取更多信息。
{: note}

## 使用 API 导入网络
{: #ibp-v2-apis-import-with-apis}

您还可以使用 API 将通过 API 或 {{site.data.keyword.blockchainfull_notm}} Platform 控制台创建的 {{site.data.keyword.blockchainfull_notm}} 组件导入到 {{site.data.keyword.blockchainfull_notm}} Platform 的其他服务实例。

1. 通过调用 [`POST /ak/api/v1/components/ca`](/apidocs/blockchain?code=try#import-a-ca) 来导入 CA。

  请记住您的输入和响应，稍后您将需要这些信息。
  {: tip}

  您需要等待 CA 启动。这可能需要几分钟时间，具体取决于环境。可以调用 [`GET /components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) 来检查 CA 状态。您将收到重复的错误，然后会收到 `200` 状态代码，此时可转至下一步。请注意，此 API 调用会在一分钟后超时。

2. 通过调用 [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp) 来导入组织 MSP 定义。

3. 通过调用 [`POST /ak/api/v1/components/orderer`](/apidocs/blockchain?code=try#import-a-ordering-service) 来导入排序服务。

4. 通过调用 [`POST /ak/api/v1/components/peer`](/apidocs/blockchain?code=try#import-a-peer) 来导入同级。

5. 如果计划使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台来操作区块链组件，那么必须将组件管理员身份导入到控制台电子钱包。有关更多信息，请参阅[将管理员身份导入到 {{site.data.keyword.blockchainfull_notm}} Platform 控制台](#ibp-v2-apis-admin-console)。

6. 部署网络后，可以使用 Fabric SDK、同级 CLI 或控制台 UI 来创建通道，然后安装或实例化智能合同。

用于 API 认证的服务凭证必须在 IAM 中具有`写入者`角色才能导入组件。请参阅有关[用户角色](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove)的主题中的表，以获取更多信息。
{: note}

## 使用 Fabric CA 客户机操作 CA
{: #ibp-v2-apis-config-fabric-ca-client}

可以使用 Fabric CA 客户机来操作 CA。运行以下 Fabric CA 客户机命令来注册组件和管理员身份，然后生成必需的证书。

### 设置 Fabric CA 客户机
{: #ibp-v2-apis-setup-fabric-ca-client}

1. 将 [Fabric CA 客户机](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#fabric-ca-client){: external}下载到本地文件系统。

  获取 Fabric CA 客户机的最简单方法是直接下载所有 Fabric 工具二进制文件。导航至要在其中使用命令行下载二进制文件的目录，然后通过运行以下 [Curl](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html#install-curl){: external} 命令来访存这些二进制文件。

  ```
  curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.0 1.4.0 -d -s
  ```
  {:codeblock}

  此命令将在 `bin/` 目录中安装二进制文件。

2. 将 `PATH` 路径设置为 Fabric 工具二进制文件的下载目录：

  ```
  export PATH=$PATH:<full/path/to/fabric-client/bin>
  ```
  {:codeblock}

  例如，如果已将二进制文件安装在主目录中，那么应将 `PATH` 设置为：

  ```
  export PATH=$PATH:$HOME/bin
  ```
  {:codeblock}

3. 创建用于存储 CA 管理员证书的文件夹。

  ```
  mkdir -p $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

4. 将 `$FABRIC_CA_CLIENT_HOME` 环境变量的值设置为 CA 客户机将用于存储所生成 MSP 证书的路径。请确保除去早先可能尝试创建的配置资料。如果之前未运行过 `enroll` 命令，那么 `msp` 文件夹和 `.yaml` 文件不存在。

  ```
  export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/ca-admin
  rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
  rm -rf $FABRIC_CA_CLIENT_HOME/msp
  ```
  {:codeblock}

5. 检索要由 Fabric CA 客户机使用的 CA 的 TLS 证书。如果使用的是 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，请打开 CA 并单击**设置**，然后在 **TLS 证书**字段中查找 Base64 格式的证书。如果使用的是 API，那么可以调用 [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components)，然后在 `"PEM"` 字段中查找 CA TLS 证书。如果是使用`创建 Fabric CA` API 创建的 CA，那么还可以在响应主体中找到 TLS 证书。

  您需要将证书从 Base64 转换为 PEM 格式，才能将其用于与 CA 进行通信。将 TLS 证书的 Base64 编码的字符串插入到下面的命令中。请确保您位于 `$HOME/fabric-ca-client` 目录中。

  ```
  cd $HOME/fabric-ca-client
  mkdir catls
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > tls.pem
  ```
  {:codeblock}

### 使用 CA 管理员生成证书
{: #ibp-v2-apis-enroll-ca-admin}

创建 CA 时，会自动为您注册 **CA 管理员**身份。现在，您可以使用该管理员名称和密码向 Fabric CA 客户机发出 `enroll` 命令，以生成包含证书的 MSP 文件夹，然后这些证书可用于注册其他同级或排序节点身份。

1. 确保完成[设置 Fabric CA 客户机](#ibp-v2-apis-config-fabric-ca-client)的步骤，并确保将 `$FABRIC_CA_CLIENT_HOME` 设置为要用于存储 CA 管理员证书的目录。

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

2. 运行以下命令生成证书：

  ```
  fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name>  --tls.certfiles  <ca_tls_cert_path>
  ```
  {:codeblock}

  命令中的 `<enroll_id>` 和 `<enroll_password>` 是创建 CA 时指定的 `enroll_id` 和 `enroll_secret`。请使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台中的**认证中心端点 URL**，或使用 API 调用返回的 `"ca_url"` 作为 `<ca_url_with_port>` 的值。请去除开头的 `http://`。`<ca_name>` 是控制台中的 **CA 名称**，或者是 API 返回的 `"ca_name"`。

  `<ca_tls_cert_path>` 是 CA TLS 证书的完整路径。

  实际调用可能类似于以下示例命令：

  ```
  fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```  
  {:codeblock}

**提示：**如果注册 URL 的值（即 `-u` 参数值）包含特殊字符，那么需要对特殊字符编码，或者用单引号将 URL 括起。例如，`!` 变成 `%21`，或者命令类似以下内容：

  ```
  ./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname ca
  ```
  {:codeblock}

  此 `enroll` 命令会生成完整的证书集，称为成员资格服务提供者 (MSP) 文件夹，该文件夹位于设置为 Fabric CA 客户机的 `$HOME` 路径的目录中。例如，`$HOME/fabric-ca-client/ca-admin`。有关 MSP 以及 MSP 文件夹所包含内容的更多信息，请参阅[成员资格服务提供者](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates-msp)。

  如果此 `enroll` 命令失败，请参阅[故障诊断主题](#ibp-v2-apis-config-troubleshooting)以了解可能的原因。

  您可以运行 tree 命令来验证是否已完成所有必备步骤。浏览至存储证书的目录。tree 命令应该生成类似于以下结构的结果：

  ```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── IssuerPublicKey
  │       ├── IssuerRevocationPublicKey
  │       ├── keystore
│       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── catls
    └── tls.pem    
```

### 向 CA 注册组件身份
{: #ibp-v2-apis-config-register-component}

首先，您需要使用 CA 注册组件身份。组件将使用此身份来生成它在部署时所需的证书。

1. 通过 Fabric CA 客户机[使用 CA 管理员生成证书](#ibp-v2-apis-enroll-ca-admin)。使用这些管理员证书发出以下命令。确保 `$FABRIC_CA_CLIENT_HOME` 设置为 `$HOME/fabric-ca-client/ca-admin`。

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```

2. 发出以下命令查找亲缘关系和组织名称。
  ```
  fabric-ca-client affiliation list --caname <ca_name> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  命令可能类似于以下示例：
  ```
  fabric-ca-client affiliation list --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  您应该看到与以下示例类似的信息：
  ```
  affiliation: org1
      affiliation: org1.department1
  ```

  记下第二个 **affiliation** 值，例如，`org1.department1`。您需要在下面的命令中使用此值。

3. 运行以下命令注册排序节点或同级。

  ```
  fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type <component_type> --id.secret <secret> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  为组件创建名称和密码，然后将其用于替换 `name` 和 `secret`。请记下这些信息。如果要部署排序节点，请将 `--id.type` 设置为 `orderer`，如果要部署同级，请将其设置为 `peer`。该命令可能类似于以下示例：

  ```
  fabric-ca-client register --caname ca --id.affiliation org1.department1 --id.name peer1 --id.secret peer1pw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  您需要保存上述命令中的 `"enrollid"` 和 `"enrollsecret"`，以在创建配置文件时使用。
  {: important}

  您只能注册一次身份。如果遇到问题，请尝试使用新的用户名和密码来执行命令。作为一项安全措施，每个身份以及附带的注册标识和私钥仅用于部署一个同级。虽然可以将一个**管理员**身份用于多个组件（这是建议的部署策略），但不要复用同级标识和密码。

  命令成功完成后，应该会看到类似于以下示例的信息：

  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: peerpw
  ```

### 注册组织管理员
{: #ibp-v2-apis-config-register-admin}

您还需要创建可用于操作网络的管理员身份。您将使用此身份来操作特定组件，例如通过在同级上安装智能合同。还可以将此身份用作组织的管理员，并将其用于创建和编辑通道。  

您首先需要使用 CA 注册此新身份，然后将其用于生成 MSP 文件夹。可以通过将此身份的 signCert 添加到组织 MSP，使此身份成为组织管理员。此外，还需要将 signCert 添加到配置文件，以便可以使其在部署期间成为排序节点或同级的管理员证书。您只需要针对组织创建一个管理员身份。因此，只需要完成这些步骤一次。可以使用生成的 signCert 来部署多个同级或排序节点。

确保将 `$FABRIC_CA_CLIENT_HOME` 设置为 CA 管理员的 MSP 的路径。

```
echo $FABRIC_CA_CLIENT_HOME
$HOME/fabric-ca-client/ca-admin
```
{:codeblock}

通过运行以下命令向 CA 注册组件管理员身份：

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type client --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

为管理员创建新的用户身份 `name` 和 `secret`。请确保使用的值与您刚刚注册的同级或排序节点身份的值不同。该命令类似于以下示例：

```
fabric-ca-client register --caname ca --id.name peeradmin --id.affiliation org1.department1 --id.type client --id.secret peeradminpw --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

请记下这些信息。您将使用此 `name` 和 `secret` 通过 `enroll` 命令来生成 MSP 文件夹。
{: important}

### 生成管理员成员资格服务提供者 (MSP) 文件夹
{: #ibp-v2-apis-config-enroll-admin}

注册组件管理员之后，可以通过运行以下命令来生成证书：

```
fabric-ca-client enroll -u https://<peer_admin_enroll_id>:<peer_admin_enroll_password>@<ca_url_with_port> --caname <ca_name> -M $HOME/path/to/peer-admin/msp --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

例如：

```
fabric-ca-client enroll -u https://peeradmin:peeradminpw@9.30.94.174:30167 --caname ca -M $HOME/fabric-ca-client/peer-admin/msp --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{:codeblock}

此命令成功完成后，将在使用 `-M` 标志指定的目录中生成新的 MSP 文件夹。此目录包含需要用于操作组件的证书。可以使用 tree 命令来验证 enroll 命令是否有效。浏览至存储证书的目录。tree 命令应该生成类似于以下结构的结果：


```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── keystore
│       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── catls
│   └── tls.pem
├── orderer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── dfe06060490bf62e2bd709433fc747ff28cdbb1e040682c5d47a4e8598db4f2e_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── peer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 24f76217c5c7e641ee29b068712181294e427cbee4dbfce230a1a98b53489cbe_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── tlsca-admin
    ├── fabric-ca-client-config.yaml
    └── msp
        ├── cacerts
        │   └── 9-30-250-70-30395-tlsca.pem
        ├── IssuerPublicKey
        ├── IssuerRevocationPublicKey
        ├── keystore
        │   └── 45a7838b1a91ddfe3d4d22a5a7f2639b868493bcce594af3e3ceb9c07899d117_sk
        ├── signcerts
        │   └── cert.pem
        └── user
```

创建组织 MSP 定义和配置文件时，需要返回到此文件夹。
{: important}

### 向 TLS CA 注册组件身份
{: #ibp-v2-apis-config-register-component-tls}

创建 CA 时，已随缺省 CA 一起部署了 TLS CA。您还需要使用 TLS CA 来注册排序节点或同级。为此，首先需要使用 TLS CA 的管理员进行注册。将 `$FABRIC_CA_CLIENT_HOME` 更改为要存储 TLS CA 管理员证书的目录。

```
cd $HOME/fabric-ca-client
mkdir tlsca-admin
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/tlsca-admin
```
{:codeblock}

运行以下命令以注册为 TLS CA 的管理员。TLS CA 管理员的注册标识和密码与缺省 CA 的相同。因此，以下命令与用于注册为 [CA 管理员](#ibp-v2-apis-enroll-ca-admin)的命令相同，只是使用的是 TLS CA 的名称。TLS CA 名称为控制台的 CA **设置**面板中的 **TLS CA 名称**值，或为`创建 CA` API 返回的 `"tlsca_name"` 值。

```
fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <tls_ca_name> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

实际调用可能类似于以下示例：

```
  fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname tlsca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

您注册之后，就有了必需的证书来向 TLS CA 注册组件。运行以下命令注册排序节点或同级：

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type peer --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

此命令类似于用于向 CA 注册组件身份的命令，但需要使用的是 TLS CA 名称。如果要部署排序节点而不是同级，请将 `--id.type` 设置为 `orderer` 而不是 `peer`。您为此身份提供的用户名和密码必须不同于对缺省 CA 使用的用户名和密码。实际注册可能类似于以下命令：

```
fabric-ca-client register --caname tlsca --id.affiliation org1.department1 --id.name peertls --id.secret peertlspw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

您需要保存上述命令中的 `"enrollid"` 和 `"enrollsecret"`，以在创建配置文件时使用。
{: important}

### 故障诊断
{: #ibp-v2-apis-config-troubleshooting}

#### **问题：**运行 `enroll` 命令时出错
{: #ibp-v2-apis-config-enroll-error1}

运行 Fabric CA 客户机 enroll 命令时，该命令可能会失败并返回以下错误：

```
Error: Failed to read config file at '/Users/chandra/fabric-ca-client/ca-admin/fabric-ca-client-config.yaml': While parsing config: yaml: line 42: mapping values are not allowed in this context
```
{:codeblock}

**解决方案：**

Fabric CA 客户机尝试注册但无法连接到 CA 时，会发生此错误。如果存在以下情况，那么会发生此错误：   

- `enroll` 命令在 `-u` 参数上包含多余的 `https://`。
- CA 名称不正确。
- 您的用户名或密码不正确。

查看在 `enroll` 命令中指定的参数，并确保上述情况都不存在。

#### **问题：**运行 `enroll` 命令时 CA URL 出错
{: #ibp-v2-apis-config-enroll-error2}

如果注册 URL 的值（即 `-u` 参数值）包含特殊字符，那么 Fabric CA 客户机的 enroll 命令可能会失败。例如，包含注册标识和密码 `admin:C25A06287!0` 的以下命令，

```
./fabric-ca-client enroll -u https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241 --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```

会失败，并生成以下错误：

```
!pw@9.12.19.115: event not found
```

#### **解决方案：**
{: #ibp-v2-apis-config-enroll-error2-solution}

您需要对特殊字符编码，或者用单引号将 URL 括起。例如，`!` 变成 `%21`，或者命令类似以下内容：

```
./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```

## 创建组织 MSP 定义
{: #ibp-v2-apis-msp}

可以使用 API 通过调用 [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp) 来创建组织 MSP 定义。此 MSP 包含用于在区块链联盟中定义组织的证书，以及可用于操作网络的管理员证书。如果您执行了上述步骤，那么已生成创建组织 MSP 所需的证书。请使用以下步骤来完成 API 调用的请求主体。

1. 输入 `msp` 作为请求 `type`。

2. 为组织选择 MSP 标识。MSP 标识是组织在联盟中的正式名称。用于创建组织 MSP 的 MSP 标识需要与用于部署同级的 MSP 标识相同。

3. 为 MSP 选择显示名称。

4. 您需要提供使用 Fabric CA 客户机注册和登记的组织管理员的 signCert（Base64 格式）。  

  导航至[使用组织管理员生成证书](#ibp-v2-apis-config-enroll-admin)时创建的 MSP 目录。
  ```
cd $HOME/fabric-ca-client/peer-admin/msp
```
在此 MSP 目录中，打开新用户的 signCert 文件，并使用以下命令将其转换为 Base64 格式：

  ```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
  {:codeblock}

  **注：**使用以上命令生成的字符串的格式务必为一行。应该类似于以下内容：

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
  ```
   而不能如下所示：

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
  ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
  Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
  VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
  WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
  ```

  例如：

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  此命令将显示类似于以下示例的字符串：

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
  ```

  为 API 请求的 `admins` 字段提供此字符串。

5. 还需要提供 CA 的根证书。此证书是在[使用 CA 管理员生成证书](#ibp-v2-apis-enroll-ca-admin)时创建的。

  导航至 CA 管理员 MSP 目录。
  ```
  cd $HOME/fabric-ca-client/ca-admin/msp
  ```
  在此 MSP 目录中，打开 cacerts 文件夹，并使用以下命令将该文件夹中的证书转换为 Base64 格式：
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-ca-admin>/msp/cacerts/<ca_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  这会将根证书作为 Base64 编码的字符串输出。

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWIyZ0F3SUJBZ0lVQmZnZzcvVnIrL25OVEFNQlQ4UUtHL00wQU8wd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU1TURVd016RXpNamt3TUZvWERUTTBNRFF5T1RFek1qa3dNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUVXMUtvN2lWeVE2VWkwdDVqbU5KaWVuSUwKR3pNM1BDWHlhL2VSQ0NWMmFQb0dTZ1lrVUg2UWN5RjAzbFlMZFU4Y0drNTQ0alViVC9KT1lYeVgzTWc4bHFORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZDK2lJR0NSb2Zvb3FsVkZoU3dOMmk2MXNJaVBNQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUURTYW9RL1E0QzkKbFl1VGNhVXVHb3d6YmhUZHBuN2F3S2lHN1Nvd2lSQXVld0lnUWlyM3RNR3IvYWo2aU5lRXJFN2NyOVowQ0gvTwp3QnNQcWd4RVR3MjVqZUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  为 API 请求的 `root_certs` 字段提供此字符串。

6. 您还应该提供 TLS CA 的根证书。TLS 根证书允许同级参与通道上的 Gossip。

  导航至[注册 TLS CA 管理员](#ibp-v2-apis-config-register-component-tls)时生成的 MSP 目录。
  ```
  cd $HOME/fabric-ca-client/tlsca-admin/msp
  ```
  在此 MSP 目录中，打开 cacerts 文件夹，并使用以下命令将该文件夹中的证书转换为 Base64 格式：
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-tlsca-admin>/msp/cacerts/<tls_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  这会将 TLS CA 根证书作为 Base64 编码的字符串输出。

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVWUVQWnprNXV2b3dobEtacG5JMXplODdIQUlnd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEExTURNeE16STVNREJhRncwek5EQTBNamt4TXpJNU1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFRdSs2UnZWd2w5T2dDVlAraEVxbjVxdExRVG9LWkw4a1lic0pOeU1JbERoc3hlNWx6cW1zQkoKbTk2eUR2TVV6OSsxL2pzb1M4M1JqMVAwc3M2TnJNb3FvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVVnUEc4anJEK1BxVjdoelc3WDlsbTFrMS91WjR3CkZRWURWUjBSQkE0d0RJY0VxVGJESW9jRUNwbnBkVEFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUVBenk3cHJZaVMKQmlDVWdYeWRkY09WMm9mZmtqaEI0N091QXFjQWNqZS9SWkVDSUdKZFgzZ1ErTDRIN3duY1RoZkwrenU1ejV1UApGUWhXTmlNS3hQWEYrZnYwCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  为 API 请求的 `tls_root_certs` 字段提供此字符串。

## 创建配置文件
{: #ibp-v2-apis-config}

您需要完成配置文件，才能使用 API 来创建同级或排序节点。此文件作为 API 调用的请求主体中的 `config` 对象提供给 API。如果要创建多个排序节点，那么需要将您要创建的每个节点的配置文件放在数组中提供给 API 请求。例如，对于五节点 Raft 排序服务，需要创建包含五个配置文件的数组。只要提供的 enrollID 具有足够高的注册限制，就可以为每个节点提供相同的文件。您需要将 CA 部署到 {{site.data.keyword.cloud_notm}} Platform 服务实例，并在完成文件之前执行相应步骤来注册和登记必需的身份。

下面是配置文件的模板：
```
{
	"enrollment": {
		"component": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"admincerts": [""]
		},
		"tls": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"csr": {
				"hosts": [""]
			}
		}
	}
}
```
{:codeblock}

将这整个文件复制到文本编辑器中，您可以在其中编辑该文件并将其作为 JSON 文件保存到本地文件系统中。使用以下步骤来完成此配置文件，然后将其用于部署排序服务或同级。

### 检索 CA 连接信息
{: #ibp-v2-apis-config-connx-info}

首先，需要在 {{site.data.keyword.cloud_notm}} Platform 上提供 CA 的连接信息。可以使用控制台 UI 或 API 来获取有关 CA 的必需信息。

**如果使用的是 {{site.data.keyword.blockchainfull_notm}} Platform 控制台：**在控制台中打开 CA 并单击**设置**，然后单击**导出**按钮以将 CA 信息导出到 JSON 文件。可以使用此文件中的值来完成配置文件。

**如果使用的是 API：**可以调用 [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) 来获取 CA 的连接信息。如果是使用`创建 Fabric CA` API 创建的 CA，那么还可以在响应主体中找到必需的信息。

- `"cahost"` 和 `"caport"` 值会在响应主体中或导出的 CA JSON 文件中的 `ca_url` 字段中显示。例如，如果 `ca_url` 为 https://9.30.94.174:30167，那么 `"cahost"` 的值将为 `9.30.94.174`，`"caport"` 的值将为 `30167`。
- `"caname"` 是部署 CA 时指定的 CA 名称。这是响应主体中或导出的 JSON 文件中 `ca_name` 字段的值。
- `"cacert"` 是 CA 的 Base64 编码的 TLS 证书。这是响应主体中或导出的 JSON 文件中 `pem` 字段的值。

- 以下 `"tls"` 部分中的字段需要您传递到上面 component 部分的相同信息，但您需要使用 CA 部署期间指定的 CA TLS 实例名称的值。将 `caname` 替换为响应主体中或导出的 JSON 文件中 `tlsca_name` 的值。对 `"cacert"` 值使用相同的 TLS 证书。
  ```
  "tls": {
    "cahost": "",
    "caport": "",
    "caname": "",
    "catls": {
      "cacert": ""
  ```
  {:codeblock}

### 提供组件的注册标识和私钥

1. 将用于[使用缺省 CA 注册组件](#ibp-v2-apis-config-register-component)的 `name` 和 `secret` 粘贴为配置文件中 `"component"` 部分下的 `"enrollid"` 和 `"enrollsecret"`：

  ```
  "component": {...
    },
    "enrollid": "peer1",
    "enrollsecret": "peer1pw",
  ```
2. 将用于[使用 TLS CA 注册组件](#ibp-v2-apis-config-register-component-tls)的 `name` 和 `secret` 粘贴为配置文件中 `"tls"` 部分下的 `"enrollid"` 和 `"enrollsecret"`：

  ```
  "tls": {...
    },
    "enrollid": "peertls",
    "enrollsecret": "peertlspw",
  ```

### 提供组织管理员的 signCert

导航至[使用组织管理员生成证书](#ibp-v2-apis-config-enroll-admin)时创建的 MSP 目录。
```
cd $HOME/fabric-ca-client/peer-admin/msp
```
在此 MSP 目录中，打开新用户的 signCert 文件，并使用以下命令将其转换为 Base64 格式：

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

**注：**使用以上命令生成的字符串的格式务必为一行。应该类似于以下内容：

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
```
而不能如下所示：

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
```

例如：

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

此命令将显示类似于以下示例的字符串：

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
```

将此字符串复制到配置文件中 component 部分下的 `"admincerts"` 字段中。

### CSR（证书签名请求）主机

可以选择使用组件文件的 `"csr"` 部分为组件提供定制域。

```
"csr": {
  "hosts": [""]
  }
  ```
{:codeblock}

此部分是针对高级用户提供的，用于指定可用于对同级端点寻址的定制主机名。大部分用户可以将此部分保留为空白。

### 完成配置文件
{: #ibp-v2-apis-config-file}

完成上述所有步骤后，更新后的配置文件可能类似于以下示例：


```
{
    "enrollment": {
        "component": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "ca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJ0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peer1",
            "enrollsecret": "peer1pw",
            "admincerts": [
                "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzRENDQW9PZ0F3SUJBZ0lVS2Vib0drZEJqenM5bllRbkVQTWp0VG56YTVrd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE9URTNNRE13TUZvWERURTVNVEV4T1RFM01EZ3dNRm93ZmpFTE1BaKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRXVNQXNHQTFVRUN4TUVkWE5sY2pBTEJnTlZCQXNUQkc5eVp6RXdFZ1lEVlFRTEV3dGtaWEJoCmNuUnRaVzUwTVRFUU1BNEdBMVVFQXhNSFlXUnRhVzR4Y0RCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUgKQTBJQUJLbjUwdEU5TmpZb0RFNDBqalh6RUJ2T2c3Y3RtOElRd0laMnRkS29iNEwwVVhKdSs1Tkt5S2dyUk9vbApWcjBmQmg5cWZWMjl4Nms0T2dmMFNiVklBZWlqZ2ZRd2dmRXdEZ1lEVlIwUEFRSC9CQVFEQWdlQU1Bd0dBMVVkCkV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZOYWFkV0VzcGp2Smk1akpiVktiS2M3ZU1wUmhNQjhHQTFVZEl3UVkKTUJhQUZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQ2NHQTFVZEVRUWdNQjZDSEdOb1lXNWtjbUZ6TFcxaQpjQzV5WVd4bGFXZG9MbWxpYlM1amIyMHdhQVlJS2dNRUJRWUhDQUVFWEhzaVlYUjBjbk1pT25zaWFHWXVRV1ptCmFXeHBZWFJwYjI0aU9pSnZjbWN4TG1SbGNHRnlkRzFsYm5ReElpd2lhR1l1Ulc1eWIyeHNiV1Z1ZEVsRUlqb2kKWVdSdGFXNHhjQ0lzSW1obUxsUjVjR1VpT2lKMWMyVnlJbjE5TUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSURGeApDYzE1bDZUZ1dqYnhSZzlmNjczOGV0K0NZZ1I3VVpGR200VHdoQk5jQWlBNmtUMFFwbDV6WnBUN3BzM0dySXlVCmEydDRHSTQ5ZTdjUm5PMmdrSzl6Z3c9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg=="
            ]
        },
        "tls": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "tlsca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peertls",
            "enrollsecret": "peertlspw",
            "csr": {
                "hosts": [
                ]
            }
        }
    }
}
```
{:codeblock}

可以将其他字段保留为空白。完成此文件后，可以将此文件作为 `config` 字段传递给`创建排序服务`或`创建同级` API 的请求主体。

### 将管理员身份导入到 {{site.data.keyword.blockchainfull_notm}} Platform 控制台
{: #ibp-v2-apis-admin-console}

如果要使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台来操作区块链组件，那么必须将管理员身份导入到控制台电子钱包。在控制台中打开“电子钱包”面板。单击概述屏幕上的**添加身份**按钮。单击此按钮将打开一个侧面板，在其中可以将身份的签名证书和专用密钥直接添加到控制台。
- **名称：**输入身份的显示名称，此名称仅供您引用。
- **证书：**上传管理员的签名证书。如果遵循了上述指示信息进行操作，那么可以在 `$HOME/fabric-ca-client/peer-admin/msp/signcerts/` 文件夹中找到此证书。
- **专用密钥：**上传管理员的专用密钥。如果遵循了上述指示信息进行操作，那么可以在 `$HOME/fabric-ca-client/peer-admin/msp/keystore/` 文件夹中找到此密钥。

导入管理员身份后，可以将此身份与已创建的组件相关联。然后，可以使用控制台来操作网络。
