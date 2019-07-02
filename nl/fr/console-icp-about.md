---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# A propos d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform peut être déployé au sein de clouds publics et privés, tels que {{site.data.keyword.cloud_notm}}, votre propre centre de données, ainsi que des clouds publics de tiers. Ce déploiement multicloud est possible via {{site.data.keyword.cloud_notm}} Private, plateforme d'orchestration de conteneurs basée sur Kubernetes d'IBM. Cette édition fournit une interface utilisateur de console de blockchain que vous pouvez utiliser pour déployer et gérer les composants de blockchain sur un cluster {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private utilise la base de code Hyperledger Fabric version 1.4.1 et prend en charge le déploiement sur {{site.data.keyword.cloud_notm}} Private version 3.2.0.
{:shortdesc}

## Offres d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-offers}

Vous pouvez utiliser cette offre pour installer la console {{site.data.keyword.blockchainfull_notm}} Platform dans un déploiement d'{{site.data.keyword.cloud_notm}} Private. Vous pouvez ensuite utiliser la console pour créer tous les composants de base de la blockchain Hyperledger Fabric, une autorité de certification ainsi qu'un service de tri et des homologues, sur votre cluster local. Pour plus d'informations sur les blocs de construction des réseaux Hyperledger Fabric, voir la [présentation du composant de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview). Vous pouvez également utiliser votre console pour exploiter un réseau multicloud distribué en important des noeuds déployés sur d'autres clusters {{site.data.keyword.cloud_notm}} Private ou sur {{site.data.keyword.cloud_notm}}.

Cette édition d'{{site.data.keyword.blockchainfull_notm}} Platform présente les caractéristiques suivantes :

