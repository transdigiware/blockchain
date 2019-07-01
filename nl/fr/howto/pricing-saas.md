---

copyright:
  years: 2019
lastupdated: "2019-06-20"

keywords: pricing model, hourly, per hour, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost, billing

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:gif: data-image-type='gif'}
{:pre: .pre}

# Tarification d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}
{: #ibp-saas-pricing}

Ce guide vous aide à comprendre le modèle de tarification d'{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}}, et à estimer vos coûts de développement et d'expansion de votre réseau de blockchain d'homologue, noeuds de tri, et composants d'autorité de certification, qui reposent sur Hyperledger Fabric version 1.4.1. {:shortdesc}

_Ce modèle de tarification concerne uniquement {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. Si vous utilisez le plan Starter ou le plan Enterprise Plan et avez des questions relatives à la tarification, consultez la rubrique relative à la [tarification](/docs/services/blockchain?topic=blockchain-ibp-pricing) des plans Starter et Enterprise._

## Modèle de tarification
{: #ibp-saas-pricing-model}

{{site.data.keyword.blockchainfull_notm}} Platform introduit un nouveau modèle de tarification horaire qui repose sur l'utilisation du coeur de processeur virtuel (VPC). Ce modèle simplifié repose sur le volume d'UC (ou VPC) consommé par vos noeuds {{site.data.keyword.blockchainfull_notm}} Platform sur une base horaire, à un prix fixe de **0,29 dollars par heure VPC**.

VPC est une unité de mesure pour déterminer les coûts de licences des produits {{site.data.keyword.IBM_notm}}. Il est basé sur le nombre de coeurs virtuels (vCPU) dont le produit dispose. Une unité vCPU est un coeur virtuel qui est affecté à une machine virtuelle ou à un coeur de processeur physique. Pour une estimation des coûts d'{{site.data.keyword.blockchainfull_notm}} Platform, **1 VPC = 1 UC = 1 vCPU = 1 Coeur**.
{:note}

Pour une estimation de coût total, gardez à l'esprit que votre réseau de blockchain se compose d'un cluster {{site.data.keyword.cloud_notm}} Kubernetes qui contient des composants {{site.data.keyword.blockchainfull_notm}} Platform et utilise le stockage de votre choix. Des frais distincts supplémentaires sont imputés pour votre cluster {{site.data.keyword.cloud_notm}} Kubernetes et le stockage que vous choisissez. Aucun frais n'est imputé pour le cluster sur lequel s'exécute l'instance Operational Tooling, également appelée console. Pour une illustration, consultez la rubrique [Référence d'architecture](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview-architecture). Des détails sur le calcul des frais sont fournis ci-après.

### Etes-vous un développeur?
{: #ibp-saas-pricing-developer}

Les développeurs peuvent commencer avec notre [extension gratuite VS Code](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external}. Utilisez cet environnement de développement intégré pour écrire, tester, déboguer, et packager des contrats intelligents en local et pour le déploiement d'{{site.data.keyword.blockchainfull_notm}} Platform, ainsi que pour écrire des applications client. Commencez à partir de zéro ou accédez à des tutoriels et des exemples pour découvrir les concepts de base de la blockchain. Ensuite, revenez et liez une instance de service {{site.data.keyword.blockchainfull_notm}} Platform à votre cluster Kubernetes de manière à pouvoir générer votre réseau de blockchain à l'aide de la console.

## Avantages du nouveau modèle de tarification
{: #ibp-saas-pricing-benefits}

- **Pas de frais d'appartenance **: Libre de frais d'appartenance, vous pouvez investir directement dans vos composants de blockchain.
- **Clarté des estimations **: Un modèle de tarification horaire simple rend l'estimation des coûts claire et facile grâce à l'outil d'estimation de coûts qui est disponible sur le tableau de bord d'{{site.data.keyword.cloud_notm}}
- **Pas de coût minimum requis **: Ne payez que pour ce que vous utilisez, aucun package horaire VPC minimum n'est requis, ce qui permet de démarrer sans trop se ruiner.
- **Evolutivité du calcul **: Vous avez la possibilité de faire évoluer votre calcul lors des pics d'utilisation ou de le diminuer d'une fraction de minute de capacité lorsqu'aucun calcul n'est nécessaire pour réduire les dépenses.  

En résumé, ces fonctions éliminent la complexité de la comptabilité pour les limitations d'appartenance ou l'achat de calcul pour anticiper vos besoins.

## Principaux éléments de tarification
{: #ibp-saas-pricing-elements}

Comme votre réseau de blockchain se compose d'un cluster {{site.data.keyword.cloud_notm}} Kubernetes contenant des composants {{site.data.keyword.blockchainfull_notm}} Platform et utilise le stockage de votre choix, votre coût total inclut les éléments suivants :

- **{{site.data.keyword.blockchainfull_notm}} Platform** à un prix fixe de 0,29 dollars/VPC-heure. Ce prix représente les frais de vos composants de blockchain dans votre cluster Kubernetes.
- **{{site.data.keyword.cloud_notm}} Kubernetes Service** à tarification échelonnée du cluster qui est visible dans {{site.data.keyword.cloud_notm}} lorsque vous mettez à disposition votre cluster payant. Cela inclut les frais pour le calcul, c'est-à-dire l'UC et la mémoire. {{site.data.keyword.cloud_notm}} Kubernetes Services est tarifé sur un modèle échelonné qui dépend du nombre d'heures d'utilisation par mois. Par conséquent, lorsque vous examinez les plans de tarification, considérez qu'une utilisation 24h/24, 7j/7 équivaut à 720 heures par mois. Consultez le tableau dans la [dans la page de catalogue Kubernetes Service](https://cloud.ibm.com/kubernetes/catalog/cluster){: external} pour plus de détails sur la tarification de cluster.
- Choisissez le plan de **stockage** adapté à vos besoins. Consultez la rubrique [Compréhension des concepts de base du stockage Kubernetes](/docs/containers?topic=containers-kube_concepts#kube_concepts) pour en apprendre davantage sur vos options de classe de stockage et leur [coût](https://www.ibm.com/cloud/file-storage/pricing){: external}. Les noeuds {{site.data.keyword.blockchainfull_notm}} Platform utilisent la classe de stockage par défaut pour le cluster. Lorsque vous mettez à disposition un cluster Kubernetes dans {{site.data.keyword.cloud_notm}}, il est préconfiguré avec le plug-in de stockage permanent [Bronze level File storage](/docs/containers?topic=containers-file_storage#file_predefined_storageclass).

## Exemples de tarification
{: #ibp-saas-pricing-scenarios}

Le tableau suivant fournit deux exemples de tarification avec des [allocations de ressource par défaut]( #ibp-saas-pricing-default), sauf indication contraire.
- Le scénario **Réseau de test** convient pour commencer et tester des contrats intelligents.
- Le scénario **Rejoindre un réseau de production** comporte deux homologues, qui sont recommandés pour une haute disponibilité, et un autorité de certification qui est obligatoire pour l'appartenance de l'organisation.
   - Ces homologues peuvent rejoindre un réseau {{site.data.keyword.blockchainfull_notm}} Platform de production qui est hébergé ailleurs.
   - Les noeuds peuvent être ramenés à un état d'utilisation minimal (0,001 UC) lorsqu'ils ne sont plus utilisés afin de [diminuer les coûts](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).
   - Comme ce scénario est destiné à un environnement de **production** :
     - Les ressources de calcul par défaut sont doublées pour offrir une plus grande capacité.
     - La classe de stockage [Silver](/docs/containers?topic=containers-file_storage#file_silver){: external} est choisie pour de meilleures performances.

| Options de tarification** (1 VPC = 1 UC)| **Réseau de test** | **Rejoindre un réseau de production** |
|-|------------|-----------------------------|
| **Allocation d'UC** |  1,85 UC <br> Inclut : <br> - 1 homologue <br> - 2 AC <br> - 1 noeud de tri| 4,9 UC <br> Inclut : <br> - 2 homologues (pour la haute disponibilité) <br> **(calcul par défaut x 2)** <br>- 1 AC <br>  |
| **Coût horaire : {{site.data.keyword.blockchainfull_notm}} Platform** | 0,54 dollars <br> (1,85 UC x 0,29 dollar/VPC-hr) | 1,42 dollar <br> (4,9 UC x 0,29 dollar/VPC-hr ) |
| **Coût horaire : cluster {{site.data.keyword.cloud_notm}} Kubernetes **    | 0,12 dollar <br> (Calcul : échelon 2 x 4) <br> (Allocation IP : 16 dollars/mois) | 0,46 dollar <br> (Calcul : échelon 8 x 32) <br> (Allocation IP : 16 dollars/mois) |
| **Coût horaire : Stockage** | 0,07 dollar <br> 340 Go  <br> [Bronze](https://www.ibm.com/cloud/file-storage/pricing){: external} <br>  2 IOPS/Go | 0,13 dollar <br> 420 Go <br> [Silver](https://www.ibm.com/cloud/file-storage/pricing){: external} <br> 4 IOPS/Go  |
| **Coût horaire total ** | **0,73 dollar** | **2,01 dollars**| |
Essai de {{site.data.keyword.blockchainfull_notm}} Platform sans frais pendant 30 jours lorsque vous liez votre instance de service {{site.data.keyword.blockchainfull_notm}} Platform à un cluster Kubernetes {{site.data.keyword.cloud_notm}} gratuit. Les performances seront limitées en termes de débit, de stockage et de fonctionnalités. {{site.data.keyword.cloud_notm}} supprimera votre cluster Kubernetes au bout de 30 jours et vous ne pouvez pas migrer les noeuds ou les données depuis un cluster gratuit vers un cluster payant.  

Vos coûts réels varieront en fonction de facteurs supplémentaires tels que le débit de transaction, le nombre de canaux dont vous avez besoin, la taille de la charge sur les transactions, ainsi que le nombre maximum de transactions simultanées. Les exemples de tarification ci-dessus reposent sur un cluster {{site.data.keyword.cloud_notm}} Kubernetes à zone unique. Si vous choisissez un cluster multizone, des frais supplémentaires sont appliqués pour les zones supplémentaires et l'équilibrage de charge multizone requis.
{:note}

Il n'y a pas de limite au nombre d'instances de service que vous pouvez mettre à disposition et associer à un cluster Kubernetes unique, mais vous devez vous assurer que les ressources adéquates sont disponibles par la surveillance de l'utilisation de l'UC, de la mémoire et du stockage afin d'éviter toute interruption du service. Les noeuds {{site.data.keyword.blockchainfull_notm}} Platform n'ont pas à être dans leur propre cluster. D'autres services {{site.data.keyword.cloud_notm}} peuvent s'exécuter dans votre cluster où s'exécutent vos composants de blockchain, mais encore une fois, vous devez vous assurer de disposer du calcul et du stockage adéquats pour répondre aux besoins de toutes les instances de service.

## Allocations de ressources par défaut
{: #ibp-saas-pricing-default}

Les valeurs du tableau suivant sont utiles pour estimer le coût horaire de votre réseau personnalisé sur la base de l'UC, du calcul et du stockage. Ces valeurs minimales recommandées sont suffisantes pour démarrer. Dès lors que vous commencerez à surveiller votre utilisation réseau, vous trouverez peut-être que vos besoins réels en termes de ressources et vos coûts vont varier en fonction de votre cas d'utilisation et de vos exigences en termes de sécurité et disponibilité.  

| **Composant** (tous les conteneurs) | UC  | Mémoire (Go) | Stockage (Go) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Homologue**                       | 1,2            | 2,4                   | 200 (inclut 100 Go pour l'homologue et 100 Go pour CouchDB)|
| **AC**                         | 0,1            | 0,2                    | 20                     |
| **Noeud de tri**                    | 0,45           | 0,9                    | 100                    |


## Facturation
{: #ibp-saas-pricing-billing}

Vos informations relatives à la facturation et à l'utilisation de votre compte **Facturation à l'utilisation** sont disponibles sur le tableau de bord {{site.data.keyword.cloud_notm}} dans votre vignette [Utilisation](https://cloud.ibm.com/billing/usage). Un service de décompte prend des instantanés horaires de votre utilisation VPC totale d'{{site.data.keyword.blockchainfull_notm}} Platform de sorte que l'utilisation mensuelle cumulée est indiquée dans la vignette **Utilisation**.

Lorsque vous créez un noeud, une heure peut être nécessaire pour l'utilisation VPC soit reflétée dans la vignette **Utilisation** du tableau de bord {{site.data.keyword.cloud_notm}}.
{:note}

### Surveillance de votre utilisation
{: #ibp-saas-pricing-usage}

Avant de recevoir une facture, vous pouvez surveiller les coûts d'{{site.data.keyword.blockchainfull_notm}} Platform et de votre cluster Kubernetes depuis la vignette **Utilisation** du tableau de bord d'{{site.data.keyword.cloud_notm}}. L'utilisation VPC d'{{site.data.keyword.blockchainfull_notm}} Platform est évaluée toutes les heures.  **Ces coûts ne sont que des estimations.** Les coûts réels sont reflétés sur votre facture mensuelle.

#### Utilisation d'{{site.data.keyword.blockchainfull_notm}} Platform et de Kubernetes Service

Ce clip fournit un exemple simple de l'affichage de vos frais pour une instance d'{{site.data.keyword.blockchainfull_notm}} Platform comportant un seul noeud AC.

![Surveillance de votre utilisation](../images/usage_monitoring.gif){: gif}

Accédez à la section **Gérer** dans la partie supérieure de votre tableau de bord {{site.data.keyword.cloud_notm}}, cliquez sur **Facturation et utilisation**, puis cliquez sur **Utilisation** dans le menu de gauche. Le graphique circulaire sous la sous-section **Services** fournit une ventilation de votre coût total par types d'offres de service que vous avez utilisées et consommées ce mois-ci. Utilisez ce graphique afin de comprendre la part d'{{site.data.keyword.blockchainfull_notm}} Platform, de Kubernetes Services et de votre stockage dans votre coût total.

Si vous faites défiler vers le bas, vous pouvez constater une ventilation similaire par **Type** et **Coût** dans une vue de liste. Vous pouvez voir dans la liste "Kubernetes Service" et "Blockchain Platform" ainsi que d'autres services que vous avez mis à disposition dans votre cluster. Cliquez sur **Afficher les plans** en regard de chacun de ces éléments afin d'en savoir plus sur la ventilation des coûts par mesure. Par exemple, `VIRTUAL_PROCESSOR_CORE_HOURS` représente le nombre total d'heures VPC qui ont été évaluées jusqu'à maintenant. Utilisez cette valeur pour voir les frais qui vont vous être imputés sur la base de la mesure de tarification **0,29 dollars/VPC-heure**.

#### Frais d'affectation IP

Lorsque vous mettez à disposition un cluster Kubernetes dans {{site.data.keyword.cloud_notm}}, des frais mensuels fixes sont évalués pour l'allocation IP. Ces frais sont imputés par zone, de sorte que si vous mettez à disposition trois zones dans votre cluster, vous pouvez multiplier ces frais par trois. L'exemple ci-dessous présente les frais pour une zone unique.

![frais d'allocation IP](../images/ip_allocation_charge.png "Frais d'allocation IP d'un cluster Kubernetes")

Ces frais sont visibles sous l'onglet **Invoices** de la vignette Utilisation. Cliquez sur le lien sous **Facture périodique suivante** pour voir vos frais d'allocation IP.

#### Utilisation du stockage

Si vous utilisez le stockage de fichiers {{site.data.keyword.cloud_notm}}, les coûts sont évalués mensuellement, de sorte qu'une estimation des coûts de stockage n'est visible qu'en fin de mois. Toutefois, le stockage que vous avez mis à disposition tout au long du moins est listé sur des lignes d'élément dans la vignette Utilisation sous **Ventes** > **Commandes**. Recherchez dans la colonne **Eléments** une description du stockage qui a été mis à disposition de manière dynamique lors du déploiement d'un homologue, d'une autorité de certification ou d'un noeud de tri.

## Optimisation du coût de vos noeuds
{: #ibp-saas-pricing-shutdown}

L'un des principaux avantage du modèle de tarification {{site.data.keyword.blockchainfull_notm}} Platform est la possibilité de revenir en arrière ou de supprimer des ressources lorsqu'elles ne sont plus nécessaires.

- **Basculement de vos noeuds à l'état d'utilisation minimum**  
  Il est possible de réduire l'UC sur les noeuds on individuels à 0,001 UC afin de réduire complètement les frais. Dans ce cas, le noeud n'est plus opérationnel. Si un calcul est nécessaire ultérieurement, vous pouvez utiliser l'option de réallocation sur la console {{site.data.keyword.blockchainfull_notm}} Platform pour étendre les ressources si nécessaire. Pour plus d'informations sur la réallocation des ressources, voir [Réallocation des ressources](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).

- **Suppression d'un homologue non utilisé et déploiement d'un nouveau si nécessaire**  
  Le registre étant stocké dans le noeud de tri, lorsque vous déployez un nouvel homologue et rejoignez un canal, cet homologue reçoit une copie du registre distribué. L'inconvénient de cette approche est que vous devez générer de nouveaux certificats et joindre de nouveau l'homologue aux cana canaux.

  Il n'est pas conseillé de supprimer un noeud d'autorité de certification car les données qui s'y trouvent ne peuvent pas être récupérées. De même, si vous ne disposez que d'un seul noeud de tri, vous ne devez jamais le supprimer.  
  {:important}

- **Surveillance et ajustement de votre allocation de ressources en fonction de vos besoins**  
  Lorsque vous surveillez votre utilisation des ressources au fil du temps, vous pouvez décider de réduire des ressources qui sont allouées à un noeud tout en garantissant des performances suffisantes. Lorsque vous suivez les instructions de [réallocation des ressources](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources) sur la console, les effets sur le VPC total du noeud sont mis à jour et peuvent être utilisés pour estimer des coûts mensuels révisés.
