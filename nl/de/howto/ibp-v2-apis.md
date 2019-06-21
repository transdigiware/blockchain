---

copyright:
  years: 2019
lastupdated: "2019-05-16"

keywords: APIs, build a network, authentication, service credentials, API key, API endpoint, IAM access token, Fabric CA client, import a network, generate certificates

subcollection: blockchain

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# Netz mit APIs erstellen
{: #ibp-v2-apis}

{{site.data.keyword.blockchainfull}} Platform stellt REST-konforme APIs bereit, mit denen Sie Blockchain-Komponenten erstellen, importieren, bearbeiten und löschen sowie die Protokollierung, das Senden von Benachrichtigungen und die Konsoleneinstellungen verwalten können. Sie können die APIs sowie die entsprechenden SDKs verwenden, um Anwendungen zu entwickeln, die mit Ihrem Blockchain-Netz interagieren.
{: shortdesc}

Im vorliegenden Lernprogramm erhalten Sie eine Einführung zum allgemeinen Ablauf bei der Erstellung eines Blockchain-Netzes mit den APIs von {{site.data.keyword.blockchainfull_notm}} Platform. Weiterführende Informationen zu den verschiedenen APIs finden Sie im [{{site.data.keyword.blockchainfull_notm}} Platform-API-Referenzdokument ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](/apidocs/blockchain "{{site.data.keyword.blockchainfull_notm}} Platform-API-Referenzdokument"){: new_window}.

Diese APIs sind nur mit {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}} v0.1.77 oder einer höheren Version des Produkts kompatibel. Um die Version zu überprüfen, rufen Sie die Adresse `https://[your_console_url]/version.txt` auf. Hierbei steht *`[your_console_url]`* für die URL Ihrer {{site.data.keyword.blockchainfull_notm}} Platform-Konsole. Beispiel: https://1ee1869ffa6496d6bc1ce4b-optools.bp01.blockchain.cloud.ibm.com/version.txt
{:note}

