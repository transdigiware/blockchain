---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# Distribuzione della console {{site.data.keyword.blockchainfull_notm}}
{: #console-deploy-icp}

Utilizza le seguenti istruzioni per distribuire la console {{site.data.keyword.blockchainfull}} Platform sul tuo cluster {{site.data.keyword.cloud_notm}} Private locale dopo che hai installato il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

## Risorse richieste
{: ##console-deploy-icp-resources-required}

Assicurati che il tuo sistema {{site.data.keyword.cloud_notm}} Private soddisfi i requisiti di risorse hardware minimi per la console e i componenti che crei.

| **Componente** (tutti i contenitori) | CPU  | Memoria (GB) | Archiviazione (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console** | 1,3 | 2,5 | 10 |
| **Peer** | 1,2 | 2,4 | 200 (include 100GB per peer e 100GB per CouchDB)|
| **CA** | 0,1 | 0,2  | 20 |
| **Ordinante** | 0,45 | 0,9 | 100 |

 **Note:**
 - Una CPU virtuale è un core virtuale assegnato a una macchina virtuale o un core di processore fisico se il server non è partizionato per le macchine virtuali. Devi considerare i requisiti di CPU virtuale quando decidi il VPC (virtual processor core) per la tua distribuzione in {{site.data.keyword.cloud_notm}} Private. VPC è un'unità di misura per determinare il costo di licenza dei prodotti {{site.data.keyword.IBM_notm}}. Per ulteriori informazioni sugli scenari per decidere il VPC, vedi [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Una vCPU è equivalente a una CPU, che è equivalente a un VPC.

## Archiviazione
{: #console-deploy-icp-storage}

Assicurati di aver creato una storageClass con una quantità sufficiente di archiviazione di supporto per i tuoi componenti di console e blockchain.

- Devi creare una nuova classe di archiviazione con risorse sufficienti per la tua console e i componenti che crei. La classe di archiviazione che fornisci alla console durante la configurazione verrà utilizzata anche per archiviare i tuoi dati di componente.
- Se utilizzi le impostazioni predefinite, il grafico Helm creerà una nuova attestazione di volume persistente (PVC, Persistent Volume Claim) con il nome della release Helm per i tuoi dati di console.
- Se non viene utilizzato il provisioning dinamico, i [volumi persistenti](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} devono essere creati e configurati con etichette che possono essere utilizzate per perfezionare il processo di bind dell'attestazione di volume persistente (PVC, Persistent Volume Claim).

## Prerequisiti per la distribuzione della console
{: #console-deploy-icp-prerequisites}

1. Prima di poter distribuire la console {{site.data.keyword.blockchainfull_notm}} Platform, devi [installare {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup) e [installare il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-helm-install.html#console_helm-install).

2. Devi creare un nuovo spazio dei nomi personalizzato per la tua distribuzione di {{site.data.keyword.blockchainfull_notm}} Platform. Il tuo spazio di nomi deve utilizzare la [PodSecurityPolicy richiesta](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install-prereqs-pod-security-requirements). Se intendi creare più reti blockchain, ad esempio per creare diversi ambienti per la distribuzione, la preparazione e la produzione, devi creare uno spazio dei nomi univoco per ogni ambiente.

3. Richiama il valore dell'indirizzo IP proxy del cluster della tua CA dalla console {{site.data.keyword.cloud_notm}} Private. **Nota:** dovrai essere un [amministratore del cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} per accedere al tuo IP proxy. Accedi al cluster {{site.data.keyword.cloud_notm}} Private. Nel pannello di navigazione di sinistra, fai clic su **Piattaforma** e quindi su **Nodi** per visualizzare i nodi definiti nel cluster. Fai clic sul nodo con il ruolo `proxy` e copia il valore dell'`IP host` dalla tabella. **Importante:** salva questo valore perché lo utilizzerai quando configuri il campo `Proxy IP` del grafico Helm.


Salva questo valore perché lo utilizzerai quando configuri il campo `Proxy IP` del grafico Helm.
{:important}

4. Crea una password che utilizzerai per accedere alla console per la prima volta e memorizzala in un oggetto di segreto in {{site.data.keyword.cloud_notm}} Private. Puoi trovare le istruzioni per creare il segreto nella [prossima sezione](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp-password-secret).

## Creazione di un segreto della password della console
{: #console-deploy-icp-password-secret}

Prima di poter accedere alla tua console, devi creare una password predefinita che utilizzerai per eseguire l'accesso alla console per la prima volta. Crea un [segreto Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/){: external} per memorizzare la password prima di distribuire la console, Un segreto Kubernetes ti consente di proteggere e condividere le informazioni senza doverle passare direttamente alla distribuzione.

1. Crea una password e codificala nel formato base64. Esegui il seguente comando in una finestra del terminale e sostituisci il valore di `password` con il valore che vuoi utilizzare.**Salva l'output di questo comando**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Accedi alla console {{site.data.keyword.cloud_notm}} Private. Nel pannello di navigazione di sinistra, fai clic su **Configurazione** e quindi su **Segreti**. Fai clic sul pulsante **Crea segreto** per aprire un pannello a comparsa che ti consente di generare un nuovo oggetto di segreto.

3. Nella scheda **Generale**, completa i seguenti campi:
  - **Nome:** fornisci al tuo segreto un nome univoco all'interno del tuo cluster. Utilizzerai questo nome quando distribuisci la tua console. Il nome deve essere tutto in minuscolo.
  - **Spazio dei nomi:** lo spazio dei nomi per aggiungere il tuo segreto. Seleziona lo `spazio dei nomi` in cui vuoi distribuire la tua console.
  - **Tipo:** immetti il valore `generic`.

4. Lascia vuota la scheda **Annotazioni**.

5. Nella scheda **Dati**, aggiungi il nome utente e la password come coppie chiave/valore.
  1. Nel primo campo **Nome**, immetti `password`.
  2. Nel primo campo **Valore**, immetti il risultato di `echo -n 'password' | base64` dal passo 1.
  3. Fai clic su **Crea** per creare un novo oggetto di segreto.

Fornisci il nome del segreto per il campo `Console administrator password secret name` della pagina di configurazione quando distribuisci la tua console. Dovrai utilizzare il valore che hai codificato nel passo 1 per eseguire l'accesso alla console per la prima volta. Questo valore diventerà la password predefinita della console, a meno che non venga modificato da un amministratore della console. Per ulteriori informazioni, vedi [Gestione degli utenti dalla console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Creazione del segreto TLS (facoltativo)
{: #console-deploy-icp-tls-secret}

La console utilizza TLS per proteggere le comunicazioni tra la tua console e i tuoi nodi blockchain. Per impostazione predefinita, la console genera i propri certificati TLS. Puoi tuttavia fornire un tuo certificato TLS e una tua chiave TLS. Utilizza la procedura riportata di seguito per memorizzare i certificati in un [segreto Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/). Puoi quindi fornire questi certificati alla console fornendo il nome del segreto per il campo relativo al segreto TLS durante la configurazione.

1. Converti il certificato e la chiave privata TLS in formato base64. Puoi convertire il contenuto del certificato o del file chiave in una stringa base64 eseguendo questo comando sulla tua macchina locale:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  Esegui questo comando sia per il tuo certificato TLS sia per la chiave TLS a corredo. **Salva** l'output.

2. Accedi alla console {{site.data.keyword.cloud_notm}} Private. Nel pannello di navigazione di sinistra, fai clic su **Configurazione** e quindi su **Segreti**. Fai clic sul pulsante **Crea segreto** per aprire un pannello a comparsa che ti consente di generare un nuovo oggetto di segreto.

3. Nella scheda **Generale**, completa i seguenti campi:
  - **Nome:** fornisci al tuo segreto un nome univoco all'interno del tuo cluster. Utilizzerai questo nome per configurare la tua CA. Il nome deve essere tutto in minuscolo.
  - **Spazio dei nomi:** lo spazio dei nomi per aggiungere il tuo segreto. Seleziona lo spazio dei nomi (`namespace`) a cui vuoi distribuire la tua CA.
  - **Tipo:** immetti il valore `kubernetes.io/tls`.

4. Lascia vuota la scheda **Annotazioni**.

5. Nella scheda **Dati**, aggiungi il nome utente e la password come coppie chiave/valore.
  1. Nel primo campo **Nome**, immetti `tls.crt`.
  2. Nel primo campo **Valore**, immetti la stringa del tuo certificato TLS con codifica base64 dal passo 1.
  3. Fai clic sul pulsante **Aggiungi dati** per aggiungere una seconda coppia chiave/valore.
  4. Nel primo campo **Nome**, immetti `tls.key`.
  5. Nel primo campo **Valore**, immetti la stringa della tua chiave TLS con codifica base64 dal passo 1.
  6. Fai clic su **Crea** per creare un novo oggetto di segreto.

Fornisci il nome del segreto che hai creato per il campo `TLS secret` nella sezione Tutti i parametri della pagina di configurazione quando distribuisci la tua console.

I segreti non vengono rimossi dal cluster {{site.data.keyword.cloud_notm}} Private quando elimini la tua release Helm. Sei responsabile della gestione di segreti TLS e password nel tuo cluster {{site.data.keyword.cloud_notm}} Private. Se intendi distribuire un'altra console in futuro, puoi riutilizzare i segreti. Altrimenti, sei responsabile della loro eliminazione dal tuo cluster {{site.data.keyword.cloud_notm}} Private.
{:note}

## Configurazione
{: #console-deploy-icp-configuration}

Dopo che hai creato l'oggetto di segreto TLS, puoi distribuire la console attenendoti alla seguente procedura:

1. Esegui l'accesso alla console {{site.data.keyword.cloud_notm}} Private e fai clic su **Catalogo** nell'angolo superiore destro.
2. Fai clic su `Blockchain` nel pannello di navigazione di sinistra e individua il tile etichettato `ibm-blockchain-platform-prod`. Fai clic sul tile per aprirlo. Dovresti vedere un file Readme che include le informazioni sull'installazione e sulla configurazione del grafico Helm.
3. Fai clic sulla scheda **Configurazione** nella parte superiore del pannello oppure fai clic sul pulsante **Configura** nell'angolo inferiore destro.
4. Specifica i valori per i [Parametri di configurazione e sicurezza del pod](#icp-peer-deploy-configuration-parms) e accetta l'accordo di licenza.
5. Passa alla sezione **Parametri**.
- Puoi distribuire la console utilizzando solo i [Parametri Quickstart](#icp-peer-deploy-quickstart-parms). Utilizza questa opzione se stai facendo delle prove o iniziando ora a utilizzare il prodotto.
- Puoi utilizzare la sezione [Tutti i parametri](#icp-peer-deploy-quickstart-parms) per personalizzare l'accesso di rete, le risorse e l'archiviazione utilizzati dalla tua console. La sezione **Tutti i parametri** è consigliata solo per gli utenti Kubernetes più esperti.
6. Fai clic su **Installa**.

Le seguenti tabelle descrivono i campi dei parametri di configurazione e i loro valori predefiniti:

### Parametri di configurazione e sicurezza del pod
{: #icp-peer-deploy-configuration-parms}

|  Parametro     | Descrizione    | Valore predefinito  | Obbligatorio |
| --------------|-----------------|-------|------- |
| `Helm release name`| Il nome per identificare la tua distribuzione della release helm. Inizia con una lettera minuscola, termina con un qualsiasi carattere alfanumerico e deve contenere solo trattini e caratteri alfanumerici minuscoli. | Nessuno | Sì |
| `Target namespace`| Specifica lo spazio dei nomi che hai creato per la tua console e i tuoi componenti. Lo spazio dei nomi deve includere una politica `ibm-privileged-psp`. Altrimenti, [associa una politica di sicurezza del pod](https://cloud.ibm.com/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) al tuo spazio dei nomi. | Nessuno | Sì |
| `Target Cluster`| Specifica i cluster a cui desideri distribuire la risorsa. I cluster disponibili sono filtrati in base ai requisiti di spazio dei nomi e versione Kubernetes selezionati. | Nessuno | Sì |
| `Target namespace policies`| Preconfigurato per utilizzare il tuo spazio dei nomi di destinazione. I valori devono includere la politica `ibm-privileged-psp` o quella `ibm-blockchain-platform-psp`. | Nessuno | Sì |

### Quickstart e Tutti i parametri
{: #icp-peer-deploy-quickstart-parms}

Utilizza la sezione Quickstart se stai facendo delle prove o iniziando ora a utilizzare il prodotto. La sezione Tutti i parametri ti consente di personalizzare l'accesso alla rete, le risorse e l'archiviazione ed è consigliata solo per gli utenti Kubernetes più esperti.

**Fai clic sulla scheda Tutti i parametri per visualizzare tutti i parametri di configurazione.**

|  Parametro     | Descrizione    | Valore predefinito  | Obbligatorio |
| --------------|-----------------|-------|------- |
| **Impostazioni della console** | **Amministrazione e distribuzione della tua console** | | |
| `Proxy IP` | L'indirizzo IP del nodo proxy del tuo cluster. | Nessuno| Sì |
| `Console administrator email` | L'email utilizzata per eseguire l'accesso alla console. | Nessuno | Sì |
| `Console administrator password secret name` | Il nome del segreto che hai [creato per memorizzare la password](#console-deploy-icp-password-secret) che utilizzerai per eseguire l'accesso alla console. | Nessuno | Sì|
| **Impostazioni di rete** | **Accesso di rete alla tua console** | | |
| `Console hostname` | Immetti lo stesso valore dell'IP proxy. | Nessuno | Sì |
| `Console port` | Immetti la porta che desideri utilizzare nell'intervallo 31210 - 31220. Questa porta non può essere utilizzata da un'altra applicazione. | Nessuno | Sì |
| **Impostazioni dell'archiviazione** | **Archiviazione che deve essere utilizzata dalla tua console** | | |
| `Storage class name` | Immetti il nome della classe di archiviazione che deve essere utilizzata dalla console e dai componenti da te creati. | Nessuno | Sì |
{: caption="Tabella 1. Parametri Quickstart" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Parametro     | Descrizione    | Valore predefinito  | Obbligatorio |
| --------------|-----------------|-------|------- |
| `Architecture` | Seleziona l'architettura della piattaforma cloud. (AMD64 o S390x) | AMD64 | No |
| **Impostazioni della console** | **Amministrazione e distribuzione della tua console** | | |
| `Service account name` | L'account di servizio che deve essere utilizzato dall'operatore. | Nessuno | No |
| `Proxy IP` | L'indirizzo IP del nodo proxy nel tuo cluster. | Nessuno | Sì|
| `Console administrator email` | L'email utilizzata per eseguire l'accesso alla console.  | Nessuno | Sì |
| `Console administrator password secret name` | Il nome del segreto che hai [creato per memorizzare la password](#console-deploy-icp-password-secret) che utilizzerai per eseguire l'accesso alla console. | Nessuno | Sì|
| **Impostazioni dell'immagine Docker** | **Utilizza queste impostazioni per personalizzare le immagini Fabric di cui deve essere eseguito il pull dalla console** | | |
| `imagePullSecret name` | imagePullSecret che deve essere utilizzato per scaricare le immagini. | `ibp-ibmregistry` | No |
| **Impostazioni di rete** | **Accesso di rete alla tua console** | | |
| `Console hostname` | Immetti lo stesso valore dell'IP proxy. | Nessuno | Sì |
| `Console port` | Immetti qualsiasi porta che desideri utilizzare nell'intervallo 31210 - 31220. | Nessuno | Sì |
| `Proxy hostname` | Il nome host del server proxy. Immetti lo stesso valore dell'IP proxy. | Nessuno | No |
| `Proxy port` | Immetti qualsiasi porta che desideri utilizzare nell'intervallo 31210 - 31220. Questa porta non può essere utilizzata da un'altra applicazione o dalla console. | Nessuno | No |
| `Configtxlator hostname` | Immetti lo stesso valore dell'IP proxy. | Nessuno | No |
| `Configtxlator port` | Immetti qualsiasi porta che desideri utilizzare nell'intervallo 31210 - 31220. Questa porta non può essere utilizzata da un'altra applicazione o dalla console. | Nessuno | No |
| `TLS secret` | Il nome del segreto che hai [creato per memorizzare i certificati TLS](#console-deploy-icp-tls-secret) che verranno utilizzati dalla console. | Nessuno | No |
| **Persistenza dei dati**| **Abilita la persistenza dei dati e il provisioning dinamico** | | |
| `Enable data persistence`| Opera questa selezione per abilitare la capacità di rendere persistenti i dati dopo i riavvii o gli errori del cluster. Per ulteriori informazioni, vedi [la sezione relativa all'archiviazione in Kubernetes](https://kubernetes.io/docs/concepts/storage/). *Se non è impostata su true, tutti i dati andranno perduti quando si verifica un failover o un riavvio del pod.* | Abilitata | No |
| `Enable dynamic provisioning`| Seleziona per abilitare il provisioning dinamico dei tuoi volumi di archiviazione. | abilitata | No |
| **Impostazioni dell'archiviazione**| **Esegui il provisioning di archiviazione per la tua console e i tuoi strumenti** | | |
| `Volume claim size`| La dimensione dell'attestazione di volume persistenza (PVC; Persistent Volume Claim) di cui deve essere eseguito il provisioning. | 10Gi  | No |
| `Storage class name`| Il nome della classe di archiviazione che deve essere utilizzata dalla console e dai componenti da te creati. | Nessuno | Sì |
| `Existing volume claim`| Specifica il nome di un'attestazione di volume esistente e lascia vuoti tutti gli altri campi. | Nessuno | No |
| `Selector label`| [Etichetta del selettore](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) per la tua PVC| Nessuno | No |
| `Selector value`| [Valore del selettore](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) per la tua PVC | Nessuno | No |
| `Storage access mode`| Specifica la [modalità di accesso](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes){: external} dell'archiviazione per la PVC. | ReadWriteMany | No |
| `PVC name`| Solo per la nuova attestazione. Immetti un nome per la tua nuova PVC. | Impostato di default sul nome della release. | No |
| **Alloca risorse**| **Alloca risorse alla console** | | |
| `Opstools CPU limit` | Il numero massimo di CPU da allocare al componente opstools. | 500m | No |
| `Opstools memory limit` | La quantità massima di memoria da allocare al componente opstools. | 1000Mi | No |
| `Opstools CPU request` | Il numero minimo di CPU da allocare al componente opstools. | 500m | No |
| `Opstools memory request` | La quantità minima di memoria da allocare al componente opstools.| 1000Mi | No |
| `Configtxlator CPU limit` | Il numero massimo di CPU da allocare allo strumento configtxlator. | 25m | No |
| `Configtxlator memory limit` | La quantità massima di memoria da allocare allo strumento configtxlator. | 50Mi | No |
| `Configtxlator CPU request` | Il numero minimo di CPU da allocare allo strumento configtxlator. | 25m | No |
| `Configtxlator memory request` | La quantità minima di memoria da allocare allo strumento configtxlator. | 50Mi | No |
| `CouchDB CPU limit` | Il numero massimo di CPU da allocare a CouchDB. | 500m | No |
| `CouchDB memory limit` | La quantità massima di memoria da allocare a CouchDB. | 1000Mi | No |
| `CouchDB CPU request` | Il numero minimo di CPU da allocare a CouchDB.| 500m | 500m |
| `CouchDB memory request` | La quantità minima di memoria da allocare a CouchDB.| 1000Mi | No |
| `Operator CPU limit` | Il numero massimo di CPU da allocare al componente operator. | 100m | No |
| `Operator memory limit` | La quantità massima di memoria da allocare al componente operator. | 200Mi | No |
| `Operator CPU request` | Il numero minimo di CPU da allocare al componente operator. | 100m | No |
| `Operator memory request` | La quantità minima di memoria da allocare al componente operator. | 200Mi | No |
| `Deployer CPU limit` | Il numero massimo di CPU da allocare al componente deployer. | 100m | No |
| `Deployer memory limit` | La quantità massima di memoria da allocare al componente deployer. | 200Mi | No |
| `Deployer CPU request` | Il numero minimo di CPU da allocare al componente deployer. | 100m | No |
| `Deployer memory request` | La quantità minima di memoria da allocare al componente deployer. | 200Mi | No |
{: caption="Tabella 2. Tutti i parametri" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Utilizzo della riga di comando Helm per installare la release Helm
{: #console-deploy-icp-helm-cli}

In alternativa, puoi utilizzare la CLI `helm` per installare la release Helm. Prima di eseguire il comando `helm install`, assicurati di [aggiungere il repository Helm del tuo cluster all'ambiente della CLI Helm](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}.

Puoi configurare i parametri necessari per l'installazione creando un file `yaml` e passandolo al seguente comando `helm install`.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Dove:
- `<helm_release name>` rappresenta il nome che vuoi dare alla tua release Helm.
- `<helm_chart>` rappresenta il nome del grafico Helm importato nel catalogo.
- `<helm_chart_version>` rappresenta la versione del grafico Helm importato nel catalogo.
- `<customvalues.yaml>` è il nome del file yaml che contiene i parametri di configurazione.

Ad esempio:
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

Puoi creare un nuovo file `yaml` modificando il `values.yaml` incluso nel file di archivio scaricato, che include tutti i parametri necessari con le rispettive impostazioni.

## Verifica dell'installazione

Una volta che hai terminato il tuo lavoro con i parametri di configurazione, fai clic sul pulsante **Installa** e sul pulsante **Visualizza release Helm** per visualizzare la tua distribuzione. Se l'azione è stata eseguita correttamente, dovresti vedere il valore 1 nei campi `DESIRED`, `CURRENT`, `UP TO DATE` e `AVAILABLE` nella tabella Distribuzione. Potresti dover fare clic su Aggiorna e attendere l'aggiornamento della tabella.

Visualizzi i dettagli della tua distribuzione passando alla schermata di panoramica **Distribuzione** e facendo clic sul pod che era stato creato. La distribuzione del grafico Helm crea cinque contenitori sul tuo cluster:
- **opstools**: l'IU della console.
- **deployer**: uno strumento che consente alla tua console di comunicare con le tue distribuzioni.
- **configtxlator**: uno strumento utilizzato dalla console per leggere e creare gli aggiornamenti di canale.
- **couchdb**: un'istanza di CouchDB che archivia i dati dalla tua console, incluse le tue informazioni di autorizzazione.
- **operator**: un operatore Kubernetes che gestisce le tue distribuzioni di componenti.

## Accesso alla console

Puoi utilizzare il tuo browser per accedere alla console dopo l'installazione. Puoi trovare l'URL nella sezione **Note:** della schermata di panoramica della release di Helm che si apre dopo la distribuzione. Controlla per assicurati che non stai utilizzando la versione ESR di Firefox. In questo caso, passa a un altro browser, come ad esempio Chrome, e riprova.

Nel tuo browser, dovresti essere in grado di visualizzare la schermata di accesso della console:
- Per l'**ID utente**, utilizza il valore che avevi fornito per il campo `Console administrator email` durante la configurazione.
- Per la **Password**, utilizza il valore che hai codificato e memorizzato nel [segreto della password](#console-deploy-icp-password-secret) e quindi passato alla console durante la configurazione. Questa password diventerà quella predefinita per la console che tutti i nuovi utenti utilizzano per eseguire l'accesso ad essa. Dopo che hai eseguito l'accesso per la prima volta, ti verrà chiesto di fornire una nuova password che puoi utilizzare per eseguire l'accesso alla console.

L'amministratore che ha eseguito il provisioning del grafico Helm può concedere ad altri utenti l'accesso alla console e specificare quali operazioni possono eseguire. Per ulteriori informazioni, vedi [Gestione degli utenti dalla console](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage-users).

## Passi successivi
{: #console-deploy-icp-next-steps}

Dopo aver eseguito l'accesso alla tua console, puoi visualizzare la scheda **nodi** della tua IU della console. Puoi utilizzare questa schermata per distribuire i componenti nel tuo cluster locale. Consulta l'[esercitazione relativa alla creazione di una rete](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) per un'introduzione all'utilizzo della console. Puoi anche utilizzare questa scheda per gestire i nodi che sono stati creati su altri cloud. Per ulteriori informazioni, vedi [Importazione di nodi](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).

Per ulteriori informazioni su come gestire gli utenti che possono accedere alla console, utilizzare le API {{site.data.keyword.blockchainfull_notm}} Platform e visualizzare i log dei tuoi componenti di console e blockchain, consulta il documento relativo all'[amministrazione della tua console su {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage).
