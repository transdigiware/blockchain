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

# Knoten mit Operationsservice betreiben
{: #operations_service}

Die {{site.data.keyword.blockchainfull}} Platform basiert auf Hyperledger Fabric v1.4.1. Die Plattform unterstützt die Operationsservicefunktion, die eine REST-konforme API für Operationen bietet, die Operatoren die Möglichkeit zur Durchführung von Knotenstatusprüfungen, zum Extrahieren von Metriken zum Systembetrieb aus dem Peer und den Anordnungsknoten sowie zur Verwaltung der Protokollierungsstufen gibt. Der Peer und der Anordnungsknoten hosten einen HTTP-Server, der die REST-konforme API für Operationen bereitstellt.  Weiter Informationen zum Operationsservice finden Sie im Abschnitt zum [Operationsservice](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html){: external}.
{:shortdesc}


## Hinweise und Einschränkungen
{: #operations_service_consideration_limitation}

- Alle Peer- und Anordnungsknoten sind mit der Einstellung `clientAuthRequired: false` konfiguriert, sodass auf das Statusprüfprogramm zugegriffen werden kann. Da für `clientAuthRequired` die Einstellung `false` angegeben ist und auch die gegenseitige TLS-Authentifizierung aktiviert wurde, müssen Sie beim Zugriff auf den REST-Server die TLS-Identitäten übergeben, um die Authentifizierung durchführen zu können. Diese Einstellung stellt sicher, dann nur Benutzer mit den entsprechenden Schlüsseln die Protokolle anzeigen können.
- Momentan wird als einziges Modell zum Extrahieren von Metriken das auf [Prometheus](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#prometheus){: external} basierende Modell unterstützt.

## Vorbemerkungen
{: #operations_service_before_you_begin}

Sie müssen die folgenden Informationen für Ihren Peer und Anordnungsknoten zusammenstellen.

- Angaben zum Peerendpunkt (**`peer-endpoint`**) oder Anordnungsknotenendpunkt (**`orderer-endpoint`**)

  Die URL für den Operationsendpunkt des Peers oder Anordnungsknotens finden Sie in der JSON-Datei des Peers oder Anordnungsknotens, die Sie von der Konsole exportiert haben.

    1. Klicken Sie auf der Registerkarte **Knoten** auf den gewünschten Peer oder Anordnungsknoten.
    2. Klicken Sie auf der Peer- oder Anordnungsknotenseite auf das Symbol **Einstellungen** neben dem Namen des Peers oder Anordnungsknotens.
    3. Klicken Sie in der Seitenanzeige auf **Exportieren**, um die JSON-Datei des Peers oder Anordnungsknotens zu speichern.
    4. Suchen Sie die Endpunkt-URL für Operationen. Diese wird im Parameter `operations_url` der JSON-Datei angegeben. Dieser Wert wird in den im Folgenden aufgeführten Befehlen als Peerendpunkt (`peer-endpoint`) oder Anordnungsknotenendpunkt (`orderer-endpoint`) bezeichnet. Beispiel:

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

- Angaben zum TLS-Zertifikat des Clients (**`client-tls-cert`**) und zum TLS-Schlüssel des Clients (**`client-tls-key`**)

  Sie können das Zertifikat und den privaten Schlüssel der TLS-Zertifizierungsstelle in der Konsole finden.

  1. Klicken Sie auf der Registerkarte **Knoten** auf den CA-Knoten für den gewünschten Peer oder Anordnungsknoten.
  2. Klicken Sie auf der Seite für die Zertifizierungsstelle (CA) auf **TLS-Zertifizierungsstelle**.
  3. Suchen Sie in der TLS-CA-Tabelle den Benutzer `admin` und klicken Sie auf die vertikal angeordneten Punkte unter **Aktionen**. Klicken Sie anschließend auf **Eintragungs-ID**.
  4. Klicken Sie in der Seitenanzeige auf **Weiter**. Daraufhin werden das Zertifikat und der private Schlüssel der TLS-Zertifizierungsstelle angezeigt.
  5. Ordnen Sie der Identität einen Anzeigenamen zu und klicken Sie dann auf **Identität exportieren** und anschließend auf **Identität zu Wallet hinzufügen**, um das Zertifikat und den privaten Schlüssel der TLS-Zertifizierungsstelle in einer JSON-Datei zu speichern.
  6. Öffnen Sie die exportierte JSON-Datei.
  7. Suchen Sie den privaten Schlüssel, der in der JSON-Datei als Wert des Parameters `private_key` angegeben ist. Dies ist Ihr Wert für `client-tls-key`, den Sie in den folgenden Befehlen verwenden können.
  8. Suchen Sie das Zertifikat der TLS-Zertifizierungsstelle, das im Wert des Parameters `cert` in der JSON-Datei angegeben ist. Dies ist Ihr Wert für `client-tls-cert`, den Sie in den folgenden Befehlen verwenden können.

- Angaben zum TLS-CA-Zertifikat des Peers (**`peer tls-ca cert`**) oder zum TLS-CA-Zertifikat des Anordnungsknotens (**`orderer tls-ca cert`**)

  Sie finden das TLS-CA-Zertifikat des Peers oder des Anordnungsknotens in der Konsole.

  1. Klicken Sie auf der Registerkarte **Knoten** auf den Knoten des Peers oder Anordnungsknotens.
  2. Klicken Sie auf das Symbol **Einstellungen** neben dem Knotennamen.
  3. Kopieren Sie die Zeichenfolge für das TLS-CA-Zertifikat des Knotens. Dies ist Ihr Wert für `peer tls-ca cert` oder `orderer tls-ca cert`, den Sie in den folgenden Befehlen verwenden können.

**Hinweis:** Für alle Zertifikate und Schlüssel müssen Sie die Zertifikatszeichenfolgen vom `Base64`-Format in individuelle Dateien im PEM-Format (`.pem`) konvertieren. Verwenden Sie hierzu die folgenden Befehle:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > <key_name>.pem
  ```
  {:codeblock}
{:important}


## Knotenstatus überprüfen
{: #operations_service_health_check}

Führen Sie den Befehl `curl -k <peer-endpoint>/healthz` oder `curl -k <orderer-endpoint>/healthz` aus, um den Status des Knotens für den Peer oder den Anordnungsknoten zu überprüfen. Beispiel:

```
curl -k https://169.46.208.93:3210/healthz
```
{:codeblock}

Weitere Informationen zu Statusprüfungen finden Sie im Abschnitt zu [Statusprüfungen](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#health-checks){: external}.


## Metriken anzeigen
{: #operations_service_view_metrics}

Führen Sie den folgenden Befehl aus, um die Metriken des Peers anzuzeigen. Zum Anzeigen der Metriken für den Anordnungsknoten kann der gleiche Befehl ausgeführt werden, dabei muss jedoch `<peer-endpoint>` durch `<orderer-endpoint>` ersetzt werden.

```
curl -k <peer-endpoint>/metrics --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Beispiel:

```
curl -k https://169.55.231.152:30766/metrics --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```
{:codeblock}


Weitere Informationen zu Metriken finden Sie unter [Metriken](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#metrics){: external}.


## Protokollierungsstufen anzeigen
{: #operations_service_log_level_view}

Führen Sie den folgenden Befehl aus, um die Protokollierungsstufe anzuzeigen. Nach Ausführung des Befehls wird die Protokollierungsstufe an Ihrem Terminal angezeigt.

```
curl -k <peer-endpoint>/logspec --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Beispiel:
```
curl -k https://169.46.208.93:3210/logspec --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

Im Folgenden wird ein Beispiel für das Ergebnis dargestellt:

```
{"spec":"info"}
```

## Protokollierungsstufen festlegen
{: #operations_service_log_level_set}

Um die vorhandene Einstellung für die Protokollierungsstufe zu ändern, müssen Sie den folgenden Befehl ausführen, der die Methode `PUT` mit einem JSON-Hauptteil verwendet, der aus einem einzelnen Attribut mit dem Namen `spec` besteht. Ersetzen Sie `<log-level>` durch die von Ihnen erwarteten Protokollierungsstufen. Weitere Information zu den Protollierungsstufen, die Sie festlegen können, finden Sie in der [Protokollierungsspezifikation](https://hyperledger-fabric.readthedocs.io/en/release-1.4/logging-control.html#logging-specification){: external} in der Hyperledger Fabric-Dokumentation.

```
curl -X PUT  <peer-endpoint>/logspec -d '{"spec":"<log-level>"}' --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Beispiel:
```
curl -X PUT  https://169.46.208.93:3210/logspec -d '{"spec":"chaincode=debug:info"}' --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

Nachdem Sie eine neue Protokollierungsstufe festgelegt haben, können Sie den [Befehl zum Anzeigen der Protokollierungsstufe](#operations_service_log_level_view) verwenden, um Ihre Einstellungen zu überprüfen.

Weitere Informationen Zur Konfiguration der Protokollierungsstufen finden Sie im Abschnitt zum [Protokollierungsstufenmanagement](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#log-level-management){: external} in der Hyperledger Fabric-Dokumentation.
