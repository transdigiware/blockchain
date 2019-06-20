---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: APIs, build a network, authentication, service credentials, API key, API endpoint, IAM access token, Fabric CA client, import a network, generate certificates

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

# Génération d'un réseau avec des API
{: #ibp-v2-apis}

{{site.data.keyword.blockchainfull}} Platform expose des API RESTful pour vous permettre de créer, importer, éditer et supprimer vos composants de blockchain, mais également de gérer la journalisation, les notifications et les paramètres de console. Vous pouvez utiliser ces API, et les logiciels SDK correspondants, pour développer des applications qui interagissent avec votre réseau de blockchain.
{: shortdesc}

Ce tutoriel présente le flux générique permettant de générer un réseau de blockchain avec des API {{site.data.keyword.blockchainfull_notm}} Platform. Pour plus d'informations sur chaque API, voir [{{site.data.keyword.blockchainfull_notm}} Platform API reference doc](/apidocs/blockchain){: external}.

Ces API sont compatibles avec {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} version 0.1.77 ou suivante. Pour vérifier la version, accédez à `https://[your_console_url]/version.txt`, où *`[your_console_url]`* est l'URL de votre console {{site.data.keyword.blockchainfull_notm}} Platform. Par exemple : https://1ee1869ffa6496d6bc1ce4b-optools.bp01.blockchain.cloud.ibm.com/version.txt
{:note}

## Avant de commencer
{: #ibp-v2-apis-prereq}

Vous devez disposer d'une instance de service {{site.data.keyword.blockchainfull_notm}} Platform dans {{site.data.keyword.cloud_notm}} afin de pouvoir émettre des appels d'API pour interagir avec le réseau. Si vous n'avez pas encore d'instance de service, suivez les [instructions de la section Mise en route](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) pour en créer une.

## Authentification
{: #ibp-v2-apis-authentication}

Pour utiliser les API afin d'accéder au réseau de blockchain que vous créez avec {{site.data.keyword.blockchainfull_notm}} Platform, votre application doit pouvoir s'authentifier auprès de {{site.data.keyword.cloud_notm}} et se connecter à votre instance de service.

### Extraction des données d'identification de service
{: #ibp-v2-apis-retrieve-service-credentials}

Vous avez besoin de données d'identification de base afin de garantir que vous avez accès à votre instance de service {{site.data.keyword.blockchainfull_notm}} Platform dans {{site.data.keyword.cloud_notm}}.

1. Dans votre liste de ressources [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/resources){: external}, ouvrez l'instance de service de blockchain que vous avez créée.
2. Cliquez sur **Données d'identification du service** dans le navigateur de gauche.
3. Cliquez sur le bouton **Nouvelles données d'identification** dans la page **Données d'identification du service** pour créer de nouvelles données d'identification.
  1. Attribuez un nom à ces données d'identification, par exemple *UseAPIs*.
  2. Vous pouvez laisser la zone **Ajouter des paramètres de configuration en ligne** à blanc.
  3. Cliquez sur le bouton **Ajouter**.
4. Une fois les nouvelles données d'identification créées, cliquez sur **Afficher les données d'identification** sous l'en-tête **ACTIONS** de ces données d'identification. Le contenu des données d'identification est semblable à l'exemple suivant :

  ```
  {
    "api_endpoint": "https://1088ac8563e44f5a92539d946733ad7e-optools.so01.blockchain.test.cloud.ibm.com"
    "apikey": "nvASKts6B6lMZGZUNTGVP7MLK2BujMnxz0plSPYaqc3R",
    "configtxlator": "https://1088ac8563e44f5a92539d946733ad7e-configtxlator.so01.blockchain.test.cloud.ibm.com",
    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:blockchain:us-south:a/9d46037caee397faa45c55e087d26baa:1088ac85-63e4-4f5a-9253-9d946733ad7e::",
    "iam_apikey_name": "auto-generated-apikey-c1981f5c-cff0-464f-9a78-ed2f52e24d1a",
    "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
    "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/9d46037caee397faa45c55e087d26baa::serviceid:ServiceId-774190c5-9c37-4f68-8572-8d6e2aabbc7e",
  }
  ```

Dans les données d'identification du service, vous pouvez trouver la **clé d'API** (`apikey`) et le **noeud final de l'API** (`api_endpoint`) dont vous avez besoin pour obtenir un jeton d'accès {{site.data.keyword.iamshort}} (IAM).

### Extraction d'un jeton d'accès
{: #ibp-v2-apis-retrieve-token}

Vous pouvez vous authentifier auprès d'{{site.data.keyword.blockchainfull_notm}} Platform en obtenant un jeton d'accès d'IAM. Avec un jeton d'accès, vous pouvez gérer l'API d'{{site.data.keyword.blockchainfull_notm}} Platform au nom de votre service ou application ou à l'extérieur de {{site.data.keyword.cloud_notm}}, sans avoir à partager vos données d'identification utilisateur personnelles.  

Appelez l'API {{site.data.keyword.iamshort}} pour extraire votre jeton d'accès.

```cURL
curl -X POST \
  "https://iam.cloud.ibm.com/identity/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \
```
{: codeblock}

Dans la demande, remplacez `<API_KEY>` par la valeur `apikey` de vos données d'identification du service. L'exemple partiel suivant montre la sortie du jeton :

```
{
"access_token": "eyJraWQiOiIyM...",
"refresh_token": "...",
"expires_in":3600,
"expiration":1555558683,
"scope":"ibm openid"}
```
{: screen}

Utilisez la valeur `access_token` complète, préfixée du type de jeton `Bearer`, pour gérer à l'aide d'un programme votre réseau de blockchain avec les API.

Les jetons d'accès sont valides pendant une heure, mais vous pouvez les régénérer si nécessaire. Pour maintenir l'accès au service, actualisez régulièrement le jeton d'accès pour votre clé d'API en appelant l'API {{site.data.keyword.iamshort}}.   
{: tip}

## Mise en forme de la demande d'API
{: #ibp-v2-apis-form-api-request}

Lorsque vous effectuez un appel API au service, structurez votre demande API conformément à la façon vous avez initialement mis à disposition votre instance de service {{site.data.keyword.blockchainfull_notm}} Platform.

Pour générer votre demande, appairez un noeud final d'API aux données d'authentification appropriées au format suivant :

```cURL
curl -X <API method> \
    <API_endpoint>/<API> \
    -H 'authorization: Bearer <access_token>' \
    -H 'Content-Type: application/json' \
    -d '<request body>' \
```
{: codeblock}

Des exemples de commandes curl sont fournis pour chaque API dans la documentation [{{site.data.keyword.blockchainfull_notm}} Platform API reference doc](/apidocs/blockchain){: external}.

Vous pouvez également utiliser la fonction **Try it out** de la documentation API Reference pour tester vos appels API avant de les ajouter à vos applications. Vous devez être connecté à {{site.data.keyword.cloud_notm}} pour pouvoir utiliser la fonction **Essayez**. Vous pouvez sélectionner une instance de service dans la liste déroulante. Toutes les demandes d'API sont envoyées au réseau spécifié dans le noeud final de l'API.

## Limitations
{: #ibp-v2-apis-limitations}

Vous pouvez uniquement importer une autorité de certification existant, ainsi que des noeuds de service de tri d'autres réseaux {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

## Génération d'un réseau à l'aide d'API
{: #ibp-v2-apis-build-with-apis}

Vous pouvez utiliser des API pour créer des réseaux de blockchain dans votre instance d'{{site.data.keyword.blockchainfull_notm}} Platform. Procédez comme suit pour générer un réseau de blockchain à l'aide d'API {{site.data.keyword.blockchainfull_notm}}.

1. Créez une autorité de certification en appelant [`POST /ak/api/v1/kubernetes/components/ca`](/apidocs/blockchain?code=try#create-a-ca).

  Notez l'entrée et la réponse, car vous en aurez besoin ultérieurement.
  {: tip}

  Vous devez attendre que l'autorité de certification démarre. Plusieurs minutes peuvent être nécessaires selon l'environnement. Vous pouvez appeler l'API `GET <ca_url>/cainfo` pour vérifier l'état de votre AC. Vous obtiendrez des erreurs répétées, puis un code d'état `200`, qui signifie que vous pouvez passer à l'étape suivante. Notez que cet appel d'API expire au bout d'une minute.

2. Utilisez votre autorité de certification pour enregistrer votre composant et les identités des administrateurs, puis générer les certificats nécessaires. Vous pouvez utiliser le client d'autorité de certification Fabric pour effectuer les étapes suivantes :

  - [Configurer le client d'autorité de certification Fabric](#ibp-v2-apis-config-fabric-ca-client).
  - [Générer des certificats avec votre admin d'autorité de certification](#ibp-v2-apis-enroll-ca-admin).
  - [Enregistrer le nouveau composant auprès de votre autorité de certification](#ibp-v2-apis-config-register-component).
  - Vous devez également [enregistrer un administrateur d'organisation](#ibp-v2-apis-config-register-admin) puis [générer des certificats pour l'admin](#ibp-v2-apis-config-enroll-admin) dans un dossier MSP. Cette étape n'est pas nécessaire si vous avez déjà enregistré l'identité de votre admin.
  - [Enregistrer le nouveau composant auprès de votre autorité de certification TLS](#ibp-v2-apis-config-register-component-tls).

  Vous pouvez également effectuer ces étapes à partir de votre console {{site.data.keyword.blockchainfull_notm}} Platform. Pour plus d'informations, voir [Création et gestion des identités](/docs/services/blockchain/howto/ibp-console-identities.html).

3. [Créer une définition MSP pour votre organisation](#ibp-v2-apis-msp) en appelant [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?#import-a-membership-service-provide-msp).

4. [Générer le fichier de configuration](#ibp-v2-apis-config) qui est nécessaire à la création d'un service de tri ou d'un homologue. Vous devez générer un fichier de configuration unique pour chaque service de tri ou homologue que vous souhaitez créer.

5. Créer un service de tri en appelant [`poster/ak/api/v1/kubernetes/components/orderer`](/apidocs/blockchain?code=try#create-an-orderer).

6. Créer un homologue en appelant [`POST /ak/api/v1/kubernetes/components/peer`](/apidocs/blockchain?code=try#create-a-peer).

7. Si vous souhaitez utiliser la console pour gérer vos composants de blockchain, vous devez importer l'identité de votre administrateur dans votre portefeuille de console. Utilisez l'onglet Portefeuille pour importer le certificat et la clé privée de votre admin de noeud sur la console et créer une identité. Vous devez ensuite utiliser la console pour associer cette identité aux composants que vous avez créés. Pour plus d'informations, voir [Importation d'une identité admin sur la console {{site.data.keyword.blockchainfull_notm}} Platform](#ibp-v2-apis-admin-console).

8. Après avoir déployé votre réseau, vous pouvez utiliser des logiciels SDK Fabric, l'interface CLI de l'homologue, ou l'interface utilisateur de la console pour créer des canaux et installer ou instancier des contrats intelligents.

Les données d'identification du service qui sont utilisées pour l'authentification API doivent avoir le rôle `Gestionnaire` dans IAM pour pouvoir créer des composants. Consultez le tableau de la rubrique relative aux [rôles utilisateur](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) pour plus d'informations.
{: note}

## Importation d'un réseau à l'aide d'API
{: #ibp-v2-apis-import-with-apis}

Vous pouvez également utiliser les API pour importer des composants {{site.data.keyword.blockchainfull_notm}} créés à l'aide des API ou de la console {{site.data.keyword.blockchainfull_notm}} Platform dans une autre instance de service d'{{site.data.keyword.blockchainfull_notm}} Platform.

1. Importez une autorité de certification en appelant [`POST /ak/api/v1/components/ca`](/apidocs/blockchain?code=try#import-a-ca).

  Notez l'entrée et la réponse, car vous en aurez besoin ultérieurement.
  {: tip}

  Vous devez attendre que l'autorité de certification démarre. Plusieurs minutes peuvent être nécessaires selon l'environnement. Vous pouvez appeler l'API [`GET /components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) pour vérifier l'état de l'autorité de certification. Vous obtiendrez des erreurs répétées, puis un code d'état `200` avant de passer à l'étape suivante. Notez que cet appel d'API expire au bout d'une minute.

2. Importez la définition MSP d'une organisation en appelant [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp).

3. Importez un service de tri en appelant [`POST /ak/api/v1/components/orderer`](/apidocs/blockchain?code=try#import-a-orderer).

4. Importez un homologue en appelant [`POST /ak/api/v1/components/peer`](/apidocs/blockchain?code=try#import-a-peer).

5. Si vous prévoyez d'utiliser la console {{site.data.keyword.blockchainfull_notm}} Platform pour exploiter vos composants de blockchain, vous devez importer les identités de votre administrateur de composant dans votre portefeuille de console. Pour plus d'informations, voir [Importation d'une identité admin sur la console {{site.data.keyword.blockchainfull_notm}} Platform](#ibp-v2-apis-admin-console).

6. Après avoir déployé votre réseau, vous pouvez utiliser des logiciels SDK Fabric, l'interface CLI de l'homologue, ou l'interface utilisateur de la console pour créer des canaux et installer ou instancier des contrats intelligents.

Les données d'identification du service qui sont utilisées pour l'authentification API doivent avoir le rôle `Auteur` dans IAM pour pouvoir importer des composants. Consultez le tableau de la rubrique relative aux [rôles utilisateur](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) pour plus d'informations.
{: note}

## Utilisation de votre autorité de certification à l'aide du client d'autorité de certification Fabric
{: #ibp-v2-apis-config-fabric-ca-client}

Vous pouvez utiliser le client d'autorité de certification Fabric pour exploiter vos autorités de certification. Exécutez les commandes de client d'autorité de certification Fabric suivantes pour enregistrer votre composant et les identités des administrateurs, puis générer les certificats nécessaires.

### Configuration du client d'autorité de certification Fabric
{: #ibp-v2-apis-setup-fabric-ca-client}

1. Téléchargez le [client CA Fabric](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#fabric-ca-client){: external} sur votre système de fichiers local.

  Le moyen le plus simple d'obtenir le client d'autorité de certification Fabric est de télécharger tous les binaires de l'outil Fabric directement. Accédez à un répertoire dans lequel vous souhaitez télécharger les binaires à partir de votre ligne de commande, puis procédez à leur extraction en exécutant la commande [Curl](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html#install-curl){: external}.

  ```
  curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.0 1.4.0 -d -s
  ```
  {:codeblock}

  Cette commande installe les binaires dans un répertoire `bin/`.

2. Définissez le chemin `PATH` d'accès au répertoire dans lequel vous avez téléchargé les binaires de l'outil Fabric :

  ```
  export PATH=$PATH:<full/path/to/fabric-client/bin>
  ```
  {:codeblock}

  Par exemple, si vous avez installé les binaires dans votre répertoire de base, vous définiriez votre `PATH` comme suit :

  ```
  export PATH=$PATH:$HOME/bin
  ```
  {:codeblock}

3. Créez le dossier dans lequel vous allez stocker les certificats de l'admin de votre autorité de certification.

  ```
  mkdir -p $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

4. Définissez la valeur de la variable d'environnement `$FABRIC_CA_CLIENT_HOME` sur le chemin dans lequel le client d'autorité de certification va stocker les certificats [MSP](/docs/services/blockchain/howto/CA_operate.html#ca-operate-msp) générés. Vérifiez que vous avez supprimé la configuration matérielle qui a peut-être été créée par des tentatives préalables. S'il s'agit de la première fois que vous exécutez la commande `enroll`, le dossier `msp` et le fichier `.yaml` n'existent pas.

  ```
  export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/ca-admin
  rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
  rm -rf $FABRIC_CA_CLIENT_HOME/msp
  ```
  {:codeblock}

5. Procédez à l'extraction du certificat TLS de votre autorité de certification qui doit être utilisé par le client d'autorité de certification Fabric. Si vous utilisez la console {{site.data.keyword.blockchainfull_notm}} Platform, ouvrez l'autorité de certification et cliquez sur **Paramètres**, puis recherchez le certificat au format base64 dans la zone **Certificat TLS**. Si vous utilisez les API, vous pouvez appeler [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) et rechercher le certificat TLS de l'autorité de certification dans la zone `"PEM"`. Si vous avez créé l'autorité de certification à l'aide de l'API `Create a Fabric CA`, vous pouvez également trouver le certificat TLS dans le corps de réponse.

  Vous devez convertir le certificat du format base64 au format PEM pour pouvoir l'utiliser pour communiquer avec votre autorité de certification. Insérez la chaîne codée en base64 du certificat TLS dans la commande ci-dessous. Vérifiez que vous êtes au niveau du répertoire `$HOME/fabric-ca-client`.

  ```
  cd $HOME/fabric-ca-client
  mkdir catls
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > tls.pem
  ```
  {:codeblock}

### Génération de certificats avec votre admin d'autorité de certification
{: #ibp-v2-apis-enroll-ca-admin}

Une identité **CA admin** a été automatiquement enregistrée pour vous lors de la création de votre autorité de certification. Vous pouvez maintenant utiliser ce nom d'administrateur et ce mot de passe admin pour émettre une commande `enroll` auprès du client d'autorité de certification Fabric afin de générer un dossier MSP avec des certificats qui peuvent ensuite être utilisés pour enregistrer d'autres identités d'homologue ou de service de tri.

1. Vérifiez que vous avez effectué les étapes relatives à la [configuration client d'autorité de certification Fabric](/docs/services/blockchain/howto/CA_operate.html#ca-operate-fabric-ca-client) et que `$FABRIC_CA_CLIENT_HOME` est défini sur le répertoire où vous souhaitez stocker vos certificats d'admin de l'autorité de certification.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

2. Exécutez la commande suivante pour générer des certificats :

  ```
  fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name>  --tls.certfiles  <ca_tls_cert_path>
  ```
  {:codeblock}

  `<enroll_id>` et `<enroll_password>` dans la commande correspondent aux éléments `enroll_id` et `enroll_secret` que vous avez spécifiés lors de la création de l'autorité de certification. Utilisez l'**URL de noeud final de l'autorité de certification** depuis votre console {{site.data.keyword.blockchainfull_notm}} Platform ou l'élément `"ca_url"` retourné par votre appel d'API comme valeur de `<ca_url_with_port>`. Retirez `http://` au début. `<ca_name>` est le **Nom d'autorité de certification** de votre console, ou l'élément `"ca_name"` retourné par les API.

  `<ca_tls_cert_path>` est le chemin d'accès complet au certificat TLC de votre autorité de certification.

  Un appel réel doit ressembler à ce qui suit dans l'exemple de commande :

  ```
  fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```  
  {:codeblock}

**Astuce :** Si la valeur de l'URL d'inscription, qui est la valeur de paramètre `-u`, contient un caractère spécial, vous devez encoder le caractère spécial ou indiquer l'URL d'enregistrement entre guillemets simples. Par exemple, `!` devient `%21`, ou la commande ressemble à ceci :

  ```
  ./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname ca
  ```
  {:codeblock}

  La commande `enroll` génère un ensemble complet de certificats, appelé dossier MSP, qui se trouve dans le répertoire où vous avez défini votre client d'autorité de certification Fabric sur le chemin`$HOME`. Par exemple, `$HOME/fabric-ca-client/ca-admin`. Pour plus d'informations sur les dossiers MSP et leur contenu, voir [Fournisseur de services aux membres (MSP)](/docs/services/blockchain/howto/CA_operate.html#ca-operate-msp).

  Si la commande `enroll` échoue, voir la section relative au [Traitement des incidents](/docs/services/blockchain/howto/CA_operate.html#ca-operate-troubleshooting) pour connaître les causes possibles.

  Vous pouvez exécuter une commande tree pour vérifier que vous avez effectué toutes les étapes prérequises. Accédez au répertoire dans lequel vous avez stocké vos certificats. Une commande tree doit générer un résultat similaire à la structure suivante :

  ```
  cd $HOME/fabric-ca-client
tree
.
  ├── ca-admin
  │   ├── fabric-ca-client-config.yaml
  │   └── msp
  │       ├── cacerts
  │       │   └── 9-30-250-70-30395-SampleOrgCA.pem
  │       ├── IssuerPublicKey
  │       ├── IssuerRevocationPublicKey
  │       ├── keystore
  │       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
  │       ├── signcerts
  │       │   └── cert.pem
  │       └── user
  └── catls
      └── tls.pem    
  ```

### Enregistrement de l'identité du composant auprès de l'autorité de certification
{: #ibp-v2-apis-config-register-component}

Tout d'abord, vous devez enregistrer une identité de composant auprès de votre autorité de certification. Votre composant utilisera cette identité pour générer les certificats nécessaires à son déploiement.

1. [Générez des certificats avec votre admin d'autorité de certification](#ibp-v2-apis-enroll-ca-admin) à l'aide du client d'autorité de certification Fabric. Utilisez ces certificats admin pour exécuter les commandes suivantes. Assurez-vous que `$FABRIC_CA_CLIENT_HOME` est défini sur `$HOME/fabric-ca-client/ca-admin`.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```

2. Exécutez la commande suivante pour rechercher votre affiliation et le nom de votre organisation.
  ```
  fabric-ca-client affiliation list --caname <ca_name> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  La commande peut ressembler à l'exemple suivant :
  ```
  fabric-ca-client affiliation list --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  Vous devez voir des informations similaires à l'exemple suivant :
  ```
  affiliation: org1
      affiliation: org1.department1
  ```

  Notez la seconde valeur **affiliation**, par exemple, `org1.department1`. Vous devez utiliser cette valeur dans la commande ci-dessous.

3. Exécutez la commande suivante pour enregistrer le service de tri ou l'homologue.

  ```
  fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type <component_type> --id.secret <secret> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  Créez un nom et un mot de passe pour le composant, puis utilisez-les pour remplacer `name` et `secret`.  Notez ces informations. Définissez `--id.type` sur `orderer` si vous déployez un service de tri, ou définissez-le sur `peer` si vous déployez un homologue. La commande peut ressembler à l'exemple suivant :

  ```
  fabric-ca-client register --caname ca --id.affiliation org1.department1 --id.name peer1 --id.secret peer1pw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  Vous devez noter les éléments `"enrollid"` et `"enrollsecret"` de la commande ci-dessus lors de la création de votre fichier de configuration.
  {: important}

  Vous ne pouvez enregistrer une identité qu'une seule fois. Si vous rencontrez un problème, essayez d'exécuter une commande avec un nouveau nom d'utilisateur et un nouveau mot de passe. Par mesure de sécurité, utilisez chaque identité, ainsi que l'ID d'inscription et la valeur confidentielle qui l'accompagnent, pour déployer un seul homologue. Même si vous pouvez utiliser une identité **admin** pour plusieurs composants (il s'agit de la stratégie de déploiement recommandée), ne réutilisez pas les ID et les mots de passe d'homologue.

  Lorsque la commande aboutit, vous pouvez voir des informations semblables à l'exemple suivant :

  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: peerpw
  ```

### Enregistrement de l'administrateur de votre organisation
{: #ibp-v2-apis-config-register-admin}

Vous devez aussi créer une identité admin que vous utilisez pour exploiter votre réseau. Vous utiliserez cette identité pour exploiter des composants spécifiques, comme l'installation d'un contrat intelligent sur votre homologue. Vous pouvez également utiliser cette identité en tant qu'administrateur de votre organisation et l'utiliser pour créer et modifier des canaux.  

Vous devez enregistrer cette nouvelle identité auprès de votre autorité de certification, puis l'utiliser pour générer un dossier MSP. Vous pouvez définir cette identité en tant qu'administrateur de l'organisation en ajoutant son signCert au MSP de votre organisation. Vous devrez également ajouter le certificat signCert dans votre fichier de configuration afin qu'il puisse être défini comme certificat admin du service de tri ou de l'homologue lors du déploiement. Il vous suffit de créer une seule identité admin pour votre organisation. Ainsi, vous ne devez effectuer ces étapes qu'une seule fois. Vous pouvez utiliser le certificat signCert que vous avez généré pour déployer de nombreux homologues ou services de tri.

Vérifiez que `$FABRIC_CA_CLIENT_HOME` est défini sur le chemin du dossier MSP de l'admin de votre autorité de certification.

```
echo $FABRIC_CA_CLIENT_HOME
$HOME/fabric-ca-client/ca-admin
```
{:codeblock}

Enregistrez l'identité admin du composant auprès de l'autorité de certification en exécutant la commande suivante :

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type client --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Créez un `nom` et `secret` pour la nouvelle identité utilisateur de l'admin. Assurez-vous d'utiliser des valeurs différentes de celles de l'homologue ou du service de tri que vous venez d'enregistrer. La commande ressemble à l'exemple suivant :

```
fabric-ca-client register --caname ca --id.name peeradmin --id.affiliation org1.department1 --id.type client --id.secret peeradminpw --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Notez ces informations. Vous utiliserez ce `nom` et ce `secret` pour générer le dossier MSP à l'aide de la commande `enroll`.
{: important}

### Génération du dossier MSP admin
{: #ibp-v2-apis-config-enroll-admin}

Après enregistrement de l'admin du composant, vous pouvez générer les certificats à l'aide de la commande suivante :

```
fabric-ca-client enroll -u https://<peer_admin_enroll_id>:<peer_admin_enroll_password>@<ca_url_with_port> --caname <ca_name> -M $HOME/path/to/peer-admin/msp --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Par exemple :

```
fabric-ca-client enroll -u https://peeradmin:peeradminpw@9.30.94.174:30167 --caname ca -M $HOME/fabric-ca-client/peer-admin/msp --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{:codeblock}

Une fois cette commande exécutée, elle génère un nouveau dossier MSP dans le répertoire que vous avez spécifié à l'aide de l'indicateur `-M`. Ce répertoire contient les certificats que vous devez utiliser pour exploiter vos composants. Vous pouvez vérifier que la commande enroll a abouti à l'aide d'une commande tree. Accédez au répertoire dans lequel vous avez stocké vos certificats. Une commande tree doit générer un résultat similaire à la structure suivante :


```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── keystore
│       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── catls
│   └── tls.pem
├── orderer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── dfe06060490bf62e2bd709433fc747ff28cdbb1e040682c5d47a4e8598db4f2e_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── peer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 24f76217c5c7e641ee29b068712181294e427cbee4dbfce230a1a98b53489cbe_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── tlsca-admin
    ├── fabric-ca-client-config.yaml
    └── msp
        ├── cacerts
        │   └── 9-30-250-70-30395-tlsca.pem
        ├── IssuerPublicKey
        ├── IssuerRevocationPublicKey
        ├── keystore
        │   └── 45a7838b1a91ddfe3d4d22a5a7f2639b868493bcce594af3e3ceb9c07899d117_sk
        ├── signcerts
        │   └── cert.pem
        └── user
```

Vous devrez revenir dans ce dossier lorsque vous créez la définition MSP de votre organisation et les fichiers de configuration.
{: important}

### Enregistrement du composant auprès de l'autorité de certification TLS
{: #ibp-v2-apis-config-register-component-tls}

Lors de la création de l'autorité de certification, une autorité de certification TLS a été déployée avec votre autorité de certification par défaut. Vous devez également enregistrer le service de tri ou l'homologue auprès de votre autorité de certification TLS. Pour ce faire, vous devez d'abord vous inscrire à l'aide de l'admin de l'autorité de certification TLS. Remplacez `$FABRIC_CA_CLIENT_HOME` par un répertoire où vous voulez stocker vos certificats admin de l'autorité de certification TLS.

```
cd $HOME/fabric-ca-client
mkdir tlsca-admin
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/tlsca-admin
```
{:codeblock}

Exécutez la commande ci-dessous pour vous inscrire en tant qu'admin auprès de l'autorité de certification TLS. L'ID d'inscription et le mot de passe de l'admin de votre autorité de certification TLS sont identiques à ceux dans votre autorité de certification par défaut. Par conséquent, La commande ci-dessous est la même que celle utilisée pour vous inscrire en tant que [admin d'autorité de certification](/docs/services/blockchain/howto/CA_operate.html#ca-operate-enroll-ca-admin) uniquement avec le nom de votre autorité de certification TLS. Ce nom est la valeur **Nom de l'autorité de certification TLS** du panneau des **paramètres** de l'autorité de certification sur votre console, ou la valeur de l'élément `"tlsca_name"` par l'API `Create a CA`.

```
fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <tls_ca_name> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Un appel réel doit ressembler à l'exemple suivant :

```
fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname tlsca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Une fois l'inscription effectuée, vous disposez des certificats nécessaires pour enregistrer votre composant auprès de l'autorité de certification TLS. Exécutez la commande suivante pour enregistrer le service de tri ou l'homologue :

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type peer --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Cette commande est similaire à celle que vous avez utilisée pour enregistrer l'identité du composant auprès de l'autorité de certification, exception faite de l'utilisation du nom de l'autorité de certification TLS. Si vous déployez un service de tri au lieu d'un homologue, définissez `--id.type` sur `orderer` au lieu de `peer`. Vous devez attribuer à cette identité un nom d'utilisateur et un mot de passe différents de ceux que vous avez utilisés pour votre autorité de certification par défaut. Un enregistrement réel peut ressembler à la commande suivante :

```
fabric-ca-client register --caname tlsca --id.affiliation org1.department1 --id.name peertls --id.secret peertlspw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Vous devez noter les éléments `"enrollid"` et `"enrollsecret"` de la commande ci-dessus lors de la création de votre fichier de configuration.
{: important}

## Création de la définition MSP d'une organisation
{: #ibp-v2-apis-msp}

Vous pouvez utiliser les API pour créer la définition MSP d'une organisation en appelant [`poster/ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp). Cette définition MSP contient les certificats qui définissent votre organisation dans un consortium de blockchain, ainsi que les certificats admin que vous pouvez utiliser pour exploiter votre réseau. Si vous avez suivi l'étape ci-dessus, vous avez déjà généré les certificats nécessaires à la création d'une définition MSP d'organisation. Utilisez la procédure suivante pour terminer le corps de la demande de l'appel d'API.

1. Entrez `msp` comme `type` de demande.

2. Sélectionnez un ID MSP pour votre organisation. L'ID MSP est le nom formel de votre organisation au sein du consortium. L'ID MSP utilisé pour créer la définition MSP d'une organisation doit être identique à celui que vous utilisez pour déployer vos homologues.

3. Sélectionnez un nom d'affichage pour votre définition MSP.

4. Vous devez fournir le certificat signCert, au format base64, de l'administrateur de votre organisation que vous avez enregistré et inscrit à l'aide du client d'autorité de certification Fabric.  

  Accédez au répertoire MSP qui a été créé lors de la [génération des certificats à l'aide de l'administrateur de votre organisation](#ibp-v2-apis-config-enroll-admin).
  ```
  cd $HOME/fabric-ca-client/peer-admin/msp
  ```
  Dans ce répertoire MSP, ouvrez le fichier SignCert du nouvel utilisateur et convertissez-le au format base64 à l'aide des commandes suivantes :

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  **Remarque :** Il est important que la chaîne générée par l'utilisation de la commande ci-dessus soit mise en forme sur une seule ligne. Elle doit ressembler à ceci :

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
  ```
  et non à ceci :

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
  ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
  Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
  VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
  WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
  ```

  Par exemple :

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  Cette commande produit une chaîne qui est semblable à l'exemple suivant :

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
  ```

  Indiquez cette chaîne dans la zone `admins` de la demande API.

5. Vous devez également fournir le certificat racine de votre autorité de certification. Ce certificat a été créé lors de la [génération des certificats à l'aide de l'administrateur de votre autorité de certification](#ibp-v2-apis-enroll-ca-admin).

  Accédez au répertoire MSP de l'admin de votre autorité de certification.
  ```
  cd $HOME/fabric-ca-client/ca-admin/msp
  ```
  Dans ce répertoire, ouvrez le dossier cacerts et convertissez le certificat qui s'y trouve au format base64 à l'aide des commandes suivantes :
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-ca-admin>/msp/cacerts/<ca_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Cette opération convertit le certificat racine en tant que chaîne codée en base64.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWIyZ0F3SUJBZ0lVQmZnZzcvVnIrL25OVEFNQlQ4UUtHL00wQU8wd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU1TURVd016RXpNamt3TUZvWERUTTBNRFF5T1RFek1qa3dNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUVXMUtvN2lWeVE2VWkwdDVqbU5KaWVuSUwKR3pNM1BDWHlhL2VSQ0NWMmFQb0dTZ1lrVUg2UWN5RjAzbFlMZFU4Y0drNTQ0alViVC9KT1lYeVgzTWc4bHFORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZDK2lJR0NSb2Zvb3FsVkZoU3dOMmk2MXNJaVBNQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUURTYW9RL1E0QzkKbFl1VGNhVXVHb3d6YmhUZHBuN2F3S2lHN1Nvd2lSQXVld0lnUWlyM3RNR3IvYWo2aU5lRXJFN2NyOVowQ0gvTwp3QnNQcWd4RVR3MjVqZUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Indiquez cette chaîne dans la zone `root_certs` de la demande API.

6. Vous devez également fournir le certificat racine de votre autorité de certification TLS. Le certificat racine TLS permet à vos homologues de participer à la communication gossip sur un canal.

  Accédez au répertoire MSP qui a été généré lors de l'[inscription de l'admin de votre autorité de certification TLS](#ibp-v2-apis-config-register-component-tls).
  ```
  cd $HOME/fabric-ca-client/tlsca-admin/msp
  ```
  Dans ce répertoire, ouvrez le dossier cacerts et convertissez le certificat qui s'y trouve au format base64 à l'aide des commandes suivantes :
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-tlsca-admin>/msp/cacerts/<tls_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Cette opération convertit le certificat racine de l'autorité de certification TLS en tant que chaîne codée en base64.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVWUVQWnprNXV2b3dobEtacG5JMXplODdIQUlnd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEExTURNeE16STVNREJhRncwek5EQTBNamt4TXpJNU1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFRdSs2UnZWd2w5T2dDVlAraEVxbjVxdExRVG9LWkw4a1lic0pOeU1JbERoc3hlNWx6cW1zQkoKbTk2eUR2TVV6OSsxL2pzb1M4M1JqMVAwc3M2TnJNb3FvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVVnUEc4anJEK1BxVjdoelc3WDlsbTFrMS91WjR3CkZRWURWUjBSQkE0d0RJY0VxVGJESW9jRUNwbnBkVEFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUVBenk3cHJZaVMKQmlDVWdYeWRkY09WMm9mZmtqaEI0N091QXFjQWNqZS9SWkVDSUdKZFgzZ1ErTDRIN3duY1RoZkwrenU1ejV1UApGUWhXTmlNS3hQWEYrZnYwCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Indiquez cette chaîne dans la zone `tls_root_certs` de la demande API.

## Création d'un fichier de configuration
{: #ibp-v2-apis-config}

Vous devez exécuter un fichier de configuration afin de créer un homologue ou un service de tri à l'aide d'API. Ce fichier est fourni à l'API en tant qu'objet `config` dans le corps de demande de l'appel d'API. Vous devez déployer une autorité de certification dans votre instance de service {{site.data.keyword.cloud_notm}} Platform et suivre les étapes d'enregistrement et d'inscription des identités requises avant d'exécuter le fichier.

Voici un modèle de fichier de configuration :
```
{
	"enrollment": {
		"component": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"admincerts": [""]
		},
		"tls": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"csr": {
				"hosts": [""]
			}
		}
	}
}
```
{:codeblock}

Copiez l'intégralité de ce fichier dans un éditeur de texte dans lequel vous pouvez l'éditer et le sauvegarder sur votre système de fichiers local en tant que fichier JSON. Procédez comme suit pour créer ce fichier de configuration et l'utiliser pour déployer un service de tri ou un homologue.

### Extraction des informations de connexion de l'autorité de certification
{: #ibp-v2-apis-config-connx-info}

Tout d'abord, nous devons fournir les informations de connexion de votre autorité de certification sur {{site.data.keyword.cloud_notm}} Platform. Vous pouvez utiliser l'interface utilisateur de la console ou les API pour obtenir les informations nécessaires concernant votre autorité de certification.

**Si vous utilisez la console {{site.data.keyword.blockchainfull_notm}} Platform :**
Ouvrez l'autorité de certification sur votre console et cliquez sur **Settings**, puis sur le bouton **Exporter** pour exporter les informations d'autorité de certification dans un fichier JSON. Vous pouvez utiliser les valeurs de ce fichier pour finaliser votre fichier de configuration.

**Si vous utilisez les API :**
Vous pouvez appeler [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) pour obtenir les informations de connexion de votre autorité de certification. Si vous avez créé l'autorité de certification à l'aide de l'API `Create a Fabric CA`, vous pouvez également trouver les informations nécessaires dans le corps de réponse.

- Les valeurs `"cahost"` et `"caport"` sont visibles dans la zone `ca_url` dans le corps de réponse ou dans le fichier JSON de l'autorité de certification que vous avez exporté.  Par exemple, si votre valeur `ca_url` est https://9.30.94.174:30167, la valeur de `"cahost"` serait `9.30.94.174` et la valeur de `"caport"` serait `30167`.
- Le `"caname"` est le nom de l'autorité de certification qui a été indiqué lors du déploiement de l'autorité de certification. Il s'agit de la valeur de la zone `ca_name` dans le corps de réponse ou le fichier JSON exporté.
- La valeur `"cacert"` est le certificat TLS encodé en base64 de votre autorité de certification. Il s'agit de la valeur de la zone `pem` dans le corps de réponse ou le fichier JSON exporté.

- Les zones de la section `"tls"` ci-dessous requièrent les mêmes informations que celles transmises aux sections component ci-dessus, excepté que vous devez utiliser la valeur du nom d'instance TLS de l'autorité de certification qui est indiqué lors du déploiement de l'autorité de certification. Remplacez `caname` par la valeur de `tlsca_name` dans le corps de réponse ou le fichier JSON exporté. Utilisez le même certificat TLS pour la valeur `"cacert"`.
  ```
  "tls": {
    "cahost": "",
    "caport": "",
    "caname": "",
    "catls": {
      "cacert": ""
  ```
  {:codeblock}

### Indication de l'ID d'inscription et du secret de votre composant

1. Collez le `nom` et le `secret` que vous avez utilisés pour [enregistrer votre composant auprès de votre autorité de certification valeur par défaut](#ibp-v2-apis-config-register-component) comme `"enrollid"` et `"enrollsecret"` dans le fichier de configuration, sous la section `"component"` :

  ```
  "component": {...
    },
    "enrollid": "peer1",
    "enrollsecret": "peer1pw",
  ```
2. Collez le `nom` et le `secret` que vous avez utilisés pour [enregistrer votre composant auprès de votre autorité de certification TLS](#ibp-v2-apis-config-register-component-tls) comme `"enrollid"` et `"enrollsecret"` dans le fichier de configuration, sous la section `"tls"` :

  ```
  "tls": {...
    },
    "enrollid": "peertls",
    "enrollsecret": "peertlspw",
  ```

### Indication du certificat signCert de l'administrateur de votre organisation

Accédez au répertoire MSP qui a été créé lors de la [génération des certificats à l'aide de l'administrateur de votre organisation](#ibp-v2-apis-config-enroll-admin).
```
cd $HOME/fabric-ca-client/peer-admin/msp
```
Dans ce répertoire MSP, ouvrez le fichier SignCert du nouvel utilisateur et convertissez-le au format base64 à l'aide des commandes suivantes :

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

**Remarque :** Il est important que la chaîne générée par l'utilisation de la commande ci-dessus soit mise en forme sur une seule ligne. Elle doit ressembler à ceci :

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
```
et non à ceci :

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
```

Par exemple :

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

Cette commande produit une chaîne qui est semblable à l'exemple suivant :

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
```

Copiez cette chaîne dans la zone `"admincerts"` sous la section component dans le fichier de configuration.

### Hôtes CSR (Demande de signature de certificat)

Vous avez la possibilité de fournir un domaine personnalisé pour votre composant en utilisant la section `"csr"` du fichier de composants.

```
"csr": {
  "hosts": [""]
  }
```
{:codeblock}

Cette section est destinée aux utilisateurs expérimentés pour indiquer un nom d'hôte qui peut être utilisé pour adresser le noeud final d'homologue. La plupart des utilisateurs peuvent laisser cette section à blanc.

### Finalisation du fichier de configuration
{: #ibp-v2-apis-config-file}

Une fois toutes les étapes ci-dessus effectuées, votre fichier de configuration mis à jour peut être similaire à l'exemple suivant :


```
{
    "enrollment": {
        "component": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "ca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJ0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peer1",
            "enrollsecret": "peer1pw",
            "admincerts": [
                "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzRENDQW9PZ0F3SUJBZ0lVS2Vib0drZEJqenM5bllRbkVQTWp0VG56YTVrd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE9URTNNRE13TUZvWERURTVNVEV4T1RFM01EZ3dNRm93ZmpFTE1BaKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRXVNQXNHQTFVRUN4TUVkWE5sY2pBTEJnTlZCQXNUQkc5eVp6RXdFZ1lEVlFRTEV3dGtaWEJoCmNuUnRaVzUwTVRFUU1BNEdBMVVFQXhNSFlXUnRhVzR4Y0RCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUgKQTBJQUJLbjUwdEU5TmpZb0RFNDBqalh6RUJ2T2c3Y3RtOElRd0laMnRkS29iNEwwVVhKdSs1Tkt5S2dyUk9vbApWcjBmQmg5cWZWMjl4Nms0T2dmMFNiVklBZWlqZ2ZRd2dmRXdEZ1lEVlIwUEFRSC9CQVFEQWdlQU1Bd0dBMVVkCkV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZOYWFkV0VzcGp2Smk1akpiVktiS2M3ZU1wUmhNQjhHQTFVZEl3UVkKTUJhQUZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQ2NHQTFVZEVRUWdNQjZDSEdOb1lXNWtjbUZ6TFcxaQpjQzV5WVd4bGFXZG9MbWxpYlM1amIyMHdhQVlJS2dNRUJRWUhDQUVFWEhzaVlYUjBjbk1pT25zaWFHWXVRV1ptCmFXeHBZWFJwYjI0aU9pSnZjbWN4TG1SbGNHRnlkRzFsYm5ReElpd2lhR1l1Ulc1eWIyeHNiV1Z1ZEVsRUlqb2kKWVdSdGFXNHhjQ0lzSW1obUxsUjVjR1VpT2lKMWMyVnlJbjE5TUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSURGeApDYzE1bDZUZ1dqYnhSZzlmNjczOGV0K0NZZ1I3VVpGR200VHdoQk5jQWlBNmtUMFFwbDV6WnBUN3BzM0dySXlVCmEydDRHSTQ5ZTdjUm5PMmdrSzl6Z3c9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg=="
            ]
        },
        "tls": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "tlsca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peertls",
            "enrollsecret": "peertlspw",
            "csr": {
                "hosts": [
                ]
            }
        }
    }
}
```
{:codeblock}

Vous pouvez laisser les autres zones vides. Une fois ce fichier finalisé, vous pouvez le transmettre en tant que zone `config` dans le corps de demande de l'API `Create an orderer` ou `Create a peer`.

### Importation d'une identité admin sur la console {{site.data.keyword.blockchainfull_notm}} Platform
{: #ibp-v2-apis-admin-console}

Si vous souhaitez utiliser la console {{site.data.keyword.blockchainfull_notm}} Platform pour exploiter vos composants de blockchain, vous devez importer votre identité d'administrateur dans le portefeuille de votre console. Ouvrez le panneau de portefeuille sur votre console. Cliquez sur le bouton **Ajouter une identité** dans l'écran de présentation. Si vous cliquez sur ce bouton, un panneau latéral s'affiche dans lequel vous pouvez ajouter le certificat signataire et la clé privée d'une identité directement sur la console.
- **Nom :** Entrez un nom d'affichage pour l'identité qui est utilisé à des fins de référence uniquement.
- **Certificat :** Téléchargez le certificat signataire de votre admin. Si vous avez suivi les instructions ci-dessus, vous pouvez trouver cette clé dans le dossier `$HOME/fabric-ca-client/peer-admin/msp/signcerts/`.
- **Clé privée :** Téléchargez la clé privée de vos admin. Si vous avez suivi les instructions ci-dessus, vous pouvez trouver cette clé dans le dossier `$HOME/fabric-ca-client/peer-admin/msp/keystore/`.

Après avoir importé votre identité admin, vous pouvez associer cette identité aux composants que vous avez créés. Vous pouvez ensuite utiliser la console pour exploiter votre réseau.
