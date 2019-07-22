---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, data storage CA, cluster ICP, configuration

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

# 设置 {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup}

在 {{site.data.keyword.cloud_notm}} Private 上部署 {{site.data.keyword.blockchainfull}} Platform 组件和构建区块链网络之前，需要在您自己的环境中设置 {{site.data.keyword.cloud_notm}} Private。
{:shortdesc}

## 先决条件
{: #icp-console-setup-prerequisites}

请完成以下先决条件并准备您的环境以安装 {{site.data.keyword.cloud_notm}} Private。

### Docker
{{site.data.keyword.cloud_notm}} Private 需要安装 Docker。请遵循[安装 {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} 中的相关指示信息来安装 Docker。

### {{site.data.keyword.cloud_notm}} Private 设置
安装 {{site.data.keyword.cloud_notm}} Private 之前，以下提示有助于准备节点以安装 {{site.data.keyword.cloud_notm}} Private。其他 {{site.data.keyword.cloud_notm}} Private 先决条件可以在 [{{site.data.keyword.cloud_notm}} Private 文档](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}中找到。

#### 更新 `vm.max_map_count` 设置
{{site.data.keyword.cloud_notm}} Private 使用 Elastic Search 进行日志记录和测量。为了避免内存不足的异常，Elasticsearch 需要配置 `vm.max_map_count` 系统属性。安装 {{site.data.keyword.cloud_notm}} Private 之前，请参阅 [Elastic Search 配置指示信息](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external}以在每个节点上配置此属性。可以使用以下命令来永久设置此属性：

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### 在集群中的每个节点上配置 `/etc/hosts` 文件

- {{site.data.keyword.cloud_notm}} Private 使用 [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} 来管理容器化应用程序。如果未在每个节点上的 `/etc/hosts` 文件中配置主机名，那么 Kubernetes 域名服务器 (DNS) 会发生故障。在每个节点上的 `/etc/hosts` 文件中[插入集群中每个节点的 IP 地址、主机名和短名称](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external}。

- [{{site.data.keyword.cloud_notm}} Private 不支持 IPv6](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}。为了避免 {{site.data.keyword.cloud_notm}} Private 集群中的 DNS 服务发生问题，请在每个节点上的 `/etc/hosts` 文件中，通过在以下行开头处添加 `#` 号注释掉该行，以禁用 IPv6 设置：
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## 需要的资源
{: #icp-console-setup-resources}

确保 {{site.data.keyword.cloud_notm}} Private 系统满足控制台以及创建的组件的最低硬件资源需求。根据您的基础架构、网络设计和性能需求，需要的 vCPU/CPU 数可能会有所不同。可以通过检查 {{site.data.keyword.cloud_notm}} 的[缺省资源分配表](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default)来大致估计 vCPU/CPU 需求，不过 {{site.data.keyword.cloud_notm}} Private 中的分配会略有不同。部署节点时，实际资源分配会显示在区块链控制台中。

**注：**  

- vCPU 是分配给虚拟机的虚拟核心，或者如果服务器没有针对虚拟机进行分区，那么 vCPU 是物理处理器核心。决定用于 {{site.data.keyword.cloud_notm}} Private 中您的部署的虚拟处理器核心 (VPC) 时，需要考虑 vCPU 需求。VPC 是确定 {{site.data.keyword.IBM_notm}} 产品许可成本的计量单位。有关决定 VPC 的场景的更多信息，请参阅 [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}。
- vCPU 相当于一个 CPU，后者相当于一个 VPC。

### 存储注意事项
{: #icp-console-setup-storage-considerations}

{{site.data.keyword.blockchainfull_notm}} Helm chart 使用动态供应来供应将由控制台使用的存储器以及将创建的区块链组件。在部署控制台之前，需要创建具有足够备用存储量可用于控制台和组件的 storageClass。您需要提供在配置期间创建的 storageClass 的名称。

- 如果使用缺省设置，Helm chart 将使用 Helm 发布名称创建一个新的持久卷声明，以用于存储控制台数据。
- 如果您使用 NFS V2/V3 持久卷，那么必须在 NFS 文件系统所在的主机系统上启用 **NFS 状态监视器，以监视 NFS V2/V3 文件系统锁定**模块（也称为 **rpc-statd**）。通过此模块，NFS 文件系统可检查在文件上是否有其他进程持有的互斥锁定。运行以下命令启用此模块：

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## 安装 {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup-install}

完成以下步骤在环境中安装和设置 {{site.data.keyword.cloud_notm}} Private。

1. 安装 [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} V3.2.0 集群。

2. 安装 {{site.data.keyword.cloud_notm}} Private CLI [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} 以安装 Helm chart。

3. 为 {{site.data.keyword.blockchainfull_notm}} Platform 部署创建一个新的定制名称空间。请注意，每个名称空间只能部署一个 Helm chart，因此如果希望控制台的多个实例在同一集群上运行，请确保分别使用单独的名称空间。

4. 设置目标名称空间的安全和访问策略。相关指示信息在[下一部分](#icp-console-setup-psp)中提供。

安装 {{site.data.keyword.cloud_notm}} Private 并将 pod 安全策略绑定到目标名称空间后，可以继续向 {{site.data.keyword.cloud_notm}} Private 集群[导入 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm chart](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install)。

## PodSecurityPolicy 需求
{: #icp-console-setup-psp}

{{site.data.keyword.blockchainfull_notm}} Platform Helm chart 在安装之前，需要将特定安全和访问策略绑定到目标名称空间。在配置 Helm chart 之前，请使用以下步骤来配置策略：

1. 为名称空间选择预定义的 PodoSecurityPolicy，或要求集群管理员为您创建定制 PodSecurityPolicy：
  - 可以使用预定义的 PodSecurityPolicy：[`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
  - 还可以使用以下 YAML 创建定制 PodSecurityPolicy 定义：

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
      volumes:
      - '*'
    ```
    {:codeblock}

2. 为 PodSecurityPolicy 创建 ClusterRole。
  - 如果创建了定制安全策略，那么可以使用以下 YAML 文件来创建 ClusterRole：

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
      - ""
      resources:
      - secrets
      verbs:
      - create
      - delete
      - get
      - list
      - patch
      - update
      - watch
    ```
    {:codeblock}

  - 如果使用的是预定义的 PodSecurityPolicy，那么只需使用第二个 apiGroups 部分来创建 ClusterRole：

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
      rules:
      - apiGroups:
      - ""
      resources:
      - secrets
      verbs:
      - create
      - delete
      - get
      - list
      - patch
      - update
      - watch
    ```
    {:codeblock}

3. 创建定制 ClusterRoleBinding。如果决定更改以下文件中的 ServiceAccount 名称，那么在部署 Helm chart 时，需要在配置页面的**所有参数**部分中为`服务帐户名称`字段提供该名称。

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

可以完成以下步骤以使用 YAML 文件将安全和访问策略绑定到名称空间：

1. 将 YAML 文件保存到本地系统。

2. 登录到 {{site.data.keyword.cloud_notm}} Private 集群，然后选择部署的目标名称空间。

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. 使用以下命令将策略应用于目标名称空间：

  ```
  kubectl apply -f <filename>.yaml
  ```
   {:codeblock}
