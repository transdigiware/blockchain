---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud について
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform は、{{site.data.keyword.cloud_notm}}、独自のデータ・センター、サード・パーティーのパブリック・クラウドなど、複数のパブリック・クラウドおよびプライベート・クラウド間にデプロイできます。このマルチクラウド・デプロイメントは、IBM の Kubernetes ベースのコンテナー・オーケストレーション・プラットフォームである {{site.data.keyword.cloud_notm}} Private によって実現できます。このリリースには、{{site.data.keyword.cloud_notm}} Private クラスターでブロックチェーン・コンポーネントをデプロイおよび管理するために使用できるコンソール・ユーザー・インターフェースが用意されています。{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private は Hyperledger Fabric v1.4.1 コード・ベースを使用し、{{site.data.keyword.cloud_notm}} Private v3.2 でのデプロイメントをサポートしています。
{:shortdesc}

この {{site.data.keyword.blockchainfull_notm}} Platform リリースには、以下の主要機能が含まれています。

**構築 ---- 一体化された開発者の操作**
- Node.js、Golang、または Java でスマート・コントラクトを**簡単にコーディングし**、新しい {{site.data.keyword.blockchainfull_notm}} VS Code 拡張機能を使用してクライアント・アプリケーションを作成し、ユーザー・インターフェース・コンソールと **SDK の統合**を活用し、豊富なチュートリアルおよびサンプルを参考にすることができます。
- **シンプルになった DevOps** により、Kubernetes リソースをスケールアップしてコンポーネントを追加することで、単一の環境で開発からテスト、本番環境へと移行できます。
- **Kubernetes サービス統合**。ロギングには Grafana や Prometheus などのサービスを利用し、モニタリングには Kibana を利用します。
- **Fabric の最新の主要な機能**。 以下の Hyperledger Fabric v1.4.1 の最新機能を利用できます。
  - [Raft 順序付けサービス](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [プライベート・データ・コレクション](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data): ゴシップ・プロトコルにより、許可されたピアでのみ台帳データを共有することでデータ・プライバシーを向上させます。
  - [サービス・ディスカバリー](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}: アプリケーションとネットワークがどのように対話しているかを動的に検出し、更新できます。
  - [チャネル・アクセス制御リスト](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}: チャネルおよびスマート・コントラクトのガバナンス・コントロールを強化します。

**操作 --- デプロイメントの完全な制御**
- **必要なコンポーネントのみデプロイする**。 ピアを複数のチャネルおよびネットワークに接続したり、ビジネス・パートナーが接続できる順序付けサービスをホストしたりできます。
- **ID の完全な制御を維持する**。 独自のセキュアな環境でノードを管理するために使用する鍵を保管および管理します。
- **運用の統合**。{{site.data.keyword.blockchainfull_notm}} Platform コンソールにより、すべての組織とノードのデプロイおよび管理を、**1 つのコンソール**で行えるので、順序付けプログラムや認証局の管理を {{site.data.keyword.IBM_notm}} や他のベンダーに委ねる必要がありません。 また、ブロックチェーン・コンソーシアムのメンバーの追加/削除、チャネルの作成およびチャネルへの参加、スマート・コントラクトのインストールおよびインスタンス化もコンソールから行えます。
- **ネットワークをホストする、またはネットワークに参加する**。 クラスターでホストされるピアを、複数のクラウド上の複数のチャネルにデプロイできます。また、コンソーシアムやチャネルに参加するように他の組織を招待できます。それらの組織は、インフラストラクチャーをまたいで自ノードを独立して管理することができます。
- **アクセスを管理できる**。ノードを管理またはモニターできるユーザーのアクセスを管理できます。
- **ログに直接アクセスできる**。Kubernetes サービスのノードのログに直接アクセスできます。 サポートされている任意のサード・パーティー・サービスを使用して、ログを抽出し、分析できます。
- Kubernetes サービスを使用して、**ポッドと直接対話できる**。
- **動的な署名の収集**。これにより、チャネルの構成に対する共同のガバナンスの制御を改善できます。

**拡張 --- スケーラビリティーと柔軟性**
- **コンピュート能力を選択できる**。 Kubernetes クラスターにプロビジョンする CPU、メモリー、およびストレージの容量を柔軟に選択できます。 詳しくは、[コンソールと Kubernetes クラスターの対話](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction)を参照してください。
- Kubernetes クラスターのリソースを**拡張/縮小**して、必要な分のみ支払うことができます。 詳しくは、[料金](/docs/services/blockchain/howto/pricing.html#ibp-pricing)を参照してください。
- **災害復旧および複数ゾーンによる高可用性。** この機能により、Kubernetes のデプロイメントをゾーン間で複製し、コンポーネントの高可用性 (HA) と災害復旧 (DR) を実現できます。
- **どこでも実行可能**。 {{site.data.keyword.blockchainfull_notm}} Platform コンソールの**統合コードベース**により、{{site.data.keyword.cloud_notm}} Private でサポートされる任意の環境でコンポーネントを実行できます。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は {{site.data.keyword.cloud_notm}} Private 用のバンドル製品で、Kubernetes Helm チャートとして提供されます。{{site.data.keyword.cloud_notm}} Private について詳しくは、[{{site.data.keyword.cloud_notm}} Private バージョン 3.1.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} の資料を参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の提供内容

{{site.data.keyword.cloud_notm}} Platform for {{site.data.keyword.blockchainfull_notm}} Private では、{{site.data.keyword.cloud_notm}} Private のデプロイメントに {{site.data.keyword.blockchainfull_notm}} Platform コンソールをインストールできます。その後、そのコンソールを使用して、Hyperledger Fabric ブロックチェーンのすべての基礎コンポーネント、認証局、順序付けサービス、およびピアをローカル・クラスター上に作成できます。Hyperledger Fabric ネットワークのビルディング・ブロックについて詳しくは、[ブロックチェーン・コンポーネントの概要](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview)を参照してください。 また、他の {{site.data.keyword.cloud_notm}} プライベート・クラスター上または {{site.data.keyword.cloud_notm}} 上にデプロイされたノードをインポートすれば、コンソールを使用して分散マルチクラウド・ネットワークを操作することもできます。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の使用に適した状況

{{site.data.keyword.cloud_notm}} 外部で {{site.data.keyword.blockchainfull_notm}} Platform を実行すると、ブロックチェーン・ネットワークの拡張や参加をより柔軟に行うことができます。 これを使用すると、新規メンバーが各自のプラットフォームを使用したまま参加できるようになるので、ネットワーク・イニシエーターはネットワークの拡張を促進できます。 これにより、ブロックチェーン・ネットワークへの参加に関心のある組織は、ピアを既存のアプリケーションと同じ場所に置いたり、組織の記録システムと統合したりできます。

このオファリングのユーザーは、独自のセキュリティーとインフラストラクチャーを管理することになります。 {{site.data.keyword.cloud_notm}} では、これらのサービスは提供されません。 開始する前に、次のセクションの[考慮事項と制限](#console-icp-about-considerations)を確認してください。

## 考慮事項と制限
{: #console-icp-about-considerations}

始める前に、以下の制限事項について理解してください。

- ブロックチェーン・コンポーネントの正常性モニター、セキュリティー、ロギング、リソース使用状況を管理するのはお客様の責任です。
- このコンソールは、Hyperledger Fabric v1.4.1 以上をベースとするコンポーネントの作成および管理にのみ使用できます。
- 他の {{site.data.keyword.blockchainfull_notm}} Platform コンソールからエクスポートされたノードのみをインポートできます。インポートされたピアや順序付けプログラムをコンソールから操作できるようにするには、関連付けられているノードの組織 MSP 定義および管理者 ID もコンソールにインポートする必要があります。

{{site.data.keyword.cloud_notm}} Private でサポートされる環境については、[Supported environments](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external} を参照してください。
{:important}

## システム前提条件
{: #console-icp-about-prerequisites}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private では、以下のオペレーティング・システムがサポートされています。
- Red Hat Enterprise Linux (RHEL) 7.3、7.4、7.5
- Ubuntu 18.04 LTS および 16.04 LTS
- SUSE Linux Enterprise Server (SLES) 12 SP3

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm チャートは、以下のワーカー・ノードおよび補助ストレージを使用して、Ubuntu Linux 上の {{site.data.keyword.cloud_notm}} Private v3.2 クラスターで稼働することが検証されました。

- **LinuxONE および {{site.data.keyword.IBM_notm}} Z**: NFS を使用する z/VM および KVM
- **x86**: GlusterFS を使用する Linux 64 ビット

## 使用許諾条件
{: #console-icp-about-license}

ライセンス交付および価格設定について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の価格設定](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)を参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private のインストール
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は、ローカル {{site.data.keyword.cloud_notm}} Private クラスターにインストール可能な Helm チャート・ファイルとして提供されます。 Helm チャートをインストールすると、{{site.data.keyword.cloud_notm}} Private カタログで {{site.data.keyword.blockchainfull_notm}} Platform をアプリケーションとして見つけることができます。 Helm チャートのインストール方法に関する手順および必要な前提条件については、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private のインストール](/docs/services/blockchain/howto/console-helm-install.html#helm-console-install)を参照してください。

{{site.data.keyword.cloud_notm}} Private の新規ユーザーであり、{{site.data.keyword.cloud_notm}} Private のインストールおよびデプロイに関する情報とヒントが必要な場合は、[{{site.data.keyword.cloud_notm}} Private のセットアップ](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup)を参照してください。

パブリック・インターネットへのアクセス権限がなくても、ファイアウォールの内側に {{site.data.keyword.blockchainfull_notm}} Platform をデプロイできます。 ダウンロードされた Helm チャート・パッケージには、{{site.data.keyword.blockchainfull_notm}} Platform で使用するすべての Fabric コンポーネントの Docker イメージが、デプロイメント時に DockerHub からプルされることなく含まれます。

## セキュリティーに関する考慮事項
{: #console-icp-about-security}

これらのコンポーネントは独自のインフラストラクチャー上にデプロイされるため、そのセキュリティーはユーザーが管理する必要があります。これには、鍵管理やデータ暗号化など、セキュリティーの重要な領域が含まれます。 コンポーネントのセキュリティーを考慮する場合は、以下のトピックを確認してください。

### データ・セキュリティー
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform のスターター・プランおよびエンタープライズ・プランでは、[対称鍵暗号](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external}に基づくディスク全体の暗号化を使用して、ネットワークで使用されるすべてのデータを保護します。 ご使用の環境でも同様のステップを実行して、ピア・データを保護する必要があります。

状態データベース内のデータは、LevelDB と CouchDB のどちらを使用するかに関係なく、暗号化されません。 アプリケーション・レベルの暗号化によって、状態データベースに保管されているデータを保護できます。

### データの常駐
{: #console-icp-about-security-data-residency}

データの常駐要件により、すべてのブロックチェーン台帳データの処理および保管を 1 つの国 (または定義された他の境界内) にとどめることを義務付けることができます。 データの常駐の実現方法について詳しくは、[データの常駐](#console-icp-about-data-residency)を参照してください。

### 鍵管理
{: #console-icp-about-security-key-management}

鍵管理はセキュリティーの重要な側面です。 秘密鍵が漏えいしたり、失われたりすると、悪意を持つアクターがデータおよび機能にアクセスできる可能性があります。 {{site.data.keyword.IBM_notm}} では、[ハードウェア・セキュリティー・モジュール](/docs/services/blockchain/glossary.html#glossary-hsm) (HSM) と呼ばれる物理アプライアンスを使用して、{{site.data.keyword.blockchainfull_notm}} Platform エンタープライズ・プラン・ネットワークの秘密鍵を保管します。

{{site.data.keyword.cloud_notm}} Private にコンポーネントをデプロイする場合は、ユーザーが秘密鍵を管理する必要があります。 {{site.data.keyword.blockchainfull_notm}} Platform によって秘密鍵が生成されますが、鍵は Platform には保管されません。 鍵が漏えいしないように、鍵を安全に保管することが重要です。 コンポーネントの秘密鍵は、ピア MSP の keystore フォルダー (コンポーネント内の `/mnt/crypto/peer/peer/msp/keystore/` ディレクトリー) にあります。 ピア内部の証明書について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform の証明書の管理](/docs/services/blockchain/certificates.html#managing-certificates)のチュートリアルの[メンバーシップ・サービス・プロバイダー](/docs/services/blockchain/certificates.html#managing-certificates-msp)のセクションを参照してください。

鍵エスクローを使用して、失われた秘密鍵を復旧することができます。 これは、鍵が失われる前に実行する必要があります。 秘密鍵を復旧できない場合は、認証局に新しい ID を登録して、新しい秘密鍵を取得する必要があります。 また、参加しているチャネルから signCert を削除し、置き換える必要があります。

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) は、Hyperledger Fabric の信頼モデルに組み込まれています。 {{site.data.keyword.blockchainfull_notm}} Platform のすべてのスターター・コンポーネントとエンタープライズ・コンポーネントでは、TLS を使用して認証し、相互に通信します。 したがって、Platform のネットワーク・コンポーネントは、ピアとの TLS ハンドシェークを実行できる必要があります。 これに関連して、ホワイト・リストなどを使用して、クライアント・アプリからピアまでファイアウォールでパススルーできる必要があります。

### メンバーシップ・サービス・プロバイダーの構成
{: #console-icp-about-security-MSP}

{{site.data.keyword.blockchainfull_notm}} Platform のコンポーネントは、メンバーシップ・サービス・プロバイダー (MSP) を介して ID を使用します。 MSP は、CA から発行された証明書を、ネットワークとチャネルの役割に関連付けます。MSP がピアを使用する方法について詳しくは、[メンバーシップ・サービス・プロバイダー (MSP)](/docs/services/blockchain/certificates.html#managing-certificates-msp) を参照してください。

### アプリケーション・セキュリティー
{: #console-icp-about-security-appl}

すべてのチェーンコード呼び出しは署名されているため、Fabric によってアプリケーション・セキュリティーが管理されます。 また、Fabric には ACL ベースのアプリケーション・レベル・チェックも含まれています。

## データの常駐
{: #console-icp-about-data-residency}

ブロックチェーン・ネットワークでは、処理されるデータのタイプが認識されないため、特定の種類のデータを保護するために追加のステップが必要になる場合があります。 データの常駐に関する最も一般的な要件は、IT システムで処理および保管されるすべてのデータを特定の国の中にとどめることを義務付ける、特定の国の法律に関連付けられています。 同様に、政府、医療、金融サービスなど、規制の厳しい業界の企業は、データを完全にファイアウォールの内側に保管することが求められます。 したがって、データの常駐を実現するには、ブロックチェーン・ネットワークのすべてのコンポーネントが同じ[チャネル](/docs/services/blockchain/glossary.html#glossary-channel)の一部であり、1 つの国に常駐する必要があります。

データの常駐要件に対応するには、{{site.data.keyword.blockchainfull_notm}} Platform の基礎となっている Hyperledger Fabric アーキテクチャーを理解することが重要です。 このアーキテクチャーは、順序付けサービス (順序付けプログラムで構成)、認証局 (CA)、およびピアという 3 つの主要コンポーネントを中核としています。 ピアは順序付けサービスから順序付け状態の更新をブロックの形式で受け取り、状態および台帳を維持します。 したがって、ピアと順序付けサービスには直接的な関係があります。 台帳には、トランザクション・ログに含まれるすべてのキーおよびデータの最新の値が含まれます。

さらに、クライアント・アプリケーションは、[Fabric SDK](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external} を使用してトランザクションをピアおよび順序付けサービスに送信します。 これらのトランザクションには、台帳のキーと値のペアを含む[読み取り/書き込みセット](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}のデータが含まれます。

データの国内常駐が要件となっている場合、順序付けプログラム、ピア、およびクライアント・アプリケーションが同じ国に常駐する必要があります。 {{site.data.keyword.blockchainfull_notm}} Platform ネットワークが {{site.data.keyword.cloud_notm}} で作成されている場合、ネットワークのロケーションを選択することができます。 <!--For a Starter Plan network, you can select from US South, United Kingdom, and Sydney. For an Enterprise Plan network, you can select from currently available locations, which include Dallas, Frankfurt, London, Sao Paulo, Tokyo, and Toronto. -->地域とロケーションについて詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform の地域とロケーション](/docs/services/blockchain/reference/ibp_regions.html#ibp-regions-locations)を参照してください。

### データの常駐のユース・ケース

4 つの組織の共同事業体とともに順序付けプログラムと CA を含む {{site.data.keyword.blockchainfull_notm}} Platform ネットワークを考えてみましょう。 組織には 1 つ以上のピア・ノードがあり、4 つの組織はすべて単一チャネルの一部で、ネットワークのすべてのコンポーネントは {{site.data.keyword.blockchainfull_notm}} Platform ネットワークがデプロイされた地域に常駐します (フランクフルトなど)。 最後に、ピアと相互動作するクライアント・アプリケーションもドイツ内にあります。

この例では、データの常駐は維持されます。

![すべてのコンポーネントが同じ国内に存在する場合のデータの常駐](images/remote_peer_data_res_1.png "すべてのコンポーネントが同じ国内に存在する場合のデータの常駐")

ここで、**分散ピア**がいずれかの組織に参加した場合の影響について考えてみましょう。 分散ピアは、ネットワークの他の部分と同じ地域に常駐することも、{{site.data.keyword.blockchainfull_notm}} Platform ネットワークの地域外の任意の場所に常駐することもできます。

-	このピアがネットワークの残り部分と同じ国にある場合は、データの常駐は維持されます。 上記の**図 1** のように、すべての台帳データはドイツ内にとどまります。
-	このピアが別の国 (米国など) に存在する場合は、ピア台帳上のデータは国境にまたがって共有されるため、データの常駐は維持されなくなります。

この問題を解決するために、**チャネル**を使用してネットワーク上のピアのサブセットに対してデータを分離できます。 {{site.data.keyword.blockchainfull_notm}} Platform ネットワークに、国をまたぐ分散ピアおよび順序付けプログラムが含まれる場合、チャネルを使用して、国外のピアを含む組織から元帳データを分離できます。

**注:** 順序付けプログラムは常に、ネットワークをホストするように選択したデータ・センターの地域に配置されます。 国境にまたがる複数の順序付けプログラムを保持することはできません。 ただし、ピアは、データ・センターと {{site.data.keyword.cloud_notm}} の外側のリモート・ロケーションのいずれにも配置できます。

![ピアが {{site.data.keyword.blockchainfull_notm}} Platform の地域の国外に存在する場合のデータの所在場所](images/remote_peer_data_res_2.png "ピアが {{site.data.keyword.blockchainfull_notm}} Platform の地域の国外に存在する場合のデータの所在場所")

**図 2** では、`OrgC` および `OrgD` にデータの常駐は必要ありません。 実際、`OrgD` には現在、*米国*に常駐する `OrgD-peer1` と `OrgD-peer2` の 2 つの分散ピアが含まれています。 したがって、`OrgA`、`OrgB`、およびドイツに常駐するそれぞれのクライアント・アプリケーションとピアがチャネル `X` の元帳データを分離できるように、新規チャネル `Y` が `OrgC` および `OrgD` 用に作成されます。

{{site.data.keyword.blockchainfull_notm}} Platform ネットワーク上のデータのフローについて詳しくは、[トランザクション・フローに関する Fabric の資料](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}を参照してください。

今後、Hyperledger Fabric の新しいテクノロジーによって、[プライベート・データ・コレクション](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external}およびゼロ知識証明を使用して、さらなるデータの常駐を実現する機能が改善されます。

- プライベート・データ・コレクションによって、プライベート・データが、国内のピアなど、表示を許可されたピアにのみピアツーピアで共有されます (ゴシップ・プロトコルによって)。 データはピアのプライベート・データベースに保管されます。 順序付けサービスは、ここでは関与しません。またプライベート・データを認識することもありません。 チャネルのすべてのピアの台帳にこのデータのハッシュが書き込まれます。 状態検証で使用されるハッシュは、トランザクションの証拠として機能し、監査目的で使用することができます。 プライベート・データは、Fabric バージョン 1.2.1 以降上で稼働している {{site.data.keyword.blockchainfull_notm}} Platform 上のネットワークで使用できます。 ただし、{{site.data.keyword.cloud_notm}} Private ネットワークでは現在ゴシップを使用していないため、{{site.data.keyword.cloud_notm}} Private で実行されているピアに対してプライベート・データ機能を使用することはできません。

- ゼロ知識証明 (ZKP) により、「証明者」は「検証者」に、機密事項自体を示すことなく、機密事項に関する知識があることを保証できます。

これらのテクノロジーについて詳しくは、[Hyperledger Fabric を使用したプライベートおよび機密トランザクション](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}に関するホワイト・ペーパーを参照してください。

## サポートについて
{: #console-icp-about-support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} に関するサポートを受ける方法、および問題のトラブルシューティングに利用できるブロックチェーン開発者向けの無料リソースとサポート・フォーラムについて詳しくは、[サポートについて](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support)を参照してください。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private ライセンスを購入し、お客様サポートに連絡する場合は、[{{site.data.keyword.IBM_notm}} Support Community へのアクセスおよびサポート・チケットのオープン](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}を参照してください。
