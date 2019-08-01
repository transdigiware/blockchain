---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Traitement des incidents
{: #ibp-v2-troubleshooting}

Des problèmes généraux peuvent se produire lorsque vous utilisez la console pour gérer des noeuds, des canaux ou des contrats intelligents. Dans de nombreux cas, ces problèmes peuvent être résolus en quelques opérations simples.
{:shortdesc}

Cette rubrique décrit les problèmes courants qui peuvent se présenter lors de l'utilisation de la console {{site.data.keyword.blockchainfull_notm}} Platform.

- [Lorsque je survole mon noeud, le statut est `Statut non disponible`. Qu'est-ce que cela signifie ?](#ibp-v2-troubleshooting-status-unavailable)
- [Lorsque je survole mon noeud, le statut est `Statut non détectable`. Qu'est-ce que cela signifie ?](#ibp-v2-troubleshooting-status-undetectable)
- [Pourquoi mes opérations de noeud échouent après la création d'un homologue ou d'un service de tri ?](#ibp-console-build-network-troubleshoot-entry1)
- [Pourquoi est-ce que je reçois le message d'erreur `Impossible d'obtenir le canal système` lorsque j'ouvre mon service de tri ?](#ibp-troubleshoot-ordering-service)
- [Pourquoi mon homologue ne parvient pas à démarrer ?](#ibp-console-build-network-troubleshoot-entry2)
- [Pourquoi ne puis-je pas installer, instancier ou mettre à niveau mes contrats intelligents ?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Pourquoi est-ce que le contrat intelligent que j'ai installé sur l'homologue n'apparaît pas dans l'interface utilisateur ?](#ibp-console-build-network-troubleshoot-missing-sc)
- [Comment puis-je consulter les journaux de mon conteneur de contrat intelligent ?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Mon canal, mes contrats intelligents et mes identités ont disparu de la console. Comment puis-je les récupérer ?](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Pourquoi est-ce que je reçois le message d'erreur `Impossible d'effectuer l'authentification avec l'ID et le secret d'inscription que vous avez fournis` lorsque je crée une nouvelle définition MSP d'organisation ?](#ibp-v2-troubleshooting-create-msp)
- [Pourquoi est-ce que je reçois le message d'erreur `Une erreur s'est produite lors de la mise à jour du canal` lorsque j'essaie d'ajouter une organisation à mon canal ?](#ibp-v2-troubleshooting-update-channel)
- [Mon cluster {{site.data.keyword.cloud_notm}} Kubernetes est arrivé à expiration. Qu'est-ce que cela signifie ?](#ibp-v2-troubleshooting-cluster-expired)
- [Pourquoi les transactions que je soumets depuis le code VS échouent ?](#ibp-v2-troubleshooting-anchor-peer)
- [Lorsque je me connecte à ma console, pourquoi est-ce que je reçois le message d'erreur 401 unauthorized ?](#ibp-v2-troubleshooting-console-401)
- [Pourquoi les noeuds que j'ai déployés dans {{site.data.keyword.cloud_notm}} Private ne traitent pas les transactions et échouent aux diagnostics d'intégrité ?](#ibp-v2-troubleshooting-healthchecks)
- [Pourquoi mes transactions retournent-elles une erreur de règle d'adhésion : signature set did not satisfy policy ?](#ibp-v2-troubleshooting-endorsement-sig-failure)

## Lorsque je survole mon noeud, l'état est `Statut non disponible`. Qu'est-ce que cela signifie ?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

Le statut de noeud dans la vignette du noeud de l'autorité de certification, de l'homologue ou du noeud de tri est grisé, ce qui signifie que le statut du noeud est non disponible. Dans l'idéal, lorsque vous survolez un noeud, l'état doit être `En fonctionnement`.
{: tsSymptoms}

Ce problème peut se produire si le noeud a récemment été créé et que le processus de déploiement n'est pas terminé. Si le noeud est une autorité de certification, il est probable que le noeud n'est pas en cours d'exécution.
Si le noeud est un homologue ou un noeud de tri, cette condition se produit lorsque le programme de diagnostic d'intégrité qui s'exécute sur l'homologue ou le noeud de tri ne parvient pas à contacter le noeud.  La demande de statut peut échouer avec une erreur de délai d'attente car le noeud n'a pas répondu dans un délai spécifique, le noeud est arrêté ou la connectivité du réseau est hors service.
{: tsCauses}

S'il s'agit d'un nouveau noeud, attendez quelques minutes que le déploiement se termine. Si le noeud n'est pas nouveau,
[examinez les journaux noeud associés](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) pour rechercher permettant d'en déterminer la cause.
{: tsResolve}

## Lorsque je survole mon noeud, le statut est `Statut non détectable`. Qu'est-ce que cela signifie ?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

Le statut de noeud dans la vignette de l'homologue ou noeud de tri est jaune, ce qui signifie que le statut du noeud ne peut pas être détecté. Dans l'idéal, lorsque vous survolez un noeud, l'état doit être `En fonctionnement`.
{: tsSymptoms}

Cette condition se produit uniquement sur les noeuds d'homologue et de tri qui ont été *importés* sur la console et si le programme de diagnostic d'intégrité ne peut pas s'exécuter sur le noeud. Ce statut existe car aucun élément `operations_url` n'a été spécifié lorsque le noeud a été importé. Une URL d'opérations est requise pour que le programme de diagnostic d'intégrité puisse s'exécuter. Le noeud lui-même est probablement `en cours d'exécution`, mais comme aucune URL d'opérations n'a été spécifiée, son statut ne peut pas être déterminé.
{: tsCauses}

Vous pouvez résoudre ce problème en suivant les étapes ci-après :
 1. Cliquez sur la vignette de noeud afin de l'ouvrir.
 2. Cliquez sur l'icône **Paramètres**.
 3. Cliquez sur **Associer une identité**, consultez et notez l'identité qui est associée à ce noeud. Cliquez sur **Annuler** pour refermer cet écran.
 4. Cliquez sur **Exporter** pour générer un fichier `JSON` pour le noeud.
 5. Editez le fichier `JSON` généré et entre l'URL `operations_url` pour le noeud. La valeur de `operations_url` dépend de la manière dont l'YRL a été configurée ainsi que de divers paramètre réseau. Cette valeur doit être fournie par l'administrateur de réseau qui a déployé le noeud.
 6. Cliquez sur **Supprimer**. Cette étape supprime le noeud importé à partir de la console, mais ne supprime pas le noeud réel.
 7. Depuis l'onglet **Noeuds**, cliquez sur **Ajouter un homologue** ou **Ajouter un noeud de tri** suivi de **Importer un homologue existant** ou **Importer un service de tri existant**.
 8. Cliquez sur **Télécharger JSON** et recherchez le fichier JSON que vous venez d'éditer. Cliquez sur **Suivant**.
 9. Associez l'identité que vous avez notée à l'étape 3.
 10. Cliquez sur **Ajouter un homologue** ou **Ajouter un noeud de tri**.

Le programme de diagnostic d'intégrité peut maintenant s'exécuter sur le noeud et indiquer le statut du noeud.
{: tsResolve}

## Pourquoi mes opérations de noeud échouent après la création d'un homologue ou d'un service de tri ?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

Il est possible que vous rencontriez une erreur lors de la gestion d'un noeud existant. Lorsque cela arrive, il est souvent utile de consulter les journaux de ce noeud.  

Par exemple, lorsque vous essayez d'exploiter le noeud, l'action peut échouer.
{: tsSymptoms}

Après la création d'un homologue ou d'un noeud de tri, selon la configuration de stockage de cluster, quelques minutes peuvent être nécessaires pour que le noeud soit opérationnel.
{: tsCauses}

Consultez votre tableau de bord Kubernetes et vérifiez que l'homologue ou le noeud est à l'état `En cours d'exécution`. Renouvelez ensuite votre opération. Si vous rencontrez encore des problèmes une fois le noeud opérationnel, [recherchez d'éventuelles erreurs dans les journaux de ce noeud](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs).  
{: tsResolve}

## Pourquoi est-ce que je reçois le message d'erreur `Impossible d'obtenir le canal système` lorsque j'ouvre mon service de tri ?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

Une fois que vous avez créé un service de tri sur votre console {{site.data.keyword.cloud_notm}} Private, le statut est `En cours d'exécution`. Cependant, lorsque vous ouvrez le service de tri, vous obtenez l'erreur :

```
Impossible d'obtenir le canal système. Si vous avez associé une identité sans privilège d'administration sur le noeud de service de tri, vous ne pourrez pas afficher ou gérer les détails de service de tri.
```

{: tsSymptoms}

Cette condition se produit sur la console de blockchain qui s'exécute dans {{site.data.keyword.cloud_notm}} Private. Dans les navigateurs autre que Chrome, vous devez accepter un certificat pour que la console puisse communiquer avec le noeud.
{: tsCauses}

Il existe plusieurs façons de résoudre ce problème :
1. Dans les notes sur l'édition de votre charte Helm, où est fournie l'URL de navigateur de votre console, figure également une note concernant l'accès à une URL pour accepter le certificat. Accédez à cette URL et acceptez le certificat. Ouvrez ensuite votre service de tri. L'erreur ne se produit plus.
2. Si vous utilisez un navigateur Chrome, ouvrez votre console de blockchain dans Chrome et ouvrez votre service de tri. L'erreur ne se produit pas. Notez que vous devrez exporter vos identités depuis votre portefeuille de console dans le navigateur non Chrome puis les importer dans le portefeuille dans le navigateur Chrome pour que tout puisse continuer à fonctionner.
{: tsResolve}

## Pourquoi mon homologue ne parvient pas à démarrer ?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

Plusieurs cas de figure sont possibles pour cette erreur.

Le journal de l'homologue comporte `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
{: tsSymptoms}

- Cette erreur peut survenir dans les conditions suivantes :
  - Lors de la création de la définition MSP de l'organisation de l'homologue ou du noeud de tri, vous avez indiqué un ID et un secret d'inscription qui correspondent à un type d'identité `homologue` et non `client`. Or le type doit être `client`.
  - Lors de la création de la définition MSP de l'organisation de l'homologue ou du noeud de tri, vous avez indiqué un ID et un secret d'inscription qui ne correspondent pas à l'ID et au secret d'inscription de l'identité admin de l'organisation correspondante.
  - Lors de la création de l'homologue ou du noeud de tri, vous avez indiqué l'ID et le secret d'inscription d'une identité qui n'est pas de type 'homologue'.

- Ouvrez le noeud de l'autorité de certification de votre homologue ou noeud de tri et consultez les identités enregistrées figurant dans le tableau **Utilisateurs enregistrés**.
- Supprimez l'homologue ou le noeud de tri et recréez-le, en veillant à indiquer l'ID et le secret d'inscription corrects.
- Notez qu'avant de créer l'homologue ou le noeud de tri, vous devez créer un ID admin organisation de type 'client'. Veillez à indiquer le même ID que l'ID d'inscription lors de la création de la définition MSP de l'organisation. Consultez les instructions relatives à l'[enregistrement des identités d'homologue](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) et celles relatives à l'[enregistrement des identités de service de tri](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## Pourquoi ne puis-je pas installer, instancier ou mettre à niveau mes contrats intelligents ?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

Il est possible que vous rencontriez une erreur lors de l'installation, de l'instanciation ou de la mise à niveau d'un contrat intelligent.  Par exemple, lorsque vous essayez d'installer un contrat intelligent sur un homologue, l'opération peut échouer avec le message d'erreur `Une erreur s'est produite lors de l'installation du contrat intelligent sur l'homologue.`
{: tsSymptoms}

Vous pouvez recevoir cette erreur si cette version de contrat intelligent existe déjà sur l'homologue, ou si votre homologue n'a plus de ressources.
{: tsCauses}

- Ouvrez votre tableau de bord Kubernetes et vérifiez que l'homologue est à l'état `En cours d'exécution`.  
- Ouvrez le noeud homologue et vérifiez que la version du contrat intelligent n'existe pas déjà sur l'homologue, puis relancez l'opération avec la version appropriée.
- Si vous rencontrez encore des problèmes une fois le noeud opérationnel, [recherchez d'éventuelles erreurs dans les journaux de ce noeud](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs).  
{: tsResolve}

## Pourquoi est-ce que le contrat intelligent que j'ai installé sur l'homologue n'apparaît pas dans l'interface utilisateur ?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

Un contrat intelligent a été installé sur un homologue mais lorsque vous cliquez sur l'onglet **Contrats intelligent**, il n'est pas répertorié.
{: tsSymptoms}

Ce problème peut se produire lorsqu'un autre utilisateur ou une autre application installe le contrat intelligent sur l'homologue et que l'identité admin de l'homologue d'administration ne se trouve pas dans le portefeuille de votre navigateur.
{: tsCauses}

Pour afficher les contrats intelligents installés sur un homologue, vous devez être un administrateur de l'homologue. Vérifiez que l'identité admin de l'homologue existe dans le portefeuille de votre navigateur. Si ce n'est pas le cas, vous devez l'importer dans le portefeuille de votre console.
{: tsResolve}

## Comment puis-je consulter les journaux de mon conteneur de contrat intelligent ?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

Vous devrez peut-être consulter les journaux de votre conteneur de contrat intelligent, ou de code blockchain, pour déboguer un problème de contrat intelligent.
{: tsSymptoms}

Suivant les instructions relatives à l'affichage des journaux de votre conteneur de contrat intelligent dans :
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs).
{: tsResolve}

## Mon canal, mes contrats intelligents et mes identités ont disparu de la console. Comment puis-je les récupérer ?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

Les identités de votre portefeuille de console se composent d'un certificat signataire et d'une clé privée qui vous permettent de gérer les composants de votre blockchain. Cependant, ces clés sont stockées uniquement dans le stockage local de votre navigateur. Vous êtes responsable de la sécurisation et de la gestion de ces clés. Nous vous recommandons de les exporter sur votre système de fichiers après leur création. Chaque fois que vous créez un nouveau noeud, vous associez une identité de votre portefeuille de console à ce noeud. Cette identité admin est celle qui vous permet de gérer le noeud. Lorsque vous changez de navigateur ou lorsque vous passez à un navigateur sur un autre ordinateur, ces identités ne sont plus dans votre portefeuille. Par conséquent, vous ne pouvez pas gérer les composants.
{: tsSymptoms}

Dans la nouvelle version d'{{site.data.keyword.blockchainfull_notm}} Platform, vous êtes désormais responsable de la sécurisation et de la gestion de vos certificats. Par conséquent, ces derniers sont uniquement conservés dans le stockage local du navigateur pour vous permettre de gérer le composant. Si vous utilisez un navigateur privé et que vous passez à une autre fenêtre de navigateur ou dans une fenêtre de navigateur non privée, les identités que vous avez créées seront supprimé de votre portefeuille de console dans la nouvelle session de navigateur. Par conséquent, il est nécessaire d'exporter les identités depuis le portefeuille de console dans votre session de navigateur privée pour votre système de fichiers. Vous pouvez ensuite les importer dans votre session de navigateur non privée si nécessaire. Sinon, il n'est pas possible de les récupérer.
{: tsCauses}

- Chaque fois que vous créez une nouvelle définition MSP d'organisation, vous générez des clés pour une identité qui est autorisée à administrer l'organisation. Par conséquent, pendant ce processus vous devez cliquer sur les boutons **Générer** et **Exporter** pour stocker l'identité générée dans votre portefeuille de console, puis la sauvegarder sur votre système de fichiers dans un fichier JSON.
- Pour résoudre ce problème dans votre navigateur, vous devez importer ces identités et les associer au noeud correspondant :
  - Dans le navigateur concerné par ce problème, cliquez sur l'onglet **Portefeuille** puis sur **Ajouter une identité** afin d'importer le fichier JSON dans votre portefeuille.
  - Cliquez sur **Télécharger JSON** et recherchez le fichier JSON que vous avez exporté à l'aide du bouton **Ajouter des fichiers**.
  - Cliquez sur **Soumettre**.
  - Ouvrez ensuite le noeud de l'homologue ou de tri auquel cette identité a été initialement associée, puis cliquez sur l'icône **Paramètres**.
  - Cliquez sur le bouton **Associer une identité**.
  - Sélectionnez dans la liste déroulante l'identité que vous venez d'importer dans votre portefeuille de console.
  - Cliquez sur **Associer**.
- Recommencez ce processus pour chaque identité figurant dans le portefeuille du navigateur d'origine.
{: tsResolve}

## Pourquoi est-ce que je reçois le message d'erreur `Impossible d'effectuer l'authentification avec l'ID et le secret d'inscription que vous avez fournis` lorsque je crée une nouvelle définition MSP d'organisation ?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

Lorsque vous essayez de créer une la définition MSP d'une nouvelle organisation depuis l'onglet Organisations, vous recevez le message d'erreur `Impossible d'effectuer l'authentification avec l'ID et le secret d'inscription que vous avez fournis`.
{: tsSymptoms}

Cette erreur se produit lorsque la valeur que vous avez indiquée pour le secret d'inscription n'est pas valide pour l'ID d'inscription que vous avez sélectionné dans la section `Générer un certificat d'administrateur d'organisation` du panneau.
{: tsCauses}

Vérifiez que vous avez sélectionné l'ID d'inscription d'admin d'organisation correct dans la liste déroulante des ID d'inscription et entrez la valeur correcte pour le secret d'inscription.
{: tsResolve}

## Pourquoi est-ce que je reçois le message d'erreur `Une erreur s'est produite lors de la mise à jour du canal` lorsque j'essaie d'ajouter une organisation à mon canal ?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

Lorsque vous essayez d'ajouter une autre organisation à un canal, la mise à jour échoue avec le message `Une erreur s'est produite lors de la mise à jour du canal`.
{: tsSymptoms}

Cette erreur se produit lorsque l'**ID MSP de programme de mise à jour de canal** sélectionné dans le panneau **Mettre à jour un canal** n'est pas un admin du canal.
{: tsCauses}

Dans le panneau **Mettre à jour un canal**, accédez à la section **ID MSP de programme de mise à jour de canal** et sélectionnez l'ID MSP qui a été spécifié lors de la création du canal ou indiquez l'ID MSP qui est l'admin du canal.
{: tsResolve}

## Mon cluster {{site.data.keyword.cloud_notm}} Kubernetes est arrivé à expiration. Qu'est-ce que cela signifie ?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

J'ai reçu un e-mail indiquant que mon cluster Kubernetes Service {{site.data.keyword.IBM_notm}} est sur le point d'expirer ou que son statuts est `Expiré`. Ou bien vous ne pouvez pas accéder à la console au bout de 30 jours.
{: tsSymptoms}

Les clusters Kubernetes gratuits ne sont valides que pendant 30 jours.
{: tsCauses}

Il n'est pas possible de migrer depuis un cluster gratuit vers un cluster payant. Au bout de 30 jours vous ne pouvez pas accéder à la console et tous vos noeuds et certificats sont supprimés. Pour plus d'informations sur cette opération et les actions que vous pouvez effectuer, consultez la rubrique relative à l'[expiration du cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration).
{: tsResolve}

## Pourquoi les transactions que je soumets depuis le code VS échouent ?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Les transactions soumises depuis le code VS échouent avec une erreur semblable à ce qui suit :
```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```
{: tsSymptoms}

Cette erreur se produit si vous utilisez la fonction Reconnaissance de service Fabric et que vous n'avez configuré aucun homologue d'ancrage sur votre canal.
{: tsCauses}

Suivez l'étape 3 de la [rubrique relative aux données privées](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) dans le tutoriel Déployer un contrat intelligent pour configurer vos homologues d'ancrage.

## Lorsque je me connecte à ma console, pourquoi est-ce que je reçois le message d'erreur 401 Unauthorized ?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

Lorsque j'essaie de me connecter à ma console, je ne parviens pas à accéder à cette console à partir de mon navigateur. Si je consulte les journaux du navigateur, je trouve l'erreur 401 Unauthorized.
{: tsSymptoms}

Le délai d'attente de session de la console du navigateur expire au bout de **8 heures** d'inactivité. Si une session devient inactive, la console empêchera un utilisateur inactif d'effectuer des actions.
{: tsCauses}

Si votre session est devenue inactive, vous pouvez simplement essayer d'actualiser votre navigateur. Si cela ne fonctionne pas, fermez le navigateur, y compris **tous** les onglets et toutes les fenêtres. Ouvrez de nouveau l'URL. Vous devrez vous connecter.

Dans le cadre des meilleures pratique, vous devez déjà avoir enregistré vos certificats et identités sur votre système de fichiers. S'il arrive que vous utilisiez une fenêtre en mode incognito, tous les certificats sont supprimés de la mémoire locale du navigateur lorsque vous fermez ce dernier. Après vous être reconnecté, vous devez réimporter vos identités et certificats.
{: note}

## Pourquoi les noeuds que j'ai déployés dans {{site.data.keyword.cloud_notm}} Private ne traitent pas les transactions et échouent aux diagnostics d'intégrité ?
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

Ma console indique que mes homologues et noeuds de tri sont toujours en cours d'exécution. Toutefois, mes transactions échouent. Lorsque j'exécute un contrôle de continuité ou de préparation sur les états de contrôle, les contrôles indiquent que les pods sont défaillants.
{: tsSymptoms}

Si vous avez souvent déployé, retiré et mis à niveau des noeuds dans votre cluster, peut-être dans le cadre d'un résultat de test, Docker peut être défaillant en raison d'un problème connu avec {{site.data.keyword.cloud_notm}} Private. Pour plus d'informations, consultez la documentation [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}.
{: tsCauses}

Pour résoudre ce problème, retirez les pods défaillants et déployez de nouveau vos noeuds. Vous pouvez également redémarrer le service Docker dans le cluster.

## Pourquoi mes transactions retournent-elles une erreur de règle d'adhésion : signature set did not satisfy policy ?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

Lorsque j'appelle un contrat intelligent pour la soumission d'une transaction, la transaction renvoie l'erreur de règle d'adhésion suivante :
```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```
{: tsSymptoms}

Si vous avez récemment rejoint un canal et installé le contrat intelligent, cette erreur se produit si vous n'avez pas ajouté votre organisation à la règle d'adhésion. Comme votre organisation ne figure pas dans la liste des organisations qui peuvent approuver une transaction du contrat intelligente, l'approbation de vos homologues est rejetée par le canal. Si vous rencontrez ce problème, vous pouvez modifier la règle d'adhésion par la mise à niveau du contrat intelligent. Pour plus d'informations, voir [Indication d'une règle d'adhésion](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) and [Upgrading a smart contract](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}
