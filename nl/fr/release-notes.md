---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# Notes sur l'édition
{: #release-notes-saas-20}

Ces notes sur l'édition sont classées par date et vous informent des dernières modifications apportées à {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} qui repose sur Hyperledger Fabric version 1.4.1.
{:shortdesc}

## 10 Juillet 2019
{: #07-10-2019}

**Correctif du noeud de tri `1.4.1-2`**  

Ce correctif résout un problème qui se produit lors du redémarrage des noeuds de tri. Le bloc d'origine est mis à jour, ce qui empêche le noeud de tri de communiquer avec les autres noeuds du service de tri. Vous pouvez appliquer ce correctif à tous les noeuds de tri existants dans vos services de tri en suivant ces [instructions](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch) pour mettre à jour chaque noeud de tri. Tous les nouveaux noeuds de tri que vous créez incluent automatiquement ce correctif.

**Retrait d'un homologue d'ancrage**  

Vous avez maintenant la possibilité de retirer des homologues d'ancrage d'un canal.

**Divers correctifs de bogues **  

Correctifs et mises à jour de traduction.

## 24 Mai 2019
{: #05-24-2019}

**Protocole de consensus Raft**  

 Le service de tri Raft à cinq noeuds, recommandé pour les réseaux de production, est désormais disponible. De plus, aux fins de développement et de tests, vous pouvez déployer un service de tri Raft à un seul noeud.

## 9 Mai 2019
{: #05-09-2019}

**Présentation des API de la console {{site.data.keyword.blockchainfull_notm}} Platform**

Des API sont désormais disponibles pour mettre à disposition, modifier et supprimer des noeuds d'homologue, de service de tri et d'autorité de certification, ce qui permet de scripter la génération de votre réseau de blockchain. Utilisez la documentation de la section [{{site.data.keyword.cloud_notm}} API documentation repository](/apidocs/blockchain#introduction){: external} pour en savoir plus sur les API et les tester. Vous pouvez également consulter la rubrique [Génération d'un réseau avec des API](/docs/services/blockchain?topic=blockchain-ibp-v2-apis) pour plus de détails sur l'utilisation des API pour générer votre réseau.  

**Gouvernance de canal**  

Les mises à jour de gouvernance de canal permettent de reconfigurer des règles. Vous pouvez également déterminer les membres de canal qui doivent signer une mise à jour de canal et vous pouvez organiser la collecte de signature.

## 17 avril 2019
{: #04-17-2019}

**Possibilité de dimensionner et de mettre à l'échelle les ressources de noeud**  

Lorsque vous déployez un noeud, vous avez désormais la possibilité d'indiquer la quantité d'UC, de mémoire et de stockage pour vos conteneurs, le cas échéant. Vous pouvez ensuite augmenter ou diminuer leurs ressources ultérieurement en fonction des modèles d'utilisation. Pour plus de détails, voir [Allocation de ressources](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources).

**Utilisation d'{{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

IAM permet de contrôler l'accès utilisateur à la console mais également de limiter les actions possibles dans cette interface.  Pour plus d'informations, consultez la rubrique relative à l'[ajout et au retrait d'utilisateurs de la console](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).

## 3 avril 2019
{: #04-03-2019}

**Prise en charge d'une autorité de certification externe**

Lorsque vous ajoutez un noeud homologue ou service de tri, vous avez la possibilité d'utiliser des certificats d'une autorité de certification externe, qui n'est pas hébergée par {{site.data.keyword.IBM_notm}}. Pour plus d'informations, consultez la rubrique relative à l'[utilisation de certificats d'une autorité de certification externe auprès de votre homologue ou service de tri](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca).

**Optimisation des performances du service de tri**

De nouveaux paramètres d'optimisation des paramètres sont disponibles sur la console pour vous donner plus de contrôle sur le débit et les performances de votre service de tri. Pour plus de détails sur la configuration de ces paramètres, consultez la rubrique [Optimisation de votre service de tri](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning).