## Vorbemerkungen
{: #ibp-v2-apis-prereq}

Sie müssen über eine {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz in {{site.data.keyword.cloud_notm}} verfügen, um API-Aufrufe absetzen zu können, über die die Interaktion mit dem Netz durchgeführt werden kann. Wenn Sie noch nicht über eine Serviceinstanz verfügen, dann verwenden Sie die [Anweisungen zu den ersten Schritten](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks), um eine Serviceinstanz zu erstellen.

## Authentifizierung
{: #ibp-v2-apis-authentication}

Um die APIs für den Zugriff auf das Blockchain-Netz zu verwenden, das Sie mit {{site.data.keyword.blockchainfull_notm}} Platform erstellen, muss Ihre Anwendung sich bei {{site.data.keyword.cloud_notm}} authentifizieren und eine Verbindung zu Ihrer Serviceinstanz herstellen können.

### Serviceberechtigungsnachweise abrufen
{: #ibp-v2-apis-retrieve-service-credentials}

Sie benötigen die Berechtigungsnachweise für die Basisauthentifizierung, um sicherzustellen, dass Sie auf Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz unter {{site.data.keyword.cloud_notm}} zugreifen können.

1. Öffnen Sie in der [{{site.data.keyword.cloud_notm}}-Ressourcenliste ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://cloud.ibm.com/resources) die Blockchain-Serviceinstanz, die Sie erstellt haben.
2. Klicken Sie auf **Serviceberechtigungsnachweise** im linken Navigator.
3. Klicken Sie auf die Schaltfläche "Neuer Berechtigungsnachweis" auf der Seite **Serviceberechtigungsnachweise**, um einen neuen Berechtigungsnachweis zu erstellen.
  1. Legen Sie einen Namen für den Berechtigungsnachweis fest, z. B. *UseAPIs*.
  2. Das Feld "Inline-Konfigurationsparameter hinzufügen" muss nicht ausgefüllt werden. 
  3. Klicken Sie auf die Schaltfläche **Hinzufügen**.
4. Klicken Sie nach dem Erstellen des neuen Berechtigungsnachweises auf **Berechtigungsnachweise anzeigen** unter der Überschrift **Aktionen** dieses Berechtigungsnachweises. Der Inhalt des Berechtigungsnachweises sieht in etwa wie das folgende Beispiel aus:

  ```
  {
    "api_endpoint": "https://1088ac8563e44f5a92539d946733ad7e-optools.so01.blockchain.test.cloud.ibm.com"
    "apikey": "nvASKts6B6lMZGZUNTGVP7MLK2BujMnxz0plSPYaqc3R",
    "configtxlator": "https://1088ac8563e44f5a92539d946733ad7e-configtxlator.so01.blockchain.test.cloud.ibm.com",
    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:blockchain:us-south:a/9d46037caee397faa45c55e087d26baa:1088ac85-63e4-4f5a-9253-9d946733ad7e::",
    "iam_apikey_name": "auto-generated-apikey-c1981f5c-cff0-464f-9a78-ed2f52e24d1a",
    "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
    "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/9d46037caee397faa45c55e087d26baa::serviceid:ServiceId-774190c5-9c37-4f68-8572-8d6e2aabbc7e",
  }
  ```

Im Serviceberechtigungsnachweis finden Sie den **API-Schlüssel** (`apikey`) und den **API-Endpunkt** (`api_endpoint`). Diese Angaben benötigen Sie zum Abrufen eines Zugriffstokens für {{site.data.keyword.iamshort}} (IAM).

### Zugriffstoken abrufen
{: #ibp-v2-apis-retrieve-token}

Die Authentifizierung bei {{site.data.keyword.blockchainfull_notm}} Platform kann durchgeführt werden, indem Sie ein Zugriffstoken von IAM abrufen. Mit diesem Zugriffstoken können Sie die {{site.data.keyword.blockchainfull_notm}} Platform-API für Ihren Service oder Ihre Anwendung innerhalb oder außerhalb von {{site.data.keyword.cloud_notm}} nutzen, ohne dass Sie hierzu Ihre persönlichen Benutzerberechtigungsnachweise mit anderen Benutzern teilen müssen.  

Rufen Sie die {{site.data.keyword.iamshort}}-API auf, um Ihr Zugriffstoken abzurufen.

```cURL
curl -X POST \
  "https://iam.cloud.ibm.com/identity/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \
```
{: codeblock}

Ersetzen Sie in der Anforderung `<API_KEY>` durch den in Ihren Serviceberechtigungsnachweisen definierten Wert für `apikey`. Das folgende verkürzte Beispiel zeigt die Ausgabe des Tokens:

```
{
"access_token": "eyJraWQiOiIyM...",
"refresh_token": "...",
"expires_in":3600,
"expiration":1555558683,
"scope":"ibm openid"}
```
{: screen}

Verwenden Sie den vollständigen Wert für `access_token` und als Präfix den Tokentyp `Bearer`, um Ihr Blockchain-Netz programmgesteuert über die APIs zu nutzen.

Zugriffstokens haben eine Gültigkeitsdauer von einer Stunde, sie können jedoch bei Bedarf erneut generiert werden. Um den Zugriff auf den Service sicherzustellen, müssen Sie das Zugriffstoken für Ihren API-Schlüssel in regelmäßigen Zeitabständen aktualisieren, indem Sie die {{site.data.keyword.iamshort}}-API aufrufen.   
{: tip}

## API-Anforderung erstellen
{: #ibp-v2-apis-form-api-request}

Wenn Sie einen API-Aufruf an den Service absetzen, dann sollten Sie die API-Anforderung so strukturieren, dass sie der Erstbereitstellung Ihrer {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz entspricht.

Zur Erstellung Ihrer Anforderung sollten Sie einen API-Endpunkt mit den entsprechenden Authentifizierungsnachweisen im folgenden Format verknüpfen:

```cURL
curl -X <API method> \
    <API_endpoint>/<API> \
    -H 'authorization: Bearer <access_token>' \
    -H 'Content-Type: application/json' \
    -d '<request body>' \
```
{: codeblock}

Für jede API werden in der [{{site.data.keyword.blockchainfull_notm}} Platform-API-Referenzdokumentation ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](/apidocs/blockchain "{{site.data.keyword.blockchainfull_notm}} Platform-API-Referenzdokumentation") Beispiele für curl-Befehle aufgeführt.

Des Weiteren können Sie die Funktion für das **Ausprobieren** in der API-Referenzdokumentation verwenden, um Ihre API-Aufrufe zu testen, bevor Sie sie zu Ihren Anwendungen hinzufügen. Sie müssen bei {{site.data.keyword.cloud_notm}} angemeldet sein, um die Funktion für das **Ausprobieren** zu benutzen. Sie können eine beliebige Serviceinstanz in der Dropdown-Liste auswählen. Alle API-Anforderungen werden an das Netz gesendet, das im API-Endpunkt angegeben ist.

## Einschränkungen
{: #ibp-v2-apis-limitations}

Sie können nur bereits vorhandene Knoten für Zertifizierungsstellen, Peers und Anordnungsknoten aus anderen {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}-Netzen importieren.

## Netz mit APIs erstellen
{: #ibp-v2-apis-build-with-apis}

Sie können APIs verwenden, um Blockchain-Komponenten in Ihrer {{site.data.keyword.blockchainfull_notm}} Platform-Instanz zu erstellen. Führen Sie die folgenden Schritte aus, um ein Blockchain-Netz mit den {{site.data.keyword.blockchainfull_notm}}-APIs zu erstellen.

1. Erstellen Sie eine Zertifizierungsstelle (CA), indem Sie [`POST /ak/api/v1/kubernetes/components/ca`](/apidocs/blockchain?code=try#create-a-ca) aufrufen.

  Notieren Sie sich Ihre Eingabe und die zugehörige Antwort, da Sie diese Daten zu einem späteren Zeitpunkt erneut benötigen.
  {: tip}

  Sie müssen warten, bis die Zertifizierungsstelle gestartet wird. Abhängig von der von Ihnen verwendeten Umgebung kann dieser Vorgang mehrere Minuten dauern. Sie können die API `GET <ca_url>/cainfo` aufrufen, um den Status Ihrer Zertifizierungsstelle zu überprüfen. Das System gibt wiederholt Fehler und dann den Statuscode `200` aus. Dieser Statuscode zeigt an, dass Sie mit dem nächsten Schritt fortfahren können. Beachten Sie hierbei, dass die Gültigkeit dieses API-Aufrufs nach einer Minute abläuft.

2. Verwenden Sie die Zertifizierungsstelle zur Registrierung Ihrer Komponenten- und Administratoridentitäten und zum Generieren der erforderlichen Zertifikate. Sie können den Fabric-CA-Client verwenden, um die folgenden Schritte auszuführen:

  - [Fabric-CA-Client einrichten](#ibp-v2-apis-config-fabric-ca-client).
  - [Zertifikate als CA-Administrator generieren](#ibp-v2-apis-enroll-ca-admin).
  - [Neue Komponente bei Ihrer Zertifizierungsstelle registrieren](#ibp-v2-apis-config-register-component).
  - Außerdem müssen Sie einen [Organisationsadministrator registrieren](#ibp-v2-apis-config-register-admin) und anschließend in einem MSP-Ordner [Zertifikate für den Administrator generieren](#ibp-v2-apis-config-enroll-admin). Wenn Sie Ihre Administratoridentität bereits registriert haben, müssen Sie diesen Schritt nicht ausführen.
  - [Registrieren Sie die neue Komponente bei Ihrer TLS-Zertifizierungsstelle](#ibp-v2-apis-config-register-component-tls).

  Sie können diese Schritte auch über die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole ausführen. Weitere Informationen hierzu finden Sie im Abschnitt zum [Erstellen und Verwalten von Identitäten](/docs/services/blockchain/howto/ibp-console-identities.html).

3. [Erstellen Sie eine MSP-Definition für Ihre Organisation](#ibp-v2-apis-msp), indem Sie [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?#import-a-membership-service-provide-msp) aufrufen.

4. [Erstellen Sie die Konfigurationsdatei](#ibp-v2-apis-config), die zum Erstellen eines Anordnungsknotens oder Peers erforderlich ist. Sie müssen für jeden Anordnungsknoten oder Peer, den Sie erstellen wollen, eine individuelle Konfigurationsdatei erstellen.

5. Erstellen Sie einen Anordnungsknoten, indem Sie [`POST /ak/api/v1/kubernetes/components/orderer`](/apidocs/blockchain?code=try#create-an-orderer) aufrufen.

6. Erstellen Sie einen Peer, indem Sie [`POST /ak/api/v1/kubernetes/components/peer`](/apidocs/blockchain?code=try#create-a-peer) aufrufen.

7. Wenn Sie die Konsole zum Betrieb Ihrer Blockchain-Komponenten benutzen möchten, dann müssen Sie Ihre Administratoridentität in Ihre Konsolenwallet importieren. Verwenden Sie die Registerkarte für Wallets, um das Zertifikat und den privaten Schlüssel Ihres Knotenadministrators in die Konsole zu importieren und eine Identität zu erstellen. Anschließend müssen Sie über die Konsole diese Identität den von Ihnen erstellten Komponenten zuordnen. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zum [Importieren einer Administratoridentität in die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole](#ibp-v2-apis-admin-console).

8. Nach der Bereitstellung Ihres Netzes können Sie die Fabric-SDKs, die Peer-CLI oder die Benutzerschnittstelle der Konsole verwenden, um Kanäle zu erstellen und Smart Contracts zu installieren oder zu instanziieren.

Die Serviceberechtigungsnachweise, die für die API-Authentifizierung verwendet werden, müssen in IAM über die Rolle `Manager` verfügen, damit Komponenten erstellt werden können. In der in diesem Abschnitt dargestellten Tabelle zu den [Benutzerrollen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) finden Sie weiterführende Informationen.
{: note}

## Netz mit APIs importieren
{: #ibp-v2-apis-import-with-apis}

Die APIs können auch zum Importieren von {{site.data.keyword.blockchainfull_notm}}-Komponenten, die mit den APIs oder über die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole erstellt wurden, in eine andere Serviceinstanz von {{site.data.keyword.blockchainfull_notm}} Platform 2.0 verwendet werden.

1. Importieren Sie eine Zertifizierungsstelle, indem Sie [`POST /ak/api/v1/components/ca`](/apidocs/blockchain?code=try#import-a-ca) aufrufen.

  Notieren Sie sich Ihre Eingabe und die zugehörige Antwort, da Sie diese Daten zu einem späteren Zeitpunkt erneut benötigen.
  {: tip}

  Sie müssen warten, bis die Zertifizierungsstelle gestartet wird. Abhängig von der von Ihnen verwendeten Umgebung kann dieser Vorgang mehrere Minuten dauern. Sie können [`GET /components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) aufrufen, um den Status der Zertifizierungsstelle zu überprüfen. Das System gibt wiederholt Fehler und dann den Statuscode `200` aus. Dieser Statuscode zeigt an, dass Sie mit dem nächsten Schritt fortfahren können. Beachten Sie hierbei, dass die Gültigkeit dieses API-Aufrufs nach einer Minute abläuft.

2. Importieren Sie die MSP-Definition einer Organisation, indem Sie [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp) aufrufen.

3. Importieren Sie einen Anordnungsknoten, indem Sie [`POST /ak/api/v1/components/orderer`](/apidocs/blockchain?code=try#import-a-orderer) aufrufen.

4. Importieren Sie einen Peer, indem Sie [`POST /ak/api/v1/components/peer`](/apidocs/blockchain?code=try#import-a-peer) aufrufen.

5. Wenn Sie die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole verwenden wollen, um Ihre Blockchain-Komponenten zu betreiben, dann müssen Sie die Identitäten Ihres Komponentenadministrators in Ihre Konsolenwallet importieren. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zum [Importieren einer Administratoridentität in die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole](#ibp-v2-apis-admin-console).

6. Nach der Bereitstellung Ihres Netzes können Sie die Fabric-SDKs, die Peer-CLI oder die Benutzerschnittstelle der Konsole verwenden, um Kanäle zu erstellen und Smart Contracts zu installieren oder zu instanziieren.

Die Serviceberechtigungsnachweise, die für die API-Authentifizierung verwendet werden, müssen in IAM über die Rolle `Schreibberechtigter` verfügen, damit Komponenten importiert werden können. In der in diesem Abschnitt dargestellten Tabelle zu den [Benutzerrollen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) finden Sie weiterführende Informationen.
{: note}

## Zertifizierungsstelle mithilfe des Fabric-CA-Clients betreiben
{: #ibp-v2-apis-config-fabric-ca-client}

Zum Betrieb Ihrer Zertifizierungsstellen können Sie den Fabric-CA-Client verwenden. Führen Sie die folgenden Befehle des Fabric-CA-Clients aus, um Ihre Komponenten- und Administratoridentitäten zu registrieren und die erforderlichen Zertifikate zu generieren.

### Fabric-CA-Client einrichten
{: #ibp-v2-apis-setup-fabric-ca-client}

1. Laden Sie den [Fabric-CA-Client ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#fabric-ca-client "Fabric-CA-Client herunterladen") in Ihr lokales Dateisystem herunter.

  Am einfachsten erhalten Sie den Fabric-CA-Client, indem Sie alle Binärdateien für die Fabric-Tools direkt herunterladen. Navigieren Sie zu einem Verzeichnis, in das Sie die Binärdateien mithilfe der Befehlszeile herunterladen wollen, und rufen Sie die Dateien durch die Ausführung des folgenden [curl-Befehls ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html#install-curl "Curl") ab.

  ```
  curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.0 1.4.0 -d -s
  ```
  {:codeblock}

  Dieser Befehl installiert die Binärdateien in einem Verzeichnis namens `bin/`.

2. Legen Sie in `PATH` den Pfad zu dem Verzeichnis fest, in das Sie die Binärdateien für das Fabric-Tool heruntergeladen haben:

  ```
  export PATH=$PATH:<full/path/to/fabric-client/bin>
  ```
  {:codeblock}

  Falls Sie die Binärdateien beispielsweise in Ihrem Ausgangsverzeichnis installiert haben, würden Sie `PATH` wie folgt festlegen:

  ```
  export PATH=$PATH:$HOME/bin
  ```
  {:codeblock}

3. Erstellen Sie den Ordner, in dem Sie die Zertifikate Ihres CA-Administrators speichern wollen.

  ```
  mkdir -p $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

4. Legen Sie als Wert der Umgebungsvariablen `$FABRIC_CA_CLIENT_HOME` den Pfad fest, in dem der CA-Client die generierten [MSP](/docs/services/blockchain/howto/CA_operate.html#ca-operate-msp)-Zertifikate speichert. Achten Sie darauf, das Konfigurationsmaterial zu entfernen, das möglicherweise bei früheren Versuchen erstellt wurde. Falls Sie den Befehl `enroll` zuvor noch nie ausgeführt haben, sind der Ordner `msp` und die Datei `.yaml` nicht vorhanden.

  ```
  export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/ca-admin
  rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
  rm -rf $FABRIC_CA_CLIENT_HOME/msp
  ```
  {:codeblock}

5. Rufen Sie das TLS-Zertifikat Ihrer Zertifizierungsstelle ab, das vom Fabric-CA-Client verwendet werden soll. Wenn Sie die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole verwenden, dann öffnen Sie die Zertifizierungsstelle und klicken Sie auf **Einstellungen**. Suchen Sie dann nach dem Zertifikat im Base64-Format, das im Feld **TLS-Zertifikat** enthalten ist. Wenn Sie mit den APIs arbeiten, dann können Sie [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) aufrufen. Im Feld `"PEM"` finden Sie dann das CA-TLS-Zertifikat. Wenn Sie die Zertifizierungsstelle mit der API zum `Erstellen einer Fabric-CA` erstellt haben, können Sie das TLS-Zertifikat auch im Antworthauptteil finden.

  Sie müssen das Zertifikat vom Base64- ins PEM-Format konvertieren, um es für die Kommunikation mit Ihrer Zertifizierungsstelle verwenden zu können. Fügen Sie die im Base64-Format codierte Zeichenfolge des TLS-Zertifikats in den folgenden Befehl ein. Vergewissern Sie sich, dass Sie sich im Verzeichnis `$HOME/fabric-ca-client` befinden.

  ```
  cd $HOME/fabric-ca-client
  mkdir catls
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > tls.pem
  ```
  {:codeblock}

### Zertifikate als CA-Administrator generieren
{: #ibp-v2-apis-enroll-ca-admin}

Beim Erstellen Ihrer Zertifizierungsstelle wurde automatisch die Identität eines **CA-Administrators** registriert. Sie können nun den Namen und das Kennwort dieses Administrators verwenden, um mit dem Fabric-CA-Client einen Befehl `enroll` zum Generieren eines MSP-Ordners mit Zertifikaten abzusetzen, die anschließend zum Registrieren anderer Identitäten für Peers oder Anordnungsknoten verwendet werden.

1. Stellen Sie sicher, dass Sie die Schritte für die [Einrichtung des Fabric-CA-Clients](/docs/services/blockchain/howto/CA_operate.html#ca-operate-fabric-ca-client) ausgeführt und für `$FABRIC_CA_CLIENT_HOME` das Verzeichnis festgelegt haben, in dem Sie die Zertifikate für den CA-Administrator speichern wollen.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

2. Führen Sie den folgenden Befehl aus, um Zertifikate zu generieren:

  ```
  fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name>  --tls.certfiles  <ca_tls_cert_path>
  ```
  {:codeblock}

  Die Angaben für `<enroll_id>` und `<enroll_password>` im Befehl entsprechen den Werten für `enroll_id` und `enroll_secret`, die Sie bei der Erstellung der Zertifizierungsstelle angegeben haben. Verwenden Sie die **Endpunkt-URL der Zertifizierungsstelle** aus Ihrer {{site.data.keyword.blockchainfull_notm}} Platform-Konsole oder den Wert für `"ca_url"`, der von Ihrem API-Aufruf als Wert für `<ca_url_with_port>` zurückgegeben wurde. Lassen Sie die Angabe `http://` am Beginn weg. In `<ca_name>` wird der **CA-Name** aus Ihrer Konsole oder der Wert für `"ca_name"` angezeigt, der von den APIs zurückgegeben wird.

  In `<ca_tls_cert_path>` ist der vollständige Pfad Ihres CA-TLS-Zertifikats angegeben.

  Ein realer Aufruf könnte ähnlich wie im folgenden Beispiel aussehen:

  ```
  fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```  
  {:codeblock}

**Tipp:** Wenn der Wert der Eintragungs-URL (Wert des Parameters `-u`) ein Sonderzeichen enthält, müssen Sie entweder das Sonderzeichen codieren oder die URL in die Hochkommas einschließen. Beispiel: `!` wird zu `%21` oder der Befehl sieht wie folgt aus:

  ```
  ./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname ca
  ```
  {:codeblock}

  Der Befehl `enroll` generiert einen vollständigen Satz von Zertifikaten, der auch als "MSP-Ordner" (MSP = Membership Service Provider) bezeichnet wird und sich innerhalb des Verzeichnisses befindet, das Sie als Pfad von `$HOME` für Ihren Fabric-CA-Client festgelegt haben. Beispiel: `$HOME/fabric-ca-client/ca-admin`. Weitere Informationen zu MSPs und zum Inhalt des MSP-Ordners finden Sie unter [Membership Service Providers (MSPs)](/docs/services/blockchain/howto/CA_operate.html#ca-operate-msp).

  Falls der Befehl `enroll` fehlschlägt, finden Sie unter [Fehlerbehebung](/docs/services/blockchain/howto/CA_operate.html#ca-operate-troubleshooting) Informationen zu möglichen Ursachen.

  Durch einen Befehl "tree" können Sie prüfen, ob Sie alle vorausgesetzten Schritte ausgeführt haben. Navigieren Sie zu dem Verzeichnis, in dem Sie Ihre Zertifikate gespeichert haben. Der Befehl "tree" sollte ein Ergebnis ähnlich der folgenden Struktur generieren:

  ```
  cd $HOME/fabric-ca-client
tree
.
  ├── ca-admin
  │   ├── fabric-ca-client-config.yaml
  │   └── msp
  │       ├── cacerts
  │       │   └── 9-30-250-70-30395-SampleOrgCA.pem
  │       ├── IssuerPublicKey
  │       ├── IssuerRevocationPublicKey
  │       ├── keystore
  │       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
  │       ├── signcerts
  │       │   └── cert.pem
  │       └── user
  └── catls
      └── tls.pem    
  ```

### Komponentenidentität bei der Zertifizierungsstelle registrieren
{: #ibp-v2-apis-config-register-component}

Zuerst müssen Sie eine Komponentenidentität bei Ihrer Zertifizierungsstelle registrieren. Ihre Komponente verwendet diese Identität, um die Zertifikate zu generieren, die bei der Bereitstellung benötigt werden.

1. [Generieren Sie Zertifikate als CA-Administrator](#ibp-v2-apis-enroll-ca-admin); hierzu verwenden Sie den Fabric-CA-Client. Verwenden Sie diese Administratorzertifikate, um die folgenden Befehle auszugeben. Stellen Sie sicher, dass für `$FABRIC_CA_CLIENT_HOME` der Wert `$HOME/fabric-ca-client/ca-admin` festgelegt ist.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```

2. Geben Sie den folgenden Befehl aus, um Ihre Zugehörigkeit und Ihren Organisationsnamen zu ermitteln.
  ```
  fabric-ca-client affiliation list --caname <ca_name> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  Ihr Befehl könnte wie im folgenden Beispiel aussehen:
  ```
  fabric-ca-client affiliation list --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  Daraufhin werden die folgenden Informationen angezeigt:
  ```
  affiliation: org1
      affiliation: org1.department1
  ```

  Notieren Sie sich den zweiten Wert für **affiliation**, z. B. `org1.department1`. Sie müssen diesen Wert im folgenden Befehl verwenden.

3. Führen Sie den folgenden Befehl aus, um den Anordnungsknoten oder Peer zu registrieren.

  ```
  fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type <component_type> --id.secret <secret> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  Erstellen Sie einen Namen und ein Kennwort für die Komponente und ersetzen Sie `name` und `secret` durch diese Werte.  Notieren Sie diese Informationen. Legen Sie für `--id.type` den Wert `orderer` fest, falls Sie einen Anordnungsknoten bereitstellen; legen Sie `peer` fest, falls Sie einen Peer bereitstellen. Der Befehl könnte wie im folgenden Beispiel aussehen:

  ```
  fabric-ca-client register --caname ca --id.affiliation org1.department1 --id.name peer1 --id.secret peer1pw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  Sie müssen die Werte für `"enrollid"` und `"enrollsecret"` aus dem obigen Befehl speichern, da Sie sie bei der Erstellung Ihrer Konfigurationsdatei benötigen.
  {: important}

  Eine Identität kann immer nur einmal registriert werden. Tritt ein Problem auf, dann wiederholen Sie den Vorgang mit einem Befehl, für den ein neuer Benutzername und ein neues Kennwort angegeben werden. Stellen Sie als Sicherheitsmaßnahme mit jeder Identität und den zugehörigen Werten für "enrollID" und "secret" nur einen einzigen Peer bereit. Sie können zwar eine einzige **Administratoridentität** für mehrere Komponenten verwenden (dies entspricht auch der empfohlenen Bereitstellungsstrategie), sollten aber keine IDs und Kennwörter für Peers wiederverwenden.

  Wird der Befehl erfolgreich ausgeführt, dann werden die im folgenden Beispiel dargestellten Informationen angezeigt:

  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: peerpw
  ```

### Organisationsadministrator registrieren
{: #ibp-v2-apis-config-register-admin}

Sie müssen außerdem eine Administratoridentität erstellen, die Sie zum Betrieb Ihres Netzes verwenden können. Sie werden diese Identität verwenden, um bestimmte Komponenten zu betreiben, z. B. durch Installation eines Smart Contract auf Ihrem Peer. Sie können diese Identität auch für den Administrator Ihrer Organisation verwenden und zum Erstellen und Bearbeiten von Kanälen benutzen.  

Diese neue Identität müssen Sie bei Ihrer Zertifizierungsstelle registrieren. Anschließend verwenden Sie sie, um einen MSP-Ordner zu generieren. Sie können diese Identität einem Organisationsadministrator zuordnen, indem Sie das zugehörige signCert-Zertifikat zum MSP Ihrer Organisation hinzufügen. Das signCert-Zertifikat muss außerdem zu Ihrer Konfigurationsdatei hinzugefügt werden, sodass es bei der Bereitstellung als Administratorzertifikat des Anordnungsknotens oder des Peers benutzt werden kann. Für Ihre Organisation müssen Sie nur eine einzige Administratoridentität erstellen. Demzufolge müssen diese Schritt auch nur einmal ausgeführt werden. Sie können das signCert-Zertifikat verwenden, das Sie zur Bereitstellung zahlreicher Peers oder Anordnungsknoten generiert haben.

Stellen Sie sicher, dass für `$FABRIC_CA_CLIENT_HOME` der Pfad zum MSP Ihrer CA-Domäne festgelegt ist.

```
echo $FABRIC_CA_CLIENT_HOME
$HOME/fabric-ca-client/ca-admin
```
{:codeblock}

Registrieren Sie die Administratoridentität für die Komponente bei der Zertifizierungsstelle, indem Sie den folgenden Befehl ausführen:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type client --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Erstellen Sie für den Administrator eine neue Benutzeridentität mit den Werten für `name` and `secret`. Achten Sie darauf, andere Werte als für die Peer- oder Anordnungsknotenidentität zu verwenden, die Sie gerade registriert haben. Der Befehl hat das im folgenden Beispiel dargestellte Format:

```
fabric-ca-client register --caname ca --id.name peeradmin --id.affiliation org1.department1 --id.type client --id.secret peeradminpw --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Notieren Sie diese Informationen. Sie verwenden diese Werte für `name` und `secret` zum Generieren des MSP-Ordners mit dem Befehl `enroll`.
{: important}

### MSP-Ordner für Administrator generieren
{: #ibp-v2-apis-config-enroll-admin}

Nachdem Sie den Komponentenadministrator registriert haben, können Sie die Zertifikate mit dem folgenden Befehl generieren:

```
fabric-ca-client enroll -u https://<peer_admin_enroll_id>:<peer_admin_enroll_password>@<ca_url_with_port> --caname <ca_name> -M $HOME/path/to/peer-admin/msp --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Beispiel:

```
fabric-ca-client enroll -u https://peeradmin:peeradminpw@9.30.94.174:30167 --caname ca -M $HOME/fabric-ca-client/peer-admin/msp --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{:codeblock}

Nach der erfolgreichen Ausführung dieses Befehls wird ein neuer MSP-Ordner in dem Verzeichnis generiert, das Sie mit dem Flag `-M` angegeben haben. Dieses Verzeichnis enthält die Zertifikate, die Sie zum Betrieb Ihrer Komponenten verwenden. Mit einem Befehl "tree" können Sie überprüfen, ob der Befehl "enroll" erfolgreich ausgeführt wurde. Navigieren Sie zu dem Verzeichnis, in dem Sie Ihre Zertifikate gespeichert haben. Der Befehl "tree" sollte ein Ergebnis ähnlich der folgenden Struktur generieren:


```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── keystore
│       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── catls
│   └── tls.pem
├── orderer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── dfe06060490bf62e2bd709433fc747ff28cdbb1e040682c5d47a4e8598db4f2e_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── peer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 24f76217c5c7e641ee29b068712181294e427cbee4dbfce230a1a98b53489cbe_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── tlsca-admin
    ├── fabric-ca-client-config.yaml
    └── msp
        ├── cacerts
        │   └── 9-30-250-70-30395-tlsca.pem
        ├── IssuerPublicKey
        ├── IssuerRevocationPublicKey
        ├── keystore
        │   └── 45a7838b1a91ddfe3d4d22a5a7f2639b868493bcce594af3e3ceb9c07899d117_sk
        ├── signcerts
        │   └── cert.pem
        └── user
```

Sie müssen diesen Ordner erneut aufrufen, um die MSP-Definition Ihrer Organisation und die Konfigurationsdateien zu erstellen.
{: important}

### Komponentenidentität bei der TLS-Zertifizierungsstelle registrieren
{: #ibp-v2-apis-config-register-component-tls}

Bei der Erstellung Ihrer Zertifizierungsstelle wurde zusammen mit Ihrer Standardzertifizierungsstelle auch eine TLS-Zertifizierungsstelle bereitgestellt. Der Anordnungsknoten oder Peer muss auch bei der TLS-Zertifizierungsstelle registriert werden. Hierzu müssen Sie zuerst die Eintragung mithilfe des Administrators der TLS-Zertifizierungsstelle vornehmen. Ändern Sie den Wert für `$FABRIC_CA_CLIENT_HOME` und geben Sie ein Verzeichnis an, in dem Sie Ihre Administratorzertifikate für die TLS-Zertifizierungsstelle speichern wollen.

```
cd $HOME/fabric-ca-client
mkdir tlsca-admin
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/tlsca-admin
```
{:codeblock}

Führen Sie den folgenden Befehl aus, um sich als Administrator für die TLS-Zertifizierungsstelle einzutragen. Die Eintragungs-ID und das Kennwort des Administrators Ihrer TLS-Zertifizierungsstelle stimmen mit den entsprechenden Werten Ihrer Standardzertifizierungsstelle überein. Demzufolge ist der folgende Befehl identisch mit dem Befehl, der zur Eintragung Ihres [CA-Administrators](/docs/services/blockchain/howto/CA_operate.html#ca-operate-enroll-ca-admin) verwendet wurde. Hierbei wird lediglich der Name der TLS-Zertifizierungsstelle angegeben. Ihr TLS-CA-Name entspricht dem Wert für **Name der TLS-Zertifizierungsstelle** aus der Anzeige mit den **Einstellungen** der Zertifizierungsstelle in der Konsole oder dem Wert für `"tlsca_name"`, der von der API zum `Erstellen einer Zertifizierungsstelle` zurückgegeben wurde.

```
fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <tls_ca_name> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Ein echter Aufruf könnte wie im folgenden Beispiel aussehen:

```
fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname tlsca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Nach der Eintragung besitzen Sie die erforderlichen Zertifikate für die Registrierung Ihrer Komponente bei der TLS-Zertifizierungsstelle. Führen Sie den folgenden Befehl aus, um den Anordnungsknoten oder Peer zu registrieren:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type peer --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Dieser Befehl ähnelt dem Befehl, den Sie zum Registrieren der Komponentenidentität bei der Zertifizierungsstelle verwendet haben, mit Ausnahme, dass hier der Name der TLS-Zertifizierungsstelle verwendet wird. Falls Sie anstelle eines Peers einen Anordnungsknoten registrieren, legen Sie für `--id.type` den Wert `orderer` statt `peer` fest. Sie müssen für diese Identität einen anderen Benutzernamen und ein anderes Kennwort angeben, als Sie für Ihre Standardzertifizierungsstelle verwendet haben. Eine echte Registrierung könnte ähnlich wie der folgende Befehl aussehen:

```
fabric-ca-client register --caname tlsca --id.affiliation org1.department1 --id.name peertls --id.secret peertlspw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Sie müssen die Werte für `"enrollid"` und `"enrollsecret"` aus dem obigen Befehl speichern, da Sie sie bei der Erstellung Ihrer Konfigurationsdatei benötigen.
{: important}

## MSP-Definition einer Organisation erstellen
{: #ibp-v2-apis-msp}

Sie können die APIs verwenden, um die MSP-Definition einer Organisation zu erstellen, indem Sie [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp) aufrufen. Dieser MSP (Membership Service Provider) enthält Zertifikate, die Ihre Organisation in einem Blockchain-Konsortium definieren, und außerdem die Administratorzertifikate, die Sie zum Betreiben Ihres Netzes verwenden können. Wenn Sie den hier aufgeführten Schritt ausgeführt haben, dann haben Sie bereits die Zertifikate generiert, die zum Erstellen des MSP einer Organisation erforderlich sind. Führen Sie die folgenden Schritte aus, um den Anforderungshauptteil des API-Aufrufs abzuschließen.

1. Geben Sie als `Typ` der Anforderung `msp` ein.

2. Wählen Sie eine MSP-ID für Ihre Organisation aus. Die MSP-ID ist der formale Name Ihrer Organisation innerhalb des Konsortiums. Die MSP-ID, die zum Erstellen des MSP der Organisation verwendet wurde, muss identisch mit der MSP-ID sein, die zum Bereitstellen Ihrer Peers verwendet wurde.

3. Wählen Sie einen Anzeigenamen für Ihren MSP aus.

4. Sie müssen das signCert-Zertifikat des Organisationsadministrators, den Sie über den Fabric-CA-Client registriert und eingetragen haben, im Base64-Format angeben.  

  Navigieren Sie zu dem MSP-Verzeichnis, das erstellt wurde, als Sie die [Zertifikate mithilfe des Organisationsadministrators generiert](#ibp-v2-apis-config-enroll-admin) haben.
  ```
  cd $HOME/fabric-ca-client/peer-admin/msp
  ```
  Öffnen Sie in diesem MSP-Verzeichnis die signCert-Datei des neuen Benutzers und konvertieren Sie sie mit den folgenden Befehlen in das Base64-Format:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  **Hinweis:** Die mit dem obigen Befehl generierte Zeichenfolge muss unbedingt in einer einzigen Zeile formatiert sein. Sie sollte in etwa wie folgt aussehen:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
  ```
  Folgende Darstellung ist nicht korrekt:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
  ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
  Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
  VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
  WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
  ```

  Beispiel:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  Dieser Befehl gibt eine Zeichenfolge aus, die ähnlich wie das folgende Beispiel aussieht:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
  ```

  Geben Sie diese Zeichenfolge im Feld `admins` der API-Anforderung an.

5. Außerdem müssen Sie das Stammzertifikat Ihrer Zertifizierungsstelle angeben. Dieses Zertifikat wurde erstellt, als Sie die [Zertifikate mit Ihrem CA-Administrator generiert](#ibp-v2-apis-enroll-ca-admin) haben.

  Navigieren Sie zum MSP-Verzeichnis des CA-Administrators.
  ```
  cd $HOME/fabric-ca-client/ca-admin/msp
  ```
  In diesem Verzeichnis müssen Sie den Ordner "cacerts" öffnen und das darin enthaltene Zertifikat ins Base64-Format konvertieren. Verwenden Sie dazu die folgenden Befehle:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-ca-admin>/msp/cacerts/<ca_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Daraufhin wird das Stammzertifikat als Base64-Zeichenfolge ausgegeben.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWIyZ0F3SUJBZ0lVQmZnZzcvVnIrL25OVEFNQlQ4UUtHL00wQU8wd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU1TURVd016RXpNamt3TUZvWERUTTBNRFF5T1RFek1qa3dNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUVXMUtvN2lWeVE2VWkwdDVqbU5KaWVuSUwKR3pNM1BDWHlhL2VSQ0NWMmFQb0dTZ1lrVUg2UWN5RjAzbFlMZFU4Y0drNTQ0alViVC9KT1lYeVgzTWc4bHFORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZDK2lJR0NSb2Zvb3FsVkZoU3dOMmk2MXNJaVBNQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUURTYW9RL1E0QzkKbFl1VGNhVXVHb3d6YmhUZHBuN2F3S2lHN1Nvd2lSQXVld0lnUWlyM3RNR3IvYWo2aU5lRXJFN2NyOVowQ0gvTwp3QnNQcWd4RVR3MjVqZUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Geben Sie diese Zeichenfolge im Feld `root_certs` der API-Anforderung an.

6. Außerdem müssen Sie das Stammzertifikat Ihrer TLS-Zertifizierungsstelle angeben. Mit dem TLS-Stammzertifikat können Ihre Peers am Gossip-Datenaustausch über einen Kanal teilnehmen.

  Navigieren Sie zu dem MSP-Verzeichnis, das generiert wurde, als Sie den [Administrator der TLS-Zertifizierungsstelle eingetragen](#ibp-v2-apis-config-register-component-tls) haben.
  ```
  cd $HOME/fabric-ca-client/tlsca-admin/msp
  ```
  In diesem Verzeichnis müssen Sie den Ordner "cacerts" öffnen und das darin enthaltene Zertifikat ins Base64-Format konvertieren. Verwenden Sie dazu die folgenden Befehle:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-tlsca-admin>/msp/cacerts/<tls_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Daraufhin wird das Stammzertifikat der TLS-Zertifizierungsstelle als Base64-Zeichenfolge ausgegeben.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVWUVQWnprNXV2b3dobEtacG5JMXplODdIQUlnd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEExTURNeE16STVNREJhRncwek5EQTBNamt4TXpJNU1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFRdSs2UnZWd2w5T2dDVlAraEVxbjVxdExRVG9LWkw4a1lic0pOeU1JbERoc3hlNWx6cW1zQkoKbTk2eUR2TVV6OSsxL2pzb1M4M1JqMVAwc3M2TnJNb3FvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVVnUEc4anJEK1BxVjdoelc3WDlsbTFrMS91WjR3CkZRWURWUjBSQkE0d0RJY0VxVGJESW9jRUNwbnBkVEFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUVBenk3cHJZaVMKQmlDVWdYeWRkY09WMm9mZmtqaEI0N091QXFjQWNqZS9SWkVDSUdKZFgzZ1ErTDRIN3duY1RoZkwrenU1ejV1UApGUWhXTmlNS3hQWEYrZnYwCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Geben Sie diese Zeichenfolge im Feld `tls_root_certs` der API-Anforderung an.

## Konfigurationsdatei erstellen
{: #ibp-v2-apis-config}

Sie müssen eine Konfigurationsdatei vervollständigen, um einen Peer oder Anordnungsknoten mithilfe der APIs erstellen zu können. Diese Datei wird für die API als Objekt `config` im Anforderungshauptteil des API-Aufrufs bereitgestellt. Sie müssen eine Zertifizierungsstelle für Ihre {{site.data.keyword.cloud_notm}} Platform-Serviceinstanz bereitstellen und dann die Schritte zur Registrierung und Eintragung der erforderlichen Identitäten ausführen, bevor Sie die Datei abschließen.

Die Vorlage für die Konfigurationsdatei ist nachfolgend dargestellt:
```
{
	"enrollment": {
		"component": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"admincerts": [""]
		},
		"tls": {
			"cahost": "",
			"caport": "",
			"caname": "",
			"catls": {
				"cacert": ""
			},
			"enrollid": "",
			"enrollsecret": "",
			"csr": {
				"hosts": [""]
			}
		}
	}
}
```
{:codeblock}

Kopieren Sie diese gesamte Datei in einen Texteditor, in dem Sie sie bearbeiten und als JSON-Datei in Ihrem lokalen Dateisystem speichern können. Führen Sie die folgenden Schritte aus, um diese Konfigurationsdatei zu vervollständigen und sie zur Bereitstellung eines Anordnungsknotens oder Peers zu verwenden.

### Verbindungsinformationen der Zertifizierungsstelle abrufen
{: #ibp-v2-apis-config-connx-info}

Zunächst müssen Sie die Verbindungsinformationen Ihrer Zertifizierungsstelle in {{site.data.keyword.cloud_notm}} Platform angeben. Sie können die Benutzerschnittstelle der Konsole oder die APIs verwenden, um die erforderlichen Informationen zu Ihrer Zertifizierungsstelle abzurufen.

**Bei Verwendung der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole:**
Öffnen Sie die Zertifizierungsstelle in der Konsole und klicken Sie auf **Einstellungen** und dann auf die Schaltfläche **Exportieren**, um die Informationen der Zertifizierungsstelle in eine JSON-Datei zu exportieren. Sie können die Werte aus dieser Datei verwenden, um Ihre Konfigurationsdatei zu vervollständigen.

**Bei Verwendung der APIs:**
Sie können [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) aufrufen, um die Verbindungsinformationen Ihrer Zertifizierungsstelle abzurufen. Wenn Sie die Zertifizierungsstelle mit der API zum `Erstellen einer Fabric-CA` erstellt haben, können Sie die erforderlichen Informationen auch im Antworthauptteil finden.

- Die Werte für `"cahost"` und `"caport"` werden im Feld `ca_url` im Antworthauptteil oder in der JSON-Datei der Zertifizierungsstelle, die Sie exportiert haben, angezeigt. Wenn der Wert für `ca_url` z. B. https://9.30.94.174:30167 lautet, dann lautet der Wert für `"cahost"` `9.30.94.174` und der Wert für `"caport"` `30167`.
- In `"caname"` wird der Name der Zertifizierungsstelle angegeben, der bei der Bereitstellung der Zertifizierungsstelle festgelegt wurde. Dies ist der Wert im Feld `ca_name` des Antworthauptteils oder der exportierten JSON-Datei.
- Der Wert für `"cacert"` ist das in Base64 codierte TLS-Zertifikat Ihrer Zertifizierungsstelle. Dies ist der Wert im Feld `pem` des Antworthauptteils oder der exportierten JSON-Datei.

- In den Feldern im nachfolgenden Abschnitt `"tls"` sind dieselben Informationen erforderlich wie in den obigen Abschnitten für die Komponenten. Die einzige Ausnahme besteht darin, dass Sie den Wert des TLS-Instanznamens für die Zertifizierungsstelle verwenden müssen, der während der Bereitstellung der Zertifizierungsstelle angegeben wird. Ersetzen Sie `caname` durch den Wert von `tlsca_name` im Antworthauptteil oder in der exportierten JSON-Datei. Verwenden Sie dasselbe TLS-Zertifikat als Wert für `"cacert"`.
  ```
  "tls": {
    "cahost": "",
    "caport": "",
    "caname": "",
    "catls": {
      "cacert": ""
  ```
  {:codeblock}

### Eintragungs-ID und geheimen Schlüssel der Komponente angeben

1. Fügen Sie die Werte für `name` und `secret`, die Sie für die [Registrierung Ihrer Komponente bei der Standardzertifizierungsstelle](#ibp-v2-apis-config-register-component) verwendet haben, als Werte für `"enrollid"` und `"enrollsecret"` im Abschnitt `"component"` in die Konfigurationsdatei ein:

  ```
  "component": {...
    },
    "enrollid": "peer1",
    "enrollsecret": "peer1pw",
  ```
2. Fügen Sie die Werte für `name` und `secret`, die Sie für die [Registrierung Ihrer Komponente bei der TLS-Zertifizierungsstelle](#ibp-v2-apis-config-register-component-tls) verwendet haben, als Werte für `"enrollid"` und `"enrollsecret"` im Abschnitt `"tls"` in die Konfigurationsdatei ein:

  ```
  "tls": {...
    },
    "enrollid": "peertls",
    "enrollsecret": "peertlspw",
  ```

### signCert-Zertifikat Ihres Organisationsadministrators angeben

Navigieren Sie zu dem MSP-Verzeichnis, das erstellt wurde, als Sie die [Zertifikate mithilfe des Organisationsadministrators generiert](#ibp-v2-apis-config-enroll-admin) haben.
```
cd $HOME/fabric-ca-client/peer-admin/msp
```
Öffnen Sie in diesem MSP-Verzeichnis die signCert-Datei des neuen Benutzers und konvertieren Sie sie mit den folgenden Befehlen in das Base64-Format:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

**Hinweis:** Die mit dem obigen Befehl generierte Zeichenfolge muss unbedingt in einer einzigen Zeile formatiert sein. Sie sollte in etwa wie folgt aussehen:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
```
Folgende Darstellung ist nicht korrekt:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
```

Beispiel:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

Dieser Befehl gibt eine Zeichenfolge aus, die ähnlich wie das folgende Beispiel aussieht:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
```

Kopieren Sie diese Zeichenfolge in das Feld `"admincerts"` unter dem Abschnitt "component" in der Konfigurationsdatei.

### Hosts für Zertifikatssignieranforderungen (Certificate Signing Request, CSR)

Sie haben die Möglichkeit, im Abschnitt `"csr"` der Komponentendatei eine angepasste Domäne für Ihre Komponente anzugeben.

```
"csr": {
  "hosts": [""]
  }
```
{:codeblock}

Dieser Abschnitt ist für fortgeschrittene Benutzer vorgesehen, die dort einen angepassten Hostnamen angeben können, der zum Adressieren des Peerendpunkts verwendet werden kann. Die meisten Benutzer müssen in diesem Abschnitt nichts angeben.

### Konfigurationsdatei vervollständigen
{: #ibp-v2-apis-config-file}

Nachdem Sie alle obigen Schritte ausgeführt haben, könnte Ihre aktualisierte Konfigurationsdatei ähnlich wie im folgenden Beispiel aussehen:


```
{
    "enrollment": {
        "component": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "ca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJ0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peer1",
            "enrollsecret": "peer1pw",
            "admincerts": [
                "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzRENDQW9PZ0F3SUJBZ0lVS2Vib0drZEJqenM5bllRbkVQTWp0VG56YTVrd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE9URTNNRE13TUZvWERURTVNVEV4T1RFM01EZ3dNRm93ZmpFTE1BaKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRXVNQXNHQTFVRUN4TUVkWE5sY2pBTEJnTlZCQXNUQkc5eVp6RXdFZ1lEVlFRTEV3dGtaWEJoCmNuUnRaVzUwTVRFUU1BNEdBMVVFQXhNSFlXUnRhVzR4Y0RCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUgKQTBJQUJLbjUwdEU5TmpZb0RFNDBqalh6RUJ2T2c3Y3RtOElRd0laMnRkS29iNEwwVVhKdSs1Tkt5S2dyUk9vbApWcjBmQmg5cWZWMjl4Nms0T2dmMFNiVklBZWlqZ2ZRd2dmRXdEZ1lEVlIwUEFRSC9CQVFEQWdlQU1Bd0dBMVVkCkV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZOYWFkV0VzcGp2Smk1akpiVktiS2M3ZU1wUmhNQjhHQTFVZEl3UVkKTUJhQUZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQ2NHQTFVZEVRUWdNQjZDSEdOb1lXNWtjbUZ6TFcxaQpjQzV5WVd4bGFXZG9MbWxpYlM1amIyMHdhQVlJS2dNRUJRWUhDQUVFWEhzaVlYUjBjbk1pT25zaWFHWXVRV1ptCmFXeHBZWFJwYjI0aU9pSnZjbWN4TG1SbGNHRnlkRzFsYm5ReElpd2lhR1l1Ulc1eWIyeHNiV1Z1ZEVsRUlqb2kKWVdSdGFXNHhjQ0lzSW1obUxsUjVjR1VpT2lKMWMyVnlJbjE5TUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSURGeApDYzE1bDZUZ1dqYnhSZzlmNjczOGV0K0NZZ1I3VVpGR200VHdoQk5jQWlBNmtUMFFwbDV6WnBUN3BzM0dySXlVCmEydDRHSTQ5ZTdjUm5PMmdrSzl6Z3c9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg=="
            ]
        },
        "tls": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "tlsca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peertls",
            "enrollsecret": "peertlspw",
            "csr": {
                "hosts": [
                ]
            }
        }
    }
}
```
{:codeblock}

Die anderen Felder können leer bleiben. Nachdem Sie diese Datei vervollständigt haben, können Sie sie im Feld `config` an den Anforderungshauptteil der API zum `Erstellen eines Anordnungsknotens` oder zum `Erstellen eines Peers` übergeben.

### Administratoridentität in {{site.data.keyword.blockchainfull_notm}} Platform-Konsole importieren
{: #ibp-v2-apis-admin-console}

Wenn Sie die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole zum Betrieb Ihrer Blockchain-Komponenten benutzen möchten, dann müssen Sie Ihre Administratoridentität in Ihre Konsolenwallet importieren. Öffnen Sie die Walletanzeige in Ihrer Konsole. Klicken Sie auf die Schaltfläche **Identität hinzufügen** in der Übersichtsanzeige. Wenn Sie auf diese Schaltfläche klicken, wird eine Seitenanzeige geöffnet, in der Sie das Signierzertifikat und den privaten Schlüssel einer Identität direkt zur Konsole hinzufügen können.
- **Name:** Geben Sie einen Anzeigenamen für die Identität ein, den Sie ausschließlich zu Referenzzwecken verwenden können.
- **Zertifikat:** Laden Sie das Signierzertifikat Ihres Administrators hoch. Wenn Sie die obigen Anweisungen befolgt haben, dann finden Sie diesen Schlüssel im Ordner `$HOME/fabric-ca-client/peer-admin/msp/signcerts/`.
- **Privater Schlüssel:** Laden Sie den privaten Schlüssel Ihres Administrators hoch. Wenn Sie die hier aufgeführten Anweisungen befolgt haben, dann finden Sie diesen Schlüssel im Ordner `$HOME/fabric-ca-client/peer-admin/msp/keystore/`.

Nachdem Sie Ihre Administratoridentität importiert haben, können Sie diese Identität den Komponenten zuordnen, die Sie erstellt haben. Anschließend können Sie die Konsole verwenden, um Ihr Netz zu betreiben.
