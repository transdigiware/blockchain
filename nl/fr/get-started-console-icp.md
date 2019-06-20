---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Initiation à {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform peut être déployé au sein de clouds publics et privés, tels que {{site.data.keyword.cloud_notm}}, votre propre centre de données, ainsi que des clouds publics de tiers. Ce déploiement multicloud est possible via {{site.data.keyword.cloud_notm}} Private, plateforme d'orchestration de conteneurs basée sur Kubernetes d'{{site.data.keyword.IBM_notm}}. Vous pouvez utiliser cette offre pour installer la console {{site.data.keyword.blockchainfull_notm}} Platform dans un déploiement d'{{site.data.keyword.cloud_notm}} Private, puis utiliser cette console pour créer des composants {{site.data.keyword.blockchainfull_notm}} dans votre cluster local. Vous pouvez également utiliser votre console pour exploiter un réseau multicloud distribué en important des noeuds déployés sur d'autres clusters {{site.data.keyword.cloud_notm}} Private ou sur {{site.data.keyword.cloud_notm}}.{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est un produit regroupé pour {{site.data.keyword.cloud_notm}} Private. Pour plus d'informations sur {{site.data.keyword.cloud_notm}} Private, consultez la documentation relative à [{{site.data.keyword.cloud_notm}} Private version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Etape 1 : Configurer un cluster Kubernetes sur {{site.data.keyword.cloud_notm}} Private
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

La première étape consiste à définir un cluster Kubernetes dans {{site.data.keyword.cloud_notm}} Private sur la plateforme cloud de votre choix.
Pour obtenir des recommandations et des conseils, vous pouvez consulter la section [Configuration d'{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_setup.html#icp-setup).

Si vous générez un réseau qui va être utilisé dans un environnement de production, vous devez configurer votre déploiement en cluster et votre réseau de blockchain pour la haute disponibilité :

- Pour obtenir des recommandations sur la configuration de votre cluster, consultez la rubrique relative à l'[implémentation de la haute disponibilité dans {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- Lorsque vous êtes prêt(e) à commencer à générer votre réseau, consultez les [recommandations relatives à la haute disponibilité pour les homologues](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-peers) et les [recommandations relatives à la haute disponibilité pour les services de tri](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-ordering-service).

## Etape 2 : Installer {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private est fourni en tant que charte Helm qui peut être téléchargée depuis Passport Advantage (PPA). Pour en savoir plus sur l'installation de la charte Helm dans votre cluster local, voir [Installation d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install).

## Etape 3 : Déployer la console {{site.data.keyword.blockchainfull_notm}} Platform 
{: #get-started-console-icp-step-three-deploy-console}

Une fois que vous avez installé la charte Helm, vous pouvez cliquer sur la vignette de l'application {{site.data.keyword.blockchainfull_notm}} Platform sur la page Catalogue afin d'installer une console {{site.data.keyword.blockchainfull_notm}} Platform dans votre cluster local. Pour en savoir plus sur la configuration de la console, ainsi que sur les ressources qui sont nécessaires à vos composants de blockchain, voir [Déploiement de la console {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).

## Etape 4 : Ajouter des utilisateurs à la console en tant qu'administrateur
{: #get-started-console-icp-step-four-add-console-admin}

L'administrateur de console peut se connecter à la console en utilisant l'adresse e-mail et le mot de passe qui ont été fournis durant le déploiement. Le mot de passe fourni devient le mot de passe par défaut de la console, et il est utilisé par tous les nouveaux utilisateurs qui se connectent pour la première fois à la console. L'administrateur peut ensuite ajouter de nouveaux utilisateurs à la console, autoriser d'autres utilisateurs à se connecter et commencer à gérer des noeuds {{site.data.keyword.blockchainfull_notm}}. L'administrateur peut également définir un nouveau mot de passe par défaut. Pour plus d'informations, voir [Gestion des utilisateurs depuis la console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Etape 5 : Utiliser la console pour créer vos composants
{: #get-started-console-icp-build-network}

Une fois que vous avez déployé la console, vous pouvez l'utiliser pour créer, exploiter et régir des composants {{site.data.keyword.blockchainfull_notm}} dans votre cluster local. Pour commencer à utiliser l'interface utilisateur de la console, consultez le [tutoriel Générer un réseau](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network).

## Etape 6 : Se connecter à des réseaux sur des clouds
{: #get-started-console-icp-import-nodes}

Vous pouvez utiliser la console pour exploiter des composants qui s'exécutent dans d'autres clusters {{site.data.keyword.cloud_notm}} Private ou dans {{site.data.keyword.cloud_notm}}. Tout d'abord, vous devez exporter les informations de composant dans un fichier JSON depuis la console où le composant a été initialement déployé. Ensuite, vous pouvez importer le fichier JSON du noeud dans la console qui est déployée dans votre cluster local et gérer les composants entre les clouds. Pour plus d'informations, voir [Importation de noeuds](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).
