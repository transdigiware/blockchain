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

# Konsole verwalten
{: #console-icp-manage}

Nachdem Sie Ihre Konsole in {{site.data.keyword.cloud}} Private bereitgestellt habe, können Sie über die Konsole Benutzer hinzufügen bzw. entfernen und auf APIs zum Steuern Ihres Netzes und Ihrer Konsole zugreifen. Darüber hinaus können Sie die Protokolle für Ihre Konsole aufrufen und anpassen.
{:shortdesc}

## Benutzer in der Konsole verwalten
{: #console-icp-manage-users}

Der Benutzer, der die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitstellt, gilt als Konsolenadministrator. Dieser Administrator kann über die Registerkarte **Benutzer** in der Konsole weitere Benutzer hinzufügen und ihnen Zugriff auf die Konsole gewähren. Jedem Benutzer, der auf die Konsole zugreift, muss eine Zugriffsrichtlinie mit einer definierten Benutzerrolle zugewiesen werden. Die Richtlinie bestimmt, welche Aktionen der Benutzer innerhalb der Konsole ausführen kann. Dem Konsolenadministrator wird standardmäßig die Rolle **Manager** für die Konsole zugeordnet. Anderen Benutzern können die Rollen **Manager**, **Schreibberechtigter** oder **Leseberechtigter** zugewiesen werden, wenn Sie von einem Konsolenmanager zur Konsole hinzugefügt werden. Die Benutzer können auch [mithilfe von APIs](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis) verwaltet werden.

| Rolle | Funktionen |
|--------|----------|
| Manager | Als Manager verfügen Sie über Berechtigungen, die über die Rolle "Schreibberechtigter" hinausgehen. Sie können alles tun, was ein Leseberechtigter und ein Schreibberechtigter tun können, sowie: <ul><li>Neue Komponenten über die Konsole oder die APIs bereitstellen.</li><li>Bereitgestellte Komponenten über die Konsole oder die APIs löschen.</li><li>Benutzer hinzufügen/entfernen und Zugriffsrichtlinien für Benutzer ändern.</li><li>Protokollierungsstufen der Konsole über die Konsole oder die APIs ändern.</li><li>Konsole über eine API neu starten.</li></ul> |
| Schreibberechtigter | Als Schreibberechtigter verfügen Sie über Berechtigungen, die über die Rolle "Leseberechtigter" hinausgehen, darunter: <ul><li>Komponenten über die Konsole oder die APIs importieren.</li><li>Importierte Komponenten über die Konsole oder die APIs entfernen.</li><li>Benutzer für eine Zertifizierungsstelle registrieren.</li><li> Benachrichtigungen über die Konsole oder die APIs hinzufügen oder entfernen.</li></ul>  |
| Leseberechtigter | Als Leseberechtigter können Sie schreibgeschützte Aktionen ausführen, darunter: <ul><li>Die Konsolen-Benutzerschnittstelle anzeigen.</li><li>Das Konsolenprotokoll anzeigen.</li><li>Komponenten exportieren.</li><li>Gewünschte GET-API ausgeben.</li></ul> | |

Berechtigungen sind kumulativ. Wenn Sie die Rolle **Manager** auswählen, kann der betreffende Benutzer auch alle Aktionen der Rollen **Schreibberechtigter** und **Leseberechtigter** ausführen, d. h. Sie müssen diese Rollen nicht zusätzlich auswählen. Ebenso kann ein Benutzer mit der Rolle **Schreibberechtigter** alle Aktionen der Rolle des **Leseberechtigten** ausführen.

### Benutzerkennwort verwalten
{: #console-icp-manage-user-pw}

Das mit dem [geheimen Passwortschlüssel](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) gespeicherte Kennwort wird anschließend beim Konfigurieren an die Konsole übergeben und wird zum Standardkennwort für die Konsole. Alle Benutzer müssen dieses Kennwort für die erste Anmeldung an der Konsole verwenden. Der Konsolenmanager kann auch ein neues Standardkennwort festlegen. Klicken Sie auf der Registerkarte **Benutzer** der Konsole auf den Link **Konfiguration aktualisieren** unter der Kachel mit dem Namen Ihres Authentifizierungsservice. Im rechten Fensterbereich können Sie das Standardkennwort für neue Benutzer der Konsole anzeigen oder ändern.

Sie müssen das Standardkennwort (oder das von Ihnen neu festgelegte Kennwort) den Benutzern mitteilen, damit sie sich an der Konsole anmelden können. Jeder Benutzer muss das Kennwort bei der ersten Anmeldung ändern.
{:note}

Benutzer mit der Rolle "Manager" können die Kennwörter für andere Benutzer zurücksetzen. Klicken Sie auf der Registerkarte **Benutzer** der Konsole auf die drei vertikal angeordneten Punkte am Ende der Zeile für einen bestimmten Benutzer und anschließend auf **Kennwort zurücksetzen**. Das Kennwort für diesen Benutzer wird in der Konsole auf das Standardkennwort zurückgesetzt. Diesen Vorgang können Sie im rechten Fensterbereich überprüfen und bestätigen. Jeder Benutzer (unabhängig von der zugeordneten Benutzerrolle) kann das eigene Kennwort jederzeit ändern. Klicken Sie auf das Avatarsymbol in der rechten oberen Ecke und klicken Sie anschließend auf **Kennwort ändern**.

Nachdem Sie der Konsole neue Benutzer hinzugefügt haben, kann es sein, dass Benutzer nicht alle Knoten, Kanäle oder Chaincodes, die von anderen Benutzern bereitgestellt werden, anzeigen können. Um mit diesen Komponenten arbeiten zu können, müssen die einzelnen Benutzer die zugehörigen Identitäten in ihre eigene Konsolenwallet importieren. Weitere Informationen hierzu finden Sie unter [Identitäten in der Konsolenwalletspeichern](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Benutzerrolle ändern
{: #console-icp-manage-reset-user-pw}

Ein Benutzer mit der Rolle "Manager" kann die Rollen anderer Konsolenbenutzer ändern und ihnen dadurch Zugriff auf mehr bzw. weniger Konsolenfunktionen gewähren. Klicken Sie auf der Registerkarte **Benutzer** der Konsole auf die drei vertikal angeordneten Punkte am Ende der betreffenden Benutzerzeile und klicken Sie anschließend auf **Authentifizierten Benutzer aktualisieren**. Wählen Sie die neue Rolle für den Benutzer im rechten Fensterbereich aus. Die Rolle des Benutzers wird beim nächsten Anmelden an der Konsole geändert.

### Benutzer in der Konsole löschen
{: #console-icp-manage-icp-remove-user}

Wenn Sie über die Benutzerrolle "Manager" verfügen, können Sie anderen Benutzern den Zugriff auf die Konsole entziehen. Klicken Sie auf der Registerkarte **Benutzer** auf das Kontrollkästchen am Anfang der betreffenden Benutzerzeile. Klicken Sie anschließend auf das Symbol **Papierkorb** am oberen Ende der Tabelle neben der Schaltfläche **Neue Benutzer hinzufügen**. Nachdem Sie den Benutzer aus der Tabelle entfernt haben, kann sich der Benutzer nicht mehr an der Konsole anmelden.

## {{site.data.keyword.blockchainfull_notm}}-APIs verwenden
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform stellt REST-konforme APIs bereit, mit denen Sie Blockchain-Knoten über die Konsole importieren, bearbeiten und entfernen können. Darüber hinaus können Sie mit APIs die Einstellungen, die Protokollierung und die Benachrichtigungen Ihrer Konsole verwalten. Verwenden Sie die folgenden Anleitungen zur Verwendung der APIs, die von einer in {{site.data.keyword.cloud_notm}} Private bereitgestellten Konsole zur Verfügung gestellt werden.

### Voraussetzungen
{: #console-icp-manage-prereqs}

Für die Verwendung der APIs müssen Sie die folgenden Informationen sammeln: 

- **API-Endpunkt** der Konsole. Über diese URL rufen Sie die Konsole in Ihrem Browser auf. Diese URL ist im Abschnitt **Hinweise:** der Übersichtsanzeige für das Helm-Release angegeben.
- Ein Benutzername und ein Kennwort für den Zugang zur Konsole. Zum Erstellen eines API-Schlüssels ist ein Konto mit einer [Managerrolle](#console-icp-manage-users) erforderlich.

### API-Schlüssel verwenden
{: #console-icp-manage-create-api-key}

Jede Konsole stellt ein eigenes Identitäts- und Zugriffsmanagement bereit. Mit Ihrem Benutzernamen und Kennwort für die Konsole können Sie einen API-Schlüssel und einen geheimen Schlüssel zum Autorisieren Ihrer API-Aufrufe erstellen.

Jedem API-Schlüssel ist eine Rolle zugeordnet, die festlegt, welche Funktionen der Benutzer ausführen darf. Die API-Schlüssel verwenden die bereits vorhandenen Benutzerzugriffsrollen der Benutzer, die sich mit Benutzername und Kennwort bei der Konsole anmelden. Eine Liste der Aktionen, die mit den einzelnen Benutzerrollen ausgeführt werden können, enthält die Tabelle im Abschnitt [Benutzer verwalten](#console-icp-manage-users). Da nur Managerrollen zum Erstellen von API-Schlüsseln berechtigt sind,  müssen API-Schlüssel für schreibberechtigte und leseberechtigte Benutzer von einem Konsolenadministrator erstellt werden. API-Schlüssel laufen niemals ab. Daher sollten nicht mehr verwendete API-Schlüssel vom Konsolenadministrator gelöscht werden.

Zum Verwalten Ihrer API-Schlüssel stehen die drei folgenden APIs zur Verfügung:
- [API-Schlüssel erstellen](#console-icp-manage-create-api-key)
- [API-Schlüssel anzeigen](#console-icp-manage-view-api-keys)
- [API-Schlüssel löschen](#console-icp-manage-delete-api-keys)

Verwenden Sie die unten angegebene Anforderung, um einen API-Schlüssel und einen geheimen Schlüssel zu erstellen. Mit dem erstellten Schlüssel und dem zugehörigen geheimen Schlüssel können Sie anschließend andere APIs verwenden. Der geheime API-Schlüssel wird nicht in der Konsole gespeichert; er muss vom Benutzer gespeichert werden.

#### API-Schlüssel erstellen
{: #console-icp-manage-create-api-key-api}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Felder im Anforderungshauptteil** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` (Mindestens ein Wert ist erforderlich.) </li><li>`string` (Optional)</li></ul>|
| **Felder im Antworthauptteil** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Speichern: der Schlüssel wird nicht gespeichert.</li><li>`["<role>"]`</li></ul>|
| Erforderliche Berechtigung | manager |

#### Beispiel für curl-Anforderung: API-Schlüssel erstellen
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

### API-Schlüssel für die Konsole verwalten
{: #console-icp-manage-api-keys}

Nachdem Sie einen API-Schlüssel und einen geheimen Schlüssel erstellt haben, können Sie die APIs verwenden, um die von Ihnen erstellten Schlüssel anzuzeigen oder zu entfernen. Die Schlüssel und geheimen Schlüssel laufen erst ab, wenn sie gelöscht werden.

#### API-Schlüssel anzeigen
{: #console-icp-manage-view-api-keys}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Felder im Antworthauptteil** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` UNIX-Zeitmarke in Millisekunden</li><li>`string`</li></ul>|
| Erforderliche Berechtigung | reader |

#### Beispiel für curl-Anforderung: API-Schlüssel anzeigen
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api-schlüssel>:<geheimer_api-schlüssel>
```

#### API-Schlüssel löschen
{: #console-icp-manage-delete-api-keys}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| Erforderliche Berechtigung | manager |

#### Beispiel für curl-Anforderung: API-Schlüssel löschen
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api-schlüssel>:<geheimer_api-schlüssel>
```

### Benutzer mit APIs verwalten
{: #console-icp-manage-users-apis}


Die APIs können auch verwendet werden, um Benutzer aufzulisten, hinzuzufügen oder zu entfernen, die sich an der Konsole anmelden bzw. die APIs der Konsole aufrufen können. Außerdem können die Benutzerrollen bearbeitet werden, um die Zugriffsebene der Benutzer zu ändern. Die folgenden APIs sind verfügbar:

- [Benutzer auflisten](#console-icp-manage-list-users-api)
- [Benutzer bearbeiten](#console-icp-manage-edit-users-api)
- [Benutzer hinzufügen](#console-icp-manage-add-users-api)
- [Benutzer entfernen](#console-icp-manage-remove-users-api)

Für die APIs zum Steuern der Benutzer und Einstellungen Ihrer Konsole und zum Verwalten der Knoten in Ihrem Netz müssen Sie ein Flag ``-K`` oder ``--insecure`` hinzufügen, um das obligatorische Angeben eines TLS-Zertifikats zu umgehen. Sie können das TLS-Zertifikat auch über Ihren Browser aus der Konsole herunterladen.

#### Benutzer auflisten
{: #console-icp-manage-list-users-api}


| **Anforderung** |  |
|-------------|-----------|
| Pfad | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **Felder im Antworthauptteil** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` Benutzer-ID</li><li>`string` E-Mail-Adresse</li><li>`["<role>"]`</li><li>`number` UNIX-Zeitmarke in Millisekunden</li></ul>|
| Erforderliche Berechtigung | reader |

#### Beispiel für curl-Anforderung: Benutzer auflisten
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api-schlüssel>:<geheimer_api-schlüssel> \
  -K
```

#### Benutzer bearbeiten
{: #console-icp-manage-edit-users-api}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **Felder im Anforderungshauptteil** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` Benutzer-ID </li><li>`["reader", "writer", "manager"]` (Mindestens ein Wert ist erforderlich.)</li></ul> |
| **Felder im Antworthauptteil** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` Benutzer-ID</li></ul>|
| Berechtigung| manager |

#### Beispiel für curl-Anforderung: Benutzer bearbeiten
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api-schlüssel>:<geheimer_api-schlüssel> \
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

#### Benutzer hinzufügen
{: #console-icp-manage-add-users-api}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **Felder im Anforderungshauptteil** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` (Mindestens ein Wert ist erforderlich.) </li><li>`string` (Optional)</li></ul>|
| Berechtigung | manager |

#### Beispiel für curl-Anforderung: Benutzer hinzufügen
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api-schlüssel>:<geheimer_api-schlüssel> \
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

#### Benutzer entfernen
{: #console-icp-manage-remove-users-api}

| **Anforderung** |  |
|-------------|-----------|
| Pfad | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **Felder im Anforderungshauptteil** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` Benutzer-ID</li></ul>|
| **Felder im Antworthauptteil** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` Benutzer-ID</li></ul>|
| Berechtigung | manager |

#### Beispiel für curl-Anforderung: Benutzer entfernen
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api-schlüssel>:<geheimer_api-schlüssel> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### Komponenten mit den APIs verwalten

Sie können alle verfügbaren APIs in der [API-Referenz für {{site.data.keyword.blockchainfull_notm}} Platform](https://test.cloud.ibm.com/apidocs/blockchain) anzeigen.

Da Sie unter Verwendung der APIs mit Ihrer Konsole in {{site.data.keyword.cloud_notm}} Private kommunizieren, müssen Sie die von {{site.data.keyword.cloud_notm}} bereitgestellte Autorisierung zusammen mit der von Ihrer Konsole bereitgestellten Autorisierung verwenden. Ersetzen Sie die Trägerautorisierung (``Bearer Auth``) in der API-Referenz durch ``-u <api_key>:<api_secret>``. Außerdem müssen Sie ein Flag ``-K`` oder ``--insecure`` zu dem Befehl hinzufügen oder das TLS-Zertifikat der Konsole über Ihren Browser herunterladen. Die Registerkarte **Ausprobieren** kann nicht zum Testen der APIs für Netze in {{site.data.keyword.cloud_notm}} Private verwendet werden.

Der API-Aufruf im folgenden Beispiel gibt Informationen zu allen Komponenten zurück, die in einer Serviceinstanz von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} ausgeführt werden.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
Für eine in {{site.data.keyword.cloud_notm}} Private bereitgestellte Konsole würde dieser API-Aufruf etwa so lauten:
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

Mithilfe der APIs können Sie Knoten in dem Cluster erstellen, auf dem Ihre Konsole bereitgestellt wird, sowie Knoten aus anderen Clustern oder aus {{site.data.keyword.cloud_notm}} importieren. Weitere Informationen finden Sie in [Netz mit APIs erstellen](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) und in [Netz mit APIs importieren](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Protokolle anzeigen
{: #icp-console-manage-logs}

Beim Arbeiten mit der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole kann es erforderlich werden, Protokolle anzuzeigen, um ein Problem zu beheben.

### Konsolenprotokolle anzeigen
{: #console-icp-manage-console-logs}

Sie können problemlos auf die Konsolenprotokolle zugreifen, wenn Sie Fehler beheben müssen, die bei der Verwendung der Konsole und der Knoten aufgetreten sind. Sie können die Anzahl der Protokolle, die von der Konsole erfasst werden, auch erhöhen oder verringern, indem Sie die Protokollierungsstufe entsprechend festlegen. Die Protokolle der Konsole werden getrennt von den [Knotenprotokollen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs) erfasst, die von {{site.data.keyword.cloud_notm}} Private gesammelt werden.

Navigieren Sie zu der Registerkarte **Einstellungen** im Konsolenbrowser, um die Protokollierungseinstellungen zu ändern. Die Konsolenprotokolle werden aus zwei separaten Quellen erfasst:

  * **Clientprotokollierung:** Diese Protokolle werden erfasst, wenn Befehle von Ihrem Browser an die Konsole gesendet werden.
  * **Serverprotokollierung:** Diese Protokolle werden erfasst, wenn die Konsole Befehle an Ihre Knoten und von der Konsolenimplementierung sendet. Zu diesen Protokollen gehört die Protokollausgabe von Hyperledger Fabric.

Legen Sie die Menge der zu erfassenden Protokolle in der Dropdown-Liste unter den einzelnen Protokolltypen fest. Beispiel: **Fehler** und **Warnung** erfassen die wenigsten Protokolle, während mit **Debug** und **Alle** die meisten Protokolle erfasst werden.

Sie können die Konsolenprotokolle nur anzeigen, wenn Sie als Konsolenadministrator angemeldet sind. Wenn Sie die Protokolle auf der Registerkarte **Einstellungen** anzeigen möchten, ersetzen Sie das Wort `settings` in der Browser-URL durch `api/v1/logs`. Wenn Ihre Konsolen-URL z. B. `localhost:3001/settings` lautet, können Sie Ihre Protokolle anzeigen, indem Sie zu `localhost:3001/api/v1/logs` navigieren. Client- und Serverprotokolle werden in separaten Dateien erfasst. Die neuesten Protokolle befinden sich oben auf der Seite.

### Konsolenprotokolle anzeigen
{: #console-icp-manage-node-logs}

Komponentenprotokolle können über die Befehlszeile mit den [Befehlen der CLI "kubectl"](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} oder über [Kibana](https://www.elastic.co/products/kibana){: external} angezeigt werden, das in Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster enthalten ist.

- Mit dem Befehl `kubectl logs` können Sie die Containerprotokolle innerhalb des Pods anzeigen. Führen Sie (falls noch nicht geschehen) die Anweisungen in [Installing the Kubernetes CLI (kubectl)](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} aus. Falls Sie den Podnamen nicht genau kennen, führen den folgenden Befehl aus, um Ihre Podliste anzuzeigen.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Rufen Sie anschließend mit dem folgenden Befehl die Protokolle für den Knotencontainer ab, der sich im Pod befindet:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Ersetzen Sie `<pod_name>` durch den Namen Ihres Pods aus der obigen Befehlsausgabe.  
  Ersetzen Sie `<node>` durch `ca`, `peer` oder `orderer`, um die Protokolle für Ihren Knoten anzuzeigen.  
  Ersetzen Sie `<node>` durch `fluentd`, um die Protokolle für Ihre Smart Contracts anzuzeigen.

  Weitere Informationen zum Befehl `kubectl logs` finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- Alternativ können Sie zum [Aufrufen der Protokolle](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} auch Ihre  {{site.data.keyword.cloud_notm}} Private-Konsole verwenden, die die Protokolle in Kibana öffnet.

  **Hinweis:** Wenn Sie Ihre Protokolle in Kibana anzeigen, empfangen Sie möglicherweise die Antwort `Keine Ergebnisse gefunden`. Diese Bedingung kann auftreten, wenn {{site.data.keyword.cloud_notm}} Private die IP-Adresse Ihres Workerknotens als Hostnamen verwendet. Entfernen Sie zur Lösung dieses Problems oben in der Anzeige den Filter, der mit `node.hostname.keyword` beginnt. Anschließend sind die Protokolle sichtbar.

### Protokolle für Ihren Smart-Contract-Container anzeigen
{: #console-icp-manage-container-logs}

Bei Problemen mit Ihrem Smart Contract können Sie die Smart Contract- oder Chaincode-Containerprotokolle aufrufen, um die Probleme zu beheben. Sie können den folgenden Befehl ausführen, um die Smart Contract-Containerprotokolle anzuzeigen:

```
kubectl  logs -f <peer_pod> -c fluentd
```
{:codeblock}

Ersetzen Sie `<peer_pod>` durch den Namen des Peer-Pods, in dem der Chaincode ausgeführt wird. Verwenden Sie den Befehl `kubectl get po`, um die Liste der aktiven Pods abzurufen.

## Programmkorrekturen für Ihre Knoten installieren
{: #console-icp-manage-patch}

Die zugrunde liegenden {{site.data.keyword.IBM_notm}} Hyperledger Fabric-Docker-Images für den Peer, die Zertifizierungsstelle und die Anordnungsknoten müssen im Laufe der Zeit möglicherweise aktualisiert werden (z. B. mit Sicherheitsupdates oder durch ein neues Punktrelease für Fabric). Sie können Ihre Fabric-Images aktualisieren, während Sie ein Upgrade des {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramms durchführen.

Nach der Durchführung eines Upgrades für Ihr Helm-Diagramm wird der Text **Patch verfügbar** auf einer Knotenkachel angezeigt, wenn ein Update für eine Komponente zur Verfügung steht. Das Patch (Programmkorrektur) kann jederzeit auf einem Knoten installiert werden. Diese Programmkorrekturen sind optional, werden aber empfohlen. Knoten, die in die Konsole importiert wurden, können nicht aktualisiert werden.

Programmkorrekturen werden auf Knoten nacheinander angewendet. Während der Anwendung der Programmkorrektur steht der Knoten für die Verarbeitung von Anforderungen und Transaktionen nicht zur Verfügung. Um Unterbrechungen der Serviceverfügbarkeit zu vermeiden, sollten Sie deshalb sicherstellen, einen weiteren Knoten desselben Typs verfügbar zu halten, der die Anforderungen verarbeiten kann, sofern dies möglich ist. Die Installation einer Programmkorrektur auf einem Knoten dauert ca. eine Minute. Nach Abschluss der Aktualisierung ist der Knoten wieder zur Verarbeitung von Anforderungen bereit.
{:note}

Öffnen Sie zum Anwenden einer Programmkorrektur auf einen Knoten die Kachel des betreffenden Knotens und klicken Sie dann auf die Schaltfläche **Programmkorrektur installieren**. Knoten, die Sie in die Konsole importiert haben, können nicht aktualisiert werden.
