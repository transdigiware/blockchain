---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# Informazioni su {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform può essere distribuito in cloud pubblici e privati, come {{site.data.keyword.cloud_notm}}, il tuo data center e cloud pubblici di terze parti. Questa distribuzione multicloud è possibile mediante {{site.data.keyword.cloud_notm}} Private, la piattaforma di orchestrazione dei contenitori basata su Kubernetes di IBM. Questa release fornisce un'interfaccia utente della console blockchain che puoi utilizzare per distribuire e gestire i componenti blockchain su un cluster {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private utilizza la base di codice Hyperledger Fabric v1.4.1 e supporta la distribuzione su {{site.data.keyword.cloud_notm}} Private v3.2.0.
{:shortdesc}

## Cosa offre {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-offers}

Puoi utilizzare questa offerta per installare la console {{site.data.keyword.blockchainfull_notm}} Platform su una distribuzione di {{site.data.keyword.cloud_notm}} Private. Puoi quindi utilizzare la console per creare tutti i componenti fondamentali di una blockchain Hyperledger Fabric, una CA (Certificate Authority, Autorità di certificazione), un servizio di ordine e i peer, sul tuo cluster locale. Per ulteriori informazioni sui blocchi di creazione di reti Hyperledger Fabric, vedi la [panoramica del componente blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview). Puoi anche utilizzare la tua console per gestire una rete multicloud distribuita importando i nodi distribuiti su altri cluster {{site.data.keyword.cloud_notm}} Private o su {{site.data.keyword.cloud_notm}}.

Questa release {{site.data.keyword.blockchainfull_notm}} Platform include le seguenti funzioni chiave:

