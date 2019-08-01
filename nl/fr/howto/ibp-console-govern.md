---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: network components, IBM Cloud Kubernetes Service, allocate resources, batch timeout, channel update, reallocate resources

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

# Gouvernance des composants
{: #ibp-console-govern}

Après avoir créé des autorités de certification, des homologues, des noeuds de tri, des organisations et des canaux, vous pouvez utiliser la console pour mettre à jour ces composants.
{:shortdesc}

Si vous utilisez la version d'essai bêta d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, il est probable que certains panneaux sur votre console ne correspondront pas à ceux de la documentation actuelle, qui est mise à jour avec l'instance de service disponible globalement. Pour bénéficier des toutes dernières fonctionnalités, nous vous encourageons à ce stade à mettre en service une nouvelle instance de service disponible globalement en suivant les instructions de la section [Initiation à {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

**Public cible :** Cette rubrique s'adresse aux opérateurs réseau qui sont responsables de la création, de la surveillance et de la gestion du réseau de blockchain.

## Comment la console interagit avec votre cluster Kubernetes
{: #ibp-console-govern-iks-console-interaction}

Il incombe à l'opérateur réseau de surveiller l'utilisation de l'UC, de la mémoire et du stockage et de s'assurer que les ressources adéquates sont disponibles **avant** d'essayer de créer ou de redimensionner un noeud.
{:important}

Comme votre instance de la console {{site.data.keyword.blockchainfull_notm}} Platform et votre cluster Kubernetes ne communiquent pas directement au sujet des ressources qui sont disponibles, le processus de déploiement ou de redimensionnement des composants à l'aide de la console doit respecter le schéma suivant :

1. **Dimensionnez le déploiement que vous voulez effectuer**. Les panneaux **Allocation de ressources** pour l'autorité de certification, l'homologue et le noeud de tri sur la console proposent une allocation par défaut de l'UC, de la mémoire et du stockage pour chaque noeud. Vous devrez peut-être ajuster ces valeurs en fonction de votre cas d'utilisation. Si vous avez des doutes, commencez par les allocations par défaut et ajustez-les dès que vous avez connaissance de vos besoins. De même, le panneau **Allocation de ressources** affiche les allocations de ressource existantes.

  Pour vous faire une idée de la quantité de stockage et de calcul dont vous aurez besoin dans votre cluster, consultez le tableau à la suite de cette liste, qui contient les valeurs par défaut actuelles pour l'homologue, le service de tri et l'autorité de certification.

2. **Vérifiez si vous disposez de suffisamment de ressources dans votre cluster Kubernetes**. Si vous utilisez un cluster Kubernetes hébergé dans {{site.data.keyword.cloud_notm}}, nous recommandons l'utilisation de l'outil [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} en association avec votre tableau de bord {{site.data.keyword.cloud_notm}} Kubernetes. Si vous n'avez pas assez d'espace dans votre cluster pour déployer ou redimensionner les ressources, vous devez augmenter la taille de votre cluster {{site.data.keyword.cloud_notm}} Kubernetes Service. Pour plus d'informations sur l'augmentation de la taille d'un cluster, voir [Mise à l'échelle de clusters](/docs/containers?topic=containers-ca#ca){: external}. Si vous utilisez un fournisseur de cloud autre que {{site.data.keyword.cloud_notm}}, vous devrez consulter sa documentation pour en savoir plus sur la mise à l'échelle des clusters. Si vous disposez de suffisamment d'espace dans le cluster, vous pouvez passer à l'étape 3.
3. **Utilisez la console pour déployer ou redimensionner votre noeud**. Si votre pod Kubernetes pod est suffisamment grand pour s'adapter à la nouvelle taille de noeud, la réallocation doit s'effectuer en douceur. Si le noeud worker sur lequel s'exécute le pod n'a plus de ressources, vous pouvez ajouter un nouveau noeud worker plus grand à votre cluster et supprimer le noeud worker existant.

| **Composant** (tous les conteneurs) | UC**  | Mémoire (Go) | Stockage (Go) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Homologue**                       | 1,1           | 2,4                   | 200 (inclut 100 Go pour l'homologue et 100 Go pour CouchDB)|
| **AC**                         | 0,1           | 0,2                   | 20                     |
| **Noeud de tri**              | 0,35          | 0,9                   | 100                    |
** Ces valeurs peuvent légèrement varier si vous utilisez {{site.data.keyword.cloud_notm}} Private. Les allocations VPC réelles sont visibles sur la console de blockchain lorsqu'un noeud est déployé.  

Si vous envisagez de déployer un service de tri Raft à cinq noeuds, notez que votre déploiement total va s'accroître d'un facteur de cinq. Soit un total de 1,75 UC, 4,5 Go de mémoire et 500 Go de stockage pour les cinq noeuds Raft. Un cluster de noeud worker unique Kubernetes à 4 UC est recommandé au minimum pour autoriser un grand nombre d'UC pour le cluster Raft cluster et les autres noeuds que vous déployez.
{:tip}

Dans les cas où un utilisateur souhaite minimiser les frais sans arrêter complètement un noeud ou sans le supprimer, il est possible de réduire un noeud à un minimum de 0,001 UC (1 milliCPU). Notez que le noeud ne sera pas opérationnel si cette quantité d'UC est utilisée.

Même si les chiffre de cette rubrique s'efforcent d'être précis, sachez qu'il peut arriver qu'un noeud ne se déploie pas si vous avez suffisamment d'espace dans votre cluster. Veillez à consulter votre tableau de bord Kubernetes pour voir dans quels cas les composants se déploient et les messages d'erreur qui s'affichent dans le cas contraire. Dans le cas où un composant ne se déploie pas par manque de ressources, même s'il semble y avoir suffisamment d'espace dans le cluster, vous devrez vraisemblablement déployer des ressources de cluster supplémentaires pour que le composant se déploie.
{:important}

## Allocation de ressources
{: #ibp-console-govern-allocate-resources}

Les utilisateurs d'un cluster gratuit **doivent utiliser les tailles par défaut** pour les conteneurs associés à leurs noeuds, tandis que les utilisateurs de clusters payants peuvent définir ces valeurs pendant la création de leurs noeuds.

Le panneau **Allocation de ressources** sur la console fournit les valeurs par défaut pour les différentes zones impliquées dans la création d'un noeud. Ces valeurs sont choisies car elles représentent un bon moyen de démarrer. Toutefois, chaque cas d'utilisation est différent. Bien que cette rubrique fournisse des conseils sur la manière de définir ces valeurs, c'est à l'utilisateur au final de surveiller ses noeuds et de déterminer les dimensions qui leurs conviennent. Par conséquent, dans le cas où des utilisateurs sont certains qu'ils auront besoin de valeurs différent des valeurs par défaut, une stratégie pragmatique consiste à utiliser d'abord ces valeurs par défaut et à les ajuster ultérieurement. Pour une vue d'ensemble des performances et de l'échelle d'Hyperledger Fabric, qui constitue la base de {{site.data.keyword.blockchainfull_notm}} Platform, voir [Answering your questions on Hyperledger Fabric performance and scale](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

Tous les conteneurs qui sont associés à un noeud ont une **UC** et de la **mémoire**, alors que certains conteneurs qui sont associés à l'homologue, au noeud de tri et à l'autorité de certification ont également un **stockage**. Pour plus d'informations sur le stockage, voir [Considérations relatives au stockage de persistance](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage).

Vous êtes responsable de la surveillance de votre UC, de votre mémoire et de la consommation d'espace de stockage dans votre cluster. S'il vous arrive de demander plus de ressources pour un noeud de blockchain que les ressources disponibles, le noeud ne démarrera pas mais les noeuds existants ne sont pas affectés. Si vous utilisez {{site.data.keyword.cloud_notm}} comme fournisseur de cloud, possible de modifier l'UC et la mémoire depuis la console et le tableau de bord {{site.data.keyword.cloud_notm}} Kubernetes Service. Néanmoins, une fois qu'un noeud a été créé, il est également possible de modifier le stockage ultérieurement à partir de l'interface CLI {{site.data.keyword.cloud_notm}}. Pour plus d'informations sur l'augmentation du l'UC, de la mémoire et du stockage dans d'autres fournisseurs de cloud, consultez la documentation de ces derniers.
{:note}

Chaque noeud dispose d'un conteneur de proxy web gRPC qui amorce la couche de communication entre la console et un noeud. Ce conteneur a des valeurs de ressource fixes et il figure dans le panneau Allocation de ressources pour fournir une estimation précise de l'espace requis dans votre cluster Kubernetes pour que le noeud puisse être déployé. Comme les valeurs de ce conteneur ne peuvent pas être modifiées, nous ne parlerons pas du proxy web gRPC dans les sections ci-après.

### Autorités de certification (AC)
{: #ibp-console-govern-CA}

A la différence des homologues et des noeuds de tri, qui sont activement impliqués dans le processus de transaction, les autorités de certification sont uniquement impliquées dans l'enregistrement et l'inscription des identités, et dans la création d'un MSP. Cela signifie qu'elles ont besoin de moins d'UC et de mémoire. Pour contraindre une autorité de certification, un utilisateur devrait la surcharger de demandes (probablement à l'aide d'API et d'un script), ou émettre un tel grand nombre de certificats qu'elle est à court de stockage. Dans des conditions de fonctionnement normales, rien de cela ne doit arriver, bien que comme toujours ces valeurs doivent refléter les besoins d'un cas d'utilisation particulier.

L'adaptateur de canal comporte un seul conteneur associé que nous pouvons ajuster :

* **L'autorité de certification elle-même**: elle encapsule les processus d'autorité de certification internes, comme l'enregistrement et l'inscription des noeuds et des utilisateurs, ainsi que le stockage d'une copie de chaque certificat émis.

#### Dimensionnement d'une autorité de certification pendant sa création
{: #ibp-console-govern-CA-sizing-creation}

L'adaptateur de canal n'a qu'un seul conteneur comportant des valeurs dont vous devez vous soucier car les valeurs du serveur proxy web gRPC ne peuvent pas être modifiées.

| Ressources | Condition d'accroissement |
|-----------------|-----------------------|
| **UC et mémoire du conteneur d'autorité de certification** | Lorsque vous prévoyez que votre autorité de certification va être bombardée d'enregistrements et d'inscriptions. |
| **Stockage de l'autorité de certification** | Lorsque vous prévoyez d'utiliser cette autorité de certification pour enregistrer un grand nombre d'utilisateurs et d'applications. |

### Homologues
{: #ibp-console-govern-peers}

L'homologue comporte trois conteneurs associés que nous pouvons ajuster :

- **L'homologue lui-même** : il encapsule les processus d'homologue internes (comme la validation des transactions) et la blockchain (en d'autres termes, l'historique de la transaction) pour l'ensemble des canaux auxquels il appartient. Notez que le stockage de l'homologue inclut également les contrats intelligents installées sur l'homologue.
- **CouchDB** : emplacement de stockage des bases de données d'état de l'homologue. Gardez à l'esprit que chaque canal a une base de données d'état distincte.
- **Contrat intelligent** : Souvenez-vous que lors d'une transaction, le contrat intelligent approprié est "appelé" (en d'autres termes, exécuté). Notez que tous les contrats intelligents que vous installez sur l'homologue s'exécuteront dans un conteneur distinct au sein du conteneur de votre contrat intelligent, appelé conteneur Docker-in-Docker.  

L'homologue comporte également un conteneur pour **collecteur de journaux** qui dirige les journaux depuis le conteneur de contrats intelligents vers le conteneur homologue. De la même façon qu'avec le conteneur de proxy Web gRPC, vous ne pouvez pas ajuster le calcul pour ce conteneur.

#### Dimensionnement d'un homologue pendant sa création
{: #ibp-console-govern-peers-sizing-creation}

Comme nous l'avons indiqué dans la section [Comment la console interagit avec votre cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction), il est recommandé d'utiliser les valeurs par défaut pour ces conteneurs homologue et de les ajuster ultérieurement lorsqu'il devient évident comment ils sont utilisés par notre cas d'utilisation.

| Ressources | Condition d'accroissement |
|-----------------|-----------------------|
| **UC et mémoire du conteneur d'homologues** | Lorsque vous anticipez un haut débit de transactions immédiat. |
| **Stockage d'homologue** | Lorsque vous anticipez d'installer de nombreux contrats intelligents sur cet homologue et de les joindre à de nombreux canaux. Gardez à l'esprit que ce stockage sera aussi utilisé pour stocker les contrats intelligents de tous les canaux que l'homologue a rejoint. N'oubliez pas qu'on estime qu'une "petite" transaction avoisine les 10 000 octets (10k). Le stockage par défaut étant de 100 Go, cela signifie qu'un total de 10 million de transactions pourront figurer dans le stockage de l'homologue avant qu'il ne soit nécessaire de l'étendre (en pratique, le nombre maximum sera inférieur à cela, car les transactions sont de différentes tailles et ce nombre n'inclut pas les contrats intelligents). Même si 100 Go peut par conséquent sembler un stockage bien supérieur que nécessaire, gardez en tête que le stockage es relativement bon marché, et qu'il est plus difficile de l'augmenter (ligne de commande nécessaire) que d'augmenter l'UC ou la mémoire. |
| **UC et mémoire du conteneur CouchDB** | Lorsque vous anticipez un volume important de requêtes sur une base de données d'état de grande taille. Cet effet peut être quelque peu atténué par l'utilisation d'[index](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html#couchdb-indexes){: external}. Néanmoins, des volumes élevés peuvent exercer une contrainte sur CouchDB, ce qui peut entraîner des délais de requêtes et de transactions. |
| **Stockage CouchDB (données de registre)** | Lorsque vous prévoyez un débit élevé via de nombreux canaux et n'envisagez pas d'utiliser des index. Toutefois, comme pour le stockage d'homologue, le stockage CouchDB par défaut est de 100 Go, ce qui est élevé. |
| **UC et mémoire du conteneur de contrats intelligents** | Lorsque vous prévoyez un débit élevé sur un canal, notamment dans les cas où plusieurs contrats intelligents vont être appelés en une seule opération.|

### Noeuds de tri
{: #ibp-console-govern-ordering-nodes}

Comme les noeuds de tri ne gèrent pas la base de données d'état et qu'ils n'hébergent pas non plus des contrats intelligents, ils ont besoin de moins de conteneurs que les homologues. Ils hébergent cependant la blockchain (l'historique de la transaction) car il s'agit de l'emplacement où est stockée la configuration de canal, et le service de tri doit avoir connaissance de la configuration de canal la plus récente pour exécuter son rôle.

Comme avec l'autorité de certification, le service de tri comporte un seul conteneur associé que nous pouvons ajuster (si vous déployiez un service de tri à cinq noeuds, cinq ensemble de conteneurs de noeuds de tri distincts) :

* **Le service de tri lui-même** : il encapsule les processus de service de tri internes (comme la validation des transactions) et la blockchain pour l'ensemble des canaux qu'il héberge.

#### Dimensionnement d'un service de tri pendant sa création
{: #ibp-console-govern-orderer-sizing-creation}

Comme nous l'avons indiqué dans la section [Comment {{site.data.keyword.cloud_notm}} Kubernetes Service interagit avec la console](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction), il est recommandé d'utiliser les valeurs par défaut pour ces conteneurs de services de tri et de les ajuster ultérieurement lorsque leur utilisation devient plus claire.

| Ressources | Condition d'accroissement |
|-----------------|-----------------------|
| **UC et mémoire du conteneur de services de tri** | Lorsque vous anticipez un haut débit de transactions immédiat. |
| **Stockage du noeud de tri** | Lorsque vous anticipez que ce service de tri va faire partie d'un service de tri sur de nombreux canaux. N'oubliez pas que les services de tri conservent une copie de la blockchain pour chaque canal qu'ils hébergent. Le stockage par défaut d'un noeud de tri est de 100 Go, tout comme le conteneur pour l'homologue lui-même. |

Une bonne pratique consiste à faire en sorte que vos noeuds de tri disposent de suffisamment d'UC et de mémoire. Si un service de tri est sursollicité, des délais peuvent se produire et il peut commencer à supprimer des transactions, lesquelles doivent alors de nouveau être soumises. Cela peut avoir des conséquences bien plus nuisibles pour un réseau qu'un simple homologue qui lutte pour maintenir l'activité. Dans une configuration de service de tri Raft, un noeud principal sursollicité peut cesser d'envoyer des messages de contact, entraînant le choix d'un élément principal, et une cessation temporaire de tri des transactions. De même, un noeud suiveur peut rater des messages et essayer de déclencher le choix d'un élément principal alors qu'aucun n'est nécessaire.
{:important}

## Réallocation des ressources
{: #ibp-console-govern-reallocate-resources}

Après le redimensionnement d'un noeud, vous pouvez constater un délai avant la prise en compte car les conteneurs sont en cours de reconstitution.
{:important}

Comme indiqué plus haut si votre fournisseur de cloud est {{site.data.keyword.cloud_notm}}, nous vous recommandons d'utiliser l'outil [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} en association avec votre tableau de bord {{site.data.keyword.cloud_notm}} Kubernetes pour surveiller votre utilisation des ressources Kubernetes. Si vous déterminez qu'un noeud worker n'a plus de ressources, vous pouvez ajouter un nouveau noeud worker plus grand à votre cluster et supprimer le noeud worker existant.
{:note}

Bien qu'il soit moins contraignant de déployer suffisamment de ressources dans votre cluster Kubernetes dès le début et d'être par conséquent en mesure de déployer et d'étendre les ressources sans avoir à augmenter les ressources dans votre cluster, il est clair que plus grand sera le déploiement, plus il sera onéreux. Les utilisateurs doivent évaluer soigneusement toutes les possibilités et tenir compte des compromis qu'ils doivent effectuer quelle que soit l'option choisie.

Si votre fournisseur de cloud est {{site.data.keyword.cloud_notm}} ou dans un autre fournisseur de cloud (utilisant {{site.data.keyword.cloud_notm}} Private), vous pouvez dimensionner votre cluster manuellement, en surveillant vos noeuds et en ajoutant des davantage de noeuds ou de plus gros noeuds. Même si ce processus peut demander beaucoup de main d'oeuvre, l'avantage est que l'utilisateur est toujours certain du montant facturé sur son compte de cloud.

Si un utilisateur a effectué un déploiement sur {{site.data.keyword.cloud_notm}} à l'aide de {{site.data.keyword.cloud_notm}} Kubernetes Service, il a aussi la possibilité d'utiliser la fonction **Mise à l'échelle** de {{site.data.keyword.cloud_notm}} Kubernetes Service. La fonction Mise à l'échelle auto permet d'augmenter ou de réduire vos noeuds worker en fonction des paramètres spécifiques de votre pod et des demandes de ressources. Pour plus d'informations sur la fonction {{site.data.keyword.cloud_notm}}Mise à l'échelle auto Kubernetes Service et comment la configurer, voir [Mise à l'échelle de clusters ](/docs/containers?topic=containers-ca#ca){: external} dans la documentation {{site.data.keyword.cloud_notm}}. Notez que si la fonction de mise à l'échelle auto est utilisée pour l'ajustement de vos ressources, des frais seront appliqués à votre compte {{site.data.keyword.cloud_notm}} Kubernetes Service et ils varieront en fonction de votre utilisation.

Pour effectuer une mise à l'échelle manuelle sur la console, cliquez sur le noeud que vous voulez ajuster sur la page **Noeuds**, puis cliquez sur l'onglet **Utilisation**. Vous pouvez alors voir un bouton libellé **Réallouer**, lequel permet d'ouvrir un onglet **Allocation de ressources** qui est très similaire à celui que vous avez vu lors de la création du noeud. Si vous voulez réduire la quantité de ressources disponibles, indiquez simplement de plus petites valeurs et cliquez sur **Réallouer les ressources** sous cet onglet. La page **Récapitulatif** s'affiche.

Si vous voulez accroître l'UC et la mémoire d'un noeud, utilisez l'onglet **Allocation de ressources** sur la console pour augmenter ces valeurs. La boîte blanche au bas de la page ajoute les nouvelles valeurs. Une fois que vous avez cliqué sur **Réallouer les ressources**, la page **Récapitulatif** convertit cette valeur en un montant **VPC**, lequel est utilisé pour calculer votre facture. Vous devrez ensuite accéder à votre cluster Kubernetes pour vous assurer que celui-ci dispose de suffisamment de ressources pour cette réallocation. Si tel est le cas, vous pouvez cliquer sur **Réallouer les ressources**. S'il n'y a pas suffisamment de ressources disponibles, vous devrez augmenter la taille de votre cluster.

La méthode que vous utiliserez pour accroître le stockage dépendra de la classe de stockage que vous avez choisie pour votre cluster. Reportez-vous à la documentation de votre fournisseur de cloud pour en savoir plus à ce sujet. Dans {{site.data.keyword.cloud_notm}}, cette rubrique s'intitule [options de stockage](/docs/containers?topic=containers-kube_concepts#kube_concepts){: external}. Notez que dans {{site.data.keyword.cloud_notm}}, si vous êtes sur le point d'épuiser la mémoire de votre homologue ou noeud de tri, vous devez déployer un nouvel homologue ou service de tri avec un système de fichiers plus important et le laisser se synchroniser par l'intermédiaire de vos autres composants sur les mêmes canaux.

Dans {{site.data.keyword.cloud_notm}}, l'UC et la mémoire peuvent être accrues à l'aide de la console (si des ressources sont disponibles dans votre cluster your {{site.data.keyword.cloud_notm}} Kubernetes Service). Toutefois, le stockage doit être augmenté à l'aide de l'interface CLI d'{{site.data.keyword.cloud_notm}}. Pour suivre un tutoriel relatif à cette opération, voir [Changing the size and IOPS of your existing storage device](/docs/containers?topic=containers-file_storage#file_change_storage_configuration){: external}. Si vous utilisez un fournisseur de cloud autre que {{site.data.keyword.cloud_notm}}, consultez la documentation de ce dernier pour plus de détails sur le processus d'augmentation de l'UC, de la mémoire et du stockage.

## Mise à jour de la définition MSP d'une organisation
{: #ibp-console-govern-update-msp}

Au fil du temps, vous devrez peut-être mettre à jour la définition MSP d'une organisation, en ajoutant des admin d'organisation supplémentaires, par exemple, ou en modifiant la nom d'affichage du MSP sur la console.

- Exportez simplement la définition MSP existante depuis l'onglet **Organisations** et éditez le fichier JSON généré afin de modifier le nom d'affichage, les certificats existants, ou afin d'ajouter des certificats. Il est déconseillé de modifier la zone `msp_id` car cela peut entraîner des modifications avec rupture sur votre réseau.
- Cliquez sur votre définition MSP sous l'onglet **Organisations** pour l'ouvrir, puis sur **Ajouter fichier** pour télécharger le nouveau fichier JSON.
- Cliquez sur **Mettre à jour une définition de MSP**. Les modifications sont effectives immédiatement.

## Mise à jour d'une configuration de canal
{: #ibp-console-govern-update-channel}

Même si la création d'un canal et la mise à jour d'un canal ont le même objectif, offrir aux utilisateurs la possibilité de vérifier que la configuration de leur canal convient autant que possible à leur cas d'utilisation, les deux processus sont en fait très différents **en tant que tâches** sur la console. Gardez à l'esprit, comme indiqué dans la documentation relative à la [Création d'un canal](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel) qu'il s'agit d'un processus mise en oeuvre par une **seule organisation**. Dès lors qu'une organisation est membre du consortium du service de tri qui va devenir le service de tri d'un canal, elle peut créer le canal de la manière qu'elle le souhaite. Elle peut lui donner le nom de son choix, lui ajouter des organisations (si elles sont membres du consortium), affecter des droits à ces organisations, définir des listes de contrôle d'accès, et ainsi de suite.

Les autres organisations ont le choix de participer ou pas à ce canal (en lui joignant des homologues, par exemple), mais on suppose de le processus collaboratif de choix des paramètres de canal s'effectue hors bande, avant qu'une organisation utilise la console pour créer le canal.

La mise à jour d'un canal est un processus différent. Elle a lieu **au sein de la console**, et elle applique des procédures de gouvernance collaborative qui sont essentielles au fonctionnement d'{{site.data.keyword.blockchainfull_notm}} Platform. Le processus collaboratif implique l'envoi des demandes de mise à jour de configuration aux organisations qui ont un rôle d'administration dans le canal. Ces organisations sont également appelées **opérateurs** de canal.

Pour mettre à jour un canal, cliquez sur le canal sous l'onglet **Canaux** . Cliquez sur le bouton **Paramètres** en regard du nom de canal en haut de la page. Le panneau qui s'affiche est très similaire au panneau qui permet de créer un canal.

### Paramètres de configuration de canal que vous pouvez mettre à jour
{: #ibp-console-govern-update-channel-available-parameters}

Il est possible de modifier certains, mais pas l'ensemble, des paramètres de configuration d'un canal une fois celui-ci créé. Et il n'est possible de mettre à jour qu'un seul paramètre qui n'est pas disponible lors de la création du canal.

Vous pouvez voir que le **Nom de canal** est grisé et ne peut pas être modifié. Ceci indique en fait que le nom de canal ne peut pas être changé une fois qu'il a été créé. Vous pouvez voir également que le nom d'affichage du service de tri n'est pas présent, car le service de tri d'un canal ne peut pas non plus être modifié une fois le canal créé.

Vous pouvez, toutefois, modifier les paramètres de configuration du canal suivants :

* **Organisations**. Cette section du panneau indique comment les organisations sont ajoutées ou supprimées d'un canal. Les organisations qui peuvent être ajoutées figurent dans la liste déroulante. Notez qu'une organisation doit être membre du consortium du service de tri pour pouvoir être ajoutée à un canal. Pour plus d'informations sur l'ajout d'une organisation au consortium, voir [Ajouter votre organisation à la liste des organisations pouvant effectuer une transaction](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-add-org).

  Vous pouvez également mettre à jour le niveau de droit d'une organisation sur le canal :

   - Un **opérateur** de canal a le droit de créer et de signer des mises à jour de configuration de canal. Il doit y avoir au moins un opérateur dans chaque canal.
   - Un **rédacteur** de canal peut mettre à jour le registre de canal en appelant un contrat intelligent. Un rédacteur de canal peut aussi instancier un contrat intelligent sur un canal.
   - Un **lecteur** de canal peut uniquement interroger le registre de canal, en appelant une fonction en lecture dans un contrat intelligent par exemple.

* **Règle de mise à jour du canal**. La règle de mise à jour d'un canal indique combien d'organisations (sur le nombre total d'opérateurs du canal), doivent approuver une mise à jour de configuration de canal. Pour garantir un bon équilibre entre une administration collaborative et un traitement efficace des mises à jour de configuration de canal, pensez à définir cette règle pour une majorité d'administrateurs. Par exemple, s'il y a cinq admins dans un canal, choisissez `3 de 5`.

* **Paramètres de découpage de bloc**. (Option avancée) Comme une modification des paramètres de découpage de bloc par défaut doit être signée par un admin de l'organisation de service de tri, ces zones ne figurent pas dans le panneau de création de canal. Toutefois, comme cette configuration de canal va être envoyée à toutes les organisations appropriées du canal, il est possible d'envoyer une demande de mise à jour de configuration de canal avec des modifications des paramètres de découpage de bloc. Ces zones déterminent les conditions dans lesquelles le service de tri découpe un nouveau bloc. Pour plus d'informations sur la manière dont ces zones affectent le découpage de blocs, voir [Paramètres de découpage de bloc](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning-batch-size).

* **Listes de contrôle d'accès**. (Option avancée) Pour assurer un contrôle plus précis des ressources, vous pouvez limiter l'accès à une ressource à une organisation et à un rôle au sein de cette organisation. Par exemple, la définition d'un accès à la ressource `ChaincodeExists` pour `Application/Admins` signifie que seul l'admin d'une application peut accéder à la ressource `ChaincodeExists`.

Si vous limitez l'accès à une ressource à une organisation spécifique, sachez que seule l'organisation pourra accéder à la ressource. Si vous souhaitez que d'autres organisations puissent accéder à la ressource, vous devrez les ajouter une par une à l'aide des zones ci-dessous. Par conséquent, réfléchissez bien aux décisions à prendre en matière de contrôle d'accès. La limitation de l'accès à certaines ressources peut avoir un effet très négatif sur le fonctionnement de votre canal.
{:important}

Comme la console offre à un seul utilisateur la possibilité de détenir et de contrôler plusieurs organisations, vous devez indiquer l'organisation que vous utilisez lorsque vous signez une mise à jour de canal dans la section **Organisation qui met à jour le canal**. Si vous détenez plusieurs organisations de ce canal, vous pouvez choisir l'une de ces organisations du canal pour la signature. Selon la **règle de mise à jour de canal** que vous avez choisie, vous pouvez recevoir une notification vous demandant de signer la demande comme une ou plusieurs des autres organisations que vous détenez.

Si vous essayez de modifier l'un des **paramètres de découpage de bloc** et détenez l'organisation de service de tri de ce canal, vous verrez une zone pour l'organisation de service de tri. Sélectionnez le MSP de l'organisation de service de tri approprié dans la liste déroulante. Si vous n'êtes pas un administrateur de l'organisation de service de tri, vous pouvez toujours effectuer une demande de modification d'un paramètres de découpage de bloc, mais la demande va être envoyée, et devra être signée, par un admin de service de tri.

### Flux de collecte de signature
{: #ibp-console-govern-update-channel-signature-collection}

Pour que les signatures puissent être vérifiées, les organisations d'un canal doivent exporter les MSP (au format JSON) représentant leurs organisations pour les autres organisations sur le canal, mais également importer les MSP des autres organisations. Pour exporter un MSP, cliquez sur le bouton de téléchargement dans votre MSP (sous l'onglet **Organisations**), puis envoyez-le aux autres organisations hors bande. Lorsque vous recevez un fichier JSON d'un MSP, importez-le à l'aide de l'écran **Organisations**.
{: important}

Une fois que la demande de mise à jour de configuration du canal à été effectuée, elle sera envoyée aux organisations du canal qui ont le droit de la signer. Par exemple, s'il y a cinq opérateurs (administrateurs de canal) dans un canal, elle sera envoyée aux cinq. Pour que la mise à jour de configuration de canal soit approuvée, le nombre d'opérateurs de canal répertoriés dans la **règle de mise à jour de canal** doivent être respecté. Si cette règle indique `3 sur 5`, la mise à jour de configuration de canal sera envoyée aux cinq opérateurs, et si trois d'entre eux la signent, elle prend effet.

Ce processus consistant à savoir quand vous avez une mise à jour à signer, et comment la signer, est géré à l'aide du bouton **Notifications** (qui ressemble à une cloche) dans l'angle supérieur droit de la console. Lorsque vous voyez un point bleu sur le bouton **Notifications**, cela signifie que avez une demande en attente à évaluer ou que vous êtes informé d'un événement de mise à jour de canal.

Lorsque vous cliquez sur le bouton **Notifications**, plusieurs actions sont possibles :

* **Intervention requise** : l'utilisateur actuel doit signer la demande (en tant qu'organisation de l'homologue ou du service de tri) ou il doit soumettre la demande (si toutes les signatures requises sont déjà collectées).
* **Ouvert** : inclut toutes les demandes pour lesquelles une **intervention est requise** ainsi que les demandes qui ont été signées par l'utilisateur et doivent encore être signées par un ou plusieurs autres membres de canal.
* **Fermé** : demandes qui ont été soumises. Aucune action ne doit être entreprise sur ces éléments. Ils peuvent uniquement être affichés.
* **Tout** : inclut les demandes à la fois ouvertes et fermées.

Si une demande de mise à jour de configuration a été effectuée, vous aurez la possibilité de cliquer sur `Vérification et mise à jour de la configuration de canal` et voir les modifications apportées de mise à jour de configuration de canal qui sont proposées ou qui ont été effectuées (si la nouvelle configuration de canal a été approuvée). Si vous êtes opérateur du canal, et qu'il n'y pas suffisamment de signatures collectées pour approuver la demande de mise à jour de configuration de canal, vous aurez la possibilité de signer la demande de mise à jour.

Vous n'êtes pas mandaté pour signer une mise à jour de configuration de canal ; cependant il n'est pas possible de signer **contre** un mise à jour de canal. Si vous n'approuvez pas une mise à jour de configuration de canal, vous pouvez simplement fermer le panneau et contacter les autres opérateurs de canal hors bande pour leur faire part de votre refus. Toutefois, si un nombre suffisant d'opérateurs du canal approuvent la mise à jour conformément à la règle de mise à jour de canal, la nouvelle configuration prend effet.
{:note}

### Configuration des homologues d'ancrage
{: #ibp-console-govern-channels-anchor-peers}

Comme la communication de protocole [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} entre organisations doit être activée pour la reconnaissance de service et les données privées, un homologue d'ancrage doit exister pour chaque organisation. Cet homologue d'ancrage n'est pas un **type** d'homologue spécial, il s'agit simplement de l'homologue que l'organisation présente aux autres organisations, et ce faisant amorce la communication gossip entre organisations. Par conséquent, au moins un [homologue d'ancrage](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html#anchor-peers){: external} doit être défini pour chaque organisation dans la définition de collecte.

Pour configurer un homologue en tant qu'homologue d'ancrage, cliquez sur l'onglet **Canaux** et ouvrez le canal dans lequel le contrat intelligent a été instancié.
 - Cliquez sur l'onglet **Détails du canal**.
 - Faites défiler jusqu'au tableau des homologues d'ancrage et cliquez sur **Ajouter un homologue d'ancrage**.
 - Sélectionnez au moins un homologue de chaque organisation dans la définition de collecte qui doit servir d'homologue d'ancrage pour l'organisation. Pour des raisons de redondance, vous pouvez envisager de sélectionner plusieurs homologues de chaque organisation dans la collecte.

## Optimisation de votre service de tri
{: #ibp-console-govern-orderer-tuning}

Les performances de votre plateforme de blockchain peuvent être affectées par de nombreuses variables, telles que la taille de transaction, la taille de bloc, la taille de réseau, ainsi que les limites du matériel. Le noeud du service de tri inclut un ensemble de paramètres d'optimisation qui peuvent être utilisés pour contrôler le débit et les performances du service de tri.  Vous pouvez utiliser ces paramètres pour personnaliser la façon dont votre service de tri traite les transactions selon que vous avez un grand nombre de petites transactions fréquentes, ou moins de transactions de plus grande taille et moins fréquentes. Fondamentalement, vous avez la possibilité de décider à quel moment découper les blocs en fonction de la taille, de la quantité et du débit d'arrivée des transactions.

Les paramètres suivants sont disponibles sur la console en cliquant sur le noeud de service de tri sous l'onglet **Noeuds**, puis en cliquant sur son icône **Paramètres**. Cliquez sur le bouton **Avancé** pour ouvrir la **Configuration de canal avancée** du service de tri.

### Paramètres de découpage de bloc
{: #ibp-console-govern-orderer-tuning-batch-size}

Les trois paramètres suivantes fonctionnent conjointement pour contrôler le moment où un bloc peut être découpé, en fonction du nombre maximum de transactions définies dans un bloc et de la taille de bloc elle-même.

- **Nombre max d'octets absolu**  
  Définissez cette valeur sur la taille de bloc la plus élevée en octets pouvant être découpée par le service de tri.  Aucune transaction ne peut être supérieure à la valeur de `Nombre max d'octets absolu`. Généralement, ce paramètre peut être sans problème deux ou trois fois supérieur à la valeur de `Nombre max d'octets préféré`.    
  **Remarque **: La taille maximum autorisée est de 99 Mo.
- **Nombre max de messages**   
  Définissez cette valeur sur le nombre maximum de transactions pouvant être incluses dans un seul bloc.
- **Nombre max d'octets préféré**  
  Définissez cette valeur sur la taille de bloc idéale en octets, celle-ci devant être inférieure à la valeur de `Nombre max d'octets absolu`. Une taille de transaction minimum (transaction sans adhésions) est d'environ 1 Ko.  Si vous ajoutez 1 Ko par adhésion requise, une taille de transaction standard est d'environ 3 à 4 Ko. Par conséquent, il est recommandé de définir pour `Nombre max d'octets préféré` une valeur égale à environ `Nombre max de messages * taille de transfert moyen prévu`. Lors de l'exécution, chaque fois que possible, les blocs ne doivent pas dépasser cette taille. Si lors de l'arrivée d'une transaction, la taille de bloc est dépassée, le bloc est découpé et un nouveau bloc est créé pour cette transaction. Néanmoins, si à l'arrivée d'une transaction, cette valeur est dépassée mais qu'elle ne dépasse pas la valeur de `Nombre max d'octets absolu`, la transaction est incluse. Si un bloc entrant est d'une taille supérieure à la valeur de la zone `Nombre max d'octets préféré`, il ne contiendra qu'une seule transaction, et cette dernière ne peut pas être d'une taille supérieure à la valeur de `Nombre max d'octets absolu`.

Ensemble, ces paramètres peuvent être configurés pour optimiser le débit de votre service de tri.

### Dépassement de délai de lot
{: #ibp-console-govern-orderer-tuning-batch-timeout}

Définissez la valeur de **Dépassement de délai** sur le délai d'attente, en secondes, à observer après l'arrivée de la première transaction avant de découper le bloc. Si vous définissez une valeur trop faible, les lots risquent de ne pas se remplir à la taille préférée. Si la valeur est trop élevée, il se peut que le service de tri attende les blocs et que les performances globales se dégradent. De manière générale, nous recommandons de définir la valeur de `Dépassement de délai de lot` sur une valeur au moins égale à `nombre de messages max / nombre max de transactions par seconde`.

Lorsque vous modifiez ces paramètres, cela n'a aucune incidence sur le comportement des canaux existants sur le service de tri ; en revanche, les modifications que vous apportez à la configuration de service de tri s'appliquent uniquement aux nouveaux canaux que vous créez sur ce service de tri.
{:important}
