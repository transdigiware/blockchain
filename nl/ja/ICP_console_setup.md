---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-16"


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

# {{site.data.keyword.cloud_notm}} Private のセットアップ
{: #icp-console-setup}

{{site.data.keyword.blockchainfull}} Platform コンポーネントをデプロイし、{{site.data.keyword.cloud_notm}} Private にブロックチェーン・ネットワークを構築する前に、ご使用の環境で {{site.data.keyword.cloud_notm}} Private をセットアップする必要があります。
{:shortdesc}

## 前提条件
{: #icp-console-setup-prerequisites}

以下の前提条件を満たし、{{site.data.keyword.cloud_notm}} Private をインストールするための環境を準備します。

### Docker
{{site.data.keyword.cloud_notm}} Private では、Docker をインストールする必要があります。 [{{site.data.keyword.cloud_notm}} Private のインストール](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external}の関連手順に従って、Docker をインストールします。

### {{site.data.keyword.cloud_notm}} Private 設定
{{site.data.keyword.cloud_notm}} Private をインストールする前に、以下のヒントを参考にして {{site.data.keyword.cloud_notm}} Private インストール用にノードを準備します。 {{site.data.keyword.cloud_notm}} Private の追加の前提条件については、[{{site.data.keyword.cloud_notm}} Private の資料](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}を参照してください。

#### `vm.max_map_count` 設定の更新
{{site.data.keyword.cloud_notm}} Private は、ロギングおよび計量に Elastic Search を使用します。 メモリー不足例外を回避するには、Elastic Search で `vm.max_map_count` システム・プロパティーを構成する必要があります。 {{site.data.keyword.cloud_notm}} Private をインストールする前に、[Elastic Search configuration instructions](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} を参照して、各ノードでこのプロパティーを構成してください。 以下のコマンドを使用して、このプロパティーを完全に設定できます。

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### クラスター内の各ノードの `/etc/hosts` ファイルの構成

- {{site.data.keyword.cloud_notm}} Private は、[Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} を使用してコンテナー化アプリケーションを管理します。 各ノードの `/etc/hosts` ファイルでホスト名が構成されていない場合、Kubernetes ドメイン・ネーム・サーバー (DNS) は失敗します。 [クラスター内の各ノードの IP アドレス、ホスト名、および短縮名](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external}を各ノードの `/etc/hosts` ファイルに挿入します。

- [IPv6 は {{site.data.keyword.cloud_notm}} Private ではサポートされません](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}。 {{site.data.keyword.cloud_notm}} Private クラスター内の DNS サービスに関する問題を回避するには、以下のように行の先頭に `#` 記号を付けてこの行をコメント化し、各ノードの `/etc/hosts` ファイルの IPv6 設定を無効にします。
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## 必要なリソース
{: #icp-console-setup-resources}

コンソールと作成するコンポーネントの最小ハードウェア・リソース要件を {{site.data.keyword.cloud_notm}} Private システムが満たしていることを確認してください。 必要な vCPU/CPU の数は、インフラストラクチャー、ネットワーク設計、およびパフォーマンス要件によって異なります。 {{site.data.keyword.cloud_notm}} の [デフォルトのリソース割り振りの表](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default)を調べることにより、vCPU/CPU 要件の概算を行うことができます。ただし、割り振りは {{site.data.keyword.cloud_notm}} Private では若干異なります。 実際のリソース割り振りは、ノードをデプロイすると、ブロックチェーン・コンソールに表示されます。

**注:**  

- vCPU は仮想マシンに割り当てられる仮想コアであるか、サーバーが仮想マシン用にパーティション化されていない場合は物理プロセッサー・コアです。 {{site.data.keyword.cloud_notm}} Private でのデプロイメントの仮想プロセッサー・コア (VPC) を決定する場合は、vCPU 要件を考慮する必要があります。 VPC は {{site.data.keyword.IBM_notm}} 製品のライセンス交付コストを決定する単位です。 VPC を決定するシナリオについて詳しくは、[仮想プロセッサー・コア (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external} を参照してください。
- vCPU 1 つは CPU 1 つに相当し、CPU 1 つは VPC 1 つに相当します。

### ストレージについての考慮事項
{: #icp-console-setup-storage-considerations}

{{site.data.keyword.blockchainfull_notm}} Helm チャートでは、コンソールおよび作成するブロックチェーン・コンポーネントによって使用されるストレージをプロビジョンするために、動的プロビジョニングが使用されます。 コンソールをデプロイする前に、コンソールおよびコンポーネント用に十分な量の補助ストレージを持つ storageClass を作成する必要があります。 作成した storageClass の名前は、構成中に指定する必要があります。

- デフォルト設定を使用した場合は、Helm チャートによって Helm リリースの名前でコンソール・データ用の永続ボリューム・クレームが新規作成されます。
- NFS v2/v3 永続ボリュームを使用する場合は、NFS ファイル・システムが存在するホスト・システムで、**rpc-statd** とも呼ばれる **NFS status monitor for NFSv2/v3 Filesystem Locks** モジュールを有効にする必要があります。 このモジュールにより、NFS ファイル・システムは、他のプロセスが保持しているファイルの排他ロックを検査できます。 以下のコマンドを実行して、このモジュールを有効にします。

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## {{site.data.keyword.cloud_notm}} Private のインストール
{: #icp-console-setup-install}

以下の手順を実行して、ご使用の環境に {{site.data.keyword.cloud_notm}} Private をインストールし、セットアップします。

1. v3.2.0 の [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} クラスターをインストールします。

2. Helm チャートをインストールするには、{{site.data.keyword.cloud_notm}} Private CLI [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} をインストールします。

3. {{site.data.keyword.blockchainfull_notm}} Platform デプロイメント用の新しいカスタム名前空間を作成します。 名前空間ごとに 1 つの Helm チャートのみをデプロイできるため、コンソールの複数インスタンスを同じクラスターで実行する場合は、必ず別個の名前空間を使用してください。

4. ターゲット名前空間のセキュリティー・ポリシーおよびアクセス・ポリシーをセットアップします。 [次のセクション](#icp-console-setup-psp)に説明があります。

{{site.data.keyword.cloud_notm}} Private をインストールして、ポッド・セキュリティー・ポリシーをターゲット名前空間にバインドしたら、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm チャート](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install)を {{site.data.keyword.cloud_notm}} Private クラスターにインポートする作業に進むことができます。

## PodSecurityPolicy の要件
{: #icp-console-setup-psp}

{{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートをインストールする前に、特定のセキュリティー・ポリシーおよびアクセス・ポリシーをターゲット名前空間にバインドする必要があります。 以下の手順でポリシーを定義した YAML ファイルが用意されています。これらのファイルをローカル・システムに保存してから、{{site.data.keyword.cloud_notm}} Private CLI を使用して名前空間にバインドします。{{site.data.keyword.blockchainfull_notm}} Platform Helm チャートをデプロイする前に、以下の手順を実行してください。

1. {{site.data.keyword.blockchainfull_notm}} Platform の PodSecurityPolicy を定義する以下のファイルを、`ibm-blockchain-platform-psp.yaml` としてローカル・システムに保存します。

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

2. PodSecurityPolicy に必須の ClusterRole を定義する以下のファイルを、`ibm-blockchain-platform-clusterrole.yaml` として保存します。

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

3. ClusterRoleBinding を定義する以下のファイルを、`ibm-blockchain-platform-clusterrolebinding.yaml` として保存します。以下のファイルの ServiceAccount 名を変更する場合は、Helm チャートのデプロイ時に、構成ページの**「All Parameters (すべてのパラメーター)」**セクションの`「サービス・アカウント名 (Service account name)」`フィールドにその名前を指定する必要があります。

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

PodSecurityPolicy、ClusterRole、および ClusterRoleBinding の各 YAML ファイルをローカル・システムに保存したら、クラスター管理者は {{site.data.keyword.cloud_notm}} Private CLI を使用してポリシーを名前空間にバインドする必要があります。

1. {{site.data.keyword.cloud_notm}} Private クラスターにログインし、デプロイメントのターゲット名前空間を選択します。

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. クラスターの Docker イメージ・レジストリーにログインします。

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. 以下のコマンドを使用して、ポリシーをターゲット名前空間に適用します。

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. ポリシーを適用したら、サービス・アカウントに、コンソールをデプロイするために必要なレベルの権限を付与する必要があります。ターゲット名前空間の名前を指定して、以下のコマンドを実行します。

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
