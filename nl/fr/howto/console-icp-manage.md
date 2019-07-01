---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

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
{:curl: #curl .ph data-hd-programlang='curl'}

# Administration de votre console
{: #console-icp-manage}

Une fois que vous avez déployé votre console dans {{site.data.keyword.cloud}} Private, vous pouvez l'utiliser pour ajouter ou retirer des utilisateurs de la console, ainsi que pour accéder aux API qui vous permettent d'exploiter votre réseau et de gouverner la console. Vous pouvez également accéder aux journaux de la console et les personnaliser.
{:shortdesc}

## Gestion des utilisateurs depuis la console
{: #console-icp-manage-users}

L'utilisateur qui met à disposition la console {{site.data.keyword.blockchainfull_notm}} Platform est considéré comme l'administrateur de console. Cet administrateur peut ensuite ajouter d'autres utilisateurs et leur accorder l'accès à la console à partir de l'onglet **Utilisateurs** sur la console. Chaque utilisateur qui accède à la console doit se voir attribuer une règle d'accès pour laquelle un rôle utilisateur est défini. La règle détermine les actions que l'utilisateur peut effectuer sur la console. Par défaut, l'administrateur de console se voit accorder le rôle **Gestionnaire** pour la console. D'autres utilisateurs peuvent se voir accorder les rôles **Gestionnaire**, **Auteur** ou **Lecteur** lorsqu'un gestionnaire de console les ajoute à la console. Notez que les utilisateurs peuvent aussi être gérés avec des [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis).

| Rôle | Fonctions |
|--------|----------|
| Gestionnaire | En tant que Gestionnaire, vos droits vont au-delà de ceux du rôle Auteur. Vous pouvez faire tout ce que peut faire un Lecteur et un Auteur ainsi que les opérations suivantes : <ul><li>Mettre à disposition de nouveaux composants à l'aide de la console ou d'API.</li><li>Supprimer des composants mis à disposition à l'aide de la console ou d'API.</li><li>Ajouter/Retirer des utilisateurs et modifier les règles d'accès utilisateur.</li><li>Modifier les niveaux de consignation de la console à l'aide de la console ou d'API.</li><li>Redémarrer la console à l'aide d'une API.</li></ul> |
| Auteur | En tant que Auteur, vos droits vont au-delà de ceux du rôle Lecteur, notamment : <ul><li>Importer des composants à l'aide de la console ou d'API.</li><li>Supprimer des composants importés à l'aide de la console ou d'API.</li><li>Enregistrer des utilisateurs auprès d'une autorité de certification.</li><li> Ajouter ou supprimer des notifications à l'aide de la console ou d'API.</li></ul>  |
| Lecteur | En tant que Lecteur, vous pouvez effectuer des actions en lecture seule, et notamment : <ul><li>Afficher l'interface utilisateur de la console.</li><li>Consulter le journal de la console.</li><li>Exporter des composants.</li><li>Emettre une API GET.</li></ul> | |

Les droits d'accès sont cumulatifs. Si vous choisissez un rôle **Gestionnaire**, l'utilisateur pourra également effectuer toutes les actions des rôles **Auteur** et **Lecteur**, et vous n'avez donc pas à sélectionner également ces rôles. De même, un utilisateur avec le rôle **Auteur** pourra effectuer toutes les actions du rôle **Lecteur**.

### Gestion du mot de passe d'un utilisateur
{: #console-icp-manage-user-pw}

Le mot de passe qui a été stocké dans le [secret de mot de passe](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) puis transmis à la console durant la configuration devient le mot de passe par défaut pour la console. Tous les utilisateurs doivent utiliser ce mot de passe lorsqu'ils se connectent à la connexion pour la première fois. Le gestionnaire de console peut également de définir un nouveau mot de passe par défaut. Sous l'onglet **Utilisateurs** de la console, cliquez sur le lien **Mettre à jour la configuration** sous le nom de la vignette de votre service d'authentification. Dans le panneau de droite, vous pouvez afficher ou mettre à jour le mot de passe par défaut pour les nouveaux utilisateurs de la console.

Vous devez partager ce mot de passe, ou le mot de passe par défaut que vous avez réinitialisé, avec les utilisateurs de sorte qu'ils puissent se connecter à la console. Ils devront modifier le mot de passe lors de leur première connexion.
{:note}

Les utilisateurs dotés du rôle de gestionnaire peuvent réinitialiser les mots de passe des autres utilisateurs. Sous l'onglet **Utilisateurs** de la console, cliquez sur les trois points verticaux en fin de ligne d'un utilisateur donné, puis cliquez sur **Réinitialiser le mot de passe**. La console réinitialisera le mot de passe de cet utilisateur sur le mot de passe par défaut, que vous pouvez vérifier et confirmer dans le panneau de droite. Les utilisateurs, quel que soit leur rôle, peuvent modifier leur propre mot de passe à tout moment. Cliquez sur l'icône d'avatar dans l'angle supérieur droit, puis cliquez sur **Modifier le mot de passe**.

Après que vous avez ajouté de nouveaux utilisateurs à la console, il se peut que ceux-ci ne puissent pas voir l'ensemble des noeuds, canaux ou codes blockchain, déployés par d'autres utilisateurs. Pour pouvoir gérer ces composants, chaque utilisateur doit importer les identités associées dans son propre portefeuille de console. Pour plus d'informations, voir [Stockage des identités dans votre portefeuille de console](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modification du rôle d'un utilisateur
{: #console-icp-manage-reset-user-pw}

Un utilisateur doté du rôle de gestionnaire peut mettre à jour les rôles des autres utilisateurs de la console et leur accorder l'accès à plus ou moins de fonctions de la console. Sous l'onglet **Utilisateurs** de la console, cliquez sur les trois points verticaux en fin de ligne d'un utilisateur donné, puis cliquez sur **Mettre à jour un utilisateur authentifié**. Sélectionnez le nouveau rôle de l'utilisateur dans le panneau de droite. Le rôle de cet utilisateur sera mis à jour lors de sa prochaine connexion à la console.

### Suppression d'un utilisateur de la console
{: #console-icp-manage-icp-remove-user}

Si vous êtes un utilisateur doté du rôle de gestionnaire, vous pouvez retirer l'accès d'un utilisateur à la console. Sous l'onglet **Utilisateurs** de la console, cliquez sur la case au début de la ligne d'un utilisateur spécifique. Ensuite, cliquez sur l'icône **Corbeille** dans la partie supérieure du tableau, en regard du bouton **Ajouter de nouveaux utilisateurs**. Une fois l'utilisateur retiré du tableau, il ne peut plus se connecter à la console.

## Utilisation des API {{site.data.keyword.blockchainfull_notm}}
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform expose des API RESTful qui vous permettent d'importer, d'éditer et de retirer des noeuds de blockchain de votre console. Vous pouvez également utiliser des API pour gérer les paramètres, la consignation et les notifications de votre console. Utilisez les instructions ci-dessous pour utiliser les API api fournies par une console déployée dans {{site.data.keyword.cloud_notm}} Private.

### Prérequis
{: #console-icp-manage-prereqs}

Pour pouvoir utiliser les API, vous devrez collecter les informations suivantes :

- **Noeud final de l'API** de la console. Il s'agit de l'URL que vous utilisez pour accéder à la console à l'aide de votre navigateur. Vous trouverez cette URL dans la section **Notes :** de l'écran de présentation de l'édition Helm.
- Nom d'utilisateur et mot de passe que vous pouvez utiliser pour l'accès à la console. Pour créer une clé d'API, vous devez disposer d'un compte avec le [rôle de gestionnaire](#console-icp-manage-users).

### Utilisation d'une clé d'API
{: #console-icp-manage-create-api-key}

Chaque console fournit sa propre identité et gestion des accès. Vous pouvez utiliser le nom d'utilisateur et le mot de passe de votre console pour générer une clé d'API et un secret qui peuvent autoriser vos appels d'API.

Chaque clé d'API est associée à un rôle qui régit les fonctions que l'utilisateur est autorisé à effectuer. Les clés d'API utilisent les même rôles de politique d'accès que ceux qui existent pour les utilisateurs qui se connectent à la console à l'aide d'un nom d'utilisateur et d'un mot de passe. Reportez-vous au tableau de la section [Gestion des utilisateurs](#console-icp-manage-users) pour voir la liste des actions que chaque rôle est autorisé à effectuer. Comme seuls les rôles de gestionnaire peuvent créer des clés d'API, un administrateur de console doit créer des clés d'API pour les utilisateurs ayant le rôle de lecteur et d'auteur. Les clés d'API n'expirent jamais. En conséquence, l¨'administrateur de console doit supprimer les clés d'API qui ne sont pas utilisées.

Trois API sont disponibles pour la gestion de vos clés d'API :
- [Création d'une clé d'API](#console-icp-manage-create-api-key)
- [Affichage des clés d'API](#console-icp-manage-view-api-keys)
- [Suppression de clés d'API](#console-icp-manage-delete-api-keys)

Utilisez la demande ci-dessous pour créer une clé et un secret d'API. Vous pouvez ensuite utiliser cette clé et le secret pour les autres API. Le secret d'API n'est pas stocké par la console et il doit être sauvegardé par l'utilisateur.

#### Création d'une clé d'API
{: #console-icp-manage-create-api-key-api}

| **Demande** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Zones du corps de la demande** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Au moins un valeur est obligatoire </li><li>`string` facultatif</li></ul>|
| **Zones du corps de la réponse** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Sauvegarder : la clé n'est pas stockée</li><li>`["<role>"]`</li></ul>|
| Autorisation requise | Gestionnaire |

#### Exemple de demande curl : Création d'une clé d'API
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### Gestion des clés d'API de votre console
{: #console-icp-manage-api-keys}

Une fois que vous avez créé une clé d'API et un secret, vous pouvez utiliser les API pour afficher ou retirer les clés que vous avez créées. Les clés et secrets n'expirent pas tant qu'ils ne sont pas supprimés.

#### Affichage des clés d'API
{: #console-icp-manage-view-api-keys}

| **Demande** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Zones du corps de la réponse** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` horodatage Unix en millisecondes</li><li>`string`</li></ul>|
| Autorisation requise | lecteur |

#### Exemple de demande curl : affichage des clés d'API
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### Suppression de clés d'API
{: #console-icp-manage-delete-api-keys}

| **Demande** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| Autorisation requise| Gestionnaire |

#### Exemple de demande curl : suppression d'une clé d'API
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### Gestion des utilisateurs à l'aide des API
{: #console-icp-manage-users-apis}


Vous pouvez également utiliser les API pour afficher la liste des utilisateurs, ajouter ou retirer des utilisateurs qui peuvent se connecter à la console ou accéder aux API de la console. Vous pouvez également modifier les rôles des utilisateurs pour mettre à niveau le niveau d'accès de ces utilisateurs. Les API suivantes sont disponibles :

- [Afficher la liste des utilisateurs](#console-icp-manage-list-users-api)
- [Editer des utilisateurs](#console-icp-manage-edit-users-api)
- [Ajouter des utilisateurs](#console-icp-manage-add-users-api)
- [Retirer des utilisateurs](#console-icp-manage-remove-users-api)

Pour les API qui régissent les utilisateurs et les paramètres de votre console, et gèrent les noeuds de votre réseau, vous devez ajouter un indicateur ``-K`` ou ``--insecure`` pour ignorer l'exigence d'un certificat TLS. Vous pouvez également télécharger le certificat TLS à partir de votre console à l'aide de votre navigateur.

#### Afficher la liste des utilisateurs
{: #console-icp-manage-list-users-api}


| **Demande** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **Zones du corps de la réponse** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` user id</li><li>`string` adresse e-mail</li><li>`["<role>"]`</li><li>`number` horodatage Unix en millisecondes</li></ul>|
| Autorisation requise | lecteur |

#### Exemple de demande curl : affichage de la liste des utilisateurs
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### Editer des utilisateurs
{: #console-icp-manage-edit-users-api}

| **Demande** |  |
|-------------|-----------|
| Path | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **Zones du corps de la demande** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` user id </li><li>`["reader", "writer", "manager"]` Au moins un valeur est obligatoire</li></ul> |
| **Zones du corps de la réponse** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` user id</li></ul>|
| Autorisation| Gestionnaire |

#### Exemple de demande curl : édition d'un utilisateur
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### Ajouter des utilisateurs
{: #console-icp-manage-add-users-api}

| **Demande** |  |
|-------------|-----------|
| Path | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **Zones du corps de la demande** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Au moins un valeur est obligatoire </li><li>`string` facultatif</li></ul>|
| Autorisation | Gestionnaire |

#### Exemple de demande curl : ajout d'un utilisateur
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### Retirer des utilisateurs
{: #console-icp-manage-remove-users-api}

| **Demande** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **Zones du corps de la demande** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` user id</li></ul>|
| **Zones du corps de la réponse** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` user id</li></ul>|
| Autorisation | Gestionnaire |

#### Exemple de demande curl : retrait d'un utilisateur
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### Utilisation d'API pour gérer vos composants

Vous pouvez afficher l'ensemble complet des API qui sont disponibles dans la référence d'API d'[{{site.data.keyword.blockchainfull_notm}} Platform](https://test.cloud.ibm.com/apidocs/blockchain).

Comme vous utilisez des API pour communiquer avec votre console sur {{site.data.keyword.cloud_notm}} Private, vous devez utiliser l'autorisation fournie par {{site.data.keyword.cloud_notm}} avec l'authentification fournie par votre console. Remplacez ``Bearer Auth`` dans la référence d'API par ``-u <api_key>:<api_secret>``. Vous devez également ajouter un indicateur ``-K`` ou ``--insecure`` à la commande, ou télécharger le certificat TLS de console TLS à l'aide de votre navigateur. Vous ne pouvez pas utiliser l'onglet **Essayez** pour tester les API des réseaux sur {{site.data.keyword.cloud_notm}} Private.

Par exemple, l'appel d'API ci-dessous va renvoyer des informations sur l'ensemble de vos composants qui s'exécutent sur une instance de service d'{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
The same API call would resemble the request below for a console deployed on {{site.data.keyword.cloud_notm}} Private.
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

Vous pouvez utiliser les API pour créer des noeuds dans le cluster où votre console est déployée, et importer des noeuds d'autres clusters ou {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [Génération d'un réseau à l'aide d'API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) et [Importation d'un réseau à l'aide d'API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Consultation des journaux
{: #icp-console-manage-logs}

Lors de l'utilisation de la console {{site.data.keyword.blockchainfull_notm}} Platform, vous devrez peut-être consulter les journaux afin de déboguer un problème.

### Consultation des journaux de la console
{: #console-icp-manage-console-logs}

Vous pouvez facilement accéder aux journaux de la console si vous avez besoin de déboguer des problèmes que vous rencontrez lors de l'utilisation de la console ou de l'exploitation de vos noeuds. Vous pouvez également définir le niveau de consignation afin d'augmenter ou de diminuer le nombre de journaux collectés par la console. Les journaux de la console sont collectés séparément des [journaux de noeud](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs), lesquels sont collectés par {{site.data.keyword.cloud_notm}} Private.

Accédez à l'onglet **Paramètres** dans le navigateur de la console pour modifier les paramètres de journalisation. Les journaux de la console sont collectés à partir de deux sources distinctes :

  * **Journalisation client :** Ces journaux sont collectés lorsque des commandes sont envoyées depuis votre navigateur à la console.
  * **Journalisation serveur :** Ces journaux sont collectés lorsque la console envoie des commandes à vos noeuds et depuis le déploiement de la console. Ils incluent la sortie de journalisation d'Hyperledger Fabric.

Définissez le nombre de journaux collectés dans la liste déroulante sous chaque type de journal. Par exemple, **Erreur** et **Avertissement** correspondent à la collecte de journaux la moins élevée, tandis que **Débogage** et **Tout** correspondent à la collecte de journaux la plus élevée.

Vous pouvez afficher uniquement les journaux de la console si vous êtes connecté en tant qu'administrateur de console. Pour afficher les journaux depuis l'onglet **Paramètres**, remplacez le mot `settings` dans l'URL du navigateur par `api/v1/logs`. Par exemple, si l'URL de votre console est `localhost:3001/settings`, vous pouvez consulter vos journaux en accédant à l'adresse `localhost:3001/api/v1/logs`. Les journaux client et serveur sont collectés dans des fichiers distincts. Les journaux les plus récents figurent en haut de la page.

### Consultation des journaux de votre noeud
{: #console-icp-manage-node-logs}

Les journaux de composant peuvent être affichés à partir de la ligne de commande à l'aide de [commandes de l'interface CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} ou avec [Kibana](https://www.elastic.co/products/kibana){: external}, qui est inclus dans votre cluster {{site.data.keyword.cloud_notm}} Private.

- Utilisez la commande `kubectl logs` pour afficher les journaux de conteneur dans le pod. Suivez les instructions pour [installer l'interface CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} si ce n'est déjà fait. Si vous avez des doutes quant à votre nom de pod, exécutez la commande suivante pour afficher la liste des pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Ensuite, exécutez la commande suivante pour extraire les journaux pour le conteneur de noeud qui réside dans le pod :

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Remplacez `<pod_name>` par le nom de votre pod du résultat de la commande ci-dessus.  
  Remplacez `<node>` par `ca`, `peer` ou `orderer` pour afficher les journaux de votre noeud.  
  Remplacez `<node>` par `fluentd` pour afficher les journaux de vos contrats intelligents.

  Pour plus d'informations sur la commande `kubectl logs`, consultez la [documentation Kubernetes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- Vous pouvez aussi [accéder aux journaux](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} depuis votre console {{site.data.keyword.cloud_notm}} Private, laquelle ouvre les journaux dans Kibana.

  **Remarque :** Lorsque vous affichez vos journaux dans Kibana, vous pouvez recevoir la réponse `No results found`. Cette condition peut se produire si {{site.data.keyword.cloud_notm}} Private utilise l'adresse IP de votre noeud worker comme nom d'hôte. Pour résoudre ce problème, supprimez le filtre qui commence par `node.hostname.keyword` en haut de l'écran et les journaux deviennent visibles.

### Consultation des journaux de votre conteneur de contrat intelligent
{: #console-icp-manage-container-logs}

Si vous rencontrez des problèmes au niveau de votre contrat intelligent, vous pouvez consulter les journaux du conteneur de contrat intelligent, ou de code blockchain, pour déboguer un problème. Vous pouvez exécuter la commande suivante pour afficher les journaux du conteneur de contrat intelligent :

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

Remplacez `<peer_pod>` par le nom du pod homologue où s'exécute le code blockchain. Utilisez la commande `kubectl get po` pour obtenir la liste des pods en cours d'exécution.

## Installation de correctifs pour vos noeuds
{: #console-icp-manage-patch}

Les images docker {{site.data.keyword.IBM_notm}} Hyperledger Fabric sous-jacentes pour les noeuds de l'homologue, de l'autorité de certification et du service de tri devront peut-être mis à jour, par exemple, avec des mises à jour de sécurité ou pour une nouvelle édition de point Fabric. Vous pouvez mettre à jour vos images Fabric lors d'une mise à niveau de la charte Helm {{site.data.keyword.blockchainfull_notm}} Platform.

Une fois la charte Helm mise à niveau, vous pourrez rechercher le texte **Correctif disponible** sur une vignette de noeud si une mise à jour de composant est disponible. Ce correctif peut être installé sur un noeud dès que vous êtes prêt. Ces correctifs sont facultatifs, mais recommandés. Vous ne pouvez pas appliquer des correctifs à des noeuds qui sont importés sur la console.

Les correctifs sont appliqués à tous les noeuds en même temps. Lorsque le correctif est appliqué, le noeud n'est pas disponible pour traiter des demandes ou des transactions. Par conséquent, pour éviter toute interruption de service, vous devez vérifier chaque fois que possible qu'un autre noeud du même type est disponible pour le traitement des demandes. L'installation de correctifs sur un noeud prend environ une minute et une fois la mise à jour terminée, le noeud est prêt à traiter des demandes.
{:note}

Pour appliquer un correctif sur un noeud, ouvrez la vignette du noeud et cliquez sur le bouton **Installer un correctif** . Vous ne pouvez pas appliquer des correctifs à des noeuds que vous avez importés sur la console.
