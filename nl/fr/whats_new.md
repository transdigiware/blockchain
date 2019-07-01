---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-21"

keywords: IBM Blockchain Platform, release, new features

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# Nouveautés
{: #whats-new}

## 18 juin 2019
{: #whats-new-6-18-2019}

{{site.data.keyword.blockchainfull}} Platform for Multicloud, qui permet l'utilisation de la seconde génération d'{{site.data.keyword.blockchainfull_notm}} Platform dans votre propre infrastructure et le fournisseur de cloud Kubernetes de votre choix, est disponible globalement. Cette édition permet d'utiliser la console d'interface utilisateur pour déployer et gérer des composants de blockchain dans votre propre environnement avec {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud utilise la base de code Hyperledger Fabric version 1.4.1 et prend en charge le déploiement dans {{site.data.keyword.cloud_notm}} Private version 3.2.

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
- **Opération unifiée**. La console {{site.data.keyword.blockchainfull_notm}} Platform vous permet de déployer et de gérer l'ensemble de vos organisations et noeuds sur **une console centrale** sans avoir à compter sur {{site.data.keyword.IBM_notm}} ou d'autres fournisseurs pour gérer vos services de tri ou votre autorité de certification. Vous pouvez aussi ajouter ou retirer des membres d'un consortium de blockchain, créer et rejoindre des canaux, puis installer et instancier des contrats intelligents depuis votre console.
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


Les **Instructions relatives au déploiement figurent dans la section [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).**

- Des informations supplémentaires figurent dans [A propos d'{{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview).
- Des tutoriels mis à jour relatifs à l'utilisation de {{site.data.keyword.blockchainfull_notm}} Platform sont disponibles dans la sous-section **{{site.data.keyword.blockchainfull_notm}} Platform console** sous la catégorie **Procédures détaillées**.
  * Le [réseau Générer un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) vous guide tout au long du processus d'hébergement d'un réseau par la création d'autorités de certification, d'un service de tri et d'un homologue.
  * Le [tutoriel Rejoindre un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network) vous explique comment rejoindre un réseau existant en créant un homologue et en le joignant à un canal.
  * [Déployer un contrat intelligent sur le réseau ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts): Ce tutoriel fournit des informations sur l'écriture d'un contrat intelligent et son déploiement sur votre réseau.
- L'offre {{site.data.keyword.blockchainfull_notm}} Platform repose sur Hyperledger Fabric version 1.4 et elle prend en charge le protocole gossip entre homologues, la reconnaissance de service et les données privées. Pour en savoir plus sur la configuration des données privées sur votre réseau, voir la rubrique [](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).

Pour plus d'informations sur Hyperledger Fabric version 1.4.1, consultez la [documentation Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la [documentation d'{{site.data.keyword.cloud_notm}} Private version 3.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external} .

La documentation relatives aux éditions précédentes d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private a été déplacée à un nouvel endroit. Si vous utilisez encore {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private version 1.0.1 ou version 1.0.2, consultez la [documentation d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain-icp-102?topic=blockchain-icp-102-get-started-icp).

## 31 mai 2019
{: #whats-new-5-31-2019}

La deuxième génération d'{{site.data.keyword.blockchainfull_notm}} Platform, qui vous permet de déployer, gérer et surveiller votre réseau de blockchain est disponible globalement. Cette édition comporte une nouvelle console d'interface utilisateur, laquelle peut être utilisée pour déployer et gérer des composants de blockchain dans votre propre cluster {{site.data.keyword.IBM_notm}} Kubernetes Service sur {{site.data.keyword.cloud_notm}}.

Cette édition d'{{site.data.keyword.blockchainfull_notm}} Platform présente les caractéristiques suivantes :

**BUILD ---- Expérience de développement intégrée**
- **Codez facilement** vos contrats intelligents en Node.js, Golang ou Java, écrivez des applications client à l'aide de la nouvelle extension {{site.data.keyword.blockchainfull_notm}} VS Code, optimisez l'**intégration SDK** avec la console, et apprenez avec nos tutoriels et exemples enrichis.
- **Simplified DevOps** vous permet de passer du développement au test en production dans un environnement unique avec l'extension de vos ressources Kubernetes pour l'ajout de composants supplémentaires.
- **A jour des fonctions principales de Fabric**. Bénéficiez des fonctionnalités les plus récentes d'Hyperledger Fabric v1.4.1 :
  -  [Service de tri Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - **Intégration de service {{site.data.keyword.cloud_notm}}.** Optimisez les services {{site.data.keyword.cloud_notm}} intégrés, tels que {{site.data.keyword.cloud_notm}} Kubernetes Service Dashboard, {{site.data.keyword.IBM_notm}} Log Analysis with LogDNA et {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
  - [**Collectes de données** privées](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) qui permettent une meilleure confidentialité des données avec la garantie que les données de registre sont partagées sur des homologues autorisés uniquement via le protocole gossip.
  - [Reconnaissance de service (Service Discovery)](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, qui vous permet de découvrir et de mettre à jour de manière dynamique la façon dont votre application interagit avec votre réseau.
  - [Listes de contrôle d'accès](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} qui vous donnent un contrôle supplémentaire sur la gouvernance de vos canaux et contrats intelligents.

**OPERATE --- Contrôle total de vos déploiements**
- **Déployez uniquement les composants dont vous avez besoin**. Connectez un homologue à plusieurs canaux et réseaux, ou hébergez un service de tri auquel les partenaires commerciaux peuvent se connecter.
- **Conservez le contrôle intégral de vos identités**. Stockez et gérez les clés qui sont utilisées pour administrer vos noeuds sans stocker vos clés privées sur {{site.data.keyword.cloud_notm}}.
- **Opération centralisée**. La console {{site.data.keyword.blockchainfull_notm}} Platform vous permet de déployer et de gérer l'ensemble de vos organisations et noeuds sur **une console centrale** sans avoir à compter sur {{site.data.keyword.IBM_notm}} ou d'autres fournisseurs pour gérer vos services de tri ou votre autorité de certification. Vous pouvez aussi ajouter ou retirer des membres d'un consortium de blockchain, créer et rejoindre des canaux, puis installer et instancier des contrats intelligents depuis votre console.
- **Hébergez ou rejoignez un réseau**. Déployez des homologues hébergés dans votre cluster sur plusieurs canaux dans différents clouds, ou invitez d'autres organisations à rejoindre votre consortium ou vos canaux, pendant que les organisations gèrent leurs noeuds de manière indépendante au sein d'infrastructures.
- **Gérez l'accès** des utilisateurs qui peuvent administrer ou surveiller vos noeuds.
- **Accédez directement aux journaux** de vos noeuds depuis le service {{site.data.keyword.IBM_notm}} Kubernetes. Utilisez le service {{site.data.keyword.cloud_notm}} Log Analysis ou un service tiers pour extraire et analyser vos journaux.
- **Interagissez directement avec vos pods** à l'aide de Kubernetes Dashboard. Lancez vos pods et conteneurs pour exécuter des commandes et mettre à jour des certificats depuis la ligne de commande.
- **Collecte de signatures dynamique** qui permet un meilleur contrôle sur la gouvernance collaborative sur les configurations de canal.

**GROW --- Evolutivité et flexibilité**
- **Choisissez votre calcul**. Vous avez la flexibilité de décider de la quantité d'UC, de mémoire et de stockage que vous voulez mettre à disposition dans votre cluster Kubernetes. Pour plus d'informations, voir [Comment {{site.data.keyword.cloud_notm}} Kubernetes Service interagit avec la console](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Augmentez** et réduisez le nombre de ressources dans votre cluster Kubernetes, en ne payant que ce que ce dont vous avez besoin. Pour plus d'informations, voir [Tarification](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Reprise après incident et disponibilité multizone.** Cette option duplique votre déploiement Kubernetes entre les zones, ce qui permet la haute disponibilité (HA) de vos composants et la reprise après incident (DR).
- **Exécutez n'importe où** (instructions bientôt disponibles). Grâce au **codebase unifié** de la console {{site.data.keyword.blockchainfull_notm}} Platform, il est possible d'exécuter vos composants dans {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Private et dans des clouds public tiers.

- Des informations supplémentaires concernant {{site.data.keyword.blockchainfull_notm}} Platform sont disponibles dans la section [A propos de {{site.data.keyword.blockchainfull_notm}} Platform version 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview).
- Les instructions relatives au déploiement de cette version dans un cluster {{site.data.keyword.IBM_notm}} Kubernetes Service sont disponibles dans la section [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform version 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
- De nouveaux tutoriels relatifs à l'utilisation de {{site.data.keyword.blockchainfull_notm}} Platform sont disponibles dans la sous-section **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** sous la catégorie **Procédures détaillées**.
  * [Générer un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) : Ce tutoriel vous guide tout au long du processus d'hébergement d'un réseau par la création d'un service de tri et d'un homologue.
  * Le [tutoriel Rejoindre un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network) vous explique comment rejoindre un réseau existant en créant un homologue et en le joignant à un canal.
  * [Déployer un contrat intelligent sur le réseau ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts): Ce tutoriel fournit des informations sur l'écriture d'un contrat intelligent et son déploiement sur votre réseau.
- L'offre {{site.data.keyword.blockchainfull_notm}} Platform repose sur Hyperledger Fabric version 1.4 et elle prend en charge le protocole gossip entre homologues, la reconnaissance de service et les données privées. Pour en savoir plus sur la configuration des données privées sur votre réseau, voir la rubrique [](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).

## 9 mai 2019
{: #whats-new-5-09-2019}

{{site.data.keyword.blockchainfull_notm}} publie la version 1.0.0 de l'extension de code {{site.data.keyword.blockchainfull_notm}} Platform Visual Studio (VS). Ce kit d'outils de développeur comporte les fonctionnalités suivantes :

**Une interface utilisateur reconfigurée pour une navigation plus simple**

**Des tutoriels prescriptifs et détaillés qui expliquent comment :**
- Développer un contrat intelligent
- Mettre à disposition une instance du service {{site.data.keyword.blockchainfull_notm}} sur {{site.data.keyword.cloud_notm}}
- Déployer une instance de service {{site.data.keyword.blockchainfull_notm}} Platform et effectuer des transactions

**L'ensemble des fonctionnalités des éditions précédentes, et notamment :**
- Fonction de débogage des contrats intelligent
- Exemple de code
- Fonctionnalité de portefeuille mise à jour

Pour plus d'informations, voir [Développement de contrats intelligents avec l'extension de code Visual Studio](/docs/services/blockchain?topic=blockchain-develop-vscode "Développement de contrats intelligents avec l'extension de code Visual Studio").

## 23 avril 2019
{: #whats-new-4-23-2019}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private présente la version 1.0.2, qui utilise la base de code Hyperledger Fabric version 1.4.0 et prend en charge le déploiement sur {{site.data.keyword.cloud_notm}} Private version 3.1.2.

Si vous voulez mettre à niveau votre version existante de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private vers la version 1.0.2, voir [Mise à niveau de la charte Helm sur {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain-icp-102/howto?topic=blockchain-icp-102-helm-install#helm-install-upgrading).

Pour plus d'informations sur Hyperledger Fabric version 1.4.0, consultez la [documentation Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la [documentation {{site.data.keyword.cloud_notm}} Private version 3.1.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.2/kc_welcome_containers.html){: external}.

## 8 Février 2019
{: #whats-new-2-08-2019}

{{site.data.keyword.blockchainfull_notm}} Platform présente une version 2.0 bêta gratuite, génération suivante des solutions {{site.data.keyword.blockchainfull_notm}} Platform qui vous permet de déployer, exploiter et surveiller votre réseau de blockchain. {{site.data.keyword.blockchainfull_notm}} Platform version 2.0 bêta gratuite comporte une nouvelle console d'interface utilisateur, laquelle peut être utilisée pour déployer et gérer des composants de blockchain dans votre propre cluster {{site.data.keyword.IBM_notm}} Kubernetes Service on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.blockchainfull_notm}} Platform version 2.0 bêta gratuite vous permet de :

**Générer votre réseau plus vite et plus facilement en toute transparence**

*	Intégration sans problème entre le développement de contrat intelligent (par opposition au code) et la gestion de réseau
*	Simplified DevOps vous permet de passer du développement au test en production depuis une simple console.
*	Possibilité d'écrire des contrats intelligents en Javascript, Java et en Go

**Exploiter et gouverner des réseaux avec un contrôle total**

*	Déployez uniquement les composants de blockchain dont vous avez besoin (homologue, service de tri, autorité de certification)
*	La nouvelle console vous permet de gérer les composants réseau à un seul emplacement, peu importe où ils sont déployés
*	Conservez le contrôle intégral de vos identités, registres et contrats intelligents

**Etendre les réseaux distribués en toute simplicité grâce à la nouvelle souplesse multicloud**

*	Connectez-vous à des noeuds de n'importe quel environnement (sur site, public, clouds hybrides)
*	Connectez facilement un seul homologue à plusieurs réseaux industriels
*	Démarrez petit, payez pour ce que vous consommez et développez-vous sans investissement initial, et mettez-vous facilement à niveau avec Kubernetes

- Des informations supplémentaires concernant la version 2.0 bêta gratuite d'{{site.data.keyword.blockchainfull_notm}} Platform sont disponibles dans la section [A propos de {{site.data.keyword.blockchainfull_notm}} Platform version 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview).
- Les instructions relatives au déploiement de la version 2.0 bêta gratuite dans un cluster {{site.data.keyword.IBM_notm}} Kubernetes Service sont disponibles dans la section [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform version 2.0](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
- De nouveaux tutoriels relatifs à l'utilisation de {{site.data.keyword.blockchainfull_notm}} Platform version 2.0 bêta gratuite sont disponibles dans la sous-section **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** sous la catégorie **Procédures détaillées**.
  * [Générer un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) : Ce tutoriel vous guide tout au long du processus d'hébergement d'un réseau par la création d'un service de tri et d'un homologue.
  * Le [tutoriel Rejoindre un réseau](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network) vous explique comment rejoindre un réseau existant en créant un homologue et en le joignant à un canal.
  * [Déployer un contrat intelligent sur le réseau ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts): Ce tutoriel fournit des informations sur l'écriture d'un contrat intelligent et son déploiement sur votre réseau.
- L'offre {{site.data.keyword.blockchainfull_notm}} Platform version 2.0 bêta gratuite repose sur Hyperledger Fabric version 1.4 et elle prend en charge le protocole gossip entre homologues, la reconnaissance de service et les données privées. Pour en savoir plus sur la configuration des données privées sur votre réseau, voir la rubrique [](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).

- L'extension {{site.data.keyword.blockchainfull_notm}} Visual Studio Code est disponible à partir de Visual Studio Code Marketplace. Les développeurs peuvent utiliser cette extension pour créer, tester et déployer des contrats intelligents dans une instance d'Hyperledger Fabric. L'extension est compatible avec Hyperledger Fabric versions 1.3 et suivantes. Pour plus d'informations sur l'utilisation de l'extension VS Code, voir [Outils pour contrats intelligents](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode).  

Des améliorations ont été apportées à la table des matières dans laquelle toutes les rubriques d'initiation sont désormais regroupées dans une section appelée **Tutoriels d'initiation**. De plus, chacune des offres {{site.data.keyword.blockchainfull_notm}} Platform est décrite dans les sous-sections **A propos de {{site.data.keyword.blockchainfull_notm}} Platform** et figurent désormais sous la section **LEARN**.

**Remarque :** Les crédits de cloud gratuits ne sont plus disponibles pour les utilisateurs de plan Starter.

## 7 Décembre 2018
{: #whats-new-12-07-2018}

Les rubriques *Exploitation d'un réseau de plan Starter* et *Exploitation d'un réseau de plan Enterprise* sont combinées et remplacées par un tutoriel unique [Utilisation du Moniteur réseau](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard).

## 27 novembre 2018
{: #whats-new-11-27-2018}

{{site.data.keyword.blockchainfull_notm}} Platform présente {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.cloud_notm}} Private est une plateforme d'application pour le développement et la gestion d'applications conteneurisées basées sur Kubernetes, qui permet aux utilisateurs de déployer des autorités de certification, des services de tri, ainsi que des homologues sur x86, LinuxONE et {{site.data.keyword.IBM_notm}} Z. {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private repose sur Hyperledger Fabric v1.2.1 et est déployé à l'aide de chartes Helm Kubernetes.. Une fois que vous avez installé la charte Helm, vous pouvez la trouver sous forme d'un service intégré dans le catalogue {{site.data.keyword.cloud_notm}} Private. Notez que [cette offre](/docs/services/blockchain-icp-102?topic=blockchain-icp-102-ibp-icp-about#ibp-icp-about) convient davantage aux utilisateurs Fabric expérimentés.

Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la [documentation {{site.data.keyword.cloud_notm}} Private version 3.1.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/kc_welcome_containers.html){: external}.

L'édition d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private marque également la fin du programme {{site.data.keyword.blockchainfull_notm}} Platform Remote Peer (bêta). Il est toujours possible de déployer un homologue dans {{site.data.keyword.cloud_notm}} Private ou dans AWS et de le connecter à un réseau de plan Starter ou Enterprise. Pour plus d'informations, voir les sections relatives au [déploiement d'un homologue pour un réseau de plan Starter ou Enterprise](/docs/services/blockchain-icp-102/howto?topic=blockchain-icp-102-ibp-peer-deploy#ibp-peer-deploy) et au [déploiement d'un homologue dans AWS](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws). Ces homologues sont considérés comme étant des homologues **distribués** au lieu d'homologues distants car le déploiement est géré par le client et par conséquent il n'y a aucun emplacement central de gestion à distance.

Cette édition introduit également des améliorations apportées à la table des matières de la documentation. Si vous êtes un utilisateur Starter ou Enterprise, votre contenu est toujours accessible dans les sections **LEARN**, **Procédures détaillées**, **Référence** et **Aide**, et les liens sont toujours identiques. Ils peuvent simplement se trouver dans des sous-sections différentes de la table des matières.

## 13 novembre 2018
{: #whats-new-11-13-2018}

{{site.data.keyword.blockchainfull_notm}} Platform présente {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services (AWS).

{{site.data.keyword.blockchainfull_notm}} Platform for AWS vous permet d'exécuter des homologues dans le cloud AWS et de connecter les homologues à des réseaux de blockchain existants dans {{site.data.keyword.cloud_notm}}. Vos homologues dans AWS peuvent optimiser le profil de connexion et d'autres composants de blockchain des réseaux existants, tout en vous offrant davantage de souplesse pour colocaliser vos homologues avec d'autres applications situées à l'extérieur de {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [A propos de {{site.data.keyword.blockchainfull_notm}} Platform pour Amazon Web Services](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws).
/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws

## 4 octobre 2018
{: #whats-new-10-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform met à jour le plan Starter depuis la version 1.1.0 d'Hyperledger Fabric vers la version 1.2.1. Pour plus de détails, voir [A propos du plan Starter](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about).

**Important :** La version 1.2.1 de Fabric n'est actuellement pas compatible avec la version bêta de l'homologue distant, qui utilise un homologue en version 1.1.0. Les réseaux de plan Starter, qui sont créés avant le 4 octobre 2018 et sont basés sur la version 1.1.0 de Fabric, ne sont pas concernés et ils peuvent toujours être utilisés avec la version bêta de l'homologue distant. {{site.data.keyword.blockchainfull_notm}} Platform mettra à jour la version bêta de l'homologue distant pour qu'elle puisse utiliser l'homologue en version 1.2.1, qui sera compatible avec les nouveaux réseaux de plan Starter, qui sont au niveau de version 1.2.1 de Fabric, et avec les réseaux de plan Enterprise, qui sont qui sont au niveau de version 1.1.0 de Fabric. Tant que la version bêta de l'homologue distant n'est pas mise à jour vers la version 1.2.1 de Fabric, n'essayez pas de déployer un homologue distant en version 1.1.0 avec un nouveau réseau de plan Starter en version 1.2.1.

## 4 septembre 2018
{: #whats-new-9-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform présente la version bêta de l'offre d'homologue distant. Cette offre est basée sur la version 1.1.0 d'Hyperledger Fabric. En utilisant l'homologue distant, vous pouvez exécuter des noeuds homologues {{site.data.keyword.blockchainfull_notm}} Platform dans votre propre environnement de cloud {{site.data.keyword.cloud_notm}} Private ou Amazon Web Services (AWS). Pour plus d'informations, voir [A propos des homologues distants](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws#remote-peer-aws).

## 15 juin 2018
{: #whats-new-6-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform annonce la disponibilité générale du plan Starter. Pour plus de détails, voir [A propos du plan Starter](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about).

## 15 mai 2018
{: #whats-new-5-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform met à niveau le plan Enterprise de la version 1.0.0 d'Hyperledger Fabric vers la version 1.1.0. Pour plus d'informations, voir [A propos du plan Enterprise](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about).

## 18 mars 2018
{: #whats-new-3-18-2018}

{{site.data.keyword.blockchainfull_notm}} Platform présente la version bêta du plan Starter. Le plan Starter vous offre un environnement de développement et de tests pour découvrir et apprendre à utiliser un réseau {{site.data.keyword.blockchainfull_notm}} Platform. Pour plus de détails, voir [A propos du plan Starter](/docs/services/blockchain?topic=blockchain-starter-plan-about#starter-plan-about).

## 11 août 2017
{: #whats-new-8-11-2017}

{{site.data.keyword.blockchainfull_notm}} Platform remplace
{{site.data.keyword.blockchainfull_notm}} on Cloud et présente le plan Enterprise. Pour plus d'informations, voir [A propos du plan Enterprise](/docs/services/blockchain?topic=blockchain-enterprise-plan-about#enterprise-plan-about).
