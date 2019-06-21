---

copyright:
  years: 2019
lastupdated: "2019-03-05"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an orderering service

subcollection: blockchain

---

{:new_window: target="_blank"}
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

Importieren Sie Zertifizierungsstellen, Anordnungsservices und Peers aus anderen {{site.data.keyword.blockchainfull_notm}} Platform-Konsolen, wenn Sie sie zusammen mit Knoten, die bereits in der Konsole vorhanden sind, betreiben müssen. Sie können zum Beispiel die Option "Peer importieren" verwenden, wenn Sie Peers von Organisationen außerhalb Ihrer Konsole mit Kanälen in Ihrer Konsole verknüpfen möchten. Da {{site.data.keyword.blockchainfull_notm}} Platform 2.0-Services in mehreren {{site.data.keyword.cloud_notm}}-Umgebungen bereitgestellt werden, ist es wahrscheinlich, dass einige Serviceinstanzen nur Zertifizierungsstellen und Peers enthalten, während andere Anordnungsservices hosten. Das Importieren der unterschiedlichen Knoten in die Konsole bietet die Möglichkeit, diese verteilten Komponenten über eine einzige Benutzerschnittstelle anzuzeigen und zu betreiben, damit sie in der Blockchain-Instanz interagieren können.

Sobald Sie Knoten in der Konsole importiert haben, steht eine leistungsfähige Gruppe von operativen Funktionen zur Verfügung, z. B.:
- Dem Konsortium neue Organisationen hinzufügen
- Neue Kanäle erstellen
- Peers mit Kanälen verknüpfen
- Smart Contracts auf Peers unabhängig davon installieren, wo diese bereitgestellt wurden
- Smart Contracts auf Kanälen instanziieren
- Smart Contracts auf Kanälen aktualisieren

Wenn Sie eine Komponente importieren, müssen Sie die zugehörigen Verbindungsinformationen angeben. Wenn Sie eine Komponente importieren, importieren Sie die physische Komponente nicht selbst. Stattdessen wird anhand der Verbindungsinformationen eine Darstellung der Komponente in der Konsole erstellt, sodass die Komponente von der Konsole aus bedient werden kann.

 Nachdem Sie einen Knoten in die Konsole importiert haben, können Sie dessen Verbindungsinformationen auch über die Registerkarte **Einstellungen** des Knotens ändern.

{: note}
Bevor Sie Knoten in die Konsole importieren können, müssen sie aus der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole, in der sie erstellt wurden, exportiert werden.  Exportieren Sie einfach die Knoteninformationen aus der Konsole in eine `JSON`-Datei und importieren Sie die generierte Datei, indem Sie die Schaltfläche `JSON hochladen` im Schritt zum Importieren des Knotens in die Konsole in der Seitenanzeige verwenden.


## Starten Sie hier: Zertifikate oder Berechtigungsnachweise zusammenstellen

Bevor Sie eine vorhandene Blockchain-Komponente aus einer anderen {{site.data.keyword.blockchainfull_notm}} Platform 2.0-Serviceinstanz importieren können, müssen Sie die Administratoridentität für die Komponente in die Konsolenwallet importieren. Durch Importieren der Identität in die Konsolenwallet wird die Knotenoperation optimiert. Der Netzoperator, der den Knoten erstellt hat, muss diese Berechtigungsnachweise für Sie bereitstellen.

## Zertifizierungsstelle importieren
{: #ibp-console-import-ca}

Ein CA-Knoten ist die Blockchain-Komponente, die Zertifikate für alle Netzentitäten (Peers, Anordnungsservices, Clients usw.) ausgibt, damit diese Entitäten miteinander kommunizieren, sich authentifizieren und letztendlich Transaktionen ausführen können. Jede Organisation verfügt als Vertrauensgrundlage über eine eigene Zertifizierungsstelle. Wenn Sie einem Blockchain-Konsortium beitreten oder eines erstellen, sollten Sie Ihre Organisationen entsprechend hinzufügen. Weitere Informationen zu Zertifizierungsstellen finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-ca).  

Es gibt mehrere Gründe, warum Sie möglicherweise eine Zertifizierungsstelle in die Konsole importieren möchten. Nachdem Sie die Zertifizierungsstelle importiert haben, können Sie sie verwenden, um weitere Peers zur Peerorganisation hinzuzufügen, indem Sie auf **Benutzer registrieren** klicken. Sie können mit der Zertifizierungsstelle auch zusätzliche Administratoridentitäten für einen Peer oder Anordnungsknoten erstellen.

