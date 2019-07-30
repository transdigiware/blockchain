---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: vs code extension, Visual Studio Code extension, smart contract, development tools

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# 使用 Visual Studio Code 扩展来开发智能合同
{: #develop-vscode}


{{site.data.keyword.blockchainfull}} Platform Visual Studio (VS) Code 扩展在 Visual Studio Code 中提供用于开发、打包和测试智能合同的环境。可以使用此扩展来创建智能合同项目，并开始开发业务逻辑。然后，在将智能合同部署到 {{site.data.keyword.blockchainfull_notm}} Platform 之前，可以使用 VS Code 通过预配置的 Hyperledger Fabric 实例在本地计算机上测试智能合同。本教程描述了如何使用 VS Code 扩展。

![典型智能合同开发工作流程](images/SmartContractflow.png "典型智能合同开发工作流程")  

<!--
<img usemap="#home_map1" border="0" class="image" id="image_ztx_crb_f1b2" src="images/SmartContractflow.png" width="750" alt="Click a box to get more details on the process." style="width:750px;" />
<map name="home_map1" id="home_map1">
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-creating-a-project" alt="Create a smart contract project" title="Create a Smart contract project" shape="rect" coords="40, 73.2, 175, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-creating-a-project" alt="Develop contract code in VS Code" title="Create key pair" shape="rect" coords="199, 73.2, 334, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#packaging-a-smart-contract" alt="Package the smart contract" title="Package the smart contract" shape="rect" coords="358, 73.2, 175, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-deploy" alt="Deploy locally to test and debug" title="Deploy locally to test and debug" shape="rect" coords="358, 73.2, 493, 128.2"/>
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-exporting-deleting-smart-contract-package" alt="Export the package" title="Export the package" shape="rect" coords="517, 152.2, 493, 207.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-connecting-ibp" alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" shape="rect" coords="700, 73.2, 835, 128.2" />
-->

{{site.data.keyword.blockchainfull_notm}} Platform 扩展可与使用 Hyperledger Fabric V1.4 和更高版本的任何 {{site.data.keyword.blockchainfull_notm}} Platform 实例无缝协作。
{: note}

## 步骤 1：免费安装 {{site.data.keyword.blockchainfull_notm}} Platform VS Code 扩展
{: #develop-vscode-install}

安装 {{site.data.keyword.blockchainfull_notm}} Platform VS Code 扩展之前，必须完成先决条件。

### 先决条件
{: #develop-vscode-prerequisites}

- 目前支持的操作系统是 Windows 10、Linux 或 Mac OS。
- [VS Code V1.32 或更高版本](https://code.visualstudio.com/){: external}。
- [Node V8.x 或更高版本以及 npm V5.x 或更高版本](https://nodejs.org/en/download/){: external}。
- [Docker V17.06.2-ce 或更高版本](https://www.docker.com/get-started){: external}。
- [Docker Compose V1.14.0 或更高版本](https://docs.docker.com/compose/install/){: external}。
- [用于开发 Go 合同的 Go V1.12 或更高版本](https://golang.org/dl/){: external}。

如果使用的是 Windows，那么还必须确保满足以下条件：

- Docker for Windows 配置为使用 Linux 容器（这是缺省设置）。
- 已通过 [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools#windows-build-tools){: external} 安装 C++ Build Tools for Windows。
- 已通过 [Win32 OpenSSL](http://slproweb.com/products/Win32OpenSSL.html){: external} 安装 OpenSSL V1.0.2。
  - 安装正常版本，而不是标记为“light”的版本。
  - 在 32 位系统上将 Win32 版本安装到 C:\OpenSSL-Win32 中。
  - 在 64 位系统上将 Win64 版本安装到 C:\OpenSSL-Win64 中。

### 安装扩展
{: #develop-vscode-installing-the-extension}

1. 浏览至 [Visual Studio Code 扩展市场页面](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external}或在 Visual Studio Code 内的“扩展”面板中搜索 **{{site.data.keyword.blockchainfull_notm}} Platform**。
2. 单击**安装**。
3. 重新启动 Visual Studio Code 以完成该扩展的安装。

安装后，可以使用 VS Code 左侧的 {{site.data.keyword.blockchainfull_notm}} 图标来打开 {{site.data.keyword.blockchainfull_notm}} Platform 面板。

![{{site.data.keyword.blockchainfull_notm}} 图标](images/vscode-blockchain.png "{{site.data.keyword.blockchainfull_notm}} 图标")

此扩展还向 Visual Studio Code 命令选用板添加了新命令。可以使用命令选用板来完成本指南中详细说明的许多操作。

## 步骤 2：创建智能合同项目
{: #develop-vscode-creating-a-project}

可以使用此扩展在 Visual Studio Code 中创建新的智能合同项目。此扩展创建的是基本智能合同，用于以您选择的语言来管理示例资产。可以使用示例的结构作为开发您自己业务逻辑的起点。此扩展提供了将智能合同部署到 Hyperledger Fabric 实例所需的所有依赖项。

1. 打开 **{{site.data.keyword.blockchainfull_notm}}** 选项卡。单击智能合同包窗格中的溢出菜单，然后单击**创建智能合同项目**。
2. 选择要用于创建智能合同的语言。当前选项包括 JavaScript、TypeScript、Go 和 Java。**注：**{{site.data.keyword.blockchainfull_notm}} Platform 不支持 Java 链代码。
3. **如果选择了 JavaScript 或 TypeScript**，请选择要由示例合同管理的资产。例如，***债券***。
4. 使用项目名称创建文件夹，然后将其打开。
5. 选择打开新项目的方式。现在项目文件夹应该已打开。

项目打开后，可以在左侧窗格的资源管理器窗口中找到新的智能合同。项目的结构取决于选择的语言。但是，每个智能合同都包含相同的元素：
- 智能合同的源代码。如果是选择创建 JavaScript 或 TypeScript 合同，那么此扩展会使用 `fabric-contract-api` 和一系列用于管理示例资产的函数来构建基本智能合同。例如，如果选择了***债券***，那么将找到 `createBond`、`updateBond`、`readBond`、`bondExists` 和 `deleteBond` 函数。
- 测试文件。
- 附带的智能合同依赖项。

## 步骤 3：打包智能合同
{: #packaging-a-smart-contract}

您需要先将智能合同打包成 `.cds` 格式，然后才能将其安装在 {{site.data.keyword.blockchainfull_notm}} Platform 网络或预配置的 Hyperledger Fabric 网络上。完成以下步骤以打包智能合同：

1. 在 VS Code 中，导航至 **{{site.data.keyword.blockchainfull_notm}} Platform** 面板。确保智能合同项目在文件查看器中打开。
2. 在**智能合同包**窗格中，单击溢出菜单，然后选择**打包智能合同项目**。系统将要求您提供包的名称以及版本。
  - 如果只有一个智能合同项目，那么该项目会自动打包，并显示在**智能合同包**窗格中。
  - 如果打开了多个智能合同文件夹，那么会询问您要打包哪个文件夹。
  - 如果未打开任何智能合同文件夹，那么会收到错误消息。

### 导出、导入或删除智能合同包
{: #develop-vscode-exporting-deleting-smart-contract-package}

打包智能合同项目后，可以通过 VS Code 将其导出：

1. 在 {{site.data.keyword.blockchainfull_notm}} Platform 扩展面板中，右键单击智能合同包，然后选择**导出包**。
2. 选择用于保存智能合同包文件的目录，然后单击**导出**。

还可以将现有智能合同包导入到 {{site.data.keyword.blockchainfull_notm}} Platform 窗格：

1. 在**智能合同包**窗格中，单击溢出菜单，然后选择**导入包**。
2. 浏览至要导入的智能合同包，然后单击**导入**。

还可以单击**删除包**以从包列表中除去智能合同包。

## 步骤 4：将智能合同部署到预配置的 Hyperledger Fabric 网络
{: #develop-vscode-deploy}

可以使用 VS Code 将智能合同部署到此扩展在本地计算机上创建的预配置 Hyperledger Fabric 网络。这允许您安装、实例化和测试智能合同后，再将其部署到实时网络。

### 部署预配置的 Hyperledger Fabric 网络
{: #develop-vscode-connecting-and-disconnecting}

使用以下步骤来部署预配置的网络：

1. 确保 Docker 在计算机上运行。
2. 在 VS Code 中打开 **{{site.data.keyword.blockchainfull_notm}} Platform** 选项卡。
3. 在**本地 Fabric 操作**窗格中，单击**本地 Fabric 运行时**。如果 Docker 正在运行，那么应该会下载并启动本地 Hyperledger Fabric 实例。
4. 双击 **Fabric 网关**窗格中的 **local_fabric** 以连接到本地网络。缺省情况下，连接使用“Fabric 电子钱包”窗格中的管理员身份。可以通过右键单击**本地 Fabric 操作**窗格中的认证中心节点来创建新身份。然后，可以将此新身份添加到电子钱包并与 **local_fabric** 连接相关联。

VS Code 扩展创建的是基本 Fabric 网络，其中包括一个排序节点、一个同级和一个认证中心。同级连接到名为 `mychannel` 的通道。可以在**本地 Fabric 操作**窗格中找到属于该网络的节点、组织和通道的列表。在这些节点上方，可以找到已安装并实例化的智能合同的列表。

### 停止、重新启动和除去预配置的网络
{: #develop-vscode-stop-Fabric-runtime}

创建预配置的网络后，可以停止或重新启动该网络：

1. 在**本地 Fabric 操作**窗格中，单击溢出菜单。
2. 选择**重新启动 Fabric 运行时**或**停止 Fabric 运行时**以停止或重新启动容器。

连接详细信息会保存到当前项目目录中所包含的名为 **local_fabric** 的目录中。还可以选择**关闭 Fabric 运行时**来完全除去本地 Fabric 网络。**注：**此除去操作将导致分类帐和全局状态数据丢失。

### 将智能合同部署到预配置的网络
{: #develop-vscode-deploy-smart-contract}

可以将**智能合同包**窗格中的任何包部署到正在运行的预配置网络。

首先，需要在同级上安装智能合同：

1. 在**本地 Fabric 操作**窗格中，单击**安装智能合同**。
2. 选择要在其上安装智能合同的同级。
3. 选择要安装的智能合同包，然后单击**安装**。

接下来，可以在通道上实例化智能合同：

1. 在**本地 Fabric 操作**窗格中，单击**实例化智能合同**。
2. 选择要实例化的已安装智能合同。
3. （可选）在智能合同中输入实例化函数的名称。如果使用的是缺省智能合同模板，那么不会使用实例化函数。
4. （可选）输入实例化函数所需的任何自变量。
5. （可选）如果智能合同使用了专用数据，请浏览至集合配置文件。
6. 单击**实例化**。

如果对智能合同代码进行了任何更改并对其重新打包，那么可以升级实例化的智能合同，以将更新版本部署到网络：

1. 确保要升级的智能合同已实例化。
2. 将智能合同的新版本安装到同一网络上的同级。
3. 右键单击实例化的智能合同，然后选择**升级智能合同**。
4. （可选）在实例化新智能合同后运行事务。

### 与智能合同进行交互
{: #develop-vscode-submitting-transactions}

安装并实例化智能合同后，可以使用 **Fabric 网关**窗格向智能合同中的函数提交事务：

1. 确保已安装并实例化智能合同，并且已连接到网络。
2. 在 **Fabric 网关**窗格中，展开**实例化的智能合同**。
3. 展开要与其进行交互的智能合同。您将找到在智能合同下方列出的事务的列表。
4. 右键单击要提交的事务，然后选择**提交事务**。例如，如果已创建并打包了示例债券智能合同，请单击 **createBond**。
5. 输入事务需要的任何自变量，然后按 **Enter** 键。例如，输入 `["bond01","100"]` 以创建第一个债券。

### 将应用程序连接到预配置的网络
{: #develop-vscode-exploring-connection-details}

可以通过将客户机应用程序连接到预配置的网络，并将事务提交给智能合同来测试这些应用程序。

首先需要导出连接概要文件：

1. 启动网络，然后在**本地 Fabric 操作**窗格中展开**节点**。
2. 右键单击同级，然后选择**导出连接概要文件**。

接着，可以使用 Fabric SDK 和连接概要文件以通过用户名 `admin` 和密码 `adminpw` 来注册管理员身份。然后，可以使用此身份来调用智能合同，或注册和登记其他用户。

## 步骤 5：使用开发方式调试智能合同
{: #develop-vscode-development-mode}

可以在**开发方式**下运行预配置的网络，以在本地迭代性开发和调试智能合同，而不必在每次更改后都重新打包和升级智能合同。通过调试智能合同，可以利用断点和输出来逐步运行智能合同事务，从而确保事务按预期进行。

使用以下步骤在预配置的网络上启用开发方式：

1. 网络启动后，在**本地 Fabric 操作**窗格中，展开**节点**部分。
2. 右键单击同级，然后选择**切换开发方式**。

正常运行时，同级会创建并维护链代码容器，以用于运行实例化的智能合同。通过切换到开发方式，同级允许手动运行链代码容器，这支持您使用 VS Code **调试**视图将智能合同的更新直接部署到正在运行的网络。

1. 确保已连接到处于开发方式的 **local_fabric** 连接。
2. 打开智能合同项目。
3. 在 VS Code 中，从左侧导航栏打开**调试**视图。
4. 从左上角的下拉列表中选择**调试智能合同配置**。
5. 通过单击**播放**按钮来打包并安装智能合同。
6. 通过单击智能合同文件中的相关行号，向智能合同添加断点。
7. 在调试工具栏上，单击**区块链**按钮以实例化智能合同。
8. 在调试工具栏上，单击**区块链**按钮以提交或评估事务。

要在调试期间修改智能合同，请在对智能合同进行更改后单击**重新启动**按钮。重新启动调试意味着您无需再次实例化合同。

## 步骤 6：测试实例化的智能合同
{: #develop-vscode-testing-instantiated-smart-contract}

可以为已在连接到的网络上实例化的智能合同生成测试。这些测试可以生成为 **JavaScript** 或 **TypeScript**，并且可以运行或调试。

1. 确保已对智能合同实例化。
2. 在 **Fabric 网关**窗格中，右键单击通道列表下要为其生成测试的智能合同。
3. 选择**生成智能合同测试**。
4. 选择测试文件的语言（**JavaScript** 或 **TypeScript**）。{{site.data.keyword.blockchainfull_notm}} Platform 扩展将安装必需的 npm 模块并构建测试文件。

构建了测试文件后，可通过单击该文件中的**运行测试**按钮来运行测试。


## 步骤 7：连接到 {{site.data.keyword.blockchainfull_notm}} Platform 网络
{: #develop-vscode-connecting-ibp}

还可以使用此扩展来连接到在 {{site.data.keyword.cloud_notm}} 或 {{site.data.keyword.cloud_notm}} Private 上运行的 {{site.data.keyword.blockchainfull_notm}} Platform，并使用 {{site.data.keyword.blockchainfull_notm}} Platform 控制台 UI 来调用已安装并实例化的任何智能合同。

打开与 {{site.data.keyword.blockchainfull_notm}} Platform 实例关联的 {{site.data.keyword.blockchainfull_notm}} Platform 控制台。导航至**智能合同**选项卡。使用“智能合同”选项卡上的**实例化的智能合同**表将[连接概要文件](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile)下载到本地文件系统。接着，使用 CA 来[创建应用程序身份](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities)，然后保存注册标识和私钥。执行以下步骤以通过 VS Code 连接到 {{site.data.keyword.blockchainfull_notm}} Platform。

1. 打开 **{{site.data.keyword.blockchainfull_notm}} Platform** 选项卡。
2. 在 **Fabric 网关**窗格中，单击 **+**。
3. 输入连接的名称。
4. 输入连接概要文件的标准文件路径。现在，该连接应该会显示在 **local_fabric** 下方的连接列表中。
5. 在 **Fabric 电子钱包**窗格中，单击 **+**。
6. 从选项中选择**新建电子钱包并添加身份**。为电子钱包和身份提供名称。
7. 输入组织的 MSP 标识。
8. 选中**选择网关并提供注册标识和私钥**选项，然后选择上面创建的网关。
9. 输入使用控制台创建的应用程序身份的注册标识和私钥。这将在 **Fabric 电子钱包**窗格中创建新身份。
10. 现在可以连接到 {{site.data.keyword.blockchainfull_notm}} Platform 网络的实例。双击连接名称，然后选择刚才创建的电子钱包的名称。还可以通过右键单击网关并选择**关联电子钱包**，将创建的电子钱包与网关相关联。这允许连接在每次进行连接时使用相同的电子钱包。

通过 VS Code 连接到 {{site.data.keyword.blockchainfull_notm}} Platform 后，可以在网关下看到组织同级已加入的通道的列表。在每个通道下，可以看到在每个通道上实例化的智能合同的列表以及每个智能合同中的函数。可以通过右键单击某个函数，选择**提交事务**并传递必需的自变量来向网络提交事务。此外，还可为在通道上实例化的智能合同生成测试文件。

### 添加电子钱包和用户
{: #develop-vscode-add-a-wallet}

执行以下步骤以使用证书和专用密钥来创建新的电子钱包：

1. 在 **Fabric 电子钱包**窗格中，单击 **+**。
2. 从选项中选择**新建电子钱包并添加身份**。为电子钱包和身份提供名称。
3. 输入组织的 MSP 标识。
4. 选择添加证书和专用密钥。
5. 如果要使用证书和专用密钥，请浏览至该证书和专用密钥。

还可以将新用户添加到已创建的电子钱包：

1. 在 **Fabric 电子钱包**窗格中，右键单击电子钱包，然后选择**添加身份**。
2. 提供身份名称和 MSP 标识。
3. 选择使用证书和专用密钥，或使用注册标识和私钥。
4. 如果要使用证书和专用密钥，请浏览至该证书和专用密钥。
5. 如果要使用注册标识和私钥，请选择要向其注册的网关，然后输入注册标识和私钥。

### 编辑、删除和断开连接
{: #develop-vscode-editing-connection}

在扩展中，右键单击左下方的连接以打开上下文菜单，其中包含用于添加身份、编辑连接或删除连接的选项。

要编辑连接，请完成以下步骤：
1. 选择**编辑连接**选项。这将打开**用户设置**页面，其中突出显示了连接详细信息。
2. 进行所需的任何更改，然后保存设置页面。

准备好断开与网络的连接时，单击 **Fabric 网关**窗格右上角的**断开连接**图标。

要删除连接，请右键单击连接，然后选择**删除网关**。
