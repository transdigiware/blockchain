---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: logging levels, metrics, health check, peer, orderer

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

# Exploitation des noeuds avec la fonction Service d'opérations
{: #operations_service}

{{site.data.keyword.blockchainfull}} Platform repose sur Hyperledger Fabric version 1.4.1  Cette plateforme prend en charge la fonction Service d'opérations qui offre une API “opérations” RESTful permettant aux opérateurs d'effectuer des diagnostics d'intégrité de noeud, d'extraire des mesures opérationnelles des noeuds d'homologue et de service de tri et de gérer les niveaux de consignation. L'homologue et le service de tri hébergent un serveur HTTP qui offre l'API “opérations” RESTful.  Pour plus d'informations sur la fonction Service d'opérations, voir [The Operations Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html){: external}.
{:shortdesc}


## Considérations et limitations
{: #operations_service_consideration_limitation}

- Tous les noeuds d'homologue et de service de tri sont configurés avec `clientAuthRequired: false` de sorte que le programme de diagnostic d'intégrité soit accessible. Comme `clientAuthRequired` est défini sur `false` et que TLS mutuel est également activé, lorsque vous accédez au serveur REST, vous devez transmettre les identités TLS pour permettre l'authentification. Ce paramètre garantit que seuls les utilisateurs avec les clés appropriées peuvent voir les journaux correspondants.
- Seul le modèle d'extraction des métriques reposant sur [Prometheus](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#prometheus){: external} est pris en charge à l'heure actuelle.

## Avant de commencer
{: #operations_service_before_you_begin}

Vous devez collecter les informations suivantes de votre homologue et service de tri.

- **`peer-endpoint`** ou **`orderer-endpoint`**

  Vous pouvez trouver l'URL de point d'extrémité des opérations de l'homologue ou du service de tri dans le fichier JSON de ces derniers que vous exportez depuis la console.

    1. Cliquez sur l'homologue ou le service de tri sous l'onglet **Noeud**.
    2. Sur la page de l'homologue ou du service de tri, cliquez sur l'icône **Paramètres** en regard du nom de l'homologue ou du service de tri.
    3. Dans le panneau latéral, cliquez sur **Exporter** afin de sauvegarder le fichier JSON de l'homologue ou du service de tri.
    4. Recherchez l'URL de point d'extrémité des opérations de l'homologue ou du service de tri, qui est la valeur du paramètres `operations_url` dans le fichier JSON. Cette valeur est appelée `peer-endpoint` ou `orderer-endpoint` dans les commandes dans la suite de cette rubrique. Par exemple :

      ```
      {
      "short_name": "Peer1Org1_0",
      "name": "Peer1 Org1",
      "url": "https://169.46.208.93:32739",
      "type": "fabric-peer",
      "msp_id": "org1msp",
      "operations_url": "https://169.46.208.93:32101"
      }
      ```

      ```
      {
      "short_name": "Orderer_0",
      "name": "Orderer",
      "url": "https://169.46.208.93:31612",
      "type": "fabric-orderer",
      "msp_id": "orderermsp",
      "operations_url": "https://169.46.208.93:30115"
      }
      ```

- **`client-tls-cert`** et **`client-tls-key`**

  Vous pouvez trouver le certificat et la clé privée de l'autorité de certification TLS sur la console.

  1. Cliquez sur le noeud de l'autorité de certification de l'homologue ou du service de tri sous l'onglet **Noeud**.
  2. Sur la page de l'autorité de certification, cliquez sur **Autorité de certification TLS**.
  3. Dans le tableau de l'autorité de certification TLS, localisez l'utilisateur `admin` et cliquez sur les points verticaux sous **Actions**. Ensuite, cliquez sur **Inscrire une identité**.
  4. Dans le panneau latéral, cliquez sur **Suivant** afin de voir le certificate et la clé privée de l'autorité de certification TLS.
  5. Donnez à l'identité un nom d'affichage et cliquez sur **Exporter une identité** puis sur **Ajouter une identité au portefeuille** afin de sauvegarder le certificat et la clé privée de l'autorité de certification TLS dans un fichier JSON.
  6. Ouvrez le fichier JSON exporté.
  7. Recherchez la clé privée qui correspond à la valeur du paramètre `private_key` dans le fichier JSON. Il s'agit de la valeur `client-tls-key` à utiliser dans la commande ci-dessous.
  8. Recherchez le certificat de l'autorité de certification TLS qui correspond à la valeur du paramètre `cert` dans le fichier JSON. Il s'agit de la valeur `client-tls-cert` à utiliser dans la commande ci-dessous.

- **`peer tls-ca cert`** ou **`orderer tls-ca cert`**

  Vous pouvez trouver le certificat de l'autorité de certification TLS de l'homologue ou du service de tri sur la console.

  1. Cliquez sur le noeud de l'homologue ou du service de tri sous l'onglet **Noeud**.
  2. Cliquez sur l'icône **Paramètres** en regard du nom de noeud.
  3. Copiez la chaîne de certificat de l'autorité de certification TLS du noeud. Il s'agit de la valeur `peer tls-ca cert` ou `orderer tls-ca cert` qui peut être utilisée dans les commandes ci-dessous.

**Remarque :** Pour l'ensemble des certificats et clés, vous devez convertir les chaînes de certificat, qui sont au format `base64`, en fichiers `.pem` individuels à l'aide des commandes suivantes :
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > <key_name>.pem
  ```
  {:codeblock}
{:important}


## Vérification de l'intégrité du noeud
{: #operations_service_health_check}

Exécutez la commande `curl -k <peer-endpoint>/healthz` ou `curl -k <orderer-endpoint>/healthz` pour vérifier l'intégrité du noeud de l'homologue ou du service de tri. Par exemple :

```
curl -k https://169.46.208.93:3210/healthz
```
{:codeblock}

Pour plus d'informations sur les diagnostics d'intégrité, voir [Health checks](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#health-checks){: external}.


## Affichage des métriques
{: #operations_service_view_metrics}

Exécutez la commande suivante pour afficher les métriques d'homologue. La même commande peut être utilisée pour afficher les métriques du service de tri, en remplaçant `<peer-endpoint>` par `<orderer-endpoint>`.

```
curl -k <peer-endpoint>/metrics --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Par exemple :

```
curl -k https://169.55.231.152:30766/metrics --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```
{:codeblock}


Pour plus d'informations sur les métriques, voir [Metrics](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#metrics){: external}.


## Affichage des niveaux de journalisation
{: #operations_service_log_level_view}

Exécutez la commande suivante pour afficher le niveau de journalisation. Le niveau de journalisation s'affichera sur votre terminal une fois la commande exécutée.

```
curl -k <peer-endpoint>/logspec --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Par exemple :
```
curl -k https://169.46.208.93:3210/logspec --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

Vous devez voir un résultat similaire à l'exemple suivant :

```
{"spec":"info"}
```

## Définition des niveaux de journalisation
{: #operations_service_log_level_set}

Pour modifier le paramètre de niveau de niveau de consignation existant, exécutez la commande suivante qui utilise la méthode `PUT` avec le corps JSON composé d'un seul attribut nommé `spec`. Remplacez `<log-level>` par vos niveaux de journalisation attendus. Pour plus d'informations sur les niveaux de consignation que vous pouvez définir, voir [Logging specification](https://hyperledger-fabric.readthedocs.io/en/release-1.4/logging-control.html#logging-specification){: external} dans la documentation Hyperledger Fabric.

```
curl -X PUT  <peer-endpoint>/logspec -d '{"spec":"<log-level>"}' --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Par exemple :
```
curl -X PUT  https://169.46.208.93:3210/logspec -d '{"spec":"chaincode=debug:info"}' --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

Après avoir défini un nouveau niveau de consignation, vous pouvez utiliser la [commande d'affiche de niveau de consignation](#operations_service_log_level_view) pour vérifier vos paramètres.

Pour plus d'informations sur la configuration des niveaux de consignation, voir [Log level management](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#log-level-management){: external} dans la documentation Hyperledger Fabric.
