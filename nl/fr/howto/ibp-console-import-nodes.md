---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Importation de noeuds
{: #ibp-console-import-nodes}

La console inclut une option d'importation des noeuds qui sont créés à l'aide d'une autre console {{site.data.keyword.blockchainfull}} Platform.
{:shortdesc}  

**Public cible :** Cette rubrique s'adresse aux opérateurs réseau qui sont responsables de la création, de la surveillance et de la gestion du réseau de blockchain.

## Pourquoi importer un noeud ?
{: #ibp-console-import-nodes-why}

Vous pouvez importer les noeuds d'une autorité de certification, d'un service de tri ou d'un homologue depuis d'autres consoles {{site.data.keyword.blockchainfull_notm}} Platform lorsque vous devez les exploiter avec des noeuds déjà existants sur votre console. Par exemple, vous pouvez utiliser l'option d'importation d'homologue lorsque vous voulez joindre des homologues d'organisations à l'extérieur de votre console à des canaux sur votre console. Comme les services {{site.data.keyword.blockchainfull_notm}} Platform sont déployés dans plusieurs environnements, il est probable que certaines instances de service incluent uniquement les autorités de certification et les homologues, alors que d'autres hébergent des services de tri. L'importation de noeuds différents dans la console constitue un moyen d'afficher et d'exploiter ces composants distribués depuis une interface utilisateur unique afin qu'ils puissent effectuer des transactions ensembles sur la blockchain.

Une fois que vous avez importé des noeuds sur la console, vous disposez d'un puissant ensemble de fonctions pour :
- Ajouter de nouvelles organisations au consortium
- Créer de nouveaux canaux
- Joindre des homologues aux canaux
- Installer des contrats intelligents sur les homologues quel que soit l'emplacement où ils ont été déployés
- Instancier des contrats intelligents sur des canaux
- Mettre à niveau des contrats intelligents sur des canaux

Lors de l'importation d'un noeud, vous devez fournir ses informations de connexion. Lorsque vous importez un composant, vous n'importez pas vraiment le composant physique lui-même ; il utilise au contraire les informations de connexion pour générer une représentation du composant sur la console de manière à pouvoir être exploité depuis la console. De même, lorsque vous supprimez un noeud importé depuis la console, le noeud lui-même, qui s'exécute toujours à l'emplacement où il a été déployé, n'est pas supprimé. Il est simplement retiré de la console où il a été importé.  

Une fois que vous avez importé un noeud sur la console, vous pouvez également modifier ses informations de connexion à l'aide de l'onglet **Paramètres** du noeud.

Pour que des noeuds puissent être importés sur la console, ils doivent être exportés depuis la console {{site.data.keyword.blockchainfull_notm}} Platform où ils ont été créés. L'opérateur réseau peut simplement importer les informations de noeud depuis la console dans un fichier `JSON`.
{: note}

## Limitations
{: #ibp-console-import-limitations}

- Vous ne pouvez pas importer des noeuds depuis des réseaux de plan Starter ou Enterprise.
- Tous les noeuds à importer doivent avoir été déployés à l'aide de la console {{site.data.keyword.blockchainfull_notm}} Platform.
- Vous ne pouvez pas appliquer des correctifs à des noeuds que vous avez importés sur la console.
- Vous ne pouvez pas supprimer des noeuds que vous avez importés sur la console depuis le cluster où ils ont été déployés. Vous pouvez uniquement supprimer le noeud à partir de la console.
- Si vous importez un noeud déployé dans {{site.data.keyword.cloud_notm}} Private, vous devez vous assurer que le proxy Web gRPC utilisé par le composant est exposé en externe à la console. Pour plus d'informations, voir [Importation de noeuds depuis {{site.data.keyword.cloud_notm}} Private](#ibp-console-import-icp)

## Premiers pas : Collecte des certificats ou des données d'identification
{: #ibp-console-import-start-here}

Avant d'importer un composant de blockchain existant à partir d'une autre instance de service {{site.data.keyword.blockchainfull_notm}} Platform, vous devez importer l'identité admin du composant dans votre portefeuille de console. L'importation de l'identité dans votre portefeuille de console simplifie le fonctionnement de noeud.  L'opérateur réseau qui a déployé le noeud doit exporter l'identité admin du noeud depuis le portefeuille dans un fichier JSON que vous pouvez [importer](#ibp-console-import-nodes-admin-identities).  Généralement, l'identité admin du noeud est l'admin de l'organisation de l'homologue ou du service de tri qui a été indiqué lors de la création de la définition MSP de l'organisation. Il doit déjà être dans le portefeuille de la console où réside le noeud.

### Importation d'identités admin dans le portefeuille de console
{: #ibp-console-import-nodes-admin-identities}

Chaque composant {{site.data.keyword.blockchainfull_notm}} Platform est déployé avec son propre certificat signataire, certificat d'un administrateur de composant interne. Lorsque des actions sont effectuées sur le composent par l'administrateur, ce certificat signataire est associé à la transaction et validée par rapport à la configuration de composant. Les clés permettent à l'administrateur d'exploiter ses composants, notamment pour enregistrer de nouveaux utilisateurs, créer un nouveau canal ou installer des contrats intelligents sur des homologues. Si vous souhaitez utiliser la console pour exploiter un service de tri ou un homologue qui a été créé à partir d'une autre console, vous devez télécharger l'identité admin associée au portefeuille de console. Ensuite, vous pouvez associer le noeud importé avec l'identité du portefeuille de console. Ces identités doivent être exportées depuis la console où elles ont été créées et elles sont obligatoires lors de l'importation du noeud.

Pour importer une nouvelle identité, ouvrez l'onglet **Portefeuille** et cliquez sur **Ajouter une identité**. Cliquez sur **Télécharger JSON** pour rechercher le fichier d'identité JSON qui a été exporté par l'opérateur réseau depuis l'emplacement où a été créé le noeud.

Une fois que vous avez terminé de compléter le panneau **Ajouter une identité** et cliqué sur Soumettre, vous pouvez voir la nouvelle identité admin dans l'écran de présentation du portefeuille. Vous pouvez maintenant faire référence à ces identités lorsque vous importez une autorité de certification, un homologue, ou des composants de service de tri.

## Importation d'une autorité de certification
{: #ibp-console-import-ca}

Un noeud d'autorité de certification est une zone de blockchain qui émet des certificats pour toutes les entités du réseau (homologues, services de tri, clients, et ainsi de suite) afin que ces entités puissent communiquer, s'authentifier et enfin effectuer des transactions. Chaque organisation a sa propre autorité de certification qui fait office de racine de confiance. Vous devriez ajouter vos organisations si vous rejoignez ou générez un consortium de blockchain. Pour plus de détails sur les autorités de certification, voir la [présentation des composants de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca).  

Vous pouvez importer une autorité de certification sur votre console pour différentes raisons. Une fois l'autorité de certification importée, vous pouvez l'utiliser pour enregistrer d'autres homologues à l'organisation homologue en cliquant sur **Enregistrer un utilisateur**. Vous pouvez également utiliser l'autorité de certification pour créer d'autres identités admin pour un homologue ou un service de tri.

Pour importer une autorité de certification sur la console {{site.data.keyword.blockchainfull_notm}} Platform et l'utiliser, l'opérateur réseau doit avoir préalablement exporté cette autorité de certification depuis la plateforme {{site.data.keyword.blockchainfull_notm}} Platform où elle a été déployée. L'importation d'une autorité de certification vous permet d'enregistrer de nouveaux utilisateurs et d'[inscrire des identités](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll).

### Avant de commencer
{: #ibp-console-import-ca-before-you-begin}

- Vérifiez que vous avez déjà importé le fichier JSON d'identité admin de l'autorité de certification dans votre portefeuille de console, ou que vous possédez l'ID d'inscription ou le secret administrateur de l'autorité de certification et de l'autorité de certification TLS qui ont été spécifiées lors du déploiement initial de cette autorité de certification.
- Vérifiez que le fichier JSON d'autorité de certification qui a été exporté depuis la console où il a été créé est disponible.

### Procédure d'importation d'une autorité de certification  
{: #ibp-console-import-nodes-howto-ca}

L'importation d'une autorité de certification s'effectue à partir de l'onglet **Noeuds** .
1. Cliquez sur **Ajouter une autorité de certification**, puis sur **Importer une autorité de certification existante**, et enfin sur **Suivant**.
2. Sélectionnez l'emplacement du déploiement initial de l'autorité de certification dans la liste déroulante **Emplacement de l'autorité de certification**.
3. Cliquez sur **Ajouter un fichier** pour télécharger le fichier JSON AC qui a été exporté depuis la console où il a été initialement déployé.
4. Dans le panneau suivant, vous pouvez entrer l'ID et le secret d'inscription qui ont été utilisés lors du déploiement de l'autorité de certification.
5. Enfin, entrez l'ID et le secret d'inscription de l'autorité de certification TLS qui ont été utilisés lors du déploiement de l'autorité de certification. Lorsque vous déployez une autorité de certification, une autorité de certification et une autorité de certification TLS sont déployées ensemble. L'opérateur réseau peut utiliser les mêmes ID et secret d'inscription pour les deux, ou bien il peut indiquer un ID et un secret d'inscription uniques pour l'autorité de certification et l'autorité de certification TLS lors du déploiement d'une autorité de certification.

Une fois que vous avez importé l'autorité de certification sur la console, vous pouvez utiliser votre autorité de certification pour créer de nouvelles identités et générer les certificats nécessaires pour exploiter vos composants et soumettre des transactions au réseau. Pour en savoir plus, voir la section [Gestion des autorités de certification](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca).

## Importation d'un service de tri
{: #ibp-console-import-orderer}

Un service de tri est le composant de blockchain qui collecte les transactions des membres réseau, trie les transactions et les regroupe dans des blocs. Il s'agit de la liaison commune des consortiums de blockchain et il doit être déployé si vous fondez un consortium que rejoindront d'autres organisations. Pour plus de détails sur les autorités de certification, voir la [présentation des composants de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer).

L'importation d'un service de tri sur la console vous permet de créer de nouveaux canaux pour les homologues afin qu'ils puissent effectuer des transactions en privé.

### Avant de commencer
{: #ibp-console-import-orderer-before-you-begin}

Avant d'importer un service de tri, vous devez collecter les informations et certificats suivants :

- Vérifiez que vous avez déjà [importé le fichier JSON de l'identité admin du service de tri](#ibp-console-import-nodes-admin-identities) dans votre portefeuille de console.
- Vérifiez que le fichier JSON de service de tri qui a été exporté depuis la console où il a été créé est disponible.

### Procédure d'importation d'un service de tri
L'importation d'un service de tri s'effectue à partir de l'onglet **Noeuds** .
1. Cliquez sur **Ajouter un service de tri**, sur **Importer un service de tri existant** puis sur **Suivant**.
2. Sélectionnez l'emplacement du déploiement initial du service de tri dans la liste déroulante **Emplacement de l'autorité de certification**.
3. Cliquez sur **Ajouter un fichier** pour télécharger le fichier JSON de service de tri qui a été exporté depuis la console où il a été initialement déployé. Notez que s'il s'agit d'un service de tri Raft à cinq noeuds, vous devez avoir un seul fichier avec les informations de connexion des cinq noeuds de tri.
4. Définissez l'identité admin du service de tri en cliquant sur **Identité existante** et en sélectionnant l'identité admin du service de tri que vous avez importé dans votre portefeuille de console.

Une fois que vous avez importé le service de tri sur la console, vous pouvez ajouter de nouveaux membres d'organisation et sélectionner le service de tri lors de la création de nouveaux canaux.

## Importation d'un homologue
{: #ibp-console-import-peer}

Un noeud homologue est un composant de blockchain qui gère un registre et exécute un contrat intelligent pour effectuer des opérations de requête et de mise à jour sur ce registre. Les membres d'une organisation détiennent et gèrent des homologues.  Chaque organisation qui rejoint un consortium doit déployer au moins un homologue et au minimum deux pour la haute disponibilité. Pour plus de détails sur les homologues, voir la [présentation des composants de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer).

Une fois que vous avez importé un homologue sur la console, vous pouvez installer des contrats intelligents sur cet homologue et joindre ce dernier à d'autres canaux dans votre blockchain.

**Remarque :** Si vous devez ajouter d'autres homologues à l'organisation homologue ou créer des identités admin supplémentaires pour un homologue, vous devez importer l'autorité de certification de l'homologue puis utiliser cette autorité de certification pour effectuer ces opérations.

### Avant de commencer
{: #ibp-console-import-peer-before-you-begin}

Avant d'importer un homologue, vous devez collecter les informations suivantes :

- Vérifiez que vous avez déjà [importé le fichier JSON de l'identité admin de l'homologue](#ibp-console-import-nodes-admin-identities) dans votre portefeuille de console.
- Vérifiez que le fichier JSON d'homologue qui a été exporté depuis la console où il a été créé est disponible.

### Procédure d'importation d'un homologue
{: #ibp-console-import-peer-howto}

L'importation d'un homologue s'effectue à partir de l'onglet **Noeuds** .
1. Cliquez sur **Ajouter un homologue**, sur **Importer un homologue existant**, puis sur **Suivant**.
2. Sélectionnez l'emplacement du déploiement initial de l'homologue dans la liste déroulante **Emplacement de l'autorité de certification**.
3. Cliquez sur **Ajouter un fichier** pour télécharger le fichier JSON homologue qui a été exporté depuis la console où il a été initialement déployé.
4. Définissez l'identité admin de l'homologue en cliquant sur **Identité existante** et en sélectionnant l'identité admin de l'homologue que vous avez importé dans votre portefeuille de console.  

Une fois que vous avez importé l'homologue sur la console, vous pouvez installer des contrats intelligents sur cet homologue et joindre ce dernier à des canaux dans votre blockchain.

## Importation de la définition MSP d'une organisation
{: #ibp-console-import-msp}

Vous devez importer la définition MSP d'une organisation si le MSP a été déployé à partir d'une autre console d'instance de service {{site.data.keyword.blockchainfull_notm}} Platform et que vous prévoyez de créer ou de mettre à jour un canal qui utilise cette définition MSP. De même, si vous prévoyez de créer un homologue ou un service de tri faisant partie du MSP d'une organisation qui a été déployé sur une autre console, vous devez importer le MSP et l'identité admin MSP associée.  L'opérateur réseau qui a créé le MSP doit exporter la définition MSP de l'organisation et l'identité admin correspondante dans des fichiers JSON et les partager avec vous.

- Si vous envisagez de créer un homologue ou un service de tri faisant partie de l'organisation, importez l'identité admin MSP associée dans votre portefeuille.
- L'importation de la définition MSP de l'organisation s'effectue depuis l'onglet **Organisations**.
- Cliquez sur **Importer une définition de MSP** pour télécharger le fichier JSON.
- (Facultatif) Si vous avez importé l'identité admin MSP dans votre portefeuille, sélectionnez la case `Je dispose d'une identité administrateur pour la définition MSP`. Si vous ne sélectionnez pas cette option, lorsque vous essaierez ultérieurement de créer un homologue ou un service de tri, la définition MSP de cette organisation ne figurera pas dans la liste déroulante MSP.

## Importation de noeuds depuis {{site.data.keyword.cloud_notm}} Private
{: #ibp-console-import-icp}

Vous pouvez importer des noeuds créés dans {{site.data.keyword.cloud_notm}} Private sur des consoles qui sont déployées dans d'autres clusters {{site.data.keyword.cloud_notm}} Private ou dans {{site.data.keyword.cloud_notm}}. Cependant, vous devez vous assurer que le port utilisé par l'URL gRPC de vos noeuds est exposé depuis l'extérieur du cluster. Si vous déployez {{site.data.keyword.cloud_notm}} Private derrière un pare-feu, vous devez activer un passe-système, à l'aide d'une liste blanche par exemple, pour autoriser la console à l'extérieur du cluster pour communiquer avec vos noeuds.

Par exemple, vous pouvez trouver le fichier JSON de l'homologue qui a été exporté depuis {{site.data.keyword.cloud_notm}} Private ci-dessous. Pour communiquer avec l'homologue à partir d'une autre console, vous devez vous assurer que le port `grpcwp_url`, port 32403 dans cet exemple, est ouvert au trafic externe.

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
