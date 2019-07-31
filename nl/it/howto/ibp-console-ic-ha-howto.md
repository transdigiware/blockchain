---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# Configurazione delle distribuzioni con alta disponibilità (HA) multiregione
{: #ibp-console-hadr-mr}

La configurazione HA multiregione fornisce il livello più elevato di copertura HA possibile. La distribuzione dei peer tra più regioni geografiche garantisce che se una qualsiasi regione diventa indisponibile, i peer nelle altre regioni possono continuare a scambiare transazioni. Tieni presente che il supporto HA multiregione per le CA e il servizio di ordinazione non è al momento disponibile.

## Panoramica
{: #ibp-console-hadr-overview}

Quando configuri il supporto HA multiregione per i peer, effettuerai le seguenti attività:
- Crea più istanze del servizio in {{site.data.keyword.cloud}}, ognuna di esse associata a un cluster Kubernetes in una regione diversa.
- Crea i tuoi nodi blockchain in regioni diverse.
- Utilizza la funzionalità esporta/importa nodo per gestire i nodi da una sola console.

## Procedura di configurazione
{: #ibp-console-hadr-config}

Per configurare l'HA multiregione creando dei peer ridondanti per ogni organizzazione, completa la seguente procedura quando configuri la tua rete blockchain:

1. Crea tre cluster {{site.data.keyword.cloud_notm}} Kubernetes nelle regioni che preferisci.Questi cluster possono essere ubicati in qualsiasi regione vuoi, anche se per prestazioni elevate dovrebbero essere relativamente vicini tra loro.Ad esempio, la scelta delle regioni, Stati Uniti Costa Est, Stati Uniti Costa Ovest e Canada è migliore rispetto a quella di Stati Uniti Costa Ovest, Londra e Tokyo.
2. Distribuisci una nuova istanza di {{site.data.keyword.blockchainfull_notm}} Platform e collegala al cluster nella prima regione. Quindi distribuisci un'altra istanza di {{site.data.keyword.blockchainfull_notm}} Platform e collegala al cluster nella seconda regione. Ripeti questa procedura per collegare una terza istanza del servizio al cluster nella terza regione. Quando hai terminato, disponi di tre istanze di {{site.data.keyword.blockchainfull_notm}} Platform separate collegate a tre cluster separati, ognuno in una regione diversa e tre console separate.

Questa esercitazione presuppone che esiste un servizio di ordinazione con un canale definito a cui possono unirsi i peer.
{: important}

### Passo uno: crea i metadati e la CA dell'organizzazione peer nel cluster uno
{: #ibp-console-hadr-peerCA}

1. Distribuisci una CA nel primo cluster seguendo le istruzioni nell'esercitazione Crea una rete per la [Creazione della CA della tua organizzazione del peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA). Prendi nota dei valori **ID di registrazione** e **segreto** della CA perché li devi fornire quando importi la CA in altri cluster.
2. Dopo che l'indicatore dello stato del tile della CA diventa verde, `Running`, segui le istruzioni per [Registrare le identità con la tua CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). In queste istruzioni, crea due identità, una per il peer e un'altra per l'identità amministratore dell'organizzazione del peer.
3. [Crea la definizione MSP dell'organizzazione del peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) per l'organizzazione del peer nel primo cluster.
4. [Crea un peer](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) nel primo cluster.
5. [Installa il tuo smart contract](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) sul tuo peer.

Prima di poter istanziare lo smart contract sul canale devi seguire questa procedura per unire il peer al canale sul servizio di ordinazione:
- [Esporta la definizione dell'organizzazione del peer](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) e condividila con un amministratore del servizio di ordinazione.
- L'amministratore del servizio di ordinazione deve seguire la procedura per [importare l'organizzazione del peer](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) e aggiungerla al consorzio. Dovrà inoltre completare la procedura in queste istruzioni per esportare il servizio di ordinazione in un file JSON.
- [Importa il file JSON del servizio di ordinazione](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) nella tua console.
- [Unisci il tuo peer al canale](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Infine, se utilizzi il rilevamento dei servizi, i dati privati e le comunicazioni tra peer mediante gossip, l'amministratore del servizio di ordinazione deve [configurare i peer di ancoraggio](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) sul canale. Per l'HA, ti consigliamo di aggiungere ogni peer ridondante come un peer di ancoraggio. In questo modo se uno dei peer non è disponibile, la comunicazione mediante gossip tra le organizzazioni può continuare.   

Ora che hai unito il peer al canale, puoi [istanziare lo smart contract](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) sul canale.

### Passo due: esporta i metadati e le identità dal cluster uno
{: #ibp-console-hadr-export-meta1}

1. Esporta la definizione CA in un file JSON.
   - Apri la CA nella scheda **Nodi**.
   - Fai clic sull'icona di scaricamento per generare il file JSON della CA dalla tua sessione del browser.
2. Esporta la definizione MSP dell'organizzazione del peer in un file JSON.
   - Passa alla definizione MSP nella scheda **Organizzazioni**.
   - Fai clic sull'icona di scaricamento sul tile.
3. Esporta l'identità amministratore dell'organizzazione del peer dal tuo portafoglio.
   - Passa alla scheda **Portafoglio**.
   - Fai clic sull'identità amministratore dell'organizzazione del peer e quindi su **Esporta identità**.
   - Viene creato un file JSON che contiene i certificati dell'amministratore dell'organizzazione. Prendi nota del nome del file e proteggilo, perché è necessario quando crei ulteriori peer nei tuoi altri cluster.

### Passo tre: importa i metadati e le identità nei cluster due e tre
{: #ibp-console-hadr-import-meta23}

1. Nei cluster due e tre, [importa il file JSON della CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca) che hai esportato dal cluster uno.  
2. Dopo aver caricato il file JSON, devi immettere il segreto e l'ID di registrazione dell'amministratore della CA e quelli dell'amministratore della CA TLS che hai utilizzato quando hai distribuito la CA al cluster uno.
2. Importa il file JSON della definizione MSP dell'organizzazione del peer che hai esportato dal cluster uno.
   - Nella scheda **Organizzazioni** fai clic su **Importa definizione MSP**.
   - Fai clic su **Aggiungi file** per caricare il file JSON.
   - Fai clic sulla casella di spunta **Dispongo di un'identità amministratore per la definizione MSP** per consentirti di utilizzare questa definizione MSP quando crei dei nuovi peer in altre zone.
   - Fai clic su **Importa definizione MSP**.
3. Importa il file JSON dell'identità amministratore dell'organizzazione che hai esportato dal cluster uno nel portafoglio della console sui cluster due e tre.

### Passo quattro: crea dei nuovi peer nei cluster due e tre e uniscili a un canale
{: #ibp-console-hadr-create-new-peers}

1. Nei cluster due e tre, utilizza la CA per registrare un nuovo utente peer, seguendo la stessa procedura utilizzata quando hai registrato l'identità peer del cluster uno. Devi solo creare gli utenti peer, non devi ricreare l'identità amministratore dell'organizzazione perché la importerai nel passo successivo.
2. Crea dei nuovi peer, utilizzando la CA che hai importato dal cluster uno come la CA del peer e utilizzando l'MSP dell'organizzazione del peer che hai importato dal cluster uno come la definizione MSP dell'organizzazione del peer.
3. Nei cluster due e tre, puoi ora ripetere la procedura eseguita per unire i peer allo stesso canale del peer nel cluster uno.
4. Dopo aver installato lo smart contract su questi peer ridondanti, il libro mastro sarà automaticamente sincronizzato tra tutti i peer.
5. Nuovamente, se pensi di utilizzare il rilevamento dei servizi, i dati privati e le comunicazioni tra peer tramite gossip, devi rendere ogni peer ridondante un peer di ancoraggio.  

La tua rete è ora configurata in modo tale che un errore in una singola regione non influenzerà il carico di lavoro del peer.  

Per ottimizzare ulteriormente la tua HA, prendi in considerazione le seguenti ulteriori opzioni:
- Esporta i peer che hai creato nei cluster due e tre e importali nella console nel cluster uno. Dopodiché può essere tutto gestito da un solo cluster.
- Tuttavia, il cluster uno può avere un malfunzionamento. Pertanto, per tutelarti maggiormente nel caso di un malfunzionamento del cluster, puoi importare tutti i tuoi peer in ognuna delle tre console. Quindi se il cluster che contiene una delle console ha un malfunzionamento, ogni chiamata può ancora essere gestita dalle console negli altri due cluster.
