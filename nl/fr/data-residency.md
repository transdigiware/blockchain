---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: IBM Blockchain Platform, IBM Cloud Private, AWS, Data residency, world state

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

# Hébergement de données
{: #console-icp-about-data-residency}

Etant donné que les réseaux de blockchain dépendent du type de données qui sont traitées, des étapes supplémentaires sont parfois nécessaires pour assurer la sécurité de certains types de données. L'exigence la plus courante sur l'hébergement de données est associée aux réglementations de certains pays, qui demandent que toutes les données qui sont traitées et stockées sur un système informatique doivent demeurer à l'intérieur des frontières d'un pays donné. De même, certaines entreprises dans des secteurs extrêmement régulés, comme le gouvernement, la santé et les services financiers, exigent que les données soient entièrement stockées derrière leur pare-feu.

Les réseaux de blockchain permettent à plusieurs organisations d'utiliser un registre distribué pour effectuer des transactions et partager des données de façon sûre et sécurisée. Cependant, cela suppose que les données puissent être réparties entre les noeuds du réseau et les régions où résident ces noeuds. Les organisations peuvent utiliser plusieurs options pour séparer les données du reste du réseau et parvenir à un hébergement des données. 
1. [Collectes de données privées sur un canal partagé](#console-icp-about-data-residency-fabric)
2. [Collectes de données privées sur un canal distinct](#console-icp-about-data-residency-use-case)
3. [Canal distinct avec tous les noeuds sur le canal au sein d'un seul pays](#console-icp-about-data-residency-use-case-channel)

Chaque approche fournit un niveau accru d'isolement et de protection pour vos données, mais elle implique un effort supplémentaire d'implémentation et de gestion. Pour vous aider à comprendre comment chaque option peut être utilisé pour parvenir à un hébergement de données, nous donnent un aperçu de la façon dont les données sont partagées au sein d'un réseau Hyperledger Fabric. Nous présentons ensuite un exemple de cas d'utilisation pour illustrer comment des organisations au sein d'un consortium de blockchain peut utiliser chaque option pour séparer ses données et les empêcher de quitter leur région.

## Comment les données sont partagés au sein d'un réseau {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-icp-about-data-residency-fabric}

L'architecture d'Hyperledger Fabric qui est sous-jacente à {{site.data.keyword.blockchainfull_notm}} Platform est centrée autour de trois composants clés : un service de tri (composé de services de tri), des autorités de certification et des homologues. De plus, les organisations envoient des transactions à ces applications client à l'aide de logiciels SDK [Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}. Lorsqu'on envisage l'hébergement de données, il est important de comprendre comment ces composants interagissent et stockent les données.

Les **homologues** sont utilisés par les membres du consortium pour stocker le [registre](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ledger/ledger.html){: external} de blockchain. Le registre de blockchain se compose de deux parties. Le premier est le world state, qui stocke la dernière valeur pour toutes les données du registre dans des paires clé-valeur. La seconde est l'enregistrement de blockchain de chaque transaction. Les homologues reçoivent les mises à jour d'état sous la forme de nouveaux blocs du service de tri. Ils utilisent ensuite les blocs et le world state pour confirmer (ou valider) des transactions, mettre à jour le world state et ajouter le journal des transactions dans la blockchain. Le service de tri établit l'ordre des transactions pour tous les homologues dans le [consortium](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium) et stocke une copie de la portion de blockchain du registre.

Les **canaux** sont un mécanisme pour la transmission des données au sein d'un réseau. Vous ne pouvez pas participer à un réseau de blockchain sans rejoindre un canal. Les canaux peut être utilisé par les membres du réseau pour créer des séparations logiques entre des applications métier et même pour booster les performances en limitant le trafic. Ils peuvent également être utilisés par des sous-ensembles d'organisations du consortium pour effectuer des transactions privées et séparer les données.

Les homologues gèrent un registre séparé pour chaque canal rejoint. Seules les organisations qui sont membres du canal peut joindre leurs homologues au canal et recevoir des mises à jour de registre du service de tri. Par conséquent, chaque canal est lié à un service de tri, lequel stocke la portion de blockchain de chaque registre de canal qu'il gère. Les applications client soumettent des transactions aux homologues et au service de tri d'un canal donné. Ces transactions sont ajoutées au journal des transactions au sein de la blockchain et elles incluent des données [read-write set](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, lesquelles sont ajoutées aux paires clé-valeur du world state.

Si l'hébergement de données dans le pays est une exigence, vous devez prévoir l'emplacement de vos homologues, du service de tri, ainsi que de vos applications client. Vous devez également connaître l'emplacement des homologues appartenant à d'autres organisations sur vos canaux. Si vous utilisez {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, vous pouvez trouver la liste des régions et emplacements [{{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference?topic=blockchain-ibp-regions-locations#ibp-regions-locations) où vous et les membres de votre consortium pouvez déployer vos composants.

## Cas d'utilisation de l'hébergement des données dans un pays
{: #console-icp-about-data-residency-use-case}

Nous pouvons utiliser un exemple de consortium pour illustrer comment les données sont distribuées sur dans {{site.data.keyword.blockchainfull_notm}} Platform, et explorer comment les membres peuvent obtenir l'hébergement de données. La figure ci-dessous contient un consortium avec un service de tri et quatre organisations. Chaque organisation a un noeud homologue. Deux organisations, Org A et Org B, ainsi que le service de tri, se trouvent aux Etats-Unis. Les deux autres organisations, Org C et Org D, se situent en Allemagne. Les quatre organisations sont des membres de leurs homologues et elles ont joint ces derniers au canal X.

![Exemple de consortium](images/data_res_use_case.svg "Exemple de consortium")

Chaque homologue qui a rejoint le canal X stocke une copie du registre de canal, comme indiqué dans la **Figure 1** en tant que registre X. Comme les homologues des Etats-Unis et d'Allemagne sont joints au canal, les données sur le registre de canal réside dans les deux zones géographiques. La portion de blockchain du registre est également stockée par le service de tri situé aux Etats-unis.

Si deux organisations du consortium créent un deuxième canal, le canal Y, un deuxième registre est créé et stocké sur l'homologue des membres du canal. Seules les organisations qui ont rejoint le canal auront une copie des données du canal.

![Ajout d'un deuxième canal](images/data_res_use_case_channel.svg "Ajout d'un deuxième canal")


Dans la **Figure 2**, Org B et Org D ont rejoint le canal Y. Les homologues de Org B et Org D stockent maintenant une copie du registre Y, en plus du registre X. Comme le même service de tri a été utilisé pour créer le canal X et le canal Y, le service de tri a désormais une copie de la portion de blockchain des deux registres de canal. Dans la **Figure 1** et la **Figure 2**, les données créées par l'application en Allemagne sont stockées aux Etats-Unis, ce qui n'est pas souhaitable si l'hébergement de données est une exigence.

Nous pouvons utiliser l'exemple ci-dessus pour explorer les options dont disposent les organisations pour obtenir un hébergement de données. Supposons qu'une loi en Allemagne impose que certaines données créées par Org C et Org D restent dans le pays. Les organisations situées en Allemagne peuvent utiliser les trois options pour éviter que des données ne soient stockées aux Etats-unis.

## Option 1 : Collectes de données privées sur un canal partagé
{: #console-icp-about-data-residency-use-case-private-data}

Org C et Org D peuvent utiliser la [fonction Private Date](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "Qu'est-ce qu'une collection de données privées ?"){: external} d'Hyperledger Fabric pour éviter que les données ne soient réparties entre toutes les organisations sur le canal. Les collections de données privées permettent aux organisations de partager des données d'état d'homologue à homologue data (via le protocole gossip) avec d'autres organisations qui sont autorisées à lire la collection. Les données sont stockées dans une base de données privée, séparées, sur l'homologue. Le service de tri n'est pas impliqué et il ne voit pas les données privées. Seul un hachage des données de la collection est ajouté au registre de canal et stocké sur les homologues des autres membres du canal et le service de tri. Cela permet aux organisations de vérifier les données privées s'ils souhaitent rendre publics les détails de transaction. Pour en savoir plus, consultez l'article [Private Data](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external} dans la documentation Fabric.

![Private data](images/data_res_private_data.svg "légende trois")

Dans la **Figure 3**, Org C et Org D ont créé une collection de données privées, la collection OrgC-OrgD, qui permet aux organisations d'effectuer des transactions sans avoir à partager des données avec Org A, Org B ou le service de tri. Les donnée d'état de la valeur de clé au sein de cette collection sont uniquement stockées sur les homologues de Org C et Org D et elles ne sortent pas d'Allemagne. Toutefois, un hachage des données au sein de la collection est stocké dans le registre X et partagé avec le canal étranger. Cela signifie qu'un hachage des données dans la collection OrgC-OrgD est stocké sur les homologues et le service de tri aux Etats-Unis.

Lorsqu'il s'agit de données privées, il est important de comprendre la différence entre **données hachées** et **données chiffrées**. Le chiffrement utilise une fonction bidirectionnelle pour transformer des données dans un format qui masque sa valeur d'origine, mais qui permet une conversion vers l'état d'origine. Par exemple, lorsque des données sont envoyées sur un réseau sécurisé via TLS, les données sont chiffrées à l'aide d'un certificat TLS. Elles sont ensuite envoyées via le réseau sous forme de texte crypté, puis déchiffrées par le destinataire. Le texte chiffré contient toutes les données d'origine, et il peut être déchiffré à l'aide d'une clé privée. Toutefois, le hachage est une fonction unidirectionnelle qui utilise les données pour créer une chaîne unique de nombres et des lettres. Les données hachées ne peuvent pas être reconverties vers leur forme d'origine à l'aide du hachage. Pour vérifier les données qui ont créé le hachage, un destinataire doit créer un hachage des données d'origine à l'aide de la même fonction de hachage, et vérifier que les valeurs de hachage correspondent. Un tiers ne peut pas utiliser le hachage sans une copie des données d'origine.  

Il est important de savoir qu'avec cette option, même si Org A et Org B ne peuvent pas voir les données de registre réelles car elles sont hachées, elles peuvent toujours voir que Org C et Org D effectuent des transactions ainsi que le volume de transactions effectuées entre ces deux organisations. 

Sachez également que les données au sein d'une collection de données privées peuvent être purgées depuis l'homologue sur lequel elles sont stockées. Alors que les données sont stockées sur un canal de manière permanente, les collections autorisent les membres à indiquer le nombre de blocs qui sont validés sur un canal avant que les [données privées soient purgées](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private_data_tutorial.html#pd-purge){: external}. Une fois les données retirées de la collection de données privées, le hachage sur le canal ne peut plus être utilisé pour vérifier la transaction qui l'a créé. Dans l'exemple de réseau de la **Figure 3**, Org C et Org D peuvent utiliser une règle de `durée de bloc` qui vérifie que les données qui ne doivent pas rester de manière permanente sont intégralement retirées du réseau dans un délai défini.

## Option 2 : Collectes de données privées sur un canal distinct
{: #console-icp-about-data-residency-use-case-private-data-channel}

Org C et Org D peuvent également utiliser des collection de données privées dans le contexte d'un canal distinct pour permettre un meilleur isolement des données. La création d'un nouveau canal, le canal Y dans ce cas, garantit que le hachage des données privées est uniquement partagé avec le service de tri, sans être partagé avec d'autres membres du consortium et stocké sur leurs homologues.

![Utilisation de données privées avec un canal distinct](images/data_res_private_data_channel.svg "Utilisation de données privées avec un canal distinct")

Dans la **Figure 4**, Org C et Org D ont formé un nouveau canal, le canal Y, qui ne contient aucun membre aux Etats-Unis. Par conséquent, le hachage des données de la collection OrgC-OrgD est stocké dans le registre Y au lieu du registre X, et il n'est pas stocké sur les homologues aux Etats-Unis. Comme le service de tri est situé aux Etats-unis, un hachage des données créées en Allemagne quitte quand même le pays.

La création d'un canal distinct permet d'éviter que les détails d'une transaction ne soient partagés avec d'autres organisations du consortium. Si les organisations en Allemagne utilisaient un canal partagé, Org A et Org B pourraient voir le nombre de hachages de transaction qui sont validés dans registre de canal par la collection de données privées. Cela pourrait donner à ces organisations une visibilité sur le fait que Org C et Org D effectuent des transactions et sur le volume des transactions générées entre elles. Il faut savoir néanmoins que la formation d'un nouveau canal implique une gestion supplémentaire pour la création et la mise à jour. Cela complique également le partage des données de Org C et Org D avec Org A et Org B.

## Option 3 : Canal avec tous les composants dans un seul pays
{: #console-icp-about-data-residency-use-case-channel}

Org C et Org D peuvent également créer un canal avec toute l'infrastructure nécessaire au sein d'un même pays. Cela implique que les homologues aient rejoint le canal, et que les applications et le service de tri résident tous dans la même région. Dans ce scénario, aucune des données stockées dans le registre de canal ne va quitter la région et être stockée en dehors du pays.

![Création d'un canal distinct](images/data_res_separate_channel.svg "Création d'un canal distinct")

Dans la **Figure 5**, Org C et Org D ont créé un nouveau canal pour les données qui ne doivent pas sortir d'Allemagne. Cela implique la création d'un nouveau service de tri situé en Allemagne pour garantir que la copie du service de tri du registre de canal est stockée dans le pays. Comme dans ce cas les services de tri, OrgC-peer et OrgD-peer, se situent en Allemagne, Org C et Org D peuvent désormais conserver les données publiques sur le canal si elles le souhaitent, ou elles peuvent encore décider d'utiliser des collections de données privées pour éviter que l'intégralité des données de transaction ne soient stockées dans le service de tri.

Créer un canal avec tous ses composants dans un seul pays garantit que l'ensemble des données résident au sein d'une région, y compris les paires clé-valeur, les journal des transactions de blockchain, ainsi que les hachages de données privées. Toutefois, cette option implique la gestion supplémentaire d'un nouveau canal et les coûts associés à la gestion du service de tri.

## Matériel de référence
{: #console-icp-about-data-residency-reference}

Pour une meilleure compréhension du flux de données sur le réseau de {{site.data.keyword.blockchainfull_notm}} Platform, voir la [documentation Fabric sur le flux de transactions](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

A l'avenir, Zero Knowledge Proof améliorera la capacité à réaliser d'autres hébergements de données dans Hyperledger Fabric. Une preuve ZKP (Zero-Knowledge Proof) permet à un “approbateur” de garantir à un “vérificateur” qu'ils ont la connaissance d'une valeur confidentielle sans révéler la valeur confidentielle elle-même. Il s'agit d'un moyen de montrer que vous savez quelque chose qui répond à une instruction sans afficher ce que vous savez.

Vous trouverez davantage d'informations sur les collections de données privées et Zero Knowledge Proof sans le livre blanc [Private and confidential transactions with Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.
