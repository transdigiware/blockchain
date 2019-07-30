---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Knoten importieren
{: #ibp-console-import-nodes}

Die Konsole enthält die Option zum Importieren von Knoten, die mit einer anderen {{site.data.keyword.blockchainfull}} Platform-Konsole erstellt werden.
{:shortdesc}  

**Zielgruppe:** Dieser Abschnitt richtet sich an Netzoperatoren, die für die Erstellung, Überwachung und Verwaltung des Blockchain-Netzes verantwortlich sind.

## Warum sollten Sie einen Knoten importieren?
{: #ibp-console-import-nodes-why}

Importieren Sie Zertifizierungsstellen, Anordnungsservices und Peers aus anderen {{site.data.keyword.blockchainfull_notm}} Platform-Konsolen, wenn Sie sie zusammen mit Knoten, die bereits in der Konsole vorhanden sind, betreiben müssen. Sie können zum Beispiel die Option "Peer importieren" verwenden, wenn Sie Peers von Organisationen außerhalb Ihrer Konsole mit Kanälen in Ihrer Konsole verknüpfen möchten. Da {{site.data.keyword.blockchainfull_notm}} Platform-Services in mehreren Umgebungen bereitgestellt werden, ist es wahrscheinlich, dass einige Serviceinstanzen nur Zertifizierungsstellen (CAs) und Peers enthalten, während in anderen Anordnungsservices gehostet werden. Das Importieren der unterschiedlichen Knoten in die Konsole bietet die Möglichkeit, diese verteilten Komponenten über eine einzige Benutzerschnittstelle anzuzeigen und zu betreiben, damit sie in der Blockchain-Instanz interagieren können.

Nach dem Importieren von Knoten in die Konsole steht eine leistungsfähige Gruppe von Betriebsfunktionen zur Verfügung. Dazu gehören beispielsweise die Folgenden:
- Neue Organisationen zum Konsortium hinzufügen
- Neue Kanäle erstellen
- Peers mit Kanälen verknüpfen
- Smart Contracts auf Peers unabhängig davon installieren, wo diese bereitgestellt wurden
- Smart Contracts auf Kanälen instanziieren
- Smart Contracts auf Kanälen aktualisieren

Beim Importieren eines Knotens müssen Sie die zugehörigen Verbindungsinformationen angeben. Wenn Sie eine Komponente importieren, importieren Sie die physische Komponente nicht selbst. Stattdessen wird anhand der Verbindungsinformationen eine Darstellung der Komponente in der Konsole erstellt, sodass die Komponente von der Konsole aus bedient werden kann. Ebenso wird beim Löschen eines importierten Knotens in der Konsole der Knoten selbst nicht gelöscht, da er an der Bereitstellungsposition weiterhin ausgeführt wird. Der Knoten wird lediglich aus der Konsole entfernt, in die er importiert wurde.  

Nachdem Sie einen Knoten in die Konsole importiert haben, können Sie dessen Verbindungsinformationen auch über die Registerkarte **Einstellungen** des Knotens ändern.

Bevor Sie Knoten in die Konsole importieren können, müssen sie aus der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole, in der sie erstellt wurden, exportiert werden. Der Netzoperator kann die Knoteninformationen einfach aus der Konsole in eine `JSON`-Datei exportieren.
{: note}

## Einschränkungen
{: #ibp-console-import-limitations}

- Sie können keine Knoten aus Starter Plan- oder Enterprise Plan-Netzen importieren.
- Alle Knoten, die importiert werden sollen, müssen über die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitgestellt worden sein.
- Knoten, die Sie in die Konsole importiert haben, können nicht aktualisiert werden.
- Knoten, die Sie in die Konsole importiert haben, können nicht aus dem Cluster gelöscht werden, in dem sie bereitgestellt wurden. Sie können Knoten nur aus der Konsole entfernen.
- Wenn Sie einen Knoten importieren, der in {{site.data.keyword.cloud_notm}} Private implementiert ist, müssen Sie sicherstellen, dass der von der Komponente verwendete gRPC-Web-Proxy-Port extern für die Konsole zugänglich ist. Weitere Informationen finden Sie im Abschnitt [Knoten aus {{site.data.keyword.cloud_notm}} Private importieren](#ibp-console-import-icp).

## Starten Sie hier: Zertifikate oder Berechtigungsnachweise zusammenstellen
{: #ibp-console-import-start-here}

Bevor Sie eine vorhandene Blockchain-Komponente aus einer anderen {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz importieren können, müssen Sie die Administratoridentität für die Komponente in Ihre Konsolenwallet importieren. Durch Importieren der Identität in die Konsolenwallet wird die Knotenoperation optimiert. Der Netzoperator des Netzes, in dem der Knoten bereitgestellt wurde, sollte die Knotenadministrator-ID aus der Wallet in eine JSON-Datei exportieren, die Sie [importieren](#ibp-console-import-nodes-admin-identities) können. Bei der Knotenadministrator-ID handelt es sich in der Regel um den Administrator des Peers oder der Organisation des Anordungsknotens, der bzw. die bei der Erstellung der MSP-Definition für die Organisation angegeben wurde. Diese ID müsste bereits in der Konsolenwallet enthalten sein, in der sich der Knoten befindet.

### Administratoridentitäten in die Konsolenwallet importieren
{: #ibp-console-import-nodes-admin-identities}

Jede {{site.data.keyword.blockchainfull_notm}} Platform-Komponente wird mit einem eigenen Signierzertifikat eines internen Komponentenadministrators bereitgestellt. Wenn der Administrator Aktionen für die Komponente ausgeführt, wird dieses Signierzertifikat der Transaktion zugeordnet und in Bezug auf die Komponentenkonfiguration geprüft. Mit diesen Schlüsseln kann der Administrator die Komponenten bedienen, beispielsweise neue Benutzer registrieren, einen neuen Kanal erstellen oder Smart Contracts für Peers installieren. Wenn Sie die Konsole verwenden möchten, um einen Anordnungsservice oder Peer zu betreiben, der mit einer anderen Konsole erstellt wurde, müssen Sie die zugeordnete Administratoridentität in die Konsolenwallet hochladen. Anschließend können Sie den importierten Knoten der Identität in der Konsolenwallet zuordnen. Diese Identitäten müssen von der Konsole aus exportiert werden, wo sie erstellt wurden, und sind erforderlich, wenn der Knoten importiert wird.

Wenn Sie eine neue Identität importieren möchten, öffnen Sie die Registerkarte **Wallet** und klicken Sie auf **Identität hinzufügen**. Klicken Sie auf **JSON hochladen**, um nach der JSON-Identitätsdatei zu suchen, die vom Netzoperator von dort aus, wo aus der Knoten erstellt wurde, exportiert wurde.

Wenn Sie in der Anzeige **Identität hinzufügen** alle erforderlichen Eingaben vorgenommen haben und auf "Übergeben" klicken, können Sie die neue Administratoridentität in der Übersichtsanzeige der Wallet anzeigen. Sie können nun beim Importieren von Zertifizierungsstellen, Peers oder Anordnungsservicekomponenten auf diese Identitäten verweisen.

## Zertifizierungsstelle importieren
{: #ibp-console-import-ca}

Ein CA-Knoten ist die Blockchain-Komponente, die Zertifikate für alle Netzentitäten (Peers, Anordnungsservices, Clients usw.) ausgibt, damit diese Entitäten miteinander kommunizieren, sich authentifizieren und letztendlich Transaktionen ausführen können. Jede Organisation verfügt als Vertrauensgrundlage über eine eigene Zertifizierungsstelle. Wenn Sie einem Blockchain-Konsortium beitreten oder eines erstellen, sollten Sie Ihre Organisationen entsprechend hinzufügen. Weitere Informationen zu Zertifizierungsstellen finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca).  

Es gibt mehrere Gründe, warum Sie möglicherweise eine Zertifizierungsstelle in die Konsole importieren möchten. Nach dem Importieren können Sie die Zertifizierungsstelle verwenden, um weitere Peerbenutzer in der Peerorganisation zu registrieren, indem Sie auf **Benutzer registrieren** klicken. Sie können mit der Zertifizierungsstelle auch zusätzliche Administratoridentitäten für einen Peer oder Anordnungsknoten erstellen.

Bevor eine Zertifizierungsstelle in die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole importiert werden kann, muss die Zertifizierungsstelle bereits vom Netzoperator aus der {{site.data.keyword.blockchainfull_notm}} Platform-Instanz exportiert worden sein, in der sie bereitgestellt wurde. Wenn Sie eine Zertifizierungsstelle importieren, können Sie neue Benutzer registrieren und [Identitäten eintragen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll).

### Vorbemerkungen
{: #ibp-console-import-ca-before-you-begin}

- Stellen Sie sicher, dass Sie die JSON-Datei mit der Administrator-ID der Zertifizierungsstelle bereits in Ihre Konsolenwallet importiert haben, oder dass Sie die Eintragungs-ID des Administrators und den geheimen Schlüssel für die Zertifizierungsstelle und die TLS-Zertifizierungsstelle kennen, die bei der ursprünglichen Bereitstellung der Zertifizierungsstelle angegeben wurden.
- Stellen Sie sicher, dass die JSON-Datei der Zertifizierungsstelle, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise zum Importieren einer Zertifizierungsstelle  
{: #ibp-console-import-nodes-howto-ca}

Der Import einer Zertifizierungsstelle erfolgt über die Registerkarte **Knoten**.
1. Klicken Sie auf **Zertifizierungsstelle hinzufügen** und anschließend auf **Vorhandene Zertifizierungsstelle importieren** und auf **Weiter**.
2. Wählen Sie die Position, an der die Zertifizierungsstelle ursprünglich bereitgestellt wurde, in der Dropdown-Liste **Position der Zertifizierungsstelle** aus.
3. Klicken Sie auf **Datei hinzufügen**, um die JSON-Datei der Zertifizierungsstelle hochzuladen, die aus der Konsole, in der Sie ursprünglich bereitgestellt wurde, exportiert wurde.
4. In der nächsten Anzeige können Sie die Eintragungs-ID und den geheimen Schlüssel eingeben, die zum Bereitstellen der Zertifizierungsstelle verwendet wurden.
5. Geben Sie schließlich die Eintragungs-ID und den geheimen Schlüssel für die TLS-Zertifizierungsstelle ein, die zum Bereitstellen der Zertifizierungsstelle verwendet wurden. Beim Bereitstellen einer Zertifizierungsstelle wird sowohl eine Zertifizierungsstelle als auch eine TLS-Zertifizierungsstelle bereitgestellt. Der Netzoperator kann beim Bereitstellen für die Zertifizierungsstelle und die TLS-Zertifizierungsstelle dieselbe Eintragungs-ID mit geheimem Schlüssel verwenden oder verschiedene Eintragungs-IDs mit geheimen Schlüsseln.

Nachdem Sie die Zertifizierungsstelle in die Konsole importiert haben, können Sie die Zertifizierungsstelle verwenden, um neue Identitäten zu erstellen und die erforderlichen Zertifikate zu generieren, um Ihre Komponenten auszuführen und Transaktionen an das Netz zu übergeben. Weitere Informationen hierzu finden Sie im Abschnitt [Zertifizierungsstellen verwalten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca).

## Anordnungsservice importieren
{: #ibp-console-import-orderer}

Ein Anordnungsservice ist die Blockchain-Komponente, die Transaktionen von Netzmitgliedern erfasst, die Transaktionen anordnet und sie zu Blöcken bündelt. Er stellt die allgemeine Bindung von Blockchain-Konsortien dar und muss bereitgestellt werden, wenn Sie ein Konsortium gründen, dem andere Organisationen beitreten werden. Weitere Informationen zu Anordnungsservices finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer).

Wenn Sie einen Anordnungsservice in die Konsole importieren, können Sie neue Kanäle für Peers erstellen, um Transaktionen privat auszuführen.

### Vorbemerkungen
{: #ibp-console-import-orderer-before-you-begin}

Bevor Sie einen Anordnungsservice importieren können, müssen Sie die folgenden Informationen zusammenstellen:

- Stellen Sie sicher, dass Sie die [JSON-Datei mit der Administrator-ID des Anordnungsservice](#ibp-console-import-nodes-admin-identities) bereits in Ihre Konsolenwallet importiert haben.
- Vergewissern Sie sich, dass die JSON-Datei des Anordnungsservice, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise beim Importieren eines Anordnungsservice
Der Import eines Anordnungsservice erfolgt über die Registerkarte **Knoten**.
1. Klicken Sie auf **Anordnungsservice hinzufügen** und anschließend auf **Vorhandenen Anordnungsservice importieren** und auf **Weiter**.
2. Wählen Sie die Position, an der der Anordungsservice ursprünglich bereitgestellt wurde, in der Dropdown-Liste **Position der Zertifizierungsstelle** aus.
3. Klicken Sie auf **Datei hinzufügen**, um die exportierte JSON-Datei des Anordnungsservice aus der Konsole, in der Sie ursprünglich bereitgestellt wurde, hochzuladen. Dabei ist zu beachten, dass für einen Raft-Anordnungsservice mit fünf Knoten eine einzige Datei mit den Verbindungsinformationen für alle fünf Anordnungsknoten vorliegen sollte.
4. Legen Sie die Administratoridentität für den Anordnungsservice fest, indem Sie auf **Vorhandene Identität** klicken und die Administrator-ID des Anordnungsservice auswählen, die Sie in Ihre Konsolenwallet importiert haben.

Nachdem Sie den Anordnungsservice in die Konsole importiert haben, können Sie neue Organisationsmitglieder hinzufügen und den Anordnungsservice auswählen, wenn Sie neue Kanäle erstellen.

## Peer importieren
{: #ibp-console-import-peer}

Ein Peerknoten ist die Blockchain-Komponente, die ein Ledger verwaltet und einen Smart Contract ausführt, um Abfrage- und Aktualisierungsoperationen für das Ledger auszuführen. Organisationsmitglieder besitzen und verwalten Peers. Jede Organisation, die einem Konsortium beitritt, sollte mindestens einen Peer bereitstellen (für Hochverfügbarkeit mindestens zwei). Weitere Informationen zu Peers finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer).

Nachdem Sie einen Peer in die Konsole importiert haben, können Sie Smart Contracts auf dem Peer installieren und den Peer mit anderen Kanälen in der Blockchain verknüpfen.

**Hinweis:** Wenn Sie mehr Peers zur Peerorganisation hinzufügen oder zusätzliche Administratoridentitäten für einen Peer erstellen müssen, müssen Sie die Zertifizierungsstelle des Peers importieren und dann diese Zertifizierungsstelle verwenden, um diese Operationen auszuführen.

### Vorbemerkungen
{: #ibp-console-import-peer-before-you-begin}

Bevor Sie einen Peer importieren können, müssen Sie die folgenden Informationen zusammenstellen:

- Stellen Sie sicher, dass Sie die [JSON-Datei mit der Peeradministrator-ID](#ibp-console-import-nodes-admin-identities) bereits in Ihre Konsolenwallet importiert haben.
- Stellen Sie sicher, dass die JSON-Datei des Peers, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise zum Importieren eines Peers
{: #ibp-console-import-peer-howto}

Der Import eines Peers erfolgt über die Registerkarte **Knoten**.
1. Klicken Sie auf **Peer hinzufügen** und anschließend auf **Vorhandenen Peer importieren** und auf **Weiter**.
2. Wählen Sie die Position, an der der Peer ursprünglich bereitgestellt wurde, in der Dropdown-Liste **Position der Zertifizierungsstelle** aus.
3. Klicken Sie auf **Datei hinzufügen**, um die JSON-Datei aus der Konsole, in der sie ursprünglich bereitgestellt wurde, hochzuladen.
4. Legen Sie die Administratoridentität für den Peer fest, indem Sie auf **Vorhandene Identität** klicken und die Peer-Administrator-ID auswählen, die Sie in Ihre Konsolenwallet importiert haben.  

Nachdem Sie den Peer in die Konsole importiert haben, können Sie Smart Contracts auf dem Peer installieren und den Peer mit anderen Kanälen in der Blockchain verknüpfen.

## MSP-Definition einer Organisation importieren
{: #ibp-console-import-msp}

Sie müssen die MSP-Definition einer Organisation importieren, wenn der MSP über die Konsole einer anderen {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz bereitgestellt wurde und wenn Sie beabsichtigen, einen Kanal zu erstellen oder zu aktualisieren, der diese MSP-Definition verwendet. Wenn Sie einen Peer oder Anordnungsservice erstellen möchten, der Teil der MSP-Definition einer Organisation ist, die in einer anderen Konsole bereitgestellt wurde, müssen Sie ebenfalls die MSP-Definition und die zugehörige MSP-Administrator-ID importieren. Der Netzoperator, von dem die MSP-Definition erstellt wurde, muss die MSP-Definition der Organisation und die zugehörige Administrator-ID in JSON-Dateien exportieren und für Sie freigeben.

- Wenn Sie einen Peer- oder Anordnungsservice erstellen möchten, der Teil der Organisation ist, importieren Sie die zugehörige MSP-Administrator-ID in Ihre Wallet.
- Das Importieren der MSP-Definition der Organisation erfolgt über die Registerkarte **Organisationen**.
- Klicken Sie auf **MSP-Definition importieren**, um die JSON-Datei hochzuladen.
- (Optional) Wenn Sie die MSP-Administrator-ID in Ihre Wallet importiert haben, aktivieren Sie das Kontrollkästchen `Ich verfüge über eine Administratoridentität für die MSP-Definition`. Wenn Sie diese Option nicht aktivieren und anschließend versuchen, einen Peer oder Anordnungsservice zu erstellen, wird diese MSP-Definition der Organisation nicht in der Dropdown-Liste für MSP aufgeführt.

## Knoten aus {{site.data.keyword.cloud_notm}} Private importieren
{: #ibp-console-import-icp}

Sie können Knoten, die mit {{site.data.keyword.cloud_notm}} Private erstellt wurden, in Konsolen importieren, die in anderen {{site.data.keyword.cloud_notm}} Private-Clustern oder in {{site.data.keyword.cloud_notm}} bereitgestellt wurden. Dabei muss jedoch sichergestellt werden, dass der in der gRPC-URL Ihrer Knoten verwendete Port für Komponenten außerhalb des Clusters zugänglich ist. Wenn Sie {{site.data.keyword.cloud_notm}} Private hinter einer Firewall bereitstellen, müssen Sie den Durchgriff von der Konsole außerhalb des Clusters auf Ihre Knoten aktivieren (z. B. über Whitelisting).

Ein Beispiel für einen Peer, der aus {{site.data.keyword.cloud_notm}} Private exportiert wurde, enthält die folgende JSON-Beispieldatei. Um die Kommunikation mit dem Peer von einer anderen Konsole aus zu ermöglichen, müssen Sie sicherstellen, dass der Port `grpcwp_url` (in diesem Beispiel der Port 32403) für externen Datenverkehr freigegeben ist.

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\Sicherstellen, dass Port 32403 extern zugänglich ist
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
