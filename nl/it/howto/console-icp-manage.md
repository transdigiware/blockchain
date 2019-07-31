---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# Amministrazione della tua console
{: #console-icp-manage}

Dopo che hai distribuito la tua console su {{site.data.keyword.cloud}} Private, puoi utilizzarla per aggiungere o rimuovere utenti della console e per accedere alle API che ti consentono di gestire la tua rete e controllare la tua console. Puoi anche accedere ai log della tua console e personalizzarli.
{:shortdesc}

## Gestione degli utenti dalla console
{: #console-icp-manage-users}

L'utente che esegue il provisioning della console {{site.data.keyword.blockchainfull_notm}} Platform ne è considerato l'amministratore. L'amministratore può quindi aggiungere e concedere ad altri utenti l'accesso alla console utilizzando la scheda **Utenti** nella console. A ogni utente che accede alla console deve essere assegnata una politica di accesso con un ruolo utente definito. La politica determina quali azioni può eseguire l'utente all'interno della console. Per impostazione predefinita, all'amministratore della console viene dato il ruolo di **Gestore** per la console. Agli altri utenti possono essere assegnati i ruoli di **Gestore**, **Scrittore** o **Lettore** quando un gestore della console li aggiunge alla console. Nota che gli utenti possono anche essere gestiti con le [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis).

| Ruolo | Funzionalità |
|--------|----------|
| manager (gestore) | In qualità di Gestore, disponi di autorizzazioni al di sopra del ruolo di Scrittore. Puoi fare tutto quello che fa un Lettore e uno Scrittore e puoi inoltre: <ul><li>Eseguire il provisioning dei nuovi componenti utilizzando la console o le API.</li><li>Eliminare i componenti di cui è stato eseguito il provisioning utilizzando la console o le API.</li><li>Aggiungi/rimuovi gli utenti e modifica le politiche di accesso utente.</li><li>Modificare i livelli di registrazione della console utilizzando la console o le API.</li><li>Riavviare la console utilizzando un'API.</li></ul> |
| Scrittore | In qualità di Scrittore, disponi di autorizzazioni che vanno oltre il ruolo di Lettore, tra cui: <ul><li>Importare i componenti utilizzando la console o le API.</li><li>Rimuovere i componenti importati utilizzando la console o le API.</li><li>Registrare gli utenti su una CA.</li><li> Aggiungere o rimuovere le notifiche utilizzando la console o le API.</li></ul>  |
| Lettore | In qualità di Lettore, puoi eseguire azioni di sola lettura, tra cui: <ul><li>Visualizzare l'IU della console.</li><li>Visualizzare il log della console.</li><li>Esportare componenti.</li><li>Emettere qualsiasi API GET.</li></ul> | |

Le autorizzazioni sono cumulative. Se selezioni un ruolo **Gestore**. l'utente potrà anche eseguire tutte le azioni di **Scrittore** e **Lettore**, senza dover selezionare anche questi ruoli. Analogamente, un utente con il ruolo **Scrittore** potrà eseguire tutte le azioni previste nel ruolo **Lettore**.

### Gestione della password di un utente
{: #console-icp-manage-user-pw}

La password che era stata archiviata all'interno del [segreto della password](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) e passata quindi alla console durante la configurazione diventa la password predefinita per la console. Tutti gli utenti devono utilizzare questa password quando eseguono l'accesso alla console per la prima volta. Il gestore della console può anche specificare una nuova password predefinita. Nella scheda **Utenti** della console, fai clic sul link **Aggiorna configurazione** sotto il tuo nome di tile del servizio di autenticazione. Nel pannello di destra, puoi visualizzare o aggiornare la password predefinita per i nuovi utenti della console.

Devi condividere la password predefinita o la password predefinita che reimposti, con gli utenti in modo che possano accedere alla console. Dovranno modificare la password al loro primo accesso.
{:note}

Gli utenti con il ruolo di gestore possono reimpostare le password per gli altri utenti. Nella scheda **Utenti** della console, fai clic sui tre puntini verticali alla fine della riga per un utente specifico e fai quindi clic su **Reimposta password**. La console reimposterà la password di tale utente sulla password predefinita, che puoi controllare e confermare nel pannello di destra. Gli utenti con qualsiasi ruolo possono modificare la loro password in qualsiasi momento. Fai clic sull'icona di avatar nell'angolo superiore destro e fai quindi clic su **Modifica password**.

Dopo che hai aggiunto dei nuovi utente alla console, potrebbero non essere in grado di visualizzare tutti i nodi, canali o chaincode distribuiti da altri utenti. Per lavorare con questi componenti, ciascun utente deve importare le identità associate in un suo portafoglio della console. Per ulteriori informazioni, vedi [Memorizzazione delle identità nel tuo portafoglio della console](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modifica del ruolo di un utente
{: #console-icp-manage-reset-user-pw}

Un utente con un ruolo di gestore può aggiornare i ruoli di altri utenti della console e fornire loro l'accesso a più o meno funzionalità della console. Nella scheda **Utenti** della console, fai clic sui tre punti verticali alla fine della riga per un utente specifico e fai quindi clic su **Aggiorna utente autenticato**. Seleziona il nuovo ruolo per l'utente nel pannello a destra. Il ruolo dell'utente verrà aggiornato quando eseguirà la prossima volta l'accesso alla console.

### Eliminazione di un utente dalla console
{: #console-icp-manage-icp-remove-user}

Se sei un utente con un ruolo di gestore, puoi rimuovere l'accesso di un utente alla console. Nella scheda **Utenti** della console, fai clic sula casella di spunta all'inizio della riga per un utente specifico. Quindi fai clic sull'icona del cestino **Cestino** all'inizio della tabella, accanto al pulsante **Aggiungi nuovi utenti**. Dopo aver rimosso l'utente dalla tabella, l'utente non può accedere nuovamente alla console.

## Utilizzo delle API {{site.data.keyword.blockchainfull_notm}}
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform presenta delle API RESTful che ti consentono di importare, modificare e rimuovere nodi blockchain dalla tua console. Puoi anche utilizzare le API per gestire le impostazioni, la registrazione e le notifiche della tua console. Utilizza le istruzioni riportate di seguito per utilizzare le API fornite da una console distribuita su {{site.data.keyword.cloud_notm}} Private.

### Prerequisiti
{: #console-icp-manage-prereqs}

Per utilizzare le API, dorrai raccogliere le seguenti informazioni:

- l'**Endpoint API** della console. Questo è l'URL che utilizzi per accedere alla console servendoti del browser. Puoi trovare questo URL nella sezione **Note:** della schermata di panoramica della release Helm.
- Un nome utente e una password che puoi utilizzare per accedere alla console. Per creare una chiave API, devi disporre di un account con un [ruolo di gestore](#console-icp-manage-users).

### Utilizzo di una chiave API
{: #console-icp-manage-create-api-key}

Ogni console fornisce la propria gestione di identità e accesso. Puoi utilizzare il nome utente e la password della console per generare una chiave e un segreto API che possono autorizzare le tue chiamate API.

Ogni chiave API è associata a un ruolo che controlla le funzionalità che un utente è autorizzato ad eseguire. Le chiavi API utilizzano gli stessi ruoli di politica di accesso che esistono per gli utenti che eseguono l'accesso alla console utilizzando un nome utente e una password. Fai riferimento alla tabella nella sezione [Gestione degli utenti](#console-icp-manage-users) per l'elenco di azioni che ciascun ruolo è autorizzato ad eseguire. Poiché solo i ruoli di gestore possono creare le chiavi API, un amministratore della console deve creare le chiavi API per gli utenti lettori e scrittori. Le chiavi API non scadono mai. Di conseguenza, l'amministratore della console deve eliminare le chiavi API non utilizzate.

Esistono tre API disponibili per gestire le tue chiavi API:
- [Crea una chiave API](#console-icp-manage-create-api-key)
- [Visualizza le chiavi API](#console-icp-manage-view-api-keys)
- [Elimina le chiavi API](#console-icp-manage-delete-api-keys)

Utilizza la richiesta di seguito indicata per creare una chiave e un segreto API. Puoi quindi servirti di questa chiave e di questo segreto per utilizzare le altre API. Il segreto API non viene memorizzato dalla console e deve essere salvato dall'utente.

#### Crea una chiave API
{: #console-icp-manage-create-api-key-api}

| **Richiesta** |  |
|-------------|-----------|
| PERCORSO | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campi del corpo della richiesta** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` È obbligatorio almeno un valore </li><li>`string` facoltativo</li></ul>|
| **Campi del corpo della risposta** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Salva: la chiave non viene memorizzata</li><li>`["<role>"]`</li></ul>|
| Autorizzazione richiesta | manager (gestore) |

#### Richiesta curl di esempio: Crea chiave API
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### Gestisci le tue chiavi API della console
{: #console-icp-manage-api-keys}

Dopo aver creato una chiave e un segreto API, puoi utilizzare le API per visualizzare o rimuovere le chiavi che hai creato. Le chiavi e i segreti non scadono fino a quando non vengono eliminati.

#### Visualizza le chiavi API
{: #console-icp-manage-view-api-keys}

| **Richiesta** |  |
|-------------|-----------|
| Percorso | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campi del corpo della risposta** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` data/ora Unix in millisecondi</li><li>`string`</li></ul>|
| Autorizzazione richiesta | lettore |

#### Richiesta curl di esempio: visualizza chiavi API
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### Elimina le chiavi API
{: #console-icp-manage-delete-api-keys}

| **Richiesta** |  |
|-------------|-----------|
| Percorso | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| Autorizzazione richiesta| manager (gestore) |

#### Richiesta curl di esempio: elimina chiave API
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### Gestione degli utenti utilizzando le API
{: #console-icp-manage-users-apis}


Puoi anche utilizzare le API per elencare, aggiungere o rimuovere gli utenti che possono accedere alla console o accedere alle API della console. Puoi anche modificare i ruoli degli utenti per eseguire l'upgrade del loro livello di accesso. Sono disponibili le seguenti API:

- [Elenca utenti](#console-icp-manage-list-users-api)
- [Modifica utenti](#console-icp-manage-edit-users-api)
- [Aggiungi utenti](#console-icp-manage-add-users-api)
- [Rimuovi utenti](#console-icp-manage-remove-users-api)

Per le API che gestiscono gli utenti e le impostazioni della tua console e i nodi nella tua rete, devi aggiungere un indicatore ``-K`` o ``--insecure`` per escludere il fabbisogno di un certificato TLS. Puoi anche scaricare il certificato TLS dalla tua console utilizzando il tuo browser.

#### Elenca utenti
{: #console-icp-manage-list-users-api}


| **Richiesta** |  |
|-------------|-----------|
| Percorso | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campi del corpo della risposta** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` id utente</li><li>`string` indirizzo email</li><li>`["<role>"]`</li><li>`number` data/ora Unix in millisecondi</li></ul>|
| Autorizzazione richiesta | lettore |

#### Richiesta curl di esempio: elenca utenti
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### Modifica utenti
{: #console-icp-manage-edit-users-api}

| **Richiesta** |  |
|-------------|-----------|
| Percorso | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campi del corpo della richiesta** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` id utente </li><li>`["reader", "writer", "manager"]` È obbligatorio almeno un valore</li></ul> |
| **Campi del corpo della risposta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` id utente</li></ul>|
| Autorizzazione| manager (gestore) |

#### Richiesta curl di esempio: modifica un utente
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### Aggiungi utenti
{: #console-icp-manage-add-users-api}

| **Richiesta** |  |
|-------------|-----------|
| Percorso | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campi del corpo della richiesta** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` È obbligatorio almeno un valore </li><li>`string` facoltativo</li></ul>|
| Autorizzazione | manager (gestore) |

#### Richiesta curl di esempio: aggiungi un utente
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### Rimuovi utenti
{: #console-icp-manage-remove-users-api}

| **Richiesta** |  |
|-------------|-----------|
| Percorso | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campi del corpo della richiesta** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` id utente</li></ul>|
| **Campi del corpo della risposta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` id utente</li></ul>|
| Autorizzazione | manager (gestore) |

#### Richiesta curl di esempio: rimuovi un utente
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### Utilizza le API per gestire i tuoi componenti

Puoi visualizzare la serie completa di API disponibili nella [Guida di riferimento API di {{site.data.keyword.blockchainfull_notm}} Platform](https://test.cloud.ibm.com/apidocs/blockchain).

Poiché stai utilizzando le API per comunicare con la tua console su {{site.data.keyword.cloud_notm}} Private, devi utilizzare l'autorizzazione fornita da {{site.data.keyword.cloud_notm}} con l'autenticazione fornita dalla tua console. Sostituisci ``Bearer Auth`` nella guida di riferimento API con ``-u <api_key>:<api_secret>``. Devi anche aggiungere un indicatore ``-K`` or ``--insecure`` al comando o scaricare il certificato TLS della console utilizzando il tuo browser. Non puoi utilizzare la scheda **Try it out** per verificare le API per le reti su {{site.data.keyword.cloud_notm}} Private.

Ad esempio, la seguente chiamata API restituirà le informazioni su tutti i tuoi componenti in esecuzione su un'istanza del servizio di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
La stessa chiamata API sarà simile alla seguente richiesta per una console distribuita su {{site.data.keyword.cloud_notm}} Private.
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

Puoi utilizzare le API per creare nodi sul cluster dove è distribuita la tua console e importare i nodi da altri cluster o {{site.data.keyword.cloud_notm}}. Per ulteriori informazioni, consulta [Crea una rete utilizzando le API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) e [Importa una rete utilizzando le API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Visualizzazione dei tuoi log
{: #icp-console-manage-logs}

Quando utilizzi la console {{site.data.keyword.blockchainfull_notm}} Platform, potresti aver bisogno di visualizzare i log per eseguire il debug di un problema.

### Visualizzazione dei tuoi log della console
{: #console-icp-manage-console-logs}

Puoi facilmente accedere ai log della console se devi eseguire il debug di problemi che riscontri quando utilizzi la console o ti servi dei tuoi nodi. Puoi anche impostare il livello di registrazione nei log per aumentare o diminuire la quantità di log raccolta dalla console. I log della console sono raccolti separatamente dai [log di nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs), che sono raccolti da {{site.data.keyword.cloud_notm}} Private.

Vai alla scheda **Impostazioni** nel browser della console per modificare le impostazioni di registrazione. I log della console sono raccolti da due origini separate:

  * **Registrazione client:** questi log vengono raccolti quando i comandi vengono inviati dal tuo browser alla console.
  * **Registrazione server:** questi log vengono raccolti quando la console invia i comandi ai tuoi nodi e dalla distribuzione della console. Questi log includono l'output di registrazione di Hyperledger Fabric.

Imposta la quantità di log raccolti utilizzando l'elenco a discesa sotto ogni tipo di log. Ad esempio, **Errore** e **Avvertenza** raccolgono la minima quantità di log, mentre **Debug** e **Tutti** raccolgono quella massima.

Puoi visualizzare i log della console solo se hai eseguito l'accesso come un amministratore della console. Per visualizzare i log dalla scheda **Impostazioni**, sostituisci la parola `settings` nell'URL del browser con `api/v1/logs`. Ad esempio, se l'URL della tua console è `localhost:3001/settings`, puoi visualizzare i tuoi log andando a `localhost:3001/api/v1/logs`. I log di client e server vengono raccolti in file separati. I log più recenti si trovano nella parte superiore della pagina.

### Visualizzazione dei tuoi log di nodo
{: #console-icp-manage-node-logs}

I log del componente possono essere visualizzati dalla riga di comando utilizzando i [comandi della CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} oppure tramite [Kibana](https://www.elastic.co/products/kibana){: external}, che è incluso nel tuo cluster {{site.data.keyword.cloud_notm}} Private.

- Utilizza il comando `kubectl logs` per visualizzare i log dei contenitori all'interno del pod. Attieniti alle istruzioni per [installare la CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}, se non lo hai già fatto. Se non sei sicuro del tuo nome di pod, esegui questo comando per visualizzare il tuo elenco di pod.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Esegui quindi questo comando per richiamare i log per il contenitore del nodo che risiede all'interno del pod.

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Sostituisci `<pod_name>` con il nome del tuo pod dall'output di comando in alto.  
  Sostituisci `<node>` con `ca`, `peer` o `orderer` per visualizzare i log per il tuo nodo.  
  Sostituisci `<node>` con `fluentd` per visualizzare i log per i tuoi smart contract.

  Per ulteriori informazioni sul comando `kubectl logs`, vedi la [documentazione di Kubernetes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- In alternativa, puoi [accedere ai log](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} dalla tua console {{site.data.keyword.cloud_notm}} Private, che apre i log in Kibana.

  **Nota:** quando visualizzi i tuoi log in Kibana, potresti ricevere la risposta `No results found`. Questa condizione può verificarsi se {{site.data.keyword.cloud_notm}} Private utilizza l'indirizzo IP del tuo nodo di lavoro come suo nome host. Per risolvere questo problema, rimuovi il filtro che inizia con `node.hostname.keyword` nella parte superiore del pannello e i log diventeranno visibili.

### Visualizzazione dei tuoi log del contenitore dello smart contract
{: #console-icp-manage-container-logs}

Se riscontri dei problemi con il tuo smart contract, puoi visualizzare i log del contenitore dello smart contract o del chaincode per eseguire il debug di un problema. Puoi immettere il seguente comando per visualizzare i log del contenitore dello smart contract:

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

Sostituisci `<peer_pod>` con il nome del pod peer in cui è in esecuzione il chaincode. Utilizza il comando `kubectl get po` per richiamare l'elenco di pod in esecuzione.

## Installazione di patch per i tuoi nodi
{: #console-icp-manage-patch}

Nel corso del tempo, potrebbe essere necessario aggiornare le immagini docker {{site.data.keyword.IBM_notm}} Hyperledger Fabric sottostanti per i nodi ordinante, CA e peer, ad esempio con aggiornamenti della sicurezza o a una nuova release secondaria Fabric. Puoi aggiornare le tue immagini Fabric quando esegui l'upgrade del grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform.

Dopo aver eseguito l'upgrade del tuo grafico Helm, potrai trovare il testo **Patch disponibile** su un tile del nodo se è disponibile un aggiornamento del componente. Questa patch può essere installata su un nodo se sei pronto. Queste patch sono facoltative, ma consigliate. Non puoi applicare patch ai nodi che sono stati importati nella console.

Le patch vengono applicate ai nodi una alla volta. Mentre una patch viene applicata, il nodo non è disponibile per elaborare richieste o transazioni. Pertanto, per evitare un'interruzione del servizio, se possibile dovresti assicurarti che un altro nodo dello stesso tipo sia disponibile per elaborare le richieste. L'installazione della patch su un nodo ha bisogno di circa un minuto per il completamento e quando l'aggiornamento è completo, il nodo è pronto ad elaborare le richieste.
{:note}

Per applicare una patch a un nodo, apri il tile del nodo e fai clic sul pulsante **Installa patch**. Non puoi applicare la patch sui nodi che hai importato nella console.
