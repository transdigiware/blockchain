---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# 故障诊断
{: #ibp-v2-troubleshooting}

使用控制台管理节点、通道或智能合同时，可能会发生一般性问题。在许多情况下，只需执行几个简单的步骤即可解决这些问题。
{:shortdesc}

本主题描述了使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台时可能会发生的常见问题。

- [将鼠标悬停在节点上时，状态为`状态不可用`，这意味着什么？](#ibp-v2-troubleshooting-status-unavailable)
- [将鼠标悬停在节点上时，状态为`无法检测到状态`，这意味着什么？](#ibp-v2-troubleshooting-status-undetectable)
- [创建同级或排序服务后，为什么节点操作会失败？](#ibp-console-build-network-troubleshoot-entry1)
- [为什么在打开排序服务时会收到错误：`无法获取系统通道`？](#ibp-troubleshoot-ordering-service)
- [为什么同级无法启动？](#ibp-console-build-network-troubleshoot-entry2)
- [为什么智能合同安装、实例化或升级失败？](#ibp-console-smart-contracts-troubleshoot-entry1)
- [如何查看智能合同容器日志？](#ibp-console-smart-contracts-troubleshoot-entry2)
- [通道、智能合同和身份已从控制台中消失。怎样才能使其重新显示？](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [为什么在创建新组织 MSP 定义时，会收到错误：`无法使用提供的注册标识和私钥进行认证`？](#ibp-v2-troubleshooting-create-msp)
- [为什么在尝试将组织添加到通道时会收到错误：`更新通道时发生错误`？](#ibp-v2-troubleshooting-update-channel)
- [我的 {{site.data.keyword.cloud_notm}} Kubernetes 集群已到期。这意味着什么？](#ibp-v2-troubleshooting-cluster-expired)
- [为什么通过 VS Code 提交的事务失败？](#ibp-v2-troubleshooting-anchor-peer)
- [登录到控制台时，为什么会收到“401 未授权”错误？](#ibp-v2-troubleshooting-console-401)
- [为什么部署在 {{site.data.keyword.cloud_notm}} Private 上的节点不会处理事务，并且未通过运行状况检查？](#ibp-v2-troubleshooting-healthchecks)

## 将鼠标悬停在节点上时，状态为`状态不可用`，这意味着什么？
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

CA、同级或排序节点的磁贴中的节点状态为灰色，这意味着节点的状态不可用。理想情况下，将鼠标悬停在任何节点上时，节点状态都应为`正在运行`。
{: tsSymptoms}

如果节点是新创建的，并且部署过程尚未完成，那么可能会发生此问题。如果节点是 CA，那么可能是节点未运行。如果节点是同级或排序节点，那么针对同级或排序节点运行的运行状况检查程序无法访问节点时，会发生此情况。状态请求可能失败并返回超时错误，因为节点未在特定时间段内做出响应，节点可能已停止运行，或者网络连接已关闭。
{: tsCauses}

如果这是新节点，请再稍候几分钟，以等待部署完成。如果这不是新节点，请[检查关联的节点日志](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)，以查找相关错误来确定原因。
{: tsResolve}

## 将鼠标悬停在节点上时，状态为`无法检测到状态`，这意味着什么？
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

同级或排序节点的磁贴中的节点状态为黄色，这意味着无法检测到节点的状态。理想情况下，将鼠标悬停在任何节点上时，节点状态都应为`正在运行`。
{: tsSymptoms}

仅在同级和排序节点*已导入*到控制台，而无法对节点执行运行状况检查程序时，才会发生此情况。发生此状态的原因是导入节点时未指定 `operations_url`。运行节点运行状况检查程序需要操作 URL。节点本身可能处于`正在运行`状态，但由于未指定操作 URL，因此无法确定其状态。
{: tsCauses}

可以通过执行以下步骤来解决此问题：
 1. 单击节点磁贴以将其打开。
 2. 单击**设置**图标。
 3. 单击**关联身份**，查看并记下与此节点关联的身份。单击**取消**可关闭此面板。
 4. 单击**导出**为节点生成 `JSON` 文件。
 5. 编辑生成的 `JSON` 文件，并输入节点的 `operations_url`。`operations_url` 的值取决于其配置方式以及各种网络设置。此值需要由部署节点的网络管理员提供。
 6. 单击**删除**。此步骤将从控制台中除去导入的节点，但不会删除实际节点。
 7. 在**节点**选项卡中，单击**添加同级**或**添加排序服务**，然后单击**导入现有同级**或**导入现有排序服务**。
 8. 单击**上传 JSON**，并浏览至刚才编辑的 JSON 文件。单击**下一步**。
 9. 关联在步骤 3 中记下的相同身份。
 10. 单击**添加同级**或**添加排序服务**。

现在，运行状况检查程序可以针对节点运行并报告节点的状态。
{: tsResolve}

## 创建同级或排序服务后，为什么节点操作会失败？
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

管理现有节点时可能会遇到错误。发生这种情况时，查阅节点日志通常非常有用。  

例如，尝试操作节点时，操作可能失败。
{: tsSymptoms}

创建新的同级或排序服务后，根据集群存储器配置，节点可能需要几分钟时间才能准备好供操作。
{: tsCauses}

检查 Kubernetes 仪表板，并确保同级或节点状态为`正在运行`。然后重试操作。如果节点启动后仍遇到问题，请[查看节点日志](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)以查找相关错误。  
{: tsResolve}

## 为什么在打开排序服务时会收到错误：`无法获取系统通道`？
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

在 {{site.data.keyword.cloud_notm}} Private 控制台中创建排序服务后，其状态为`正在运行`。但是，打开排序服务时，会看到以下错误：

```
无法获取系统通道。如果关联的身份没有对排序服务节点的管理特权，那么无法查看或管理排序服务详细信息。
```

{: tsSymptoms}

此情况在 {{site.data.keyword.cloud_notm}} Private 上运行的区块链控制台中发生。在非 Chrome 浏览器上，您需要接受证书才能使控制台与节点正常通信。
{: tsCauses}

有多种方法可以解决此问题：
1. 在 Helm 发布注释中，提供了控制台浏览器 URL，还有一个注释用于转至该 URL 并接受证书。请浏览至该 URL 并接受证书。现在打开排序服务。应该不会再发生该错误。
2. 如果可以使用 Chrome 浏览器，请在 Chrome 中打开区块链控制台，然后打开排序服务。不会发生该错误。请注意，您需要从非 Chrome 浏览器上的控制台电子钱包中导出身份，然后将其导入到 Chrome 浏览器上的电子钱包，才能使一切继续正常运行。
{: tsResolve}

## 为什么同级无法启动？
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

您可能会在各种情况下遇到此错误。

同级日志包含 `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
{: tsSymptoms}

- 在以下情况下可能会发生此错误：
  - 创建同级或排序服务组织 MSP 定义时，指定的注册标识和私钥对应的是类型为 `peer`（而不是 `client`）的身份。身份的类型必须是 `client`。
  - 创建同级或排序服务组织 MSP 定义时，指定的注册标识和私钥与相应组织管理员身份的注册标识或私钥不匹配。
  - 创建同级或排序服务时，指定的是类型不为“client”的身份的注册标识和私钥。

- 打开同级或排序服务 CA 节点，并查看**注册用户**表中列出的注册身份。
- 删除同级或排序服务，然后进行重新创建，务必指定正确的注册标识和私钥。
- 请注意，在创建同级或排序服务之前，需要创建类型为“client”的组织管理员标识。创建组织 MSP 定义时，请确保指定与注册标识相同的标识。请参阅有关[注册同级身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1)的指示信息和有关[注册排序节点身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer)的指示信息。
{: tsResolve}

## 为什么智能合同安装、实例化或升级失败？
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

安装、实例化或升级智能合同时可能会遇到错误。例如，尝试在同级上安装智能合同时，安装失败并返回错误：`在同级上安装智能合同时发生错误`。
{: tsSymptoms}

如果同级上已存在此版本的智能合同，或者同级的资源不足，那么可能会收到此错误。
{: tsCauses}

- 打开 Kubernetes 仪表板，并确保同级状态为`正在运行`。  
- 打开同级节点并确保同级上尚不存在该智能合同版本，然后使用正确的版本重试。
- 如果节点启动后仍遇到问题，请[查看节点日志](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)以查找相关错误。  
{: tsResolve}

## 如何查看智能合同容器日志？
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

您可能需要查看智能合同（或链代码）容器日志来调试智能合同问题。
{: tsSymptoms}

遵循这些指示信息在以下产品上查看智能合同容器日志：
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs)。
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs)。
{: tsResolve}

## 通道、智能合同和身份已从控制台中消失。怎样才能使其重新显示？
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

控制台电子钱包身份由签名证书和专用密钥组成，通过这些对象可以管理区块链组件，但这些对象仅存储在浏览器本地存储器中。您负责保护和管理这些身份。建议您在创建身份后，将其导出到文件系统。每当创建新节点时，都要将控制台电子钱包中的一个身份与该节点相关联。通过此管理员身份，您可以管理节点。切换浏览器或更改为使用其他计算机上的浏览器时，这些身份不会再位于您的电子钱包中。因此，您无法管理组件。
{: tsSymptoms}

{{site.data.keyword.blockchainfull_notm}} Platform 的其中一个新功能就是现在您负责保护和管理证书。因此，证书仅持久存储在浏览器本地存储器中，以允许您管理组件。如果使用的是专用浏览器窗口，然后切换到其他浏览器或非专用浏览器窗口，那么已创建的身份在新浏览器会话的控制台电子钱包中不复存在。因此，需要在专用浏览器会话中将控制台电子钱包中的身份导出到文件系统。然后，根据需要，可以将这些身份导入到非专用浏览器会话。否则，没有任何办法可以恢复这些身份。
{: tsCauses}

- 无论何时创建新组织 MSP 定义时，都请为允许管理组织的身份生成密钥。因此，在该过程中，必须单击**生成**，再单击**导出**按钮将生成的身份存储在控制台电子钱包中，然后以 JSON 文件格式将其保存到文件系统。
- 要在浏览器中解决此问题，需要导入这些身份，然后将其与相应的节点相关联：
  - 在遇到问题的浏览器中，单击**电子钱包**选项卡，然后单击**添加身份**以将 JSON 文件导入到电子钱包。
  - 单击**上传 JSON**，然后使用**添加文件**按钮浏览至已导出的 JSON 文件。
  - 单击**提交**。
  - 现在，打开此身份初始关联的同级或排序服务节点，然后单击**设置**图标。
  - 单击**关联身份**按钮。
  - 从下拉列表中选择刚才导入到控制台电子钱包的身份。
  - 单击**关联**。
- 对原始浏览器电子钱包中的每个身份重复此过程。
{: tsResolve}

## 为什么在创建新组织 MSP 定义时，会收到错误：`无法使用提供的注册标识和私钥进行认证`？
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

在“组织”选项卡中尝试创建新组织 MSP 定义时，遇到错误：`无法使用提供的注册标识和私钥进行认证`。
{: tsSymptoms}

为注册私钥指定的值对于在该面板的`生成组织管理员证书`部分中选择的注册标识无效时，会发生此错误。
{: tsCauses}

验证从“注册标识”下拉列表中选择的组织管理员注册标识是否正确，然后为注册私钥输入正确的值。
{: tsResolve}

## 为什么在尝试将组织添加到通道时会收到错误：`更新通道时发生错误`？
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

尝试将其他组织添加到通道时，更新失败并返回消息：`更新通道时发生错误`。
{: tsSymptoms}

在**更新通道**面板上选择的**通道更新节点 MSP 标识**不是通道的管理员时，会发生此错误。
{: tsCauses}

在**更新通道**面板上，向下滚动到**通道更新节点 MSP 标识**，然后选择创建通道时指定的 MSP 标识，或指定作为通道管理员的 MSP 标识。
{: tsResolve}

## 我的 {{site.data.keyword.cloud_notm}} Kubernetes 集群已到期。这意味着什么？
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

我收到一封电子邮件，指示我的 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群即将到期或集群状态为`已到期`。或者，您在 30 天后将无法访问控制台。
{: tsSymptoms}

免费 Kubernetes 集群的有效期仅为 30 天。
{: tsCauses}

无法从免费集群迁移到付费集群。30 天后，您将无法访问控制台，并且所有节点和证书都会被删除。请参阅有关 [Kubernetes 集群到期](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration)的主题，以获取有关所发生情况以及可以执行的操作的信息。
{: tsResolve}

## 为什么通过 VS Code 提交的事务失败？
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

通过 VS Code 提交的事务失败，返回的错误类似于：
```
提交事务时出错：没有可用于 {"chaincodes":[{"name":"hello-world"}]} 的背书计划
```
{: tsSymptoms}

如果使用了 Fabric 服务发现功能，但未在通道上配置任何锚点同级，那么会发生此错误。
{: tsCauses}

执行“部署智能合同”教程的[专用数据](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data)主题中的步骤 3 来配置锚点同级。

## 登录到控制台时，为什么会收到“401 未授权”错误？
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

尝试登录到控制台时，无法通过浏览器访问控制台。如果查看浏览器日志，可以找到“401 未授权”错误。
{: tsSymptoms}

浏览器控制台会话在处于不活动状态 **8 小时**后会超时。如果会话变为不活动状态，那么控制台会阻止不活动用户执行任何操作。
{: tsCauses}

如果会话已变为不活动状态，可以直接尝试刷新浏览器。如果此操作不起作用，请关闭浏览器，包括**所有**选项卡和窗口。重新打开该 URL。系统会要求您登录。

作为最佳实践，您应该已经将证书和身份存储在文件系统上。如果使用的是无痕浏览窗口，那么关闭浏览器时，会从浏览器本地存储器中删除所有证书。重新登录后，需要重新导入身份和证书。
{: note}

## 为什么部署在 {{site.data.keyword.cloud_notm}} Private 上的节点不会处理事务，并且未通过运行状况检查？
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

控制台表明同级和排序节点仍在运行。但是，事务处理失败。对节点 pod 运行活性或就绪性检查时，检查表明这些 pod 运行状况不佳。
{: tsSymptoms}

如果在集群上多次部署、除去和升级节点，那么可能由于测试原因，Docker 可能会失败，这是 {{site.data.keyword.cloud_notm}} Private 的已知问题。有关更多信息，请参阅 [{{site.data.keyword.cloud_notm}} Private 文档](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}中的此问题。
{: tsCauses}

要解决此问题，请除去失败的 pod，然后重新部署节点。还可以在集群上重新启动 Docker 服务。
