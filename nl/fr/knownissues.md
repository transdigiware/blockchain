---

copyright:
  years: 2017, 2019

lastupdated: "2019-06-18"

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

# Problèmes connus
{: #known-issues}

Cette page décrit les problèmes connus que vous pouvez rencontrer lors de l'utilisation des plans Starter ou Enterprise.
{:shortdesc}

Les problèmes suivants ont été signalés :
- **La configuration d'une autorité de certification externe n'est pas encore prise en charge**. Vous pouvez dans ce cas générer et envoyer par téléchargement des certificats admin via le moniteur réseau. Pour plus d'informations, voir la description sous l'[onglet "Certificats" de l'écran "Membre"](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) dans le Moniteur de réseau.
- Dans le Moniteur réseau d'un réseau du plan Starter, lorsque vous cliquez sur **Afficher les journaux** sur les noeuds répertoriés à l'écran "Présentation", l'interface kibana {{site.data.keyword.cloud}} Logging s'ouvre. **Par défaut, Kibana est préconfigurée pour afficher les journaux des 30 derniers jours d'activité**. Si aucune activité n'est intervenue dans les 30 derniers jours, un message indique *Aucun résultat trouvé*. Pour afficher tous les journaux, vous pouvez cliquer sur l'icône temporisateur dans l'angle supérieur droit sous votre nom d'utilisateur et définir un intervalle de temps plus large, comme *date année*.
- Les journaux de votre réseau du plan Starter sont collectés par le service [{{site.data.keyword.cloud_notm}} Log Analysis](https://cloud.ibm.com/catalog/services/log-analysis){: external}. Par défaut, les journaux sont collectés par le plan Lite du service Log Analysis. Ce plan est gratuit et **ne vous permet d'effectuer une recherche que sur les premiers 500 Mo de vos journaux par jour**. Si les journaux de votre réseau dépassent 500 Mo, vous ne pouvez pas visualiser de nouveaux journaux dans Kibana. Si votre réseau génère plus de 500 Mo de journaux, vous pouvez effectuer une mise à niveau vers une version payante du service Log Analysis.
- Etant donné que le plan Starter n'est pas destiné à un environnement de production, **il se peut que des applications ne puissent pas accéder immédiatement à une ressource réseau**.
  - Si ceci se produit, il est recommandé en premier lieu d'augmenter les valeurs de délai d'attente dans le SDK Fabric. Pour plus d'informations sur la définition des valeurs de délai, voir [Définition de valeurs de délai dans les kits SDK Fabric](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-set-timeout-in-sdk).
  - Vous pouvez également relancer votre demande au niveau de l'application.
- **Les conteneurs de Code blockchain peuvent parfois être arrêtés** par un problème réseau en arrière-plan et doivent éventuellement être régénérés et redémarrés après avoir été appelés par un utilisateur. Si ceci se produit, votre code blockchain peut demander quelques minutes pour répondre.
- En raison de la limitation des ressources dans le réseau du plan Starter, (à savoir 1 UC et 4 Go de RAM pour chaque homologue), **vous pourriez rencontrer une erreur `REQUEST_TIMEOUT` lors de l'instanciation du code blockchain**. Dans ce cas, relancez l'étape d'instanciation. Si l'erreur persiste, vous pouvez augmenter le délai d'attente de l'instanciation du code blockchain. Dans le profil de connexion, le délai d'attente d'instanciation du code blockchain est fixé à 300 secondes.
  - Si vous utilisez la valeur par défaut dans le SDK, copiez la section **client** du profil de connexion comme indiqué ci-dessous afin de définir le délai d'attente à 300 secondes et de vous assurer que votre SDK puisse le lire. Notez que pour Node SDK, ce paramètre de délai d'attente dans le profil de connexion affecte tous les appels, tels que `invoke` et `queries`.
    ```
    "client": {
       "organization": "Org1",
       "connection": {
           "timeout": {
               "peer": {
                   "endorser": "300"
               }
           }
       }
    },
    ```
    {:codeblock}
  - Si vous remplacez le délai d'attente des commandes d'instanciation du code blockchain dans vos SDK, rétablissez la valeur par défaut ou fixez-la à 300 secondes ou plus.
    - Si vous utilisez Node SDK, vous pouvez modifier le paramètre de délai d'attente de la méthode `sendInstantiateProposal(request, timeout)`. Pour plus d'informations, voir [sendInstantiateProposal(request, timeout)](https://fabric-sdk-node.github.io/Channel.html#sendInstantiateProposal){: external}.
    - Si vous utilisez le SDK Java, vous pouvez modifier le paramètre de délai d'attente de la commande `instantiateProposalRequest.setProposalWaitTime(DEPLOYWAITTIME)` en éditant le fichier `src/test/java/org/hyperledger/fabric/sdkintegration/End2endIT.java`.

Pour obtenir de l'aide sur votre réseau {{site.data.keyword.blockchainfull_notm}} Platform sur {{site.data.keyword.cloud_notm}}, consultez [Support](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
