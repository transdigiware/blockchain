---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Initiation à {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform fournit une offre de blockchain en tant que service (BaaS) de pile gérée et complète qui vous permet de déployer des composants de blockchain dans les environnements de votre choix. L'environnement peut être {{site.data.keyword.cloud_notm}}, sur site via {{site.data.keyword.cloud_notm}} Private, et des clouds tiers, comme Amazon Web Services (AWS). Dans ce tutoriel, nous vous guiderons tout au long du processus général de configuration d'un réseau de blockchain de base avec {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

Avant d'utiliser une offre d'{{site.data.keyword.blockchainfull_notm}} Platform, lisez les informations relatives aux données techniques et à l'aide dans la section [Clause de protection](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer).
{: important}


## Avant de commencer
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform propose différentes offres pour aider tous les types d'utilisateurs à démarrer leurs parcours sur la blockchain et transférer leurs applications en production. Vous devez décider de votre environnement et de l'offre que vous allez utiliser. La Figure 1 présente les fonctions, le public cible, la tarification et les plateformes de cloud pour les différentes offres. Pour plus d'informations sur chaque offre, cliquez sur son nom dans le tableau suivant.

| **Offres** | **Eléments inclus** | **Règle facturation** | **Plateforme cloud** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [Extension **{{site.data.keyword.blockchainfull_notm}} Platform pour VS**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | Les développeurs peuvent démarrer avec l'environnement de développement intégré qui fournit un explorateur et des commandes accessibles depuis la palette de commandes pour le développement rapide de contrats intelligents. | Gratuit | S'exécute sur votre machine locale |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) | Console {{site.data.keyword.blockchainfull_notm}} Platform et API qui peuvent être utilisées pour déployer et pour déployer et gérer des composants de blockchain dans votre cluster {{site.data.keyword.cloud_notm}} Kubernetes. | [Tarification VPC 0,29 dollars/VPC-heure](/docs/services/blockchain/howto?topic=blockchain-ibp-saas-pricing) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about) | Console {{site.data.keyword.blockchainfull_notm}} Platform déployée dans un cluster {{site.data.keyword.cloud_notm}} Private à l'aide d'une charte Helm Kubernetes et d'API pour la mise à disposition et la gestion de composants de blockchain | [Tarification VPC](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws-about#remote-peer-aws-about) | Modèle de démarrage rapide AWS pour le déploiement d'homologues distants situés à l'extérieur d'{{site.data.keyword.cloud_notm}}. | Gratuit | AWS |

*Figure 1. Offres {{site.data.keyword.blockchainfull_notm}} Platform*


## Etape 1 : Obtenir une offre
{: #get-started-ibp-step1}

Vérifiez que vous disposez de la licence de compte cloud ou PPA pour obtenir une offre {{site.data.keyword.blockchainfull_notm}} Platform.

* **Extension de code {{site.data.keyword.blockchainfull_notm}} Platform pour VS**

  Cette extension VS Code est disponible gratuitement dans l'extension [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} et elle peut être utilisée pour développer, déboguer et tester des contrats intelligents en vue d'un possible déploiement dans {{site.data.keyword.blockchainfull_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Cette offre est disponible dans le tableau de bord du catalogue [{{site.data.keyword.cloud_notm}} ](https://cloud.ibm.com/catalog){: external} de {{site.data.keyword.cloud_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Cette offre est fournie en tant que charte Helm déployable via [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Cette offre est disponible dans AWS et vous pouvez déployer un homologue de blockchain dans AWS à l'aide d'un [modèle de démarrage rapide](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Etape 2 : Déployer {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Connectez-vous à {{site.data.keyword.cloud_notm}} et créez une instance de service avec l'offre. Suivez les instructions de l'assistant pour terminer la configuration initiale de votre réseau. Pour plus d'informations, voir [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Avant de déployer un réseau, vous devez installer la charte Helm dans un cluster {{site.data.keyword.cloud_notm}} Private. Ensuite, vous pouvez déployer la console {{site.data.keyword.blockchainfull_notm}} Platform et l'utiliser pour déployer et gérer des composants de blockchain dans votre cluster local. Pour plus d'informations, voir [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Cette offre est disponible dans AWS et vous pouvez déployer un homologue de blockchain dans AWS à l'aide d'un [modèle de démarrage rapide](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Etapes suivantes
{: #get-started-ibp-next-steps}

Une fois {{site.data.keyword.blockchainfull_notm}} Platform déployé dans l'environnement de votre choix, vous pouvez configurer le réseau avec un consortium, des noeuds, des canaux, des contrats intelligents et y démarrer des transactions. Pour plus d'informations, consultez les rubriques sous la section **Procédures détaillées** dans la zone de navigation gauche de la présente documentation.

## Support
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} offre différente options de support sur des solutions de blockchain implémentées par {{site.data.keyword.IBM_notm}}. Pour plus d'informations sur le support {{site.data.keyword.blockchainfull_notm}} Platform, voir [Support](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
