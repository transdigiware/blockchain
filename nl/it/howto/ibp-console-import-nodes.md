---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Importazione di nodi
{: #ibp-console-import-nodes}

La console include l'opzione per importare i nodi creati utilizzando un'altra console {{site.data.keyword.blockchainfull}} Platform.
{:shortdesc}  

**Gruppi di destinatari:** questo argomento è pensato per gli operatori di rete che sono responsabili della creazione, del monitoraggio e della gestione della rete blockchain.

## Perché importare un nodo?
{: #ibp-console-import-nodes-why}

Importa le CA, i servizi di ordine e i peer da altre console {{site.data.keyword.blockchainfull_notm}} Platform quando devi gestirli insieme a nodi già presenti nella tua console. Ad esempio, puoi utilizzare l'opzione per importare i peer se vuoi unire i peer di organizzazioni esterne alla console ai canali nella tua console. Poiché i servizi {{site.data.keyword.blockchainfull_notm}} Platform vengono distribuiti in più ambienti, è probabile che alcune istanze del servizio includano solo CA e peer, mentre altre ospitano i servizi di ordine. L'importazione dei vari nodi nella console offre un modo per visualizzare e gestire questi componenti distribuiti da un'unica interfaccia utente in modo che possano effettuare transazioni insieme sulla blockchain.

Dopo che hai importato i nodi nella console, diventa disponibile una solida serie di funzionalità operative che ti consentono ad esempio di:
- Aggiungere nuove organizzazioni al consorzio
- Creare nuovi canali
- Unire i peer ai canali
- Installare smart contract sui peer indipendentemente da dove sono stati distribuiti
- Istanziare gli smart contract sui canali
- Aggiornare gli smart contract sui canali

Quando importi un nodo, devi fornire le relative informazioni di connessione. Quando importi un componente, non importi effettivamente il componente fisico stesso; vengono utilizzate invece le informazioni di connessione per creare una rappresentazione del componente nella console in modo che possa essere gestito dalla console. Allo stesso modo, quando elimini un nodo importato dalla console, il nodo stesso, che è ancora in esecuzione nell'ubicazione in cui è stato distribuito, non viene eliminato. Viene piuttosto rimosso solo dalla console dove era stato importato.  

Dopo aver importato un nodo nella console, puoi anche modificare le sue informazioni di connessione utilizzando la scheda **Impostazioni** del nodo.

Prima di poter importare i nodi nella console, devono essere esportati dalla console {{site.data.keyword.blockchainfull_notm}} Platform in cui sono stati creati. L'operatore di rete può semplicemente esportare le informazioni sul nodo dalla console in un file `JSON`.
{: note}

## Limitazioni
{: #ibp-console-import-limitations}

- Non puoi importare i nodi da reti del piano Starter o Enterprise.
- Tutti i nodi da importare devono essere stati distribuiti utilizzando la console {{site.data.keyword.blockchainfull_notm}} Platform.

## Inizia da qui: raccolta di certificati o credenziali
{: #ibp-console-import-start-here}

Prima di poter importare un componente blockchain esistente da un'altra istanza del servizio {{site.data.keyword.blockchainfull_notm}} Platform, devi importare l'identità di amministratore del componente nel tuo portafoglio della console. L'importazione dell'identità nel portafoglio della console semplifica l'operazione del nodo.  L'operatore di rete in cui è stato distribuito il nodo deve esportare l'identità di amministratore del nodo dal portafoglio a un file JSON che puoi [importare](#ibp-console-import-nodes-admin-identities).  Tipicamente, l'identità di amministratore del nodo è l'organizzazione peer od ordinante che era stata specificata quando è stata creata la definizione di MSP dell'organizzazione. Dovrebbe già essere presente nel portafoglio della console in cui risiede il nodo.

### Importazione delle identità di amministratore nel portafoglio della console
{: #ibp-console-import-nodes-admin-identities}

Ogni componente di {{site.data.keyword.blockchainfull_notm}} Platform viene distribuito con il relativo certificato di firma, il certificato di firma, di un amministratore del componente al suo interno. Quando l'amministratore effettua azioni sul componente, questo certificato di firma viene allegato alla transazione e convalidato rispetto alla configurazione del componente. Le chiavi consentono all'amministratore di svolgere attività di gestione sui propri componenti, come la registrazione di nuovi utenti, la creazione di un nuovo canale o l'installazione di smart contract sui peer. Se vuoi utilizzare la console per gestire un servizio di ordine o un peer creato utilizzando un'altra console, devi caricare l'identità di amministratore associata nel portafoglio della console. Quindi, puoi associare il nodo importato con l'identità nel portafoglio della console. Queste identità devono essere esportate dalla console in cui sono state create e sono necessarie quando il nodo viene importato.

Per importare una nuova identità, apri la scheda **Portafoglio** e fai clic su **Aggiungi identità**. Fai clic su **Carica JSON** per cercare il file di identità JSON esportato dall'operatore di rete da dove era stato creato il nodo.

Dopo aver completato il pannello **Aggiungi identità** e fatto clic su Invia, puoi visualizzare la nuova identità di amministratore nella schermata di panoramica del portafoglio. Puoi ora fare riferimento a queste identità quando importi i componenti CA, peer o servizio di ordine.

## Importazione di una CA
{: #ibp-console-import-ca}

Un nodo CA è il componente blockchain che emette i certificati per tutte le identità della rete (peer, servizi di ordine, client e così via) in modo che queste entità possano comunicare, autenticarsi e infine effettuare transazioni. Ogni organizzazione ha la propria CA che funge da radice di attendibilità. Devi aggiungere le tue organizzazioni se ti unisci o crei un consorzio blockchain. Puoi scoprire di più sulle CA nella [panoramica dei componenti blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-ca).  

Esistono diversi motivi per cui potresti voler importare una CA nella tua console. Dopo che hai importato la CA, puoi utilizzarla per registrare altri utenti peer all'organizzazione peer facendo clic su **Registra utente**. Oppure, puoi utilizzare la CA per creare ulteriori identità di amministratore per un peer o un servizio di ordine.

Per importare una CA nella console {{site.data.keyword.blockchainfull_notm}} Platform e gestirla, l'operatore di rete deve aver già esportato la CA da {{site.data.keyword.blockchainfull_notm}} Platform dove ne era stata eseguita la distribuzione. L'importazione di una CA ti consente di registrare nuovi utenti e [iscrivere le identità](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-enroll).

### Prima di cominciare
{: #ibp-console-import-ca-before-you-begin}

- Assicurati di avere già importato il file JSON dell'identità di amministratore della CA nel tuo portafoglio della console o di disporre dell'ID e del segreto di registrazione dell'amministratore sia per la CA che per la CA TLS che erano state specificate quando la CA è stata originariamente distribuita.
- Assicurati che il file JSON della CA esportato dalla console in cui è stato creato sia disponibile.

### Come importare una CA  
{: #ibp-console-import-nodes-howto-ca}

L'importazione di una CA viene eseguita dalla scheda **Nodi**.
1. Fai clic su **Aggiungi CA (Certificate Authority)**, seguito da **Importa una CA (Certificate Authority) esistente**, e fai clic su **Avanti**.
2. Seleziona l'ubicazione dove era stata originariamente distribuita la CA dall'elenco a discesa **Ubicazione della CA (Certificate Authority)**.
3. Fai clic su **Aggiungi file** per caricare il file JSON della CA che era stato esportato dalla console dove ne era stata originariamente eseguita la distribuzione.
4. Nel pannello successivo, puoi immettere l'ID e il segreto di registrazione di cui si è fatto uso quando è stata distribuita la CA.
5. Per finire, immetti l'ID e il segreto di registrazione per la CA TLS che era stata utilizzata quando è stata distribuita la CA. Quando distribuisci una CA, vengono distribuite insieme sia una CA che una CA TLS. L'operatore di rete può utilizzare gli stessi ID e segreto di registrazione per entrambe oppure può specificare degli ID e dei segreti di registrazione univoci per la CA e la CA TLS quando viene distribuita una CA.

Dopo aver importato la CA nella console, puoi utilizzare la CA per creare nuove identità e generare i certificati necessari per gestire i componenti e inviare transazioni alla rete. Per ulteriori informazioni, vedi [Gestione delle autorità di certificazione (CA)](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-manage-ca).

## Importazione di un servizio di ordine
{: #ibp-console-import-orderer}

Un servizio di ordine è il componente blockchain che raccoglie le transazioni dai membri della rete, ordina le transazioni e le raggruppa in blocchi. È l'associazione comune dei consorzi blockchain e deve essere distribuito se stai costituendo un consorzio a cui si uniranno altre organizzazioni. Puoi scoprire di più sui servizi di ordine nella [panoramica dei componenti blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-orderer).

L'importazione di un servizio di ordine nella console ti consente di creare nuovi canali in modo che i peer possano effettuare transazioni private.

### Prima di cominciare
{: #ibp-console-import-orderer-before-you-begin}

Prima che tu possa importare un servizio di ordine, devi raccogliere le seguenti informazioni:

- Assicurati di avere già [importato il file JSON di identità dell'amministratore del servizio di ordine](#ibp-console-import-nodes-admin-identities) nel tuo portafoglio della console.
- Assicurati che il file JSON del servizio di ordine esportato dalla console in cui è stato creato sia disponibile.

### Come importare un servizio di ordine
L'importazione di un servizio di ordine viene eseguita dalla scheda **Nodi**.
1. Fai clic su **Aggiungi servizio di ordine**, seguito da **Importa un servizio di ordine esistente** e fai clic su **Avanti**.
2. Seleziona l'ubicazione dove era stato originariamente distribuito il servizio di ordine dall'elenco a discesa **Ubicazione della CA (Certificate Authority)**.
3. Fai clic su **Aggiungi file** per caricare il file JSON del servizio di ordine che era stato esportato dalla console dove ne era stata originariamente eseguita la distribuzione. Si noti che, se si trattava di un servizio di ordine Raft a cinque nodi, dovresti avere un singolo file con le informazioni di connessione per tutti e cinque i nodi di ordine nel file.
4. Imposta l'identità di amministratore per il servizio di ordine facendo clic su **Identità esistente** e selezionando l'identità di amministratore del servizio di ordine che avevi importato nel tuo portafoglio della console.

Dopo aver importato il servizio di ordine nella console, puoi aggiungere nuovi membri dell'organizzazione e selezionare il servizio di ordine quando crei nuovi canali.

## Importazione di un peer
{: #ibp-console-import-peer}

Un nodo peer è il componente blockchain che gestisce un libro mastro e che esegue uno smart contract per effettuare operazioni di query e aggiornamento sul libro mastro. I membri dell'organizzazione possiedono e gestiscono i peer.  Ogni organizzazione che si unisce a un consorzio deve distribuire almeno un peer e un minimo di due per l'alta disponibilità (HA, High Availability). Puoi scoprire di più sui peer nella [panoramica dei componenti blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-peer).

Dopo aver importato un peer nella console, puoi installare gli smart contract sul peer e unire il peer ad altri canali nella tua blockchain.

**Nota:** se hai bisogno di aggiungere altri peer all'organizzazione del peer o di creare ulteriori identità di amministratore per un peer, devi importare la CA del peer e quindi utilizzare tale CA per effettuare queste operazioni.

### Prima di cominciare
{: #ibp-console-import-peer-before-you-begin}

Prima che tu possa importare un peer, devi raccogliere le seguenti informazioni:

- Assicurati di avere già [importato il file JSON di identità dell'amministratore del peer](#ibp-console-import-nodes-admin-identities) nel tuo portafoglio della console.
- Assicurati che il file JSON del peer esportato dalla console in cui è stato creato sia disponibile.

### Come importare un peer
{: #ibp-console-import-peer-howto}

L'importazione di un peer viene eseguita dalla scheda **Nodi**.
1. Fai clic su **Aggiungi peer**, seguito da **Importa un peer esistente** e fai quindi clic su **Avanti**.
2. Seleziona l'ubicazione dove era stato originariamente distribuito il peer dall'elenco a discesa **Ubicazione della CA (Certificate Authority)**.
3. Fai clic su **Aggiungi file** per caricare il file JSON del peer che era stato esportato dalla console dove ne era stata originariamente eseguita la distribuzione.
4. Imposta l'identità di amministratore per il peer facendo clic su **Identità esistente** e selezionando l'identità di amministratore del peer che avevi importato nel tuo portafoglio della console.  

Dopo aver importato il peer nella console, puoi installare gli smart contract sul peer e unire il peer ai canali nella tua blockchain.

## Importazione di una definizione di MSP dell'organizzazione
{: #ibp-console-import-msp}

Devi importare la definizione di MSP di un'organizzazione se l'MSP era stato distribuito utilizzando un'altra console dell'istanza del servizio {{site.data.keyword.blockchainfull_notm}} Platform e intendi creare o aggiornare un canale che utilizza tale definizione di MSP. Inoltre, se intendi creare un peer o un servizio di ordine che fa parte di un MSP dell'organizzazione che era stato distribuito in un'altra console, devi importare l'MSP e l'identità di amministratore dell'MSP associata. L'operatore di rete che ha creato l'MSP deve esportare la definizione di MSP dell'organizzazione e l'identità di amministratore corrispondente in file JSON e condividerli con te.

- Se intendi creare un peer o un servizio di ordine che fa parte dell'organizzazione, importa l'identità di amministratore dell'MSP associata nel tuo portafoglio.
- l'importazione della definizione di MSP dell'organizzazione viene eseguita dalla scheda **Organizzazioni**.
- Fai clic su **Importa definizione di MSP** per caricare il file JSON
- (Facoltativo) Se hai importato l'identità di amministratore MSP nel tuo portafoglio, selezionala casella di spunta `I have an administrator identity for the MSP definition`. Se non selezioni questa opzione, quando successivamente provi a creare un peer o un servizio di ordine, questa definizione di MSP dell'organizzazione non sarà elencata nell'elenco a discesa dell'MSP.
