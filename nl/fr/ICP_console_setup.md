---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-16"


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

Vérifiez que votre système {{site.data.keyword.cloud_notm}} Private respecte les exigences de ressources matérielle minimum pour la console et les composant que vous créez . Le nombre de vCPU/UC requis peut varier en fonction de la conception de votre infrastructure, de votre réseau et des exigences de performances. Une approximation de vos besoins en termes de vCPU/UC peut être effectuée par un examen du [tableau des allocations de ressources par défaut](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default) pour {{site.data.keyword.cloud_notm}}, bien que les allocations soient légèrement différentes dans {{site.data.keyword.cloud_notm}} Private. Vos allocations de ressources réelles sont visibles sur la console de votre blockchain lorsque vous déployez un noeud.

**Remarques :**  

- Une unité vCPU est un coeur virtuel qui est affecté à une machine virtuelle ou à un coeur de processeur physique si le serveur n'est pas partitionné pour les machines virtuelles. Vous devez tenir compte des exigences vCPU lorsque vous décidez d'utiliser le coeur de processeur virtuel (VPC) pour votre déploiement dans {{site.data.keyword.cloud_notm}} Private. VPC est une unité de mesure pour déterminer les coûts de licences des produits {{site.data.keyword.IBM_notm}}. Pour plus d'informations sur les scénarios VPC, voir [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- Une unité vCPU équivaut à une UC, qui équivaut à un VPC.

### Remarques relatives au stockage
{: #icp-console-setup-storage-considerations}

La charte {{site.data.keyword.blockchainfull_notm}} Helm utilise la mise à disposition dynamique pour mettre à disposition le stockage qui va être utilisé par la console et les composants de blockchain que vous allez créer. Avant de déployer la console, vous devez créer une classe de stockage avec suffisamment de stockage de secours pour votre console et vos composants. Vous devez indiquer le nom de la classe de stockage que vous avez créée lors de la configuration.

- Si vous utilisez les paramètres par défaut, la charte Helm crée un nouveau volume persistant dont le nom est celui de la version de votre charte Helm de vos données de console.
- Si vous utilisez des Volumes permanents NFS v2/v3, vous devez activer le module **NFS status monitor for NFSv2/v3 Filesystem Locks**, également appelé **rpc-statd**, sur le système hôte où réside le système de fichiers NFS. Ce module permet à votre système de fichiers NFS de vérifier les verrous exclusifs sur les fichiers maintenus par d'autres processus. Exécutez les commandes suivantes pour activer ce module :

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

3. Créez un nouvel espace de nom personnalisé pour votre déploiement d'{{site.data.keyword.blockchainfull_notm}} Platform. Notez que vous ne pouvez déployer qu'une seule charte Helm par espace de nom, de sorte que si vous voulez que plusieurs instances de la console s'exécutent dans le même cluster, assurez-vous d'utiliser des espaces de nom distincts.

4. Configurez les politiques de sécurité et d'accès du pod pour l'espace de nom cible. Les instructions sont fournies dans la [section suivante](#icp-console-setup-psp).

Après que vous avez installé {{site.data.keyword.cloud_notm}} Private et lié une règle de sécurité du pod à un espace de nom cible, vous pouvez passer à l'[importation de la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install) dans votre cluster {{site.data.keyword.cloud_notm}} Private.

## Exigences PodSecurityPolicy
{: #icp-console-setup-psp}

La charte Helm d'{{site.data.keyword.blockchainfull_notm}} Platform exige que des politiques de sécurité et d'accès spécifiques soit liées à l'espace de nom cible avant l'installation. Nous fournissons des fichiers YAML qui définissent les règles dans les étapes ci-dessous. Vous pouvez sauvegarder ces fichiers sur votre système local et les lier ensuite à votre espace de noms à partir de l'interface CLI {{site.data.keyword.cloud_notm}}. Suivez les étapes ci-après avant de déployer la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform.

1. Sauvegardez le fichier ci-dessous qui définit {{site.data.keyword.blockchainfull_notm}} Platform PodSecurityPolicy as `ibm-blockchain-platform-psp.yaml` sur votre système local :

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

2. Sauvegardez le fichier ci-dessous qui définit le ClusterRole requis pour PodSecurityPolicy en tant que `ibm-blockchain-platform-clusterrole.yaml` :

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

3. Sauvegardez le fichier ci-dessous qui définit le ClusterRoleBinding en tant que `ibm-blockchain-platform-clusterrolebinding.yaml`. Si vous décider de modifier le nom ServiceAccount dans le fichier ci-dessous, vous devez indiquer ce nom dans la zone `Service account name` de la section **All Parameters** de la page de configuration lors du déploiement de la charte Helm.

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

Une fois que vous avez sauvegardé les fichiers PodSecurityPolicy, ClusterRole et ClusterRoleBinding YAML sur votre système local, un administrateur de cluster devra utiliser l'interface CLI {{site.data.keyword.cloud_notm}} Private pour lier les règles à votre espace de nom.

1. Connectez-vous à votre cluster {{site.data.keyword.cloud_notm}} Private et sélectionnez l'espace de nom cible de votre déploiement.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Connectez-vous au registre d'image docker pour votre cluster :

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Utilisez les commandes suivantes pour appliquer des règles à l'espace de nom cible :

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. Après avoir appliqué les règles, vous devez accorder à votre compte de service le niveau d'autorisations requis pour déployer votre console. Exécutez la commande suivante avec le nom de votre espace de nom cible :

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
