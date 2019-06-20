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

# Configuration de {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup}

Avant de déployer des composants {{site.data.keyword.blockchainfull}} Platform et de générer un réseau de blockchain sur {{site.data.keyword.cloud_notm}} Private, vous devez configurer {{site.data.keyword.cloud_notm}} Private dans votre propre environnement.
{:shortdesc}

## Prérequis
{: #icp-console-setup-prerequisites}

Effectuez les opérations prérequises suivantes et préparez votre environnement pour installer {{site.data.keyword.cloud_notm}} Private.

### Docker
{{site.data.keyword.cloud_notm}} Private exige que Docker soit installé. Suivez les instructions associées dan [Installation d'{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} pour installer Docker.

### Paramètres de {{site.data.keyword.cloud_notm}} Private
Avant d'installer {{site.data.keyword.cloud_notm}} Private, les conseils suivants peuvent s'avérer utiles pour préparer vos noeuds à l'installation d'{{site.data.keyword.cloud_notm}} Private. Des prérequis {{site.data.keyword.cloud_notm}} Private supplémentaires figurent dans la section [{{site.data.keyword.cloud_notm}} Private documentation](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Mise à jour du paramètre `vm.max_map_count`
{{site.data.keyword.cloud_notm}} Private utilise Elastic Search pour la consignation et le décompte. Pour éviter mes exceptions out-of-memory, Elastic Search exige que la propriété système `vm.max_map_count` soit configurée. Avant d'installer {{site.data.keyword.cloud_notm}} Private, voir les [instructions de configuration Elastic Search](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} pour configurer cette propriété sur chaque noeud. Vous pouvez utiliser les commandes suivantes pour définir cette propriété de façon permanente :

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Configuration du fichier `/etc/hosts` sur chaque noeud dans votre cluster

- {{site.data.keyword.cloud_notm}} Private utilise [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} pour gérer les applications conteneurisées. Le serveur DNS Kubernetes est défaillant si les noms d'hôte ne sont pas configurés dans le fichier `/etc/hosts` sur chaque noeud. [Insérez l'adresse IP, le nom d'hôte et le nom abrégé de chaque noeud dans votre cluster ](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} dans le fichier `/etc/hosts` sur chaque noeud.

- [IPv6 n'est pas pris en charge par {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. Pour éviter les problèmes avec le service DNS dans un cluster {{site.data.keyword.cloud_notm}} Private, désactivez les paramètres IPv6 dans le fichier `/etc/hosts` sur chaque noeud en mettant en commentaire la ligne suivante à l'aide du signe `#` au début de la ligne :
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Ressources obligatoires
{: #icp-console-setup-resources}

Vérifiez que votre système {{site.data.keyword.cloud_notm}} Private respecte les exigences de ressources matérielle minimum pour chaque composant d'exécution Fabric :

| **Composant** (tous les conteneurs) | vCPU  | Mémoire (Go) | Stockage (Go) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console**                    | 1,3            | 2,5                   | 10                     |
| **Homologue**                       | 1,2            | 2,4                   | 200 (inclut 100 Go pour l'homologue et 100 Go pour CouchDB)|
| **AC**                         | 0,1            | 0,2                   | 20                     |
| **Service de tri**                    | 0,45           | 0,9                   | 100                    |

 **Remarques :**
 - Une unité vCPU est un coeur virtuel qui est affecté à une machine virtuelle ou à un coeur de processeur physique si le serveur n'est pas partitionné pour les machines virtuelles. Vous devez tenir compte des exigences vCPU lorsque vous décidez d'utiliser le coeur de processeur virtuel (VPC) pour votre déploiement dans {{site.data.keyword.cloud_notm}} Private. VPC est une unité de mesure pour déterminer les coûts de licences des produits {{site.data.keyword.IBM_notm}}. Pour plus d'informations sur les scénarios VPC, voir [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Une unité vCPU équivaut à une UC, qui équivaut à un VPC.

### Remarques relatives au stockage
{: #icp-console-setup-storage-considerations}

* Vous devez créer une nouvelle classe de stockage avec des ressources suffisantes pour la console et les composants que vous créez. La classe de stockage que vous fournissez à la console pendant la configuration sera également utilisée pour stocker les données de votre composant.
* Si vous utilisez les paramètres par défaut, la charte Helm crée un nouveau volume persistant dont le nom est celui de la version de votre charte Helm de vos données de console.
* La [mise à disposition dynamique](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/){: external} est uniquement disponible pour les noeuds amd64 dans {{site.data.keyword.cloud_notm}} Private. Par conséquent, si votre cluster combine à la fois des noeuds worker s390x et amd64, la mise à disposition dynamique ne peut pas être utilisée.
* Si la mise à disposition dynamique n'est pas utilisée, des [Volumes permanents](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} doivent être créés et configurés avec des libellés qui peuvent être utilisés pour affiner le processus de liaison PVC.
* Si vous utilisez des Volumes permanents NFS v2/v3, vous devez activer le module **NFS status monitor for NFSv2/v3 Filesystem Locks**, également appelé **rpc-statd**, sur le système hôte où réside le système de fichiers NFS. Ce module permet à votre système de fichiers NFS de vérifier les verrous exclusifs sur les fichiers maintenus par d'autres processus. Exécutez les commandes suivantes pour activer ce module :

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## Installation d'{{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup-install}

Pour installer et configurer {{site.data.keyword.cloud_notm}} Private dans votre environnement, procédez comme suit.

1. Installez un cluster [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} en version 3.2.0.

2. Installez l'interface CLI d'{{site.data.keyword.cloud_notm}} Private [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} to install the Helm chart.

3. Configurez la règle de sécurité du pod pour l'espace de nom cible. Les instructions sont fournies dans la [section suivante](#icp-setup-psp).

Après que vous avez installé {{site.data.keyword.cloud_notm}} Private et lié une règle de sécurité du pod à un espace de nom cible, vous pouvez passer à l'[importation de la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install) dans votre cluster {{site.data.keyword.cloud_notm}} Private.

## Exigences PodSecurityPolicy
{: #icp-console-setup-psp}

La charte Helm d'{{site.data.keyword.blockchainfull_notm}} Platform exige qu'une règle [PodSecurityPolicy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/){: external} soit liée à l'espace de nom cible avant l'installation. Choisissez un règle PodSecurityPolicy prédéfinie ou demandez à votre administrateur de cluster de créer une règle PodSecurityPolicy personnalisée pour vous :
- Nom de la règle PodSecurityPolicy prédéfinie : [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- Définition de règle PodSecurityPolicy personnalisée :
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
- Rôle ClusterRole personnalisé pour la règle PodSecurityPolicy personnalisée :
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

- Liaison ClusterRoleBinding personnalisée pour le rôle ClusterRole personnalisé :
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
