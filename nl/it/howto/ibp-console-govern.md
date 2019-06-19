---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: network components, IBM Cloud Kubernetes Service, allocate resources, batch timeout, channel update, reallocate resources

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

# Governance dei componenti
{: #ibp-console-govern}

Dopo aver creato CA, peer, ordinanti, organizzazioni e canali, puoi utilizzare la console per aggiornare questi componenti.
{:shortdesc}

Se stai utilizzando la versione di prova beta di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, è probabile che alcuni pannelli nella tua console non corrisponderanno alla documentazione corrente, che viene mantenuta aggiornata con l'istanza del servizio generalmente disponibile (GA). Per avvalerti dei vantaggi di tutte le ultime funzionalità, ti incoraggiamo in questo momento ad eseguire il provisioning a una nuova istanza del servizio GA seguendo le istruzioni in [Introduzione a {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).

**Gruppi di destinatari:** questo argomento è pensato per gli operatori di rete che sono responsabili della creazione, del monitoraggio e della gestione della rete blockchain.

## In che modo la console interagisce con il tuo cluster Kubernetes
{: #ibp-console-govern-iks-console-interaction}

È responsabilità dell'operatore della rete monitorare l'utilizzo di CPU, memoria e archiviazione e assicurarsi che le risorse disponibili siano adeguate **prima** di creare o ridimensionare un nodo.
{:important}

Poiché la tua istanza della console {{site.data.keyword.blockchainfull_notm}} e il tuo cluster Kubernetes non comunicano direttamente le informazioni relative alle informazioni disponibili, il processo per distribuire o ridimensionare i componenti utilizzando la console deve seguire il seguente schema.

1. **Definisci la dimensione della distribuzione che vuoi eseguire**. I pannelli **Allocazione risorse** per CA, peer e ordinante nella console offrono allocazioni di CPU, memoria e archiviazione predefinite per ciascun nodo. Potresti dover regolare questi valori in base al tuo caso d'uso. Se non sei sicuro, inizia con le allocazioni predefinite e regolale man mano che comprendi le tue esigenze. In modo analogo, il pannello **Riallocazione risorse** visualizza le allocazioni di risorse esistenti.

  Per avere un'idea di che quantità di archiviazione e calcolo avrai bisogno nel tuo cluster, fai riferimento al grafico dopo questo elenco, che contiene i valori predefiniti correnti per peer, ordinante e CA.

2. **Verifica se disponi di risorse sufficienti nel tuo cluster Kubernetes**. Se stai utilizzando un cluster Kubernetes ospitato in {{site.data.keyword.cloud_notm}}, ti consigliamo di utilizzare lo strumento [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} in combinazione con il tuo dashboard {{site.data.keyword.cloud_notm}} Kubernetes. Se non disponi di spazio a sufficienza nel tuo cluster per distribuire o ridimensionare le risorse, devi aumentare la dimensione del tuo cluster {{site.data.keyword.cloud_notm}} Kubernetes Service. Per ulteriori informazioni su come aumentare la dimensione di un cluster, vedi [Ridimensionamento dei cluster](/docs/containers?topic=containers-ca#ca){: external}. Se stai utilizzando un provider cloud diverso da {{site.data.keyword.cloud_notm}}, dovrai consultare la relativa documentazione per informazioni su come ridimensionare i cluster. Se disponi di spazio sufficiente nel tuo cluster, puoi continuare con il passo 3.
3. **Utilizza la console per distribuire o ridimensionare il tuo nodo**. Se il tuo pod Kubernetes è abbastanza grande da accogliere la nuova dimensione del nodo, la riallocazione dovrebbe procedere senza alcun problema. Se il nodo di lavoro su cui è in esecuzione il pod sta esaurendo le risorse, puoi aggiungere un nuovo nodo di lavoro più grande al tuo cluster ed eliminare quindi il nodo di lavoro esistente.

| **Componente** (tutti i contenitori) | CPU  | Memoria (GB) | Archiviazione (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1,2           | 2,4                   | 200 (include 100GB per peer e 100GB per CouchDB)|
| **CA**                         | 0,1           | 0,2                   | 20                     |
| **Nodo di ordine**        | 0,45          | 0,9                   | 100                    |

Se intendi distribuire un servizio di ordine Raft a cinque nodi, tieni presente che il totale della tua distribuzione aumenterà di un fattore di cinque. Quindi un totale di 2,25 CPU, 4,5 GB di memoria e 500 GB di archiviazione per i cinque nodi Raft. Ciò rende il servizio di ordine a cinque nodi più grande di un nodo di lavoro singolo Kubernetes di 2 CPU.
{:tip}

Per i casi in cui un utente desideri ridurre al minimo gli addebiti senza disattivare del tutto o eliminare un nodo, è possibile ridurre un nodo a un minimo di 0,001 CPU (1 milliCPU). Nota: il nodo non sarà operativo quando si utilizza questa quantità di CPU.

Mentre le figure in questo argomento tentano di essere precise, fai attenzione che ci sono delle volte in cui un nodo potrebbe non venire distribuito anche se sembra che disponi di abbastanza spazio nel tuo cluster. Assicurati di fare riferimento al tuo dashboard Kubernetes per vedere quando i componenti vengono distribuiti e per i messaggi di errore quando non avviene. Nei casi in cui un componente non viene distribuito per mancanza di risorse, anche se sembra che ci sia abbastanza spazio nel cluster, è probabile che dovrai distribuire ulteriori risorse cluster per il componente da distribuire.
{:important}

## Allocazione di risorse
{: #ibp-console-govern-allocate-resources}

Mentre gli utenti di un cluster gratuito **devono utilizzare le dimensioni predefinite** per i contenitori associati ai loro nodi, gli utenti dei cluster a pagamento possono impostare questi valori durante la creazione dei loro nodi.

Il pannello **Allocazione risorse** nella console fornisce i valori predefiniti per i diversi campi coinvolti nella creazione di un nodo. Questi valori vengono scelti perché rappresentano un buon modo per iniziare. Tuttavia, ogni caso d'uso è diverso. Mentre questo argomento fornirà una guida sui modi in cui pensare a questi valori, in ultima istanza è responsabilità dell'utente monitorare i suoi nodi e trovare le dimensioni a lui adatte. Pertanto, fatta eccezione per le situazioni in cui gli utenti sono certi che avranno bisogno di valori diversi da quelli predefiniti, una strategia pratica consiste nell'utilizzare inizialmente questi valori predefiniti e regolarli in un secondo momento. Per una panoramica delle prestazioni e della scala di Hyperledger Fabric, su cui è basato {{site.data.keyword.blockchainfull_notm}} Platform, vedi [Answering your questions on Hyperledger Fabric performance and scale](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

Tutti i contenitori associati al nodo hanno **CPU** e **memoria**, mentre alcuni contenitori associati a peer, ordinante e CA hanno anche **archiviazione**. Per ulteriori informazioni sull'archiviazione, vedi [Considerazioni sull'archiviazione persistente](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage).

Sei responsabile del monitoraggio del tuo consumo di CPU, memoria e archiviazione nel tuo cluster. Se si verifica una situazione in cui devi richiedere per un nodo blockchain più risorse di quante ne siano disponibili, il nodo non verrà avviato ma i nodi esistenti non ne risentiranno. Sei stai utilizzando {{site.data.keyword.cloud_notm}} come tuo provider cloud, CPU e memoria devono essere modificati utilizzando la console e il dashboard {{site.data.keyword.cloud_notm}} Kubernetes Service. Tuttavia, dopo che un nodo è stato creato, l'archiviazione può essere modificata in un secondo momento solo utilizzando la CLI {{site.data.keyword.cloud_notm}}. Per informazioni su come aumentare la CPU, la memoria e l'archiviazione in altri provider cloud, consulta la documentazione di quei provider cloud.
{:note}

Ogni nodo ha un proxy web gRPC che esegue il bootstrap del livello di comunicazione tra la console e un nodo. Questo contenitore ha dei valori di risorsa fissi ed è incluso nel pannello Allocazione risorse per fornire una stima accurata di quanto spazio è necessario sul tuo cluster Kubernetes per la distribuzione del nodo. Poiché i valori per questo contenitore non possono essere modificati, non tratteremo del proxy web gRPC nelle seguenti sezioni:

### Autorità di certificazione (CA, Certificate Authority)
{: #ibp-console-govern-CA}

A differenza di peer e ordinanti, che sono attivamente coinvolti nel processo di transazione, le CA sono coinvolte solo nella registrazione e nell'iscrizione delle identità e nella creazione di un MSP. Questo significa che richiedono meno CPU e memoria. Per sottoporre a stress una CA, un utente dovrebbe sovraccaricarla di richieste (probabilmente utilizzando delle API o uno script) oppure la CA avrebbe dovuto emettere un quantitativo tale di certificati da esaurire l'archiviazione. In condizioni operative normali, non dovrebbe verificarsi nessuna di queste due situazioni sebbene, come sempre, questi valori dovrebbero riflettere le esigenze di uno specifico caso d'uso.

La CA ha solo un contenitore associato che possiamo regolare.

* **La CA stessa**: incapsula i processi CA interni, come la registrazione e l'iscrizione di nodi e utenti e l'archiviazione di una copia di ogni certificato che emette.

#### Dimensionamento di una CA durante la creazione
{: #ibp-console-govern-CA-sizing-creation}

La CA ha solo un singolo contenitore con valori di cui ti devi preoccupare perché i valori del server proxy web gRPC non possono essere modificati.

| Risorse | Condizione da aumentare |
|-----------------|-----------------------|
| **CPU e memoria del contenitore CA** | Quando prevedi che la tua CA sarà bombardata di registrazioni e iscrizioni. |
| **Archiviazione CA** | Quando intendi utilizzare questa CA per registrare un numero elevato di utenti e applicazioni. |

### Peer
{: #ibp-console-govern-peers}

Il peer ha tre contenitori associati che possiamo regolare:

- **Il peer stesso**: incapsula i processi peer interni (come la convalida delle transazioni) e la blockchain (in altre parole, la cronologia delle transazioni) per tutti i canali a cui appartiene. Nota: l'archiviazione del peer include anche gli smart contract installati sul peer.
- **CouchDB**: dove vengono archiviati i database dello stato del peer. Ricordati che ciascun canale ha un database dello stato distinto.
- **Smart contract**: ricordati che, durante una transazione, lo smart contract pertinente viene "richiamato" (in altre parole, eseguito). Nota: tutti gli smart contract che installi sul peer verranno eseguiti in un contenitore separato all'interno del contenitore smart contract, che è noto come contenitore Docker-in-Docker.  

Il peer include anche un contenitore per il **Log Collector** che incanala i log dal contenitore smart contract al contenitore peer. Analogamente al contenitore proxy web gRPC, non puoi regolare il calcolo per questo contenitore.

#### Dimensionamento di un peer durante la creazione
{: #ibp-console-govern-peers-sizing-creation}

Come detto nella nostra sezione relativa alla [Modalità di interazione della console con il tuo cluster Kubernetes](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction), ti consigliamo di utilizzare i valori predefiniti per questi contenitori peer e regolarli in un secondo momento quando diventa evidente come sono utilizzati dal tuo caso d'uso.

| Risorse | Condizione da aumentare |
|-----------------|-----------------------|
| **CPU e memoria del contenitore peer** | Quando prevedi un'immediata velocità effettiva elevata delle transazioni. |
| **Archiviazione peer** | Quando prevedi l'installazione di molti smart contract su questo peer e la sua unione a molti canali. Ricordati che questa archiviazione verrà utilizzata anche per archiviare gli smart contract da tutti i canali a cui viene unito il peer. Tenere presente che stimiamo che una transazione "piccola" sia compresa nell'intervallo di 10.000 byte (10k). Poiché l'archiviazione predefinita è 100G, significa che nell'archiviazione peer c'entreranno fino a 10 milioni di transazioni in totale prima che sia necessario espanderla (in pratica, il numero massimo sarà inferiore a questo poiché le transazioni possono variare per dimensione e il numero non include gli smart contract). Mentre 100G potrebbero pertanto sembrare molta più archiviazione di quanta ne occorra, tieni presente che l'archiviazione costa relativamente poco e che il processo per aumentarla è più difficile (richiede la riga di comando) che aumentare la CPU o la memoria. |
| **CPU e memoria del contenitore CouchDB** | Quando prevedi un elevato volume di query su un database dello stato di grandi dimensioni. Questo effetto può essere in qualche modo attenuato utilizzando gli [indici](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html#couchdb-indexes){: external}. Tuttavia, degli elevati volumi potrebbero sovraccaricare CouchDB, con dei conseguenti timeout di query e transazioni. |
| **Archiviazione CouchDB (dati del libro mastro)** | Quando prevedi una velocità effettiva elevata su molti canali e non prevedi di utilizzare gli indici. Tuttavia, come per l'archiviazione peer, l'archiviazione CouchDB predefinita è di 100GB, che è considerevole. |
| **CPU e memoria del contenitore smart contract** | Quando prevedi una velocità effettiva elevata su un canale, soprattutto nei casi in cui verranno richiamati più smart contract per volta.|

### Nodi di ordine
{: #ibp-console-govern-ordering-nodes}

Poiché i nodi di ordine non gestiscono il DB dello stato e non ospitano smart contract, hanno bisogno di meno contenitori rispetto ai peer. Ospitano però la blockchain (la cronologia delle transazioni) perché la blockchain è dove viene archiviata la configurazione dei canali e il servizio di ordine deve conoscere la configurazione dei canali più recente per svolgere il suo ruolo.

Analogamente alla CA, un nodo di ordine ha solo un singolo contenitore associato che possiamo regolare (se stai distribuendo un servizio di ordine a cinque nodi, cinque insiemi separati di contenitori di nodo di ordine):

* **Il nodo di ordine stesso**: incapsula i processi ordinante interni (come la convalida delle transazioni) e la blockchain per tutti i canali che ospita.

#### Dimensionamento di un ordinante durante la creazione
{: #ibp-console-govern-orderer-sizing-creation}

Come detto nella nostra sezione relativa al [Modo in cui {{site.data.keyword.cloud_notm}} Kubernetes Service interagisce con la console](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction), ti consigliamo di usare i valori predefiniti per questi contenitori ordinante e di regolarli in un secondo momento quando diventa evidente come verranno utilizzati.

| Risorse | Condizione da aumentare |
|-----------------|-----------------------|
| **CPU e memoria del contenitore nodo di ordine** | Quando prevedi un'immediata velocità effettiva elevata delle transazioni. |
| **Archiviazione del nodo di ordine** | Quando prevedi che questo nodo di ordine farà parte di un servizio di ordine su molti canali. Ricordati che il servizio di ordine conserva una copia della blockchain per ogni canale che ospita. L'archiviazione predefinita di un nodo di ordine è 100G, uguale al contenitore per il peer stesso. |

Assicurarti che i tuoi nodi di ordine dispongano di CPU e memoria sufficienti è considerata una prassi ottimale. Se un servizio di ordine è sottoposto a uno stress eccessivo, potrebbe raggiungere dei timeout e iniziare a eliminare le transazioni, rendendo necessario inoltrare nuovamente le transazioni. Ciò causa molto più danno a una rete di un singolo peer che fa fatica a tenere il passo. In una configurazione del servizio di ordine Raft, un nodo libro mastro sottoposto a stress eccessivo potrebbe smettere di inviare messaggi heartbeat, attivando una designazione del leader e una temporanea cessazione dell'ordinazione di transazioni. Allo stesso modo, un nodo follower potrebbe non rilevare dei messaggi e provare ad attivare una designazione del leader laddove non ne occorre uno.
{:important}

## Riallocazione di risorse
{: #ibp-console-govern-reallocate-resources}

Dopo il ridimensionamento di un nodo, potresti riscontrare un ritardo prima che diventi effettivo poiché i contenitori vengono ricreati.
{:important}

Come abbiamo detto in precedenza, se il tuo provider cloud è {{site.data.keyword.cloud_notm}}, ti consigliamo di utilizzare lo strumento [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} in combinazione con il tuo dashboard {{site.data.keyword.cloud_notm}} Kubernetes per monitorare il tuo utilizzo delle risorse Kubernetes. Se determini che un nodo di lavoro sta esaurendo le risorse, puoi aggiungere un nuovo nodo di lavoro più grande al tuo cluster ed eliminare quindi il nodo di lavoro esistente.
{:note}

Se da una parte ci vuole uno sforzo minore per distribuire risorse sufficienti al tuo cluster Kubernetes dall'inizio e potere pertanto distribuire ed espandere le risorse senza dover aumentare le risorse nel tuo cluster, dall'altra è vero che più grande è la distribuzione e più sarà costosa. Gli utenti devono valutare con attenzione le loro opzioni e riconoscere i compromessi che stanno facendo indipendentemente dall'opzione che scelgono.

Se il tuo provider cloud è {{site.data.keyword.cloud_notm}} o in un provider cloud diverso (utilizzando {{site.data.keyword.cloud_notm}} Private), puoi ridimensionare il tuo cluster manualmente, monitorando i tuoi nodi e aggiungendo più nodi o nodi più grandi. Se da una parte questo processo è più laborioso, dall'altra presenta il vantaggio di consentire all'utente di essere sempre sicuro di cosa gli viene addebitato sul suo account cloud.

Se un utente ha eseguito la distribuzione a {{site.data.keyword.cloud_notm}} utilizzando {{site.data.keyword.cloud_notm}} Kubernetes Service, può anche utilizzare l'**autoscaler** di {{site.data.keyword.cloud_notm}} Kubernetes Service. Il programma di ridimensionamento automatico ridimensionerà i tuoi nodi di lavoro ampliandoli o riducendoli in risposta alle tue impostazioni delle specifiche dei pod e alle tue richieste di risorse. Per ulteriori informazioni sull'autoscaler di {{site.data.keyword.cloud_notm}} Kubernetes Service e su come configurarlo, vedi [Ridimensionamento dei cluster](/docs/containers?topic=containers-ca#ca){: external} nella documentazione di {{site.data.keyword.cloud_notm}}. Nota: consentire al programma di ridimensionamento automatico di regolare le tue risorse comporterà degli addebiti sul tuo account del servizio {{site.data.keyword.cloud_notm}} Kubernetes che varieranno con il tuo utilizzo.

Per eseguire un ridimensionamento in modo manuale nella console, fai clic sul nodo che desideri regolare nella pagina **Nodi** e fai quindi clic sulla scheda **Utilizzo**. Puoi vedere un pulsante denominato **Rialloca**, che avvierà una scheda **Allocazione risorse** che è molto simile a quella che vedevi quando hai creato il nodo. Se desideri ridurre la quantità di risorse disponibili, ti basta fornire dei valori più bassi e fare clic su **Rialloca risorse** in tale scheda e nella pagina **Riepilogo** risultante.

Se desideri aumentare la CPU e la memoria per un nodo, utilizza la scheda **Allocazione risorse** nella console per aumentare i valori. La casella bianca in fondo alla pagina sommerà i nuovi valori. Dopo che hai fatto clic su **Rialloca risorse**, la pagina **Riepilogo** convertirà questo valore in una quantità di **VPC**, che viene utilizzata per calcolare la tua fattura. Devi quindi andare al tuo cluster Kubernetes per assicurarti che il tuo cluster disponga di risorse sufficienti per questa riallocazione. In caso affermativo, puoi fare clic su **Rialloca risorse**. Se non è disponibile una quantità sufficiente di risorse, dovrai aumentare le dimensioni del tuo cluster.

Il metodo che utilizzerai per aumentare l'archiviazione dipenderà dalla classe di archiviazione da te scelta per il tuo cluster. Fai riferimento alla documentazione del tuo provider cloud per saperne di più su questo argomento. In {{site.data.keyword.cloud_notm}}, questo argomento è denominato [opzioni di archiviazione](/docs/containers?topic=containers-kube_concepts#kube_concepts){: external}. Nota: in {{site.data.keyword.cloud_notm}}, se stai per esaurire l'archiviazione sul tuo peer od ordinante, devi distribuire un nuovo peer od ordinante con un file system più grande e consentirgli di eseguire la sincronizzazione tramite i tuoi altri componenti sugli stessi canali.

In {{site.data.keyword.cloud_notm}}, CPU e memoria possono essere aumentate utilizzando la console (se hai risorse disponibili nel tuo cluster {{site.data.keyword.cloud_notm}} Kubernetes Service). Tuttavia, l'archiviazione deve essere aumentata utilizzando la CLI {{site.data.keyword.cloud_notm}}. Per un'esercitazione su come eseguire questa operazione, vedi [Modifica della dimensione e dell'IOPS del tuo dispositivo di archiviazione esistente](/docs/containers?topic=containers-file_storage#file_change_storage_configuration){: external}. Se stai utilizzando un provider cloud diverso da {{site.data.keyword.cloud_notm}}, fai riferimento alla documentazione di tale provider per il processo relativo all'aumento di CPU, memoria e archiviazione.

## Aggiornamento di una definizione di MSP dell'organizzazione
{: #ibp-console-govern-update-msp}

Nel corso del tempo, potresti dover aggiornare una definizione di MSP dell'organizzazione, ad esempio aggiungendo degli ulteriori amministratori dell'organizzazione o modificando il nome di visualizzazione dell'MSP nella console.

- Ti basta esportare la definizione di MSP esistente dalla scheda **Organizzazioni** e modificare il file JSON generato per modificare il nome di visualizzazione, i certificati esistenti o per aggiungere dei nuovi certificati.Ti sconsigliamo di modificare il campo `msp_id` poiché ciò potrebbe causare modifiche che causano interruzioni alla tua rete.
- Fai clic sulla tua definizione di MSP nella scheda **Organizzazioni** per aprirla e fai quindi clic su **Aggiungi file** per caricare il nuovo file JSON.
- Fai clic su **Aggiorna definizione di MSP**. Le modifiche hanno effetto immediatamente.

## Aggiornamento di una configurazione del canale
{: #ibp-console-govern-update-channel}

Mentre la creazione e l'aggiornamento di un canale hanno lo stesso obiettivo, fornendo agli utenti la possibilità di assicurarsi che la configurazione dei propri canali sia il più adatta possibile al loro caso di utilizzo, i due processi sono di fatto **attività** molto diverse nella console. Ricorda che dalla nostra documentazione sulla [Creazione di un canale](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-create-channel) questo è un processo utilizzato da una **sola organizzazione**. Finché un'organizzazione fa parte del consorzio del servizio di ordine che diventerà il servizio di ordine di un canale, possono creare il canale in qualsiasi modo vogliano. Possono fornire qualsiasi nome, aggiungere qualsiasi organizzazione (purché siano membri del consorzio), assegnare le autorizzazioni delle organizzazioni, impostare gli elenchi di controllo dell'accesso e così via.

Le altre organizzazioni possono scegliere se partecipare a questo canale (ad esempio, se unire i peer ad esso), ma si presume che il processo collaborativo di scelta dei parametri del canale avvenga fuori banda, prima che un'organizzazione utilizzi la console per creare il canale.

L'aggiornamento di un canale è diverso. Si verifica **all'interno della console** e segue le procedure di governance collaborativa che sono fondamentali per la modalità di funzionamento di {{site.data.keyword.blockchainfull_notm}} Platform. Questo processo collaborativo implica l'invio delle richieste di aggiornamento della configurazione del canale alle organizzazioni che hanno un ruolo amministrativo nel canale. Queste organizzazioni sono note anche come **operatori** del canale.

Per aggiornare un canale, fai clic sul canale nella scheda **Canali**. Fai clic sul pulsante **Impostazioni** accanto al nome del canale all'inizio della pagina. Sarà visualizzato un pannello che è molto simile al pannello che utilizzi per creare un canale.

### Parametri di configurazione del canale che puoi aggiornare
{: #ibp-console-govern-update-channel-available-parameters}

È possibile modificare alcuni, ma non tutti, dei parametri di configurazione di un canale dopo che il canale è stato creato. Ed esiste solo un parametro che è possibile aggiornare e che non è disponibile durante la creazione del canale.

Vedrai che il **Nome del canale** è grigio e non può essere modificato. Questo rispecchia il fatto che un nome del canale non può essere modificato dopo essere stato creato. Inoltre, nota che il nome di visualizzazione del servizio di ordine non è presente, poiché anche il servizio di ordine di un canale non può essere modificato dopo la creazione del canale.

Puoi, tuttavia, modificare i seguenti parametri di configurazione del canale:

* **Organizzazioni**. Questa sezione del pannello rappresenta come le organizzazioni vengono aggiunte o rimosse da un canale. Le organizzazioni che possono venire aggiunte possono essere visualizzate nell'elenco a discesa. Tieni presente che un'organizzazione deve essere un membro del consorzio del servizio di ordine prima di poter essere aggiunta a un canale. Per ulteriori informazioni su come aggiungere un'organizzazione al consorzio, vedi [Aggiungere la propria organizzazione all'elenco di organizzazioni che possono eseguire transazioni](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-add-org).

* **Politica di aggiornamento canale**. La politica di aggiornamento di un canale specifica quante organizzazioni (rispetto al numero totale di organizzazioni nel canale), devono approvare un aggiornamento alla configurazione del canale. Per garantire un buon bilanciamento tra una gestione collaborativa e un'elaborazione efficiente degli aggiornamenti della configurazione del canale, prendi in considerazione di impostare questa politica su una maggioranza di amministratori. Ad esempio, se sono presenti cinque amministratori in un canale, scegli `3 out of 5`.

* **Parametri di taglio blocco**. (Opzione avanzata) Poiché una modifica ai parametri di taglio blocco deve essere firmata da un amministratore dell'organizzazione del servizio di ordine, questi campi non sono presenti nel pannello di creazione del canale. Tuttavia, poiché questa configurazione del canale sarà inviata a tutte le organizzazioni pertinenti nel canale, è possibile inviare una richiesta di aggiornamento della configurazione del canale con delle modifiche ai parametri di taglio blocco. Questi campi determinano le condizioni in cui il servizio di ordine taglia un nuovo blocco. Per informazioni su come questi campi influenzano il taglio dei blocchi, vedi [Parametri di taglio blocco](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-orderer-tuning-batch-size).

* **Elenchi di controllo dell'accesso**. (Opzione avanzata) Per specificare un controllo più dettagliato sulle risorse, puoi limitare l'accesso a una risorsa a un'organizzazione e a un ruolo all'interno di tale organizzazione. Ad esempio, impostando l'accesso alla risorsa `ChaincodeExists` su `Application/Admins` significa che solo l'amministratore di un'applicazione sarà in grado accedere alla risorsa `ChaincodeExists`.

Se si limita l'accesso a una risorsa a una determinata organizzazione, tenere presente che solo tale organizzazione sarà in grado di accedere alla risorsa. Se si desidera che altre organizzazioni siano in grado di accedere alla risorsa, sarà necessario aggiungerle una alla volta utilizzando i campi sottostanti. Di conseguenza, considerare attentamente le decisioni sul controllo degli accessi. Limitare l'accesso a specifiche risorse in determinati modi può avere un effetto altamente negativo sul funzionamento del canale.
{:important}

Poiché la console consente a un singolo utente di possedere e controllare diverse organizzazioni, è necessario specificare l'organizzazione che si sta utilizzando quando si firma un aggiornamento del canale nella sezione **Organizzazione utilità di aggiornamento canale**. Se gestisci più di un'organizzazione in questo canale, per la firma puoi scegliere una qualsiasi delle organizzazioni che gestisci nel canale. A seconda della **politica di aggiornamento del canale** che hai selezionato, puoi ricevere una notifica in cui ti viene richiesto di firmare la richiesta per una o più delle altre organizzazioni che gestisci.

Se stai tentando di modificare uno dei **parametri di taglio blocco** e gestisci l'organizzazione del servizio di ordine di questo canale, vedrai un campo per tale organizzazione. Seleziona l'MSP dell'organizzazione del servizio di ordine pertinente dall'elenco a discesa. Se non sei un amministratore dell'organizzazione del servizio di ordine, puoi ancora effettuare una richiesta per modificare uno dei parametri di taglio blocco, ma la richiesta sarà inviata e dovrà essere firmata da un altro amministratore del servizio di ordine.

### Flusso di raccolta delle firme
{: #ibp-console-govern-update-channel-signature-collection}

Per la verifica delle firme, le organizzazioni su un canale devono esportare gli MSP (in formato JSON) che rappresentano le proprie organizzazioni in altre organizzazioni sul canale, nonché importare gli MSP delle altre organizzazioni. Per esportare un MSP, fai clic sul pulsante di scaricamento sul tuo MSP (nella schermata **Organizzazioni**), poi invialo alle altre organizzazioni fuori banda. Quando ricevi un JSON di un MSP, importalo utilizzando la schermata **Organizzazioni**.
{: important}

Dopo aver effettuato una richiesta di aggiornamento della configurazione del canale, sarà inviata alle organizzazioni nel canale che hanno il diritto di firmarla. Ad esempio, se esistono cinque operatori (amministratori del canale) in un canale, sarà inviata a tutti e cinque. Per approvare un aggiornamento della configurazione del canale, il numero di operatori del canale elencato nella **politica di aggiornamento del canale** deve essere soddisfatto. Se la politica indica `3 out of 5`, l'aggiornamento della configurazione del canale sarà inviato a tutti e cinque gli operatori e quando tre di loro lo firmano, il nuovo aggiornamento della configurazione del canale diventerà effettivo.

Questo processo per conoscere quando è presente un aggiornamento da firmare, così come la firma, viene gestito tramite il pulsante **Notifiche** (che è simile a una campana) nel lato superiore destro della console. Quando vedi un punto blu sul pulsante **Notifiche**, indica che hai una richiesta in sospeso da valutare o che vieni avvisato di un evento di aggiornamento del canale.

Quando fai clic sul pulsante **Notifiche**, puoi eseguire una o più azioni:

* **Richiede attenzione**: l'utente corrente deve firmare la richiesta (come un'organizzazione del servizio di ordine o un peer) oppure deve inoltrare la richiesta (se tutte le firme richieste sono già state raccolte).
* **Aperte**: include tutto quanto **richiede attenzione** nonché le richieste che sono state firmate dall'utente ma che devono ancora essere firmate da uno o più degli altri membri del canale.
* **Chiuse**: le richieste sono state inoltrate. Nessuna azione da eseguire su questi elementi. Possono essere solo visualizzati.
* **Tutte**: include sia le richieste aperte che quelle chiuse.

Se è stata effettuata una richiesta di aggiornamento della configurazione del canale, hai la possibilità di fare clic su `Esaminare e aggiornare la configurazione del canale` e visualizzare le modifiche all'aggiornamento della configurazione del canale che vengono proposte o che sono state fatte (se la nuova configurazione del canale è stata approvata). Se sei un operatore sul canale e non sono state raccolte abbastanza firme per approvare la richiesta di aggiornamento della configurazione del canale, potrai firmare la richiesta di aggiornamento.

Non sei obbligato a firmare un aggiornamento della configurazione di un canale, tuttavia non c'è modo di specificare **di non essere d'accordo** con un aggiornamento del canale. Se non approvi un aggiornamento della configurazione di un canale, puoi semplicemente chiudere il pannello e contattare gli altri operatori del canale fuori banda per esprimere le tue preoccupazioni. Tuttavia, se abbastanza operatori nel canale approvano l'aggiornamento in modo da soddisfare la politica di aggiornamento del canale, la nuova configurazione diventerà effettiva.
{:note}

### Configurazione di peer di ancoraggio
{: #ibp-console-govern-channels-anchor-peers}

Poiché è necessario abilitare il [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} tra le organizzazioni perché il rilevamento dei servizi e i dati privati funzionino, deve esistere un peer di ancoraggio per ciascuna organizzazione. Questo peer di ancoraggio non è un **tipo** di peer speciale ma solo il peer che l'organizzazione rende noto alle altre organizzazioni e che esegue il bootstrap del gossip tra le organizzazioni. Pertanto, deve essere definito almeno un [peer di ancoraggio](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html#anchor-peers){: external} per ciascuna organizzazione nella definizione della raccolta.

Per configurare un peer in modo che sia un peer di ancoraggio, fai clic sulla scheda **Canali** e apri il canale dove era stato istanziato lo smart contract.
 - Fai clic sulla scheda **Dettagli del canale**.
 - Scorri verso il basso fino alla tabella Peer di ancoraggio e fai clic su **Aggiungi peer di ancoraggio**.
 - Seleziona almeno un peer da ciascuna organizzazione nella definizione della raccolta che desideri funga da peer di ancoraggio per l'organizzazione. Per motivi di ridondanza, puoi considerare la selezione di più di un peer da ciascuna organizzazione nella raccolta.

## Regolazione del tuo ordinante
{: #ibp-console-govern-orderer-tuning}

Le prestazioni di una piattaforma blockchain possono essere influenzate da molte variabili quali la dimensione delle transazioni, la dimensione dei blocchi, la dimensione della rete nonché dai limiti dell'hardware. Il nodo ordinante include un insieme di parametri di regolazione che, insieme, possono essere utilizzati per controllare la velocità effettiva e le prestazioni dell'ordinante.  Puoi utilizzare questi parametri per personalizzare il modo in cui il tuo ordinante elabora le transazioni, a seconda del fatto che tu abbia molte piccole transazioni frequenti o un numero inferiore di transazioni però di grandi dimensioni che arrivano con minore frequenza. Essenzialmente, hai il controllo per decidere quando vengono tagliati i blocchi sulla base della dimensione, della quantità e della frequenza di arrivo delle transazioni che specifichi.

I seguenti parametri sono disponibili nella console facendo clic sul nodo ordinante nella scheda **Nodi** e quindi facendo clic sulla relativa icona **Impostazioni**. Fai clic sul pulsante **Avanzate** per aprire la **Configurazione del canale avanzata** per l'ordinante.

### Parametri di taglio blocco
{: #ibp-console-govern-orderer-tuning-batch-size}

I tre parametri di seguito indicati lavorano insieme per controllare quando viene tagliato un blocco, in base a una combinazione dell'impostazione del numero massimo di transazioni in un blocco e della dimensione del blocco stessa.

- **Numero massimo di byte assoluto**  
  Imposta questo valore sulla dimensione di blocco più grande in byte che può essere tagliata dall'ordinante.  Nessuna transazione può essere più grande del valore di `Numero massimo di byte assoluto`. Di norma, questa impostazione può essere senza problemi da due a dieci volte più grande del tuo valore di `Numero massimo di byte preferito`.    
  **Nota**: la dimensione massima consentita è di 99MB.
- **Numero massimo di messaggi**   
  Imposta questo valore sul numero massimo di transazioni che può essere incluso in un singolo blocco.
- **Numero massimo di byte preferito**  
  Imposta questo valore sulla dimensione di blocco ideale in byte che deve però deve essere inferiore a `Numero massimo di byte assoluto`. Una dimensione minima della transazione, una che non contenga alcuna approvazione, è di circa 1KB.  Se aggiungi 1KB per ogni approvazione richiesta, una tipica dimensione della transazione è circa 3-4KB. Ti consigliamo pertanto di impostare il valore di `Numero massimo di byte preferito` più o meno su `Numero massimo di messaggi * dimensione transazione media prevista`. Al runtime, quando possibile, i blocchi non supereranno questa dimensione. Se arriva una transazione a causa della quale il blocco supera questa dimensione, il blocco viene tagliato e per tale transazione viene creato un nuovo blocco. Se però arriva una transazione che supera questo valore senza superare il valore di `Numero massimo di byte assoluto`, la transazione verrà inclusa. Se arriva un blocco che è più grande di `Numero massimo di byte preferito`, conterrà solo una singola transazione e tale dimensione della transazione non può essere più grande di `Numero massimo di byte assoluto`.

Insieme, questi parametri possono essere configurati per ottimizzare la velocità effettiva del tuo ordinante.

### Batch timeout
{: #ibp-console-govern-orderer-tuning-batch-timeout}

Imposta il valore **Timeout** sulla quantità di tempo, in secondi, per cui attendere, dopo l'arrivo della prima transazione, prima di tagliare il blocco. Se l'impostazione che specifichi per questo valore è troppo bassa, rischi di impedire che i batch riempiano la tua dimensione preferita. Se l'impostazione che specifichi per questo valore è troppo alta, l'ordinante potrebbe restare in attesa dei blocchi e le prestazioni generali potrebbero degradarsi. In generale, ti consigliamo di impostare il valore di `Batch timeout` in modo che sia almeno `numero massimo messaggi / numero massimo di transazioni al secondo`.

Quando modifichi questi parametri, non influisci sulla modalità di funzionamento dei canali esistenti sull'ordinante, piuttosto le eventuali modifiche che apporti alla configurazione dell'ordinante si applicano solo ai nuovi canali che crei su questo ordinante.
{:important}
