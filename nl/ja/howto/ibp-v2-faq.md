---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

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

# FAQ
{: #ibp-v2-faq}

**一般**   

- [ネイティブの Hyperledger Fabric よりも {{site.data.keyword.blockchainfull_notm}} Platform を使用するメリットとして、どのようなものがありますか?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [どのバージョンの Hyperledger Fabric を {{site.data.keyword.blockchainfull_notm}} Platform と併用できますか?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [ピアはどのデータベースを台帳用に使用しますか?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [スマート・コントラクトの言語として、どの言語がサポートされていますか?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [IBM 以外の認証局 (CA) からの証明書の使用はサポートされますか?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [V1.0 から新しい {{site.data.keyword.blockchainfull_notm}} Platform にアップグレードできますか?](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [{{site.data.keyword.blockchainfull_notm}} Platform サービスを削除したらどうなりますか?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [{{site.data.keyword.cloud_notm}} で実行されているブロックチェーン・サービスにはどの地域を使用できますか?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [既存の {{site.data.keyword.cloud_notm}} Kubernetes Service クラスターを使用できますか?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [ロギング・サービスにはアクセスできますか? どのログが使用可能ですか?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の利点は何ですか?](#ibp-v2-faq-icp-benefits)
- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ではどのような環境がサポートされますか?](#ibp-v2-faq-icp-environments)
- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud はどのような料金モデルですか?](#ibp-v2-faq-icp-pricing)
- [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud を使用するためにインストールする必要があるサービスは何ですか?](#ibp-v2-faq-icp-services)
- [開発環境、テスト環境、および実稼働環境の {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud のサイジング要件は、どのように見積もればよいですか?](#ibp-v2-faq-icp-sizing)
- [Helm リリースを削除するとブロックチェーン・コンポーネントはどうなりますか?](#ibp-v2-faq-icp-delete)

## ネイティブの Hyperledger Fabric よりも {{site.data.keyword.blockchainfull_notm}} Platform を使用するメリットとして、どのようなものがありますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform を使用すると、お客様はカスタマイズされたブロックチェーン・ネットワークを簡単にデプロイできます。 直観的なコンソール UI を使用して、迅速にネットワークをデプロイし、容易にスマート・コントラクトをインストールおよびインスタンス化して、トランザクションをモニターできます。

## どのバージョンの Hyperledger Fabric を {{site.data.keyword.blockchainfull_notm}} Platform と併用できますか?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} および {{site.data.keyword.blockchainfull_notm}} Platform for Platform for Multicloud は Hyperledger Fabric v1.4.1 を使用します

## ピアはどのデータベースを台帳用に使用しますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform でデプロイされたすべてのピアでは、台帳のデータベースとして CouchDB が使用されます。

## スマート・コントラクトの言語として、どの言語がサポートされていますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform は、Go および Node.js で作成されたスマート・コントラクトをサポートしています。 新しい Hyperledger Fabric プログラミング・モデルは、現在は Node.js のみサポートしており、それ以外は近日サポートされます。 既存のアプリケーション・コードを保持や、Node.js 以外の言語用の Fabric SDK の使用に関心がある場合には、以前と変わらず低水準の Fabric SDK API を使用して {{site.data.keyword.blockchainfull_notm}} Platform ネットワークに接続できます。

## IBM 以外の認証局からの証明書の使用はサポートされますか?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

はい。X.509 に準拠した CA から発行されたものである限り、独自の証明書を持ち込めます。

## V1.0 から新しい {{site.data.keyword.blockchainfull_notm}} Platform にアップグレードできますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

スターター・プランではできません。 スターター・プランから新しい {{site.data.keyword.blockchainfull_notm}} Platform にアップグレードすることはできません。
エンタープライズ・プランの場合は、将来的に新しい {{site.data.keyword.blockchainfull_notm}} Platform にアップグレードできます。 アカウント所有者に、{{site.data.keyword.blockchainfull_notm}} Platform チームから、支援のための E メールが送信されます。

## {{site.data.keyword.blockchainfull_notm}} Platform サービスを削除したらどうなりますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

{{site.data.keyword.blockchainfull_notm}} プラットフォーム・サービス・インスタンスを削除すると、ブロックチェーンのすべての CA、ピア、および順序付けノードが、関連付けられたストレージとともに削除されます。

## {{site.data.keyword.cloud_notm}} で実行されているブロックチェーン・サービスにはどの地域を使用できますか?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform に使用可能な地域は、[{{site.data.keyword.blockchainfull_notm}} Platform のロケーション](/docs/services/blockchain?topic=blockchain-ibp-regions-locations)にリストされます。 クラスターが認識されるように、ブロックチェーン・サービスと同じ地域に {{site.data.keyword.cloud_notm}} Kubernetes Service クラスターを作成する必要があることに注意してください。 近日中に追加の地域が使用可能になります。

## 既存の {{site.data.keyword.cloud_notm}} Kubernetes Service クラスターを使用できますか?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

既存の Kubernetes クラスターが以下の条件を満たしている限り、そのクラスターは {{site.data.keyword.blockchainfull_notm}} Platform で使用できます。
- Kubernetes v1.11 以降の安定バージョンを実行している。
- クラスター内に使用できるリソースが十分ある。

## ロギング・サービスにはアクセスできますか? どのログが使用可能ですか?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform をご利用の場合、現在 Kubernetes ダッシュボードからピア、CA、順序付けノードのログに直接アクセスできます。 ログをリアルタイムで簡単に解析できる {{site.data.keyword.cloud_notm}} LogDNA サービスを利用することをお勧めします。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の利点は何ですか?
{: #ibp-v2-faq-icp-benefits}
{: faq}

[{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の提供内容](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers)で該当するトピックを参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ではどのような環境がサポートされますか?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud は、{{site.data.keyword.cloud_notm}} Private v3.2 がサポートするすべての環境をサポートします。 詳しくは、[サポートされるオペレーティング・システムおよびプラットフォーム](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external}を参照してください。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud はどのような料金モデルですか?
{: #ibp-v2-faq-icp-pricing}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の[ライセンス交付](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)は、製品で使用可能な CPU (VPC) の量に基づいています。 詳しくは、[IBM にお問い合わせください](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}。

## {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud を使用するためにインストールする必要があるサービスは何ですか?
{: #ibp-v2-faq-icp-services}
{: faq}

インストールする必要があるのは、{{site.data.keyword.cloud_notm}} Private v3.2 のみです。

## 開発環境、テスト環境、および実稼働環境の {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud のサイジング要件は、どのように見積もればよいですか?
{: #ibp-v2-faq-icp-sizing}
{: faq}

必要な CA、ピア、および順序付けノードの数がわかったら、[デフォルトのリソース割り振りの表](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources)を参照し、ネットワークに必要な CPU (VPC) を大まかに見積もれます。

## Helm リリースを削除するとブロックチェーン・コンポーネントはどうなりますか?
{: #ibp-v2-faq-icp-delete}
{: faq}

{{site.data.keyword.cloud_notm}} Private クラスターから Helm リリースを削除しても、関連付けられたブロックチェーン・コンポーネントは削除されません。クラスターから Helm リリースを適切に削除するには、まずブロックチェーン・コンソールまたはブロックチェーン API を使用して、すべてのコンポーネントを削除する必要があります。その後、Helm チャートを削除できます。  
