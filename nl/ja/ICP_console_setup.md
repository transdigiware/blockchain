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

- {{site.data.keyword.cloud_notm}} Private は、[Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} を使用してコンテナー化アプリケーションを管理します。各ノードの `/etc/hosts` ファイルでホスト名が構成されていない場合、Kubernetes ドメイン・ネーム・サーバー (DNS) は失敗します。 [クラスター内の各ノードの IP アドレス、ホスト名、および短縮名](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external}を各ノードの `/etc/hosts` ファイルに挿入します。

- [IPv6 は {{site.data.keyword.cloud_notm}} Private ではサポートされません](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}。{{site.data.keyword.cloud_notm}} Private クラスター内の DNS サービスに関する問題を回避するには、以下のように行の先頭に `#` 記号を付けてこの行をコメント化し、各ノードの `/etc/hosts` ファイルの IPv6 設定を無効にします。
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## 必要なリソース
{: #icp-console-setup-resources}

{{site.data.keyword.cloud_notm}} Private システムが各 Fabric ランタイム・コンポーネントの最小ハードウェア・リソース要件を満たしていることを確認します。

| **コンポーネント** (すべてのコンテナー) | vCPU  | メモリー (GB) | ストレージ (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **コンソール**                    | 1.3            | 2.5                   | 10                     |
| **ピア**                       | 1.2            | 2.4                   | 200 (ピア用に 100 GB と CouchDB 用に 100 GB を含む)|
| **CA**                         | 0.1            | 0.2                   | 20                     |
| **順序付けプログラム**                    | 0.45           | 0.9                   | 100                    |

 **注:**
 - vCPU は仮想マシンに割り当てられる仮想コアであるか、サーバーが仮想マシン用にパーティション化されていない場合は物理プロセッサー・コアです。 {{site.data.keyword.cloud_notm}} Private でのデプロイメントの仮想プロセッサー・コア (VPC) を決定する場合は、vCPU 要件を考慮する必要があります。 VPC は {{site.data.keyword.IBM_notm}} 製品のライセンス交付コストを決定する単位です。 VPC を決定するシナリオについて詳しくは、[仮想プロセッサー・コア (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external} を参照してください。
 - vCPU 1 つは CPU 1 つに相当し、CPU 1 つは VPC 1 つに相当します。

### ストレージについての考慮事項
{: #icp-console-setup-storage-considerations}

* コンソールと作成するコンポーネント用に十分なリソースを持つ新しいストレージ・クラスを作成する必要があります。構成時にコンソール用に指定するストレージ・クラスは、コンポーネント・データの保管にも使用されます。
* デフォルト設定を使用すると、Helm チャートによって、コンソール・データ用に Helm リリースの名前で新しい永続ボリューム・クレームが作成されます。
* [動的プロビジョニング](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/){: external}は、{{site.data.keyword.cloud_notm}} Private では amd64 ノードでのみ使用できます。したがって、クラスターに s390x と amd64 ワーカー・ノードが混在している場合は、動的プロビジョニングを使用できません。
* 動的プロビジョニングを使用しない場合は、[永続ボリューム](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}を作成し、Kubernetes 永続ボリューム・クレーム (PVC) のバインド処理の向上のために使用できるラベルを指定してセットアップする必要があります。
* NFS v2/v3 永続ボリュームを使用する場合は、NFS ファイル・システムが存在するホスト・システムで、**rpc-statd** とも呼ばれる **NFS status monitor for NFSv2/v3 Filesystem Locks** モジュールを有効にする必要があります。 このモジュールにより、NFS ファイル・システムは、他のプロセスが保持しているファイルの排他ロックを検査できます。 以下のコマンドを実行して、このモジュールを有効にします。

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

3. ターゲット名前空間のポッド・セキュリティー・ポリシーをセットアップします。 [次のセクション](#icp-setup-psp)に説明があります。

{{site.data.keyword.cloud_notm}} Private をインストールして、ポッド・セキュリティー・ポリシーをターゲット名前空間にバインドしたら、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm チャート](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install)を {{site.data.keyword.cloud_notm}} Private クラスターにインポートする作業に進むことができます。

## PodSecurityPolicy の要件
{: #icp-console-setup-psp}

{{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートをインストールする前に、[PodSecurityPolicy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/){: external} をターゲット名前空間にバインドしておく必要があります。事前定義された PodSecurityPolicy を選択するか、クラスター管理者にカスタム PodSecurityPolicy の作成を依頼してください。
- 事前定義された PodSecurityPolicy 名: [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- カスタム PodSecurityPolicy 定義:
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
- カスタム PodSecurityPolicy のカスタム ClusterRole:
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

- カスタム ClusterRole のカスタム ClusterRoleBinding:
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
