---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# リリース情報
{: #release-notes-saas-20}

ここでは、Hyperledger Fabric v1.4.1 を基盤にした {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} の最新の変更内容を確認できるように、リリース情報を日付ごとにまとめています。
{:shortdesc}

## 2019 年 7 月 10 日
{: #07-10-2019}

**順序付けノード・パッチ `1.4.1-2`**  

このパッチは、順序付けノードの再開時に発生する問題を解決します。順序付けサービスの順序付けノード間の通信を妨害するジェネシス・ブロックが更新されます。このパッチを順序付けサービスの既存のすべての順序付けノードに適用するには、これらの[説明](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch)に従って各順序付けノードを更新してください。新たに作成するすべての順序付けノードには、自動的にこの修正が含まれます。

**アンカー・ピアの削除**  

チャネルからアンカー・ピアを削除する機能が追加されました。

**各種のバグ修正**  

バグ修正と翻訳の更新。

## 2019 年 5 月 24 日
{: #05-24-2019}

**Raft コンセンサス・プロトコル**  

実動ネットワークに推奨される 5 ノード構成の Raft 順序付けサービスが使用可能になりました。 さらに、開発およびテストの目的には、単一ノードの Raft 順序付けサービスをデプロイできます。

## 2019 年 5 月 9 日
{: #05-09-2019}

**{{site.data.keyword.blockchainfull_notm}} Platform コンソール API の導入**

ピア、順序付けプログラム、および CA のノードのプロビジョン、編集、および削除に API を使用できるようになったため、ブロックチェーン・ネットワークの構築をスクリプト化することができます。 API の詳細を確認して試行するには、[{{site.data.keyword.cloud_notm}} API 資料のリポジトリー](/apidocs/blockchain#introduction){: external} の資料を使用します。 また、API を使用してネットワークを構築する手順については、[API を使用したネットワークの構築](/docs/services/blockchain?topic=blockchain-ibp-v2-apis)のトピックを参照してください。  

**チャネル・ガバナンス**  

チャネル・ガバナンスの更新により、ポリシーを再構成できます。 チャネルの更新に署名する必要があるチャネル・メンバーを制御して、署名の収集を調整することもできます。

## 2019 年 4 月 17 日
{: #04-17-2019}

**ノード・リソースのサイズとスケールが指定可能に**  

ノードをデプロイする時に、必要に応じて、コンテナーの CPU、メモリー、ストレージの量を指定できるようになりました。 使用状況のパターンに基づいて、後からリソースをスケールアップ/スケールダウンすることもできます。 詳しくは、[リソースの割り振り](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources)を参照してください。

**{{site.data.keyword.cloud_notm}} ID およびアクセス管理 (IAM) の使用**  

コンソール UI へのユーザー・アクセスの制御、および UI で実行可能なユーザー・アクションの制限を行うために、IAM が使用されます。  [コンソールでのユーザーの追加と削除](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove)の方法に関するトピックを参照してください。

## 2019 年 4 月 3 日
{: #04-03-2019}

**外部 CA のサポート**

ピア・ノードや順序付けプログラム・ノードを追加するときに、外部 CA ({{site.data.keyword.IBM_notm}} がホストする CA ではないもの) で作成された証明書を使用できます。 詳しくは、[外部 CA からの証明書をピアまたは順序付けプログラムに使用する](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca)のトピックを参照してください。

**順序付けプログラムのパフォーマンスの調整**

順序付けプログラムのスループットやパフォーマンスを細かく制御できるように、順序付けプログラムの新しいチューニング・パラメーターがコンソールに用意されました。 パラメーターの構成方法については、[順位付けプログラムのチューニング](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning)に関するトピックを参照してください。
