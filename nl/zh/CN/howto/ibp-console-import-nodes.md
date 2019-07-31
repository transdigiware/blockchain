---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# 导入节点
{: #ibp-console-import-nodes}

控制台包含一个选项，用于导入使用其他 {{site.data.keyword.blockchainfull}} Platform 控制台创建的节点。
{:shortdesc}  

**目标受众：**本主题适用于负责创建、监视和管理区块链网络的网络操作员。

## 为什么要导入节点？
{: #ibp-console-import-nodes-why}

需要操作其他 {{site.data.keyword.blockchainfull_notm}} Platform 控制台中的 CA、排序服务和同级以及您的控制台中已存在的节点时，请从其他控制台导入这些对象。例如，要将控制台外部组织的同级加入控制台中的通道，可以使用“导入同级”选项。由于 {{site.data.keyword.blockchainfull_notm}} Platform 服务在多个环境中进行部署，因此一些服务实例可能仅包含 CA 和同级，而另一些服务实例托管排序服务。通过将不同的节点导入到控制台，可以在单个用户界面中查看和操作这些分布式组件，以便这些组件可以在区块链上一起进行事务处理。

在控制台中导入节点后，有一组强大的操作功能会变为可用，例如：
- 向联盟添加新组织
- 创建新通道
- 将同级加入通道
- 在同级上安装智能合同，不管同级部署在何处
- 在通道上实例化智能合同
- 升级通道上的智能合同

导入节点时，您需要提供其连接信息。导入组件时，实际上并不是导入物理组件本身，而是使用连接信息在控制台中构建该组件的表示，以便可以通过控制台对其进行操作。同样，从控制台中删除导入的节点时，不会删除该节点本身，该节点仍会在其部署位置运行。该节点只是从原先导入到的控制台中除去。  

将节点导入控制台后，还可以使用节点的**设置**选项卡来修改其连接信息。

要能够将节点导入到控制台，必须先从在其中创建这些节点的 {{site.data.keyword.blockchainfull_notm}} Platform 控制台将这些节点导出。网络操作员可以直接将控制台中的节点信息导出为 `JSON` 文件。
{: note}

## 限制
{: #ibp-console-import-limitations}

- 无法从入门套餐或企业套餐网络导入节点。
- 要导入的所有节点必须已使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台进行部署。
- 无法修补导入到控制台的节点。
- 导入到控制台的节点无法从这些节点部署到的集群中删除。只能从控制台中除去节点。
- 如果要导入在 {{site.data.keyword.cloud_notm}} Private 上部署的节点，那么必须确保该组件使用的 gRPC Web 代理端口外部公开给控制台。有关更多信息，请参阅[从 {{site.data.keyword.cloud_notm}} Private 导入节点](#ibp-console-import-icp)。
- 打开已导入节点的磁贴时，Fabric 版本不可见，并且**使用情况和信息**选项卡不可用。

## 准备工作：收集证书或凭证
{: #ibp-console-import-start-here}

要能够从其他 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例导入现有区块链组件，需要先将该组件的管理员身份导入到控制台电子钱包。通过将身份导入到控制台电子钱包，可简化节点操作。部署节点的网络操作员应将电子钱包中的节点管理员身份导出为 JSON 文件，以供您[导入](#ibp-console-import-nodes-admin-identities)。通常，节点管理员身份是在创建组织 MSP 定义时指定的同级或排序节点组织管理员。此身份应该已经位于节点所在的控制台的电子钱包中。

### 将管理员身份导入到控制台电子钱包
{: #ibp-console-import-nodes-admin-identities}

每个 {{site.data.keyword.blockchainfull_notm}} Platform 组件都部署有其自己的内部组件管理员的签名证书。管理员对组件执行操作时，此签名证书会附加到事务并根据组件配置进行验证。密钥允许管理员操作其组件，例如通过注册新用户，创建新通道或在同级上安装智能合同。如果要使用控制台来操作使用其他控制台创建的排序服务或同级，需要将关联的管理员身份上传到控制台电子钱包。然后，可以将导入的节点与控制台电子钱包中的该身份相关联。必须从创建身份的控制台导出这些身份，在导入节点时需要这些身份。

要导入新身份，请打开**电子钱包**选项卡，然后单击**添加身份**。单击**上传 JSON** 以浏览至网络操作员从创建节点的位置导出的 JSON 身份文件。

完成**添加身份**面板并单击“提交”后，可以在电子钱包概述屏幕中查看新的管理员身份。现在，可以在导入 CA、同级或排序服务组件时引用这些身份。

## 导入 CA
{: #ibp-console-import-ca}

CA 节点是区块链组件，用于向所有网络实体（同级、排序服务、客户机等）签发证书，以便这些实体可以进行通信、认证和最终进行事务处理。每个组织都有自己的 CA，用于充当其信任根。无论您是加入还是构建区块链联盟，都应该添加您的组织。您可以在[区块链组件概述](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca)中了解有关 CA 的更多信息。  

出于多种原因，您可能希望将 CA 导入到控制台。导入 CA 后，可以使用 CA 通过单击**注册用户**向同级组织注册更多同级用户。或者，可以使用 CA 为同级或排序服务创建其他管理员身份。

要将 CA 导入到 {{site.data.keyword.blockchainfull_notm}} Platform 控制台并对其进行操作，网络操作员必须已经从部署该 CA 的 {{site.data.keyword.blockchainfull_notm}} Platform 中将其导出。通过导入 CA，可以注册新用户并[注册身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)。

### 开始之前
{: #ibp-console-import-ca-before-you-begin}

- 确保已将 CA 的管理员身份 JSON 文件导入到控制台电子钱包，或者您具有初始部署 CA 时指定的 CA 和 TLS CA 的管理员注册标识和私钥。
- 确保从创建 CA 的控制台导出的 CA JSON 文件可用。

### 如何导入 CA  
{: #ibp-console-import-nodes-howto-ca}

导入 CA 在**节点**选项卡中执行。
1. 单击**添加认证中心**，接着单击**导入现有认证中心**，然后单击**下一步**。
2. 从**认证中心位置**下拉列表中，选择初始部署 CA 的位置。
3. 单击**添加文件**以上传从初始部署 CA 的控制台导出的 CA JSON 文件。
4. 在下一个面板上，可以输入部署 CA 时使用的注册标识和私钥。
5. 最后，输入部署 CA 时使用的 TLS CA 的注册标识和私钥。部署 CA 时，CA 和 TLS CA 会一起进行部署。网络操作员可以对这两个 CA 使用相同的注册标识和私钥，也可以在部署 CA 时，为 CA 和 TLS CA 指定唯一的注册标识和私钥。

将 CA 导入到控制台后，可以使用 CA 来创建新身份并生成必要的证书，以操作组件并将事务提交到网络。要了解更多信息，请参阅[管理认证中心](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca)。

## 导入排序服务
{: #ibp-console-import-orderer}

排序服务是区块链组件，用于收集来自网络成员的事务处理，对事务处理排序，然后将其捆绑成区块。排序服务是区块链联盟的公共绑定，如果要建立联盟供其他组织加入，那么需要部署排序服务。您可以在[区块链组件概述](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer)中了解有关排序服务的更多信息。

通过将排序服务导入到控制台，可以为同级创建新的通道以用于私下进行事务处理。

### 开始之前
{: #ibp-console-import-orderer-before-you-begin}

要能够导入排序服务，需要先收集以下信息：

- 确保已向控制台电子钱包[导入排序服务的管理员身份 JSON 文件](#ibp-console-import-nodes-admin-identities)。
- 确保从创建排序服务的控制台导出的排序服务 JSON 文件可用。

### 如何导入排序服务
导入排序服务在**节点**选项卡中执行。
1. 单击**添加排序服务**，接着单击**导入现有排序服务**，然后单击**下一步**。
2. 从**认证中心位置**下拉列表中，选择初始部署排序服务的位置。
3. 单击**添加文件**以上传从初始部署排序服务的控制台导出的排序服务 JSON 文件。请注意，如果这是五节点 Raft 排序服务，那么您应该有一个文件，该文件中包含所有五个排序节点的连接信息。
4. 通过单击**现有身份**，并选择已导入到控制台电子钱包的排序服务管理员身份，为排序服务设置管理员身份。

将排序服务导入到控制台后，可以添加新的组织成员，并在创建新通道时选择该排序服务。

## 导入同级
{: #ibp-console-import-peer}

同级节点是区块链组件，用于维护分类帐并运行智能合同，以对分类帐执行查询和更新操作。组织成员拥有并维护同级。加入联盟的每个组织都应该至少部署一个同级，而要实现高可用性 (HA)，应该至少部署两个同级。您可以在[区块链组件概述](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer)中了解有关同级的更多信息。

将同级导入到控制台后，可以在同级上安装智能合同，并将同级加入区块链中的其他通道。

**注：**如果需要向同级组织添加更多同级，或为同级创建其他管理员身份，那么需要导入同级的 CA，然后使用该 CA 来执行这些操作。

### 开始之前
{: #ibp-console-import-peer-before-you-begin}

要能够导入同级，需要先收集以下信息：

- 确保已向控制台电子钱包[导入同级的管理员身份 JSON 文件](#ibp-console-import-nodes-admin-identities)。
- 确保从创建同级的控制台导出的同级 JSON 文件可用。

### 如何导入同级
{: #ibp-console-import-peer-howto}

导入同级在**节点**选项卡中执行。
1. 单击**添加同级**，接着单击**导入现有同级**，然后单击**下一步**。
2. 从**认证中心位置**下拉列表中，选择初始部署同级的位置。
3. 单击**添加文件**以上传从初始部署同级的控制台导出的同级 JSON 文件。
4. 通过单击**现有身份**，并选择已导入到控制台电子钱包的同级管理员身份，为同级设置管理员身份。  

将同级导入到控制台后，可以在同级上安装智能合同，并将同级加入区块链中的通道。

## 导入组织 MSP 定义
{: #ibp-console-import-msp}

如果已使用其他 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例控制台部署了组织的 MSP，并且计划创建或更新使用该 MSP 定义的通道，那么需要导入该 MSP 定义。此外，如果计划创建属于部署在其他控制台中的组织 MSP 的同级或排序服务，那么需要导入该 MSP 和关联的 MSP 管理员身份。创建 MSP 的网络操作员需要将组织 MSP 定义和相应的管理员身份导出为 JSON 文件，然后将其与您共享。

- 如果计划创建属于该组织的同级或排序服务，那么需要将关联的 MSP 管理员身份导入到电子钱包。
- 导入组织 MSP 定义是在**组织**选项卡中执行的。
- 单击**导入 MSP 定义**以上传 JSON 文件。
- （可选）如果已将 MSP 管理员身份导入到电子钱包，请选中`我有 MSP 定义的管理员身份`复选框。如果未选中此选项，那么随后尝试创建同级或排序服务时，此组织 MSP 定义不会列在 MSP 下拉列表中。

## 从 {{site.data.keyword.cloud_notm}} Private 导入节点
{: #ibp-console-import-icp}

可以将在 {{site.data.keyword.cloud_notm}} Private 上创建的节点导入到已部署在其他 {{site.data.keyword.cloud_notm}} Private 集群上或 {{site.data.keyword.cloud_notm}} 上的控制台。但是，需要确保节点的 gRPC URL 使用的端口从集群外部公开。如果要在防火墙后面部署 {{site.data.keyword.cloud_notm}} Private，那么需要启用传递（例如，使用白名单来实现），以允许集群外部的控制台与节点进行通信。

例如，可以在下面找到从 {{site.data.keyword.cloud_notm}} Private 导出的同级的 JSON 文件。要与其他控制台中的同级进行通信，需要确保 `grpcwp_url` 端口（在此示例中为端口 32403）打开用于外部流量。

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
