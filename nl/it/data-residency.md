---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: IBM Blockchain Platform, IBM Cloud Private, AWS, Data residency, world state

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

# Residenza dei dati
{: #console-icp-about-data-residency}

Poiché le reti blockchain non rilevano il tipo di dati che viene elaborato, è a volte necessario eseguire delle operazioni supplementari per mantenere al sicuro certi tipi di dati. Il requisito più comune sulla residenza dei dati è associato alle leggi di alcuni paesi, che impongono che tutti i dati elaborati e memorizzati in un sistema IT debbano rimanere all'interno dei confini di un determinato paese. Allo stesso modo, alcune aziende in settori altamente regolamentati, come il governo, la sanità e i servizi finanziari, richiedono che i dati siano memorizzati interamente dietro il proprio firewall.

Le reti blockchain consentono a più organizzazioni di utilizzare un libro mastro distribuito per eseguire transazioni e condividere i dati in modo attendibile e sicuro. Tuttavia, questo implica che i dati possono essere distribuiti tra i nodi della rete e le regioni in cui risiedono tali nodi. Le organizzazioni possono utilizzare diverse opzioni per suddividere i dati dal resto della rete e ottenere la residenza dei dati:
1. [Raccolte di dati privati su un canale condiviso](#console-icp-about-data-residency-fabric)
2. [Raccolte di dati privati su un canale separato](#console-icp-about-data-residency-use-case)
3. [Un canale separato con tutti i nodi nel canale all'interno di un solo paese](#console-icp-about-data-residency-use-case-channel)

Ciascun approccio fornisce un livello di isolamento e protezione più elevato per i tuoi dati, ma richiede uno sforzo aggiuntivo di implementazione e gestione. Per aiutarti a comprendere come ciascuna opzione può essere utilizzata per ottenere la residenza dei dati, ti forniamo una panoramica di come i dati vengono condivisi all'interno della rete Hyperledger Fabric. Ti forniamo quindi un caso di utilizzo di esempio per illustrare come le organizzazioni all'interno di un consorzio blockchain dovrebbe utilizzare ciascuna opzione per suddividere i propri dati ed evitare che lascino la propria regione.

## Come i dati vengono condivisi all'interno di una rete {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-icp-about-data-residency-fabric}

L'architettura di Hyperledger Fabric che sta alla base di {{site.data.keyword.blockchainfull_notm}} Platform è incentrata su tre componenti chiave: un servizio di ordinazione (costituito da nodi di ordinazione), CA (Certificate Authority, Autorità di certificazione) e peer. Inoltre, le organizzazioni inviano transazioni a questi nodi dalle applicazioni client utilizzando gli [SDK Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}. Quando prendi in considerazione la residenza dei dati, è importante capire come questi componenti interagiscono e archiviano i dati.

I **peer** vengono utilizzati dai membri del consorzio per archiviare il [libro mastro](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ledger/ledger.html){: external} blockchain. Il libro mastro blockchain è composto da due parti. La prima parte è lo stato globale, che archivia l'ultimo valore di tutti i dati nel libro mastro in coppie chiave-valore. La seconda parte è il record blockchain di ogni transazione. I peer ricevono gli aggiornamenti di stato sotto forma di nuovi blocchi dal servizio di ordinazione. Utilizzano poi i blocchi e lo stato globale per confermare le transazioni (o eseguirne il commit), aggiornare lo stato globale e aggiungere il log delle transazioni sulla blockchain. Il servizio di ordinazione stabilisce l'ordine delle transazioni di tutti i peer sul [consorzio](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium) e archivia una copia della porzione del libro mastro blockchain.

I **canali** sono un meccanismo per la trasmissione dei dati all'interno di una rete. Non puoi partecipare a una rete blockchain senza unirti a un canale. I canali possono essere utilizzati dai membri della rete per creare separazione logica tra le applicazioni di business e anche per migliorare le prestazioni limitando il traffico. Possono inoltre essere utilizzati dai sottoinsiemi di organizzazioni nel consorzio per eseguire transazioni in modo privato e per suddividere i dati.

I peer conservano un libro mastro separato per ogni canale a cui si uniscono. Solo le organizzazioni che sono membri del canale possono unire i propri peer al canale e ricevere gli aggiornamenti del libro mastro dal servizio di ordinazione. Di conseguenza, ogni canale è associato a un servizio di ordinazione, che archivia la porzione di blockchain di ogni libro mastro del canale che conserva. Le applicazioni client inviano le transazioni ai peer e al servizio di ordinazione di un determinato canale. Queste transazioni vengono aggiunte al log di transazioni all'interno della blockchain e includono una [serie di lettura-scrittura](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, che viene aggiunta alle coppie chiave-valore nello stato globale.

Se la residenza dei dati all'interno del paese è un requisito, devi considerare l'ubicazione dei tuoi peer, del servizio di ordinazione, nonché delle tue applicazioni client. Hai anche bisogno di conoscere l'ubicazione dei peer che appartengono ad altre organizzazioni sui tuoi canali.  Se stai utilizzando {{site.data.keyword.blockchainfull_notm}} Platform per {{site.data.keyword.cloud_notm}}, puoi trovare l'elenco di [regioni e ubicazioni {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference?topic=blockchain-ibp-regions-locations#ibp-regions-locations) in cui tu e i membri del consorzio possono distribuire i tuoi componenti.

## Un caso di utilizzo per la residenza dei dati
{: #console-icp-about-data-residency-use-case}

Possiamo utilizzare un consorzio di esempio per illustrare come i dati vengono distribuiti a {{site.data.keyword.blockchainfull_notm}} Platform ed esplorare come i membri possono ottenere la residenza dei dati. La seguente figura contiene un consorzio con un servizio di ordinazione e quattro organizzazioni. Ciascuna organizzazione ha un nodo peer. Due organizzazioni, Org A e Org B e il servizio di ordinazione sono ubicati negli Stati Uniti. Le altre due organizzazioni, Org C e Org D, sono ubicate in Germania. Tutte e quattro le organizzazioni sono membri del canale X e hanno unito i propri peer ad esso.

![Un consorzio di esempio](images/data_res_use_case.svg "Un consorzio di esempio")

Ogni peer che si è unito al canale X archivia una copia del libro mastro del canale, visualizzato nella **Figura 1** come libro mastro X. Poiché i peer dagli Stati Uniti e dalla Germania si sono uniti al canale, i dati sul libro mastro del canale risiedono in entrambe le aree geografiche. La porzione del libro mastro della blockchain viene archiviata anche dal servizio di ordinazione ubicato negli Stati Uniti.

Se due organizzazioni nel consorzio creano un secondo canale, il canale Y, viene creato e archiviato un secondo libro mastro sui peer dei membri del canale. Solo le organizzazioni che si sono unite al canale avranno una copia dei dati del canale.

![Aggiunta di un secondo canale](images/data_res_use_case_channel.svg "Aggiunta di un secondo canale")


Nella **Figura 2**, Org B e Org D si sono unite al canale Y. I peer di Org B e Org D archiviano ora una copia del libro mastro Y, in aggiunta al libro mastro X. Poiché è stato utilizzato lo stesso servizio di ordinazione per creare i canali X e Y, il servizio di ordinazione dispone ora di una copia della porzione dei libri mastri di entrambi i canali blockchain. Sia nella **Figura 1** che nella **Figura 2**, i dati creati dalle applicazioni in Germania vengono archiviati negli Stai Uniti, cosa non desiderabile se è richiesta la residenza dei dati.

Possiamo utilizzare il precedente esempio per esplorare le opzioni a disposizione delle organizzazioni per ottenere la residenza dei dati. Supponiamo che una normativa in Germania richiede che alcuni dei dati creati da Org C e Org D rimangano all'interno del paese. Le organizzazioni in Germania possono utilizzare tutte e tre le opzioni per evitare che i dati vengano archiviati negli Stai Uniti.

## Opzione uno: raccolte dei dati privati su un canale condiviso
{: #console-icp-about-data-residency-use-case-private-data}

Org C e Org D possono utilizzare la [funzione di dati privati](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "What is a private data collection?"){: external} di Hyperledger Fabric per evitare che i dati vengano distribuiti a tutte le organizzazioni sul canale. Le raccolte di dati privati consentono alle organizzazioni di condividere i dati di stato peer-to-peer (tramite il protocollo Gossip) con altre organizzazioni autorizzate a leggere la raccolta. I dati vengono memorizzati in un database privato e separato sul peer. Il servizio di ordine non è coinvolto e non vede i dati privati. Solo un hash dei dati nella raccolta viene aggiunto al libro mastro del canale e archiviato sui peer degli altri membri del canale o del servizio di ordinazione. Questo consente alle organizzazioni di verificare i dati privati se vogliono rendere pubblici i dettagli della transazione. Per ulteriori informazioni, consulta l'articolo relativo al concetto di [Dati privati](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external} nella documentazione di Fabric.

![Dati privati](images/data_res_private_data.svg "didascalia tre")

Nella **Figura 3**, Org C e Org D hanno creato una raccolta di dati privati, la raccolta OrgC-OrgD, che consente alle organizzazioni di eseguire transazioni senza dover condividere i dati con Org A, Org B o il servizio di ordinazione. I dati di stato di valore chiave in questa raccolta vengono archiviati solo sui peer di Org C e Org D e non lasciano la Germania. Tuttavia, un hash di dati all'interno della raccolta viene archiviato sul libro mastro X e condiviso con il canale in modo più ampio. Questo significa che un hash di dati nella raccolta OrgC-OrgD viene archiviato sui peer e sul servizio di ordinazione negli Stati Uniti.

Quando prendi in considerazione i dati privati, è importante comprendere la differenza tra **dati con hash** e **dati crittografati**. La crittografia utilizza una funzione bidirezionale per trasformare i dati in un formato che ne nasconde il valore originale, ma che può essere riconvertito allo stato originale. Ad esempio, quando i dati vengono inviati su una rete protetta tramite TLS, i dati vengono crittografati utilizzando un certificato TLS. Vengono quindi inviati sulla rete come testo crittografato e poi decrittografati dal destinatario. Il testo crittografato contiene tutti i dati originali e può essere decrittografato utilizzando una chiave privata. Tuttavia, l'hashing è una funzione unidirezionale che utilizza i dati per creare una stringa univoca di numeri e lettere. I dati con hash non posso essere riconvertiti al formato originale utilizzando l'hash. Per verificare i dati creati dall'hash, un destinatario deve creare un nuovo hash dei dati originali utilizzando la stessa funzione hash e verificare che i valori di hash corrispondano. Una terza parte non può utilizzare l'hash senza una copia dei dati originali.  

È importante fare attenzione con questa opzione perché mentre le organizzazioni A e B non possono visualizzare i dati del libro mastro reali perché sono con hash, sono ancora in grado di visualizzare che le organizzazioni C e D stanno eseguendo transazioni e possono vedere il volume delle transazioni che si verificano tra loro.

Considera inoltre che i dati in una raccolta di dati privati possono essere eliminati dai peer che li archiviano. Mentre i dati vengono archiviati su un canale per sempre, le raccolte permettono ai membri di specificare di quanti blocchi viene eseguito il commit a un canale prima che [i dati privati vengano eliminati](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private_data_tutorial.html#pd-purge){: external}. Dopo che i dati sono stati rimossi dalla raccolta di dati privati, l'hash sul canale non può più essere utilizzato per verificare la transazione che lo ha creato. Nella rete di esempio nella **Figura 3**, Org C e Org D possono utilizzare una politica `block to live` per assicurarsi che tutti i dati che non hanno bisogno di essere conservati per sempre vengano rimossi completamente dalla rete entro un periodo di tempo specificato.

## Opzione due: raccolte dei dati privati su un canale separato
{: #console-icp-about-data-residency-use-case-private-data-channel}

Org C e Org D possono utilizzare anche raccolte di dati privati nel contesto di un canale separato per fornire ulteriore isolamento dei propri dati. La creazione di un nuovo canale, il canale Y in questo caso, garantisce che l'hash dei dati privati sia condiviso solo con il servizio di ordinazione, senza essere condiviso con gli altri membri del consorzio e archiviato sui loro peer.

![Utilizzo dei dati privati con un canale separato](images/data_res_private_data_channel.svg "Utilizzo dei dati privati con un canale separato")

Nella **Figura 4**, Org C e Org D hanno formato un nuovo canale, il canale Y, che non contiene alcun membro negli Stati Uniti. Di conseguenza, gli hash di dati nella raccolta OrgC-OrgD vengono archiviati sul libro mastro Y invece di X e non vengono archiviati sui peer negli Stati Uniti. Poiché il servizio di ordinazione è ubicato negli Stati Uniti, un hash di dati creato in Germania lascia ancora il paese.

La creazione di un canale separato può evitare che i dettagli della transazione siano condivisi con le altre organizzazioni nel consorzio. Se le organizzazioni in Germania utilizzano un canale condiviso, Org A e Org B saranno in grado di vedere il numero di hash della transazione di cui è stato eseguito il commit sul libro mastro del canale dalla raccolta di dati privati. Questo potrebbe fornire a tali organizzazioni la visibilità sul fatto che le organizzazioni C e D stanno eseguendo transazioni e il volume di transazioni generate tra loro. Tieni presente però che formare un nuovo canale richiede un ulteriore costo di gestione in termini di creazione e aggiornamento. Formare un nuovo canale rende più difficile per Org C e Org D condividere i dati con Org A e Org B.

## Opzione tre: un canale con tutti i componenti nel paese
{: #console-icp-about-data-residency-use-case-channel}

Org C e Org D possono anche creare un canale con tutta l'infrastruttura nello stesso paese. Questo richiede che tutti i peer uniti al canale, le applicazioni e il servizio di ordinazione risiedano all'interno della stessa regione. In questo scenario, nessuno dei dati archiviati sul libro mastro del canale lascerà la regione e sarà archiviato al di fuori del paese.

![Creazione di un canale separato](images/data_res_separate_channel.svg "Creazione di un canale separato")

Nella **Figura 5**, Org C e Org D hanno creato un nuovo canale per i dati che non devono lasciare la Germania. Questo richiede la creazione di un nuovo servizio di ordinazione ubicato in Germania per garantire che la copia del libro mastro del canale dell'ordinante sia archiviata nel paese. Poiché in questo caso il servizio di ordinazione, OrgC-peer e OrgD-peer sono ubicati in Germania, Org C e Org D possono ora mantenere i dati pubblici sul canale se lo desiderano oppure possono ancora decidere di utilizzare le raccolte di dati privati per evitare che tutti i dati della transazione vengano archiviati sul servizio di ordinazione.

La creazione di un canale con tutti i componenti in un solo paese garantisce che tutti i dati risiedano all'interno di una regione, questo include le coppie di chiave e valore, il log della transazione blockchain e gli hash di tutti i dati privati. Tuttavia, questa opzione richiede il costo di mantenimento di un nuovo canale e i costi associati alla gestione del servizio di ordinazione.

## Materiale di riferimento
{: #console-icp-about-data-residency-reference}

Per una comprensione più approfondita del flusso di dati sulla rete {{site.data.keyword.blockchainfull_notm}} Platform, fai riferimento alla [documentazione di Fabric sul flusso di transazioni](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

In futuro, una ZKP (zero knowledge proof, dimostrazione a conoscenza zero) migliorerà la capacità di ottenere un'ulteriore residenza dei dati in Hyperledger Fabric. Una ZKP (zero knowledge proof, dimostrazione a conoscenza zero) consente a un “prover” di assicurare a un “verificatore” di essere a conoscenza di un segreto senza rivelare il segreto stesso. È un modo per dimostrare che sai qualcosa che soddisfa un'affermazione senza mostrare ciò che sai.

Puoi ottenere ulteriori informazioni sulle raccolte di dati privati e sulla ZKP (zero knowledge proof, dimostrazione a conoscenza zero) nel white paper sulle [Transazioni private e confidenziali con Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.