**COMPILAZIONE ---- Esperienza di sviluppatore integrata**
- **Codifica facilmente** i tuoi smart contract in Node.js, Golang o Java, scrivi le applicazioni client utilizzando la nuova estensione {{site.data.keyword.blockchainfull_notm}} VS Code, avvaliti dell'**integrazione SDK** con la console di interfaccia utente e impara dalle nostre esercitazioni e dai nostri esempi molto esaurienti.
- **DevOps semplificato** ti consente di passare dallo sviluppo alla verifica e alla produzione aumentando le tue risorse Kubernetes per aggiungere ulteriori componenti.
- **Integrazione del servizio Kubernetes.** Avvaliti di servizi quali Grafana e Prometheus per la registrazione e Kibana per il monitoraggio.
- **Funzioni chiave di Fabric aggiornate**. Avvaliti delle funzioni più recenti di Hyperledger Fabric v1.4.1:
  - [Servizio di ordine Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Raccolte di dati privati](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) che forniscono una maggiore riservatezza dei dati garantendo che i dati del libro mastro siano condivisi solo con i peer autorizzati tramite il protocollo gossip.
  - Il [rilevamento dei servizi](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, che ti consente di rilevare e aggiornare in modo dinamico il modo in cui la tua applicazione interagisce con la tua rete.
  - Gli ACL ([Channel access control List](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}), che ti consentono un controllo aggiuntivo sulla governance dei tuoi canali e smart contract.

**UTILIZZO --- Controllo totale delle tue distribuzioni**
- **Distribuisci solo i componenti di cui hai bisogno**. Connetti un peer a più canali e reti oppure ospita un servizio di ordine a cui possono connettersi i business partner.
- **Mantieni un controllo completo delle tue identità**. Memorizza e gestisci le chiavi utilizzate per amministrare i tuoi nodi nel tuo ambiente protetto.
- **Operazione unificata**. La console {{site.data.keyword.blockchainfull_notm}} Platform ti consente di distribuire e gestire tutte le tue organizzazioni e tutti i tuoi nodi in **una singola console** senza dover fare affidamento su {{site.data.keyword.IBM_notm}} o altri fornitori per gestire i tuoi nodi di ordinazione o la tua Certificate Authority. Puoi anche aggiungere o rimuovere membri da un consorzio blockchain, creare e unirti a canali e installare e istanziare smart contract dalla tua console.
- **Ospita o unisciti a una rete**. Distribuisci i peer ospitati nel tuo cluster a più canali su più cloud oppure invita altre organizzazioni a unirsi al tuo consorzio o ai tuoi canali mentre le organizzazioni gestiscono i loro nodi in modo indipendente nelle infrastrutture.
- **Gestisci l'accesso** degli utenti che possono amministrare o monitorare i tuoi nodi.
- **Accesso diretto ai log** dei tuoi nodi dal tuo servizio Kubernetes. Utilizza qualsiasi servizio di terze parti supportato per estrarre e analizzare i tuoi log.
- **Interagisci direttamente con i tuoi pod** utilizzando il tuo servizio Kubernetes.
- **Raccolta della firma dinamica** che permette un migliore controllo sulla governance collaborativa sulle configurazioni del canale.

**CRESCITA --- Scalabilità e flessibilità**
- **Scegli la tua capacità di calcolo**. Hai la flessibilità di decidere la quantità di CPU, memoria e archiviazione di cui desideri eseguire il provisioning nel tuo cluster Kubernetes. Per ulteriori informazioni, vedi [In che modo la console interagisce con il tuo cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Ridimensiona** ampliando e riducendo le risorse nel tuo cluster Kubernetes, pagando solo per quello di cui hai bisogno. Per ulteriori informazioni, vedi [Prezzi](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Ripristino di emergenza e disponibilità multizona.** Questa capacità duplica la tua distribuzione Kubernetes tra le zone, abilitando l'alta disponibilità (HA, High Availability) dei tuoi componenti e il ripristino di emergenza (DR, Disaster Recovery).
- **Esegui ovunque**. Grazie alla **base di codice unificata** della console {{site.data.keyword.blockchainfull_notm}} Platform, puoi eseguire i tuoi componenti su qualsiasi ambiente supportato da {{site.data.keyword.cloud_notm}} Private.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è un prodotto in bundle per {{site.data.keyword.cloud_notm}} Private e viene fornito come un grafico Helm Kubernetes. Per ulteriori informazioni su {{site.data.keyword.cloud_notm}} Private, vedi la documentazione per [{{site.data.keyword.cloud_notm}} Private versione 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è adatto per te
{: #console-icp-about-suitable}

L'esecuzione di {{site.data.keyword.blockchainfull_notm}} Platform al di fuori di {{site.data.keyword.cloud_notm}} ti fornisce maggiore flessibilità di crescita o adesione a una rete blockchain. Aiuta gli iniziatori della rete a far crescere le loro reti consentendo a nuovi membri di unirsi pur utilizzando la piattaforma di loro scelta. Consentirà alle organizzazioni interessate di unirsi alle reti blockchain per collocare i loro peer con le loro applicazioni esistenti o di integrarsi con i loro system of record.

Gli utenti di questa offerta gestiranno la loro sicurezza e la loro infrastruttura. {{site.data.keyword.cloud_notm}} non fornisce tali servizi. Prima di iniziare, esamina [Considerazioni e limitazioni](#console-icp-about-considerations) nella prossima sezione.

## Considerazioni e limitazioni
{: #console-icp-about-considerations}

Prima di iniziare, assicurati di comprendere le seguenti limitazioni:

- Sei responsabile della gestione del monitoraggio dell'integrità, della sicurezza, della registrazione e dell'uso delle risorse dei tuoi componenti blockchain.
- La console può essere utilizzata solo per creare e controllare i componenti basati su Hyperledger Fabric v1.4.1 o successive.
- Puoi distribuire solo uno spazio dei nomi Kubernetes peer della console. Se intendi creare più reti blockchain, ad esempio per creare diversi ambienti per la distribuzione, la preparazione e la produzione, devi creare uno spazio dei nomi univoco per ogni ambiente.
- Puoi anche importare i nodi che sono stati esportati da altre console {{site.data.keyword.blockchainfull_notm}} Platform. Per poter gestire un nodo di ordinazione o un peer importato dalla console, devi anche importare l'identità dell'amministratore e la definizione di MSP dell'organizzazione del nodo associato nella tua console.
- {{site.data.keyword.blockchainfull_notm}} Platform è supportato solo su {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition.  {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition non è supportato.
- Non puoi utilizzare {{site.data.keyword.IBM_notm}} Multicloud Manager per installare il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform.

Per una visualizzazione degli ambienti supportati da {{site.data.keyword.cloud_notm}} Private, vedi il documento relativo agli [ambienti supportati](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Prerequisiti del sistema
{: #console-icp-about-prerequisites}

Vedi [System requirements](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} per l'elenco di prerequisiti hardware e software per {{site.data.keyword.cloud_notm}} Private. Tieni presente tuttavia che {{site.data.keyword.blockchainfull_notm}} Platform non è supportato su sistemi Linux su Power (ppc64le) POWER8.

Il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è stato convalidato per l'esecuzione sui cluster {{site.data.keyword.cloud_notm}} Private v3.2 su Ubuntu Linux utilizzando i seguenti nodi di lavoro e la seguente archiviazione di supporto:

- **LinuxONE e {{site.data.keyword.IBM_notm}} Z**: z/VM e KVM, che stanno utilizzando NFS.
- **x86**: Linux a 64 bit che utilizza GlusterFS.

## Licenza
{: #console-icp-about-license}

Per ulteriori informazioni sulla concessione di licenze e sui prezzi, vedi [Prezzi per {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing).

## Installazione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private viene fornito come un file di grafico Helm che può essere installato in un cluster {{site.data.keyword.cloud_notm}} Private locale. Dopo che hai installato il grafico Helm, puoi trovare {{site.data.keyword.blockchainfull_notm}} Platform come un'applicazione nel catalogo {{site.data.keyword.cloud_notm}} Private. Per istruzioni su come installare il grafico Helm e i prerequisiti necessari, vedi [Installazione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install).

Se non hai dimestichezza di {{site.data.keyword.cloud_notm}} Private e vorresti informazioni e suggerimenti sull'installazione e la distribuzione di {{site.data.keyword.cloud_notm}} Private, vedi [Configurazione di {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

Puoi anche distribuire {{site.data.keyword.blockchainfull_notm}} Platform dietro un firewall, senza avere accesso all'Internet pubblico. Il pacchetto del grafico Helm scaricato include tutte le immagini Docker del componente Fabric che {{site.data.keyword.blockchainfull_notm}} Platform utilizza, senza estrarle da DockerHub durante la distribuzione.

## Considerazioni sulla sicurezza
{: #console-icp-about-security}

Poiché questi componenti sono distribuiti sulla tua infrastruttura, sei responsabile della gestione della loro sicurezza. Ciò include importanti aree di sicurezza, come la gestione delle chiavi e la crittografia dei dati. Esamina i seguenti argomenti quando prendi in considerazione la sicurezza per i tuoi componenti.

### Sicurezza dei dati
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} utilizza la crittografia dell'intero disco, che è basata sulla [crittografia a chiave simmetrica](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} per proteggere tutti i dati creati da ogni istanza del servizio. Devi adottare misure simili nel tuo ambiente per proteggere i dati del tuo peer.

I dati nel tuo database dello stato, indipendentemente dal fatto che tu usi LevelDB o CouchDB, non sono crittografati. Puoi utilizzare la crittografia a livello delle applicazioni per proteggere i dati inattivi nel tuo database dello stato.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### Gestione delle chiavi
{: #console-icp-about-security-key-management}

La gestione delle chiavi è un aspetto critico della sicurezza. Se una chiave privata è compromessa o persa, attori ostili potrebbero essere in grado di accedere ai tuoi dati e alle tue funzionalità. {{site.data.keyword.IBM_notm}} utilizza dispositivi fisici denominati HSM ([Hardware Security Module](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm)) per archiviare le chiavi private delle reti piano Enterprise di {{site.data.keyword.blockchainfull_notm}} Platform.

Sei responsabile della gestione delle tue chiavi private. Anche se puoi utilizzare l'IU della console {{site.data.keyword.blockchainfull_notm}} Platform per generare delle chiavi private, tali chiavi non vengono archiviate dalla console o all'interno di {{site.data.keyword.cloud_notm}} Private. È essenziale archiviare le chiavi in modo protetto in modo che non vengano compromesse.

Puoi utilizzare Key Escrow per recuperare le chiavi private perse. Questo deve essere eseguito prima della perdita di qualsiasi chiave. Se non è possibile recuperare una chiave privata, devi ottenere nuove chiavi private registrando una nuova identità con la tua Certificate Authority (Autorità di certificazione). Devi inoltre rimuovere e sostituire il tuo signCert da tutti i canali a cui ti sei unito.

### TLS
{: #console-icp-about-security-tls}

[TLS (Transport Layer Security)](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} è integrato nel modello di attendibilità di Hyperledger Fabric. Tutti i componenti di {{site.data.keyword.blockchainfull_notm}} Platform utilizzano TLS per autenticarsi e comunicare tra loro. Pertanto, i nodi su {{site.data.keyword.cloud_notm}} Private devono essere in grado di completare un handshake TLS con altri componenti e con le tue applicazioni. Ciò implica, tra le altre cose, che devi abilitare il pass-through, utilizzando ad esempio l'elenco elementi consentiti (whitelist), nel tuo firewall dalle applicazioni client ai tuoi nodi.

### Sicurezza delle applicazioni
{: #console-icp-about-security-appl}

Poiché tutti i richiami del chaincode sono firmati, Fabric gestisce la sicurezza delle applicazioni. Inoltre, Fabric include anche controlli a livello di applicazione basati su ACL.

## Richiesta di assistenza tecnica
{: #console-icp-about-support}

Per ulteriori informazioni su come ottenere supporto su {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, e per risorse per sviluppatori di blockchain gratuite e forum di supporto che puoi utilizzare per risolvere i problemi, vedi [Richiesta di assistenza tecnica](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Se hai acquistato una licenza {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private e vuoi contattare il supporto clienti, vedi le informazioni sull'[accesso alla community di supporto {{site.data.keyword.IBM_notm}} e sull'apertura di un ticket di supporto](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
