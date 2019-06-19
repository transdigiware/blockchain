---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# {{site.data.keyword.blockchainfull_notm}} Platform の概説
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform には、任意の環境にブロックチェーン・コンポーネントをデプロイできる、フルスタックのマネージド Blockchain as a Service (BaaS) オファリングが用意されています。 {{site.data.keyword.cloud_notm}} 環境にも、{{site.data.keyword.cloud_notm}} Private 経由でオンプレミス環境にも、サード・パーティーのクラウド (Amazon Web Services (AWS) など) 環境にもデプロイできます。 このチュートリアルでは、{{site.data.keyword.blockchainfull_notm}} Platform を使用して基本的なブロックチェーン・ネットワークをセットアップする一般的な方法について順を追って説明します。
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform オファリングを使用する前に、[特記事項](/docs/services/blockchain/needtoknow.html#disclaimer)のセクションにある技術情報とサポート情報をお読みください。
{: important}


## 始めに
{: #get-started-ibp-prereqs}

あらゆるタイプのユーザーがブロックチェーンの利用を開始してアプリケーションを実稼働環境に移行できるように、{{site.data.keyword.blockchainfull_notm}} Platform にはさまざまなオファリングが用意されています。 使用する環境およびオファリングを決定する必要があります。 図 1 に、各種オファリングの機能、対象者、価格、およびクラウド・プラットフォームを示します。 それぞれのオファリングについて詳しくは、以下の表のオファリングをクリックしてください。

| **オファリング** | **組み込まれている機能** | **請求ポリシー** | **クラウド・プラットフォーム** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**{{site.data.keyword.blockchainfull_notm}} Platform の VS Code 用の拡張機能**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | 開発者は、スマート・コントラクトを短時間で開発するために、コマンド・パレットからアクセスできるコマンドやエクスプローラーを備えた IDE を使用して作業を開始できます。 | 無料 | ローカル・マシンで実行 |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) | ユーザーの {{site.data.keyword.cloud_notm}} Kubernetes クラスターにブロックチェーン・コンポーネントをデプロイして管理するために使用できる {{site.data.keyword.blockchainfull_notm}} Platform コンソールおよび API | [$0.29 USD/1 時間の VPC である VPC の料金](/docs/services/blockchain/howto/pricing-saas.html) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) | Kubernetes Helm チャートを使用して {{site.data.keyword.cloud_notm}} Private クラスターにデプロイする {{site.data.keyword.blockchainfull_notm}} Platform コンソール  | [VPC 料金](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto/remote_peer.html#remote-peer-aws-about) | {{site.data.keyword.cloud_notm}} の外部にリモート・ピアをデプロイするための AWS クイック・スタート・テンプレート | 無料 | AWS |

*図 1. {{site.data.keyword.blockchainfull_notm}} Platform オファリング*


## 手順 1: オファリングを取得する
{: #get-started-ibp-step1}

{{site.data.keyword.blockchainfull_notm}} Platform オファリングを取得するためのクラウド・アカウントまたは PPA ライセンスを持っていることを確認します。

* **{{site.data.keyword.blockchainfull_notm}} Platform の VS Code 用の拡張機能**

  この VS Code 拡張機能は、[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} から無料で入手できます。この拡張機能を使用すれば、最終的に {{site.data.keyword.blockchainfull_notm}} にデプロイするスマート・コントラクトの開発、デバッグ、テストを行えます。

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  このオファリングは、{{site.data.keyword.cloud_notm}} の[{{site.data.keyword.cloud_notm}}「カタログ」ダッシュボード](https://cloud.ibm.com/catalog){: external}で提供されています。

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  このオファリングは、[パスポート・アドバンテージ・オンライン](https://www.ibm.com/software/passportadvantage/pao_customer.html)で、デプロイ可能な Helm チャートとして提供されています。

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  このオファリングは AWS で提供されています。[クイック・スタート・テンプレート](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}を使用して、ブロックチェーン・ピアを AWS にデプロイできます。

## 手順 2: {{site.data.keyword.blockchainfull_notm}} Platform をデプロイする
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  {{site.data.keyword.cloud_notm}} にログインし、オファリングを使用してサービス・インスタンスを作成します。 ウィザードに従って、ネットワークの初期構成を実行します。 詳しくは、[Getting started  {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks) を参照してください。

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  ネットワークをデプロイする前に、Helm チャートを {{site.data.keyword.cloud_notm}} Private クラスターにインストールする必要があります。その後、{{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイし、それを使用してローカル・クラスターにブロックチェーン・コンポーネントをデプロイして操作することができます。詳しくは、[{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の概説](/docs/services/blockchain/get-started-console-icp.html#get-started-console-icp)を参照してください。

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  このオファリングは AWS で提供されています。[クイック・スタート・テンプレート](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}を使用して、ブロックチェーン・ピアを AWS にデプロイできます。

## 次のステップ
{: #get-started-ibp-next-steps}

選択した環境に {{site.data.keyword.blockchainfull_notm}} Platform をデプロイしたら、ネットワークのコンソーシアム、ノード、チャネル、およびスマート・コントラクトをセットアップし、ネットワーク上でトランザクションを開始できます。 詳しくは、この文書の左側のナビゲーションの**「方法」**セクションにあるタスクのトピックを参照してください。

## サポートについて
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} では、{{site.data.keyword.IBM_notm}} が実装したブロックチェーン・ソリューションのさまざまなサポート・オプションを提供しています。 {{site.data.keyword.blockchainfull_notm}} Platform サポートの詳細については、[サポートのページ](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support)を参照してください。