Um eine Zertifizierungsstelle in die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole zu importieren und zu betreiben, müssen Sie die Zertifizierungsstelle bereits aus einer anderen {{site.data.keyword.blockchainfull_notm}} Platform-Instanz exportiert haben. Wenn Sie eine Zertifizierungsstelle importieren, können Sie neue Benutzer registrieren und [Identitäten eintragen](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-enroll).

### Vorbemerkungen
{: #ibp-console-import-ca-before-you-begin}

- Stellen Sie sicher, dass Sie die JSON-Datei der Administratoridentität für die Zertifizierungsstelle bereits in die Konsolenwallet importiert haben.
- Stellen Sie sicher, dass die JSON-Datei der Zertifizierungsstelle, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise zum Importieren einer Zertifizierungsstelle  

Der Import einer Zertifizierungsstelle erfolgt über die Registerkarte **Knoten**. Klicken Sie im Abschnitt **Zertifizierungsstellen** für "Zertifizierungsstelle hinzufügen" auf das `+`-Zeichen und anschließend auf **Vorhandene Zertifizierungsstelle importieren**. Befolgen Sie die Schritte in der Seitenanzeige, die geöffnet wird, um die Verbindungsinformationen anzugeben.

- Klicken Sie auf die Schaltfläche **JSON hochladen**, um die manuelle Eingabe der Informationen zu überspringen und zu der JSON-Datei zu navigieren, die aus der Konsole exportiert wurde, in der die Zertifizierungsstelle erstellt wurde.
- Legen Sie die Administratoridentität für die Zertifizierungsstelle fest, indem Sie auf **Vorhandene Identität** klicken und die entsprechende Identität aus der Konsolenwallet auswählen.

Beim Imporieren einer Zertifizierungsstelle können Sie optional auch die Berechtigungsnachweise für die zusätzliche TLS-Zertifizierungsstelle angeben, falls eine solche vorhanden ist.  

Nachdem Sie die Zertifizierungsstelle in die Konsole importiert haben, können Sie die Zertifizierungsstelle verwenden, um neue Identitäten zu erstellen und die erforderlichen Zertifikate zu generieren, um Ihre Komponenten auszuführen und Transaktionen an das Netz zu übergeben. Weitere Informationen hierzu finden Sie im Abschnitt [Zertifizierungsstellen verwalten](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-manage-ca).

### Administratoridentitäten in die Konsolenwallet importieren

Jede {{site.data.keyword.blockchainfull_notm}} Platform-Komponente wird mit einem eigenen Signierzertifikat eines internen Komponentenadministrators bereitgestellt. Wenn der Administrator Aktionen für die Komponente ausgeführt, wird dieses Signierzertifikat der Transaktion zugeordnet und in Bezug auf die Komponentenkonfiguration geprüft. Mit diesen Schlüsseln kann der Administrator die Komponenten bedienen, beispielsweise neue Benutzer registrieren, einen neuen Kanal erstellen oder Smart Contracts für Peers installieren. Wenn Sie die Konsole verwenden möchten, um einen Anordnungsservice oder Peer zu betreiben, der mit einer anderen Konsole erstellt wurde, müssen Sie die zugeordnete Administratoridentität in die Konsolenwallet hochladen. Anschließend können Sie den importierten Knoten der Identität in der Konsolenwallet zuordnen. Diese Identitäten müssen von der Konsole aus exportiert werden, wo sie erstellt wurden, und sind erforderlich, wenn der Knoten importiert wird.

Wenn Sie eine neue Identität importieren möchten, öffnen Sie die Registerkarte **Wallet** und klicken Sie auf **Identität hinzufügen**. Klicken Sie auf **JSON hochladen**, um nach der JSON-Identitätsdatei zu suchen, die vom Netzoperator von dort aus, wo aus der Knoten erstellt wurde, exportiert wurde.

Wenn Sie in der Anzeige **Identität hinzufügen** alle erforderlichen Eingaben vorgenommen haben und auf "Übergeben" klicken, können Sie die neue Administratoridentität in der Übersichtsanzeige der Wallet anzeigen. Sie können nun beim Importieren von Zertifizierungsstellen, Peers oder Anordnungsservicekomponenten auf diese Identitäten verweisen.

## Anordnungsservice importieren
{: #ibp-console-import-orderer}

Ein Anordnungsservice ist die Blockchain-Komponente, die Transaktionen von Netzmitgliedern erfasst, die Transaktionen anordnet und sie zu Blöcken bündelt. Er stellt die allgemeine Bindung von Blockchain-Konsortien dar und muss bereitgestellt werden, wenn Sie ein Konsortium gründen, dem andere Organisationen beitreten werden. Weitere Informationen zu Anordnungsservices finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-orderer).

Wenn Sie einen Anordnungsservice in die Konsole importieren, können Sie neue Kanäle für Peers erstellen, um Transaktionen privat auszuführen.

### Vorbemerkungen
{: #ibp-console-import-orderer-before-you-begin}

- Vergewissern Sie sich, dass Sie die JSON-Datei der Administratoridentität für den Anordnungsservice bereits in die Konsolenwallet importiert haben.
- Vergewissern Sie sich, dass die JSON-Datei des Anordnungsservice, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise beim Importieren eines Anordnungsservice
Der Import eines Anordnungsservice erfolgt über die Registerkarte **Knoten**. Klicken Sie im Abschnitt für den Anordnungsservice auf das Pluszeichen (`+`) für **Anordnungsservice hinzufügen** und anschließend auf **Vorhandenen Anordnungsservice importieren**. Befolgen Sie die Schritte in der Seitenanzeige, die geöffnet wird, um die Verbindungsinformationen manuell anzugeben.
- Klicken Sie auf die Schaltfläche **JSON hochladen**, um die manuelle Eingabe der Informationen zu überspringen und zu der JSON-Datei zu navigieren, die aus der Konsole exportiert wurde, in der der Anordnungsservice erstellt wurde.
- Legen Sie die Administratoridentität für den Anordnungsservice fest, indem Sie auf **Vorhandene Identität** klicken und die Administratoridentität des Anordnungsservice in der Konsolenwallet auswählen.

Nachdem Sie den Anordnungsservice in die Konsole importiert haben, können Sie neue Organisationsmitglieder hinzufügen und den Anordnungsservice auswählen, wenn Sie neue Kanäle erstellen.

## Peer importieren
{: #ibp-console-import-peer}

Ein Peerknoten ist die Blockchain-Komponente, die ein Ledger verwaltet und einen Smart Contract ausführt, um Abfrage- und Aktualisierungsoperationen für das Ledger auszuführen. Organisationsmitglieder besitzen und verwalten Peers.  Jede Organisation, die einem Konsortium beitritt, sollte mindestens einen Peer bereitstellen. Weitere Informationen zu Peers finden Sie in der [Übersicht zu Blockchain-Komponenten](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-peer).

Nachdem Sie einen Peer in die Konsole importiert haben, können Sie Smart Contracts auf dem Peer installieren und den Peer mit anderen Kanälen in der Blockchain verknüpfen.

**Hinweis:** Wenn Sie mehr Peers zur Peerorganisation hinzufügen oder zusätzliche Administratoridentitäten für einen Peer erstellen müssen, müssen Sie die Zertifizierungsstelle des Peers importieren und dann diese Zertifizierungsstelle verwenden, um diese Operationen auszuführen.

### Vorbemerkungen
{: #ibp-console-import-peer-before-you-begin}

Bevor Sie einen Peer importieren können, müssen Sie die folgenden Informationen und Zertifikate zusammenstellen:

- Stellen Sie sicher, dass Sie die JSON-Datei der Administratoridentität für den Peer bereits in die Konsolenwallet importiert haben.
- Stellen Sie sicher, dass die JSON-Datei des Peers, die aus der Konsole exportiert wurde, in der sie erstellt wurde, verfügbar ist.

### Vorgehensweise zum Importieren eines Peers

Der Import eines Peers erfolgt über die Registerkarte **Knoten**. Klicken Sie im Abschnitt **Peers** für "Peer hinzufügen" auf das `+`-Zeichen und anschließend auf **Vorhandenen Peer importieren**. Befolgen Sie die Schritte in der Seitenanzeige, die geöffnet wird, um die Verbindungsinformationen anzugeben.
- Klicken Sie auf die Schaltfläche **JSON hochladen**, um die manuelle Eingabe der Informationen zu überspringen und zu der JSON-Datei zu navigieren, die aus der Konsole exportiert wurde, in der der Peer erstellt wurde.
- Legen Sie die Administratoridentität für den Peer fest, indem Sie auf **Vorhandene Identität** klicken und die entsprechende Identität aus der Konsolenwallet auswählen.

Nachdem Sie den Peer in die Konsole importiert haben, können Sie Smart Contracts auf dem Peer installieren und den Peer mit anderen Kanälen in der Blockchain verknüpfen.


<!-- ## Importing an MSP -->
