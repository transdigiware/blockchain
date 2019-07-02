---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# ノードのインポート
{: #ibp-console-import-nodes}

コンソールには、別の {{site.data.keyword.blockchainfull}} Platform コンソールを使用して作成されたノードをインポートするためのオプションが用意されています。
{:shortdesc}  

**対象者:** このトピックは、ブロックチェーン・ネットワークの作成、モニター、管理を担当するネットワーク・オペレーターを対象に設計されています。

## ノードをインポートする理由
{: #ibp-console-import-nodes-why}

コンソール内に既に存在しているノードとともに、CA、順序付けサービス、およびピアを操作する必要がある場合は、それらを他の {{site.data.keyword.blockchainfull_notm}} Platform コンソールからインポートします。 例えば、コンソール外の組織からピアをコンソール内のチャネルに参加させたい場合は、ピアのインポート・オプションを使用できます。 {{site.data.keyword.blockchainfull_notm}} Platform サービスは複数の環境にまたがってデプロイされるため、一部のサービス・インスタンスには CA とピアのみが含まれている一方で、他のサービス・インスタンスでは順序付けサービスがホストされている可能性があります。 さまざまなノードをコンソールにインポートすることで、これらの分散されたコンポーネントを単一のユーザー・インターフェースから表示および操作することが可能になるため、これらのコンポーネントはブロックチェーン上で一緒にトランザクションを実行できるようになります。

コンソールにノードをインポートすると、次のような一連の堅固な操作機能が使用可能になります。
- 新しい組織を共同事業体に追加する
- 新しいチャネルを作成する
- ピアをチャネルに参加させる
- ピアのデプロイ場所に関係なく、スマート・コントラクトをピアにインストールする
- チャネル上のスマート・コントラクトをインスタンス化する
- チャネル上のスマート・コントラクトをアップグレードする

ノードをインポートする際は、その接続情報を提供する必要があります。 コンポーネントをインポートしても、その物理コンポーネント自体が実際にインポートされるわけではありません。代わりに、接続情報に基づいてそのコンポーネントの表現がコンソール内に構築され、コンポーネントをコンソールから操作できるようになります。 同様に、インポートしたノードをコンソールから削除した場合も、デプロイされた場所でまだ実行されているノード自体が削除されるわけではありません。 インポートしたコンソールから削除されるだけです。  

ノードをコンソールにインポートした後に、ノードの**「設定」**タブでその接続情報を変更することもできます。

ノードをコンソールにインポートする前に、それらのノードが作成された {{site.data.keyword.blockchainfull_notm}} Platform コンソールからそれらのノードをエクスポートする必要があります。 ネットワーク・オペレーターが、コンソールのノードの情報を `JSON` ファイルにエクスポートするだけです。
{: note}

## 制限
{: #ibp-console-import-limitations}

- スターター・プラン・ネットワークおよびエンタープライズ・プラン・ネットワークからノードをインポートすることはできません。
- インポートするノードはすべて、{{site.data.keyword.blockchainfull_notm}} Platform コンソールを使用してデプロイされたものでなければなりません。
- コンソールにインポートしたノードにパッチを適用することはできません。
- コンソールにインポートしたノードを、それらがデプロイされたクラスターから削除することはできません。ノードは、コンソールからのみ削除できます。
- {{site.data.keyword.cloud_notm}} Private にデプロイされたノードをインポートする場合は、コンポーネントが使用する gRPC Web プロキシー・ポートが、コンソールに外部から公開されるようにする必要があります。詳しくは、[{{site.data.keyword.cloud_notm}} Private からのノードのインポート](#ibp-console-import-icp)を参照してください。

## 開始: 証明書または資格情報の収集
{: #ibp-console-import-start-here}

別の {{site.data.keyword.blockchainfull_notm}} Platform サービス・インスタンスから既存のブロックチェーン・コンポーネントをインポートする前に、そのコンポーネントの管理者アイデンティティーをコンソール・ウォレットにインポートする必要があります。 アイデンティティーをコンソール・ウォレットにインポートすると、ノードの操作が簡素化されます。  ノードがデプロイされている場所のネットワーク・オペレーターが、[インポート](#ibp-console-import-nodes-admin-identities)可能な JSON ファイルにウォレットのノード管理者 ID をエクスポートする必要があります。  通常、ノード管理者 ID は、組織 MSP 定義の作成時に指定されたピア組織または順序付けプログラム組織の管理者です。 これは、ノードが存在するコンソールのウォレットに既に入っているはずです。

### コンソール・ウォレットへの管理者 ID のインポート
{: #ibp-console-import-nodes-admin-identities}

それぞれの {{site.data.keyword.blockchainfull_notm}} Platform コンポーネントは、その内部のコンポーネント管理者の独自の署名証明書 (署名付き証明書) とともにデプロイされます。 管理者がそのコンポーネントに対してアクションを実行すると、この署名証明書がトランザクションに添付されて、コンポーネントの構成と照らし合わせて検証されます。 鍵により、管理者は、新規ユーザーの登録、新規チャネルの作成、ピアへのスマート・コントラクトのインストールなどによって、コンポーネントを操作することが可能になります。 コンソールを使用して、別のコンソールを使用して作成された順序付けサービスまたはピアを操作することを希望する場合は、関連する管理者アイデンティティーをコンソール・ウォレットにアップロードする必要があります。 その後、インポートしたノードをコンソール・ウォレット内のアイデンティティーに関連付けることができます。 アイデンティティーは、これらが作成されたコンソールからエクスポートされる必要があり、ノードをインポートする際に必要になります。

新しいアイデンティティーをインポートするには、**「ウォレット (Wallet)」**タブを開いて、**「アイデンティティーの追加 (Add identity)」**をクリックします。 **「JSON のアップロード (Upload JSON)」**をクリックして、ノードが作成された場所からネットワーク・オペレーターによってエクスポートされた JSON アイデンティティー・ファイルを参照します。

**「アイデンティティーの追加 (Add identity)」**パネルを完了して「送信」をクリックした後に、ウォレットの概要画面で新しい管理者アイデンティティーを表示できます。 これで、CA、ピア、または順序付けサービスのコンポーネントのインポート時に、これらのアイデンティティーを参照できるようになりました。

## CA のインポート
{: #ibp-console-import-ca}

CA ノードは、すべてのネットワーク・エンティティー (ピア、順序付けサービス、クライアントなど) に証明書を発行するブロックチェーン・コンポーネントです。その結果として、これらのエンティティーは通信および認証できるようになり、最終的にトランザクションを実行できます。 各組織には、そのトラスト・ルートとして機能する独自の CA があります。 ブロックチェーン共同事業体への参加や構築時のいずれであっても、自身の組織を追加する必要があります。 CA について詳しくは、[ブロックチェーン・コンポーネントの概要](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca)を参照してください。  

CA をコンソールにインポートする理由はいくつかあります。 CA をインポートすると、**「ユーザーの登録」**をクリックし、その CA を使用して追加のピア・ユーザーをピア組織に登録できます。 または、その CA を使用して、ピアまたは順序付けサービス用の追加の管理者アイデンティティーを作成できます。

CA を {{site.data.keyword.blockchainfull_notm}} Platform コンソールにインポートして操作するには、ネットワーク・オペレーターが、CA がデプロイされている {{site.data.keyword.blockchainfull_notm}} Platform から事前に CA をエクスポートしておく必要があります。 CA をインポートすると、新規ユーザーの登録と [アイデンティティーのエンロール](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)が可能になります。

### 始めに
{: #ibp-console-import-ca-before-you-begin}

- CA の管理者 ID の JSON ファイルをコンソール・ウォレットに既にインポートしたこと、または、CA が最初にデプロイされたときに指定された CA と TLS CA の両方の管理者の登録 ID とシークレットがあることを確認します。
- CA が作成されたコンソールからエクスポートされた CA の JSON ファイルが使用可能であることを確認します。

### CA のインポート方法  
{: #ibp-console-import-nodes-howto-ca}

CA のインポートは、**「ノード」**タブから実行します。
1. **「認証局の追加 (Add Certificate Authority)」**、**「既存の認証局のインポート (Import an existing Certificate Authority)」**の順にクリックし、**「次へ」**をクリックします。
2. **「認証局の場所 (Certificate Authority location)」**ドロップダウン・リストから、CA が最初にデプロイされた場所を選択します。
3. **「ファイルの追加 (Add file)」**をクリックして、CA が最初にデプロイされたコンソールからエクスポートされた JSON ファイルをアップロードします。
4. 次のパネルで、CA のデプロイ時に使用した登録 ID とシークレットを入力できます。
5. 最後に、CA のデプロイ時に使用した TLS CA の登録 ID とシークレットを入力します。 CA をデプロイすると、CA と TLS CA の両方が一緒にデプロイされます。 ネットワーク・オペレーターは、両方に同じ登録 ID とシークレットを使用することも、CA をデプロイするときに CA と TLS CA に固有の登録 ID とシークレットを指定することもできます。

CA をコンソールにインポートしたら、その CA を使用して、新しいアイデンティティーを作成したり、コンポーネントを操作するために必要な証明書を生成したり、トランザクションをネットワークに送信したりできます。 詳しくは、[認証局の管理](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca)を参照してください。

## 順序付けサービスのインポート
{: #ibp-console-import-orderer}

順序付けサービスは、ネットワーク・メンバーからトランザクションを収集し、これらのトランザクションを順序付けてブロック単位にバンドルするブロックチェーン・コンポーネントです。 これは、ブロックチェーン・コンソーシアムの共通バインディングであり、他の組織が参加するコンソーシアムを設立する場合はデプロイする必要があります。 順序付けサービスについて詳しくは、[ブロックチェーン・コンポーネントの概要](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer)を参照してください。

順序付けサービスをコンソールにインポートすると、ピアがプライベートにトランザクションを実行するための新しいチャネルを作成できます。

### 始めに
{: #ibp-console-import-orderer-before-you-begin}

順序付けサービスをインポートする前に、以下の情報を収集する必要があります。

- [順序付けサービスの管理者アイデンティティーの JSON ファイルをコンソール・ウォレットに既にインポート](#ibp-console-import-nodes-admin-identities)していることを確認します。
- 順序付けサービスが作成されたコンソールからエクスポートされた順序付けサービスの JSON ファイルが使用可能であることを確認します。

### 順序付けサービスのインポート方法
順序付けサービスのインポートは、**「ノード」**タブから実行します。
1. **「順序付けサービスの追加 (Add ordering service)」**、**「既存の順序付けサービスのインポート (Import an existing ordering service)」**の順にクリックし、**「次へ」**をクリックします。
2. **「認証局の場所 (Certificate Authority location)」**ドロップダウン・リストから、順序付けサービスが最初にデプロイされた場所を選択します。
3. **「ファイルの追加 (Add file)」**をクリックして、順序付けサービスが最初にデプロイされたコンソールからエクスポートされた JSON ファイルをアップロードします。 これが 5 ノードの Raft 順序付けサービスである場合は、5 つすべての順序付けノードの接続情報が単一のファイルに入っていなければなりません。
4. **「既存の ID (Existing identity)」**をクリックし、コンソール・ウォレットにインポートした順序付けサービスの管理者 ID を選択して順序付けサービスの管理者 ID を設定します。

順序付けサービスをコンソールにインポートした後に、新しい組織メンバーを追加して、新しいチャネルの作成時にその順序付けサービスを選択できます。

## ピアのインポート
{: #ibp-console-import-peer}

ピア・ノードは、台帳を保守して、スマート・コントラクトを実行することで、台帳に対する照会操作と更新操作を実行するブロックチェーン・コンポーネントです。 組織メンバーはピアを所有および保守します。  コンソーシアムに参加する各組織はピアを 1 つ以上デプロイする必要があります。高可用性 (HA) のためには 2 つ以上をデプロイする必要があります。 ピアについて詳しくは、[ブロックチェーン・コンポーネントの概要](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer)を参照してください。

ピアをコンソールにインポートした後に、スマート・コントラクトをピアにインストールして、ブロックチェーン内の他のチャネルにピアを参加させることができます。

**注:** ピア組織にさらにピアを追加する必要がある場合や、ピアの追加の管理者アイデンティティーを作成する必要がある場合は、ピアの CA をインポートしてから、その CA を使用してこれらの操作を実行する必要があります。

### 始めに
{: #ibp-console-import-peer-before-you-begin}

ピアをインポートする前に、以下の情報を取得する必要があります。

- [ピアの管理者アイデンティティーの JSON ファイルをコンソール・ウォレットに既にインポート](#ibp-console-import-nodes-admin-identities)していることを確認します。
- ピアが作成されたコンソールからエクスポートされたピアの JSON ファイルが使用可能であることを確認します。

### ピアのインポート方法
{: #ibp-console-import-peer-howto}

ピアのインポートは、**「ノード」**タブから実行します。
1. **「ピアの追加 (Add Peer)」**、**「既存のピアのインポート (Import an existing peer)」**の順にクリックした後に、**「次へ」**をクリックします。
2. **「認証局の場所 (Certificate Authority location)」**ドロップダウン・リストから、ピアが最初にデプロイされた場所を選択します。
3. **「ファイルの追加 (Add file)」**をクリックして、ピアが最初にデプロイされたコンソールからエクスポートされた JSON ファイルをアップロードします。
4. **「既存の ID (Existing identity)」**をクリックし、コンソール・ウォレットにインポートしたピアの管理者 ID を選択してピアの管理者 ID を設定します。  

ピアをコンソールにインポートした後に、スマート・コントラクトをピアにインストールして、ブロックチェーン内のチャネルにピアを参加させることができます。

## 組織 MSP 定義のインポート
{: #ibp-console-import-msp}

別の {{site.data.keyword.blockchainfull_notm}} Platform サービス・インスタンス・コンソールを使用してデプロイされた組織 MSP 定義を使用するチャネルを作成または更新する場合は、その MSP 定義をインポートする必要があります。 また、別のコンソールでデプロイされた組織 MSP に含まれているピア・サービスまたは順序付けサービスを作成する場合も、その MSP および関連する MSP 管理者 ID をインポートする必要があります。  MSP を作成したネットワーク・オペレーターから、組織 MSP 定義および対応する管理者 ID を JSON ファイルにエクスポートしたものを提供してもらう必要があります。

- 組織に含まれているピア・サービスまたは順序付けサービスを作成する場合は、関連する MSP 管理者 ID をウォレットにインポートしてください。
- 組織 MSP 定義のインポートは、**「組織」**タブから実行します。
- **「MSP 定義のインポート (Import MSP Definition)」**をクリックして、JSON ファイルをアップロードします。
- (オプション) MSP 管理者 ID をウォレットにインポートした場合は、`「MSP 定義の管理者 ID がある (I have an administrator identity for the MSP definition)」`チェック・ボックスにチェック・マークを付けます。 このオプションを選択しない場合、後でピア・サービスまたは順序付けサービスを作成するときに、この組織 MSP 定義は MSP ドロップダウン・リストにリストされません。

## {{site.data.keyword.cloud_notm}} Private からのノードのインポート
{: #ibp-console-import-icp}

{{site.data.keyword.cloud_notm}} Private で作成されたノードを、他の {{site.data.keyword.cloud_notm}} Private クラスターまたは {{site.data.keyword.cloud_notm}} にデプロイされたコンソールにインポートできます。ただし、ノードの gRPC URL で使用されるポートが、クラスターの外部から公開されるようにする必要があります。{{site.data.keyword.cloud_notm}} Private をファイアウォールの内側にデプロイする場合は、ホワイト・リストなどを使用してパススルーを有効にし、クラスター外部のコンソールがノードと通信できるようにする必要があります。

例えば、以下の {{site.data.keyword.cloud_notm}} Private からエクスポートされたピアの JSON ファイルを見つけることができます。別のコンソールからピアと通信するには、`grpcwp_url` ポート (この例ではポート 32403) が外部トラフィックに対して開いた状態にする必要があります。

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
