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

{{site.data.keyword.blockchainfull}} Platform può essere distribuito in cloud pubblici e privati, come {{site.data.keyword.cloud_notm}}, il tuo data center e cloud pubblici di terze parti. Questa distribuzione multicloud è possibile mediante {{site.data.keyword.cloud_notm}} Private, la piattaforma di orchestrazione dei contenitori basata su Kubernetes di IBM. Questa release fornisce un'interfaccia utente della console che puoi utilizzare per distribuire e gestire i componenti blockchain su un cluster {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private utilizza la base di codice Hyperledger Fabric v1.4.1 e supporta la distribuzione su {{site.data.keyword.cloud_notm}} Private v3.2.
{:shortdesc}

Questa release {{site.data.keyword.blockchainfull_notm}} Platform include le seguenti funzioni chiave:

**COMPILAZIONE ---- Esperienza di sviluppatore integrata**
- **Codifica facilmente** i tuoi smart contract in Node.js, Golang o Java, scrivi le applicazioni client utilizzando la nuova estensione {{site.data.keyword.blockchainfull_notm}} VS Code, avvaliti dell'**integrazione SDK** con la console di interfaccia utente e impara dalle nostre esercitazioni e dai nostri esempi molto esaurienti.
- **DevOps semplificato** ti consente di passare dallo sviluppo alla verifica e alla produzione aumentando le tue risorse Kubernetes per aggiungere ulteriori componenti.
- **Integrazione del servizio Kubernetes.** Avvaliti di servizi quali Grafana e Prometheus per la registrazione e Kibana per il monitoraggio.
- **Funzioni chiave di Fabric aggiornate**. Avvaliti delle funzioni più recenti di Hyperledger Fabric v1.4.1:
  - [Servizio di ordine Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Raccolte di dati privati](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) che forniscono una maggiore riservatezza dei dati garantendo che i dati del libro mastro siano condivisi solo con i peer autorizzati tramite il protocollo gossip.
  - Il [rilevamento dei servizi](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, che ti consente di rilevare e aggiornare in modo dinamico il modo in cui la tua applicazione interagisce con la tua rete.
  - Gli ACL ([Channel access control List](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}), che ti consentono un controllo aggiuntivo sulla governance dei tuoi canali e smart contract.

**UTILIZZO --- Controllo totale delle tue distribuzioni**
- **Distribuisci solo i componenti di cui hai bisogno**. Connetti un peer a più canali e reti oppure ospita un servizio di ordine a cui possono connettersi i business partner.
- **Mantieni un controllo completo delle tue identità**. Memorizza e gestisci le chiavi utilizzate per amministrare i tuoi nodi nel tuo ambiente protetto.
- **Operazione unificata**. La console {{site.data.keyword.blockchainfull_notm}} Platform ti consente di distribuire e gestire tutte le tue organizzazioni e tutti i tuoi nodi in **una singola console** senza dover fare affidamento su {{site.data.keyword.IBM_notm}} o altri fornitori per gestire i tuoi ordinanti o la tua Certificate Authority. Puoi anche aggiungere o rimuovere membri da un consorzio blockchain, creare e unirti a canali e installare e istanziare smart contract dalla tua console.
- **Ospita o unisciti a una rete**. Distribuisci i peer ospitati nel tuo cluster a più canali su più cloud oppure invita altre organizzazioni a unirsi al tuo consorzio o ai tuoi canali mentre le organizzazioni gestiscono i loro nodi in modo indipendente nelle infrastrutture.
- **Gestisci l'accesso** degli utenti che possono amministrare o monitorare i tuoi nodi.
- **Accesso diretto ai log** dei tuoi nodi dal tuo servizio Kubernetes. Utilizza qualsiasi servizio di terze parti supportato per estrarre e analizzare i tuoi log.
- **Interagisci direttamente con i tuoi pod** utilizzando il tuo servizio Kubernetes.
- **Raccolta della firma dinamica** che permette un migliore controllo sulla governance collaborativa sulle configurazioni del canale.

**CRESCITA --- Scalabilità e flessibilità**
- **Scegli la tua capacità di calcolo**. Hai la flessibilità di decidere la quantità di CPU, memoria e archiviazione di cui desideri eseguire il provisioning nel tuo cluster Kubernetes. Per ulteriori informazioni, vedi [In che modo la console interagisce con il tuo cluster Kubernetes](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction).
- **Ridimensiona** ampliando e riducendo le risorse nel tuo cluster Kubernetes, pagando solo per quello di cui hai bisogno. Per ulteriori informazioni, vedi [Prezzi](/docs/services/blockchain/howto/pricing.html#ibp-pricing).
- **Ripristino di emergenza e disponibilità multizona.** Questa capacità duplica la tua distribuzione Kubernetes tra le zone, abilitando l'alta disponibilità (HA, High Availability) dei tuoi componenti e il ripristino di emergenza (DR, Disaster Recovery).
- **Esegui ovunque**. Grazie alla **base di codice unificata** della console {{site.data.keyword.blockchainfull_notm}} Platform, puoi eseguire i tuoi componenti su qualsiasi ambiente supportato da {{site.data.keyword.cloud_notm}} Private.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è un prodotto in bundle per {{site.data.keyword.cloud_notm}} Private e viene fornito come un grafico Helm Kubernetes. Per ulteriori informazioni su {{site.data.keyword.cloud_notm}} Private, vedi la documentazione per [{{site.data.keyword.cloud_notm}} Private versione 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Cosa offre {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private ti consente di installare la console {{site.data.keyword.blockchainfull_notm}} Platform su una distribuzione di {{site.data.keyword.cloud_notm}} Private. Puoi quindi utilizzare la console per creare tutti i componenti fondamentali di una blockchain Hyperledger Fabric, una CA (Certificate Authority, Autorità di certificazione), un servizio di ordine e i peer, sul tuo cluster locale. Per ulteriori informazioni sui blocchi di creazione di reti Hyperledger Fabric, vedi la [panoramica del componente blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview). Puoi anche utilizzare la tua console per gestire una rete multicloud distribuita importando i nodi distribuiti su altri cluster {{site.data.keyword.cloud_notm}} Private o su {{site.data.keyword.cloud_notm}}.


## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è adatto per te

L'esecuzione di {{site.data.keyword.blockchainfull_notm}} Platform al di fuori di {{site.data.keyword.cloud_notm}} ti fornisce maggiore flessibilità di crescita o adesione a una rete blockchain. Aiuta gli iniziatori della rete a far crescere le loro reti consentendo a nuovi membri di unirsi pur utilizzando la piattaforma di loro scelta. Consentirà alle organizzazioni interessate di unirsi alle reti blockchain per collocare i loro peer con le loro applicazioni esistenti o di integrarsi con i loro system of record.

Gli utenti di questa offerta gestiranno la loro sicurezza e la loro infrastruttura. {{site.data.keyword.cloud_notm}} non fornisce tali servizi. Prima di iniziare, esamina [Considerazioni e limitazioni](#console-icp-about-considerations) nella prossima sezione.

## Considerazioni e limitazioni
{: #console-icp-about-considerations}

Prima di iniziare, assicurati di comprendere le seguenti limitazioni:

- Sei responsabile della gestione del monitoraggio dell'integrità, della sicurezza, della registrazione e dell'uso delle risorse dei tuoi componenti blockchain.
- La console può essere utilizzata solo per creare e controllare i componenti basati su Hyperledger Fabric v1.4.1 o successive.
- Puoi anche importare i nodi che sono stati esportati da altre console {{site.data.keyword.blockchainfull_notm}} Platform. Per poter gestire un ordinante o un peer importato dalla console, devi anche importare l'identità dell'amministratore e la definizione di MSP dell'organizzazione del nodo associato nella tua console.

Per una visualizzazione degli ambienti supportati da {{site.data.keyword.cloud_notm}} Private, vedi il documento relativo agli [ambienti supportati](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Prerequisiti del sistema
{: #console-icp-about-prerequisites}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private supporta i seguenti sistemi operativi:
- RHEL (Red Hat Enterprise Linux) 7.3, 7.4, 7.5
- Ubuntu 18.04 LTS e 16.04 LTS
- SLES (SUSE Linux Enterprise Server) 12 SP3

Il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private è stato convalidato per l'esecuzione sui cluster {{site.data.keyword.cloud_notm}} Private v3.2 su Ubuntu Linux utilizzando i seguenti nodi di lavoro e la seguente archiviazione di supporto:

- **LinuxONE e {{site.data.keyword.IBM_notm}} Z**: z/VM e KVM, che stanno utilizzando NFS.
- **x86**: Linux a 64 bit che utilizza GlusterFS.

## Licenza
{: #console-icp-about-license}

Per ulteriori informazioni sulla concessione di licenze e sui prezzi, vedi [Prezzi per {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing).

## Installazione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private viene fornito come un file di grafico Helm che può essere installato in un cluster {{site.data.keyword.cloud_notm}} Private locale. Dopo aver installato il grafico Helm, puoi trovare {{site.data.keyword.blockchainfull_notm}} Platform come applicazione nel catalogo di {{site.data.keyword.cloud_notm}} Private. Per istruzioni su come installare il grafico Helm e i prerequisiti necessari, vedi [Installazione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#helm-console-install).

Se non hai dimestichezza di {{site.data.keyword.cloud_notm}} Private e vorresti informazioni e suggerimenti sull'installazione e la distribuzione di {{site.data.keyword.cloud_notm}} Private, vedi [Configurazione di {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup).

Puoi anche distribuire {{site.data.keyword.blockchainfull_notm}} Platform dietro un firewall, senza avere accesso all'Internet pubblico. Il pacchetto del grafico Helm scaricato include tutte le immagini Docker del componente Fabric che {{site.data.keyword.blockchainfull_notm}} Platform utilizza, senza estrarle da DockerHub durante la distribuzione.

## Considerazioni sulla sicurezza
{: #console-icp-about-security}

Poiché questi componenti sono distribuiti sulla tua infrastruttura, sei responsabile della gestione della loro sicurezza. Ciò include importanti aree di sicurezza, come la gestione delle chiavi e la crittografia dei dati. Esamina i seguenti argomenti quando prendi in considerazione la sicurezza per i tuoi componenti.

### Sicurezza dei dati
{: #console-icp-about-security-data}

I piani Starter ed Enterprise di {{site.data.keyword.blockchainfull_notm}} Platform utilizzano la crittografia dell'intero disco, che è basata sulla [crittografia a chiave simmetrica](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} per proteggere tutti i dati utilizzati dalle reti. Devi adottare misure simili nel tuo ambiente per proteggere i dati del tuo peer.

I dati nel tuo database dello stato, indipendentemente dal fatto che tu usi LevelDB o CouchDB, non sono crittografati. Puoi utilizzare la crittografia a livello delle applicazioni per proteggere i dati inattivi nel tuo database dello stato.

### Residenza dei dati
{: #console-icp-about-security-data-residency}

I requisiti di residenza dei dati possono imporre che l'elaborazione e l'archiviazione di tutti i dati del libro mastro blockchain rimangano entro i confini di un singolo paese (o entro quale altro limite definito). Per ulteriori informazioni su come è possibile realizzare la residenza dei dati, vedi [Residenza dei dati](#console-icp-about-data-residency).

### Gestione delle chiavi
{: #console-icp-about-security-key-management}

La gestione delle chiavi è un aspetto critico della sicurezza. Se una chiave privata è compromessa o persa, attori ostili potrebbero essere in grado di accedere ai tuoi dati e alle tue funzionalità. {{site.data.keyword.IBM_notm}} utilizza dispositivi fisici denominati HSM ([Hardware Security Module](/docs/services/blockchain/glossary.html#glossary-hsm)) per archiviare le chiavi private delle reti piano Enterprise di {{site.data.keyword.blockchainfull_notm}} Platform.

Quando distribuisci un componente su {{site.data.keyword.cloud_notm}} Private, sei responsabile della gestione delle tue chiavi private. Sebbene {{site.data.keyword.blockchainfull_notm}} Platform generi le chiavi private, tali chiavi non vengono archiviate su Platform. È essenziale archiviare le chiavi in modo protetto in modo che non vengano compromesse. Puoi trovare la chiave privata del tuo componente nella cartella keystore dell'MSP del peer, nella directory `/mnt/crypto/peer/peer/msp/keystore/` all'interno del tuo componente. Per ulteriori informazioni sui certificati all'interno del tuo peer, vedi la sezione [Membership Services Provider](/docs/services/blockchain/certificates.html#managing-certificates-msp) dell'esercitazione [Gestione dei certificati su {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/certificates.html#managing-certificates).

Puoi utilizzare Key Escrow per recuperare le chiavi private perse. Questo deve essere eseguito prima della perdita di qualsiasi chiave. Se non è possibile recuperare una chiave privata, devi ottenere nuove chiavi private registrando una nuova identità con la tua Certificate Authority (Autorità di certificazione). Devi inoltre rimuovere e sostituire il tuo signCert da tutti i canali a cui ti sei unito.

### TLS
{: #console-icp-about-security-tls}

[TLS (Transport Layer Security)](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} è integrato nel modello di attendibilità di Hyperledger Fabric. Tutti i componenti Starter ed Enterprise in {{site.data.keyword.blockchainfull_notm}} Platform utilizzano TLS per autenticarsi e comunicare tra loro. Pertanto, i componenti di rete su Platform devono essere in grado di completare un handshake TLS con i tuoi peer. Ciò implica, tra le altre cose, che devi abilitare il pass-through, utilizzando ad esempio l'elenco elementi consentiti (whitelist), nel tuo firewall dalle applicazioni client al tuo peer.

### Configurazione di Membership Service Provider
{: #console-icp-about-security-MSP}

I componenti di {{site.data.keyword.blockchainfull_notm}} Platform utilizzano le identità tramite gli MSP (Membership Service Provider). Gli MSP associano i certificati emessi dalle CA ai ruoli di rete e di canale. Per ulteriori informazioni sulla modalità di funzionamento degli MSP con il peer, vedi [Membership Service Provider (MSP)](/docs/services/blockchain/certificates.html#managing-certificates-msp).

### Sicurezza delle applicazioni
{: #console-icp-about-security-appl}

Poiché tutti i richiami del chaincode sono firmati, Fabric gestisce la sicurezza delle applicazioni. Inoltre, Fabric include anche controlli a livello di applicazione basati su ACL.

## Residenza dei dati
{: #console-icp-about-data-residency}

Poiché le reti blockchain non rilevano il tipo di dati che viene elaborato, è a volte necessario eseguire delle operazioni supplementari per mantenere al sicuro certi tipi di dati. Il requisito più comune sulla residenza dei dati è associato alle leggi di alcuni paesi, che impongono che tutti i dati elaborati e memorizzati in un sistema IT debbano rimanere all'interno dei confini di un determinato paese. Allo stesso modo, alcune aziende in settori altamente regolamentati, come il governo, la sanità e i servizi finanziari, richiedono che i dati siano memorizzati interamente dietro il proprio firewall. Pertanto, per ottenere la residenza dei dati, tutti i componenti della rete blockchain devono far parte dello stesso [canale](/docs/services/blockchain/glossary.html#glossary-channel) e risiedere all'interno del confine di un singolo paese.

Per soddisfare i requisiti di residenza dei dati, è importante comprendere l'architettura di Hyperledger Fabric che sta alla base di {{site.data.keyword.blockchainfull_notm}} Platform. L'architettura è incentrata su tre componenti chiave: un servizio di ordine (costituito da ordinanti), CA (Certificate Authority, Autorità di certificazione) e peer. Un peer riceve gli aggiornamenti di stato ordinati sotto forma di blocchi dal servizio di ordine e mantiene lo stato e il libro mastro. Pertanto, un peer e un servizio di ordine hanno una relazione diretta. Il libro mastro contiene i valori più recenti per tutte le chiavi e i dati inclusi nei log delle transazioni.

Inoltre, le applicazioni client utilizzano gli [SDK Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external} per inviare transazioni ai peer e al servizio di ordine. Queste transazioni includono i dati della [serie di lettura-scrittura](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, che contengono le coppie chiave-valore sul libro mastro.

Se la residenza dei dati all'interno del paese è un requisito, l'ordinante, i peer e le applicazioni client devono risiedere nello stesso paese. Quando viene creata una rete {{site.data.keyword.blockchainfull_notm}} Platform in {{site.data.keyword.cloud_notm}}, hai la possibilità di selezionare un'ubicazione per la rete. <!--For a Starter Plan network, you can select from US South, United Kingdom, and Sydney. For an Enterprise Plan network, you can select from currently available locations, which include Dallas, Frankfurt, London, Sao Paulo, Tokyo, and Toronto. -->Per ulteriori informazioni sulle regioni e sulle ubicazioni, vedi [Regioni e ubicazioni {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference/ibp_regions.html#ibp-regions-locations).

### Un caso di utilizzo per la residenza dei dati

Prendi in considerazione una rete {{site.data.keyword.blockchainfull_notm}} Platform che include l'ordinante e la CA insieme a un consorzio di quattro organizzazioni. Le organizzazioni hanno uno o più nodi peer e tutte e quattro le organizzazioni fanno parte di un singolo canale e tutti i componenti della rete risiedono nella regione (ad esempio Francoforte) dove è stata distribuita la rete {{site.data.keyword.blockchainfull_notm}} Platform. Infine, le applicazioni client che interagiscono con i peer risiedono anch'esse in Germania.

In questo esempio, la residenza dei dati viene mantenuta.

![Residenza dei dati quando tutti i componenti si trovano nello stesso paese](images/remote_peer_data_res_1.png "Residenza dei dati quando tutti i componenti si trovano nello stesso paese")

Prendiamo ora in considerazione le implicazioni quando un **peer distribuito** si unisce a una delle organizzazioni. Un peer distribuito può risiedere nella stessa regione del resto della rete o dovunque al di fuori della regione della rete {{site.data.keyword.blockchainfull_notm}} Platform:

-	Se il peer risiede nello stesso paese del resto della rete, la residenza dei dati viene mantenuta. Tutti i dati di libro mastro restano all'interno della Germania come nella **Figura 1** in alto.
-	Se il peer risiede in un altro paese (ad esempio negli Stati Uniti), la residenza dei dati non viene più mantenuta poiché i dati nel libro mastro del peer sono condivisi fuori dai confini nazionali.

Per risolvere questo problema, è possibile utilizzare i **canali** per segregare i dati a un sottoinsieme di peer sulla rete. Quando la rete {{site.data.keyword.blockchainfull_notm}} Platform contiene un peer distribuito e ordinanti che oltrepassano i confini nazionali, i canali forniscono l'isolamento dei dati di libro mastro dalle organizzazioni con dei peer fuori dai confini nazionali.

**Nota:** gli ordinanti si trovano sempre nella regione del data center che hai selezionato per ospitare la rete. Non è possibile avere più ordinanti oltre i confini nazionali. Tuttavia, i peer possono trovarsi nel data center o in un'ubicazione remota al di fuori di {{site.data.keyword.cloud_notm}}.

![Residenza dei dati quando i peer si trovano fuori dal paese della regione di {{site.data.keyword.blockchainfull_notm}} Platform](images/remote_peer_data_res_2.png "Residenza dei dati quando i peer si trovano fuori dal paese della regione di {{site.data.keyword.blockchainfull_notm}} Platform")

Nella **Figura 2**, la residenza dei dati non è richiesta per `OrgC` e `OrgD`. In effetti, `OrgD` ora include due peer distribuiti, `OrgD-peer1` e `OrgD-peer2`, che si trovano negli *Stati Uniti*. Pertanto, affinché `OrgA`, `OrgB` e le loro rispettive applicazioni client e i loro rispettivi peer che risiedono in Germania isolino i dati di libro mastro sul canale `X`, viene creato un nuovo canale `Y` per `OrgC` e `OrgD`.

Per una comprensione più approfondita del flusso di dati sulla rete {{site.data.keyword.blockchainfull_notm}} Platform, vedi la [documentazione di Fabric sul flusso di transazioni](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

In futuro, la nuova tecnologia in Hyperledger Fabric migliorerà la capacità di ottenere un'ulteriore residenza dei dati utilizzando le [raccolte di dati privati](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} e la ZKP (zero knowledge proof, dimostrazione a conoscenza zero).

- Una raccolta di dati privati garantisce che i dati privati siano condivisi peer-to-peer (tramite il protocollo gossip) solo con i peer autorizzati a vederli, ad esempio, i peer che si trovano all'interno dei confini nazionali. I dati vengono memorizzati in un database privato sul peer. Qui il servizio di ordine non è coinvolto e non vede i dati privati. Un hash di questi dati viene scritto nei libri mastro di ogni peer sul canale. L'hash utilizzato per la convalida dello stato funge da prova della transazione e può essere utilizzato per scopi di controllo. I dati privati sono disponibili per le reti in {{site.data.keyword.blockchainfull_notm}} Platform in esecuzione su Fabric versione 1.2.1 o successiva. Tuttavia, la funzione dei dati privati non è disponibile per i peer in esecuzione in {{site.data.keyword.cloud_notm}} Private poiché le reti {{site.data.keyword.cloud_notm}} Private attualmente non utilizzano gossip.

- Una ZKP (zero knowledge proof, dimostrazione a conoscenza zero) consente a un “prover” (dimostratore) di assicurare a un “verifier” (verificatore) di essere a conoscenza di un segreto senza rivelare il segreto stesso.

Puoi ottenere ulteriori informazioni su queste tecnologie nel white paper sulle [Transazioni private e confidenziali con Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.

## Richiesta di assistenza tecnica
{: #console-icp-about-support}

Per ulteriori informazioni su come ottenere supporto su {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, e per risorse per sviluppatori di blockchain gratuite e forum di supporto che puoi utilizzare per risolvere i problemi, vedi [Richiesta di assistenza tecnica](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).

Se hai acquistato una licenza {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private e vuoi contattare il supporto clienti, vedi le informazioni sull'[accesso alla community di supporto {{site.data.keyword.IBM_notm}} e sull'apertura di un ticket di supporto](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
