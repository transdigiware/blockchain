---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"


keywords: IBM Cloud Private, IBM Blockchain Platform, install, Helm chart, PodSecurityPolicy

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

# Installazione del grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private viene fornito come un grafico Helm che può essere installato su un cluster {{site.data.keyword.cloud_notm}} Private locale. Dopo che hai installato il grafico Helm, puoi trovare {{site.data.keyword.blockchainfull_notm}} Platform come un'applicazione nel catalogo {{site.data.keyword.cloud_notm}} Private.

Il grafico Helm deve essere acquistato mediante [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Quando effettui l'acquisto, il supporto tecnico per {{site.data.keyword.blockchainfull_notm}} Platform è incluso.

Prima di installare {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private, esamina le [Considerazioni e limitazioni](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations). Per ulteriori informazioni sui prezzi, sul supporto e sulla sicurezza e per le considerazioni sulla residenza dei dati, vedi [Informazioni su {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about).

## Prerequisiti per l'installazione del grafico Helm
{: #console-helm-install-prereqs}

Prima di installare il grafico Helm, devi aver configurato un cluster {{site.data.keyword.cloud_notm}} Private e creato un nuovo spazio dei nomi di destinazione associato a una politica di sicurezza del pod. Rivedi le istruzioni per [impostare e configurare un cluster {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup). Puoi distribuire solo una console per spazio dei nomi. Se intendi creare più reti blockchain, ad esempio per creare diversi ambienti per la distribuzione, la preparazione e la produzione, devi creare uno spazio dei nomi univoco per ogni ambiente.

### Requisiti PodSecurityPolicy
{: #console-helm-install-prereqs-pod-security-requirements}

Il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform richiede che delle politiche di accesso e di sicurezza specifiche siano associate allo spazio dei nomi di destinazione prima dell'installazione. Forniamo dei file YAML che definiscono le politiche nei prossimi passi. Puoi salvare questi file sul tuo sistema locale e poi associarli al tuo spazio dei nomi utilizzando la CLI {{site.data.keyword.cloud_notm}} Private. Attieniti alle seguenti istruzioni prima di distribuire il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform.

1. Salva il seguente file che definisce la PodSecurityPolicy di {{site.data.keyword.blockchainfull_notm}} Platform come `ibm-blockchain-platform-psp.yaml` sul tuo sistema locale:

    ```
    apiVersion: extensions/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: ibm-blockchain-platform-psp
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      privileged: true
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      seLinux:
        rule: RunAsAny
      supplementalGroups:
        rule: RunAsAny
      runAsUser:
        rule: RunAsAny
      fsGroup:
        rule: RunAsAny
      requiredDropCapabilities:
      - ALL
      allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - FOWNER
      volumes:
      - '*'
    ```
    {:codeblock}

2. Salva il seguente file che definisce il ClusterRole richiesto per la PodSecurityPolicy come `ibm-blockchain-platform-clusterrole.yaml`:

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
    rules:
    - apiGroups:
      - extensions
      resourceNames:
      - ibm-blockchain-platform-psp
      resources:
      - podsecuritypolicies
      verbs:
      - use
    - apiGroups:
      - "*"
      resources:
      - pods
      - services
      - endpoints
      - persistentvolumeclaims
      - persistentvolumes
      - events
      - configmaps
      - secrets
      - ingresses
      - roles
      - rolebindings
      - serviceaccounts
      verbs:
      - '*'
    - apiGroups:
      - apiextensions.k8s.io
      resources:
      - persistentvolumeclaims
      - persistentvolumes
      - customresourcedefinitions
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      - ibpservices
      - ibpcas
      - ibppeers
      - ibpfabproxies
      - ibporderers
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      verbs:
      - '*'
    - apiGroups:
      - apps
      resources:
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
      verbs:
      - '*'
    ```
    {:codeblock}

3. Salva il seguente file che definisce il ClusterRoleBinding come `ibm-blockchain-platform-clusterrolebinding.yaml`. Se decidi di modificare il nome ServiceAccount nel seguente file, devi fornire il nome nel campo `Service account name` nella sezione **Tutti i parametri** della pagina di configurazione quando distribuisci il grafico Helm.

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
   name: ibm-blockchain-platform-clusterrolebinding
  roleRef:
   apiGroup: rbac.authorization.k8s.io
   kind: ClusterRole
   name: ibm-blockchain-platform-clusterrole
  subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
  ```
  {:codeblock}

Dopo aver salvato i file YAML PodSecurityPolicy, ClusterRole e ClusterRoleBinding nel tuo sistema locale, un amministratore del cluster dovrà utilizzare la CLI {{site.data.keyword.cloud_notm}} Private per associare le politiche al tuo spazio dei nomi.

1. Accedi al tuo cluster {{site.data.keyword.cloud_notm}} Private e seleziona lo spazio dei nomi di destinazione della tua distribuzione.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Accedi al registro di immagini Docker del tuo cluster:

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Utilizza i seguenti comandi per applicare le politiche al tuo spazio dei nomi di destinazione:

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. Dopo aver applicato le politiche, devi concedere al tuo account di servizio il livello richiesto di autorizzazioni per la distribuzione alla tua console. Immetti il seguente comando con il nome del tuo spazio dei nomi di destinazione:

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```

## Importazione del grafico Helm in {{site.data.keyword.cloud_notm}} Private
{: #console-helm-install-importing}

1. Scarica il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private da [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}.

2. Se non l'hai già fatto, accedi al tuo cluster {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Assicurati che la [CLI Docker](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) sia configurata. Dopo aver configurato la CLI Docker, accedi al registro delle immagini sul tuo cluster utilizzando il seguente comando:
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Trova il nome del repository in {{site.data.keyword.cloud_notm}} Private per caricare il tuo grafico Helm utilizzando il seguente comando:
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  Se questo comando viene completato correttamente, puoi visualizzare un elenco di repository nel tuo cluster. Scegli il nome del repository di destinazione e salvalo. Dovrai utilizzarlo nel comando riportato di seguito.

5. Importa il grafico Helm utilizzando la riga di comando. Dalla directory in cui hai memorizzato il pacchetto del grafico Helm scaricato da PPA, immetti il seguente comando nella CLI {{site.data.keyword.cloud_notm}} Private per importare il grafico Helm nel tuo cluster {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Sostituisci i seguenti valori:
  - `<archive-name>` con il nome del file `.tgz` scaricato.
  - `<cluster_CA_domain>:8500` con il dominio che utilizzi per eseguire l'accesso al tuo cluster {{site.data.keyword.cloud_notm}} Private.
  - `<repo-name>` con il repository Helm in cui vuoi caricare il grafico. Esegui 'cloudctl catalog repos' per elencare i tuoi repository.

  Se questo comando viene completato correttamente, puoi visualizzare qualcosa di simile alle seguenti informazioni:

  ```  
  Uploading Helm chart(s)
   Processing chart: charts/ibm-blockchain-platform-prod-1.1.0.tgz
   Updating chart values.yaml
   Uploading chart
  Loaded Helm chart
  OK

  Synch charts
  OK

  Archive finished processing
  ```  
  </details>

Fai clic sul pulsante **Catalogo** nella console {{site.data.keyword.cloud_notm}} Private e fai quindi clic su **Blockchain** nel pannello di navigazione a sinistra. Se l'importazione ha avuto esito positivo, il tile **ibm-blockchain-platform-prod** sarà visibile nella pagina del catalogo di {{site.data.keyword.cloud_notm}} Private.

## Passi successivi
{: #console-helm-install-next-steps}

Dopo che hai installato il grafico Helm puoi utilizzare il tile **ibm-blockchain-platform-prod** nel tuo catalogo di {{site.data.keyword.cloud_notm}} Private per distribuire la console {{site.data.keyword.blockchainfull_notm}} Platform. Devi creare un nuovo spazio dei nomi di destinazione per la distribuzione e assicurarti che il tuo cluster disponga di risorse sufficienti per i tuoi componenti {{site.data.keyword.blockchainfull_notm}} Platform prima di completare la pagina di configurazione. Per ulteriori informazioni, vedi [Distribuzione della console {{site.data.keyword.blockchainfull_notm}} su {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp).
