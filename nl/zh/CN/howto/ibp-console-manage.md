---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform console, administer a console, add users, remove users, modify a user's role, install patches, Kubernetes cluster expiration

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
{:gif: data-image-type='gif'}


# 管理控制台
{: #ibp-console-manage-console}

您可以执行各种操作来管理控制台行为。本主题描述的是除了区块链节点操作以外的其他操作，这些操作可帮助日常使用控制台。
{:shortdesc}

**目标受众：**本主题适用于负责创建、监视和管理区块链网络的网络操作员。

## 通过控制台添加和除去用户
{: #ibp-console-manage-console-add-remove}

对于访问控制台的每个用户，必须为其分配定义了 {{site.data.keyword.cloud}} Identity and Access Management (IAM) 用户角色的访问策略。策略确定用户可以在控制台中执行的操作。{{site.data.keyword.blockchainfull_notm}} Platform 控制台是使用作为控制台管理员的 {{site.data.keyword.cloud_notm}} 所有者的电子邮件地址供应的。缺省情况下，在 IAM 中将为此 {{site.data.keyword.cloud_notm}} 用户授予对 {{site.data.keyword.blockchainfull_notm}} Platform 服务的**管理者**角色。然后，控制台管理员可以使用 IAM UI 来授予其他用户对控制台的访问权。有关 IAM 的更多信息，请参阅[什么是 IAM](/docs/iam?topic=iam-iamoverview#iamoverview){: external}。  

[使用 IAM 邀请用户](/docs/iam?topic=iam-iamuserinv#iamuserinv){: external}时，需要完成以下步骤来配置这些用户对控制台的角色和访问权：
 1. 在菜单栏中，单击**管理** > **访问权 (IAM)**，然后选择**用户**。
 2. 单击**邀请用户**。
 3. 输入用户的电子邮件地址。
 4. 从**服务**下拉列表中，选择 **Blockchain Platform**。
 5. 向下滚动到**选择角色**。
 6. 在**分配服务访问角色**下，为用户选择一个角色，角色可以是**管理者**、**写入者**和**读取者**。
 7. 单击**邀请用户**。

|角色|功能|
|--------|----------|
|管理者|“管理者”拥有的许可权范围大于“写入者”角色。“管理者”除了可以执行“读取者”和“写入者”可以执行的所有操作外，还可以执行以下操作：<ul><li>使用控制台或 API 来供应新组件。</li><li>使用控制台或 API 删除已供应的组件。</li><li>使用控制台或 API 更改控制台日志记录级别。</li><li>使用 API 重新启动控制台。</li></ul> |
|写入者 |“写入者”拥有的许可权范围大于“读取者”角色，包括：<ul><li>使用控制台或 API 导入组件。</li><li>使用控制台或 API 除去导入的组件。</li><li>在 CA 上注册用户。</li><li> 使用控制台或 API 添加或除去通知。</li></ul>  |
|读取者 |“读取者”可以执行只读操作，包括：<ul><li>查看控制台 UI。</li><li>查看控制台日志。</li><li>导出组件。</li><li>发出任何 GET API。</li></ul> | |

 许可权是累积的。如果选择**管理者**角色，那么用户还能够执行所有**写入者**和**读取者**操作，无需另外选中这些角色。同样，具有`写入者`角色的用户将能够执行**读取者**角色中的所有操作。要访问控制台，您只需在**服务访问角色**下选择一个角色，而无需在**平台访问角色**下选择任何内容。如果服务实例应当显示在受邀用户的 {{site.data.keyword.cloud_notm}} 仪表板中，请选中**分配平台访问角色**下的相应角色。

![添加用户](../images/AddICPUser.gif){: gif}


将新用户添加到控制台后，这些用户可能无法查看其他用户部署的所有节点、通道或链代码。要使用这些组件，每个用户都需要将关联的身份导入到其自己的控制台电子钱包。有关更多信息，请参阅[在控制台电子钱包中存储身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet)。
{:important}

如果需要修改用户角色，请执行以下操作：
 1. 在菜单栏中，单击**管理** > **访问权 (IAM)**，然后选择**用户**。
 2. 单击要修改的用户旁边的“操作”菜单，然后选择**分配访问权**。
 3. 选择**分配对资源的访问权**磁贴。
 4. 从**服务**下拉列表中，选择 **Blockchain Platform 2.0**。
 5. 向下滚动到**选择角色**。
 6. 在**分配服务访问角色**下，为用户选择一个角色，角色可以是**管理者**、**写入者**和**读取者**。
 7. 单击**分配**。

需要除去用户对控制台的访问权时，请遵循[从 IAM 中除去用户](/docs/iam?topic=iam-remove#remove){: external}主题中的指示信息进行操作。

### 在 IAM 中将访问角色分配给单个用户或用户组
{: #ibp-console-manage-console-users-groups}

设置 {{site.data.keyword.cloud_notm}} IAM 策略时，可以将角色分配给单个用户或用户组。


<dl>
<dt>单个用户</dt>
<dd>您可能有特定用户需要的许可权多于或少于团队中其他用户的许可权。您可以分别定制许可权，以便每人都具有完成其任务所需的许可权。您可以为每个用户分配多个 {{site.data.keyword.cloud_notm}} IAM 角色。</dd>
<dt>访问组中的多个用户</dt>
<dd>您可以创建一组用户，然后为该组分配许可权。例如，您可以将所有团队负责人分组在一起，并将管理员访问权分配给该组。然后，可以将所有开发者分组在一起，并为该组仅分配写访问权。您可以为每个访问组分配多个 {{site.data.keyword.cloud_notm}} IAM 角色。为组分配许可权后，将影响添加到该组或从该组中除去的任何用户。如果将用户添加到组，那么这些用户还具有该组所授予的额外访问权。如果从组中除去用户，那么将撤销其访问权。
</dd>
</dl>

在 IAM 中添加的用户只是可以登录到控制台的用户的电子邮件地址。它们与“组织”选项卡中的**可用组织**或控制台电子钱包中存储的身份无关。
{:note}

## 更新管理员电子邮件地址

如果控制台由多人或多个组织使用，建议您创建功能性电子邮件地址来访问网络。即使原始管理员离开组织，或者其电子邮件地址被暂挂，您也可以通过使用功能性电子邮件继续访问控制台。只有一个电子邮件地址可用作管理员联系人。

要更新部署控制台时配置的控制台管理员的电子邮件地址，您必须以控制台管理员身份登录。导航至**用户**选项卡，然后单击 **{{site.data.keyword.IBM_notm}} 标识**磁贴上的**配置**。在打开的面板中，为控制台管理员指定新的电子邮件地址。

## 查看日志
{: #ibp-console-manage-logs}

使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台时，可能需要查看日志来调试问题。

### 查看控制台日志
{: #ibp-console-manage-console-logs}

如果需要调试在使用控制台或操作节点时遇到的问题，可以轻松访问控制台日志。您还可以设置日志记录级别，以增大或减小控制台收集的日志量。控制台日志与[节点日志](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs)分开收集，节点日志由 {{site.data.keyword.cloud_notm}} Kubernetes Service 进行收集。

在控制台浏览器中，导航至**设置**选项卡以更改日志记录设置。控制台日志从两个不同的源进行收集：

  * **客户机日志记录：**通过浏览器向控制台发送命令时会收集这些日志。
  * **服务器日志记录：**控制台向节点发送命令时会收集这些日志，还会从控制台部署中收集这些日志。这些日志包含 Hyperledger Fabric 日志记录输出。

使用每种日志类型下的下拉列表来设置收集的日志量。例如，**错误**和**警告**收集的日志量最少，而**调试**和**所有**收集的日志量最多。

仅当以控制台管理员身份登录时，才能查看控制台日志。要在**设置**选项卡中查看日志，请将浏览器 URL 中的 `settings` 一词替换为 `api/v1/logs`。例如，如果控制台 URL 为 `localhost:3001/settings`，那么可以通过导航至 `localhost:3001/api/v1/logs` 来查看日志。客户机和服务器日志在不同的文件中进行收集。最近的日志位于页面顶部。

### 查看节点日志
{: #ibp-console-manage-console-node-logs}

同级、排序节点和认证中心的日志由 {{site.data.keyword.IBM_notm}} Kubernetes Service 进行收集。使用以下步骤查看部署了 {{site.data.keyword.blockchainfull_notm}} Platform 网络的集群中节点的日志。

为了更轻松地找到节点日志，建议过滤在部署节点时使用的名称空间。要查找名称空间，请在控制台中打开任何 CA 节点，然后单击**设置**图标。查看**认证中心端点 URL** 的值。例如：`https://n2734d0-paorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054`。

名称空间是 URL 的第一部分，以字母 `n` 开头，后跟包含六个字母数字字符的随机字符串。所以在上面的示例中，名称空间的值为 `n2734d0`。

1. 打开 [{{site.data.keyword.cloud_notm}} 仪表板](https://cloud.ibm.com/resources){: external}，并导航至 {{site.data.keyword.IBM_notm}} Kubernetes Service 集群的概述屏幕。
2. 在集群概述屏幕上方，单击 **Kubernetes 仪表板**。
3. 在 Kubernetes 仪表板中，使用左侧导航**名称空间**下拉列表来切换到上面发现的 {{site.data.keyword.blockchainfull_notm}} Platform 服务实例的名称空间。
4. 在左侧导航中，单击 **Pod** 以查看已部署的节点 pod 的列表。
5. 单击 pod。然后单击顶部菜单上的**日志**以打开节点的日志。在日志上方，可以使用**日志源**后面的下拉菜单来查看来自 pod 中不同容器的日志。例如，同级和状态数据库（例如，CouchDB）在不同的容器中运行，并生成不同的日志。

缺省情况下，节点的日志是在集群中本地收集的。您还可以使用 {{site.data.keyword.cloud_notm}} 服务或第三方服务来收集、存储和分析来自网络的日志。有关更多信息，请参阅[对 {{site.data.keyword.IBM_notm}} Kubernetes Service 进行日志记录和监视](/docs/containers?topic=containers-health#health){: external}。建议您利用 [{{site.data.keyword.cloud_notm}} LogDNA](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube){: external} 服务，此服务支持您轻松地实时解析日志。

### 查看智能合同容器日志
{: #ibp-console-manage-console-container-logs}

如果遇到与智能合同相关的问题，可以查看智能合同（或链代码）容器日志来调试问题：

- 打开 Kubernetes 仪表板，过滤[名称空间](#ibp-console-manage-console-node-logs)，然后单击正在运行智能合同的同级 pod。
- 单击仪表板中的`日志`链接。缺省情况下，此链接指向同级容器。
- 通过从下拉列表中选择 `fluentd` 容器来切换到该容器。  

所有智能合同日志都会显示在此窗口中，并可使用面板上的“下载”图标进行下载。

## 为节点安装补丁
{: #ibp-console-manage-patch}

同级、CA 和排序节点的底层 {{site.data.keyword.IBM_notm}} Hyperledger Fabric Docker 映像可能需要在一段时间后进行更新，例如使用安全性更新进行更新，或者更新为新的 Fabric 单点版本。节点磁贴上的**补丁可用**文本指示此类补丁可用，并且可以随时在节点上进行安装。这些补丁是可选的，但建议进行安装。无法修补已导入到控制台的节点。

将补丁应用于节点时，一次只能应用一个。在应用补丁期间，节点不可用于处理请求或事务。因此，为了避免任何服务中断，应尽可能确保有相同类型的另一个节点可用于处理请求。在节点上安装补丁大约需要一分钟时间才能完成，更新完成后，节点即可处理请求。
{:note}

要将补丁应用于节点，请打开节点磁贴，然后单击**安装补丁**按钮。无法修补已导入到控制台的节点。

## Kubernetes 集群到期
{: #ibp-console-manage-console-cluster-expiration}

如果使用的是免费 {{site.data.keyword.cloud_notm}} Kubernetes Service 集群，那么该集群将在 30 天后到期。到期后，即无法再访问控制台。同时，所有关联的节点和证书都会被删除。一次只能有一个免费集群。因此，要能够创建其他区块链服务实例，需要先确保已从 {{site.data.keyword.cloud_notm}} 中删除到期的集群及其关联的服务实例。要确认这些资源是否已删除，或者要手动删除这些资源，请执行以下步骤：

1. 在 {{site.data.keyword.cloud_notm}} 仪表板中，单击汉堡菜单，然后打开**资源列表**。
2. 滚动到**服务**折叠标记，并将其展开以查看服务实例。
3. 如果列出了您的实例，请单击此服务实例的“操作”菜单，然后单击**删除**以删除此服务实例。
4. 滚动到 **Kubernetes 集群**折叠标记，并将其展开以查看免费集群。
5. 如果列出了免费集群，请单击此集群的“操作”菜单，然后单击**删除**以删除此免费集群。

完成这些操作后，可以执行[原始步骤](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)，在 {{site.data.keyword.cloud_notm}}“目录”页面中创建新的 Kubernetes 集群和区块链服务实例。