**BUILD ---- Expérience de développement intégrée**
- **Codez facilement ** vos contrats intelligents en Node.js, Golang ou Java, écrivez des applications client à l'aide de la nouvelle extension {{site.data.keyword.blockchainfull_notm}} VS Code, optimisez l'**intégration de logiciels SDK** avec la console d'interface utilisateur, et apprenez avec nos tutoriels et exemples enrichis.
- **Simplified DevOps** vous permet de passer du développement au test en production dans un environnement unique avec l'extension de vos ressources Kubernetes pour l'ajout de composants supplémentaires.
- **Intégration de service Kubernetes.** Optimisez des services tels que Grafana et Prometheus pour la consignation et Kibana pour la surveillance.
- **A jour des fonctions principales de Fabric**. Bénéficiez des fonctionnalités les plus récentes d'Hyperledger Fabric v1.4.1 :
  - [Service de tri Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Collectes de données privées](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) qui permettent une meilleure confidentialité des données avec la garantie que les données de registre sont partagées sur des homologues autorisés uniquement via le protocole gossip.
  - [Reconnaissance de service (Service discovery)](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, qui vous permet de découvrir et de mettre à jour de manière dynamique la façon dont votre application interagit avec votre réseau.
  - [Listes de contrôle d'accès](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} qui vous donnent un contrôle supplémentaire sur la gouvernance de vos canaux et contrats intelligents.

**OPERATE --- Contrôle total de vos déploiements**
- **Déployez uniquement les composants dont vous avez besoin**. Connectez un homologue à plusieurs canaux et réseaux, ou hébergez un service de tri auquel les partenaires commerciaux peuvent se connecter.
- **Conservez le contrôle intégral de vos identités**. Stockez et gérez les clés qui sont utilisées pour administrer vos noeuds dans votre propre environnement sécurisé.
- **Opération unifiée**. La console {{site.data.keyword.blockchainfull_notm}} Platform vous permet de déployer et de gérer l'ensemble de vos organisations et noeuds sur **une console** sans avoir à compter sur {{site.data.keyword.IBM_notm}} ou d'autres fournisseurs pour gérer vos noeuds de tri ou votre autorité de certification. Vous pouvez aussi ajouter ou retirer des membres d'un consortium de blockchain, créer et rejoindre des canaux, puis installer et instancier des contrats intelligents depuis votre console.
- **Hébergez ou rejoignez un réseau**. Déployez des homologues hébergés dans votre cluster sur plusieurs canaux dans différents clouds, ou invitez d'autres organisations à rejoindre votre consortium ou vos canaux, pendant que les organisations gèrent leurs noeuds de manière indépendante au sein d'infrastructures.
- **Gérez l'accès** des utilisateurs qui peuvent administrer ou surveiller vos noeuds.
- **Accédez directement aux journaux ** de vos noeuds de votre service Kubernetes. Utilisez un service tiers pour extraire et analyser vos journaux.
- **Interagissez directement avec vos pods** à l'aide de votre service Kubernetes.
- **Collecte de signatures dynamique** qui permet un meilleur contrôle sur la gouvernance collaborative sur les configurations de canal.

**GROW --- Evolutivité et flexibilité**
- **Choisissez votre calcul**. Vous avez la flexibilité de décider de la quantité d'UC, de mémoire et de stockage que vous voulez mettre à disposition dans votre cluster Kubernetes. Pour plus d'informations, voir [Comment la console interagit avec votre cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Augmentez** et réduisez le nombre de ressources dans votre cluster Kubernetes, en ne payant que ce que ce dont vous avez besoin. Pour plus d'informations, voir [Tarification](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Reprise après incident et disponibilité multizone.** Cette option duplique votre déploiement Kubernetes entre les zones, ce qui permet la haute disponibilité (HA) de vos composants et la reprise après incident (DR).
- **Exécutez n'importe où**. Grâce au **codebase unifié** de la console {{site.data.keyword.blockchainfull_notm}} Platform, il est possible d'exécuter vos composants dans n'importe quel environnement pris en charge par {{site.data.keyword.cloud_notm}} Private.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est un produit regroupé pour {{site.data.keyword.cloud_notm}} Private et il est fourni en tant que charte Helm Kubernetes. Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la documentation relative à [{{site.data.keyword.cloud_notm}} Private version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private vous convient-il ?
{: #console-icp-about-suitable}

L'exécution d'{{site.data.keyword.blockchainfull_notm}} Platform à l'extérieur de {{site.data.keyword.cloud_notm}} vous offre davantage de souplesse pour croître ou rejoindre un réseau de blockchain. Les créateurs de réseaux peuvent ainsi autoriser l'accès de nouveaux membres en utilisant la plateforme de leur choix. Les organisations qui souhaitent rejoindre des réseaux de blockchain peuvent ainsi regrouper leurs homologues avec leurs applications existantes ou s'intégrer à leurs systèmes d'enregistrement.

Les utilisateurs de cette offre gèrent leur propre sécurité et infrastructure. {{site.data.keyword.cloud_notm}} ne fournit pas ces services. Passez en revue les [Considerations et limitations](#console-icp-about-considerations) de la section suivante avant de poursuivre.

## Considérations et limitations
{: #console-icp-about-considerations}

Avant de commencer, tenez compte des limitations suivantes :

- Vous êtes chargé de gérer la surveillance, de la sécurité, de la journalisation et l'utilisation des ressources de vos composants de blockchain.
- La console peut uniquement être utilisée pour créer et gérer des composants basés sur Hyperledger Fabric versions 1.4.1 ou suivantes.
- Vous pouvez uniquement déployer un homologue de console par espace de noms Kubernetes. Si vous prévoyez de créer plusieurs réseaux de blockchain, par exemple pour créer différents environnements de développement, de transfert et de production, vous devez créer un espace de nom unique pour chaque environnement.
- Vous pouvez importer uniquement des noeuds qui ont été exportés depuis d'autres consoles {{site.data.keyword.blockchainfull_notm}} Platform. Pour pouvoir exploiter un homologue ou un noeud de tri importé depuis la console, vous devez également importer la définition MSP de l'organisation du noeud associé et l'identité administrateur sur votre console.
- {{site.data.keyword.blockchainfull_notm}} Platform est uniquement pris en charge dans {{site.data.keyword.cloud_notm}} Private version 3.2 Enterprise Edition. {{site.data.keyword.cloud_notm}} Private version 3.2 Community Edition n'est pas pris en charge.
- Vous ne pouvez pas utiliser {{site.data.keyword.IBM_notm}} Multicloud Manager pour installer la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform.

Pour une vue d'ensemble des environnement pris en charge par {{site.data.keyword.cloud_notm}} Private, voir [Supported environments](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Configuration système prérequise
{: #console-icp-about-prerequisites}

Consultez [Configuration requise](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} pour connaître la liste des éléments logiciels et matériels prérequis pour {{site.data.keyword.cloud_notm}} Private. Notez toutefois qu'{{site.data.keyword.blockchainfull_notm}} Platform n'est pas pris en charge sur les systèmes Linux on Power (ppc64le) POWER8.

La charte Helm {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est validée pour une exécution sur des clusters {{site.data.keyword.cloud_notm}} Private version 3.2 sur Ubuntu Linux en utilisant les noeuds worker et le stockage de secours suivants :

- **LinuxONE et {{site.data.keyword.IBM_notm}} Z** : z/VM et KVM, qui utilisent NFS.
- **x86** : Linux 64 bits qui utilise GlusterFS.

## Licence
{: #console-icp-about-license}

Pour plus d'informations sur les licences et la tarification, voir [Tarification de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing).

## Installation d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est fourni en tant que fichier de charte Helm qui peut être installé dans un cluster {{site.data.keyword.cloud_notm}} Private local. Une fois que vous avez installé la charte Helm, vous pouvez trouver {{site.data.keyword.blockchainfull_notm}} Platform en tant qu'application dans le catalogue {{site.data.keyword.cloud_notm}} Private. Pour plus de détails sur l'installation de la charte Helm et des prérequis, voir [Installation de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install).

Si vous êtes un nouvel utilisateur de {{site.data.keyword.cloud_notm}} Private et souhaitez des informations et des conseils sur l'installation et le développement d'{{site.data.keyword.cloud_notm}} Private, voir [Configuration de {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

Vous pouvez déployer {{site.data.keyword.blockchainfull_notm}} Platform derrière un pare-feu sans avoir à accéder à l'Internet public. Le package de charte Helm téléchargé inclut l'ensemble des images Docker du composant Fabric utilisées par {{site.data.keyword.blockchainfull_notm}} Platform, sans leur extraction de DockerHub durant le déploiement.

## Remarques sur la sécurité
{: #console-icp-about-security}

Etant donné que les composants sont déployés dans votre propre infrastructure, vous êtes responsable de la gestion de leur sécurité. Cela inclut des zones de sécurité importantes, comme la gestion des clés et le chiffrement de données. Passez en revue les rubriques suivantes concernant la sécurité de vos composants.

### Sécurité des données
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} utilise le chiffrement de disque intégral qui repose sur le [chiffrement de clé symétrique](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} pour protéger l'ensemble des données crées par chaque instance de service. Vous devez suivre des étapes similaires dans votre environnement pour protéger les données de votre homologue.

Les données de votre base de données d'état, que vous utilisiez LevelDB ou CouchDB, ne sont pas chiffrées. Vous pouvez utiliser le chiffrement au niveau de l'application afin de protéger les données au repos dans votre base de données d'état.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### Gestion des clés
{: #console-icp-about-security-key-management}

La gestion des clés est un aspect essentiel de la sécurité. Si une clé privée est compromise ou égarée, des acteurs hostiles pourraient accéder aux données et aux fonctionnalités. {{site.data.keyword.IBM_notm}} utilise des dispositifs physiques appelés [modules de sécurité matérielle](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) pour stocker les clés privées des réseaux {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan.

Vous êtes responsable de la gestion de vos clés privées. Bien que vous puissiez utiliser l'interface utilisateur de la console {{site.data.keyword.blockchainfull_notm}} Platform pour générer des clés privées, celles-ci ne sont pas stockées par la console ou dans {{site.data.keyword.cloud_notm}} Private. Il est essentiel que vous stockiez vos clés dans un emplacement sécurisé afin qu'elles ne soient pas compromises.

Vous pouvez utiliser Key Escrow pour récupérer des clés privées perdues. Cette opération doit être effectuée avant la perte de clés. Si une clé privée ne peut pas être récupérée, vous devez obtenir de nouvelles clés privées en enregistrant une nouvelle identité avec votre autorité de certification. Vous devez également retirer et remplacer votre signCert des canaux que vous avez rejoints.

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) est intégré au modèle de confiance d'Hyperledger Fabric. Tous les composants {{site.data.keyword.blockchainfull_notm}} Platform utilisent TLS pour s'authentifier et communiquer entre eux. Par conséquent, les noeuds sur {{site.data.keyword.cloud_notm}} Private doivent pouvoir effectuer un établissement de liaison TLS avec d'autres composants et vos applications. Cela implique que vous devez activer un passe-système, à l'aide d'une liste blanche par exemple, dans vos applications client de pare-feu sur vos noeuds.

### Sécurité des applications
{: #console-icp-about-security-appl}

Tous les appels de code blockchain étant signés, Fabric gère la sécurité des applications. De plus, Fabric inclut également des vérifications au niveau de l'application basées sur ACL.

## Support
{: #console-icp-about-support}

Pour plus d'informations sur l'obtention d'un support sur {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, et pour accéder à des ressources de développeur de blockchain gratuites et à des forums de support pour la résolution de problèmes, voir [Support](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Si vous avez acheté une licence {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private et souhaitez contacter le support client, consultez les informations relatives à l'[accès à {{site.data.keyword.IBM_notm}} Support Community et à l'ouverture d'un ticket de demande de service](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
