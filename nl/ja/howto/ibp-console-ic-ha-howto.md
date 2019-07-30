---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# 複数地域の高可用性 (HA) デプロイメントのセットアップ
{: #ibp-console-hadr-mr}

複数地域の HA 構成により、可能な限り最高の HA 範囲が実現します。 複数の地理的地域にピアをデプロイすることにより、1 つの地域が使用不可になった場合に、他の地域のピアでトランザクションを続行できるようになります。 CA の複数地域の HA サポートおよび順序付けサービスは現在使用できないことに注意してください。

## 概要
{: #ibp-console-hadr-overview}

ピアに対して複数地域の HA サポートをセットアップする場合は、以下のタスクを実行します。
- {{site.data.keyword.cloud}} 内に、それぞれが異なる地域の Kubernetes クラスターにバインドされた複数のサービス・インスタンスを作成します。
- さまざまな地域にブロックチェーン・ノードを作成します。
- ノードのエクスポート/インポート機能を使用して、単一のコンソールからノードを管理します。

## 構成手順
{: #ibp-console-hadr-config}

組織ごとに冗長ピアを作成して複数地域の HA を構成するには、ブロックチェーン・ネットワークを構成する際に以下のステップを実行します。

1. 任意の地域に 3 つの {{site.data.keyword.cloud_notm}} Kubernetes クラスターを作成します。これらのクラスターは任意の地域に配置できますが、ハイパフォーマンスを実現するには、互いに比較的近くなるようにしてください。例えば、ウェスト・コースト (米国)、ロンドン、および東京の地域よりも、イースト・コースト (米国)、ウェスト・コースト (米国)、およびカナダの地域の方が適切です。
2. 新しい {{site.data.keyword.blockchainfull_notm}} Platform インスタンスをデプロイし、それを最初の地域のクラスターにリンクします。 次に、別の {{site.data.keyword.blockchainfull_notm}} Platform インスタンスをデプロイし、2 つ目の地域のクラスターにリンクします。 これらのステップを繰り返して、3 つ目のサービス・インスタンスを 3 つ目の地域のクラスターにリンクします。 完了すると、それぞれが異なる地域に配置された 3 つの別個のクラスターにリンクされた 3 つの別個の {{site.data.keyword.blockchainfull_notm}} Platform インスタンスと、3 つの別個のコンソールを持つようになります。

このチュートリアルでは、ピアが参加できるチャネルが定義された順序付けサービスが存在することを想定しています。
{: important}

### ステップ 1: ピア組織の CA およびメタデータをクラスター 1 に作成する
{: #ibp-console-hadr-peerCA}

1. ネットワーク構築チュートリアルの[ピア組織の CA を作成する](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA)の説明に従って、最初のクラスターに CA をデプロイします。 CA の**登録 ID** と**機密事項**の値をメモしておいてください。CA を他のクラスターにインポートするときに、これらの値を指定する必要があるためです。
2. CA タイルの状況標識が緑色の `Running` になったら、[CA を使用して ID を登録](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1)するための手順に従います。 この手順では、ピア用とピアの組織管理者 ID 用の 2 つの ID を作成します。
3. 最初のクラスターで、[ピアの組織の MSP 定義を作成](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1)します。
4. 最初のクラスターで、[ピアを作成](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create)します。
5. ピアに[スマート・コントラクトをインストール](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install)します。

チャネルでスマート・コントラクトをインスタンス化する前に、以下のステップに従って、順序付けサービスのチャネルにピアを参加させる必要があります。
- [ピアの組織定義をエクスポート](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote)し、順序付けサービス管理者と共有します。
- 順序付けサービス管理者は、[ピアの組織をインポート](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp)し、それをコンソーシアムに追加するための手順に従う必要があります。 また、これらの説明の手順を実行して、順序付けサービスを JSON ファイルにエクスポートする必要もあります。
- コンソールに[順序付けサービスの JSON ファイルをインポート](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer)します。
- [ピアをチャネルに参加させます](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)。
- 最後に、サービス・ディスカバリー、プライベート・データ、およびピア・ゴシップを使用する場合は、順序付けサービス管理者がチャネルで[アンカー・ピアを構成](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers)する必要があります。 HA のために、各冗長ピアをアンカー・ピアとして追加することをお勧めします。 これにより、ピアの 1 つが使用できなくなった場合に、組織間のゴシップを続行できます。   

これで、ピアをチャネルに参加させたため、チャネルで[スマート・コントラクトをインスタンス化](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)できます。

### ステップ 2: クラスター 1 からメタデータおよび ID をエクスポートする
{: #ibp-console-hadr-export-meta1}

1. CA 定義を JSON ファイルにエクスポートします。
   - **「ノード」**タブで CA を開きます。
   - ダウンロード・アイコンをクリックして、ブラウザー・セッションから CA の JSON ファイルを生成します。
2. ピアの組織の MSP 定義を JSON ファイルにエクスポートします。
   - **「組織」**タブの MSP 定義にナビゲートします。
   - タイルのダウンロード・アイコンをクリックします。
3. ウォレットから、ピアの組織管理者 ID をエクスポートします。
   - **「ウォレット (Wallet)」**タブにナビゲートします。
   - ピアの組織管理者 ID をクリックし、**「アイデンティティーのエクスポート (Export identity)」**をクリックします。
   - 組織管理者証明書を含む JSON ファイルが作成されます。 ファイル名をメモし、ファイルを保護します。これは、他のクラスターに追加のピアを作成するときに必要になるためです。

### ステップ 3: メタデータと ID をクラスター 2 および 3 にインポートする
{: #ibp-console-hadr-import-meta23}

1. クラスター 2 および 3 で、クラスター 1 からエクスポートした [CA の JSON ファイルをインポート](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca)します。  
2. JSON ファイルをアップロードしたら、CA 管理者の登録 ID と機密事項、および CA をクラスター 1 にデプロイしたときに使用した TLS CA 管理者の登録 ID と機密事項を入力する必要があります。
2. クラスター 1 からエクスポートしたピア組織の MSP 定義の JSON ファイルをインポートします。
   - **「組織」**タブで**「MSP 定義のインポート (Import MSP Definition)」**をクリックします。
   - **「ファイルの追加 (Add file)」**をクリックして、JSON ファイルをアップロードします。
   - **「MSP 定義の管理者 ID がある (I have an administrator identity for the MSP definition)」**チェック・ボックスをクリックして、他のゾーンに新規ピアを作成するときにこの MSP 定義を使用できるようにします。
   - **「MSP 定義のインポート (Import MSP Definition)」**をクリックします。
3. クラスター 1 からエクスポートした組織管理者 ID の JSON ファイルを、クラスター 2 および 3 のコンソール・ウォレットにインポートします。

### ステップ 4: クラスター 2 および 3 に新規ピアを作成し、チャネルに参加させる
{: #ibp-console-hadr-create-new-peers}

1. クラスター 2 および 3 で、クラスター 1 でピア ID を登録したときと同じ手順に従い、CA を使用して新規ピア・ユーザーを登録します。 ピア・ユーザーの作成のみが必要です。組織管理者 ID は、次のステップでインポートするため、再作成する必要はありません。
2. ピアの CA としてクラスター 1 からインポートした CA を使用し、ピア組織の MSP 定義としてクラスター 1 からインポートしたピア組織の MSP を使用して、新規ピアを作成します。
3. クラスター 2 および 3 で、実行した手順を繰り返して、ピアをクラスター 1 のピアと同じチャネルに参加させることができるようになりました。
4. これらの冗長ピアにスマート・コントラクトをインストールすると、台帳がすべてのピア間で自動的に同期されます。
5. この場合も、サービス・ディスカバリー、プライベート・データ、およびピア・ゴシップを使用する予定の場合は、各冗長ピアをアンカー・ピアにする必要があります。  

これで、単一の地域での障害がピアのワークロードに影響しないようにネットワークが構成されました。  

さらに HA を最大化するには、以下の追加オプションを検討してください。
- クラスター 2 および 3 で作成したピアをエクスポートし、それらをクラスター 1 のコンソールにインポートします。 これにより、すべてを単一のクラスターから管理できます。
- ただし、クラスター 1 で障害が発生する可能性があります。 したがって、クラスターの障害をさらに考慮するために、すべてのピアを 3 つのコンソールのそれぞれにインポートできます。 その後、1 つのコンソールを含むクラスターで障害が発生した場合でも、他の 2 つのクラスターのコンソールから引き続きすべてを管理できます。
