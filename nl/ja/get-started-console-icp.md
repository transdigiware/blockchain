---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud の概説
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform は、{{site.data.keyword.cloud_notm}}、独自のデータ・センター、サード・パーティーのパブリック・クラウドなど、複数のパブリック・クラウドおよびプライベート・クラウド間にデプロイできます。このマルチクラウド・デプロイメントは、{{site.data.keyword.IBM_notm}} の Kubernetes ベースのコンテナー・オーケストレーション・プラットフォームである {{site.data.keyword.cloud_notm}} Private によって実現できます。このオファリングでは、{{site.data.keyword.cloud_notm}} Private のデプロイメントに {{site.data.keyword.blockchainfull_notm}} Platform コンソールをインストールしてから、そのコンソールを使用してローカル・クラスター上に {{site.data.keyword.blockchainfull_notm}} コンポーネントを作成できます。また、他の {{site.data.keyword.cloud_notm}} プライベート・クラスター上または {{site.data.keyword.cloud_notm}} 上にデプロイされたノードをインポートすれば、コンソールを使用して分散マルチクラウド・ネットワークを操作することもできます。
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は {{site.data.keyword.cloud_notm}} Private 用のバンドル製品です。{{site.data.keyword.cloud_notm}} Private について詳しくは、[{{site.data.keyword.cloud_notm}} Private バージョン 3.1.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} の資料を参照してください。

## 手順 1: {{site.data.keyword.cloud_notm}} Private に Kubernetes クラスターをセットアップする
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

最初のステップは、選択したクラウド・プラットフォーム上の {{site.data.keyword.cloud_notm}} Private に Kubernetes クラスターをセットアップすることです。
推奨事項およびガイダンスについては、[ {{site.data.keyword.cloud_notm}} Private のセットアップ](/docs/services/blockchain/ICP_setup.html#icp-setup)を参照してください。

実動で使用するネットワークを構築する場合は、クラスター・デプロイメントとブロックチェーン・ネットワークを高可用性構成でセットアップする必要があります。

- クラスターの構成に関する推奨事項については、[Implement high availability on {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external} を参照してください。
- ネットワークの構築を開始できる状態になったら、[ピアの高可用性に関する考慮事項](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-peers)および[順序付けサービスの高可用性に関する考慮事項](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-ordering-service)を参照してください。

## 手順 2: {{site.data.keyword.blockchainfull_notm}} Platform をインストールする
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private は、パスポート・アドバンテージ (PPA) からダウンロード可能な Helm チャートとして提供されます。Helm チャートをローカル・クラスターにインストールする方法については、[{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private のインストール](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install)を参照してください。

## 手順 3: {{site.data.keyword.blockchainfull_notm}} Platform コンソールをデプロイする
{: #get-started-console-icp-step-three-deploy-console}

Helm チャートをインストールしたら、「カタログ」ページの {{site.data.keyword.blockchainfull_notm}} Platform アプリケーション・タイルをクリックして、ローカル・クラスターに {{site.data.keyword.blockchainfull_notm}} Platform コンソールをインストールできます。コンソールの構成方法、およびブロックチェーン・コンポーネントで必要なリソースの構成方法については、[{{site.data.keyword.blockchainfull_notm}} Platform コンソールのデプロイ](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp)を参照してください。

## 手順 4: ユーザーを管理者としてコンソールに追加する
{: #get-started-console-icp-step-four-add-console-admin}

コンソール管理者は、デプロイメント時に指定した E メール・アドレスとパスワードを使用してコンソールにログインできます。この指定したパスワードはコンソールのデフォルト・パスワードになるので、すべての新規ユーザーがコンソールへの初回ログインに使用します。その後、管理者が新規ユーザーをコンソールに追加して、他のユーザーがログインして {{site.data.keyword.blockchainfull_notm}} ノードの操作を開始できるようにします。管理者は、新しいデフォルト・パスワードを設定することもできます。詳しくは、[コンソールからのユーザーの管理](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users)を参照してください。

## 手順 5: コンソールを使用してコンポーネントを作成する
{: #get-started-console-icp-build-network}

コンソールをデプロイしたら、それを使用して、ローカル・クラスターで {{site.data.keyword.blockchainfull_notm}} コンポーネントを作成、操作、および管理できます。コンソール UI の使用を開始する方法については、[ネットワーク構築のチュートリアル](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network)を参照してください。

## 手順 6: クラウドをまたいでネットワークを接続する
{: #get-started-console-icp-import-nodes}

コンソールを使用して、他の {{site.data.keyword.cloud_notm}} Private クラスター上または {{site.data.keyword.cloud_notm}} 上で稼働しているコンポーネントを操作できます。まずは、コンポーネントが最初にデプロイされたコンソールから、コンポーネント情報を JSON ファイルにエクスポートする必要があります。その後、ローカル・クラスターにデプロイしたコンソールに、そのノード JSON ファイルをインポートし、複数のクラウドをまたいでコンポーネントを管理できます。詳しくは、[ノードのインポート](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes)を参照してください。
