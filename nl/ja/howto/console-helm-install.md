---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

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

# {{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートのインストール
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private は、ローカル {{site.data.keyword.cloud_notm}} Private クラスターにインストールできる Helm チャートとして提供されます。 Helm チャートをインストールすると、{{site.data.keyword.cloud_notm}} Private カタログで {{site.data.keyword.blockchainfull_notm}} Platform をアプリケーションとして見つけることができます。

Helm チャートは、[パスポート・アドバンテージ・オンライン](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}から購入する必要があります。購入時に、{{site.data.keyword.blockchainfull_notm}} Platform のテクニカル・サポートが含まれています。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private をインストールする前に、[考慮事項と制限](/docs/services/blockchain/console-icp-about.html#console-icp-about-considerations)を確認してください。 料金、サポート、およびセキュリティーとデータ・レジデンシーに関する考慮事項について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private について](/docs/services/blockchain/console-icp-about.html#console-icp-about)を参照してください。

## Helm チャートをインストールするための前提条件
{: #console-helm-install-prereqs}

Helm チャートをインストールする前に、{{site.data.keyword.cloud_notm}} Private クラスターを構成し、ポッド・セキュリティー・ポリシーにバインドされた新しいターゲット名前空間を作成しておく必要があります。 [{{site.data.keyword.cloud_notm}} Private クラスターのセットアップおよび構成](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup)手順を確認してください。 複数のブロックチェーン・ネットワークを作成する場合は (例えば、開発用、ステージング用、実動用に別々の環境を作成する場合など)、環境ごとに固有の名前空間を作成しておく必要があります。

### PodSecurityPolicy の要件
{: #console-helm-install-prereqs-pod-security-requirements}

{{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートをインストールする前に、PodSecurityPolicy をターゲット名前空間にバインドしておく必要があります。事前定義された PodSecurityPolicy を選択するか、クラスター管理者にカスタム PodSecurityPolicy の作成を依頼してください。
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

## {{site.data.keyword.cloud_notm}} Private への Helm チャートのインポート
{: #console-helm-install-importing}

1. [パスポート・アドバンテージ・オンライン](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}から {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の Helm チャートをダウンロードします。

2. {{site.data.keyword.cloud_notm}} Private クラスターにまだログインしていない場合は、ログインします。

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. [Docker CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) が構成されていることを確認します。Docker CLI を構成したら、以下のコマンドを使用してクラスター上のイメージ・レジストリーにアクセスします。
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. 以下のコマンドを使用して、Helm チャートをアップロードする {{site.data.keyword.cloud_notm}} Private 内のリポジトリーの名前を見つけます。
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  このコマンドが正常に完了すると、クラスター内のリポジトリーのリストが表示されます。 ターゲット・リポジトリーの名前を選択して保存します。 これは、以下のコマンドで使用する必要があります。

5. コマンド・ラインを使用して Helm チャートをインポートします。 PPA からダウンロードした Helm チャート・パッケージを保管したディレクトリーから、{{site.data.keyword.cloud_notm}} Private CLI で以下のコマンドを実行して、Helm チャートを {{site.data.keyword.cloud_notm}} Private クラスターにインポートします。

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  以下の値を置き換えます。
  - `<archive-name>` は、ダウンロードした `.tgz` ファイルの名前で置き換えます。
  - `<cluster_CA_domain>:8500` は、{{site.data.keyword.cloud_notm}} Private クラスターにログインするために使用するドメインで置き換えます。
  - `<repo-name>` は、チャートをアップロードする Helm リポジトリーで置き換えます。 「cloudctl catalog repos」を実行してリポジトリーをリストします。

  このコマンドが正常に完了すると、以下のような情報が表示されます。

  ```  
  Loading Helm chart
  Loaded Helm chart

  Synch charts on repo: <repo-name>
  OK
  ```  
  </details>

{{site.data.keyword.cloud_notm}} Private コンソールで**「カタログ」**ボタンをクリックし、左側のナビゲーション・パネルで**「ブロックチェーン」**をクリックします。 インポートが成功した場合は、{{site.data.keyword.cloud_notm}} Private の「カタログ」ページに **ibm-blockchain-platform-prod** タイルが表示されます。

## 次のステップ
{: #console-helm-install-next-steps}

Helm チャートをインストールしたら、{{site.data.keyword.cloud_notm}} Private のカタログの **ibm-blockchain-platform-prod** タイルを使用して、{{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイできます。構成ページに入力する前に、デプロイメント用の新しいターゲット名前空間を作成し、{{site.data.keyword.blockchainfull_notm}} Platform コンポーネント用の十分なリソースがクラスターにあることを確認する必要があります。詳しくは、[{{site.data.keyword.cloud_notm}} Private への {{site.data.keyword.blockchainfull_notm}} コンソールのデプロイ](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp)を参照してください。
