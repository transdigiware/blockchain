---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

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

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private viene fornito come un grafico Helm che può essere installato su un cluster {{site.data.keyword.cloud_notm}} Private locale. Dopo aver installato il grafico Helm, puoi trovare {{site.data.keyword.blockchainfull_notm}} Platform come applicazione nel catalogo di {{site.data.keyword.cloud_notm}} Private.

Il grafico Helm deve essere acquistato mediante [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Quando effettui l'acquisto, il supporto tecnico per {{site.data.keyword.blockchainfull_notm}} Platform è incluso.

Prima di installare {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private, esamina le [Considerazioni e limitazioni](/docs/services/blockchain/console-icp-about.html#console-icp-about-considerations). Per ulteriori informazioni sui prezzi, sul supporto e sulla sicurezza e per le considerazioni sulla residenza dei dati, vedi [Informazioni su {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/console-icp-about.html#console-icp-about).

## Prerequisiti per l'installazione del grafico Helm
{: #console-helm-install-prereqs}

Prima di installare il grafico Helm, devi aver configurato un cluster {{site.data.keyword.cloud_notm}} Private e creato un nuovo spazio dei nomi di destinazione associato a una politica di sicurezza del pod. Rivedi le istruzioni per [impostare e configurare un cluster {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup). Se intendi creare più reti blockchain, ad esempio per creare diversi ambienti per la distribuzione, la preparazione e la produzione, devi creare uno spazio dei nomi univoco per ogni ambiente.

### Requisiti PodSecurityPolicy
{: #console-helm-install-prereqs-pod-security-requirements}

Il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform richiede che una PodSecurityPolicy sia associata allo spazio dei nomi di destinazione prima dell'installazione. Scegli una PodSecurityPolicy predefinita o chiedi al tuo amministratore del cluster di crearti una PodSecurityPolicy personalizzata:
- Nome PodSecurityPolicy predefinita: [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- Definizione della PodSecurityPolicy personalizzata:
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
    volumes:
    - '*'
  ```
- Il ClusterRole personalizzato per la PodSecurityPolicy personalizzata:
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
    - ""
    resources:
    - secrets
    verbs:
    - create
    - delete
    - get
    - list
    - patch
    - update
    - watch
  ```
- Il ClusterRoleBinding per il ClusterRole personalizzato:
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
  Loading Helm chart
  Loaded Helm chart

  Synch charts on repo: <repo-name>
  OK
  ```  
  </details>

Fai clic sul pulsante **Catalogo** nella console {{site.data.keyword.cloud_notm}} Private e fai quindi clic su **Blockchain** nel pannello di navigazione a sinistra. Se l'importazione ha avuto esito positivo, il tile **ibm-blockchain-platform-prod** sarà visibile nella pagina del catalogo di {{site.data.keyword.cloud_notm}} Private.

## Passi successivi
{: #console-helm-install-next-steps}

Dopo che hai installato il grafico Helm puoi utilizzare il tile **ibm-blockchain-platform-prod** nel tuo catalogo di {{site.data.keyword.cloud_notm}} Private per distribuire la console {{site.data.keyword.blockchainfull_notm}} Platform. Devi creare un nuovo spazio dei nomi di destinazione per la distribuzione e assicurarti che il tuo cluster disponga di risorse sufficienti per i tuoi componenti {{site.data.keyword.blockchainfull_notm}} Platform prima di completare la pagina di configurazione. Per ulteriori informazioni, vedi [Distribuzione della console {{site.data.keyword.blockchainfull_notm}} su {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).
