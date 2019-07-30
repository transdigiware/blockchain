---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: create identities, manage identities, Certificate Authorities, register, enroll, TLS CA, wallet, certificate expiration

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:important: .important}
{:note: .note}
{:pre: .pre}

# 创建和管理身份
{: #ibp-console-identities}

{{site.data.keyword.blockchainfull_notm}} Platform 的节点基于 Hyperledger Fabric，可构建许可的区块链网络。这意味着区块链联盟的所有参与者都需要拥有由公共密钥基础架构持续验证的身份。通过 {{site.data.keyword.blockchainfull_notm}} Platform 控制台，可以使用组织的认证中心 (CA) 来创建这些身份。您需要将这些身份存储在控制台电子钱包中，以便可以将其用于操作您的网络。

**目标受众：**本主题适用于负责创建、监视和管理区块链网络的网络操作员。

## 管理认证中心
{: #ibp-console-identities-manage-ca}

CA 类似于公共可信公证方，充当多个参与方之间的信任锚点，联盟中每个组织会维护其自己的 CA。CA 会创建属于组织的身份，并为每个身份签发签名证书和专用密钥。这些密钥允许所有节点和应用程序对自已的操作进行签名和验证。有关如何使用 CA 来建立身份的更多详细信息，请访问 Hyperledger Fabric 文档中的 [Identity](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html){: external} 主题。

使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台创建 CA 时，将使用相同的端点 URL 创建两个 CA：根 CA 和 TLS CA。可以在控制台的**节点**页面中查看相同显示名称下的这两个 CA。根 CA 为节点和应用程序提供密钥。TLS CA 提供用于保护网络中通信的证书。要了解有关网络上 TLS 的更多信息，请参阅[使用 TLS CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-tlsca)。

创建节点时，需要使用根 CA 来创建以下身份，部署和操作网络以及与网络进行交互需要这些身份：
- **CA 管理员：**CA 包含缺省 CA 管理员身份。此身份具有在部署 CA 期间指定的注册标识和私钥（类似于用户名和密码）。可以使用此管理员用户名和密码来操作 CA 并创建新身份。如果是使用控制台创建的 CA，那么根 CA 的 CA 管理员也是 TLS CA 的管理员，除非您决定创建单独的 TLS CA 管理员。
- **同级或排序节点：**您需要为属于组织的每个同级和排序节点注册身份。然后，节点将在部署期间使用这些身份来生成自己加入网络所需的密钥。为了安全起见，请为部署的每个节点创建唯一的注册标识和私钥。
- **同级或排序节点管理员：**{{site.data.keyword.blockchainfull_notm}} Platform 节点使用其内部的组件管理员身份的签名证书进行部署。这些证书允许管理员通过远程客户机或使用控制台来操作组件。
- **组织管理员：**加入由排序服务托管的联盟时，您会提供将成为组织管理员的身份的签名证书。可以使用这些身份来创建或编辑通道。
- **应用程序：**应用程序需要在提交事务以供网络验证之前对事务签名。您需要创建可用于对来自客户机应用程序的事务进行签名的身份。

可以使用控制台通过[注册过程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-register)来创建这些身份。注册管理员身份后，需要为每个身份签发签名证书和专用密钥，向组织 MSP 定义提供签名证书，以及将身份添加到控制台电子钱包。在[创建组织 MSP](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-create-msp) 时，可以对一个管理员身份完成这些步骤。您可以将不同的身份用作组织管理员或节点管理员，也可以使用一个身份执行这两项任务。[构建网络教程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)使用一个身份作为教程中创建的每个组织的管理员。

## 设置 CA 身份
{: #ibp-console-identities-ca-identity}

使用身份之前，需要使用创建 CA 期间创建的管理员身份或通过建立新的管理员身份来设置 CA 管理员的身份。在**节点**选项卡中打开 CA。

当前处于活动状态的身份的注册标识会显示在 CA 面板左侧，位于“CA 名称”、“节点位置”和“Fabric 版本”下方。可以使用此身份通过**注册**按钮来创建其他身份。CA 管理员还允许您获取已注册身份的列表，然后使用这些身份通过**注册**按钮来生成证书。

要切换到其他身份以用作 CA 管理员，请单击当前设置为 CA 管理员的身份。这将打开**关联身份**滑块。使用**注册标识**选项卡可提供其他 CA 管理员的注册标识和私钥。可以使用**现有身份**选项卡指定电子钱包中存在的身份。或者，可以使用**新建身份**选项卡为新管理员上传包含证书的文件（Base64 或 PEM 格式），或上传包含证书的 JSON 文件。

并非所有身份都能够注册新用户。有关更多信息，包括如何建立其他 CA 管理员，请参阅[创建新的 CA 管理员](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-admin)。
{: note}

## 注册身份
{: #ibp-console-identities-register}

创建身份的第一步称为**注册**。注册期间，将创建注册标识和私钥，然后节点或组织管理员可以将其用于通过生成签名证书和专用密钥来**注册**此身份。  

使用 CA 注册新身份时，需要输入以下信息。

- **注册标识**和**注册私钥**：此身份具有在部署 CA 期间指定的标识和私钥（类似于用户名和密码）。可以使用此管理员标识和私钥通过此控制台或 Fabric CA 客户机来创建此身份的证书。
- **类型**：部署 CA 时，管理员已指定 CA 签发的身份类型。身份类型的常见示例包括同级、排序节点和客户机（应用程序）。从可用类型列表中选择此用户的身份类型。
- **亲缘关系**：（可选）仅限高级用户。仅当为 CA 定义了亲缘关系时，此字段才会显示。亲缘关系是组织中要与此用户关联的一部分。例如，这可能是操作应用程序或同级的部门或单元。通过设置亲缘关系，可以将特定 CA 管理员限制为只能在组织内其自己的部门中注册用户。CA 亲缘关系可使用 Fabric CA [Affiliation 命令](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/clientcli.html#affiliation-command){: external}进行定义。
- **最大注册数**：（可选）输入可以使用此身份注册或生成证书的次数。指定有限的注册数有助于保护节点和应用程序的安全性。此值缺省为无限次数注册。
- **属性**：（可选）可以为用户指定任何[基于属性的访问控制](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#attribute-based-access-control){: external}属性。例如，可以使用此部分来[创建其他 CA 管理员](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-identity)，该管理员具有注册和登记新身份的权限。您可以在 Fabric CA User’s Guide 的 [Registering a new identity](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external} 部分中查看可用 Fabric CA 属性的完整列表。

单击 CA 概述面板上的**注册用户**按钮以创建新用户。尝试此任务前，请确保已[设置身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-identity)，即能够注册新用户的身份。通常这是 `admin` 用户。如果按钮为灰色，表示您尚未设置身份，或者该身份无法创建新身份。  

单击**注册用户**将打开一系列侧面板：
1. 在第一个侧面板上，输入新身份的**注册标识**和**注册私钥**。**保存这些值**，因为控制台不会存储这些值。
2. 选择身份**类型**。下拉列表包含此 CA 支持的类型列表。
3. 如果已为此 CA 定义了亲缘关系，那么可以选择将亲缘关系与用户相关联。如果未定义，那么不会显示亲缘关系下拉列表。如果希望用户拥有根亲缘关系，并且能够查看使用此 CA 注册的其他所有用户，请选中该用户的**使用根亲缘关系**复选框。取消选中**使用根亲缘关系**时，可以从列表中选择特定亲缘关系来与该用户相关联。
4. 输入允许此身份使用的**最大注册数**。如果未指定，那么值会缺省为无限次数注册。
5. 在最后一个侧面板上，添加要创建的身份的**属性**。

单击**注册**后，新身份将添加到 CA 创建的**已认证的用户**列表。身份按其**注册标识**列出，同时会显示**类型**和**亲缘关系**。单击表中的身份将打开一个侧面板，在其中可以查看在注册期间设置的**最大注册数**和创建的**属性**。

### 创建新的 CA 管理员
{: #ibp-console-identities-ca-admin}

缺省情况下，只有部署期间创建的 CA 管理员才能注册新身份。还可以使用注册过程中的**属性**面板来创建能够注册新用户的其他身份。

在侧面板 4 上，单击**添加属性**按钮。提供**属性名称** `hf.Registrar.Roles`。输入**属性值** `*`。还可以使用此面板来创建一个身份，仅用于注册某些类型的身份，例如客户机或同级，或者用于某个亲缘关系中。还可以创建具备以下能力的一种身份：撤销某个身份以及该身份已签发的所有证书。您可以在 Fabric CA User’s Guide 的 [Registering a new identity](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external} 部分中查看属性的完整列表。

## 注册身份
{: #ibp-console-identities-enroll}

您可以为已使用 CA 注册的每个身份生成签名证书和专用密钥。如果已使用 CA 注册了其他管理员身份，那么可以为管理员身份生成密钥，然后在[创建组织 MSP](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-create-msp) 时也包含相应密钥。

注册身份之前，需要[设置身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-ca-identity)，使其能够操作 CA。通常，应将其设置为创建 CA 时指定的管理员身份。可以通过检查 CA 详细信息页面，并查看 CA 名称旁边当前处于活动状态的身份的注册标识，以确认 CA 是否已设置为该身份。确认身份设置为管理员身份后，单击用户溢出菜单上的**注册身份**，可为使用该 CA 注册的任何用户生成证书和密钥。

- 输入用户的`注册私钥`。
- 在下一步中，将显示生成的密钥。
  - 签名证书显示在**证书**字段中。此证书也称为注册证书、签名证书或 signCert。需要将 signCert 导出为本地系统上的文件，这样才能在通过 VS Code 扩展创建客户机应用程序时使用此证书。
  - 可以在**专用密钥**字段中找到相应的专用密钥。同样，需要将专用密钥导出到本地系统，以用于通过 VS Code 扩展创建的客户机应用程序。
  - 通过单击**注册身份**创建的证书和专用密钥仅生成一次，并且不会由控制台或浏览器进行存储。此外，单击**注册身份**按钮将计入为 CA 管理员设置的最大注册数。在此注册过程中，应该通过将身份下载到本地文件系统或将其添加到控制台电子钱包，以存储签名证书和专用密钥。在**名称**字段中输入此签名证书和专用密钥的新名称，以便对其进行检索。
- **重要信息：**单击**导出身份**可将证书和密钥作为 JSON 格式的单个文件下载到本地文件系统。您负责保护和管理这些密钥。
- 单击**向电子钱包添加身份**以将这些证书添加到控制台电子钱包。然后，可以在[“电子钱包”选项卡](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet)的新磁贴中找到此身份的名称和密钥。

您还可以使用 Fabric CA 客户机或 Fabric SDK 来注册已在控制台中创建的身份。控制台提供了完成这些步骤所需的全部信息。确保已保存在注册期间指定的**注册标识**和**注册私钥**。

## 使用 TLS CA
{: #ibp-console-identities-tlsca}

网络中的通信通过 TLS 证书进行保护。TLS 会加密节点之间以及节点和应用程序之间的通信。使用 TLS 可以防止攻击者中断节点之间的通信或读取从应用程序提交的事务。用于 TLS 的密钥和证书不同于用于对事务进行签名和验证的证书，前者是由单独的认证中心签发的。

{{site.data.keyword.blockchainfull_notm}} Platform 控制台创建的每个 CA 都包含根 CA 和 TLS CA。可以在控制台的“节点”选项卡中查看相同显示名称下的这两个 CA。单击 CA 概述面板，然后单击 **TLS 认证中心**以操作 TLS CA。TLS CA 具有与根 CA [相同的注册](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-register)和[登记过程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)。这两个 CA 都是使用相同的 CA 管理员标识和私钥部署的。

部署的每个同级或排序节点都需要生成公共 TLS 证书。使用控制台创建节点时，可以将用来生成同级或排序节点身份的相同注册标识和私钥用于 TLS 注册标识和私钥，因为 TLS CA 使用与组织 CA 相同的用户存储库。然后，节点在部署期间使用此身份来生成其 TLS 证书。任何需要与排序节点或同级进行通信的应用程序都需要此证书。可以通过导航至节点概述面板并单击“设置”，找到节点的 TLS 证书。还可以在连接概要文件中找到同级和排序节点的 TLS 证书，连接概要文件可在[“智能合同”选项卡](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-connect-to-SDK)中进行下载。

使用控制台创建同级或排序节点时，还可以使用 TLS CA 为每个节点指定其他域名。在部署排序节点或同级时，在 **TLS CSR 主机名**字段中输入新域名。此主机名将添加到签发给节点的 TLS 证书中的公共名称列表。

## 证书到期时间
{: #ibp-console-identities-expiration}

使用 {{site.data.keyword.blockchainfull_notm}} Platform CA 生成的证书将在一年后到期。对于使用 Fabric SDK、Fabric CA 客户机或使用控制台生成的证书，到期时间段是相同的。如果证书到期，应用程序将无法再与网络进行交互，您需要重新注册以生成新证书。如果您已达到用户的注册限制，那么可以注册新用户，然后进行注册。

可以使用命令行来检查证书的到期日期。首先，需要通过在本地计算机上运行以下命令，将 Base64 格式的证书转换为 PEM 格式：

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
echo <base64_string> | base64 --decode $FLAG > <key>.pem
```
{:codeblock}

运行以下命令以人类可读的格式显示 PEM 编码的证书：
```
  openssl x509 -in <certificate file> -text
  ```
{:codeblock}

证书的内容将类似于以下示例：

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
        20:3d:3e:c5:31:4f:85:7a:30:9f:b5:67:47:3d:b0:10:70:80:f6:18
Signature Algorithm: ecdsa-with-SHA256
    Issuer: C=US, ST=North Carolina, O=Hyperledger, OU=Fabric, CN=fabric-ca-server-org1CA
    Validity
        Not Before: Nov 28 19:18:00 2018 GMT
        Not After : Nov 28 19:23:00 2019 GMT
    Subject: C=US, ST=North Carolina, O=Hyperledger, OU=peer, OU=org1, CN=1peeradmin
    ...
    ...
```

您可以在 **Validity** 部分中找到到期日期，即位于 `Not After:` 之后的日期。在此示例中，证书将于 2019 年 11 月 28 日到期。

## 在控制台电子钱包中存储身份
{: #ibp-console-identities-wallet}

电子钱包可存储 {{site.data.keyword.blockchainfull_notm}} Platform 控制台用于操作网络节点的身份和密钥。您需要将同级、排序节点和组织管理员添加到此电子钱包，然后才能使用控制台来操作通道和智能合同。此外，还可以使用电子钱包来方便地存储应用程序将使用的身份。可以随时使用电子钱包导出这些身份。使用左侧导航浏览至电子钱包概述面板。可以使用概述屏幕在此电子钱包中添加、更新和导出身份。

电子钱包是控制台的一个组件，而不是单独的服务。电子钱包将密钥存储在用于访问控制台（而不是本地文件系统）的浏览器的本地存储器中。如果通过其他浏览器访问控制台，或者启动专用浏览会话，那么无法访问存储在电子钱包中的身份。**建议从控制台中导出管理员身份，并将其存储在安全的地方**。
{:important}

### 添加身份
{: #ibp-console-identities-add}

在[创建组织 MSP](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-create-msp) 时，可以将管理员身份添加到电子钱包。由控制台管理的 CA 还可以在[注册过程](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)中向电子钱包添加身份。

可以使用概述屏幕上的**添加身份**按钮将身份直接导入到电子钱包。单击此按钮将打开一个侧面板，在其中可以添加身份的签名证书和专用密钥。
- **名称：**输入身份名称，此名称仅供您引用。
- **证书：**上传包含使用 CA 生成的身份签名证书（Base64 或 PEM 格式）的文件。
- **专用密钥：**上传包含使用 CA 生成的身份专用密钥（Base64 或 PEM 格式）的文件。


还可以通过上传以下格式的 JSON 文件来添加身份。此外，可以使用**上传 JSON** 按钮一次上传多个身份。

```
{
    "name": "peerAdminCerts",
    "private_key": "LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tDQpNSUdIQWdFQU1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhCRzB3YXdJQkFRUWdIVk5Kc0tHYnl6eTVZWEQ2DQovaEhKOVZlR0QrZVhmenpxUi9PQWVZYnY5UWloUkFOQ0FBUmZ1WlB1OTRzNnJQSEVCMjJlWFhpSWozd0lKYkg4DQpRYVpaRkJlNFp5SnU5UllHbkxQWUdLRnhETkRVMUZkU1NxUGNLcnczUXpxT3hXNU1NZDVWbEtVdw0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQ0K",
    "cert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlCNURDQ0FZdWdBd0lCQWdJVUhhUFNNdlcxOEwyaGNTeGlJK1ZqT1d0WnlyZ3dDZ1lJS29aSXpqMEVBd0l3YTHJNM1o5TUZicGt3SGthYnB0MmZBekFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUFmdUhjY2R6WExqZ3BLDQplVFhnNmNNcnRzRUs4ZVlKTVFMNlZLU0xwUXIvN3dJZ1hyK2ZuVjUrb2RHWnNRUU94SjZ1aDVTV0tJVkdZQ2ZGDQp1Ykw4VmJNdnRRZz0NCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0NCg=="
}
```
{:codeblock}

如果是使用 Fabric CA 客户机或 Fabric SDK 注册的身份，那么密钥需要从 PEM 格式转换为 Base64 格式。可以通过在本地计算机上运行以下命令，将证书转换为 Base64 格式：
```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-certificate>/cert.pem | base64 $FLAG
```
{:codeblock}

### 查看和更新身份
{: #ibp-console-identities-update-identities}

在**电子钱包**选项卡中，单击磁贴以查看、更新或除去电子钱包中的身份。如果身份的证书已到期，那么可能需要更新身份，并且需要从 CA 向其签发新密钥。还可以使用此选项卡从控制台和本地系统中删除密钥。

单击身份将打开一个侧面板，其中以 Base64 格式显示该身份的证书和专用密钥。单击**导出**可将身份的证书下载到本地文件系统。单击**更新**可更改电子钱包中的身份名称或将一组新密钥粘贴到面板中。不再需要使用此身份并要删除其密钥时，请单击**除去**。

## 关联身份
{: #ibp-console-identities-associate-admin}

您需要向[组织 MSP 定义](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-create-msp)提供组织和节点管理员的签名证书。控制台创建的节点或通道使用 MSP 定义中的证书来检查谁是有效的管理员。因此，用于将管理员证书添加到 MSP 定义的相同签名证书和专用密钥需要存储在控制台电子钱包中。

使用控制台创建排序节点或同级时，将遇到**关联身份**面板。在电子钱包中选择其证书也在组织 MSP 定义中的身份。创建或编辑通道时，还需要在**关联身份**部分中选择管理员身份。通过此操作，控制台可以了解在与同级、排序节点和排序服务联盟进行通信时要使用的身份。当前与同级或排序服务关联的身份会显示在节点面板左侧，位于“名称”、“节点位置”和“Fabric 版本”下方。
