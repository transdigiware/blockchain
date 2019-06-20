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

{{site.data.keyword.blockchainfull}} Platform peut être déployé au sein de clouds publics et privés, tels que {{site.data.keyword.cloud_notm}}, votre propre centre de données, ainsi que des clouds publics de tiers. Ce déploiement multicloud est possible via {{site.data.keyword.cloud_notm}} Private, plateforme d'orchestration de conteneurs basée sur Kubernetes d'IBM. Cette édition fournit une interface utilisateur de console que vous pouvez utiliser pour déployer et gérer les composants de blockchain sur un cluster {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private utilise la base de code Hyperledger Fabric version 1.4.1 et prend en charge le déploiement sur {{site.data.keyword.cloud_notm}} Private version 3.2.
{:shortdesc}

Cette édition d'{{site.data.keyword.blockchainfull_notm}} Platform présente les caractéristiques suivantes :

**BUILD ---- Expérience de développement intégrée**
- **Codez facilement ** vos contrats intelligents en Node.js, Golang ou Java, écrivez des applications client à l'aide de la nouvelle extension {{site.data.keyword.blockchainfull_notm}} VS Code, optimisez l'**intégration de logiciels SDK** avec la console d'interface utilisateur, et apprenez avec nos tutoriels et exemples enrichis.
- **Simplified DevOps** vous permet de passer du développement au test en production dans un environnement unique avec l'extension de vos ressources Kubernetes pour l'ajout de composants supplémentaires.
- **Intégration de service Kubernetes.** Optimisez des services tels que Grafana et Prometheus pour la consignation et Kibana pour la surveillance.
- **A jour des fonctions principales de Fabric**. Bénéficiez des fonctionnalités les plus récentes d'Hyperledger Fabric v1.4.1 :
  - [Service de tri Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Collectes de données privées](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) qui permettent une meilleure confidentialité des données avec la garantie que les données de registre sont partagées sur des homologues autorisés uniquement via le protocole gossip.
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
- **Choisissez votre calcul**. Vous avez la flexibilité de décider de la quantité d'UC, de mémoire et de stockage que vous voulez mettre à disposition dans votre cluster Kubernetes. Pour plus d'informations, voir [Comment la console interagit avec votre cluster Kubernetes](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction).
- **Augmentez** et réduisez le nombre de ressources dans votre cluster Kubernetes, en ne payant que ce que ce dont vous avez besoin. Pour plus d'informations, voir [Tarification](/docs/services/blockchain/howto/pricing.html#ibp-pricing).
- **Reprise après incident et disponibilité multizone.** Cette option duplique votre déploiement Kubernetes entre les zones, ce qui permet la haute disponibilité (HA) de vos composants et la reprise après incident (DR).
- **Exécutez n'importe où**. Grâce au **codebase unifié** de la console {{site.data.keyword.blockchainfull_notm}} Platform, il est possible d'exécuter vos composants dans n'importe quel environnement pris en charge par {{site.data.keyword.cloud_notm}} Private.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est un produit regroupé pour {{site.data.keyword.cloud_notm}} Private et il est fourni en tant que charte Helm Kubernetes. Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la documentation relative à [{{site.data.keyword.cloud_notm}} Private version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Offres d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private vous permet d'installer la console {{site.data.keyword.blockchainfull_notm}} Platform dans un déploiement d'{{site.data.keyword.cloud_notm}} Private. Vous pouvez ensuite utiliser la console pour créer tous les composants de base de la blockchain Hyperledger Fabric, une autorité de certification ainsi qu'un service de tri et des homologues, sur votre cluster local. Pour plus d'informations sur les blocs de construction des réseaux Hyperledger Fabric, voir la [présentation du composant de blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview). Vous pouvez également utiliser votre console pour exploiter un réseau multicloud distribué en important des noeuds déployés sur d'autres clusters {{site.data.keyword.cloud_notm}} Private ou sur {{site.data.keyword.cloud_notm}}.

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private vous convient-il ?

L'exécution d'{{site.data.keyword.blockchainfull_notm}} Platform à l'extérieur de {{site.data.keyword.cloud_notm}} vous offre davantage de souplesse pour croître ou rejoindre un réseau de blockchain. Les créateurs de réseaux peuvent ainsi autoriser l'accès de nouveaux membres en utilisant la plateforme de leur choix. Les organisations qui souhaitent rejoindre des réseaux de blockchain peuvent ainsi regrouper leurs homologues avec leurs applications existantes ou s'intégrer à leurs systèmes d'enregistrement.

Les utilisateurs de cette offre gèrent leur propre sécurité et infrastructure. {{site.data.keyword.cloud_notm}} ne fournit pas ces services. Passez en revue les [Considerations et limitations](#console-icp-about-considerations) de la section suivante avant de poursuivre.

## Considérations et limitations
{: #console-icp-about-considerations}

Avant de commencer, tenez compte des limitations suivantes :

- Vous êtes chargé de gérer la surveillance, de la sécurité, de la journalisation et l'utilisation des ressources de vos composants de blockchain.
- La console peut uniquement être utilisée pour créer et gérer des composants basés sur Hyperledger Fabric versions 1.4.1 ou suivantes. 
- Vous pouvez importer uniquement des noeuds qui ont été exportés depuis d'autres consoles {{site.data.keyword.blockchainfull_notm}} Platform. Pour pouvoir exploiter un homologue ou un service de tri importé depuis la console, vous devez également importer la définition MSP de l'organisation du noeud associé et l'identité administrateur sur votre console.

Pour une vue d'ensemble des environnement pris en charge par {{site.data.keyword.cloud_notm}} Private, voir [Supported environments](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Configuration système prérequise
{: #console-icp-about-prerequisites}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private prend en charge les systèmes d'exploitation suivants :
- Red Hat Enterprise Linux (RHEL) 7.3, 7.4, 7.5
- Ubuntu 18.04 LTS et 16.04 LTS
- SUSE Linux Enterprise Server (SLES) 12 SP3

La charte Helm {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est validée pour une exécution sur des clusters {{site.data.keyword.cloud_notm}} Private version 3.2 sur Ubuntu Linux en utilisant les noeuds worker et le stockage de secours suivants :

- **LinuxONE et {{site.data.keyword.IBM_notm}} Z** : z/VM et KVM, qui utilisent NFS.
- **x86** : Linux 64 bits qui utilise GlusterFS.

## Licence
{: #console-icp-about-license}

Pour plus d'informations sur les licences et la tarification, voir [Tarification de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing).

## Installation d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est fourni en tant que fichier de charte Helm qui peut être installé dans un cluster {{site.data.keyword.cloud_notm}} Private local. Une fois que vous avez installé la charte Helm, vous pouvez trouver {{site.data.keyword.blockchainfull_notm}} Platform en tant qu'application dans le catalogue {{site.data.keyword.cloud_notm}} Private. Pour plus de détails sur l'installation de la charte Helm et des prérequis, voir [Installation de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#helm-console-install).

Si vous êtes un nouvel utilisateur de {{site.data.keyword.cloud_notm}} Private et souhaitez des informations et des conseils sur l'installation et le développement d'{{site.data.keyword.cloud_notm}} Private, voir [Configuration de {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup).

Vous pouvez déployer {{site.data.keyword.blockchainfull_notm}} Platform derrière un pare-feu sans avoir à accéder à l'Internet public. Le package de charte Helm téléchargé inclut l'ensemble des images Docker du composant Fabric utilisées par {{site.data.keyword.blockchainfull_notm}} Platform, sans leur extraction de DockerHub durant le déploiement.

## Remarques sur la sécurité
{: #console-icp-about-security}

Etant donné que les composants sont déployés dans votre propre infrastructure, vous êtes responsable de la gestion de leur sécurité. Cela inclut des zones de sécurité importantes, comme la gestion des clés et le chiffrement de données. Passez en revue les rubriques suivantes concernant la sécurité de vos composants.

### Sécurité des données
{: #console-icp-about-security-data}

Les plans Starter et Enterprise d'{{site.data.keyword.blockchainfull_notm}} Platform utilisent le chiffrement de disque intégral qui repose sur le [chiffrement de clé symétrique](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} pour protéger toutes les données utilisées par le réseau. Vous devez suivre des étapes similaires dans votre environnement pour protéger les données de votre homologue.

Les données de votre base de données d'état, que vous utilisiez LevelDB ou CouchDB, ne sont pas chiffrées. Vous pouvez utiliser le chiffrement au niveau de l'application afin de protéger les données au repos dans votre base de données d'état.

### Hébergement de données
{: #console-icp-about-security-data-residency}

L'hébergement de données indique que le traitement et le stockage de l'ensemble des données de registre de blockchain demeurent à l'intérieur des frontières d'un pays unique. Pour plus de détails sur la mise en oeuvre de l'hébergement de données, voir [Hébergement de données](#console-icp-about-data-residency).

### Gestion des clés
{: #console-icp-about-security-key-management}

La gestion des clés est un aspect essentiel de la sécurité. Si une clé privée est compromise ou égarée, des acteurs hostiles pourraient accéder aux données et aux fonctionnalités. {{site.data.keyword.IBM_notm}} utilise des dispositifs physiques appelés [modules de sécurité matérielle](/docs/services/blockchain/glossary.html#glossary-hsm) (HSM) pour stocker les clés privées des réseaux {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan.

Lorsque vous déployez un composant sur {{site.data.keyword.cloud_notm}} Private, vous êtes responsable de la gestion de vos clés privées. Bien qu'{{site.data.keyword.blockchainfull_notm}} Platform génère des clés privées, ces clés ne sont pas stockées sur cette plateforme. Il est essentiel que vous stockiez vos clés dans un emplacement sécurisé afin qu'elles ne soient pas compromises. Vous pouvez trouver la clé privée de votre homologue dans le dossier du magasin de clés de votre MSP, dans le répertoire `/mnt/crypto/peer/peer/msp/keystore/` au sein de votre composant. Pour plus d'informations sur les certificats au sein de votre homologue, voir la section [Membership Services Provider](/docs/services/blockchain/certificates.html#managing-certificates-msp) du tutoriel [Gestion des certificats sur {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/certificates.html#managing-certificates).

Vous pouvez utiliser Key Escrow pour récupérer des clés privées perdues. Cette opération doit être effectuée avant la perte de clés. Si une clé privée ne peut pas être récupérée, vous devez obtenir de nouvelles clés privées en enregistrant une nouvelle identité avec votre autorité de certification. Vous devez également retirer et remplacer votre signCert des canaux que vous avez rejoints.

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) est intégré au modèle de confiance d'Hyperledger Fabric. Tous les composants Starter et Enterprise sur {{site.data.keyword.blockchainfull_notm}} Platform utilisent TLS pour s'authentifier et communiquer entre eux. Par conséquent, les composants réseau sur Platform doivent pouvoir effectuer un établissement de liaison TLS avec vos homologues. Ceci implique entre autres que vous devez activer le passe-système, en utilisant des listes blanches par exemple, dans le pare-feu entre vos applications client et votre homologue.

### Configuration de Membership Service Provider (Fournisseur de services aux membres)
{: #console-icp-about-security-MSP}

Les composants d'{{site.data.keyword.blockchainfull_notm}} Platform consomment des identités via des Fournisseur de services aux membres (MSP). Les MSP associent les certificats émis par les autorités de certification à des rôles de réseau et de canal. Pour plus d'informations sur le fonctionnement des MSP avec l'homologue, voir [Membership Service Providers (MSPs)](/docs/services/blockchain/certificates.html#managing-certificates-msp).

### Sécurité des applications
{: #console-icp-about-security-appl}

Tous les appels de code blockchain étant signés, Fabric gère la sécurité des applications. De plus, Fabric inclut également des vérifications au niveau de l'application basées sur ACL.

## Hébergement de données
{: #console-icp-about-data-residency}

Etant donné que les réseaux de blockchain dépendent du type de données qui sont traitées, des étapes supplémentaires sont parfois nécessaires pour assurer la sécurité de certains types de données. L'exigence la plus courante sur l'hébergement de données est associée aux réglementations de certains pays, qui demandent que toutes les données qui sont traitées et stockées sur un système informatique doivent demeurer à l'intérieur des frontières d'un pays donné. De même, certaines entreprises dans des secteurs extrêmement régulés, comme le gouvernement, la santé et les services financiers, exigent que les données soient entièrement stockées derrière leur pare-feu. Par conséquent, pour appliquer l'hébergement de données, tous les composants du réseau de blockchain doivent faire partie du même [canal](/docs/services/blockchain/glossary.html#glossary-channel) et se trouver à l'intérieur des frontières d'un seul pays.

Pour respecter les exigences d'hébergement de données, il est important de comprendre l'architecture Hyperledger Fabric qui est sous-jacente à {{site.data.keyword.blockchainfull_notm}} Platform. L'architecture est centrée autour de trois composants clés : un service de tri (composé de tris), des autorités de certification et des homologues. Un homologue reçoit les mises à jour de l'état de tri sous la forme de blocs du service de tri et gère l'état et la registre. Par conséquent, un homologue et un service de tri ont une relation directe. Le registre contient les valeurs les plus récentes de toutes les clés et les données incluses dans les journaux de transaction.

En outre, les applications client utilisent les [logiciels SDK Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external} pour envoyer des transactions aux homologues et au service de tri. Ces transactions incluent les données [read-write set](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, qui contiennent les paires clé-valeur sur le registre. 

Si l'hébergement de données dans le pays est une exigence, le service de tri, les homologues et les applications client doivent résider dans le même pays. Lorsqu'un réseau {{site.data.keyword.blockchainfull_notm}} Platform est créé dans {{site.data.keyword.cloud_notm}}, vous avez la possibilité de choisir un emplacement pour le réseau. <!--For a Starter Plan network, you can select from US South, United Kingdom, and Sydney. For an Enterprise Plan network, you can select from currently available locations, which include Dallas, Frankfurt, London, Sao Paulo, Tokyo, and Toronto. -->Pour plus d'informations sur les régions et les emplacements, voir [Régions et emplacements d'{{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference/ibp_regions.html#ibp-regions-locations).

### Cas d'utilisation de l'hébergement des données dans un pays

Envisagez d'utiliser un réseau {{site.data.keyword.blockchainfull_notm}} Platform qui inclut le service de tri défini et une autorité de certification avec un consortium de quatre organisations. Les organisations ont une ou plusieurs noeuds homologues, et les quatre organisations font partie d'un canal unique et tous les composants de réseau résident dans la région (par exemple, Francfort) où le réseau {{site.data.keyword.blockchainfull_notm}} Platform a été déployé. Enfin, les applications client qui interagissent avec les homologues résident également en Allemagne.

Dans cet exemple, l'hébergement de données est géré.

![Hébergement de données lorsque tous les composants résident dans le même pays](images/remote_peer_data_res_1.png "Hébergement de données lorsque tous les composants résident dans le même pays")

Nous allons maintenant étudier les conséquences lorsqu'un **homologue distribué** rejoint l'une des organisations. Un homologue distribué peut résider dans la même région que le reste du réseau ou en dehors de la région du réseau de {{site.data.keyword.blockchainfull_notm}} Platform :

-	Si l'homologue réside dans le même pays que le reste du réseau, l'hébergement de données est géré. Toutes les données du registre demeurent en Allemagne comme illustré dans la **Figure 1** ci-dessus.
-	Si l'homologue réside dans un pays différent (par exemple, les Etats-Unis), l'hébergement de données n'est plus géré car les données sur le registre homologue sont partagées à l'extérieur des frontières.

Pour résoudre ce problème, des **canaux** peuvent être utilisés pour séparer les données dans un sous-ensemble d'homologues sur le réseau. Lorsque le réseau {{site.data.keyword.blockchainfull_notm}} Platform comporte un homologue distribué et des services de tri entres les frontières de pays, les canaux assurent l'isolement des données de registre des organisations avec des homologues à l'extérieur des frontières.

**Remarque :** Les services de tri sont toujours situés dans la région du centre de données que vous avez sélectionnée pour héberger le réseau. Il n'est pas possible d'avoir plusieurs services de tri entre les frontières. Les homologues peuvent cependant se situer dans le centre de données ou sur un site distant à l'extérieur de {{site.data.keyword.cloud_notm}}.

![Hébergement de données lorsque les homologues résident à l'extérieur du pays de la région {{site.data.keyword.blockchainfull_notm}} Platform](images/remote_peer_data_res_2.png "Hébergement de données lorsque les homologues résident à l'extérieur du pays de la région d'{{site.data.keyword.blockchainfull_notm}} Platform")

Dans la **Figure 2**, l'hébergement de données n'est pas obligatoire pour `OrgC` et `OrgD`. En fait, `OrgD` inclut désormais deux homologues distribués,`OrgD-peer1` et `OrgD-peer2`, qui résident aux *Etats-Unis*. Par conséquent, pour que `OrgA`, `OrgB` et leurs applications client et homologues respectifs qui résident en Allemagne puissent isoler les données de registre sur le canal `X`, un nouveau canal `Y` est créé pour `OrgC` et `OrgD`.

Pour une meilleure compréhension du flux de données sur le réseau {{site.data.keyword.blockchainfull_notm}} Platform, voir la [documentation Fabric sur le flux de transactions](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

A l'avenir, la nouvelle technologie dans Hyperledger Fabric améliorera la capacité à réaliser d'autres hébergements de données à l'étranger en utilisant des [collectes de données privées](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} et zéro connaissance.

- Une collecte de données privées garantit que les données privées sont partagés homologue par homologue (via le protocole gossip) sur des homologues autorisés uniquement pour voir si, par exemple, des homologues se trouvent entre les frontières d'un pays. Les données sont stockées dans une base de données privée sur l'homologue. Le service de tri n'est pas impliqué ici et il ne voit pas les données privées. Un hachage de ces données est écrit dans les registres de chaque homologue sur le canal. Le hachage qui est utilisé pour la validation d'état fait office de preuve de la transaction, et il peut être utilisé à des fins d'audit. Des données privées sont disponibles pour les réseaux sur {{site.data.keyword.blockchainfull_notm}} Platform qui s'exécutent dans Fabric version 1.2.1 ou ultérieure. Toutefois, la fonction Données privées n'est pas disponible pour les homologues s'exécutant dans {{site.data.keyword.cloud_notm}} Private, car les réseaux {{site.data.keyword.cloud_notm}} Private n'utilisent pas actuellement gossip.

- Une preuve ZKP (Zero-Knowledge Proof) permet à un “approbateur” de garantir à un “vérificateur” qu'ils ont la connaissance d'une valeur confidentielle sans révéler la valeur confidentielle elle-même.

Vous trouverez davantage d'informations sur ces technologies dans le livre blanc sur les [Transactions privées et confidentielles avec Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.

## Support
{: #console-icp-about-support}

Pour plus d'informations sur l'obtention d'un support sur {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, et pour accéder à des ressources de développeur de blockchain gratuites et à des forums de support pour la résolution de problèmes, voir [Support](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).

Si vous avez acheté une licence {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private et souhaitez contacter le support client, consultez  les informations relatives à l'[accès à {{site.data.keyword.IBM_notm}} Support Community et à l'ouverture d'un ticket de demande de service](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
