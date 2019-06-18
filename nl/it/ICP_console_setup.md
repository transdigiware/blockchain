---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, data storage CA, cluster ICP, configuration

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

# Configurazione di {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup}

Prima di distribuire i componenti {{site.data.keyword.blockchainfull}} Platform e di creare la rete blockchain su {{site.data.keyword.cloud_notm}} Private, devi configurare {{site.data.keyword.cloud_notm}} Private nel tuo ambiente.
{:shortdesc}

## Prerequisiti
{: #icp-console-setup-prerequisites}

Completa i seguenti prerequisiti e prepara il tuo ambiente per installare {{site.data.keyword.cloud_notm}} Private.

### Docker
{{site.data.keyword.cloud_notm}} Private richiede che sia installato Docker. Attieniti alle istruzioni correlate in [Installing {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} per installare Docker.

### Impostazioni di {{site.data.keyword.cloud_notm}} Private
Prima di installare {{site.data.keyword.cloud_notm}} Private, i seguenti suggerimenti sono utili per preparare i tuoi nodi per l'installazione di {{site.data.keyword.cloud_notm}} Private. Dei prerequisiti di {{site.data.keyword.cloud_notm}} Private aggiuntivi sono disponibili nella [documentazione di {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Aggiorna l'impostazione `vm.max_map_count`
{{site.data.keyword.cloud_notm}} Private utilizza Elastic Search per la registrazione e la misurazione. Per evitare eccezioni di memoria esaurita, Elastic Search richiede che sia configurata la proprietà di sistema `vm.max_map_count`. Prima di installare {{site.data.keyword.cloud_notm}} Private, vedi le [istruzioni di configurazione di Elastic Search](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} per configurare questa proprietà su ciascun nodo. Puoi utilizzare i seguenti comandi per impostare questa proprietà permanentemente:

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Configura il file `/etc/hosts` su ciascun nodo nel tuo cluster

- {{site.data.keyword.cloud_notm}} Private utilizza [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} per gestire le applicazioni inserite nel contenitore. Il DNS (Domain Name Server) Kubernetes non riesce se i nomi host non sono configurati nel file `/etc/hosts` su ciascun nodo. [Inserisci indirizzo IP, nome host e nome breve di ciascun nodo nel tuo cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} nel file `/etc/hosts` su ciascun nodo.

- [IPv6 non è supportato da {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. Per evitare problemi con il servizio DNS in un cluster {{site.data.keyword.cloud_notm}} Private, disabilita le impostazioni IPv6 nel file `/etc/hosts` su ciascun nodo indicando come commento la seguente riga con un carattere `#` all'inizio della riga:
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Risorse richieste
{: #icp-console-setup-resources}

Assicurati che il tuo sistema {{site.data.keyword.cloud_notm}} Private soddisfi i requisiti di risorse hardware minimi per ogni componente di runtime Fabric.

| **Componente** (tutti i contenitori) | CPU virtuale  | Memoria (GB) | Archiviazione (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console**                    | 1,3            | 2,5                   | 10                     |
| **Peer**                       | 1,2            | 2,4                   | 200 (include 100GB per peer e 100GB per CouchDB)|
| **CA**                         | 0,1            | 0,2                   | 20                     |
| **Ordinante**                    | 0,45           | 0,9                   | 100                    |

 **Note:**
 - Una CPU virtuale è un core virtuale assegnato a una macchina virtuale o un core di processore fisico se il server non è partizionato per le macchine virtuali. Devi considerare i requisiti di CPU virtuale quando decidi il VPC (virtual processor core) per la tua distribuzione in {{site.data.keyword.cloud_notm}} Private. VPC è un'unità di misura per determinare il costo di licenza dei prodotti {{site.data.keyword.IBM_notm}}. Per ulteriori informazioni sugli scenari per decidere il VPC, vedi [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Una vCPU è equivalente a una CPU, che è equivalente a un VPC.

### Considerazioni sull'archiviazione
{: #icp-console-setup-storage-considerations}

* Devi creare una nuova classe di archiviazione con risorse sufficienti per la tua console e i componenti che crei. La classe di archiviazione che fornisci alla console durante la configurazione verrà utilizzata anche per archiviare i tuoi dati di componente.
* Se utilizzi le impostazioni predefinite, il grafico Helm crea una nuova attestazione di volume persistente (PVC, Persistent Volume Claim) con il nome della tua release helm per i tuoi dati di console.
* Il [provisioning dinamico](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/){: external} è disponibile solo per i nodi amd64 in {{site.data.keyword.cloud_notm}} Private. Pertanto, se il tuo cluster include una combinazione di nodi di lavoro s390x e amd64, non è possibile utilizzare il provisioning dinamico.
* Se il provisioning dinamico non viene utilizzato, è necessario creare e configurare i [volumi persistenti](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} con delle etichette che possono essere utilizzate per perfezionare il processo di bind dell'attestazione di volume persistente (PVC, Persistent Volume Claim).
* Se utilizzi i volumi persistenti NFS v2/v3 , devi abilitare il modulo **NFS status monitor for NFSv2/v3 Filesystem Locks**, noto anche come **rpc-statd**, sul sistema host dove esiste il file system NFS. Questo modulo consente al tuo file system NFS di controllare l'eventuale presenza di blocchi esclusivi sui file detenuti da altri processi. Per abilitare questo modulo, esegui questi comandi:

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## Installazione di {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup-install}

Completa la seguente procedura per installare e configurare {{site.data.keyword.cloud_notm}} Private nel tuo ambiente.

1. Installa un cluster [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} alla v3.2.0.

2. Installa la CLI {{site.data.keyword.cloud_notm}} Private [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} per installare il grafico Helm.

3. Configura la politica di sicurezza del pod per lo spazio dei nomi di destinazione. Le istruzioni sono fornite nella [prossima sezione](#icp-setup-psp).

Dopo che hai installato {{site.data.keyword.cloud_notm}} Private e associato una politica di sicurezza del pod a uno spazio dei nomi di destinazione, puoi continuare a [importare il grafico Helm {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install) nel tuo cluster {{site.data.keyword.cloud_notm}} Private.

## Requisiti PodSecurityPolicy
{: #icp-console-setup-psp}

Il grafico Helm di {{site.data.keyword.blockchainfull_notm}} Platform richiede che una [PodSecurityPolicy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/){: external} sia associata allo spazio dei nomi di destinazione prima dell'installazione. Scegli una PodSecurityPolicy predefinita o chiedi al tuo amministratore del cluster di crearti una PodSecurityPolicy personalizzata:
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
  {:codeblock}
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
  {:codeblock}

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
  {:codeblock}
