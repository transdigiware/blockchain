---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"


keywords: IBM Cloud Private, IBM Blockchain Platform, install, Helm chart, PodSecurityPolicy

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

# 安装 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private 作为 Helm chart 交付，可安装在本地 {{site.data.keyword.cloud_notm}} Private 集群中。安装 Helm chart 之后，您可在 {{site.data.keyword.cloud_notm}} Private 目录中找到作为应用程序的 {{site.data.keyword.blockchainfull_notm}} Platform。

Helm chart 必须通过 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external} 进行购买。购买时，将包含 {{site.data.keyword.blockchainfull_notm}} Platform 技术支持。

在安装 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 之前，请查看[注意事项和限制](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations)。有关定价、支持和安全性以及数据存储位置注意事项的更多信息，请参阅[关于 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about)。

## 安装 Helm chart 的先决条件
{: #console-helm-install-prereqs}

安装 Helm chart 之前，您必须已配置 {{site.data.keyword.cloud_notm}} Private 集群，创建与 pod 安全策略绑定的新目标名称空间。查看[设置和配置 {{site.data.keyword.cloud_notm}} Private 集群](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)的指示信息。每个名称空间只能部署一个控制台。如果计划创建多个区块链网络，例如为开发、编译打包和生产创建不同的环境，那么应该为每个环境创建唯一的名称空间。

### PodSecurityPolicy 需求
{: #console-helm-install-prereqs-pod-security-requirements}

{{site.data.keyword.blockchainfull_notm}} Platform Helm chart 在安装之前，需要将特定安全和访问策略绑定到目标名称空间。我们在下面的步骤中提供了用于定义策略的 YAML 文件。您可以将这些文件保存至本地系统，然后使用 {{site.data.keyword.cloud_notm}} Private CLI 将其与您的名称空间绑定。部署 {{site.data.keyword.blockchainfull_notm}} Platform Helm chart 之前，请先执行下面的步骤。

1. 在本地系统上将定义 {{site.data.keyword.blockchainfull_notm}} Platform PodSecurityPolicy 的以下文件保存为 `ibm-blockchain-platform-psp.yaml`：

    ```
    apiVersion: extensions/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: ibm-blockchain-platform-psp
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      privileged: true
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      seLinux:
        rule: RunAsAny
      supplementalGroups:
        rule: RunAsAny
      runAsUser:
        rule: RunAsAny
      fsGroup:
        rule: RunAsAny
      requiredDropCapabilities:
      - ALL
      allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - FOWNER
      volumes:
      - '*'
    ```
    {:codeblock}

2. 将定义 PodSecurityPolicy 所需 ClusterRole 的以下文件保存为 `ibm-blockchain-platform-clusterrole.yaml`：

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
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
      verbs:
      - '*'
    - apiGroups:
      - apiextensions.k8s.io
      resources:
      - persistentvolumeclaims
      - persistentvolumes
      - customresourcedefinitions
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

3. 将定义 ClusterRoleBinding 的以下文件保存为 `ibm-blockchain-platform-clusterrolebinding.yaml`。如果决定更改以下文件中的 ServiceAccount 名称，那么在部署 Helm chart 时，需要在配置页面的**所有参数**部分中为`服务帐户名称`字段提供该名称。

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
   name: ibm-blockchain-platform-clusterrolebinding
  roleRef:
   apiGroup: rbac.authorization.k8s.io
   kind: ClusterRole
   name: ibm-blockchain-platform-clusterrole
  subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
  ```
  {:codeblock}

将 PodSecurityPolicy、ClusterRole 和 ClusterRoleBinding YAML 文件保存到本地系统之后，集群管理员将需要使用 {{site.data.keyword.cloud_notm}} Private CLI 来将策略与您的名称空间绑定。

1. 登录到 {{site.data.keyword.cloud_notm}} Private 集群，然后选择部署的目标名称空间。

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. 登录到集群的 Docker 映像注册表：

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. 使用以下命令将策略应用到目标名称空间：

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. 应用策略之后，您必须授予服务帐户所需的许可权级别以部署控制台。使用您的目标名称空间的名称运行以下命令：

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```

## 将 Helm chart 导入 {{site.data.keyword.cloud_notm}} Private
{: #console-helm-install-importing}

1. 从 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external} 下载 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 的 Helm chart。

2. 如果尚未登录到 {{site.data.keyword.cloud_notm}} Private 集群，请进行登录。

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. 确保已配置 [Docker CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html)。配置 Docker CLI 后，使用以下命令访问集群上的映像注册表：
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. 通过使用以下命令，在 {{site.data.keyword.cloud_notm}} Private 中查找要上传 Helm chart 的存储库的名称：
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  成功完成此命令后，您可以看到集群中存储库的列表。选择目标存储库的名称并进行保存。您需要在以下命令中使用该名称。

5. 使用命令行导入 Helm chart。在 {{site.data.keyword.cloud_notm}} Private CLI 中，从存储通过 PPA 下载的 Helm chart 包的目录中运行以下命令，以将 Helm chart 导入 {{site.data.keyword.cloud_notm}} Private 集群。

  ```
    cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
    ```
  {:codeblock}

  替换以下值：
  - `<archive-name>`，替换为所下载的 `.tgz` 文件的名称。
  - `<cluster_CA_domain>:8500`，替换为用于登录到 {{site.data.keyword.cloud_notm}} Private 集群的域。
  - `<repo-name>`，替换为要在其中上传 chart 的 helm 存储库。运行“cloudctl catalog repos”以列出您的存储库。

  成功完成此命令后，可以看到类似于以下信息的内容：
    

  ```  
  Uploading Helm chart(s)
   Processing chart: charts/ibm-blockchain-platform-prod-1.1.0.tgz
   Updating chart values.yaml
   Uploading chart
  Loaded Helm chart
  OK

  Synch charts
  OK

  Archive finished processing
  ```  
  </details>

单击 {{site.data.keyword.cloud_notm}} Private 控制台中的**目录**按钮，然后单击左侧导航面板中的**区块链**。如果导入成功，**ibm-blockchain-platform-prod** 磁贴会显示在 {{site.data.keyword.cloud_notm}} Private 目录页面上。

## 后续步骤
{: #console-helm-install-next-steps}

安装 Helm chart 之后，可以使用 {{site.data.keyword.cloud_notm}} Private 目录中的 **ibm-blockchain-platform-prod** 磁贴来部署 {{site.data.keyword.blockchainfull_notm}} Platform 控制台。您需要为部署创建新的目标名称空间，并在完成配置页面之前，确保集群有足够的资源用于 {{site.data.keyword.blockchainfull_notm}} Platform 组件。有关更多信息，请参阅[在 {{site.data.keyword.cloud_notm}} Private 上部署 {{site.data.keyword.blockchainfull_notm}} 控制台](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp)。
