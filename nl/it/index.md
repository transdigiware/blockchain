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

# Introduzione a {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform fornisce un'offerta Baas (blockchain-as-a-service) gestita e full-stack che ti consente di distribuire componenti blockchain in ambienti a tua scelta. L'ambiente può essere {{site.data.keyword.cloud_notm}}, in loco tramite {{site.data.keyword.cloud_notm}} Private e cloud di terze parti, come AWS (Amazon Web Services). In questa esercitazione, ti mostreremo il processo generale di configurazione di una rete blockchain di base con {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

Prima di utilizzare un'offerta {{site.data.keyword.blockchainfull_notm}} Platform, leggi le informazioni tecniche e di supporto nella sezione [Dichiarazione di non responsabilità](/docs/services/blockchain/needtoknow.html#disclaimer).
{: important}


## Prima di cominciare
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform fornisce diverse offerte per aiutare tutti i tipi di utenti ad iniziare il loro percorso blockchain e a passare le loro applicazioni in produzione. Devi decidere il tuo ambiente e l'offerta che utilizzerai. La Figura 1 elenca le funzionalità, i gruppi di destinatari, i prezzi e le piattaforme cloud per diverse offerte. Per ulteriori informazioni su ciascuna offerta, fai clic sull'offerta nella seguente tabella.

| **Offerte** | **Cos'è incluso** | **Politica di fatturazione** | **Piattaforma cloud** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | Gli sviluppatori possono iniziare con l'IDE che fornisce un programma di esplorazione e i comandi accessibili dalla tavolozza dei comandi per lo sviluppo di smart contract in tempi rapidi. | Gratuito | Viene eseguito sulla tua macchina locale |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) | API e console {{site.data.keyword.blockchainfull_notm}} Platform che possono essere utilizzate per distribuire e gestire i componenti blockchain nel tuo cluster {{site.data.keyword.cloud_notm}} Kubernetes. | [Prezzi VPC $0,29 USD/VPC-ora](/docs/services/blockchain/howto/pricing-saas.html) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) | Console {{site.data.keyword.blockchainfull_notm}} Platform distribuita su un cluster {{site.data.keyword.cloud_notm}} Private utilizzando un grafico Helm di Kubernetes. | [Prezzi VPC](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto/remote_peer.html#remote-peer-aws-about) | Template Quick Start AWS per distribuire i peer remoti che si trovano fuori da {{site.data.keyword.cloud_notm}} | Gratuito | AWS |

*Figura 1. Offerte {{site.data.keyword.blockchainfull_notm}} Platform*


## Passo uno: Ottieni un'offerta
{: #get-started-ibp-step1}

Assicurati di avere l'account cloud o la licenza PPA per ottenere un'offerta {{site.data.keyword.blockchainfull_notm}} Platform.

* **{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**

  Questa estensione VS Code è disponibile gratuitamente nel [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} e può essere utilizzata per sviluppare, sottoporre a debug e testare gli smart contract per un'eventuale distribuzione in {{site.data.keyword.blockchainfull_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Questa offerta è disponibile nel [Dashboard del catalogo {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog){: external} di {{site.data.keyword.cloud_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Questa offerta viene fornita come un grafico Helm distribuibile tramite [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Questa offerta è disponibile in AWS e puoi distribuire un peer blockchain in AWS utilizzando il [template Quick Start](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Passo due: Distribuisci {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Accedi a {{site.data.keyword.cloud_notm}} e crea un'istanza del servizio con l'offerta. Segui la procedura guidata per completare la configurazione iniziale per la tua rete. Per ulteriori informazioni, vedi [Introduzione a {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Prima di distribuire una rete, devi installare il grafico Helm su un cluster {{site.data.keyword.cloud_notm}} Private. Puoi quindi distribuire la console {{site.data.keyword.blockchainfull_notm}} Platform e utilizzarla per distribuire e gestire componenti blockchain sul tuo cluster locale. Per ulteriori informazioni, vedi [Introduzione a {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain/get-started-console-icp.html#get-started-console-icp).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Questa offerta è disponibile in AWS e puoi distribuire un peer blockchain in AWS utilizzando il [template Quick Start](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Passi successivi
{: #get-started-ibp-next-steps}

Dopo che hai distribuito {{site.data.keyword.blockchainfull_notm}} Platform nell'ambiente di tua scelta, puoi configurare la rete con il consorzio, i nodi, i canali e gli smart contract e puoi iniziare le transazioni su di esso. Per ulteriori informazioni, vedi gli argomenti relativi alle attività nella sezione **COME** nella navigazione di sinistra di questa documentazione.

## Richiesta di assistenza tecnica
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offre diverse opzioni di supporto sulle soluzioni blockchain implementate da {{site.data.keyword.IBM_notm}}. Per ulteriori informazioni sul supporto {{site.data.keyword.blockchainfull_notm}} Platform, vedi [Richiesta di assistenza tecnica](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).
