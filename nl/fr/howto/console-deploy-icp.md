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

# Déploiement de la console {{site.data.keyword.blockchainfull_notm}}
{: #console-deploy-icp}

Utilisez les instructions ci-après pour déployer la console {{site.data.keyword.blockchainfull}} Platform dans votre cluster {{site.data.keyword.cloud_notm}} Private local après avoir installé la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

## Ressources obligatoires
{: ##console-deploy-icp-resources-required}

Vérifiez que votre système {{site.data.keyword.cloud_notm}} Private respecte les exigences de ressources matérielle minimum pour la console et les composant que vous créez :

| **Composant** (tous les conteneurs) | UC  | Mémoire (Go) | Stockage (Go) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console** | 1,3 | 2,5 | 10 |
| **Homologue** | 1,2 | 2,4 | 200 (inclut 100 Go pour l'homologue et 100 Go pour CouchDB)|
| **AC** | 0,1 | 0,2  | 20 |
| **Service de tri** | 0,45 | 0,9 | 100 |

 **Remarques :**
 - Une unité vCPU est un coeur virtuel qui est affecté à une machine virtuelle ou à un coeur de processeur physique si le serveur n'est pas partitionné pour les machines virtuelles. Vous devez tenir compte des exigences vCPU lorsque vous décidez d'utiliser le coeur de processeur virtuel (VPC) pour votre déploiement dans {{site.data.keyword.cloud_notm}} Private. VPC est une unité de mesure pour déterminer les coûts de licences des produits {{site.data.keyword.IBM_notm}}. Pour plus d'informations sur les scénarios VPC, voir [Virtual processor core (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Une unité vCPU équivaut à une UC, qui équivaut à un VPC.

## Stockage
{: #console-deploy-icp-storage}

Vérifiez que vous avez créé une classe de stockage avec suffisamment de stockage de sauvegarde pour votre console et vos composants de blockchain.

- Vous devez créer une nouvelle classe de stockage avec des ressources suffisantes pour la console et les composants que vous créez. La classe de stockage que vous fournissez à la console pendant la configuration sera également utilisée pour stocker les données de votre composant.
- Si vous utilisez les paramètres par défaut, la charte Helm crée un nouveau volume persistant dont le nom est celui de la version de votre charte Helm de vos données de console.
- Si la mise à disposition dynamique n'est pas utilisée, des [Volumes persistants](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}. doivent être créés et configurés avec des libellés qui peuvent être utilisés pour affiner le processus de liaison PVC.

## Prérequis pour le déploiement de la console
{: #console-deploy-icp-prerequisites}

1. Avant de déployer la console {{site.data.keyword.blockchainfull_notm}} Platform, vous devez [installer {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup) et [installer la charte Helm d'{{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-helm-install.html#console_helm-install).

2. Vous devez créer un nouvel espace de nom personnalisé pour votre déploiement d'{{site.data.keyword.blockchainfull_notm}} Platform. Votre espace de nom doit utiliser [required PodSecurityPolicy](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install-prereqs-pod-security-requirements). Si vous prévoyez de créer plusieurs réseaux de blockchain, par exemple pour créer différents environnements de développement, de transfert et de production, vous devez créer un espace de nom unique pour chaque environnement.

3. Procédez à l'extraction de la valeur de l'adresse IP proxy du cluster de votre autorité de certification depuis la console {{site.data.keyword.cloud_notm}} Private. **Remarque :** Vous devrez être [administrateur de cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} pour accéder à votre IP proxy. Connectez-vous au cluster {{site.data.keyword.cloud_notm}} Private. Dans le panneau de navigation gauche, cliquez sur **Plateforme** puis sur **Noeuds** pour afficher les noeuds qui sont définis dans le cluster. Cliquez sur le noeud avec le rôle `proxy`, puis copiez la valeur de l'`IP hôte` de la table. **Important :** Conservez cette valeur car vous allez l'utiliser lors de la configuration de la zone `Adresse IP du proxy` de la charte Helm.


Conservez cette valeur car vous allez l'utiliser lors de la configuration de la zone `Adresse IP du proxy` de la charte Helm.
{:important}

4. Créez un mot de passe que vous utiliserez pour vous connecter à la console pour la première fois et stockez-le dans un objet secret dans {{site.data.keyword.cloud_notm}} Platform. Vous trouverez les étapes de création de ce secret dans la [section suivante](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp-password-secret).

## Création d'un secret de mot de passe de console
{: #console-deploy-icp-password-secret}

Avant d'accéder à votre console, vous devez créer un mot de passe par défaut que vous utiliserez pour votre première connexion à la console. Créez un [secret Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/){: external} pour stocker le mot de passe avant le déploiement de la console. Un secret Kubernetes vous permet de protéger et de partager des informations sans avoir à les transmettre directement au déploiement.

1. Créez un mot de passe et codez-le au format base64. Exécutez la commande suivante dans une fenêtre de terminal et remplacez la valeur `password` par la valeur de votre choix. **Sauvegardez la sortie de cette commande**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Connectez-vous à la console {{site.data.keyword.cloud_notm}} Private. Dans le panneau de navigation gauche, cliquez sur **Configuration** puis sur **Secrets**. Cliquez sur le bouton **Créer Secret** pour ouvrir un panneau en incrustation qui vous permet de générer un nouvel objet secret.

3. Sous l'onglet **Général**, remplissez les zones suivantes :
  - **Nom :** Donnez un nom unique à votre secret au sein de votre cluster. Vous utiliserez ce nom lors du déploiement de votre console. Il doit être entièrement en minuscules.
  - **Espace de nom :** espace de nom pour l'ajout de votre secret. Sélectionnez l'`espace de nom` dans lequel vous voulez déployer votre console.
  - **Type :** entrez la valeur `generic`.

4. Laissez l'onglet **Annotations** à blanc.

5. Sous l'onglet **Données**, ajoutez le nom d'utilisateur et le mot de passe en tant que paires clé-valeur.
  1. Dans la première zone **Nom**, entrez `password`.
  2. Dans la première zone **Valeur**, entrez le résultat de `echo -n 'password' | base64` from step 1.
  3. Cliquez sur **Créer** pour former un nouvel objet Secret.

Entrez le nom du secret dans la zone `Console administrator password secret name` de la page de configuration lors du déploiement de votre console. Vous devrez utiliser la valeur codée à l'étape 1 pour la première connexion à la console. Cette valeur deviendra le mot de passe par défaut de la console sauf si ce dernier est modifié par un administrateur de la console. Pour plus d'informations, voir [Gestion des utilisateurs depuis la console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Création d'un secret TLS (facultatif)
{: #console-deploy-icp-tls-secret}

La console utilise TLS pour sécuriser les communications entre la console et vos noeuds de blockchain. Par défaut, la console génère ses propres certificats TLS. Toutefois, vous avez la possibilité de fournir vos propres certificat et clé TLS. Procédez comme suit pour stocker les certificats dans un [secret Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/). Vous pouvez ensuite fournir ces certificats à la console en indiquant le nom du secret dans la zone du secret TLS lors de la configuration.

1. Convertissez le certificat TLS et la clé privée au format base64. Vous pouvez convertir le contenu du certificat ou du fichier de clés en une chaîne base64 en exécutant la commande suivante sur votre machine locale :

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  Exécutez la commande suivante pour votre certificat TLS et la clé TLS associée. **Sauvegardez** le résultat.

2. Connectez-vous à la console {{site.data.keyword.cloud_notm}} Private. Dans le panneau de navigation gauche, cliquez sur **Configuration** puis sur **Secrets**. Cliquez sur le bouton **Créer Secret** pour ouvrir un panneau en incrustation qui vous permet de générer un nouvel objet secret.

3. Sous l'onglet **Général**, remplissez les zones suivantes :
  - **Nom :** Donnez un nom unique à votre secret au sein de votre cluster. Vous utiliserez ce nom lors de la configuration de votre autorité de certification. Il doit être entièrement en minuscules.
  - **Espace de nom :** espace de nom pour l'ajout de votre secret. Sélectionnez l'`espace de nom` dans lequel vous voulez déployer votre autorité de certification.
  - **Type :** entrez la valeur `kubernetes.io/tls`.

4. Laissez l'onglet **Annotations** à blanc.

5. Sous l'onglet **Données**, ajoutez le nom d'utilisateur et le mot de passe en tant que paires clé-valeur.
  1. Dans la première zone **Nom**, entrez `tls.crt`.
  2. Dans la première zone **Valeur**, entrez la chaîne de votre certificat TLS codé au format base64 à l'étape 1.
  3. Cliquez sur le bouton **Ajouter des données** pour ajouter une seconde paire clé-valeur.
  4. Dans la première zone **Nom**, entrez `tls.key`.
  5. Dans la première zone **Valeur**, entrez la chaîne de votre clé TLS codée au format base64 à l'étape 1.
  6. Cliquez sur **Créer** pour former un nouvel objet Secret.

Entrez le nom du secret que vous créez dans la zone `TLS secret` de la page de tous les paramètres de la page de configuration lors du déploiement de votre console. 

Les secrets ne sont pas supprimés de votre cluster {{site.data.keyword.cloud_notm}} Private lorsque vous supprimez votre édition Helm. Vous êtes responsable de la gestion du mot de passe et des secrets TLS dans votre cluster {{site.data.keyword.cloud_notm}} Private. Si vous prévoyez de déployer une autre console dans le futur, vous pouvez réutiliser ces secrets. Sinon, vous êtes responsable de leur suppression dans votre cluster {{site.data.keyword.cloud_notm}} Private.{:note}

## Configuration
{: #console-deploy-icp-configuration}

Une fois l'objet du secret TLS créé, vous pouvez déployer la console en suivant les étapes ci-après.

1. Connectez-vous à la console {{site.data.keyword.cloud_notm}} Private et cliquez sur **Catalogue** dans l'angle supérieur droit.
2. Cliquez sur `Blockchain` dans le panneau de navigation gauche et localisez la vignette libellée `ibm-blockchain-platform-prod`. Cliquez sur la vignette afin de l'ouvrir. Vous devez voir un fichier Readme qui contient des informations sur l'installation et la configuration de la charte Helm.
3. Cliquez sur l'onglet **Configuration** dans la partie supérieure du panneau ou cliquez sur le bouton **Configurer** dans l'angle inférieur droit.
4. Indiquez les valeurs relatives aux [paramètres de configuration et de sécurité des pods](#icp-peer-deploy-configuration-parms) et acceptez le contrat de licence.
5. Accédez à la section **Paramètres** : 
- Vous pouvez déployer la console en utilisant uniquement les [paramètres de démarrage rapide](#icp-peer-deploy-quickstart-parms). Utilisez cette option si vous faites des essais ou si vous démarrez.
- Vous pouvez utiliser la section [Tous les paramètres](#icp-peer-deploy-quickstart-parms) pour personnaliser l'accès réseau, les ressources, ainsi que le stockage utilisé par votre console. La section **Tous les paramètres** n'est recommandée que pour les utilisateurs de Kubernetes les plus expérimentés.
6. Cliquez sur **Installer**.

Les tableaux ci-après décrivent les zones de paramètres de configuration et leurs valeurs par défaut.

### Paramètres de configuration et de sécurité de pod
{: #icp-peer-deploy-configuration-parms}

|  Paramètre     | Description    | Val. déf  | Requis |
| --------------|-----------------|-------|------- |
| `Helm release name`| Nom identifiant le déploiement de votre édition Helm. Il doit commencer par une lettre minuscule et se terminer par une caractère alphanumérique, doit contenir uniquement des traits d'union et des caractère alphanumérique minuscules. | Aucune | Oui |
| `Target namespace`| Indiquez l'espace de nom que vous avez créé pour votre console et vos composants. L'espace de nom doit inclure une politique `ibm-privileged-psp`. Sinon, une règle [bind a PodSecurityPolicy](https://cloud.ibm.com/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) est lié à votre espace de nom. | Aucune | Oui |
| `Target Cluster`| Indiquez les clusters dans lesquels vous souhaitez déployer la ressource. Les clusters disponibles sont filtrés en fonction de l'espace de nom sélectionné et de la version Kubernetes requise. | Aucune | Oui |
| `Target namespace policies`| Préconfiguré pour l'utilisation de votre espace de nom cible. Les valeurs doivent inclure la politique `ibm-privileged-psp` ou `ibm-blockchain-platform-psp`. | Aucune | Oui |

### Démarrage rapide et Tous les paramètres
{: #icp-peer-deploy-quickstart-parms}

Utilisez la section de démarrage rapide si vous faites des essais ou si vous démarrez. La section Tous les paramètres vous permet de personnaliser l'accès réseau, les ressources, ainsi que le stockage et elle n'est recommandée que pour les utilisateurs de Kubernetes les plus expérimentés.

**Cliquez sur l'onglet Tous les paramètres afin d'afficher tous les paramètres de configuration.**

|  Paramètre     | Description    | Val. déf  | Requis |
| --------------|-----------------|-------|------- |
| **Paramètres de la console** | **Administration et déploiement de votre console** | | |
| `Proxy IP` | Adresse IP du noeud proxy de votre cluster. | Aucune| Oui |
| `Console administrator email` | E-mail utilisé pour la connexion à la console. | Aucune | Oui |
| `Console administrator password secret name` | Nom du secret que vous avez [créé pour stocker le mot de passe](#console-deploy-icp-password-secret) et que vous utiliserez pour la connexion à la console. | Aucune | Oui |
| **Paramètres réseau** | **Accès réseau à votre console** | | |
| `Console hostname` | Entrez la même valeur que pour l'IP proxy. | Aucune | Oui |
| `Console port` | Entrez le port que vous souhaitez utiliser dans la plage 31210 - 31220. Ce port ne peut pas être utilisé par une autre application. | Aucune | Oui |
| **Storage settings** | **Stockage qui doit être utilisé par votre console** | | |
| `Storage class name` | Entrez le nom de la classe de stockage qui doit être utilisée par la console et les composants que vous créez. | Aucune | Oui |
{: caption="Table 1. Paramètres de démarrage rapide" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Paramètre     | Description    | Val. déf  | Requis |
| --------------|-----------------|-------|------- |
| `Architecture` | Sélectionnez votre architecture de plateforme cloud. (AMD64 ou S390x) | AMD64 | Non |
| **Paramètres de la console** | **Administration et déploiement de votre console** | | |
| `Service account name` | Compte chez un fournisseur qui doit être utilisé par l'opérateur. | Aucune | Non |
| `Proxy IP` | Adresse IP du noeud proxy de votre cluster. | Aucune | Oui |
| `Console administrator email` | E-mail utilisé pour la connexion à la console. | Aucune | Oui |
| `Console administrator password secret name` | Nom du secret que vous avez [créé pour stocker le mot de passe](#console-deploy-icp-password-secret) et que vous utiliserez pour la connexion à la console. | Aucune | Oui |
| **Docker image settings** | **Utilisez ces paramètres pour personnaliser les images Fabric qui doivent être extraites par la console** | | |
| `imagePullSecret name` | imagePullSecret à utiliser pour le téléchargement des images. | `ibp-ibmregistry` | Non |
| **Paramètres réseau** | **Accès réseau à votre console** | | |
| `Console hostname` | Entrez la même valeur que pour l'IP proxy. | Aucune | Oui |
| `Console port` | Entrez un port que vous souhaitez utiliser dans la plage 31210 - 31220. | Aucune | Oui |
| `Proxy hostname` | Nom d'hôte du serveur proxy.  Entrez la même valeur que pour l'IP proxy. | Aucune | Non |
| `Proxy port` | Entrez un port que vous souhaitez utiliser dans la plage 31210 - 31220. Ce port ne peut pas être utilisé par une autre application ou la console. | Aucune | Non |
| `Configtxlator hostname` | Entrez la même valeur que pour l'IP proxy. | Aucune | Non |
| `Configtxlator port` | Entrez un port que vous souhaitez utiliser dans la plage 31210 - 31220. Ce port ne peut pas être utilisé par une autre application ou la console. | Aucune | Non |
| `TLS secret` | Nom du secret que vous avez [créé pour stocker les certificats TLS](#console-deploy-icp-tls-secret) qui seront utilisés par la console. | Aucune | Non |
| **Data persistence**| **Activation de la persistance des données et de la mise à disposition dynamique** | | |
| `Enable data persistence`| Activation de la capacité de persistance des données après un redémarrage ou une défaillance du cluster. Pour plus d'informations, voir la rubrique relative au [stockage dans Kubernetes](https://kubernetes.io/docs/concepts/storage/). *Si ce paramètre n'est pas défini en cas de reprise en ligne ou de redémarrage du pod.* | enabled | Non |
| `Enable dynamic provisioning`| Sélectionnez pour activer la mise à disposition dynamique pour les volumes de stockage. | enabled | Non |
| **Storage settings**| **Allocation d'espace de stockage pour votre console et outils** | | |
| `Volume claim size`| Taille de la Réservation de volume persistant à mettre à disposition. | 10Gi  | Non |
| `Storage class name`| Nom de la classe de stockage qui doit être utilisée par la console et les composants que vous créez. | Aucune | Oui |
| `Existing volume claim`| Indiquez le nom d'une Réservation de volume persistant existante et laissez toutes les autres zones vides. | Aucune | Non |
| `Selector label`| [Libellé du sélecteur](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) pour votre PVC| Aucune | Non |
| `Selector value`| [Valeur du sélecteur](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) pour votre PVC | Aucune | Non |
| `Storage access mode`| Indiquez le [mode d'accès](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes){: external} du stockage pour le PVC.  | ReadWriteMany | Non |
| `PVC name`| Pour une nouvelle réservation uniquement. Entrez un nom pour votre nouveau PVC. | Nom de l'édition par défaut. | Non |
| **Allocate resources**| **Allocation de ressources à la console** | | |
| `Opstools CPU limit` | Nombre maximum d'UC à allouer au composant opstools. | 500m | Non |
| `Opstools memory limit` | Quantité maximum de mémoire à allouer au composant opstools. | 1000Mi | Non |
| `Opstools CPU request` | Nombre minimum d'UC à allouer au composant opstools. | 500m | Non |
| `Opstools memory request` | Quantité minimum de mémoire à allouer au composant opstools. | 1000Mi | Non |
| `Configtxlator CPU limit` | Nombre maximum d'UC à allouer à l'outil configtxlator. | 25m | Non |
| `Configtxlator memory limit` | Quantité maximum de mémoire à allouer à l'outil configtxlator. | 50Mi | Non |
| `Configtxlator CPU request` | Nombre minimum d'UC à allouer à l'outil configtxlator. | 25m | Non |
| `Configtxlator memory request` | Quantité minimum de mémoire à allouer à l'outil configtxlator. | 50Mi | Non |
| `CouchDB CPU limit` | Nombre maximum d'UC à allouer à CouchDB. | 500m | Non |
| `CouchDB memory limit` | Quantité maximum de mémoire à allouer à CouchDB. | 1000Mi | Non |
| `CouchDB CPU request` | Nombre minimum d'UC à allouer à CouchDB.| 500m | 500m |
| `CouchDB memory request` | Quantité minimum de mémoire à allouer à CouchDB.| 1000Mi | Non |
| `Operator CPU limit` | Nombre maximum d'UC à allouer au composant operator. | 100 m | Non |
| `Operator memory limit` | Quantité maximum de mémoire à allouer au composant operator. | 200 Mi | Non |
| `Operator CPU request` | Nombre minimum d'UC à allouer au composant operator. | 100 m | Non |
| `Operator memory request` | Quantité minimum de mémoire à allouer au composant operator. | 200 Mi | Non |
| `Deployer CPU limit` | Nombre maximum d'UC à allouer au composant deployer. | 100 m | Non |
| `Deployer memory limit` | Quantité maximum de mémoire à allouer au composant deployer. | 200 Mi | Non |
| `Deployer CPU request` | Nombre minimum d'UC à allouer au composant deployer. | 100 m | Non |
| `Deployer memory request` | Quantité minimum de mémoire à allouer au composant deployer. | 200 Mi | Non |
{: caption="Tableau 2. Tous les paramètres" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Utilisation de la ligne de commande Helm pour installer l'édition Helm
{: #console-deploy-icp-helm-cli}

Vous pouvez aussi utiliser l'interface de ligne de commande `helm` pour installer l'édition Helm. Avant d'exécuter la commande `helm install`, assurez-vous d'avoir [ajouté le référentiel Helm de votre cluster à l'environnement CLI Helm](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}.

Vous pouvez définir les paramètres requis pour l'installation en créant un fichier `yaml` et en le transmettant à la commande `helm install` suivante.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Où :
- `<helm_release name>` représente le nom que vous voulez affecter à votre édition Helm.
- `<helm_chart>` représente le nom de la charte Helm importée dans le catalogue.
- `<helm_chart_version>` représente la version de la charte Helm importée dans le catalogue.
- `<customvalues.yaml>` est le nom du fichier yaml qui contient les paramètres de configuration.

Par exemple :
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

Vous pouvez créer un nouveau fichier `yaml` en éditant `values.yaml` inclus dans le fichier archive téléchargé, lequel inclut tous les paramètres nécessaires avec leurs valeurs par défaut.

## Vérification de l'installation

Une fois que vous avez entré les paramètres de configuration et cliqué sur le bouton **Installer**, cliquez sur le bouton **Afficher l'édition Helm** pour afficher votre déploiement. Si l'opération aboutit, vous devez voir la valeur 1 dans les zones `DESIRED`, `CURRENT`, `UP TO DATE` et `AVAILABLE` dans le tableau Déploiement. Vous devrez peut-être cliquer sur Actualiser et attendre que le tableau soit mis à jour.

Pour afficher les détails de votre déploiement; accédez à l'écran de présentation du **déploiement**, puis cliquez sur le pod qui a été créé. Le déploiement de la charte Helm crée cinq conteneurs dans votre cluster:
- **opstools** : interface utilisateur de la console.
- **deployer** : outil qui permet à votre console de communiquer avec vos déploiements.
- **configtxlator** : outil utilisé par la console pour lire et créer les mises à jour de canal.
- **couchdb** : instance de CouchDB qui stocke les données de votre console, y compris vos informations d'autorisation.
- **operator** : opérateur Kubernetes qui gère vos déploiements de composant.

## Connexion à la console

Vous pouvez utiliser votre navigateur pour accéder à la console après l'installation. Vous trouverez l'URL dans la section **Notes :** de l'écran de présentation de l'édition Helm qui s'affiche après le déploiement. Vérifiez que vous n'utilisez pas la version ESR de Firefox. Si tel est le cas, utilisez un autre navigateur tel que Chrome et réessayez.

Dans votre navigateur, vous devez voir l'écran de connexion à la console :
- En regard de **ID utilisateur**, entrez la valeur fournie dans la zone `E-mail de l'administrateur de console` durant la configuration.
- En regard de **Mot de passe**, entrez la valeur que vous avez codée et stockée dans le [secret de mot de passe](#console-deploy-icp-password-secret) puis transmise à la console durant la configuration. Ce mot de passe deviendra le mot de passe par défaut de la console que tous les nouveaux utilisateurs utiliseront pour se connecter à la console. Après la connexion initiale, vous êtes invité à fournir un nouveau mot de passe que vous pouvez utiliser pour la connexion à la console.

L'administrateur qui a mis à disposition la charte Helm peut accorder à d'autres utilisateurs l'accès à la console et indiquer les opérations qu'ils peuvent effectuer. Pour plus d'informations, voir [Gestion des utilisateurs depuis la console](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage-users).

## Etapes suivantes
{: #console-deploy-icp-next-steps}

Une fois que vous avez accès à votre console, vous pouvez voir l'onglet **nodes** de l'interface utilisateur de votre console. Vous pouvez utiliser cet écran pour déployer des composants dans votre cluster local. Consultez le tutoriel [Générer un réseau](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) pour commencer à utiliser la console. Vous pouvez également utiliser cet onglet pour exploiter les noeuds qui ont été créés dans d'autres clouds. Pour plus d'informations, voir [Importation de noeuds](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).

Pour en savoir plus sur la gestion des utilisateurs qui peuvent accéder à la console, utilisez les API {{site.data.keyword.blockchainfull_notm}} Platform et consultez les journaux de votre console et composants de blockchain, et enfin consultez la section [Administration de votre console dans {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage).
