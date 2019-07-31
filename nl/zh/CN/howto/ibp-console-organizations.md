---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: organizations, MSPs, create an MSP, MSP JSON file, consortium, system channel

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理组织
{: #ibp-console-organizations}

您可以使用 {{site.data.keyword.blockchainfull}} Platform 控制台来创建正式组织定义，这称为成员资格服务提供者 (MSP)。通过组织的 MSP 定义，区块链联盟的其他成员可以验证节点和应用程序的身份。MSP 定义还包含组织的管理员证书。

您还可以使用控制台来管理哪些组织作为网络的成员。排序服务的管理员可以使用“组织”选项卡将成员添加到区块链[联盟](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium)。然后，联盟的成员可以使用控制台将成员添加到新通道或现有通道。

**目标受众：**本主题适用于负责创建、监视和管理区块链网络的网络操作员。

## 了解 MSP
{: #console-organizations-about-msp}

{{site.data.keyword.blockchainfull_notm}} Platform 基于 Hyperledger Fabric，可构建许可的区块链网络。参与者需要是网络的已知对象，然后才能提交事务并与分类帐上的资产进行交互。Fabric 通过一组受邀组织（称为联盟）来识别身份。联盟中的组织能够向其成员签发有效凭证，并让其成为网络中的参与者。然后，参与者可以操作区块链节点并从客户机应用程序提交事务。

联盟中的每个组织都需要操作自己的认证中心，此认证中心称为根 CA。认证中心（或其中间 CA）会创建属于您组织的所有身份，并为每个身份签发签名证书和专用密钥。这些密钥由 CA 签名，并由组织成员用于对自己的操作进行签名和验证。通过加入联盟，其他组织可以识别 CA 签名，并验证同级和应用程序是否为有效参与者。有关 Hyperledger Fabric 中成员资格的更多信息，请参阅 Fabric 文档中的[成员资格概念](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external}主题。

要使组织能够加入联盟，需要先创建组织定义，这称为**成员资格服务提供者 (MSP)**。MSP 包含以下信息：
- 由**根认证中心**签名的证书。此证书用于验证节点、通道和应用程序的身份。
- 由 **TLS CA** 签名的证书。通过根 TLS 证书，同级可参与跨组织 Gossip，这对于利用 Hyperledger Fabric 的[**专用数据**集合](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)和[服务发现](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}功能是必需的。
- **MSP 标识**。MSP 标识是组织在联盟中的正式名称。您需要记住节点和应用程序的 MSP 标识。
- **同级管理员**和**组织管理员**身份的**管理员证书**。这些证书将传递给排序服务，用于验证允许组织中的哪些身份创建或编辑通道。使用控制台创建排序节点或同级时，MSP 内的管理员证书会部署在新节点中。然后，可以使用这些证书通过控制台或客户机应用程序来操作同级或排序节点。

## 在控制台中管理 MSP

导航至**组织**选项卡。可以使用此选项卡通过控制台中存在的认证中心来[创建 MSP 定义](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-create-msp)。还可以使用此选项卡来[导入 MSP](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-import-msp)（已由其他组织创建的 MSP）。

可以在**可用组织**下查看已创建或导入的所有 MSP。可以使用“组织”选项卡中的 MSP 定义来获取控制台中的重要功能：
- 如果要创建同级或排序节点，那么组织的 MSP 将用于向新节点提供管理员证书。
- 如果您是排序节点的管理员，那么可以使用 MSP [向联盟添加新组织](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-add-consortium)。
- 如果您是联盟的成员，那么可以将其他联盟成员的 MSP 导入到您的控制台，然后将这些成员添加到新通道或现有通道。

## 为组织创建 MSP
{: #console-organizations-create-msp}

使用**组织**选项卡为组织生成 MSP 定义。单击**创建 MSP** 后，将打开一个侧面板，在其中可以输入所有必需的信息：根 CA、MSP 标识和组织管理员。

- 使用面板的 **MSP 详细信息**部分来选择组织 MSP 标识。此标识是网络的其他成员将用于引用该组织的正式名称。**保存 MSP 标识**。

- 使用**根认证中心详细信息**部分来选择组织的根 CA。这是已用于（或将用于）创建属于您组织的所有节点和应用程序身份的 CA。如果使用中间 CA，那么这是用于创建这些中间 CA 的 CA。从使用控制台管理的 CA 的列表中选择根 CA。如果是使用控制台创建的 CA，那么选择根 CA 还将创建根 TLS 证书。

- 您还可以使用**根认证中心详细信息**部分来生成其中一个组织管理员证书。在创建组织 MSP 定义之前，需要使用根 CA 来注册组织和节点管理员。然后，需要完成以下步骤以使用这些身份来操作网络：

  1. 为每个管理员身份生成签名证书和专用密钥。
  2. 在 MSP 定义中提供每个管理员身份的签名证书。
  3. 将身份添加到控制台电子钱包。控制台创建的节点或通道使用 MSP 中的证书来检查谁是有效的管理员。因此，用于将管理员证书添加到 MSP 的相同签名证书和专用密钥对需要存储在控制台电子钱包中。

  可以使用**创建 MSP 定义**面板对其中一个管理员身份完成这些操作。选择根 CA 后，请在**生成组织管理员证书**部分中完成以下步骤：
  1. 输入使用根 CA 注册的管理员身份的注册标识和注册私钥。输入注册标识和注册私钥后，选择一个名称用于在控制台电子钱包中显示该身份。
  2. 单击**生成**。这将生成证书和专用密钥，并自动将密钥添加到控制台电子钱包。然后，可以使用在此面板上选择的名称在电子钱包中找到相应的管理员身份。这些密钥仅存储在浏览器本地存储器中，因此如果更改浏览器，这些密钥不会位于控制台电子钱包中。您需要从原始浏览器中导出身份，并将其导入到新浏览器的控制台电子钱包。
  3. 接着，单击**导出**以将密钥对下载到您的文件系统并妥善保管。

- 侧面板的**管理员（可选）**部分包含管理员的签名证书密钥。您可以在**管理员证书**字段的第一行中找到使用**生成组织管理员证书**部分生成的证书。如果要使用多个管理员身份来操作网络，可以将其他节点或组织管理员的证书粘贴到**管理员证书**字段中。

由于管理员证书是使用 MSP 定义传递到节点和通道的，因此需要确保每个节点和组织管理员证书都存储在 MSP 中。使用控制台创建排序节点、同级或通道时，需要将在控制台电子钱包中导出的其中一个身份与提供给 MSP 定义的管理员证书相**关联**。遇到**关联身份**部分或面板时，请选择在创建 MSP 定义时生成并保存到电子钱包的身份。

选择了根 CA、MSP 标识并创建了管理员证书后，单击**创建 MSP 定义**以创建 MSP 定义。现在，MSP 定义会在“组织”选项卡中列为组织。部署节点并加入区块链联盟时，将使用该 MSP 定义。

## 手动构建 MSP JSON 文件
{: #console-organizations-build-msp}

**此选项仅适用于熟悉如何在区块链身份管理中使用证书的高级用户。**

如果您希望使用来自**外部 CA**（非 {{site.data.keyword.IBM_notm}} 托管的 CA）的同级或排序服务的证书，那么需要构建 MSP 定义 JSON 文件，用于表示同级或排序服务组织 MSP 定义。

请注意，所有证书都必须以 Base64 格式编码。
{:important}

可以通过在本地计算机上运行以下命令，将证书文件 `<cert.pem>` 的内容从 `PEM` 格式转换为 Base64 字符串：

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat <cert.pem> | base64 $FLAG
```
{:codeblock}


使用以下格式创建 JSON 文件：

```
{
    "name": "<organization_name>",
    "msp_id": "<organization_id>",
    "type": "msp",
    "root_certs": [
        "<root_certs>"
    ],
     "intermediate_certs": [
         "<intermediate_certs>"
     ],
    "admins": [
        "<admins>"
    ],
    "tls_root_certs": [
        "<tls_root_certs>"
    ],
    "tls_intermediate_certs": [
        "<tls_intermediate_certs>"
    ]
}
```
{:codeblock}

- **organization_name**：指定用于在控制台中标识此 MSP 定义的任何名称。
- **organization_id**：指定用于在控制台内部表示此 MSP 的标识。
- **root_certs**：（可选）粘贴包含来自外部 CA 的一个或多个 `base64` 格式根证书的数组。必须提供 CA 根证书或中间 CA 证书，也可以同时提供这两者。
- **intermediate_certs**：（可选）粘贴包含来自外部中间 CA 的一个或多个 `base64` 格式证书的数组。必须提供 CA 根证书或中间 CA 证书，也可以同时提供这两者。
- **admins**：粘贴 `base64` 格式的组织管理员签名证书。
- **tls_root_certs**：（可选）粘贴包含来自 TLS CA 的一个或多个 `base64` 格式根证书的数组。必须提供外部 TLS CA 根证书或外部中间 TLS CA 证书，也可以同时提供这两者。
- **tls_intermediate_certs**：（可选）粘贴包含来自中间 TLS CA 的一个或多个 `base64` 格式证书的数组。必须提供外部 TLS CA 根证书或外部中间 TLS CA 证书，也可以同时提供这两者。  

MSP 定义中还提供了以下更多字段，但这些字段不是必需的：
- **organizational_unit_identifiers**：此 MSP 的有效成员应该在其 X.509 证书中包含的组织单元 (OU) 的列表。这是可选配置参数，在多个组织利用相同的信任根和中间 CA，并且这些组织保留了 OU 字段用于其成员时，会使用此参数。一个组织通常划分为多个组织单元 (OU)，每个组织单元具有一组特定的责任。例如，ORG1 组织可能同时具有 ORG1-MANUFACTURING 和 ORG1-DISTRIBUTION OU，以反映这些独立的业务线。CA 签发 X.509 证书时，证书中的 OU 字段会指定身份所属的业务线。请参阅 Fabric 文档中有关[身份分类](https://hyperledger-fabric.readthedocs.io/en/latest/msp.html#identity-classification){: external}的主题，以获取更多信息。  
- **fabric_node_OUs**：支持身份分类的特定于 Fabric 的 OU。`NodeOUs` 包含有关如何根据 OU 来区分客户机、同级和排序节点的信息。如果通过将“已启用”设置为 true 来强制执行检查，那么当身份为 `client`、`peer` 或 `orderer` 时，MSP 即认为该身份有效。一个身份应该只具有这些特殊 OU 中的一个 OU。请参阅 Fabric 服务发现文档中有关[如何在 MSP 中指定 `fabric_node_OU`](https://hyperledger-fabric.readthedocs.io/en/latest/discovery-cli.html#configuration-query){: external} 的示例的主题。
- **revocation_list**：不再有效的证书的列表。对于基于 X.509 的身份，这些标识是称为主题密钥标识 (SKI) 和授权访问标识 (AKI) 的字符串对，每次使用 X.509 证书时都会检查这些字符串对，以确保证书未撤销。请参阅 Fabric 文档中有关[证书撤销列表](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html?highlight=revocation%20list#revoking-a-certificate-or-identity){: external}的主题，以获取更多信息。

例如，JSON 文件将类似于以下内容：

```
{
    "name": "Org1 MSP",
    "msp_id": "org1msp",
    "type": "msp",
    "root_certs": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVZFhUcWNZVVhRS3U3WHVQWmcxUHBsekpFVFlNd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTS0K"
    ],
    "admins": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlDWHpDQ0FnYWdBd0lCQWdJVVp6NFdQdWwxRXRVOUNIcTl4NFg0Y2QwakNpNHdDZ1lJS29aSXpqMEVBd0l3DQphREVMTUFrR0ExVUVCaE1DVlZNeEZ6QVZCZ05WQkFnVERrNXZjblJvSUVOaGNtOXNhVzVoTVJRd0VnWURWUVFLDQpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbFLS0tLS0NCg=="
    ],
    "tls_root_certs": [
        "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVTzVhWU9WbjNwTkRMZGVLTFlIanRIUEtNTnY4d0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWamd5TURRM01EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdztLS0K"
    ]
}
```
{:codeblock}

将此定义保存为 MSP 定义 `JSON` 文件。  

您已构造了 MSP 定义，用于为同级或排序服务节点定义组织，并使用来自外部 CA 的证书。现在，可以返回到描述[如何将来自外部 CA 的证书用于同级或排序节点](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca)的指示信息。

## 导入 MSP
{: #console-organizations-import-msp}

只有排序节点管理员可以将新组织添加到联盟。如果您是排序节点管理员，那么需要收集已受邀加入联盟的所有组织的 MSP 定义，并将这些 MSP 导入到控制台。然后，可以使用排序节点将 MSP 添加到排序服务。

管理员创建 MSP 定义后，可以使用“组织”选项卡将 JSON 格式的 MSP 下载到其本地文件系统。然后，他们可以在频带外操作中向您发送 MSP JSON 文件。导航至**组织**选项卡，然后使用**导入 MSP 定义**将 MSP 文件导入到您的控制台。MSP 定义在**可用组织**部分中显示后，可以接着导航至排序节点以[向联盟添加组织](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-add-consortium)。


## 向联盟添加组织
{: #console-organizations-add-consortium}

组织的联盟由排序服务进行托管。

如果您是排序服务的管理员，那么可以使用控制台向联盟添加组织。导航至**节点**选项卡，然后单击排序节点。在排序节点面板的**联盟成员**下，单击**添加组织**。这将打开一个侧面板，在其中可以从[已导入到组织选项卡](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#console-organizations-import-msp)的可用 MSP 定义的列表中进行选择。还可以使用**上传 JSON** 选项直接导入其他组织创建的 MSP 定义文件。

## 创建和编辑通道
{: #console-organizations-create-channel}

将组织添加到联盟后，该组织可以使用排序服务来创建新通道，也可以将该组织添加到现有通道。允许您加入通道的信息（例如，将同级加入通道，实例化智能合约和提交事务）是使用 MSP 定义提供的。

将组织添加到联盟后，可以使用以下步骤来创建通道：

1. 将托管联盟的排序节点导入到控制台。您无需是排序节点的管理员；但您需要在控制台中提供排序节点名称和端点信息。
2. 在**组织**选项卡中，将要添加到新通道的组织的 MSP 导入到控制台。**请注意**，需要先将组织添加到联盟，然后才能将组织添加到通道。
3. 导航至**通道**选项卡，然后单击**创建通道**。这将打开一个侧面板，在其中可以指定通道名称、成员资格和通道策略。可以将已添加到联盟的任何组织添加到新通道。

有关这些步骤的更多信息，请参阅**构建网络**教程中的[创建通道](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel1)。

### 在通道定义中更新 MSP
{: #console-organizations-update-channel}

随着时间的推移，可能需要更新已与通道关联的 MSP 定义中的证书。发生这种情况时，请执行以下步骤以在通道中更新组织的 MSP 定义：  

1. 导航至控制台中的**通道**选项卡。
2. 单击包含要更新的组织 MSP 的通道并将其打开。
3. 单击**通道详细信息**选项卡。
4. 单击要更新的关联通道成员的磁贴。
5. 如果尚未将更新的 MSP 定义导入到控制台，可以在此处上传文件。**注：**此操作不会更新“组织”选项卡中的关联 MSP 定义。如果已在控制台的“组织”选项卡中更新了 MSP 定义，那么可以从下拉列表中选择该定义。
