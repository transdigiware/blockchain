---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: pricing model, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の料金
{: #ibp-software-pricing}

このガイドにより、{{site.data.keyword.blockchainfull}} Platform for Multicloud の料金モデル、および Hyperledger Fabric v1.4.1 に基づくピア、順序付けサービス、および認証局のコンポーネントのブロックチェーン・ネットワークを開発して拡大するときに生じる支払い金額を理解することができます。
{:shortdesc}

_この料金モデルの対象は、{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud のみです。 スターター・プランまたはエンタープライズ・プランを使用しており、料金について質問がある場合は、スターター・プランおよびエンタープライズ・プランの[料金](/docs/services/blockchain?topic=blockchain-ibp-pricing)を参照してください。_

## 使用許諾条件
{: #ibp-software-pricing-license}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud は、ユーザー独自のインフラストラクチャーでブロックチェーン・ネットワークを実行するために必要なコンポーネントを提供します。これは、{{site.data.keyword.cloud_notm}} Private アプリケーションを使用してデプロイします。 パスポート・アドバンテージ (PPA) にアクセスして、[Helm チャートをダウンロード](/docs/services/blockchain?topic=blockchain-console-helm-install#console-helm-install-importing)できます。 {{site.data.keyword.blockchainfull_notm}} からの技術サポートが購入内容に含まれています。

## 料金
{: #ibp-software-pricing-pricing}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の料金は、使用される仮想プロセッサー・コア (VPC) の数に基づきます。 VPC または vCPU は、仮想サーバーに割り当てられた仮想コアか、非パーティション化サーバーの物理プロセッサー・コアのいずれかです。 {{site.data.keyword.blockchainfull_notm}} Platform で使用可能な VPC ごとに、ライセンス交付を受けた使用権を取得する必要があります。 必要な vCPU または CPU の数は、インフラストラクチャー、ネットワーク設計、およびパフォーマンス要件によって異なります。 

{{site.data.keyword.cloud_notm}} Private の VPC について詳しくは、{{site.data.keyword.IBM_notm}} Knowledge Center の [仮想プロセッサー・コア (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external} を参照してください。 [{{site.data.keyword.IBM_notm}} License Metric Tool](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/welcome/LMT_welcome.html){: external} を使用して、ライセンスを取得するために必要な VPC の数を判別するために使用できるレポートを構成および作成できます。

さまざまなライセンス交付オプションが使用可能です。 詳しくは[お問い合わせください](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}。
