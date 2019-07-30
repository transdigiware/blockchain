---

copyright:
  years: 2019

lastupdated: "2019-06-21"

keywords: getting started tutorials, create a CA, enroll, register, create an MSP, wallet, create a peer, create ordering service, Raft, join a network, system channel

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

# Lernprogramm zum Teilnehmen an einem Netz
{: #ibp-console-join-network}

{{site.data.keyword.blockchainfull}} Platform ist ein Blockchain-as-a-Service-Angebot, mit dem Sie Blockchain-Anwendungen und -Netze entwickeln, bereitstellen und betreiben können. In der [Übersicht zu den Blockchain-Komponenten](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview) erfahren Sie mehr zu den Blockchain-Komponenten und darüber, wie sie miteinander interagieren. Dieses Lernprogramm ist der zweite Teil der [Lernprogrammreihe für Beispielnetze](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-sample-tutorial), in dem die Vorgehensweise zum Erstellen von Knoten in der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole und dem Verbinden dieser Knoten mit einem Blockchain-Konsortium beschrieben wird, das in einem anderen Cluster gehostet ist.
{:shortdesc}

Wenn Sie die Betatestversion von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} verwenden, dann stimmen wahrscheinlich manche Anzeigen in Ihrer Konsole nicht mit den Darstellungen in der aktuellen Dokumentation überein, die stets auf den Stand der allgemein verfügbaren Serviceinstanz aktualisiert wird. Wenn Sie die Betaserviceinstanz verwenden und alle Vorteile der neuesten Funktionalität nutzen möchten, sollten Sie eine neue GA-Serviceinstanz bereitstellen. Befolgen Sie hierzu die Anweisungen in der [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
{: important}

**Zielgruppe:** Dieser Abschnitt richtet sich an Netzoperatoren, die für die Erstellung, Überwachung und Verwaltung des Blockchain-Netzes verantwortlich sind.  

Wenn Sie die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole noch nicht verwendet haben, um Komponenten mithilfe von {{site.data.keyword.cloud_notm}} Kubernetes Service in einem Kubernetes-Cluster bereitzustellen, lesen Sie die Informationen im Abschnitt [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) (sofern Sie mit einem {{site.data.keyword.cloud_notm}}-Cluster arbeiten) oder in der [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp) (sofern Sie mit {{site.data.keyword.cloud_notm}} Private die Bereitstellung in einem anderen Cloud-Provider als {{site.data.keyword.cloud_notm}} vornehmen). Beachten Sie, dass die Konsole selbst sich nicht in Ihrem Cluster befindet. Es handelt sich dabei um ein Tool, das Sie zur Bereitstellung von Komponenten in Ihrem Cluster verwenden können.

Unabhängig davon, ob Sie Komponenten in einem gebührenpflichtigen oder kostenlosen Kubernetes-Cluster bereitstellen, sollten Sie sorgfältig darauf achten, welche Ressourcen zur Verfügung stehen, wenn Sie Knoten bereitstellen und Kanäle erstellen. Es liegt in Ihrer Verantwortung, Ihren Kubernetes-Cluster zu verwalten und bei Bedarf zusätzliche Ressourcen zu implementieren. Obwohl Komponenten in einem kostenlosen {{site.data.keyword.cloud_notm}}-Cluster erfolgreich bereitgestellt werden können, gilt dennoch Folgendes: Je mehr Komponenten Sie hinzufügen, umso langsamer werden die Komponenten ausgeführt. Weitere Informationen zur Komponentengröße und zur Interaktion der Konsole mit Ihrem {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster finden Sie im Abschnitt [Ressourcen zuordnen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction). Wenn Sie mit {{site.data.keyword.cloud_notm}} Private die Bereitstellung in einem anderen Cloud-Provider vornehmen, erfahren Sie in der Dokumentation zu diesem Provider, wie Sie Ihre Ressourcen dort überwachen können.

## Lernprogrammreihe für Beispielnetze
{: #ibp-console-join-network-structure}

Diese dreiteilige Lernprogrammreihe führt Sie durch den Prozess der Erstellung und Verbindung eines relativ einfachen Hyperledger Fabric-Netzes mit mehreren Knoten, indem Sie mithilfe der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole ein Netz in Ihrem Kubernetes-Cluster bereitstellen und einen Smart Contract installieren und instanziieren. Dabei ist zu beachten, dass dieser Prozess im vorliegenden Lernprogramm anhand eines gebührenpflichtigen {{site.data.keyword.cloud_notm}}-Kubernetes-Clusters dargestellt wird. Der gleiche grundlegende Ablauf gilt jedoch (mit geringfügigen Einschränkungen) auch für kostenlose Cluster. So ist es beispielsweise in einem kostenlosen Cluster nicht möglich, die Größe von Knoten zu ändern.

* Das [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) führt Sie durch das Hosten eines Netzes, indem ein Anordnungsservice und ein Peer erstellt werden.
* Im **Lernprogramm zum Teilnehmen an einem Netz** erfahren Sie, wie Sie durch Erstellen eines Peers und Verknüpfen des Peers mit einem Kanal an einem vorhandenen Netz teilnehmen können.
* Im [Lernprogramm zum Bereitstellen eines Smart Contract im Netz](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts) erfahren Sie, wie Sie einen Smart Contract schreiben und in Ihrem Netz bereitstellen.

Sie können die Schritte in diesen Lernprogrammen verwenden, um ein Netz mit mehreren Organisationen in einem Cluster für Entwicklungs- und Testzwecke zu erstellen. Verwenden Sie das Lernprogramm zum **Erstellen eines Netzes**, wenn Sie ein Blockchain-Konsortium gründen möchten, indem Sie einen Anordnungsservice erstellen und Organisationen hinzufügen. Verwenden Sie das Lernprogramm zum **Teilnehmen an einem Netz**, um einen Peer mit dem Netz zu verbinden. Indem Sie die Lernprogramme mit unterschiedlichen Konsortiumsmitgliedern durchführen, können Sie ein wirklich **verteiltes** Blockchain-Netz erstellen.

In diesem Lernprogramm soll gezeigt werden, wie Sie einen Peer mit einem **vorhandenen** Netz verknüpfen. Dabei wird vorausgesetzt, dass bereits ein Anordnungsservice in Ihrem oder in einem anderen {{site.data.keyword.blockchainfull_notm}} Platform-Cluster erstellt wurde. Wenn kein Netz vorhanden ist, dem Sie beitreten können, erfahren Sie im [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network), wie Sie ein Netz erstellen können. Das Lernprogramm zum **Teilnehmen an einem Netz** führt Sie durch die Schritte zum Erstellen der folgenden Blockchain-Komponenten für `Org2`, die im blauen Kasten markiert sind:
![Struktur zum Teilnehmen an einem Netz](../images/ibp2-join-network.svg "Teilnehmen an einem Netz")  

Führen Sie die Schritte im Lernprogramm zum **Teilnehmen an einem Netz** aus, um die folgenden Komponenten zu erstellen und die folgenden Aktionen auszuführen:

* **Eine Peerorganisation** `Org2`  
  Erstellen Sie die MSP-Definition (MSP, Membership Services Provider) für "Org12", die die Organisation `Org2` definiert.
* **Ein Peer** `Peer Org2`   
  Das Ledger (`Ledger x` in der Abbildung oben) wird von verteilten Peers verwaltet. Der Peer wird mit [CouchDB](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html){: external} als Statusdatenbank bereitgestellt.
* **Eine Zertifizierungsstelle (CA)** `Org2 CA`
  Eine Zertifizierungsstelle (CA) ist der Knoten, der Zertifikate für alle Organisationsadministratoren und für die Knoten ausgibt, die einer Organisation zugeordnet sind. Sie erstellen eine Zertifizierungsstelle für die Peerorganisation `Org2`.
* **Einem Kanal beitreten**
  Dieses Lernprogramm beschreibt die Vorgehensweise zum Beitreten zu einem Kanal, der im Rahmen des [Lernprogramms zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) erstellt wurde.

In diesem Lernprogramm werden **empfohlene Werte** für einige der Felder in der Konsole angegeben. Auf diese Weise können Sie die Namen und Identitäten in den Registerkarten und Dropdown-Listen leichter erkennen. Diese Werte sind zwar nicht obligatorisch, jedoch hilfreich, da Sie bestimmte Werte wie beispielsweise IDs und geheime Schlüssel registrierter Benutzer, die Sie in einem vorherigen Schritt eingegeben haben, für weitere Schritte benötigen. Falls Sie diese Werte vergessen, müssen Sie zusätzliche Benutzer für Ihre Administratoren und Komponenten registrieren. Nach jeder Tasks finden Sie eine Tabelle mit den empfohlenen Werten. Sollten Sie nicht die Beispielwerte verwenden, dann sollten Sie sich die von Ihnen benutzten Werte notieren, während Sie das Lernprogramm durcharbeiten.
{:tip}

## Schritt 1: Eine Peerorganisation und einen Peer erstellen
{: #ibp-console-join-network-create-ca-org2}

Für jede Organisation, die Sie mit der Konsole erstellen möchten, sollten Sie mindestens eine Zertifizierungsstelle implementieren. Eine Zertifizierungsstelle (CA) ist der Knoten, der Zertifikate für alle Netzteilnehmer (Peers, Anordnungsservices, Clients, Administratoren usw.) ausgibt. Mit diesen Zertifikaten, die ein Signierzertifikat und einen privaten Schlüssel enthalten, können Netzteilnehmer miteinander kommunizieren, sich authentifizieren und letztendlich Transaktionen ausführen. Diese Zertifizierungsstellen erstellen alle Identitäten und Zertifikate, die zu Ihrer Organisation gehören. Außerdem definieren sie die Organisation an sich. Anschließend können Sie diese Identitäten verwenden, um Knoten bereitzustellen, Administratoridentitäten zu erstellen und Transaktionen zu übergeben. Weitere Informationen zu Ihrer Zertifizierungsstelle und zu den Identitäten, die Sie erstellen müssen, finden Sie im Abschnitt [Identitäten verwalten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities).

Im vorliegenden Lernprogramm wird eine Organisation erstellt. Aus diesem Grund muss auch **eine Zertifizierungsstelle (CA)** erstellt werden.

### Zertifizierungsstelle für Peerorganisation erstellen
{: #ibp-console-join-network-create-CA-org2CA}

Im Rahmen dieses Lernprogramms gibt Ihre Zertifizierungsstelle die Zertifikate und privaten Schlüssel für Ihre Benutzer und Knoten aus. Diese Identitäten werden nicht von {{site.data.keyword.IBM_notm}} verwaltet und die Schlüssel werden nicht in der Konsole gespeichert. Sie werden nur im lokalen Speicher Ihres Browsers gespeichert. Stellen Sie daher sicher, dass Sie Ihre Identitäten und den MSP der Organisation exportieren. Wenn Sie versuchen, von einer anderen Maschine oder über einen anderen Browser auf die Konsole zuzugreifen, müssen Sie diese Identitäten und Organisationsdefinitionen importieren.
{:important}

Führen Sie die folgenden Schritte in der Konsole aus:  

1. Navigieren Sie zu der Registerkarte **Knoten** auf der linken Seite und klicken Sie auf **Zertifizierungsstelle hinzufügen**. In den Seitenanzeigen können Sie die Zertifizierungsstelle anpassen, die Sie erstellen möchten, und die Organisation, für die die Zertifizierungsstelle Schlüssel ausgibt.
2. In diesem Lernprogramm werden Knoten erstellt. Stellen Sie daher sicher, dass die Option zum **Erstellen** einer Zertifizierungsstelle ausgewählt ist. Klicken Sie anschließend auf **Weiter**.
3. Verwenden Sie die zweite Seitenanzeige, um der Zertifizierungsstelle einen **Anzeigename** zu geben. Der empfohlener Wert für diese Zertifizierungsstelle ist `Org2 CA`.
4. Geben Sie in der nächsten Anzeige die Administratorberechtigungsnachweise für die Zertifizierungsstelle an, indem Sie als **Eintragungs-ID des Zertifizierungsstellenadministrators** die Zeichenfolge `admin` und als geheimen Schlüssel `adminpw` angeben. Auch hierbei handelt es sich um die **empfohlenen Werte**.
5. Wenn Sie einen gebührenpflichtigen Cluster verwenden, haben Sie die Möglichkeit, die Ressourcenzuordnung für den Knoten zu konfigurieren. Im Rahmen dieses Lernprogramms können Sie alle Standardeinstellungen akzeptieren und auf **Weiter** klicken. Weitere Informationen über die Zuordnung von Ressourcen für Ihren Knoten in {{site.data.keyword.cloud_notm}} enthält der Abschnitt [Ressourcen zuordnen](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources). Wenn Sie einen kostenlosen Cluster verwenden, wird die Seite **Zusammenfassung** angezeigt.
6. Überprüfen Sie die Seite "Zusammenfassung" und klicken Sie dann auf **Zertifizierungsstelle hinzufügen**.

**Task: Zertifizierungsstelle für die Peerorganisation erstellen**

  | **Feld** | **Anzeigename** | **Eintragungs-ID** | **Geheimer Schlüssel** |
  | ------------------------- |-----------|-----------|-----------|
  | **Zertifizierungsstelle erstellen** | Org2 CA  | admin | adminpw |

*Abbildung 2. Zertifizierungsstelle für die Peerorganisation erstellen*  

Nach der Bereitstellung der Zertifizierungsstelle verwenden Sie diese zum Erstellen des MSP für Ihre Organisation, zum Registrieren von Benutzern und zum Erstellen des Einstiegspunkts für ein Netz (den **Peer**).

Fortgeschrittene Benutzer verfügen möglicherweise bereits über eine eigene Zertifizierungsstelle und möchten keine neue Zertifizierungsstelle in der Konsole erstellen. Wenn Ihre vorhandene Zertifizierungsstelle Zertifikate im Format `X.509` ausgeben kann, können Sie Zertifikate von Ihrer eigenen externen Zertifizierungsstelle verwenden, anstatt hier neue Zertifikate zu erstellen. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zur [Verwendung einer unabhängigen Zertifizierungsstelle mit Ihrem Peer oder Anordnungsservice](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-identities).

### Zertifizierungsstelle zum Registrieren von Identitäten verwenden
{: #ibp-console-join-network-use-CA-org2}

Jeder Knoten oder jede Anwendung, die Sie erstellen möchten, benötigt Zertifikate und private Schlüssel für die Teilnahme am Blockchain-Netz. Außerdem müssen Sie Administratoridentitäten für diese Knoten und Anwendungen erstellen, damit Sie sie über die Konsole verwalten können. Im vorliegenden Szenario wird eine Zertifizierungsstelle erstellt, um damit zwei Identitäten zu erstellen:

* **Organisationsadministrator**: Diese Identität ermöglicht es Ihnen, Knoten über die Platform-Konsole zu bedienen.
* **Peeridentität**: Dies ist die Identität des Peers selbst. Wenn ein Peer eine Aktion ausführt (z. B. die Bewilligung einer Transaktion), erfolgt die Signierung mit dem zugehörigen Zertifikat.

Je nach Ihrem Clustertyp kann die Bereitstellung der Zertifizierungsstelle bis zu zehn Minuten dauern. Wenn die Zertifizierungsstelle zum ersten Mal bereitgestellt wird (oder wenn die Zertifizierungsstelle aus anderen Gründen nicht verfügbar ist), dann wird das Feld in der Kachel für die Zertifizierungsstelle abgeblendet dargestellt. Nachdem die Zertifizierungsstelle erfolgreich bereitgestellt wurde und ausgeführt wird, wird dieses Feld grün dargestellt. Dies zeigt an, dass sie "Aktiv" ist und zur Registrierung von Identitäten verwendet werden kann. Bevor Sie mit den Schritten zur Registrierung von Identitäten fortfahren können, müssen Sie warten, bis der Status der Zertifizierungsstelle "Aktiv" lautet.
{:important}

Nachdem die Zertifizierungsstelle aktiviert wurde, was durch das grüne Feld in der Kachel angezeigt wird, müssen Sie diese Zertifikate generieren, indem Sie die folgenden Schritte ausführen:

1. Klicken Sie auf `Org2 CA` und vergewissern Sie sich, dass die Identität `admin`, die Sie für die Zertifizierungsstelle erstellt haben, in der Tabelle angezeigt wird. Klicken Sie dann auf die Schaltfläche **Benutzer registrieren**.
2. Zuerst wird der Administrator der Organisation registriert. Dazu können Sie die **Eintragungs-ID** `org2admin` und den **geheimen Schlüssel** `org2adminpw` festlegen. Geben Sie dann als `Typ` für diese Identität `client` an (Administratoridentitäten sollten immer als `client` registriert werden, während Knotenidentitäten immer mit dem Typ `peer` registriert werden sollten. Das Feld für die Zugehörigkeit (sog. Affiliation) ist für fortgeschrittene Benutzer vorgesehen und wird in diesem Lernprogramm nicht verwendet. Klicken Sie also auf das Feld **Stammzugehörigkeit verwenden**. Weitere Informationen über die Verwendung von Zugehörigkeiten durch die Fabric-Zertifizierungsstelle finden Sie im Abschnitt [Neue Identität registrieren](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external}. Wählen Sie vorläufig eine beliebige Zugehörigkeit in der Liste aus (z. B. `Org1`). Das Feld **Maximale Anzahl der Eintragungen** kann ignoriert werden. Weitere Informationen zu Eintragungen finden Sie im Abschnitt zur [Registrierung von Identitäten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-register). Klicken Sie auf **Weiter**.
4. Im Rahmen dieses Lernprogramms muss **Attribut hinzufügen** nicht verwendet werden. Weitere Informationen zu Identitätsattributen finden Sie im Abschnitt zur [Registrierung von Identitäten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-register).
5. Nachdem der Organisationsadministrator registriert wurde, wiederholen Sie diesen Prozess für die Identität des Peers (und verwenden Sie dazu ebenfalls `Org2 CA`). Geben Sie für die Peeridentität die Eintragungs-ID `peer2` und den geheimen Schlüssel `peer2pw` an. Dies ist eine Knotenidentität. Wählen Sie daher `peer` als **Typ** aus. Klicken Sie auf das Feld **Stammzugehörigkeit verwenden** und ignorieren Sie die Option **Maximale Anzahl der Eintragungen**. In der nächsten Anzeige müssen Sie wie zuvor keine **Attribute** zuordnen.

Die Registrierung dieser Identitäten bei der Zertifizierungsstelle stellt lediglich den ersten Schritt bei der **Erstellung** einer Identität dar. Sie können diese Identitäten erst benutzen, nachdem Sie die **Eintragung** durchgeführt haben. Für die Identität `org2admin` erfolgt dieser Schritt während der Erstellung des MSP (siehe hierzu den nächsten Schritt). Im Falle des Peers wird er während der Erstellung des Peers ausgeführt.
{:note}

**Task: Benutzer registrieren**

  |  **Feld** | **Beschreibung** | **Eintragungs-ID** | **Geheimer Schlüssel** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Benutzer registrieren** |  Org2 admin | org2admin | org2adminpw |
  | | Peeridentität |  peer2 | peer2pw |

*Abbildung 3. Zertifizierungsstelle zum Registrieren von Benutzern verwenden*  

### MSP-Definition der Peerorganisation erstellen
{: #ibp-console-join-network-create-peers-org2}

Nachdem Sie nun die Zertifizierungsstelle des Peers erstellt und diese zum **Registrieren** der Organisationsidentitäten verwendet haben, müssen Sie eine formale Definition der Peerorganisation erstellen, die als MSP-Definition (MSP, Membership Services Provider) bezeichnet wird. Viele Peers können zu einer Organisation gehören. **Sie müssen nicht jedes Mal, wenn Sie einen Peer erstellen, auch eine neue Organisation erstellen.** Da Sie dieses Lernprogramm zum ersten Mal durchlaufen, erstellen Sie die MSP-ID für diese Organisation. Während der Erstellung des MSP werden Sie Zertifikate für die Identität `org2admin` generieren und sie zu Ihrer Wallet hinzufügen.

1. Navigieren Sie zur Registerkarte **Organisationen** im linken Navigationsbereich und klicken Sie auf **MSP-Definition erstellen**.
2. Geben Sie für den MSP den Anzeigenamen `Org2 MSP` und die MSP-ID `org2msp` an. Wenn Sie in diesem Feld eine eigene MSP-ID angeben möchten, müssen Sie die Spezifikationen zu den Einschränkungen für diesen Namen in der QuickInfo befolgen.
3. Geben Sie unter den **Details für die Stammzertifizierungsstelle** die Zertifizierungsstelle an, die Sie im vorherigen Schritt zur Registrierung der Identitäten verwendet haben. Wenn Sie dieses Lernprogramm zum ersten Mal verwenden, sollten Sie nur eine Zertifizierungsstelle sehen: `Org2 CA`.
4. Die Felder **Eintragungs-ID** und **Geheimer Eintragungsschlüssel** darunter werden automatisch mit der Eintragungs-ID und dem geheimen Schlüssel für den ersten Benutzer gefüllt, den Sie mit der Zertifizierungsstelle erstellt haben: `admin` und `adminpw`. Wenn Sie diese Identität verwenden, dann erhält Ihre Organisation jedoch die gleiche Identität wie Ihre CA-Identität, was aus Sicherheitsgründen nicht zu empfehlen ist. Wählen Sie stattdessen in der Dropdown-Liste die Eintragungs-ID aus, die Sie für Ihren Organisationsadministrator erstellt haben (`org2admin`), und geben Sie als zugehörigen geheimen Schlüssel `org2adminpw` ein. Geben Sie dann dieser Identität einen Anzeigenamen (`Org2 Admin`).
5. Klicken Sie auf die Schaltfläche **Generieren**, um diese Identität als Administrator Ihrer Organisation einzutragen und die Identität in die Wallet zu exportieren, wo sie verwendet wird, wenn Sie den Peer und Kanäle erstellen.
6. Klicken Sie auf **Exportieren**, um die Administratorzertifikate in Ihr Dateisystem zu exportieren. Wie bereits oben erwähnt, wird diese Identität nicht in Ihrem Cluster gespeichert oder von {{site.data.keyword.IBM_notm}} verwaltet. Sie wird nur im lokalen Speicher Ihres Browsers gespeichert. Wenn Sie zu einem anderen Browser wechseln, müssen Sie diese Identität in Ihre Wallet importieren, damit Sie den Peer verwalten können.
7. Klicken Sie auf **MSP-Definition erstellen**.

**Task: MSP-Definition für die Peerorganisation erstellen**

  |  | **Anzeigename** | **MSP-ID** | **Eintragungs-ID**  | **Geheimer Schlüssel** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Organisation erstellen** | Org2 MSP | org2msp |||
  | **Stammzertifizierungsstelle** | Org2 CA ||||
  | **Administratorzertifikat für Organisation** | |  | org2admin | org2adminpw |
  | **Identität** | Org2 Admin |||||

  *Abbildung 4. MSP-Definition für Peerorganisation erstellen*  

Nachdem Sie den MSP erstellt haben, sollte der Administrator der Peerorganisation in Ihrer **Wallet** angezeigt werden. Klicken Sie im linken Navigationsbereich auf **Wallet**, um darauf zuzugreifen.

**Task: Wallet überprüfen**

  | **Feld** |  **Anzeigename** | **Beschreibung** |
  | ------------------------- |-----------|----------|
  | **Identität** | Org2 Admin | Org2 identity |

  *Abbildung 5. Wallet überprüfen*  

Weitere Informationen zu MSPs finden Sie im Abschnitt [Organisationen verwalten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-organizations#ibp-console-organizations).

Das Exportieren der Identität des Organisationsadministrators ist wichtig, da Sie für die Verwaltung und Sicherung dieser Zertifikate verantwortlich sind.
{:important}

### Peer erstellen
{: #ibp-console-join-network-peer-create}

Nachdem Sie eine [Zertifizierungsstelle erstellt](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-create-CA-org2CA), diese zum Registrieren von Identitäten verwendet und den [MSP der Peerorganisation erstellt](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-create-peers-org2) haben, können Sie nun einen Peer erstellen.

#### Welche Rolle spielen Peers?
{: #ibp-console-join-network-peer-role}

Es ist wichtig zu wissen, dass die Organisationen selbst keine Ledger pflegen. Dies übernehmen Peers. Organisationen verwenden auch Peers, um Transaktionsvorschläge zu signieren und Kanalkonfigurationsaktualisierungen zu genehmigen. Da das Vorhandensein von mindestens zwei Peers in einem Kanal diese hoch verfügbar macht, wird die Implementierung von mindestens zwei Peers, die mit einem Kanal verbunden sind, als Best Practice für Implementierungen auf Produktionsebene betrachtet. Dieses Lernprogramm enthält jedoch nur den Prozess zum Erstellen eines einzelnen Peers.

Aus Sicht einer Ressourcenzuordnung ist es möglich, dieselben Peers mit mehreren Kanälen zu verknüpfen. Das Design des Peers stellt sicher, dass die Daten von einem Kanal nicht über den Peer an einen anderen Kanal übergeben werden können. Da der Peer jedoch für jeden Kanal ein separates Ledger speichert, muss sichergestellt werden, dass der Peer genügend Verarbeitungsleistung und Speicher hat, um die Transaktion und das Laden der Daten zu verarbeiten.

#### Peer bereitstellen
{: #ibp-console-join-network-deploy-peer-role}

Verwenden Sie die Konsole, um die folgenden Schritte auszuführen:

1. Klicken Sie auf der Seite **Knoten** auf **Peer hinzufügen**.
2. Stellen Sie sicher, dass die Optionen zum **Erstellen** eines Peers ausgewählt ist. Klicken Sie anschließend auf **Weiter**.
3. Geben Sie für den Peer als **Anzeigename** den Namen `Peer Org2` an. Im Rahmen des vorliegenden Lernprogramms sollten Sie nicht auswählen, dass eine externe Zertifizierungsstelle für Ihren Peer verwendet wird. Weitere Informationen zu diesem Thema können Sie im Abschnitt zur [Verwendung von Zertifikaten einer externen Zertifizierungsstelle](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca) finden. Klicken Sie auf **Weiter**.
4. Wählen Sie in der nächsten Anzeige den Eintrag `Org2 CA` aus. Hierbei handelt es sich um die Zertifizierungsstelle, die Sie zur Registrierung der Peeridentität verwendet haben. Wählen Sie in der Dropdown-Liste die **Eintragungs-ID** für die Peeridentität aus, die Sie für Ihren Peer erstellt haben (`peer2`), und geben Sie den zugehörigen **geheimen Schlüssel** (`peer2pw`) ein. Wählen Sie dann in der Dropdown-Liste `Org2 MSP` aus und klicken Sie auf **Weiter**.
5. In der nächsten Seitenanzeige werden Informationen zur TLS-Zertifizierungsstelle abgefragt. Bei der Erstellung der Zertifizierungsstelle wird außerdem eine TLS-Zertifizierungsstelle (TLSCA) erstellt. Diese Zertifizierungsstelle wird zur Erstellung von Zertifikaten für die sichere Kommunikationsebene für Knoten verwendet. Wählen Sie deshalb in der Dropdown-Liste die **Eintragungs-ID** für die Peeridentität aus, die Sie für Ihren Peer erstellt haben (`peer2`), und geben Sie den zugehörigen **geheimen Schlüssel** (`peer2pw`) ein. Der **TLS-CSR-Hostname** ist eine Option, die für fortgeschrittene Benutzer zur Verfügung steht, die einen angepassten Domänennamen angeben möchten, der zur Adressierung des Peerendpunkts verwendet werden kann. Angepasste Domänennamen werden im Rahmen des vorliegenden Lernprogramms nicht behandelt, sodass Sie momentan für **TLS-CSR-Hostname** keinen Wert angeben sollten.
6. Wenn Sie einen gebührenpflichtigen Cluster verwenden, haben Sie auf der nächsten Anzeige die Möglichkeit, die Ressourcenzuordnung für den Knoten zu konfigurieren. Im Rahmen dieses Lernprogramms können Sie alle Standardeinstellungen akzeptieren und auf **Weiter** klicken. Weitere Informationen über die Zuordnung von Ressourcen für Ihren Knoten in {{site.data.keyword.cloud_notm}} enthält der Abschnitt [Ressourcen zuordnen](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources). Wenn Sie einen kostenlosen {{site.data.keyword.cloud_notm}}-Cluster verwenden, wird die Seite **Identität zuordnen** angezeigt.
7. Auf der letzten Seitenanzeige wird die Aufforderung **Identität zuordnen** angezeigt, um diese als Administrator Ihres Peers festzulegen. Im Rahmen des vorliegenden Lernprogramms werden Sie Ihren Organisationsadministrator `Org2 Admin` auch als Administrator Ihres Peers festlegen. Es ist möglich, bei `Org2 CA` eine andere Identität zu registrieren und einzutragen und diese Identität als Administrator Ihres Peers festzulegen. Im vorliegenden Lernprogramm wird jedoch die Identität `Org2 Admin` verwendet.
8. Überprüfen Sie die Zusammenfassung und klicken Sie auf **Peer hinzufügen**.

**Task: Peer bereitstellen**

  |  | **Anzeigename** | **MSP-ID** | **Eintragungs-ID** | **Geheimer Schlüssel** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Peer erstellen** | Peer Org2 | org2msp |||
  | **Zertifizierungsstelle** | Org2 CA ||||
  | **Peeridentität** | |  | peer2 | peer2pw |
  | **Administratorzertifikat** | org2msp ||||
  | **TLS-Zertifizierungsstelle** | Org2 CA ||||
  | **ID der TLS-Zertifizierungsstelle** | || peer2 | peer2pw |
  | **Identität zuordnen** | Org2 Admin |||||

  *Abbildung 6. Peer bereitstellen*  

In einem Produktionsszenario sollten Sie drei Peers für jeden Kanal bereitstellen. Dadurch kann der Ausfall eines Peers (z. B. während eines Wartungszyklus) ausgeglichen werden, sodass weiterhin die Hochverfügbarkeit der Peers erhalten bleibt. Wenn Sie für eine Organisation mehrere Peers bereitstellen wollen, dann verwenden Sie dieselbe Zertifizierungsstelle, die Sie auch zur Registrierung Ihrer ersten Peeridentität verwendet haben. Im vorliegenden Lernprogramm ist dies `Org2 CA`. Führen Sie dann die Registrierung einer neuen Peeridentität mit einer anderen Eintragungs-ID und einem anderen geheimen Schlüssel durch. Beispiel: `org2secondpeer` und `org2secondpeerpw`. Geben Sie dann bei der Erstellung des Peers die Eintragungs-ID und den geheimen Schlüssel an. Da dieser Peer weiterhin Org2 zugeordnet ist, müssen Sie in der Dropdown-Liste `Org2 CA`, `Org2 MSP` und `Org2 Admin` auswählen. Sie können diesem neuen Peer einen anderen Administrator zuordnen, der mit `Org2 CA` registriert und eingetragen werden kann. Dies ist jedoch ein optionaler Schritt. In dieser Lernprogrammreihe wird lediglich der Prozess zur Erstellung eines einzelnen Peers für jede Peerorganisation erläutert.
{:tip}

## Schritt 2: Dem Konsortium beitreten, das vom Anordnungsservice gehostet wird
{: #ibp-console-join-network-add-org2}

Wie bereits erwähnt, muss eine Peerorganisation für den Anordnungsservice bekannt sein, bevor sie einen Kanal erstellen oder einem Kanal beitreten kann (dieser Schritt wird auch als Teilnahme am "Konsortium", d. h. der Liste der Organisationen bezeichnet, die für den Anordnungsservice bekannt sind). Dies wird dadurch begründet, dass Kanäle in technischer Hinsicht **Nachrichtenübertragungswege** zwischen Peers über einen Anordnungsservice darstellen. So wie ein Peer mehreren Kanälen zugeordnet werden kann, ohne dass Informationen von einem Kanal an einen anderen Kanal übergeben werden, kann ein Anordnungsservice zur Ausführung von mehreren Kanälen verwendet werden, ohne dass Daten für Organisationen in anderen Kanälen offengelegt werden.

Da nur Administratoren von Anordnungsservices Peerorganisationen zum Konsortium hinzufügen können, müssen Sie über eine der folgenden Rollen verfügen:

* **Administrator des Anordnungsservice**. In diesem Fall können Sie die von Ihnen erstellte Peerorganisation direkt zum Konsortium hinzufügen.
* **Absender** der MSP-Informationen an den Administrator des Anordnungsservice, der Ihre Organisation zum entsprechenden Konsortium hinzufügt.

Im nächsten Schritt wird gezeigt, wie Sie Ihre Peerorganisation zu einem Konsortium hinzufügen, das von einem Anordnungsservice in einer anderen {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz gehostet wird. Im Lernprogramm wird vorausgesetzt, dass Sie den Anordnungsservice erstellt haben und jeden Schritt selbst nachvollziehen können. Wenn der Anordnungsservice von einer anderen Person erstellt wurde, stellen Sie sicher, dass der Administrator des Anordnungsservice die Schritte ausführt, die oben mit dem blauen Tippsymbol markiert sind.

Wenn Sie das Lernprogramm zum Erstellen eines Netzes ausgeführt haben oder der Anordnungsservice bereits in Ihrer Konsole enthalten ist, können Sie mit dem Schritt [Peerorganisation zum Anordnungsservice hinzufügen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-local) fortfahren. Führen Sie anschließend [Schritt 3: Peerorganisation zu einem vorhandenem Kanal hinzufügen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-channel) aus.

### Organisationsinformationen exportieren
{: #ibp-console-join-network-add-org2-remote}

Sie müssen die MSP-Definition Ihrer Organisation an den Administrator des Anordnungsservice senden und mit den nachfolgenden Schritten zum Konsortium hinzufügen. Zum Ausführen dieser Schritte müssen Sie der Administrator der **Peerorganisation** sein. Dies bedeutet, die Administratoridentität für die Peerorganisation muss in Ihrer Wallet enthalten sein.

1. Navigieren Sie zur Registerkarte **Organisationen**. Die Organisationen, die für den Export zur Verfügung stehen, sind unter **Verfügbare Organisationen** aufgelistet. Klicken Sie auf die Schaltfläche **Herunterladen** innerhalb der Kachel mit den Organisationen, um die JSON-Konfigurationsdatei herunterzuladen, die den MSP der Peerorganisation darstellt.
2. Senden Sie diese Datei in einer externen Operation an den Administrator des Anordnungsservice.

### Organisationsdefinition importieren
{: #ibp-console-join-network-import-remote-msp}

Dieser Schritt muss von einem Administrator des Anordnungsservice ausgeführt werden.
{:tip}

Der Administrator des Anordnungsservice muss diese JSON-Datei importieren, um Ihre Organisation zu seiner Konsole hinzuzufügen:

- Navigieren Sie zur Registerkarte **Organisationen**, klicken Sie auf die Schaltfläche **MSP-Definition importieren** und wählen Sie die JSON-Datei aus, die die MSP-Definition der Peerorganisation darstellt. Lassen Sie das Kontrollkästchen `Ich verfüge über eine Administratoridentität für die MSP-Definition` inaktiviert, da die Administratoridentität hier nicht erforderlich ist.

### Peerorganisation zum Anordnungsservice hinzufügen
{: #ibp-console-join-network-add-org2-local}

Dieser Schritt muss von einem Administrator des Anordnungsservice ausgeführt werden.
{:tip}

Führen Sie diese Schritte aus, um die Peerorganisation zum Konsortium hinzuzufügen, das von dem Anordnungsservice gehostet wird. Nur Administratoren des Anordnungsservice können Peerorganisationen zum Konsortium hinzufügen. Zum Ausführen dieser Task muss die Administratoridentität für den Anordnungsservice in Ihrer Wallet enthalten sein.

1. Navigieren Sie zur Registerkarte **Knoten**.
2. Blättern Sie abwärts zu dem Anordnungsservice, den Sie verwenden möchten, und klicken Sie auf diesen Service, um ihn zu öffnen.
3. Klicken Sie unter **Konsortiumsmitglieder** auf **Organisation hinzufügen**.
4. Wählen Sie in der Dropdown-Liste `Org2 MSP` aus, da es sich hierbei um den MSP handelt, der die Organisation `Org2` darstellt.
5. Klicken Sie auf **Organisation hinzufügen**.

Wenn Sie das Lernprogramm zum **Erstellen eines Netzes** ausgeführt haben oder Ihre Konsole bereits den Anordnungsservice und einen Kanal enthält, können Sie mit [Schritt 3: Peerorganisation zu einem vorhandenen Kanal hinzufügen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-channel) fortfahren.

### Anordnungsservice exportieren
{: #ibp-console-join-network-export-ordering-service}

Dieser Schritt muss vom Administrator des Anordnungsservice ausgeführt werden.
{:tip}

Führen Sie die folgenden Schritte aus, um den Anordnungsservice zu **exportieren**, bevor er von der Peerorganisation importiert werden kann.

1. Navigieren Sie zu dem Anordnungsservice auf der Registerkarte **Knoten**. Klicken Sie auf den Pfeil **Herunterladen** unter dem Namen des Anordnungsservice, um eine JSON-Konfigurationsdatei herunterzuladen.
2. Senden Sie diese Datei in einer externen Operation an die Peerorganisation. Der Administrator der Peerorganisation kann dann die Konfigurationsdatei verwenden, um den Anordnungsservice zur Konsole hinzuzufügen.

### Anordnungsservice aus einem anderen Cluster importieren
{: #ibp-console-join-network-import-remote-orderer}

Führen Sie die folgenden Schritte aus, um den Anordnungsservice in Ihre Konsole zu **importieren**:

1. Navigieren Sie zur Seite **Knoten** und klicken Sie auf **Anordnungsservice hinzufügen**.
2. Klicken Sie auf die Option **Vorhandenen Anordnungsservice importieren**.
3. Wählen Sie die **Serviceposition** aus, an der der Anordnungsservice bereitgestellt wurde, und klicken Sie auf die Schaltfläche **Datei hinzufügen**, um die JSON-Datei auszuwählen, die den Anordnungsservice darstellt.
4. Wenn Sie zum Zuordnen einer Identität aufgefordert werden, wählen Sie die Identität der Peerorganisation aus. Im vorliegenden Lernprogramm ist dies `Org2 Admin`. Klicken Sie auf **Anordnungsservice hinzufügen**.

Wenn dieser Prozess abgeschlossen ist, kann die Organisation `Org2` einen Kanal erstellen, der im `Anordnungsservice` gehostet wird. Wenn Sie einen neuen Kanal erstellen möchten, anstatt einem vorhandenen Kanal beizutreten, fahren Sie mit [Kanal erstellen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-create-channel) fort.

## Schritt 3: Peerorganisation zu einem vorhandenen Kanal hinzufügen
{: #ibp-console-join-network-add-channel}

Dieser Schritt muss von einem Administator der Organisation "Org1" ausgeführt werden.
{:tip}

Jetzt kann der Administrator des Anordnungsservice die Peerorganisation zum Kanal hinzufügen:
1. Navigieren Sie zur Registerkarte **Kanäle** und klicken Sie auf `Kanal 1`.
2. Klicken Sie auf das Symbol **Einstellungen**, um den Kanal zu aktualisieren und die Peerorganisation zum Kanal hinzuzufügen.
3. Öffnen Sie im Abschnitt **Organisationen** die Dropdown-Liste `Kanalmitglied auswählen` und wählen Sie den MSP der Organisation (`Org2 MSP`) aus.
4. Klicken Sie auf **Hinzufügen** und ordnen Sie dann Berechtigungen für diese Organisation zu. Sie sollten Berechtigungen des Typs `Operator` verwenden, damit der Kanal aktualisiert werden kann.
5. Stellen Sie sicher, dass in der Dropdown-Liste **MSP des Kanalaktualisierungsberechtigten** (unter der Überschrift **Organisation des Aktualisierungsberechtigten für den Kanal**) der Eintrag `Org1 MSP` ausgewählt ist.
6. Stellen Sie sicher, dass in der Dropdown-Liste **Identität** der Eintrag `Org1 Admin` ausgewählt ist.
7. Nachdem Sie diese Schritte abgeschlossen haben, klicken Sie auf **Vorschlag senden**.

Nachdem Ihre Peerorganisation vom Administrator des Anordnungsservice zu dem Kanal hinzugefügt wurde, können Sie nun Ihre Peers mit den Kanälen verknüpfen, die vom Anordnungsservice gehostet werden.

## Schritt 4: Peer mit dem Kanal verknüpfen
{: #ibp-console-join-network-join-peer-org2}

Sie haben es fast geschafft. Ihr Peer kann nun mit einem vorhandenen Kanal verknüpft werden. Sie müssen in einer externen Operation den `Kanalnamen` von einer Organisation abrufen, die Mitglied des Kanals ist. Im Lernprogramm zum **Erstellen eines Netzes** haben Sie einen Kanal namens `Kanal 1` erstellt. Navigieren Sie dazu ggf. zur Registerkarte **Kanäle** im linken Navigationsbereich.

Führen Sie die folgenden Schritte in der Konsole aus:

1. Klicken Sie auf die Schaltfläche **Kanal beitreten**, um die Seitenanzeigen zu starten.
2. Wählen Sie den Anordnungsservice mit dem Namen `Anordnungsservice` aus und klicken Sie auf **Weiter**.
3. Geben Sie den Namen des Kanals ein, dem Sie beitreten möchten (`Kanal 1`), und klicken Sie auf **Weiter**.
4. Wählen Sie die Peers aus, die dem Kanal beitreten sollen. Klicken Sie im Rahmen dieses Lernprogramms auf `Peer Org2`.
5. Klicken Sie auf **Kanal beitreten**.

Wenn Sie die Hyperledger Fabric-Funktionen [Private Daten](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} oder [Serviceerkennung](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} nutzen möchten, müssen Sie auf der Registerkarte **Kanäle** Ankerpeers in Ihren Organisationen konfigurieren. Weitere Informationen zum Konfigurieren von Ankerpeers für private Daten über die Registerkarte **Kanäle** in der Konsole finden Sie unter [Private Daten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).

## Kanal erstellen
{: #ibp-console-join-network-create-channel}

Da Ihre Organisation jetzt Mitglied des Konsortiums ist, haben Sie auch die Möglichkeit, einen neuen Kanal zu erstellen. Zunächst müssen Sie die MSP-Definition anderer Kanalmitglieder importieren. Erstellen Sie mit den nachfolgenden Schritten einen Kanal mit zwei Mitgliedern: die Organisation `Org1`, die im [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) erstellt wurde, und die Organisation `Org2`, die in den vorherigen Schritten erstellt wurde.

### Organisationsinformationen exportieren
{: #ibp-console-join-network-add-org2-export-info}

Dieser Schritt muss von einem Administrator der Organisation "Org1" ausgeführt werden.
{:tip}

Die Organisation `Org1` muss die zugehörige MSP-Definition an Sie senden, damit Sie die Organisation zu dem Kanal hinzufügen können. Sie müssen ein Mitglied des vom Anordnungsservice gehosteten Konsortiums sein, damit Sie zum Kanal hinzugefügt werden können. Zum Ausführen dieser Schritte müssen Sie der Administrator der **Peerorganisation** sein. Dies bedeutet, die Administratoridentität für die Peerorganisation muss in Ihrer Wallet enthalten sein.

1. Navigieren Sie zur Registerkarte **Organisationen**. Die Organisationen, die für den Export zur Verfügung stehen, sind unter **Verfügbare Organisationen** aufgelistet. Klicken Sie auf die Schaltfläche **Herunterladen** innerhalb der Kachel mit den Organisationen, um die JSON-Konfigurationsdatei herunterzuladen, die den MSP der Peerorganisation darstellt.
2. Senden Sie diese Datei in einer externen Operation an das Mitglied des Konsortiums, das den Kanal erstellen wird. 

### Organisationsdefinition importieren
{: #ibp-console-join-network-create-channel-import-msp}

Anschließend müssen Sie die MSP-Definition der Organisation `Org1` in Ihre Konsole importieren:

1. Navigieren Sie zur Registerkarte **Organisationen**, klicken Sie auf die Schaltfläche **MSP-Definition importieren** und wählen Sie die JSON-Datei aus, die die MSP-Definition der Peerorganisation darstellt.

#### Kanal erstellen
{: #ibp-console-join-network-channels-create}

Führen Sie die folgenden Schritte in der Konsole aus:

1. Klicken Sie auf **Kanal erstellen**. Es wird eine Seitenanzeige geöffnet.
2. Ordnen Sie dem Kanal einen **Namen** zu  (z. B. `Kanal 2`). Notieren Sie sich diesen Wert, da Sie ihn mit jedem teilen müssen, der diesem Kanal beitreten möchte.
3. Wählen Sie in der Dropdown-Liste `Anordnungsservice` aus.
4. Wählen Sie die **Organisationen** aus, die Teil dieses Kanals sein sollen. Da wir mit zwei Organisationen arbeiten, müssen Sie zuerst `Org1 MSP (org1msp)` auswählen und hinzufügen und danach `Org2 MSP (org2msp)`. Definieren Sie mindestens die beiden Organisationen als **Operator**. Hinweis: Verwenden Sie hier nicht `MSP des Anordnungsservice`.
5. Wählen Sie für den Kanal eine **Kanalaktualisierungsrichtlinie** aus. Hierbei handelt es sich um die Richtlinie, die vorgibt, wie viele Organisationen Aktualisierungen an der Kanalkonfiguration genehmigen müssen. Sie können `1 von 2` auswählen. Beim Hinzufügen von Organisationen zum Kanal sollten Sie diese Richtlinie an die Anforderungen Ihres Anwendungsfalls anpassen. Ein sinnvoller Standard besteht in der Verwendung einer Mehrheit von Organisationen. Beispiel: `3 von 5`.
6. Geben Sie sämtliche Einschränkungen für die **Zugriffssteuerung** an, die Sie festlegen wollen. Hinweis: Dies ist eine **erweiterte Option**. Wenn Sie einer bestimmten Organisation den Zugriff auf eine Ressource erteilen, beschränkt dies den Zugriff aller Organisationen auf diese Ressource. Wenn der Standardzugriff auf eine bestimmte Ressource z. B. für `Leseberechtigte` aller Organisationen gilt und dieser Zugriff dem `Administrator` der Organisation `Org2` erteilt wird, kann **nur** der Administrator von "Org2" auf diese Ressource zugreifen. Da der Zugriff auf bestimmte Ressourcen für den reibungslosen Betrieb eines Kanals von grundlegender Bedeutung ist, sollten Sie Entscheidungen zur Zugriffssteuerung sorgfältig abwägen. Wenn Sie den Zugriff auf eine Ressource einschränken möchten, stellen Sie sicher, dass der Zugriff auf diese Ressource nach Bedarf für jede Organisation hinzugefügt wird.
7. Wählen Sie die **Organisation des Kanalerstellers** aus. Da Sie das Lernprogramm als `Org2` ausführen und die Zertifikate für `Org2 Admin` in Ihrer Wallet enthalten sind, wählen Sie in der Dropdown-Liste den Eintrag `Org2 MSP` aus. Wählen Sie außerdem `Org2 Admin` als Identität zum Erstellen des Kanals aus.

Nachdem Sie diesen Vorgang abgeschlossen haben, klicken Sie auf **Kanal erstellen**. Daraufhin wird wieder die Registerkarte "Kanäle" aufgerufen, auf der für den soeben erstellten Kanal eine Kachel im Status "Anstehend" angezeigt wird.

**Task: Kanal erstellen**

  |  **Feld** | **Name** |
  | ------------------------- |-----------|
  | **Kanalname** | Kanal 2 |
  | **Anordnungsservice** | Anordnungsservice |
  | **Organisationen** | Org2 MSP |
  | **Kanalaktualisierungsrichtlinie** | 1 von 2 |
  | **Zugriffssteuerungsliste** | Keine |
  | **Organisation des Kanalerstellers** | Org2 MSP |
  | **Identität** | Org2 Admin|

*Abbildung 7. Kanal erstellen*

## Nächste Schritte
{: #ibp-console-join-network-next-steps}

Nachdem Sie den Peer mit einem Kanal verknüpft haben, verwenden Sie nun die folgenden Schritte, um einen Smart Contract bereitzustellen und mit der Übertragung von Transaktionen an die Blockchain zu beginnen:

- [Stellen Sie einen Smart Contract in Ihrem Netz bereit](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts). Verwenden Sie dazu die Konsole.
- Nachdem Sie Ihren Smart Contract installiert und instanziiert haben, können Sie [Transaktionen mit Ihrer Clientanwendung übergeben](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-connect-to-SDK).
- Verwenden Sie das [Wertpapier-Beispiel (commercial-paper)](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-commercial-paper), um einen beispielhaften Smart Contract bereitzustellen und Transaktionen mithilfe von Anwendungscode zu übergeben.
