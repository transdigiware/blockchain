---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Risoluzione dei problemi
{: #ibp-v2-troubleshooting}

Si possono verificare dei problemi generali quando utilizzi la console per gestire i nodi, i canali o gli smart contract. In molti casi, puoi risolvere questi problemi seguendo pochi semplici passi.
{:shortdesc}

Questo argomento descrive i problemi comuni che si possono verificare quando utilizzi la console {{site.data.keyword.blockchainfull_notm}} Platform.

- [Quando passo il puntatore del mouse sul mio nodo, lo stato è `Status unavailable`, che significa?](#ibp-v2-troubleshooting-status-unavailable)
- [Quando passo il puntatore del mouse sul mio nodo, lo stato è `Status undetectable`, che significa?](#ibp-v2-troubleshooting-status-undetectable)
- [Perché le mie operazioni del nodo hanno esisto negativo dopo che creo il mio peer o servizio di ordinazione?](#ibp-console-build-network-troubleshoot-entry1)
- [Perché sto ricevendo l'errore `Unable to get system channel` quando apro il mio servizio di ordinazione?](#ibp-troubleshoot-ordering-service)
- [Perché il mio peer non si avvia?](#ibp-console-build-network-troubleshoot-entry2)
- [Perché l'installazione, l'iniziazione o l'upgrade del mio smart contract hanno esito negativo?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Perché lo smart contract che ho installato sul peer non è elencato nell'IU?](#ibp-console-build-network-troubleshoot-missing-sc)
- [Come posso visualizzare i miei log del contenitore dello smart contract?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Il mio canale, gli smart contract e le identità sono scomparsi dalla console. Come posso recuperarli?](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Perché sto ottenendo l'errore `Unable to authenticate with the enroll ID and secret you provided` quando creo una nuova definizione di MSP dell'organizzazione?](#ibp-v2-troubleshooting-create-msp)
- [Perché sto ricevendo l'errore `An error occurred when updating channel` mentre provo ad aggiungere un'organizzazione al mio canale?](#ibp-v2-troubleshooting-update-channel)
- [Il mio cluster {{site.data.keyword.cloud_notm}} Kubernetes è scaduto. Che significa?](#ibp-v2-troubleshooting-cluster-expired)
- [Perché le transazioni che invio da VS Code hanno esito negativo?](#ibp-v2-troubleshooting-anchor-peer)
- [Quando eseguo l'accesso alla mia console, perché ricevo un errore 401 Non autorizzato?](#ibp-v2-troubleshooting-console-401)
- [Perché i nodi che ho distribuito su {{site.data.keyword.cloud_notm}} Private non elaborano le transazioni e non superano i controlli di integrità?](#ibp-v2-troubleshooting-healthchecks)
- [Perché le mie transazioni hanno restituito un errore di politica di approvazione: la firma impostata non soddisfa la politica?](#ibp-v2-troubleshooting-endorsement-sig-failure)

## Quando passo il puntatore del mouse sul mio nodo, lo stato è `Status unavailable`, che significa?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

Lo stato del nodo nel tile per il mio nodo CA, peer o di ordinazione è grigio, il che indica che lo stato del nodo non è disponibile. Idealmente, quando passi il puntatore del mouse su qualsiasi nodo, il suo stato dovrebbe essere `Running`.
{: tsSymptoms}

Questo problema può verificarsi se il nodo è stato appena creato e il processo di distribuzione non è stato completato. Se il nodo è una CA, è probabile che non sia in esecuzione.
Se il nodo è un peer o un nodo di ordinazione, questa condizione si verifica quando il programma di controllo dell'integrità che viene eseguito sui nodi di ordinazione o peer non riesce a contattare il nodo.  La richiesta dello stato può non riuscire con un errore di timeout perché il nodo non ha risposto entro un certo periodo di tempo, il nodo potrebbe essere inattivo oppure la connettività di rete è inattiva.
{: tsCauses}

Se si tratta di un nuovo nodo, attendi qualche minuto che la distribuzione venga completata. Se non il nodo non è nuovo,
[esamina i log di nodo associati](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) per rilevare l'eventuale presenza di errori per determinare la causa.
{: tsResolve}

## Quando passo il puntatore del mouse sul mio nodo, lo stato è `Status undetectable`, che significa?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

Lo stato del nodo nel tile per il nodo peer o di ordinazione è giallo, il che indica che lo stato del nodo non può essere rilevato. Idealmente, quando passi il puntatore del mouse su qualsiasi nodo, il suo stato dovrebbe essere `Running`.
{: tsSymptoms}

Questa condizione si verifica solo sui nodi peer o di ordinazione che erano stati *importati* sulla console e su cui non può essere eseguito il programma di controllo dell'integrità. Questo stato si verifica perché non era stato specificato un `operations_url` quando è stato importato il nodo. Un url delle operazioni è necessario per l'esecuzione del programma di controllo dell'integrità del nodo. Il nodo stesso è probabilmente in esecuzione (`Running`) ma, poiché l'url delle operazioni non era stato specificato, non è possibile determinarne lo stato.
{: tsCauses}

Puoi risolvere questo problema attenendoti alla seguente procedura:
 1. Fai clic sul tile del nodo per aprirlo.
 2. Fai clic sull'icona **Impostazioni**.
 3. Fai clic su **Associa identità** e visualizza e annota l'identità associata a questo nodo. Fai clic su **Annulla** per chiudere il pannello.
 4. Fai clic su **Esporta** per generare un file `JSON` per il nodo.
 5. Modifica il file `JSON` generato e immetti l'url delle operazioni (`operations_url`) per il nodo. Il valore dell'url delle operazioni (`operations_url`) dipende da come era stato configurato e da diverse impostazioni di rete. Questo valore deve essere fornito dall'amministratore di rete che ha distribuito il nodo.
 6. Fai su **Elimina**. Questo passo rimuove il nodo importato dalla console ma non elimina il nodo effettivo.
 7. Dalla scheda **Nodi**, fai clic su **Aggiungi peer** o **Aggiungi servizio di ordinazione**, seguito da **Importa un peer esistente** o **Importa un servizio di ordinazione esistente**.
 8. Fai clic su **Carica JSON** e vai al file JSON che hai appena modificato. Fai clic su **Avanti**.
 9. Associa la stessa identità che avevi annotato al passo tre.
 10. Fai clic su **Aggiungi peer** o **Aggiungi servizio di ordinazione**.

Il programma di controllo dell'integrità può ora essere eseguito sul nodo e notificare lo stato del nodo.
{: tsResolve}

## Perché le mie operazioni del nodo hanno esisto negativo dopo che creo il mio peer o servizio di ordinazione?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

È possibile che tu stia riscontrando un errore durante la gestione di un nodo esistente. Quando questo si verifica, è spesso utile consultare i log del nodo.  

Ad esempio, quando provi a gestire il nodo, l'azione potrebbe non riuscire.
{: tsSymptoms}

Dopo aver creato un nuovo peer o servizio di ordinazione, a seconda della tua configurazione di archiviazione del cluster, potrebbero essere necessari alcuni minuti perché i nodi diventino operativi.
{: tsCauses}

Controlla il tuo dashboard Kubernetes e assicurati che lo stato del peer o del nodo sia `Running`. Poi riprova la tua azione. Se stai ancora riscontrando dei problemi dopo che il nodo diventa attivo, [controlla i tuoi log del nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) per degli errori.  
{: tsResolve}

## Perché sto ricevendo l'errore `Unable to get system channel` quando apro il mio servizio di ordinazione?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

Dopo aver creato un servizio di ordinazione nella tua console {{site.data.keyword.cloud_notm}} Private, lo stato è `Running`. Ma quando apri il servizio di ordinazione vedi l'errore:

```
Unable to get system channel. If you associated an identity without administrative privilege on the ordering service node,
you will not be able to view or manage ordering service details.
```

{: tsSymptoms}

Questa condizione si verifica nella console blockchain in esecuzione su {{site.data.keyword.cloud_notm}} Private. Su browser diversi da Chrome devi accettare un certificato in modo che la console comunichi correttamente con il nodo.
{: tsCauses}

Esistono diversi modi per risolvere questo problema:
1. Nelle tue note sulla release Helm, dove viene fornito il tuo URL browser della console, è presente anche una nota per andare a un URL e accettare il certificato. Vai a tale URL e accetta il certificato. Ora apri il tuo servizio di ordinazione. L'errore non dovrebbe più verificarsi.
2. Se puoi utilizzare un browser Chrome, apri la tua console blockchain in Chrome e apri il tuo servizio di ordinazione. L'errore non si verifica. Tieni presente che dovrai esportare le tue identità dal tuo portafoglio della console sui tuoi browser non Chrome e poi importarle nel portafoglio per continuare a lavorare.
{: tsResolve}

## Perché il mio peer non si avvia?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

È possibile che tu stia riscontrando questo errore per molti motivi.

Il log del peer include `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
{: tsSymptoms}

- Questo errore può verificarsi nelle seguenti condizioni:
  - Quando hai creato la definizione di MSP dell'organizzazione del peer o del servizio di ordinazione, hai specificato un segreto e un ID di registrazione che corrispondono a un'identità del tipo `peer` e non `client`. Deve essere del tipo `client`.
  - Quando hai creato la definizione di MSP dell'organizzazione del peer o del servizio di ordinazione, hai specificato un segreto e un ID di registrazione che non corrispondono a quelli dell'identità amministratore dell'organizzazione corrispondente.
  - Quando hai creato il peer o il servizio di ordinazione, hai specificato il segreto e l'ID di registrazione di un'identità che non è del tipo 'peer'.

- Apri il tuo nodo CA del peer o del servizio di ordinazione e visualizza le identità registrate elencate nella tabella **Utenti registrati**.
- Elimina il peer o il servizio di ordinazione e ricrealo, stando attento a specificare il segreto e l'ID di registrazione corretti.
- Tieni presente che prima di creare il peer o il servizio di ordinazione, devi creare un ID di amministratore dell'organizzazione, di tipo 'client'. Assicurati di specificare lo stesso ID di quello di registrazione quando crei la definizione di MSP dell'organizzazione. vedi queste istruzioni per [registrare le identità del peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) e queste istruzioni per [registrare le identità dell'ordinante](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## Perché l'installazione, l'iniziazione o l'upgrade del mio smart contract hanno esito negativo?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

È possibile che tu stia riscontrando un errore durante l'installazione, l'iniziazione o l'upgrade di uno smart contract.  Ad esempio, quanto tenti di installare uno smart contract su un peer, hai il seguente errore `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

Puoi ricevere questo errore se questa versione dello smart contract esiste già sul peer o se il tuo peer ha esaurito le risorse.
{: tsCauses}

- Apri il tuo dashboard Kubernetes e assicurati che lo stato del peer sia `Running`.  
- Apri il nodo del peer e assicurati che lo smart contract non esista già sul peer e riprova con la versione corretta.
- Se stai ancora riscontrando dei problemi dopo che il nodo diventa attivo, [controlla i tuoi log del nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) per degli errori.  
{: tsResolve}

## Perché lo smart contract che ho installato sul peer non è elencato nell'IU? 
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

Uno smart contract è stato installato su un peer ma quando fai clic sulla scheda **Smart contract**, non viene elencato.
{: tsSymptoms}

Questo problema può verificarsi quando un altro utente o applicazione installa lo smart contract nel peer e tu non disponi dell'identità di amministratore peer nel tuo portafoglio browser.
{: tsCauses}

Per visualizzare gli smart contract installati su un peer, devi essere un amministratore peer. Assicurati che l'identità di amministratore peer sia presente nel tuo portafoglio browser. In caso contrario, devi importarla nel tuo portafoglio della console.
{: tsResolve}

## Come posso visualizzare i miei log del contenitore dello smart contract?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

Potresti dover visualizzare i tuo log del contenitore dello smart contract o del chaincode per eseguire il debug di un problema con lo smart contract.
{: tsSymptoms}

Segui queste istruzioni per visualizzare i tuoi log del contenitore dello smart contract su:
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs).
{: tsResolve}

## Il mio canale, gli smart contract e le identità sono scomparsi dalla console. Come posso recuperarli?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

Le tue identità del portafoglio della console sono costituite da un certificato di firma e una chiave privata che ti permettono di gestire i tuoi componenti blockchain ma essi vengono archiviati solo nella tua memoria locale del browser. Sei responsabile per la protezione e la gestione di queste identità. Ti consigliamo di esportarle nel tuo file system dopo averle create. Se crei un nuovo nodo, associa un'identità dal tuo portafoglio della console al nodo. Questa identità amministratore è quella che ti consente di gestire il nodo. Quando passi da un browser a un altro o a un browser su una macchina differente, queste identità non sono più presenti nel tuo portafoglio. Pertanto, non puoi gestire i componenti.
{: tsSymptoms}

Una delle nuove funzioni di {{site.data.keyword.blockchainfull_notm}} Platform è che ora sei responsabile della protezione e della gestione dei tuoi certificati. Pertanto, vengono conservati solo nella memoria locale del browser per consentirti di gestire il componente. Se stai utilizzando una finestra del browser privata e passi a un altro browser o a una finestra del browser non privata, le identità che hai creato saranno scomparse dal portafoglio della console nella nuova sessione del browser. Pertanto, è necessario che esporti al tuo file system le identità dal portafoglio della console nella tua sessione del browser privata. Puoi quindi importarle nella tua sessione del browser non privata, se occorrono. Altrimenti, non c'è modo di recuperarle.
{: tsCauses}

- Ogni volta che crei una nuova definizione di MSP dell'organizzazione, generi delle chiavi per un'identità a cui è consentito gestire l'organizzazione. Pertanto, durante tale processo devi fare clic sui pulsanti **Genera** e **Esporta** per archiviare l'identità generata nel tuo portafoglio della console e salvarla nel tuo file system come un file JSON.
- Per risolvere questo problema nel tuo browser, devi importare tali identità e associarle al nodo corrispondente:
  - Nel browser in cui stai riscontrando il problema, fai clic sulla scheda **Portafoglio** e poi su **Aggiungi identità** per importare il file JSON nel tuo portafoglio.
  - Fai clic su **Carica JSON** e seleziona il file JSON che hai esportato utilizzando il pulsante **Aggiungi file**.
  - Fai clic su **Invia**.
  - Ora apri il nodo del peer o del servizio di ordinazione a cui era stata in origine associata questa identità e fai clic sull'icona **Impostazioni**.
  - Fai clic sul pulsante **Associa identità**.
  - Seleziona l'identità che hai appena importato nel tuo portafoglio della console dall'elenco a discesa.
  - Fai clic su **Associa**.
- Ripeti questo processo per ogni identità presente nel portafoglio del browser originale.
{: tsResolve}

## Perché sto ottenendo l'errore `Unable to authenticate with the enroll ID and secret you provided` quando creo una nuova definizione di MSP dell'organizzazione?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

Quando provi a creare una nuova definizione di MSP dell'organizzazione dalla scheda Organizzazioni, riscontri l'errore `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

Questo errore si verifica quando il valore che hai specificato per il segreto di registrazione non è valido per l'ID di registrazione che hai selezionato nella sezione `Generate organization admin certificate` del pannello.
{: tsCauses}

Verifica di aver selezionato l'ID di registrazione dell'amministratore dell'organizzazione corretto dall'elenco a discesa degli ID di registrazione e immetti il valore corretto per il segreto di registrazione.
{: tsResolve}

## Perché sto ricevendo l'errore `An error occurred when updating channel` mentre provo ad aggiungere un'organizzazione al mio canale?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

Quando tenti di aggiungere un'altra organizzazione a un canale, l'aggiornamento non riesce con il messaggio `An error occurred when updating channel`.
{: tsSymptoms}

Questo errore si verifica quando l'**ID MSP del programma di aggiornamento del canale** selezionato sul pannello **Aggiorna canale** non è un amministratore del canale.
{: tsCauses}

Sul pannello **Aggiorna canale**, scorri in basso fino all'**ID MSP del programma di aggiornamento del canale** e seleziona l'ID MSP che era stato specificato durante la creazione del canale o specifica l'ID MSP che è l'amministratore del canale.
{: tsResolve}

## Il mio cluster {{site.data.keyword.cloud_notm}} Kubernetes è scaduto. Cosa significa?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

Ho ricevuto un'email che mi informa che il mio cluster {{site.data.keyword.IBM_notm}} Kubernetes Service sta per scadere o che il suo stato è `Expired`. Oppure, non sei in grado di accedere alla console dopo 30 giorni.
{: tsSymptoms}

I cluster Kubernetes gratuiti sono validi solo per 30 giorni.
{: tsCauses}

Non è possibile eseguire la migrazione da un cluster gratuito a un cluster a pagamento. Dopo 30 giorni, non potrai accedere alla console e tutti i tuoi nodi e certificati verranno eliminati. Vedi questo argomento relativo alla [scadenza dei cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration) per informazioni su cosa sta accadendo e cosa puoi fare.
{: tsResolve}

## Perché le transazioni che invio da VS Code hanno esito negativo?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Le transazioni inviate da VS Code hanno esito negativo con un errore simile a:
```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```
{: tsSymptoms}

Questo errore si verifica se stai utilizzando la funzione di rilevamento dei servizi (Service Discovery) Fabric ma non hai configurato alcun peer di ancoraggio sul tuo canale.
{: tsCauses}

Segui il passo tre dell'[argomento sui dati privati](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) nell'esercitazione sulla distribuzione di uno smart contract per configurare i tuoi peer di ancoraggio.

## Quando eseguo l'accesso alla mia console, perché ricevo un errore 401 Non autorizzato?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

Quando provo ad eseguire l'accesso alla mia console, non sono in grado di accedere alla console dal mio browser. Se controllo i log del browser, posso trovare l'errore 401 Non autorizzato.
{: tsSymptoms}

La tua sessione della console del browser va in timeout dopo **8 ore** di inattività. Se una sessione diventa inattiva, la console impedirà all'utente non attivo di eseguire azioni.
{: tsCauses}

Se la tua sessione è diventata inattiva, puoi provare ad aggiornare semplicemente il tuo browser. Se non funziona, chiudi il browser, comprese **tutte** le schede e le finestre. Apri nuovamente l'URL, Ti verrà richiesto di eseguire l'accesso.

Come prassi ottimale, dovresti avere già archiviato i tuoi certificati e le tue identità sul tuo file system. Se ti capita di utilizzare una finestra in incognito, tutti i certificati vengono eliminati dall'archiviazione locale del browser quando chiudi il browser. Dopo aver eseguito nuovamente l'accesso, dovrai reimportare le tue identità e i tuoi certificati.
{: note}

## Perché i nodi che ho distribuito su {{site.data.keyword.cloud_notm}} Private non elaborano le transazioni e non superano i controlli di integrità?
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

La mia console indica che i miei peer e nodi di ordinazione sono ancora in esecuzione. Tuttavia, le mie transazioni hanno esito negativo. Quando eseguo un controllo di attività o di disponibilità sui pod del nodo, il controllo indica che i pod non sono integri.
{: tsSymptoms}

Se hai distribuito, rimosso e aggiornato i nodi sul tuo cluster molte volte, forse come risultato del test, Docker potrebbe avere esito negativo a causa di un problema noto con {{site.data.keyword.cloud_notm}} Private. Per ulteriori informazioni, vedi questo problema nella [documentazione di {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}.
{: tsCauses}

Per risolvere questo problema, rimuovi i pod in errore e ridistribuisci i tuoi nodi. Puoi anche riavviare il servizio Docker sul cluster.

## Perché le mie transazioni hanno restituito un errore di politica di approvazione: la firma impostata non soddisfa la politica? 
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

Quando richiamo uno smart contract per inviare una transazione, la transazione restituisce il seguente errore di politica di approvazione:
```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```
{: tsSymptoms}

Se hai recentemente unito un canale e installato lo smart contract, questo errore si verifica se non hai aggiunto la tua organizzazione alla politica di approvazione. Poiché la tua organizzazione non è nell'elenco di organizzazioni che possono approvare una transazione dallo smart contract, l'approvazione dai tuoi peer viene rifiutata dal canale. Se riscontri questo problema, puoi modificare la politica di approvazione eseguendo l'upgrade dello smart contract. Per ulteriori informazioni, vedi [Specifica di una politica di approvazione](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) e [Upgrade di uno smart contract](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}
