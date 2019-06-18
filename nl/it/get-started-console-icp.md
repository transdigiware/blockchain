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

# Introduzione a {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform può essere distribuito in cloud pubblici e privati, come {{site.data.keyword.cloud_notm}}, il tuo data center e cloud pubblici di terze parti. Questa distribuzione multicloud è possibile mediante {{site.data.keyword.cloud_notm}} Private, la piattaforma di orchestrazione dei contenitori basata su Kubernetes di {{site.data.keyword.IBM_notm}}. Puoi utilizzare questa offerta per installare la console {{site.data.keyword.blockchainfull_notm}} Platform su una distribuzione di {{site.data.keyword.cloud_notm}} Private e utilizza quindi la console per creare componenti {{site.data.keyword.blockchainfull_notm}} sul tuo cluster locale. Puoi anche utilizzare la tua console per gestire una rete multicloud distribuita importando i nodi distribuiti su altri cluster {{site.data.keyword.cloud_notm}} Private o su {{site.data.keyword.cloud_notm}}.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è un prodotto in bundle per {{site.data.keyword.cloud_notm}} Private. Per ulteriori informazioni su {{site.data.keyword.cloud_notm}} Private, vedi la documentazione per [{{site.data.keyword.cloud_notm}} Private versione 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Passo uno: Configura un cluster Kubernetes su {{site.data.keyword.cloud_notm}} Private
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

Il primo passo consiste nel configurare un cluster Kubernetes su {{site.data.keyword.cloud_notm}} Private sulla piattaforma cloud da te scelta.
Puoi visitare [Configurazione di {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_setup.html#icp-setup) per suggerimenti e orientamenti.

Se stai creando una rete che verrà utilizzata in produzione, devi configurare la tua distribuzione cluster e la tua rete blockchain per l'alta disponibilità.

- Per consigli sulla configurazione del tuo cluster, visita [Implement high availability on {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- Quando sei pronti a iniziare a creare la tua rete, vedi i documenti relativi alle [considerazioni sull'alta disponibilità per i peer](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-peers) e alle [considerazioni sull'alta disponibilità per i servizi di ordine](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-ordering-service).

## Passo due: Installa {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private viene fornito come un grafico Helm che può essere scaricato da PPA (Passport Advantage). Per istruzioni su come installare il grafico Helm sul tuo cluster locale, vedi [Installazione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install).

## Passo tre: Distribuisci la console {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-three-deploy-console}

Dopo che hai installato il grafico Helm, puoi fare clic sul tile dell'applicazione {{site.data.keyword.blockchainfull_notm}} Platform nella pagina del catalogo per installare la console {{site.data.keyword.blockchainfull_notm}} Platform sul tuo cluster locale. Per istruzioni su come configurare la console, e sulle risorse richieste dai tuoi componenti blockchain, vedi [Distribuzione della console {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).

## Passo quattro: Aggiungi utenti alla console come amministratore
{: #get-started-console-icp-step-four-add-console-admin}

L'amministratore della console può accedere alla console utilizzando l'indirizzo email e la password che sono stati forniti durante la distribuzione. La password fornita diventa la password predefinita della console ed è utilizzata da tutti i nuovi utenti per accedere alla console per la prima volta. L'amministratore può quindi aggiungere nuovi utenti alla console, consentendo ad altri di collegarsi e iniziare a lavorare con i nodi {{site.data.keyword.blockchainfull_notm}}. L'amministratore può inoltre impostare una nuova password predefinita. Per ulteriori informazioni, vedi [Gestione degli utenti dalla console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Passo cinque: Usa la console per creare i tuoi componenti
{: #get-started-console-icp-build-network}

Dopo che hai distribuito la console, puoi utilizzarla per creare, gestire e controllare i componenti {{site.data.keyword.blockchainfull_notm}} sul tuo cluster locale. Per iniziare a utilizzare l'IU della console, vedi l'[esercitazione relativa alla creazione di una rete](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network).

## Passo sei: Connetti le reti nei cloud
{: #get-started-console-icp-import-nodes}

Puoi utilizzare la console per gestire i componenti che sono in esecuzione su altri cluster {{site.data.keyword.cloud_notm}} Private o su {{site.data.keyword.cloud_notm}}. Dovrai innanzitutto esportare le informazioni sul componente in un file JSON dalla console dove era stato originariamente distribuito il componente. Puoi quindi importare il file JSON del nodo nella console distribuita sul tuo cluster locale e gestire i componenti nei cloud. Per ulteriori informazioni, vedi [Importazione di nodi](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).
