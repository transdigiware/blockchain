---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# コンソールの管理
{: #console-icp-manage}

コンソールを {{site.data.keyword.cloud}} Private にデプロイしたら、コンソールを使用してコンソール・ユーザーを追加/削除したり、ネットワークの操作やコンソールの管理を行う API を利用したりできます。 コンソールのログにアクセスしてカスタマイズすることもできます。
{:shortdesc}

## コンソールからのユーザーの管理
{: #console-icp-manage-users}

{{site.data.keyword.blockchainfull_notm}} Platform コンソールをプロビジョンしたユーザーは、コンソール管理者と見なされます。 その後、その管理者が、コンソールの**「ユーザー (Users)」**タブを使用して、他のユーザーを追加し、コンソールに対するアクセス権限を付与することができます。 コンソールにアクセスするすべてのユーザーに、ユーザー役割を定義したアクセス・ポリシーを割り当てる必要があります。 ユーザーがコンソール内でどのような操作を行えるかは、ポリシーによって決まります。 デフォルトでは、コンソール管理者にはコンソールの**管理者**役割が与えられます。 他のユーザーには、コンソール管理者がコンソールに追加するときに、**管理者**、**ライター**、または**リーダー**の役割を割り当てることができます。 [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis) を使用してユーザーを管理することもできます。

| 役割 | 権限 |
|--------|----------|
| 管理者 | 管理者には、ライター役割を超える許可が与えられます。 リーダーとライターが行えるすべての操作に加えて、以下の操作を行えます。 <ul><li>コンソールまたは API を使用して、新規コンポーネントをプロビジョンする。</li><li>コンソールまたは API を使用して、プロビジョン済みコンポーネントを削除する。</li><li>ユーザーを追加/削除し、ユーザー・アクセス・ポリシーを変更する。</li><li>コンソールまたは API を使用して、コンソールのロギング・レベルを変更する。</li><li>API を使用して、コンソールを再始動する。</li></ul> |
| ライター | ライターには、リーダー役割を超える許可が与えられます。例えば、以下の操作を行えます。 <ul><li>コンソールまたは API を使用して、コンポーネントをインポートする。</li><li>コンソールまたは API を使用して、インポート済みコンポーネントを削除する。</li><li>ユーザーを CA に登録する。</li><li> コンソールまたは API を使用して、通知を追加または削除する。</li></ul>  |
| リーダー | リーダーは、読み取り専用の操作を行えます。例えば、以下の操作を行えます。 <ul><li>コンソール UI を表示する。</li><li>コンソール・ログを表示する。</li><li>コンポーネントをエクスポートする。</li><li>GET API を発行する。</li></ul> | |

許可は累積型です。 **管理者**役割を選択した場合、そのユーザーは**ライター**と**リーダー**の操作もすべて実行できるようになるので、それらの役割にまでチェック・マークを付ける必要はありません。 同様に、**ライター**役割のユーザーは、**リーダー**役割の操作をすべて実行できるようになります。

### ユーザーのパスワードの管理
{: #console-icp-manage-user-pw}

[パスワード・シークレット](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)内に保管された後に、構成時にコンソールに渡されたパスワードが、コンソールのデフォルト・パスワードになります。 どのユーザーも、初めてコンソールにログインするときはこのパスワードを使用する必要があります。 コンソール管理者は、新しいデフォルト・パスワードを指定することもできます。 コンソールの**「ユーザー (Users)」**タブで、認証サービス・タイル名の下にある**「構成の更新 (Update configuration)」**リンクをクリックします。 右側のパネルで、コンソールの新規ユーザーのデフォルト・パスワードを表示したり更新したりできます。

ユーザーがコンソールにログインできるように、デフォルトのパスワード、または再設定したデフォルトのパスワードをユーザーに伝える必要があります。 ユーザーは初回ログイン時にパスワードを変更する必要があります。
{:note}

管理者役割を持つユーザーは、他のユーザーのパスワードを再設定できます。 コンソールの**「ユーザー (Users)」**タブで、特定のユーザーの行の末尾にある縦 3 点をクリックし、次に**「パスワードのリセット」**をクリックします。 コンソールがそのユーザーのパスワードをデフォルトのパスワードに再設定するので、その値を右側のパネルで確認し、確定できます。 役割にかかわらず、ユーザーは自分のパスワードをいつでも変更できます。 右上隅にあるアバター・アイコンをクリックし、次に**「パスワードの変更」**をクリックします。

コンソールに追加された新規ユーザーは、通常、他のユーザーがデプロイしたノード、チャネル、チェーンコードをすべて表示することはできません。 これらのコンポーネントを操作するためには、各ユーザーが自分のコンソール・ウォレットに関連 ID をインポートする必要があります。 詳しくは、[アイデンティティーのコンソール・ウォレットへの保管](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet)を参照してください。
{:important}

### ユーザーの役割の変更
{: #console-icp-manage-reset-user-pw}

管理者役割を持つユーザーは、他のコンソール・ユーザーの役割を更新して、アクセスできるコンソールの機能を増やしたり減らしたりできます。 コンソールの**「ユーザー」**タブで、特定のユーザーの行の末尾にある縦 3 点をクリックし、次に**「認証済みユーザーの更新 (Update authenticated user)」**をクリックします。 右側のパネルで、ユーザーの新しい役割を選択します。 ユーザーの役割が更新されるのは、そのユーザーが次回コンソールにログインしたときです。

### コンソールからのユーザーの削除
{: #console-icp-manage-icp-remove-user}

管理者役割を持つユーザーは、コンソールに対するユーザーのアクセス権限を削除できます。 コンソールの**「ユーザー」**タブで、特定のユーザーの行の先頭にあるチェック・ボックスをクリックします。 次に、表の上部の**「新規ユーザーの追加」**ボタンの横にある**ごみ箱**アイコンをクリックします。 表から削除したユーザーはコンソールに再びログインできなくなります。

## {{site.data.keyword.blockchainfull_notm}} API の使用
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform は、ブロックチェーン・ノードのインポート、編集、コンソールからの削除を行う RESTful API を公開しています。 また、API を使用して、コンソールの設定、ロギング、通知を管理することもできます。 {{site.data.keyword.cloud_notm}} Private にデプロイしたコンソールが提供する API を使用するには、以下の手順を使用します。

### 前提条件
{: #console-icp-manage-prereqs}

API を使用するには、以下の情報を集める必要があります。

- コンソールの **API エンドポイント**。 これは、ブラウザーを使用してコンソールにアクセスするために使用する URL です。 この URL は、Helm リリースの概要画面の**「注: (Notes:)」**セクションに表示されます。
- コンソールへのアクセスに使用できるユーザー名とパスワード。 API キーを作成するには、[管理者役割](#console-icp-manage-users)を持つアカウントが必要です。

### API キーの使用
{: #console-icp-manage-create-api-key}

ID とアクセス管理は、コンソールごとに独自に行われます。 コンソールのユーザー名とパスワードを使用して、API 呼び出しを許可できる API キーとシークレットを生成できます。

各 API キーには、ユーザーに実行を許可する機能を管理するための役割が関連付けられます。 API キーは、ユーザー名とパスワードを使用してコンソールにログインするユーザーのために存在するアクセス・ポリシー役割と同じアクセス・ポリシー役割を使用します。 各役割で実行を許可されるアクションのリストについては、[ユーザーの管理](#console-icp-manage-users)のセクションの表を参照してください。 API キーを作成できるのは管理者の役割に限られるので、コンソールの管理者が、リーダー・ユーザーとライター・ユーザーのために API キーを作成する必要があります。 API キーに有効期限はありません。 そのため、使用されていない API キーは、コンソール管理者が削除する必要があります。

API キーを管理するために、以下の 3 つの API を使用できます。
- [API キーの作成](#console-icp-manage-create-api-key)
- [API キーの表示](#console-icp-manage-view-api-keys)
- [API キーの削除](#console-icp-manage-delete-api-keys)

API キーとシークレットを作成するには、以下の要求を使用します。 その後、そのキーとシークレットを使用して、他の API を使用できます。 コンソールは API シークレットを保管しないので、ユーザーが保存する必要があります。

#### API キーの作成
{: #console-icp-manage-create-api-key-api}

| **要求** |  |
|-------------|-----------|
| パス | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **要求本文フィールド** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 少なくとも 1 つの値が必要です </li><li>`string` オプション</li></ul>|
| **応答本文フィールド** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` 保存: キーは保管されません</li><li>`["<role>"]`</li></ul>|
| 必要な許可 | 管理者 |

#### curl 要求の例: API キーの作成
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### コンソール API キーの管理
{: #console-icp-manage-api-keys}

API キーとシークレットを作成したら、API を使用して、作成したキーを表示したり削除したりできます。 キーとシークレットは、削除されない限り有効です。

#### API キーの表示
{: #console-icp-manage-view-api-keys}

| **要求** |  |
|-------------|-----------|
| パス | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **応答本文フィールド** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` UNIX タイム・スタンプ (ミリ秒)</li><li>`string`</li></ul>|
| 必要な許可 | リーダー |

#### curl 要求の例: API キーの表示
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### API キーの削除
{: #console-icp-manage-delete-api-keys}

| **要求** |  |
|-------------|-----------|
| パス | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| 必要な許可| 管理者 |

#### curl 要求の例: API キーの削除
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### API を使用したユーザーの管理
{: #console-icp-manage-users-apis}


API を使用して、コンソールにログインできるユーザーやコンソール API にアクセスできるユーザーをリスト表示、追加、または削除することもできます。 ユーザーの役割を編集して、ユーザーのアクセス・レベルをアップグレードすることもできます。 以下の API が用意されています。

- [ユーザーのリスト表示](#console-icp-manage-list-users-api)
- [ユーザーの編集](#console-icp-manage-edit-users-api)
- [ユーザーの追加](#console-icp-manage-add-users-api)
- [ユーザーの削除](#console-icp-manage-remove-users-api)

コンソールのユーザーと設定を管理し、ネットワークのノードを管理する API の場合、TLS 証明書の要件をバイパスするために ``-K`` フラグまたは ``--insecure`` フラグを追加する必要があります。 ブラウザーを使用して、コンソールから TLS 証明書をダウンロードすることもできます。

#### ユーザーのリスト表示
{: #console-icp-manage-list-users-api}


| **要求** |  |
|-------------|-----------|
| パス | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **応答本文フィールド** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` ユーザー ID</li><li>`string` E メール・アドレス</li><li>`["<role>"]`</li><li>`number` UNIX タイム・スタンプ (ミリ秒)</li></ul>|
| 必要な許可 | リーダー |

#### curl 要求の例: ユーザーのリスト表示
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### ユーザーの編集
{: #console-icp-manage-edit-users-api}

| **要求** |  |
|-------------|-----------|
| パス | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **要求本文フィールド** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` ユーザー ID </li><li>`["reader", "writer", "manager"]` 少なくとも 1 つの値が必要です</li></ul> |
| **応答本文フィールド** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` ユーザー ID</li></ul>|
| 許可| 管理者 |

#### curl 要求の例: ユーザーの編集
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### ユーザーの追加
{: #console-icp-manage-add-users-api}

| **要求** |  |
|-------------|-----------|
| パス | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **要求本文フィールド** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 少なくとも 1 つの値が必要です </li><li>`string` オプション</li></ul>|
| 許可 | 管理者 |

#### curl 要求の例: ユーザーの追加
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### ユーザーの削除
{: #console-icp-manage-remove-users-api}

| **要求** |  |
|-------------|-----------|
| パス | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **要求本文フィールド** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` ユーザー ID</li></ul>|
| **応答本文フィールド** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` ユーザー ID</li></ul>|
| 許可 | 管理者 |

#### curl 要求の例: ユーザーの削除
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### API を使用したコンポーネントの管理

使用可能な API の完全なセットは、[{{site.data.keyword.blockchainfull_notm}} Platform API リファレンス](https://test.cloud.ibm.com/apidocs/blockchain)で参照できます。

これらの API では {{site.data.keyword.cloud_notm}} Private 上のコンソールと通信するので、コンソールによる認証を受け、{{site.data.keyword.cloud_notm}} による許可を受ける必要があります。 API リファレンスの ``Bearer Auth`` を ``-u <api_key>:<api_secret>`` に置き換えてください。 コマンドに ``-K`` フラグまたは ``--insecure`` フラグを追加するか、ブラウザーを使用してコンソールの TLS 証明書をダウンロードする必要もあります。 **「Try it out」**タブを使用して、これらの API を {{site.data.keyword.cloud_notm}} Private 上のネットワークでテストすることはできません。

例えば、以下の API 呼び出しは、{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} のサービス・インスタンスで実行されているすべてのコンポーネントに関する情報を返します。
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
{{site.data.keyword.cloud_notm}} Private にデプロイされたコンソールの場合、同じ API 呼び出しは以下の要求のようになります。
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

API を使用して、コンソールをデプロイしたクラスター上にノードを作成し、他のクラスターまたは {{site.data.keyword.cloud_notm}} からノードをインポートすることができます。 詳しくは、[API を使用したネットワークの構築](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis)、および [API を使用したネットワークのインポート](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis)を参照してください。

## ログの表示
{: #icp-console-manage-logs}

{{site.data.keyword.blockchainfull_notm}} Platform コンソールを使用していると、ログを表示して問題をデバッグする必要がある場合があります。

### コンソール・ログの表示
{: #console-icp-manage-console-logs}

コンソールの使用時やノードの操作時に発生した問題のデバッグが必要な場合、コンソール・ログに簡単にアクセスできます。 ロギング・レベルを設定して、コンソールが収集するログの量を増減することもできます。 コンソール・ログは、[ノード・ログ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs) ({{site.data.keyword.cloud_notm}} Private. によって収集されます) とは別に収集されます。

コンソール・ブラウザーの**「設定」**タブにナビゲートして、ロギング設定を変更します。 コンソール・ログは、2 つの別個のソースから収集されます。

  * **クライアント・ロギング:** これらのログは、ブラウザーからコンソールにコマンドが送信されたときに収集されます。
  * **サーバー・ロギング:** これらのログは、コンソールからノードにコマンドが送信されたとき、およびコンソールのデプロイメント時に収集されます。 これらのログには、Hyperledger Fabric のロギング出力が含まれます。

各ログ・タイプの下のドロップダウン・リストを使用して、収集されるログの量を設定します。 例えば、**「エラー」**および**「警告」**では最少量のログが収集され、**「デバッグ」**および**「すべて」**では最大量が収集されます。

コンソール・ログは、コンソール管理者としてログインしている場合にのみ表示できます。 **「設定」**タブからログを表示するには、ブラウザー URL の語 `settings` を `api/v1/logs` に置き換えます。 例えば、コンソール URL が `localhost:3001/settings` の場合、`localhost:3001/api/v1/logs` にナビゲートすることによってログを表示できます。 クライアント・ログとサーバー・ログは別個のファイルに収集されます。 最新のログがページの先頭に表示されます。

### ノード・ログの表示
{: #console-icp-manage-node-logs}

コンポーネント・ログは、コマンド・ラインから [kubectl CLI コマンド](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}を使用して表示することも、[Kibana](https://www.elastic.co/products/kibana){: external} ({{site.data.keyword.cloud_notm}} Private クラスターに含まれています) を使用して表示することもできます。

- `kubectl logs` コマンドを使用して、ポッド内のコンテナー・ログを表示します。 手順に従って、[kubectl cli をインストール](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}します (まだインストールしていない場合)。 ポッド名が不明な場合は、以下のコマンドを実行してポッドのリストを表示します。

  ```
  kubectl get pods
  ```
  {:codeblock}

  次に、以下のコマンドを実行して、ポッド内にあるノード・コンテナーのログを取得します。

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  `<pod_name>` を、前のコマンド出力のポッド名に置き換えてください。  
  ノードのログを表示するには、`<node>` を `ca`、`peer`、または `orderer` に置き換えます。  
  スマート・コントラクトのログを表示するには、`<node>` を `fluentd` に置き換えます。

  `kubectl logs` コマンドについて詳しくは、[Kubernetes の資料](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}を参照してください。

- あるいは、{{site.data.keyword.cloud_notm}} Private コンソールから[ログにアクセスする](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external}こともできます。この場合は Kibana でログが開きます。

  **注:** Kibana でログを表示する場合、`「No results found」`という応答を受け取ることがあります。 この状態は、{{site.data.keyword.cloud_notm}} Private がワーカー・ノードの IP アドレスをそのホスト名として使用する場合に起こることがあります。 この問題を解決するには、パネルの上部で `node.hostname.keyword` で始まるフィルターを削除すると、ログが表示されるようになります。

### スマート・コントラクトのコンテナー・ログの表示
{: #console-icp-manage-container-logs}

スマート・コントラクトに関する問題が発生した場合は、スマート・コントラクト (チェーンコード) のコンテナー・ログを表示して問題をデバッグできます。 以下のコマンドを実行して、スマート・コントラクト・コンテナーのログを表示できます。

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

`<peer_pod>` を、チェーンコードが実行されているピアのポッドの名前に置き換えます。 コマンド `kubectl get po` を使用して、実行中のポッドのリストを取得します。

## ノードのパッチのインストール
{: #console-icp-manage-patch}

ピア・ノード、CA ノード、および順序付けプログラム・ノードの基盤となる {{site.data.keyword.IBM_notm}} Hyperledger Fabric Docker イメージは、セキュリティー更新や新しい Fabric ポイント・リリースへの更新などのために、時間の経過とともに更新する必要が生じることがあります。 {{site.data.keyword.blockchainfull_notm}} Platform の Helm チャートをアップグレードすると、Fabric イメージを更新できます。

Helm チャートをアップグレードした後で、コンポーネントの更新が使用可能な場合は、ノード・タイルに**「使用可能なパッチ (Patch available)」**というテキストが表示されます。 準備ができたら、このパッチをノードにインストールできます。 これらのパッチはオプションですが、推奨されます。 コンソールにインポートされたノードにパッチを適用することはできません。

パッチは、一度に 1 つのノードに適用されます。 パッチが適用されている間、そのノードでは要求やトランザクションを処理できません。 そのため、サービスの中断を防ぐために、可能な場合は常に、同じタイプの別のノードが要求を処理できることを確認する必要があります。 ノードへのパッチのインストールが完了するまでには約 1 分かかり、更新が完了すると、ノードで要求を処理できるようになります。
{:note}

ノードにパッチを適用するには、ノード・タイルを開き、**「パッチのインストール (Install patch)」**ボタンをクリックします。 コンソールにインポートしたノードにパッチを適用することはできません。
