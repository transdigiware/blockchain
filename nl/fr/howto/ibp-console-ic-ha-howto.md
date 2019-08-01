---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# Configuration déploiements haute disponibilité (HA) multi-région
{: #ibp-console-hadr-mr}

La configuration de couverture multi-région offre le plus haut degré de couverture HA possible. Le déploiement d'homologues dans plusieurs régions géographiques garantit que si une région devient indisponible, les homologues des autres régions peuvent continuer à réaliser des transactions. Notez que la prise en charge de la haute disponibilité multi-région pour les autorités de certification et le service de tri n'est pas actuellement disponible.

## Présentation
{: #ibp-console-hadr-overview}

Lorsque vous configurez la prise en charge de la haute disponibilité multi-région, vous devez exécuter les tâches suivantes :
- Créez plusieurs instances de service dans {{site.data.keyword.cloud}}, chacune étant liée à un cluster Kubernetes dans une région différente.
- Créez vos noeuds blockchain dans différentes régions.
- Utilisez la fonctionnalité d'importation/exportation de noeud pour gérer les noeuds à partir d'une seule console.

## Etapes de la configuration
{: #ibp-console-hadr-config}

Pour configurer la haute disponibilité multi-région en créant des homologues redondants pour chaque organisation, exécutez les étapes suivantes lorsque vous configurez votre réseau de blockchain :

1. Créez trois clusters {{site.data.keyword.cloud_notm}} Kubernetes dans les régions de votre choix. Ces clusters peuvent se situer dans une région que vous choisissez, mais si pour des performance élevés, ils doivent être relativement proches les uns des autres. Par exemple, les régions E.U cote Est et E.U cote Ouest, ainsi que le Canada sont préférables aux régions E.U cote Ouest, Londres et Tokyo.
2. Déployer une nouvelle instance d'{{site.data.keyword.blockchainfull_notm}} Platform et liez-la au cluster dans la première région. Déployez ensuite une autre instance d'{{site.data.keyword.blockchainfull_notm}} Platform et liez-la au cluster dans la seconde région. Répétez ces étapes pour lier une troisième instance de service dans cluster de la troisième zone. Lorsque vous avez terminé, vous disposez de trois instances distinctes d'{{site.data.keyword.blockchainfull_notm}} liées à trois clusters distincts, chacun dans une région différente, et sur trois consoles distinctes.

Ce tutoriel suppose qu'un service de tri existe avec un canal défini qui peut être rejoint par les homologues.
{: important}

### Etape 1 : Créer l'autorité de certification de l'organisation homologue et les métadonnées dans le cluster 1
{: #ibp-console-hadr-peerCA}

1. Déployez une autorité de certification dans le premier cluster en suivant les instructions fournies dans le tutoriel Générer un réseau pour [créer l'autorité de certification de votre organisation homologue](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA). Notez les valeurs **ID d'inscription** et **secret** de l'autorité de certification car vous devez fournir ces valeurs lors de l'importation de l'AC dans d'autres clusters.
2. Dès que le voyant d'état de la vignette AC est vert, `En cours d'exécution`, suivez les instructions pour [Enregistrer les identités auprès de votre autorité de certification](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). Dans ces instructions, vous créez deux identités, une pour l'homologue et une autre pour l'identité admin de l'organisation homologue.
3. [Créez la définition MSP de l'organisation de l'homologue](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) pour l'organisation de l'homologue dans le premier cluster.
4. [Créez un homologue](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) dans le premier cluster.
5. [Installez votre contrat intelligent](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) sur votre homologue.

Avant d'instancier le contrat intelligent sur le canal, vous devez suivre les étapes permettant de joindre l'homologue au canal dans le service de tri :
- [Exportez la définition de l'organisation de l'homologue](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) et partagez-la avec un admin de service de tri.
- L'admin de service de tri doit suivre les étapes lui permettant d'[importer l'organisation de l'homologue](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) et de l'ajouter au consortium. Il devra également effectuer les étapes de ces instructions pour exporter le service de tri dans un fichier JSON.
- [Importez le fichier JSON du service de tri](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) sur votre console.
- [Joignez votre homologue au canal](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Enfin, si vous utilisez la reconnaissance de service, les données privées et les communications gossip, l'admin de service de tri doit [configurer des homologues d'ancrage](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) sur le canal. Pour la haute disponibilité, il est recommandé d'ajouter chaque homologue redondant en tant qu'homologue d'ancrage. De cette manière, si l'un des homologues est non disponible, la communication gossip entre organizations peut se poursuivre.   

A présent que vous avez joint l'homologue au canal, vous pouvez [instancier le contrat intelligent](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) sur le canal.

### Etape 2 : Exporter les métadonnées et les identités à partir du cluster 1
{: #ibp-console-hadr-export-meta1}

1. Exportez la définition AC dans un fichier JSON.
   - Ouvrez l'autorité de certification sous l'onglet **Noeuds**.
   - Cliquez sur l'icône de téléchargement afin de générer le fichier JSON AC depuis votre session de navigateur.
2. Exportez la définition MSP de l'organisation de l'homologue dans un fichier JSON.
   - Accédez à la définition MSP sous l'onglet **Organisations**.
   - Cliquez sur l'icône de téléchargement sur la vignette.
3. Exportez l'identité admin de l'organisation de l'homologue depuis votre portefeuille.
   - Accédez à l'onglet **Portefeuille**.
   - Cliquez sur l'identité admin de l'organisation de l'homologue, puis cliquez sur **Exporter une identité**.
   - Un fichier JSON contenant les certificats admin de l'organisation est créé. Prenez note du nom de fichier et gardez-le en lieu sûr car il est demandé lors de la création d'homologues dans vos autres clusters.

### Etape 3 : Importer les métadonnées et les identités dans les clusters 2 et 3
{: #ibp-console-hadr-import-meta23}

1. Dans les clusters 2 et 3, [importez le fichier JSON AC](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca) que vous avez exporté depuis le cluster 1.  
2. Une fois le fichier JSON téléchargé, vous devez entrer l'ID d'inscription et le secret de l'administrateur AC, ainsi que l'ID d'inscription et le secret de l'administrateur AC TLS que vous avez utilisés lors du déploiement de l'AC dans le cluster 1.
2. Importer le fichier JSON de la de définition MSP de l'organisation homologue que vous avez exporté à partir du cluster 1.
   - Sous l'onglet **Organisations**, cliquez sur **Importer une définition de MSP**.
   - Cliquez sur **Ajouter un fichier** afin de télécharger le fichier JSON.
   - Cliquez sur **Je dispose d'une identité administrateur pour la définition MSP** pour être autorisé à utiliser cette définition MSP lors de la création de nouveaux homologues dans d'autres zones.
   - Cliquez sur **Importer une définition de MSP**.
3. Importez le fichier JSON de l'identité admin d'organisation que vous avez exporté depuis le cluster 1 dans le portefeuille de console des clusters 2 et 3.

### Etape 4 : Créer de nouveaux homologues dans les clusters 2 et 3 et joindre un canal
{: #ibp-console-hadr-create-new-peers}

1. Dans les clusters 2 et 3, utilisez l'autorité de certification pour enregistrer un nouvel utilisateur homologue, en suivant les mêmes étapes que pour l'enregistrement de l'identité homologue dans le cluster 1. Vous devez uniquement créer les utilisateurs homologue, vous ne devez pas recréer l'identité admin de l'organisation car vous importerez celle-ci à l'étape suivante.
2. Créez de nouveaux homologues, en utilisant l'AC que vous avez importée depuis le cluster 1 en tant qu' AC de l'homologue et en utilisant le MSP d'organisation que vous avez importé depuis le cluster 1 en tant que définition MSP de l'organisation homologue.
3. Dans les clusters 2 et 3, vous pouvez maintenant répéter les étapes que vous avez effectuées pour joindre les homologues au même canal que l'homologue dans le cluster 1. 
4. Une fois le contrat intelligent installé sur ces homologues redondants, le registre se synchronise automatiquement entre toutes les homologues.
5. Encore une fois, si vous prévoyez d'utiliser la reconnaissance de service, des données privées, et la communication gossip d'homologue, vous devez définir chaque homologue redondant en tant qu'homologue d'ancrage.  

Votre réseau est maintenant configuré de sorte que si une défaillance se produit dans une seule région, elle n'affecte pas la charge de travail de l'homologue.  

Pour optimiser encore davantage votre haute disponibilité, vous disposez des options suivantes :
- Exportez les homologues que vous avez créés dans les clusters 2 et 3 et les importer sur la console dans le cluster 1. Ensuite tout peut être géré à partir d'un cluster unique.
- Toutefois, le cluster 1 peut être défaillant. Par conséquent, pour mieux tenir compte de l'éventualité d'une défaillance de cluster, vous pouvez importer tous vos homologues sur chacune des trois consoles. Ensuite, si le cluster contenant une seule console est défaillant, tout peut encore être géré à partir des consoles des deux autres clusters.
