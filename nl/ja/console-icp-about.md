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

{{site.data.keyword.blockchainfull}} Platform は、{{site.data.keyword.cloud_notm}}、独自のデータ・センター、サード・パーティーのパブリック・クラウドなど、複数のパブリック・クラウドおよびプライベート・クラウド間にデプロイできます。 このマルチクラウド・デプロイメントは、IBM の Kubernetes ベースのコンテナー・オーケストレーション・プラットフォームである {{site.data.keyword.cloud_notm}} Private によって実現できます。 このリリースには、{{site.data.keyword.cloud_notm}} Private クラスターでブロックチェーン・コンポーネントをデプロイおよび管理するために使用できるブロックチェーン・コンソール・ユーザー・インターフェースが用意されています。{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private は Hyperledger Fabric v1.4.1 コード・ベースを使用し、{{site.data.keyword.cloud_notm}} Private v3.2.0 でのデプロイメントをサポートしています。
{:shortdesc}

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の提供内容
{: #console-icp-about-offers}

このオファリングを使用して、{{site.data.keyword.cloud_notm}} Private のデプロイメントに {{site.data.keyword.blockchainfull_notm}} Platform コンソールをインストールできます。その後、そのコンソールを使用して、Hyperledger Fabric ブロックチェーンのすべての基礎コンポーネント、認証局、順序付けサービス、およびピアをローカル・クラスター上に作成できます。 Hyperledger Fabric ネットワークのビルディング・ブロックについて詳しくは、[ブロックチェーン・コンポーネントの概要](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview)を参照してください。 また、他の {{site.data.keyword.cloud_notm}} プライベート・クラスター上または {{site.data.keyword.cloud_notm}} 上にデプロイされたノードをインポートすれば、コンソールを使用して分散マルチクラウド・ネットワークを操作することもできます。

この {{site.data.keyword.blockchainfull_notm}} Platform リリースには、以下の主要機能が含まれています。

**構築 ---- 一体化された開発者の操作**
- Node.js、Golang、または Java でスマート・コントラクトを**簡単にコーディングし**、新しい {{site.data.keyword.blockchainfull_notm}} VS Code 拡張機能を使用してクライアント・アプリケーションを作成し、ユーザー・インターフェース・コンソールと **SDK の統合**を活用し、豊富なチュートリアルおよびサンプルを参考にすることができます。
- **シンプルになった DevOps** により、Kubernetes リソースをスケールアップしてコンポーネントを追加することで、単一の環境で開発からテスト、本番環境へと移行できます。
- **Kubernetes サービス統合**。 ロギングには Grafana や Prometheus などのサービスを利用し、モニタリングには Kibana を利用します。
- **Fabric の最新の主要な機能**。 以下の Hyperledger Fabric v1.4.1 の最新機能を利用できます。
  - [Raft 順序付けサービス](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [プライベート・データ・コレクション](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data): ゴシップ・プロトコルにより、許可されたピアでのみ台帳データを共有することでデータ・プライバシーを向上させます。
  - [サービス・ディスカバリー](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}: アプリケーションとネットワークがどのように対話しているかを動的に検出し、更新できます。
  - [チャネル・アクセス制御リスト](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}: チャネルおよびスマート・コントラクトのガバナンス・コントロールを強化します。

**操作 --- デプロイメントの完全な制御**
- **必要なコンポーネントのみデプロイする**。 ピアを複数のチャネルおよびネットワークに接続したり、ビジネス・パートナーが接続できる順序付けサービスをホストしたりできます。
- **ID の完全な制御を維持する**。 独自のセキュアな環境でノードを管理するために使用する鍵を保管および管理します。
- **運用の統合**。 {{site.data.keyword.blockchainfull_notm}} Platform コンソールにより、すべての組織とノードのデプロイおよび管理を、**1 つのコンソール**で行えるので、順序付けノードや認証局の管理を {{site.data.keyword.IBM_notm}} や他のベンダーに委ねる必要がありません。 また、ブロックチェーン・コンソーシアムのメンバーの追加/削除、チャネルの作成およびチャネルへの参加、スマート・コントラクトのインストールおよびインスタンス化もコンソールから行えます。
- **ネットワークをホストする、またはネットワークに参加する**。 クラスターでホストされるピアを、複数のクラウド上の複数のチャネルにデプロイできます。また、コンソーシアムやチャネルに参加するように他の組織を招待できます。それらの組織は、インフラストラクチャーをまたいで自ノードを独立して管理することができます。
- **アクセスを管理できる**。ノードを管理またはモニターできるユーザーのアクセスを管理できます。
- **ログに直接アクセスできる**。Kubernetes サービスのノードのログに直接アクセスできます。 サポートされている任意のサード・パーティー・サービスを使用して、ログを抽出し、分析できます。
- Kubernetes サービスを使用して、**ポッドと直接対話できる**。
- **動的な署名の収集**。これにより、チャネルの構成に対する共同のガバナンスの制御を改善できます。

**拡張 --- スケーラビリティーと柔軟性**
- **コンピュート能力を選択できる**。 Kubernetes クラスターにプロビジョンする CPU、メモリー、およびストレージの容量を柔軟に選択できます。 詳しくは、[コンソールと Kubernetes クラスターの対話](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction)を参照してください。
- Kubernetes クラスターのリソースを**拡張/縮小**して、必要な分のみ支払うことができます。 詳しくは、[料金](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing)を参照してください。
- **災害復旧および複数ゾーンによる高可用性。** この機能により、Kubernetes のデプロイメントをゾーン間で複製し、コンポーネントの高可用性 (HA) と災害復旧 (DR) を実現できます。
- **どこでも実行可能**。 {{site.data.keyword.blockchainfull_notm}} Platform コンソールの**統合コードベース**により、{{site.data.keyword.cloud_notm}} Private でサポートされる任意の環境でコンポーネントを実行できます。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は {{site.data.keyword.cloud_notm}} Private 用のバンドル製品で、Kubernetes Helm チャートとして提供されます。 {{site.data.keyword.cloud_notm}} Private について詳しくは、[{{site.data.keyword.cloud_notm}} Private バージョン 3.1.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} の資料を参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private の使用に適した状況
{: #console-icp-about-suitable}

{{site.data.keyword.cloud_notm}} 外部で {{site.data.keyword.blockchainfull_notm}} Platform を実行すると、ブロックチェーン・ネットワークの拡張や参加をより柔軟に行うことができます。 これを使用すると、新規メンバーが各自のプラットフォームを使用したまま参加できるようになるので、ネットワーク・イニシエーターはネットワークの拡張を促進できます。 これにより、ブロックチェーン・ネットワークへの参加に関心のある組織は、ピアを既存のアプリケーションと同じ場所に置いたり、組織の記録システムと統合したりできます。

このオファリングのユーザーは、独自のセキュリティーとインフラストラクチャーを管理することになります。 {{site.data.keyword.cloud_notm}} では、これらのサービスは提供されません。 開始する前に、次のセクションの[考慮事項と制限](#console-icp-about-considerations)を確認してください。

## 考慮事項と制限
{: #console-icp-about-considerations}

始める前に、以下の制限事項について理解してください。

- ブロックチェーン・コンポーネントの正常性モニター、セキュリティー、ロギング、リソース使用状況を管理するのはお客様の責任です。
- このコンソールは、Hyperledger Fabric v1.4.1 以上をベースとするコンポーネントの作成および管理にのみ使用できます。
- Kubernetes 名前空間ごとに 1 つのコンソールのみをデプロイできます。複数のブロックチェーン・ネットワークを作成する場合は (例えば、開発用、ステージング用、実動用に別々の環境を作成する場合など)、環境ごとに固有の名前空間を作成しておく必要があります。
- 他の {{site.data.keyword.blockchainfull_notm}} Platform コンソールからエクスポートされたノードのみをインポートできます。 インポートされたピアや順序付けノードをコンソールから操作できるようにするには、関連付けられているノードの組織 MSP 定義および管理者 ID もコンソールにインポートする必要があります。
- {{site.data.keyword.blockchainfull_notm}} Platform は、{{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition でのみサポートされます。{{site.data.keyword.cloud_notm}} Private v3.2 Community Edition はサポートされていません。
- {{site.data.keyword.IBM_notm}} Multicloud Manager を使用して {{site.data.keyword.blockchainfull_notm}} Platform Helm チャートをインストールすることはできません。

{{site.data.keyword.cloud_notm}} Private でサポートされる環境については、[Supported environments](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external} を参照してください。
{:important}

## システム前提条件
{: #console-icp-about-prerequisites}

{{site.data.keyword.cloud_notm}} Private のハードウェアおよびソフトウェアの前提条件のリストについては、[システム要件](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external}を参照してください。ただし、{{site.data.keyword.blockchainfull_notm}} Platform は、Linux on Power (ppc64le) POWER8 システムではサポートされないことに注意してください。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm チャートは、以下のワーカー・ノードおよび補助ストレージを使用して、Ubuntu Linux 上の {{site.data.keyword.cloud_notm}} Private v3.2 クラスターで稼働することが検証されました。

- **LinuxONE および {{site.data.keyword.IBM_notm}} Z**: NFS を使用する z/VM および KVM
- **x86**: GlusterFS を使用する Linux 64 ビット

## 使用許諾条件
{: #console-icp-about-license}

ライセンス交付および価格設定について詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の価格設定](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)を参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private のインストール
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は、ローカル {{site.data.keyword.cloud_notm}} Private クラスターにインストール可能な Helm チャート・ファイルとして提供されます。 Helm チャートをインストールすると、{{site.data.keyword.cloud_notm}} Private カタログで {{site.data.keyword.blockchainfull_notm}} Platform をアプリケーションとして見つけることができます。 Helm チャートのインストール方法に関する手順および必要な前提条件については、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private のインストール](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install)を参照してください。

{{site.data.keyword.cloud_notm}} Private の新規ユーザーであり、{{site.data.keyword.cloud_notm}} Private のインストールおよびデプロイに関する情報とヒントが必要な場合は、[{{site.data.keyword.cloud_notm}} Private のセットアップ](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)を参照してください。

パブリック・インターネットへのアクセス権限がなくても、ファイアウォールの内側に {{site.data.keyword.blockchainfull_notm}} Platform をデプロイできます。 ダウンロードされた Helm チャート・パッケージには、{{site.data.keyword.blockchainfull_notm}} Platform で使用するすべての Fabric コンポーネントの Docker イメージが、デプロイメント時に DockerHub からプルされることなく含まれます。

## セキュリティーに関する考慮事項
{: #console-icp-about-security}

これらのコンポーネントは独自のインフラストラクチャー上にデプロイされるため、そのセキュリティーはユーザーが管理する必要があります。 これには、鍵管理やデータ暗号化など、セキュリティーの重要な領域が含まれます。 コンポーネントのセキュリティーを考慮する場合は、以下のトピックを確認してください。

### データ・セキュリティー
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} では、[対称鍵暗号](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} に基づくディスク全体の暗号化を使用して、各サービス・インスタンスで作成されるすべてのデータを保護します。ご使用の環境でも同様のステップを実行して、ピア・データを保護する必要があります。

状態データベース内のデータは、LevelDB と CouchDB のどちらを使用するかに関係なく、暗号化されません。 アプリケーション・レベルの暗号化によって、状態データベースに保管されているデータを保護できます。

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### 鍵管理
{: #console-icp-about-security-key-management}

鍵管理はセキュリティーの重要な側面です。 秘密鍵が漏えいしたり、失われたりすると、悪意を持つアクターがデータおよび機能にアクセスできる可能性があります。 {{site.data.keyword.IBM_notm}} では、[ハードウェア・セキュリティー・モジュール](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) と呼ばれる物理アプライアンスを使用して、{{site.data.keyword.blockchainfull_notm}} Platform エンタープライズ・プラン・ネットワークの秘密鍵を保管します。

秘密鍵の管理は、お客様の責任となります。{{site.data.keyword.blockchainfull_notm}} Platform コンソール UI を使用して秘密鍵を生成できますが、それらの鍵は、コンソールによって保管されることや、{{site.data.keyword.cloud_notm}} Private 内に保管されることはありません。鍵が漏えいしないように、鍵を安全に保管することが重要です。

鍵エスクローを使用して、失われた秘密鍵を復旧することができます。 これは、鍵が失われる前に実行する必要があります。 秘密鍵を復旧できない場合は、認証局に新しい ID を登録して、新しい秘密鍵を取得する必要があります。 また、参加しているチャネルから signCert を削除し、置き換える必要があります。

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) は、Hyperledger Fabric の信頼モデルに組み込まれています。 すべての {{site.data.keyword.blockchainfull_notm}} Platform コンポーネントは、TLS を使用して相互に認証し、通信します。したがって、{{site.data.keyword.cloud_notm}} Private のノードは、他のコンポーネントおよびアプリケーションと TLS ハンドシェークを実行できる必要があります。これに関連して、ホワイト・リストなどを使用して、クライアント・アプリからノードまでファイアウォールでパススルーできる必要があります。

### アプリケーション・セキュリティー
{: #console-icp-about-security-appl}

すべてのチェーンコード呼び出しは署名されているため、Fabric によってアプリケーション・セキュリティーが管理されます。 また、Fabric には ACL ベースのアプリケーション・レベル・チェックも含まれています。

## サポートについて
{: #console-icp-about-support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} に関するサポートを受ける方法、および問題のトラブルシューティングに利用できるブロックチェーン開発者向けの無料リソースとサポート・フォーラムについて詳しくは、[サポートについて](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)を参照してください。

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private ライセンスを購入し、お客様サポートに連絡する場合は、[{{site.data.keyword.IBM_notm}} Support Community へのアクセスおよびサポート・チケットのオープン](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}を参照してください。
