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

# {{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートのインストール
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private は、ローカル {{site.data.keyword.cloud_notm}} Private クラスターにインストールできる Helm チャートとして提供されます。 Helm チャートをインストールすると、{{site.data.keyword.cloud_notm}} Private カタログで {{site.data.keyword.blockchainfull_notm}} Platform をアプリケーションとして見つけることができます。

Helm チャートは、[パスポート・アドバンテージ・オンライン](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}から購入する必要があります。 購入時に、{{site.data.keyword.blockchainfull_notm}} Platform のテクニカル・サポートが含まれています。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private をインストールする前に、[考慮事項と制限](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations)を確認してください。 料金、サポート、およびセキュリティーとデータ・レジデンシーに関する考慮事項について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private について](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about)を参照してください。

## Helm チャートをインストールするための前提条件
{: #console-helm-install-prereqs}

Helm チャートをインストールする前に、{{site.data.keyword.cloud_notm}} Private クラスターを構成し、ポッド・セキュリティー・ポリシーにバインドされた新しいターゲット名前空間を作成しておく必要があります。 [{{site.data.keyword.cloud_notm}} Private クラスターのセットアップおよび構成](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)手順を確認してください。 デプロイできるコンソールは、名前空間ごとに 1 つのみです。 複数のブロックチェーン・ネットワークを作成する場合は (例えば、開発用、ステージング用、実動用に別々の環境を作成する場合など)、環境ごとに固有の名前空間を作成しておく必要があります。

### PodSecurityPolicy の要件
{: #console-helm-install-prereqs-pod-security-requirements}

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

## {{site.data.keyword.cloud_notm}} Private への Helm チャートのインポート
{: #console-helm-install-importing}

1. [パスポート・アドバンテージ・オンライン](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}から {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の Helm チャートをダウンロードします。

2. {{site.data.keyword.cloud_notm}} Private クラスターにまだログインしていない場合は、ログインします。

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. [Docker CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) が構成されていることを確認します。 Docker CLI を構成したら、以下のコマンドを使用してクラスター上のイメージ・レジストリーにアクセスします。
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

{{site.data.keyword.cloud_notm}} Private コンソールで**「カタログ」**ボタンをクリックし、左側のナビゲーション・パネルで**「ブロックチェーン」**をクリックします。 インポートが成功した場合は、{{site.data.keyword.cloud_notm}} Private の「カタログ」ページに **ibm-blockchain-platform-prod** タイルが表示されます。

## 次のステップ
{: #console-helm-install-next-steps}

Helm チャートをインストールしたら、{{site.data.keyword.cloud_notm}} Private のカタログの **ibm-blockchain-platform-prod** タイルを使用して、{{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイできます。 構成ページに入力する前に、デプロイメント用の新しいターゲット名前空間を作成し、{{site.data.keyword.blockchainfull_notm}} Platform コンポーネント用の十分なリソースがクラスターにあることを確認する必要があります。 詳しくは、[{{site.data.keyword.cloud_notm}} Private への {{site.data.keyword.blockchainfull_notm}} コンソールのデプロイ](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp)を参照してください。
