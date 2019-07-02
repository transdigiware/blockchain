---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-31"

keywords: blockchain components, ca, certificate authorities, peer, ordering service, orderer, channel, smart contract, applications

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

# Panoramica dei componenti blockchain
{: #blockchain-component-overview}

I componenti e la struttura di {{site.data.keyword.blockchainfull}} Platform (IBP) si basano sugli strumenti e l'infrastruttura sottostanti di [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}, una soluzione blockchain con autorizzazioni open source di cui {{site.data.keyword.IBM_notm}} è uno dei principali collaboratori. Le reti basate su Fabric includono diversi componenti standard che possono essere distribuiti in alcune configurazioni per supportare un'ampia varietà di casi di utilizzo.

Per una visione più completa delle reti Fabric e dell'interrelazione dei componenti che la comprendono, vedi [questo documento sulla struttura di una rete blockchain](https://hyperledger-fabric.readthedocs.io/en/release-1.4/network/network.html) dalla documentazione della community Fabric che mostra come una rete può essere avviata e fatta evolvere.

Per una panoramica di alto livello dei componenti in una rete basata su Fabric, vedi il seguente video:

<iframe class="embed-responsive-item" id="youtubeplayer" title="Video del piano Starter" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/sJaT2L99BUo" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

*Sebbene questo video si concentri sui componenti dal punto di vista delle reti Starter ed Enterprise, le informazioni sono ancora molto rilevanti per la soluzione gestita dal cliente di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private.*

Ai fini di questa panoramica, ci concentreremo solo sulle autorità di certificazione (CA, Certificate Authority), sugli ordinanti, sui peer, sugli smart contract e sulle applicazioni. Come puoi vedere dagli argomenti della [Guida per la creazione di una rete di {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) e [Guida alla distribuzione di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ibp_for_icp_deployment_guide.html#get-started-icp), questa sequenza non è arbitraria; riflette la sequenza in cui verranno distribuiti i componenti in una rete basata su Fabric.

## Autorità di certificazione (CA, Certificate Authority)
{: #blockchain-component-overview-ca}

La base di una rete blockchain basata su Fabric sono le identità e le autorizzazioni. Le identità prendono la forma di certificati x.509 emessi da una CA e sono come una carta di credito in cui *identificano* qualcuno e possono includere attributi ad esso relativi. Questi certificati vengono poi collegati alle autorizzazioni tramite la loro inclusione nelle cartelle MSP al livello di componente o canale. Quindi, ad esempio, una MSP peer avrà una sottocartella MSP denominata **admins**. Ogni utente i cui certificati sono all'interno della cartella admin è quindi un amministratore del peer, il che significa che ha la possibilità di eseguire tutte le azioni che possono essere effettuate dall'amministratore di tale peer. Un sistema di convalida all'interno del peer esegue un controllo ogni volta che un utente, identificato dal proprio certificato di firma, prova ad eseguire un'azione di gestione. Il certificato corrisponde a quello nella cartella "admin"? Se corrisponde, l'azione può essere eseguita. Se non corrisponde, la richiesta per eseguire l'azione sarà rifiutata.

Le CA {{site.data.keyword.blockchainfull_notm}} Platform si basano sull'[Hyperledger Fabric-CA](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/){: external}, sebbene sia possibile utilizzare un'altra CA a condizione che utilizzi una PKI basata sui certificati x.509. Potrebbero esserci, e di solito ci sono, più livelli di CA. La "CA root" per una rete non sarà normalmente esposta tranne che per fornire i certificati alle "CA intermedie", che emetteranno i certificati direttamente agli utenti e ai componenti o a più livelli di CA intermedie. Per ulteriori dettagli su come vengono utilizzate le autorità di certificazione per stabilire identità e adesione, vedi la [documentazione di Hyperledger Fabric sull'identità](https://hyperledger-fabric.readthedocs.io/en/release-1.4/identity/identity.html){: external} e sull'[adesione](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external}.

## Servizi di ordine
{: #blockchain-component-overview-orderer}

Mentre il servizio di ordine viene spesso definito come il "cuore" di una rete, la sua funzione è in realtà piuttosto semplice: ordinare le transazioni che sono state convalidate dai peer in blocchi e rimandarle ai peer per essere scritte nei loro libri mastro. Nelle versioni precedenti di Fabric, questa funzionalità era integrata all'interno del peer, ma a partire da Fabric v1.0, è stata separata in un altro componente per aumentare le prestazioni del peer ed evitare anomalie che potrebbero causare potenziali fork di stato.

A un livello fisico, questa funzione di ordine normalmente richiede una serie di ordinanti collettivamente conosciuti come "servizio di ordine".

Per ulteriori informazioni sul servizio di ordine, vedi [The Ordering Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html){: external}.

## Peer
{: #blockchain-component-overview-peer}

A un livello fisico, una rete blockchain è formata principalmente da nodi peer (o semplicemente peer). I peer sono gli elementi fondamentali della rete perché ospitano i libri mastro e gli smart contract (che sono contenuti in ["chaincode"](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html){: external}. Più precisamente, i peer ospitano le **istanze** del libro mastro e le **istanze** degli smart contract. Poiché gli smart contract e i libri mastro vengono utilizzati, rispettivamente, per incapsulare i processi condivisi e le informazioni condivise in una rete, questi aspetti di un peer li rendono un buon punto di partenza per comprendere cosa faccia realmente una rete Fabric.

Per maggiori informazioni specifiche sui peer, vedi [questo documento che si concentra solo sui peer](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html){: external} dalla documentazione della community Fabric.

## Canali
{: #blockchain-component-overview-channels}

Un canale è un meccanismo che fornisce un livello aperto di comunicazione tra i membri nella rete. È possibile creare più canali tra sottoinsiemi di membri, supportando in tal modo uno dei [molti meccanismi per implementare la privacy](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}. I dati sulla rete blockchain sono archiviati sui libri mastro del canale. I libri mastro del canale sono ospitati sui peer delle organizzazioni che si sono unite al canale. Per ulteriori informazioni sui canali e su come utilizzarli, vedi la [documentazione di Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/channels.html){: external}.

## Smart contract
{: #blockchain-component-overview-smart-contracts}

Prima che le aziende di business possano negoziare tra loro, una comprensione comune sulle regole e sui processi deve essere raggiunta e definita in una sorta di contratto. Presi insieme, questi contratti definiscono il "modello di business" che regola tutte le interazioni tra i partner di business.

La stessa esigenza esiste nelle reti blockchain e, dove il termine tecnico per questi modelli di business è "smart contract", Fabric e {{site.data.keyword.blockchainfull_notm}} Platform contengono questi contratti in una struttura più ampia nota come "chaincode", che include non solo la logica di business ma anche l'infrastruttura sottostante che convalida le identità degli utenti che tentano di richiamare lo smart contract.

Nel mondo degli affari i contratti sono firmati e depositati tramite studi legali, mentre gli smart contract sono installati sui peer e "istanziati" su un canale.

## Applicazioni
{: #blockchain-component-overview-applications}

Le applicazioni client in una rete basata su Fabric come {{site.data.keyword.blockchainfull_notm}} Platform utilizzano infrastrutture sottostanti come API, SDK e smart contract per consentire le interazioni tra i client (richiami e query) a un livello di astrazione più elevato.

Per vedere come le applicazioni interagiscono con una rete basata su Fabric, consulta l'argomento relativo allo [sviluppo di applicazioni](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} nella documentazione di Hyperledger Fabric. Puoi anche visitare l'argomento [creazione di applicazioni](/docs/services/blockchain/howto/ibp-console-create-app.html#ibp-console-app) per informazioni su come connettere le tue applicazioni a {{site.data.keyword.blockchainfull_notm}} Platform.

## Una rete di esempio
{: #blockchain-component-overview-example-network}

La **Figura 1** mostra un esempio di rete blockchain distribuita composta da quattro membri (ognuno proprietario di due peer), CA (Certificate Authority, Autorità di certificazione) responsabili della distribuzione del materiale di identità crittografico e un servizio di ordine che definisce le politiche e i partecipanti della rete. Il canale blu contiene tutti i e quattro i membri della rete e il canale giallo è limitato a soli tre membri, le banche B, C e D. Puoi anche vedere che la banca A ricopre il ruolo di iniziatore della rete e che la banca D esiste solo come un membro nel contesto del canale giallo. Infine, un utente o un'applicazione in possesso di un certificato x509 correttamente firmato possono inviare chiamate ai peer sulla rete.

![Rete blockchain](images/blockchain_network_2-01.png "Rete blockchain di esempio")
