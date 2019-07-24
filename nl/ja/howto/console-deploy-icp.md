---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-21"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# {{site.data.keyword.blockchainfull_notm}} コンソールのデプロイ
{: #console-deploy-icp}

{{site.data.keyword.blockchainfull}} Platform の Helm チャートをインストールしたら、以下の手順で、ローカルの {{site.data.keyword.cloud_notm}} Private クラスターに {{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイします。
{:shortdesc}

## 必要なリソース
{: #console-deploy-icp-resources-required}

コンソールと作成するコンポーネントの最小ハードウェア・リソース要件を {{site.data.keyword.cloud_notm}} Private システムが満たしていることを確認してください。 必要な vCPU/CPU の数は、インフラストラクチャー、ネットワーク設計、およびパフォーマンス要件によって異なります。 {{site.data.keyword.cloud_notm}} の [デフォルトのリソース割り振りの表](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default)を調べることにより、vCPU/CPU 要件の概算を行うことができます。ただし、割り振りは {{site.data.keyword.cloud_notm}} Private では若干異なります。 実際のリソース割り振りは、ノードをデプロイすると、ブロックチェーン・コンソールに表示されます。

**注:**  

- vCPU は仮想マシンに割り当てられる仮想コアであるか、サーバーが仮想マシン用にパーティション化されていない場合は物理プロセッサー・コアです。 {{site.data.keyword.cloud_notm}} Private でのデプロイメントの仮想プロセッサー・コア (VPC) を決定する場合は、vCPU 要件を考慮する必要があります。 VPC は {{site.data.keyword.IBM_notm}} 製品のライセンス交付コストを決定する単位です。 VPC を決定するシナリオについて詳しくは、[仮想プロセッサー・コア (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external} を参照してください。
- 1 つの vCPU は 1 つの CPU に相当し、1 つの VPC に相当します。

## ストレージ
{: #console-deploy-icp-storage}

{{site.data.keyword.blockchainfull_notm}} Helm チャートでは、コンソールおよび作成するブロックチェーン・コンポーネントによって使用されるストレージをプロビジョンするために、動的プロビジョニングが使用されます。 コンソールをデプロイする前に、コンソールおよびコンポーネント用に十分な量の補助ストレージを持つ storageClass を作成する必要があります。 作成した storageClass の名前は、構成中に指定する必要があります。

- デフォルト設定を使用した場合は、Helm チャートによって Helm リリースの名前でコンソール・データ用の永続ボリューム・クレームが新規作成されます。


## コンソールをデプロイするための前提条件
{: #console-deploy-icp-prerequisites}

1. {{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイするには、その前に [{{site.data.keyword.cloud_notm}} Private をインストール](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)し、[{{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートをインストール](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console_helm-install)する必要があります。

2. {{site.data.keyword.blockchainfull_notm}} Platform デプロイメント用の新しいカスタム名前空間を作成する必要があります。 名前空間には、[必要な PodSecurityPolicy](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-prereqs-pod-security-requirements) を使用する必要があります。 複数のブロックチェーン・ネットワークを作成する場合は (例えば、開発用、ステージング用、実動用に別々の環境を作成する場合など)、環境ごとに固有の名前空間を作成しておく必要があります。 名前空間ごとに 1 つの Helm チャートのみをデプロイできるため、コンソールの複数インスタンスを同じクラスターで実行する場合は、必ず別個の名前空間を使用してください。

3. {{site.data.keyword.cloud_notm}} Private コンソールから、CA のクラスター・プロキシー IP アドレスの値を取得します。 **注:** プロキシー IP にアクセスするには、[クラスター管理者](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external}である必要があります。 {{site.data.keyword.cloud_notm}} Private クラスターにログインします。 左側のナビゲーション・パネルで**「プラットフォーム」**、**「ノード」**の順にクリックし、クラスターで定義されているノードを表示します。 `proxy` 役割を持つノードをクリックし、テーブルから`「ホスト IP」`の値をコピーします。 **重要:** この値を保存して、Helm チャートの`「プロキシー IP (Proxy IP)」`フィールドを構成する際に使用します。

4. デプロイメントでクラスターの Docker レジストリーから必要なイメージをダウンロードできるようにする[イメージ・セキュリティー・ポリシー](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-image-policy)を作成します。

5. 初めてコンソールにログインする際に使用するパスワードを作成し、{{site.data.keyword.cloud_notm}} Private 内のシークレット・オブジェクトの中に保管します。 機密事項の作成手順は、[次のセクション](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)を参照してください。


## クラスター・イメージ・ポリシーの要件
{: #console-deploy-icp-image-policy}

ICP 3.2 以降では、[コンテナー・イメージ・セキュリティー](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_images/image_security.html)はデフォルトで有効になっています。 そのため、Helm チャートのインストール時に指定した Docker Hub コンテナー・レジストリーを、トラステッド・レジストリーのリストに追加する必要があります。 そうしないと、コンソール・デプロイメントがそのレジストリー内のイメージにアクセスできなくなります。 以下の手順に従って、新しいイメージ・ポリシーを作成します。

1. {{site.data.keyword.cloud_notm}} Private コンソールにログインします。 左側のナビゲーション・パネルで、**「管理」**、**「リソース・セキュリティー」**の順にクリックします。 **「リソース・セキュリティー」**メニューで、**「イメージ・ポリシー」**にナビゲートし、**「イメージ・ポリシーの作成」**をクリックします。

2. **「ポリシーの詳細」**セクションで、以下のフィールドに次のように入力します。
  - 新しいイメージ・ポリシーの**「名前」**を入力します。 例えば、`ibp-imagepolicy` などです。
  - **「有効範囲」**で、`namespace` を選択します。
  - **「名前空間」**で、Helm チャートをインストールした名前空間を選択します。   

3. **「ポリシーの詳細」**セクションで**「レジストリーの追加」**をクリックし、[Helm チャートをインストールした](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-importing)ときに指定したイメージ・レジストリーの後に `/*` を付けて指定します。
  - 例えば、`<cluster_CA_domain>:8500/*`、または `<cluster_CA_domain>:8500/ibp/*` と `<cluster_CA_domain>:8500/op-tools/*` のレジストリーを指定できます。

YAML ファイルおよび kubectl コマンド・ライン・ツールを使用して、新しいクラスター・イメージ・ポリシーを追加することもできます。 YAML ファイルの例を以下に示します。

```
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ClusterImagePolicy
metadata:
  name: ibp-imagepolicy
spec:
  repositories:
  - name: <cluster_CA_domain>:8500/ibp/*
  - name: <cluster_CA_domain>:8500/op-tools/*
```

## コンソール・パスワードのシークレットの作成
{: #console-deploy-icp-password-secret}

コンソールにアクセスするためには、初めてコンソールにログインする際に使用するデフォルト・パスワードを作成しておく必要があります。 コンソールをデプロイする前に、パスワードを保管するための [Kubernetes シークレット](https://kubernetes.io/docs/concepts/configuration/secret/){: external} を作成します。 Kubernetes 秘密により、情報をデプロイメントに直接渡すことなく、情報を保護および共有できます。

1. パスワードを作成し、base64 形式でエンコードします。 次のコマンドを端末ウィンドウで実行します。`password` の値は、使用する値に置き換えてください。 **このコマンドの出力を保存します**。
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. {{site.data.keyword.cloud_notm}} Private コンソールにログインします。 左側のナビゲーション・パネルで、**「構成」**、**「秘密」**の順にクリックします。 **「秘密の作成」**ボタンをクリックし、新規秘密オブジェクトを生成できるポップ・パネルを開きます。

3. **「一般」**タブで、以下のフィールドに次のように入力します。
  - **名前:** 秘密にクラスター内で固有の名前を付けます。 この名前は、コンソールをデプロイするときに使用します。 名前はすべて小文字でなければなりません。
  - **名前空間**: 秘密を追加する名前空間。 コンソールのデプロイ先となる `namespace` を選択してください。
  - **タイプ:** 値 `generic` を入力します。

4. **注釈**タブはブランクのままにします。

5. **データ**タブに、キーと値のペアとしてユーザー名とパスワードを追加します。
  1. 最初の**「名前」**フィールドに、`password` を入力します。
  2. 最初の**「値」**フィールドに、ステップ 1 の `echo -n 'password' | base64` の結果を入力します。
  3. **「作成」**をクリックして、新しい秘密オブジェクトを作成します。

コンソールをデプロイするときに、構成ページの`「コンソール管理者のパスワード・シークレットの名前 (Console administrator password secret name)」 `フィールドに、このシークレットの名前を入力します。 初めてコンソールにログインする際は、ステップ 1 でエンコードした値を使用する必要があります。 コンソール管理者が変更しない限り、この値がコンソールのデフォルト・パスワードになります。 詳しくは、[コンソールからのユーザーの管理](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)を参照してください。

## TLS シークレットの作成 (オプション)
{: #console-deploy-icp-tls-secret}

コンソールでは、コンソールとブロックチェーン・ノードの間の通信を保護するために TLS が使用されます。 デフォルトでは、コンソール用の TLS 証明書はコンソールが生成します。 ただし、ユーザー独自の TLS 証明書と鍵を指定することもできます。 そのような証明書は以下の手順で [Kubernetes シークレット](https://kubernetes.io/docs/concepts/configuration/secret/)に保管します。 そして、そのシークレットの名前を構成時に「TLS シークレット (TLS secret)」フィールドに入力することで、コンソールにそれらの証明書を設定できます。

1. TLS 証明書と秘密鍵を base64 形式に変換します。 ローカル・マシンで次のコマンドを実行すると、証明書ファイルや鍵ファイルの内容を base64 ストリングに変換できます。

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  TLS 証明書および対応する TLS 鍵の両方に対してこのコマンドを実行します。 出力を**保存**します。

2. {{site.data.keyword.cloud_notm}} Private コンソールにログインします。 左側のナビゲーション・パネルで、**「構成」**、**「秘密」**の順にクリックします。 **「秘密の作成」**ボタンをクリックし、新規秘密オブジェクトを生成できるポップ・パネルを開きます。

3. **「一般」**タブで、以下のフィールドに次のように入力します。
  - **名前:** 秘密にクラスター内で固有の名前を付けます。 この名前は CA の構成で使用します。 名前はすべて小文字でなければなりません。
  - **名前空間**: 秘密を追加する名前空間。 CA をデプロイする`名前空間`を選択します。
  - **タイプ:** 値 `kubernetes.io/tls` を入力します。

4. **注釈**タブはブランクのままにします。

5. **データ**タブに、キーと値のペアとしてユーザー名とパスワードを追加します。
  1. 最初の**「名前」**フィールドに、`tls.crt` と入力します。
  2. 最初の**「値」**フィールドに、ステップ 1 で base64 エンコードした TLS 証明書のストリングを入力します。
  3. **「データの追加」**ボタンをクリックして、2 番目のキーと値のペアを追加します。
  4. 最初の**「名前」**フィールドに、`tls.key` と入力します。
  5. 最初の**「値」**フィールドに、ステップ 1 で base64 エンコードした TLS 鍵のストリングを入力します。
  6. **「作成」**をクリックして、新しい秘密オブジェクトを作成します。

コンソールをデプロイするときに、構成ページの「すべてのパラメーター (all parameters)」セクションの`「TLS シークレット (TLS secret)」`フィールドに、この作成したシークレットの名前を入力します。

Helm リリースを削除しても、{{site.data.keyword.cloud_notm}} Private クラスターのシークレットは削除されません。 {{site.data.keyword.cloud_notm}} Private クラスター内のパスワード・シークレットと TLS シークレットはユーザーが管理する必要があります。 後で別のコンソールをデプロイする場合は、シークレットを再利用できます。 そうでない場合は、ユーザーが {{site.data.keyword.cloud_notm}} Private クラスターからシークレットを削除する必要があります。
{:note}

## 構成
{: #console-deploy-icp-configuration}

TLS シークレット・オブジェクトを作成したら、以下の手順でコンソールをデプロイできます。

1. {{site.data.keyword.cloud_notm}} Private コンソールにログインし、右上隅の**「カタログ」**をクリックします。
2. 左側のナビゲーション・パネルの`「ブロックチェーン」`をクリックし、`「ibm-blockchain-platform-prod」`というラベルのタイルを見つけます。 タイルをクリックして開きます。 Helm チャートのインストールと構成に関する情報が含まれた Readme ファイルが表示されます。
3. パネル上部にある**「構成」**タブをクリックするか、右下隅にある**「構成」**ボタンをクリックします。
4. [構成とポッド・セキュリティーのパラメーター](#icp-peer-deploy-configuration-parms)の値を指定し、使用条件に同意します。
5. **「パラメーター」**セクションにナビゲートします。
- [クイック・スタート・パラメーター](#icp-peer-deploy-quickstart-parms)のみを使用してコンソールをデプロイすることができます。 機能や概要を知ることが目的であれば、このオプションを使用してください。
- [「すべてのパラメーター (All parameters)」](#icp-peer-deploy-quickstart-parms)セクションを使用して、コンソール用のネットワーク・アクセス、リソース、ストレージをカスタマイズできます。 **「すべてのパラメーター (All parameters)」**セクションは、経験豊富な Kubernetes ユーザーにのみお勧めします。
6. **「インストール」**をクリックします。

次の表に、構成パラメーターの各フィールドとデフォルト値を示します。

### 構成とポッド・セキュリティーのパラメーター
{: #icp-peer-deploy-configuration-parms}

|  パラメーター     | 説明    | デフォルト  | 必須 |
| --------------|-----------------|-------|------- |
| `Helm リリース名 (Helm release name)`| Helm リリース・デプロイメントを識別するための名前。 小文字から始めて、任意の英数字で終わります。ハイフンと小文字の英数字のみで構成されなければなりません。 | なし | はい |
| `ターゲット名前空間 (Target namespace)`| コンソールとコンポーネント用に作成した名前空間を指定します。 名前空間には `ibm-privileged-psp` ポリシーが含まれていなければなりません。 そうでない場合は、その名前空間に [PodSecurityPolicy をバインド](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp)してください。 | なし | はい |
| `ターゲット・クラスター (Target Cluster)`| リソースをデプロイするクラスターを指定します。 選択した名前空間と Kubernetes のバージョンの要件によって、使用可能なクラスターがフィルタリングされます。 | なし | はい |
| `ターゲット名前空間のポリシー (Target namespace policies)`| ターゲット名前空間を使用するように事前構成されます。 `ibm-privileged-psp` ポリシーまたは `ibm-blockchain-platform-psp` ポリシーが値に含まれていなければなりません。 | なし | はい |

### クイック・スタート・パラメーターとすべてのパラメーター
{: #icp-peer-deploy-quickstart-parms}

機能や概要を知ることが目的であれば、「クイック・スタート」セクションを使用してください。 「すべてのパラメーター (All parameters)」セクションでは、ネットワーク・アクセス、リソース、ストレージをカスタマイズできます。このセクションは、経験豊富な Kubernetes ユーザーにのみお勧めします。

**「すべてのパラメーター (All Parameters)」タブをクリックすると、構成パラメーターがすべて表示されます。**

|  パラメーター     | 説明    | デフォルト  | 必須 |
| --------------|-----------------|-------|------- |
| **コンソール設定 (Console settings)** | **コンソールの管理とデプロイメント** | | |
| `プロキシー IP (Proxy IP)` | クラスターのプロキシー・ノードの IP アドレス。 | なし| はい |
| `コンソール管理者の E メール (Console administrator email)` | コンソールへのログインに使用する E メール。 | なし | はい |
| `コンソール管理者のパスワード・シークレットの名前 (Console administrator password secret name)` | コンソールへのログインに使用する[パスワードを保管するために作成した](#console-deploy-icp-password-secret)シークレットの名前。 | なし | はい|
| **ネットワーク設定 (Network settings)** | **コンソールへのネットワーク・アクセス** | | |
| `コンソールのホスト名 (Console hostname)` | プロキシー IP と同じ値を入力します。 | なし | はい |
| `コンソールのポート (Console port)` | 使用するポートを入力します (31210 から 31220 までの範囲)。 このポートを別のアプリケーションが使用することはできません。 | なし | はい |
| `プロキシーのホスト名 (Proxy hostname)` | プロキシー・サーバーのホスト名。 プロキシー IP と同じ値を入力します。 | なし | はい |
| `プロキシーのポート (Proxy port)` | 使用するポートを入力します (31210 から 31220 までの範囲)。 このポートを別のアプリケーションまたはコンソールが使用することはできません。 | なし | はい |
| **ストレージ設定 (Storage settings)** | **コンソールで使用するストレージ** | | |
| `ストレージ・クラス名 (Storage class name)` | コンソールと作成するコンポーネントで使用するストレージ・クラスの名前を入力します。 | なし | はい |
{: caption="表 1. クイック・スタート・パラメーター" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  パラメーター     | 説明    | デフォルト  | 必須 |
| --------------|-----------------|-------|------- |
| `使用許諾条件` | ライセンス条件に同意することを示すには、「受け入れる」に設定します。 | 受け入れない (Not accepted) | はい |
| `アーキテクチャー (Architecture)` | クラウド・プラットフォーム・アーキテクチャーを選択します。 (AMD64 または S390x) | AMD64 | いいえ |
| **コンソール設定 (Console settings)** | **コンソールの管理とデプロイメント** | | |
| `サービス・アカウント名 (Service account name)` | オペレーターが使用するサービス・アカウント。 | なし | いいえ |
| `プロキシー IP (Proxy IP)` | クラスター内のプロキシー・ノードの IP アドレス。 | なし | はい|
| `コンソール管理者の E メール (Console administrator email)` | コンソールへのログインに使用する E メール。 | なし | はい |
| `コンソール管理者のパスワード・シークレットの名前 (Console administrator password secret name)` | コンソールへのログインに使用する[パスワードを保管するために作成した](#console-deploy-icp-password-secret)シークレットの名前。 | なし | はい|
| **Docker イメージ設定 (Docker image settings)** | **これらの設定を使用して、コンソールがプルする Fabric イメージをカスタマイズできます** | | |
| `imagePullSecret 名 (imagePullSecret name)` | イメージのダウンロードに使用する imagePullSecret。 | `ibp-ibmregistry` | いいえ |
| **ネットワーク設定 (Network settings)** | **コンソールへのネットワーク・アクセス** | | |
| `コンソールのホスト名 (Console hostname)` | プロキシー IP と同じ値を入力します。 | なし | はい |
| `コンソールのポート (Console port)` | 使用するポートを入力します (31210 から 31220 までの範囲)。 | なし | はい |
| `プロキシーのホスト名 (Proxy hostname)` | プロキシー・サーバーのホスト名。 プロキシー IP と同じ値を入力します。 | なし | はい |
| `プロキシーのポート (Proxy port)` | 使用するポートを入力します (31210 から 31220 までの範囲)。 このポートを別のアプリケーションまたはコンソールが使用することはできません。 | なし | はい |
| `TLS シークレット (TLS secret)` | コンソールで使用する [TLS 証明書を保管するために作成した](#console-deploy-icp-tls-secret)シークレットの名前。 | なし | いいえ |
| **ストレージ設定 (Storage settings)**| **コンソールとツール用のストレージをプロビジョンします**。 | | |
| `ボリューム・クレーム・サイズ (Volume claim size)`| プロビジョンする永続ボリューム・クレームのサイズ。 | 10Gi  | いいえ |
| `ストレージ・クラス名 (Storage class name)`| コンソールと作成するコンポーネントで使用するストレージ・クラスの名前。 | なし | はい |
| `ストレージ・アクセス・モード (Storage access mode)`| PVC のストレージ・[アクセス・モード](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes)を指定します。  | ReadWriteMany | いいえ |
| **リソースの割り当て (Allocate resources)**| **コンソールにリソースを割り当てます**。 | | |
| `opstools CPU 制限 (Opstools CPU limit)` | opstools コンポーネントに割り当てる CPU の最大数。 | 500m | いいえ |
| `opstools メモリー制限 (Opstools memory limit)` | opstools コンポーネントに割り当てるメモリーの最大容量。 | 1000Mi | いいえ |
| `opstools CPU 要求 (Opstools CPU request)` | opstools コンポーネントに割り当てる CPU の最小数。 | 500m | いいえ |
| `opstools メモリー要求 (Opstools memory request)` | opstools コンポーネントに割り当てるメモリーの最小容量。| 1000Mi | いいえ |
| `configtxlator CPU 制限 (Configtxlator CPU limit)` | configtxlator ツールに割り当てる CPU の最大数。 | 25m | いいえ |
| `configtxlator メモリー制限 (Configtxlator memory limit)` | configtxlator ツールに割り当てるメモリーの最大容量。 | 50Mi | いいえ |
| `configtxlator CPU 要求 (Configtxlator CPU request)` | configtxlator ツールに割り当てる CPU の最小数。| 25m | いいえ |
| `configtxlator メモリー要求 (Configtxlator memory request)` | configtxlator ツールに割り当てるメモリーの最小容量。| 50Mi | いいえ |
| `CouchDB CPU 制限 (CouchDB CPU limit)` | CouchDB に割り当てる CPU の最大数。 | 500m | いいえ |
| `CouchDB メモリー制限 (CouchDB memory limit)` | CouchDB に割り当てるメモリーの最大量。 | 1000Mi | いいえ |
| `CouchDB CPU 要求 (CouchDB CPU request)` | CouchDB に割り当てる CPU の最小数。| 500m | いいえ |
| `CouchDB メモリー要求 (CouchDB memory request)` | CouchDB に割り当てるメモリーの最小量。| 1000Mi | いいえ |
| `operator CPU 制限 (Operator CPU limit)` | operator コンポーネントに割り当てる CPU の最大数。 | 100m | いいえ |
| `operator メモリー制限 (Operator memory limit)` | operator コンポーネントに割り当てるメモリーの最大容量。 | 200Mi | いいえ |
| `operator CPU 要求 (Operator CPU request)` | operator コンポーネントに割り当てる CPU の最小数。 | 100m | いいえ |
| `operator メモリー要求 (Operator memory request)` | operator コンポーネントに割り当てるメモリーの最小容量。 | 200Mi | いいえ |
| `deployer CPU 制限 (Deployer CPU limit)` | deployer コンポーネントに割り当てる CPU の最大数。 | 100m | いいえ |
| `deployer メモリー制限 (Deployer memory limit)` | deployer コンポーネントに割り当てるメモリーの最大容量。 | 200Mi | いいえ |
| `deployer CPU 要求 (Deployer CPU request)` | deployer コンポーネントに割り当てる CPU の最小数。 | 100m | いいえ |
| `deployer メモリー要求 (Deployer memory request)` | deployer コンポーネントに割り当てるメモリーの最小容量。 | 200Mi | いいえ |
{: caption="表 2. すべてのパラメーター" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Helm コマンド・ラインを使用した Helm リリースのインストール
{: #console-deploy-icp-helm-cli}

また、`helm` CLI を使用して Helm リリースをインストールすることもできます。 `helm install` コマンドを実行する前に、[Helm CLI 環境にクラスターの Helm リポジトリーを追加](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}してください。

インストールに必要なパラメーターを設定するには、`yaml` ファイルを作成し、以下の `helm install` コマンドにこれを渡します。
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

説明:
- `<helm_release name>`: Helm リリースに指定する名前を表します。
- `<helm_chart>`: カタログにインポートされる Helm チャートの名前を表します。
- `<helm_chart_version>`: カタログにインポートされる Helm チャートのバージョンを表します。
- `<customvalues.yaml>`: 構成パラメーターを含む yaml ファイルの名前です。

例えば、次のようにします。
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

ダウンロードしたアーカイブ・ファイルに含まれる、必要なパラメーターとそのデフォルト設定をすべて含む `values.yaml` を編集することで、新規 `yaml` ファイルを作成することができます。

## インストールの検証

構成パラメーターを入力し、**「インストール」**ボタンをクリックした後、**「Helm リリースの表示」**ボタンをクリックしてデプロイメントを表示します。 成功した場合は、「デプロイメント」テーブルの`「必要数」`、`「現行」`、`「最新」`、および`「使用可能」`の各フィールドに値 1 が表示されます。 最新表示をクリックしてテーブルが更新されるまで待つ必要がある場合があります。

デプロイメントの詳細を表示するには、**「デプロイメント」**概要画面にナビゲートし、作成されたポッドをクリックします。 Helm チャート・デプロイメントにより、次の 5 つのコンテナーがクラスター上に作成されます。
- **optools**: コンソール UI。
- **deployer**: コンソールがデプロイメントと通信できるようにするツール。
- **configtxlator**: コンソールがチャネル更新の読み取りと作成に使用するツール。
- **couchdb**: 許可情報などのコンソールのデータを保管する CouchDB のインスタンス。
- **operator**: コンポーネント・デプロイメントを管理する kubernetes オペレーター。

## コンソールへのログイン

インストールしたら、ブラウザーを使用してコンソールにアクセスできます。 URL は、デプロイメント後に開く Helm リリースの概要画面の**「注: (Notes:)」**セクションに示されます。 ESR バージョンの Firefox を使用していないことを確認してください。 ESR バージョンを使用している場合は、Chrome などの別のブラウザーに変更して再試行してください。

ブラウザーに、コンソール・ログイン画面が表示されます。
- **「ユーザー ID」**には、構成時に`「コンソール管理者の E メール (Console administrator email)」`フィールドに指定した値を使用します。
- **「パスワード」**には、エンコードして[パスワード・シークレット](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)内に保管し、構成時にコンソールに渡した値を使用します。 このパスワードが、すべての新規ユーザーがコンソールへの初回ログインに使用するデフォルト・パスワードになります。 初回ログインの後に、コンソールへのログインに使用する新規パスワードの入力を求められます。

helm チャートをプロビジョンした管理者は、他のユーザーにコンソールへのアクセス権限を付与し、それらのユーザーが実行できる操作を指定できます。 詳しくは、[コンソールからのユーザーの管理](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)を参照してください。

## 次のステップ
{: #console-deploy-icp-next-steps}

コンソールにアクセスすると、コンソール UI の**「ノード」**タブが表示されます。 この画面を使用して、ローカル・クラスターにコンポーネントをデプロイできます。 コンソールを使い始めるには、[ネットワーク構築チュートリアル](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)を参照してください。 このタブを使用して、他のクラウドで作成されたノードを操作することもできます。 詳しくは、[ノードのインポート](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes)を参照してください。

コンソールにアクセスできるユーザーの管理方法、{{site.data.keyword.blockchainfull_notm}} Platform API の使用方法、コンソールとブロックチェーン・コンポーネントのログの表示方法については、[{{site.data.keyword.cloud_notm}} Private でのコンソールの管理](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage)を参照してください。
