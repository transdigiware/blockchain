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

# Installation de la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private est fourni en tant que charte Helm qui peut être installée dans un cluster {{site.data.keyword.cloud_notm}} Private local. Une fois que vous avez installé la charte Helm, vous pouvez trouver {{site.data.keyword.blockchainfull_notm}} Platform en tant qu'application dans le catalogue {{site.data.keyword.cloud_notm}} Private.

La charte Helm doit être achetée via [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Dans le cadre de cet achat, le support technique de {{site.data.keyword.blockchainfull_notm}} Platform est inclus.

Avant d'installer {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private, passez en revue la section [Considérations et limitations](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations). Pour plus d'informations sur la tarification, le support, la sécurité et l'hébergement de données, voir [A propos d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about).

## Prérequis pour l'installation de la charte Helm
{: #console-helm-install-prereqs}

Avant d'installer la charte Helm, vous devez avoir configuré un cluster {{site.data.keyword.cloud_notm}} Private et créé un nouvel espace de nom cible qui est lié à une politique de sécurité de pod. Passez en revue les instructions relatives à la [définition et à la configuration d'un cluster {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup). Vous pouvez uniquement déployer une console par espace de nom. Si vous prévoyez de créer plusieurs réseaux de blockchain, par exemple pour créer différents environnements de développement, de transfert et de production, vous devez créer un espace de nom unique pour chaque environnement.

### Exigences PodSecurityPolicy
{: #console-helm-install-prereqs-pod-security-requirements}

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

## Importation de la charte Helm dans {{site.data.keyword.cloud_notm}} Private
{: #console-helm-install-importing}

1. Téléchargez la charte Helm d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private depuis [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}.

2. Si ce n'est déjà fait, connectez-vous à votre cluster {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Vérifiez que l'[interface CLI Docker](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) est configurée. Une fois l'interface CLI Docker configurée, accédez au registre d'images sur votre cluster à l'aide de la commande suivante :
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Recherchez le nom du référentiel dans {{site.data.keyword.cloud_notm}} Private pour télécharger la charte Helm à l'aide de la commande suivante :
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  Lorsque cette commande est exécutée correctement, vous pouvez voir une liste des référentiels dans votre cluster. Sélectionnez le nom du référentiel cible et sauvegardez-le. Vous devez l'utiliser dans la commande ci-dessous.

5. Importez la charte Helm à l'aide de la ligne de commande. Depuis le répertoire où vous avez stocké le package de la charte Helm téléchargée depuis PPA, exécutez la commande suivante dans l'interface CLI {{site.data.keyword.cloud_notm}} Private afin d'importer la charte Helm dans votre cluster {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Remplacez les valeurs suivantes :
  - `<archive-name>` par le nom du fichier `.tgz` téléchargé.
  - `<cluster_CA_domain>:8500` par le domaine que vous utilisez pour vous connecter à votre cluster {{site.data.keyword.cloud_notm}} Private.
  - `<repo-name>` par le référentiel Helm ou vous voulez télécharger la charte. Exécutez 'cloudctl catalog repos' pour afficher la liste de vos référentiels.

  Lorsque cette commande est exécutée correctement, vous pouvez voir quelque chose similaire aux informations suivantes :

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

Cliquez sur le bouton **Catalogue** sur la console {{site.data.keyword.cloud_notm}} Private, puis cliquez sur **Blockchain** dans le panneau de navigation de gauche. Si l'importation a abouti, la vignette **ibm-blockchain-platform-prod** doit être visible sur la page du catalogue d'{{site.data.keyword.cloud_notm}} Private.

## Etapes suivantes
{: #console-helm-install-next-steps}

Une fois la charte Helm installée, vous pouvez utiliser la vignette **ibm-blockchain-platform-prod** dans votre catalogue {{site.data.keyword.cloud_notm}} Private pour déployer la console {{site.data.keyword.blockchainfull_notm}} Platform. Vous devez créer un nouvel espace de nom cible pour le déploiement et vous assurer que votre cluster dispose de ressources suffisantes pour vos composants de plateforme {{site.data.keyword.blockchainfull_notm}} avant de terminer la page de configuration. Pour plus d'informations, voir [Déploiement de la console {{site.data.keyword.blockchainfull_notm}} dans {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp).
