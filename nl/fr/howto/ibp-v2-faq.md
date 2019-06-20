---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

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

# Foire aux questions
{: #ibp-v2-faq}

**Général**   

- [Quelle base de données les homologues utilisent-ils pour leur registre ?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [Quels langages sont pris en charge pour les contrats intelligents ?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Quel avantage présente l'utilisation d'{{site.data.keyword.blockchainfull_notm}} Platform sur Hyperledger Fabric natif ? ](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Quelle version d'Hyperledger Fabric est utilisée avec {{site.data.keyword.blockchainfull_notm}} Platform ? ](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Est-il possible d'utiliser des certificats d'autorités de certification non IBM ?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [Puis-je effectuer une mise à niveau de la version 1.0 vers la nouvelle version d'{{site.data.keyword.blockchainfull_notm}} Platform ? ](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [Que se passe-t-il si je supprime mon service {{site.data.keyword.blockchainfull_notm}} Platform ? ](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Quelles régions sont disponibles pour le service de blockchain s'exécutant sur {{site.data.keyword.cloud_notm}} ? ](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Puis-je utiliser mon cluster {{site.data.keyword.cloud_notm}} Kubernetes Service existant ?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Avons-nous accès aux services de consignation et quels journaux me sont accessibles ?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [Quels sont les avantages d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?](#ibp-v2-faq-icp-benefits)
- [Quels sont les environnements pris en charge par {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?](#ibp-v2-faq-icp-environments)
- [Quel est le modèle de tarification de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?](#ibp-v2-faq-icp-pricing)
- [Quels services dois-je installer pour pouvoir utiliser {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?](#ibp-v2-faq-icp-services)
- [Comment puis-je estimer la taille nécessaire d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud pour mes environnements de développement, de tests et de production ?](#ibp-v2-faq-icp-sizing)

## Quelle base de données les homologues utilisent-ils pour leur registre ?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Tous les homologues qui sont déployés avec {{site.data.keyword.blockchainfull_notm}} Platform utilisent CouchDB comme base de données pour le registre.

## Quels langages sont pris en charge pour les contrats intelligents ?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform prend en charge les contrats intelligents écrits en Go et Node.js. Le nouveau modèle de programmation d'Hyperledger Fabric ne prend actuellement en charge que Node.js, mais d'autres langages seront prochainement pris en charge. Si vous souhaitez conserver votre code de l'application existant, ou encore utiliser des logiciels SDK Fabric pour des langages autres que Node.js, vous pouvez encore vous connecter à votre réseau {{site.data.keyword.blockchainfull_notm}} Platform à l'aide d'API SDK Fabric de niveau inférieur.

## Quel avantage présente l'utilisation d'{{site.data.keyword.blockchainfull_notm}} Platform sur Hyperledger Fabric natif ?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

The {{site.data.keyword.blockchainfull_notm}} Platform permet au clients de déployer facilement un réseau de blockchain personnalisé. Vous pouvez utiliser une interface utilisateur de console intuitive pour déployer rapidement votre réseau, installer et and instancier facilement des contrats intelligents, puis surveiller vos transactions.

## Quelle version d'Hyperledger Fabric est utilisée avec {{site.data.keyword.blockchainfull_notm}} Platform ?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} et {{site.data.keyword.blockchainfull_notm}} Platform for Platform for Multicloud utilisent Hyperledger Fabric version 1.4.1


## Est-il possible d'utiliser des certificats d'autorités de certification non IBM ?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Oui, vous pouvez fournir vos propres certificats dès lors qu'ils sont émis par une autorité de certification compatible X.509.

## Puis-je effectuer une mise à niveau de la version 1.0 vers la nouvelle version d'{{site.data.keyword.blockchainfull_notm}} Platform ?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Ce n'est pas possible pour le plan Starter. Vous ne pouvez pas effectuer une mise à niveau du plan Starter vers la nouvelle version d'{{site.data.keyword.blockchainfull_notm}} Platform.
Pour le plan Enterprise, vous pourrez effectuer une mise à niveau vers la nouvelle version d'{{site.data.keyword.blockchainfull_notm}} Platform dans le futur. Vous recevrez un e-mail pour le propriétaire du compte de l'équipe de {{site.data.keyword.blockchainfull_notm}} Platform pour vous aider.

## Que se passe-t-il si je supprime mon service {{site.data.keyword.blockchainfull_notm}} Platform ?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Lorsque vous supprimez une instance de service {{site.data.keyword.blockchainfull_notm}} Platform, l'ensemble des autorités de certification, des homologues et des noeuds de tri de la blockchain sont supprimés ainsi que leur stockage associé.

## Quelles régions sont disponibles pour le service de blockchain s'exécutant sur {{site.data.keyword.cloud_notm}} ?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

Les régions disponibles pour {{site.data.keyword.blockchainfull_notm}} Platform sont répertoriées dans la section [Emplacements d'{{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations). Notez que vous devez créer un cluster {{site.data.keyword.cloud_notm}} Kubernetes Service dans la même région que le service de blockchain pour reconnaître le cluster. Des régions supplémentaires seront disponibles prochainement.

## Puis-je utiliser mon cluster {{site.data.keyword.cloud_notm}} Kubernetes Service existant ?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Votre cluster Kubernetes existant est compatible avec {{site.data.keyword.blockchainfull_notm}} Platform dès lors qu'il respecte les conditions suivantes :
- Il exécute la version 1.11 de Kubernetes ou une version supérieure très stable.
- Il y a suffisamment de ressources disponibles dans le cluster.

## Avons-nous accès aux services de consignation et quels journaux me sont accessibles ?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Avec {{site.data.keyword.blockchainfull_notm}} Platform, vous pouvez accéder directement aux journaux de l'homologue, de l'autorité de certification et du service de tri depuis votre tableau de bord Kubernetes. Nous vous recommandons d'utiliser le service {{site.data.keyword.cloud_notm}} LogDNA qui vous permet d'analyser facilement les journaux en temps réel.

## Quels sont les avantages d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?
{: #ibp-v2-faq-icp-benefits}
{: faq}

Pour plus détails, voir la rubrique [Contenu d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## Quels sont les environnements pris en charge par {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud prend en charge tous les environnements pris en charge par {{site.data.keyword.cloud_notm}} Private version 3.2. Pour plus d'informations, voir [Supported operating systems and platforms](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external}.

## Quel est le modèle de tarification d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?
{: #ibp-v2-faq-icp-pricing}
{: faq}

Les licences d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) sont basées sur le nombre d'UC (VPC) disponibles pour le produit. Pour plus de détails, [contactez IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Quels services dois-je installer pour pouvoir utiliser {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud ?
{: #ibp-v2-faq-icp-services}
{: faq}

Vous devez uniquement installer {{site.data.keyword.cloud_notm}} Private version 3.2.

## Comment puis-je estimer la taille nécessaire d'{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud pour mes environnements de développement, de tests et de production ?
{: #ibp-v2-faq-icp-sizing}
{: faq}

Dès que vous connaissez le nombre d'autorités de certification, d'homologues et de noeuds de tri nécessaires, vous pouvez consulter le [tableau des allocations de ressources par défaut](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) pour faire une estimation approximative des UC (VPC) requises pour votre réseau.
