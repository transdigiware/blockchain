---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain, IBM Blockchain Platform, terms, Fabric, Raft, CouchDB, consortium

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Glossar
{: #glossary}

In diesem Abschnitt werden spezielle Begriffe von {{site.data.keyword.blockchainfull}} definiert, die in der vorliegenden Dokumentation Verwendung finden. Eine eingehendere Erläuterung der Begriffe sowie ein Glossar der Begriffe, die im Zusammenhang mit den Konzepten von Hyperledger Fabric verwendet werden, enthält das [Hyperledger Fabric-Glossar](https://hyperledger-fabric.readthedocs.io/en/release-1.4/glossary.html){: external}.
{:shortdesc}

## Aktueller Status
{: #glossary-current-state}
Der aktuelle Status des Ledgers stellt die neuesten Werte für alle Schlüssel dar, die im zugehörigen Chaintransaktionsprotokoll enthalten sind. Da der aktuelle Status die neuesten Werte aller Schlüssel darstellt, die dem Kanal bekannt sind, wird er auch als **World-Status** bezeichnet. Smart Contracts führen Transaktionsvorschläge für Daten mit dem aktuellen Status aus. Der aktuelle Status wird geändert, sobald der Wert eines Schlüssels geändert oder ein neuer Schlüssel hinzugefügt wird. Er ist für einen Transaktionsfluss kritisch, da das neueste Schlüssel/Wert-Paar vor einer Änderung bekannt sein muss. Peers schreiben die neuesten Werte für jede gültige Transaktion in einem Block im aktuellen Status des Ledgers fest. Der aktuelle Status wird in der Statusdatenbank gespeichert, die einem Peer zugeordnet ist.

## Anordnungsknoten
{: #glossary-orderer}
Der Knoten, der Transaktionen von Netzmitgliedern erfasst, die Transaktionen anordnet und sie zu Blöcken bündelt. Wird auch als "Bündelungsknoten" bezeichnet. Diese Blöcke werden anschließend an Peers verteilt, die wiederum die Blöcke prüfen und den Ledgern in jedem Kanal hinzufügen. Anordnungsknoten enthalten das Identitätsmaterial für die Verschlüsselung, das an jedes Mitglied gebunden ist, und authentifizieren die Identitä von Clients und Peers für den Zugriff auf das Netz. Die Funktion, die von einem Anordnungsknoten oder einer solchen Knotengruppe bereitgestellt wird, wird als **Anordnungsservice** bezeichnet.

## Asset
{: #glossary-asset}
Materielle oder immaterielle Güter, Services oder Liegenschaften, die als Element dargestellt werden, die im Blockchain-Netz gehandelt werden.

## Benutzer
{: #glossary-user}
Ein Benutzer ist ein Teilnehmer an einem Blockchain-Netz, der durch eine Vertrauensbeziehung zu einem vorhandenen Mitglied direkten Zugriff auf das Ledger hat.

## Bewilligung
{: #glossary-endorsement}
Der Prozess, durch den Chaincode-Transaktionen validiert werden. Bewilligungsregeln werden durch die Angabe von Bewilligungsrichtlinien implementiert.

## Bewilligungsrichtlinie
{: #glossary-endorsement-policy}
Definiert die Peerknoten für einen Kanal, die Transaktionen ausführen müssen, die an eine bestimmte Chaincode-Anwendung angehängt sind, sowie die angeforderte Kombination von Antworten (Bewilligungen). Eine Richtlinie könnte zum Beispiel erfordern, dass eine Transaktion von einer Mindestanzahl bewilligender Peers, durch einen Mindestprozentsatz der bewilligenden Peers oder durch alle bewilligenden Peers bewilligt wird, die einer bestimmten Chaincode-Anwendung zugeordnet sind. Richtlinien können je nach Anwendung und gewünschter Ausfallsicherheitstufe gegen schädliches Verhalten unabhängig davon, ob vorsätzlich oder nicht, durch die bewilligenden Peers betreut werden. Eine übergebene Transaktion muss die Bewilligungsrichtlinie erfüllen, bevor sie durch festschreibende Peers als gültig markiert wird. Darüber hinaus ist auch eine bestimmte Bewilligungsrichtlinie zum Installieren und Instanziieren von Transaktionen erforderlich.

## Block
{: #glossary-block}
Eine geordnete Gruppe von Transaktionen, die kryptografisch mit dem vorangehenden Block in einem Kanal verknüpft ist.

## Chain
{: #glossary-chain}
Die Chain (Kette) des Ledgers ist ein Transaktionsprotokoll, das die Struktur von durch Hashwerte verknüpften Blöcken von Transaktionen hat. Peers empfangen Blöcke von Transaktionen aus dem Anordnungsservice, markieren die Transaktionen des Blocks je nach Bewilligungsrichtlinien und Verletzungen des gemeinsamen Zugriffs als gültig oder ungültig und hängen den Block an die Hash-Chain im Dateisystem des Peers an.

## Chaincode
{: #glossary-chaincode}
Bei Chaincode, auch als **Smart Contract** bezeichnet, handelt es sich um Teile von Software, die eine Gruppe von Funktionen zum Abfragen oder Aktualisieren des Ledgers enthalten.

## Client
{: #glossary-client}
Der Client stellt die Entität dar, die im Namen eines Benutzers agiert. Er muss eine Verbindung zu einem Peer herstellen, um mit dem Blockchain-System kommunizieren zu können. Der Client kann die Verbindung zu einem beliebigen Peer seiner Wahl herstellen. Clients erstellen Transaktionen und rufen sie dadurch auf. Der Client übergibt einen tatsächlichen Transaktionsaufruf an die Bewilliger und sendet Transaktionsvorschläge an den Anordnungsservice.

## CouchDB
{: #glossary-couchdb}
Ein Dokumentspeicher, der komplexe Abfragen für Daten zulässt, die für die Statusdatenbank in {{site.data.keyword.blockchainfull_notm}} Platform und in Starter Plan-Netzen verwendet werden. CouchDB ist neben LevelDB eine Option für Enterprise Plan-Netze.

## Dynamische Mitgliedschaft
{: #glossary-dynamic-memership}
Ein Mitglied kann dem Netz von einem Benutzer mit der Berechtigung **Registrator** (registrar) dynamisch hinzugefügt werden. Mitglieder werden auch Rollen und Attribute zugeordnet, von denen ihr Zugriff und ihre Berechtigung im Netz gesteuert wird. Weder Rollen noch Attribute können jedoch dynamisch zugeordnet werden. Hyperledger Fabric unterstützt das Hinzufügen oder Entfernen von Mitgliedern, Peers und Anordnungsserviceknoten ohne den Betrieb des Gesamtnetzes zu beeinträchtigen. Die dynamische Mitgliedschaftsänderung ist von entscheidender Bedeutung, wenn Geschäftsbeziehungen aus verschiedenen Gründen angepasst werden und Entitäten hinzugefügt oder entfernt werden müssen.

## Genesis-Block
{: #glossary-genesis-block}
Der Konfigurationsblock, der ein Blockchain-Netz oder einen Blockchain-Kanal initialisiert und zugleich als erster Block in einer Chain dient.

## Gossip
{: #glossary-gossip}
Hyperledger Fabric ermöglicht Peers die Zusammenstellung wichtiger Netzinformationen anderer Peers, ohne dass hierzu der Anordnungsservice benötigt wird. Das [Gossip-Datenverteilungsprotokoll](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} bietet eine sichere, zuverlässige und skalierbare Möglichkeit, mit deren Hilfe Peers Nachrichten austauschen können. Wenn Peers beispielsweise bestimmte Blöcke nicht erhalten, weil es zu Verzögerungen, Netzausfällen oder anderen Problemen gekommen ist, können Sie sich auf den aktuellen Ledgerstatus synchronisieren, indem Sie über den Gossip-Nachrichtenaustausch Kontakt zu anderen Peers aufnehmen, die über die fehlenden Blöcke verfügen.

## HSM
{: #glossary-hsm}
Hardwaresicherheitsmodul (Hardware Security Module). Stellt Verschlüsselung, Schlüsselmanagement und Schlüsselspeicher als verwalteten Service auf Anforderung zur Verfügung. HSM ist eine physische Einheit (Appliance), die die ressourcenintensiven Tasks der Verschlüsselungsverarbeitung abwickelt und die Latenz für Anwendungen verringert. Weitere Informationen finden Sie im Abschnitt zum [Hardwaresicherheitsmodul](https://www.ibm.com/cloud/hardware-security-module){: external}.

## Hyperledger Fabric
{: #glossary-hyperledger-fabric}
[Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external} ist ein unternehmensorientiertes Blockchain-Framework, das von der Linux Foundation als Basis für die Entwicklung von Blockchain-Anwendungen oder -Lösungen mit einer modularen Architektur zur Verfügung gestellt wird. Hyperledger Fabric-Komponenten wie Konsens- und Mitgliedschaftsservices sind Plug-and-play-fähig.

## Installation
{: #glossary-install}
Der Prozess des Anordnens eines Chaincodes im Dateisystem eines Peers. Sie müssen den Chaincode auf allen Peers installieren, auf denen dieser Chaincode ausgeführt werden soll.

## Instanziieren
{: #glossary-instantiate}
Der Prozess des Startens und Initialisierens eines Chaincode-Containers auf einem bestimmten Kanal. Wenn der Chaincode auf den Peers installiert wurde und jeder Peer dem Kanal zugeordnet wurde, muss der Chaincode auf dem Kanal instanziiert werden. Durch die Instanziierung wird die erforderliche Initialisierung des Chaincodes durchgeführt. Dazu gehört die Einstellung der Schlüssel/Wert-Paare, die den ersten World-Status eines Chaincodes bilden. Nach der Instanziierung können Peers, auf denen der Chaincode installiert ist, Chaincode-Aufrufe akzeptieren.

## Kafka
{: #glossary-kafka}
Eine Implementierung eines Konsens-Plug-ins für Hyperledger Fabric, deren Ergebnis ein Cluster mit Anordnungsserviceknoten im Blockchain-Netz ist. Kafka- und Raft-Implementierungen sind für Produktionsnetze konzipiert. Allerdings werden nur Cluster mit Raft-Anordnungsservices nativ unterstützt und können über {{site.data.keyword.blockchainfull_notm}} Platform erstellt werden.

## Kanal
{: #glossary-channel}
Ein Kanal besteht aus einer Untergruppe der Netzmitglieder, die Transaktionen privat ausführen möchten. Kanäle sorgen für Datenisolation und Vertraulichkeit, indem sie es den Mitgliedern eines Kanals ermöglichen, bestimmte Regeln und ein separates Ledger einzurichten, auf das nur Kanalmitglieder Zugriff haben. Peers, bei denen es sich um Knoten handelt, die als Transaktionsendpunkte für Organisationen fungieren, werden mit Kanälen verknüpft.

## Knoten
{: #glossary-node}
Die Kommunikationsentität der Blockchain. Es gibt drei Typen von Knoten: Zertifizierungsstellenknoten, Peerknoten und Anordnungsknoten.

## Konsens
{: #glossary-consensus}
Ein kooperativer Prozess, durch den die Ledgertransaktionen im gesamten Netz synchron gehalten werden. Der Konsens stellt sicher, dass Ledger nur aktualisiert werden, wenn die richtigen Teilnehmer Transaktionen genehmigen, und dass Ledger mit denselben Transaktionen in derselben Reihenfolge aktualisiert werden. Es gibt mehrere algorithmische Möglichkeiten, den Konsens zu realisieren.

## Konsole
{: #glossary-console}
Der Name der Benutzerschnittstelle von {{site.data.keyword.blockchainfull_notm}} Platform. Die Konsole erlaubt Benutzern das Anzeigen, Erstellen und Verwalten ihrer Bereitstellungen. Da öffentliche und private Schlüssel ausschließlich lokal in dem Browser gespeichert werden, unter dem die Konsole ausgeführt wird, behalten Benutzer die vollständige Kontrolle über ihre Schlüssel.

## Konsortium
{: #glossary-consortium}
Die Gruppe der Organisationen, bei denen es sich nicht um Anordnungsknoten handelt und die im Systemkanal des Anordnungsknotens aufgelistet sind. Dies sind die einzigen Organisationen, die Kanäle erstellen können. Zum Zeitpunkt der Kanalerstellung müssen alle zum Kanal hinzugefügten Organisationen Mitglieder eines Konsortiums sein. Eine Organisation, die nicht in einem Konsortium definiert ist, kann allerdings zu einem vorhandenen Kanal hinzugefügt werden. Ein Blockchain-Netz kann zwar mehrere Konsortien umfassen, aber die meisten Blockchain-Netze beinhalten nur ein einziges Konsortium.

## Ledger
{: #glossary-ledger}
Das Ledger besteht aus einer Literalkette von Blöcken ("chain of blocks"), die die unveränderliche Aufzeichnung von Transaktionen sowie eine Statusdatenbank speichern, in der der aktuelle Status verwaltet wird. Es gibt ein Ledger pro Kanal und Aktualisierungen des Ledgers werden durch den Konsensprozess nach den Richtlinien eines bestimmten Kanals verwaltet.

## LevelDB
{: #glossary-leveldb}
Ein Schlüssel/Wert-Speicher, der neben CouchDB als Statusdatenbankoption für Enterprise Plan-Netze zur Verfügung steht. In LevelDB wird der aktuelle Status als Schlüssel/Wert-Paar gespeichert, die Verwendung von Indizes oder komplexen Abfragen wird nicht unterstützt.

## Mitglied
{: #glossary-member}
Mitglieder in einem Blockchain-Netz, die auch als "Organisationen" bezeichnet werden, bilden ganz ähnlich wie Mitglieder einer beliebigen Gruppe die Struktur des Netzes. Ein Mitglied kann so groß wie ein multinationales Unternehmen oder auch so klein wie eine Einzelperson sein. Mitglieder werden im Netz mit einem Zertifikat eingetragen, das ihnen Berechtigungen zur Verwendung des Netzes als Service-Provider (z. B.: Zertifikate ausstellen, Transaktionen validieren/anordnen) oder als Nutzer erteilt. Das Erstere stellt grundlegende Blockchain-Services bereit, die Services für Transaktionsvalidierung, Transaktionsanordnung und Zertifikatsmanagement einschließen. Nutzermitglieder verwenden das Netz, um Transaktionen für das verteilte Ledger aufzurufen. Mitglieder können mehrere Peers haben.

## MSP
{: #glossary-msp}
Eine Abkürzung für **Membership Service Provider**. Der MSP gibt die Definition einer Organisation einschließlich des Stammzertifikats der Zertifizierungsstelle an, die Zertifikate für die Entitäten ausstellt, die der betreffenden Organisation zugeordnet sind. Des Weiteren wird das Signierzertifikat des Administrators dieser Organisation bereitgestellt. MSPs existieren auch auf der lokalen Ebene eines Peers oder Anordnungsknotens und stellen den Authentifizierungsmechanismus dar, mit dem Benutzer mit Administratorberechtigungen des jeweiligen Knotens verifiziert werden. In {{site.data.keyword.blockchainfull_notm}} Platform können MSPs von einer Konsole auf eine andere exportiert werden, sodass Benutzer eine Organisation in einer Konsole erstellen, diese Organisation dann auf eine andere Konsole importieren und dort betreiben können, um beispielsweise einen Kanal zu erstellen. MSPs können außerdem in einen Anordnungsservice importiert werden, um ein "Konsortium" zu bilden, die Liste der Organisationen, die zum Erstellen von und zur Teilnahme an Kanälen berechtigt sind.

## Network Monitor
{: #glossary-network-monitor}
Das GUI-Dashboard für die Starter- und Enterprise-Netze von {{site.data.keyword.blockchainfull_notm}} Platform, über die Benutzer das Blockchain-Netz anzeigen und verwalten können.

## Netz
{: #glossary-network}
Eine Instanz eines {{site.data.keyword.blockchainfull_notm}} Platform-Service in {{site.data.keyword.cloud_notm}}.

## Netzberechtigungsnachweise
{: #glossary-network-credentials}
Werden in der Anzeige "APIs" im Network Monitor angezeigt. Berechtigungsnachweise beinhalten Ihren Schlüssel ("key" - Benutzername) und Ihren geheimen Schlüssel ("secret" - Kennwort) in der Swagger-Benutzerschnittstelle. Sie müssen diese Netzberechtigungsnachweise zum Authentifizieren verwenden, bevor Sie die REST-APIs ausprobieren.

## Organisation
{: #glossary-organization}
Siehe [Mitglied](/docs/services/blockchain?topic=blockchain-glossary#glossary-member).

## Peer
{: #glossary-peer}
Eine Blockchain-Netzressource, die die Services zur Ausführung und Validierung von Transaktionen sowie zur Verwaltung von Ledgern bereitstellt. Der Peer führt Chaincode aus und enthält das Transaktionsprotokoll und den aktuellen Status der Assets auf den Netzkanälen, das heißt, des Ledgers. Peers gehören zu Organisationen und werden Kanälen zugeordnet.

## Raft
{: #glossary-raft}
Raft ist ist ein CFT-Anordnungsservice (CFT = Crash Fault Tolerant, fehlertolerant), der auf einer Implementierung des [Raft-Protokolls](https://raft.github.io/raft.pdf){: external} in `etcd` basiert. Raft orientiert sich an einem sog. Leader-and-Follower-Modell, das vorgibt, dass ein Leaderknoten (pro Kanal) ausgewählt wird, dessen Entscheidungen von den Followern repliziert werden. Raft-Anordnungsservices sind im Vergleich zu Kafka-basierten Anordnungsservices einfacher einzurichten und zu verwalten. Ein Cluster dieser Knoten kann über {{site.data.keyword.blockchainfull_notm}} Platform erstellt werden.

## SDK
{: #glossary-sdk}
Hyperledger Fabric unterstützt zwei Software Development Kits (SDKs). Ein Node-SDK und ein Java-SDK.  Das Node-SDK kann über NPM und das Java-SDK über Maven installiert werden.  Die SDKs verfügen über eigene Git-Repositorys, nämlich [Fabric Node SDK](https://github.com/hyperledger/fabric-sdk-node){: external} und [Fabric Java SDK](https://github.com/hyperledger/fabric-sdk-java){: external} mit Dokumentation für die verfügbaren APIs. Die Hyperledger Fabric Client-SDKs ermöglichen die Interaktion zwischen Ihrer Clientanwendung und Ihrem Blockchain-Netz.

## Serviceberechtigungsnachweise
{: #glossary-service-credentials}
Serviceberechtigungsnachweise liegen im JSON-Format vor und enthalten die Informationen zu API-Endpunkten sowie die Eintrags-IDs und geheimen Schlüssel für Ihre Netzressourcen, d. h. für Zertifizierungsstellen (CAs), Anordnungsknoten und Peers. Ihre Anwendung interagiert mit Netzressourcen über diese API-Endpunkte.

## signCert-Zertifikat
{: #glossary-sign-cert}
Das Zertifikat, das alle Entitäten (Organisationen oder Administratoren) an ihre Vorschläge oder Antworten auf Vorschläge anhängen. signCert-Zertifikate sind für eine Entität eindeutig und werden durch den Anordnungsservice geprüft, um sicherzustellen, dass sie mit dem signCert-Zertifikat in der Datei für diese Entität identisch sind.

## Smart Contracts
{: #glossary-smart-contracts}
Siehe [Chaincode](/docs/services/blockchain?topic=blockchain-glossary#glossary-chaincode).

## Solo
{: #glossary-solo}
Eine Implementierung eines Konsens-Plug-ins für Hyperledger Fabric, die zur Folge hat, dass nur ein Anordnungsservice im Blockchain-Netz vorhanden ist. Das Starter Plan-Netz verwendet die Solo-Implementierung. Eine Solo-Implementierung ist nicht für ein Produktionsnetz vorgesehen. Alternativ zu Solo-Clustern können Raft- und Kafka-Cluster verwendet werden.

## Statusdatenbank
{: #glossary-state-database}
Daten zum aktuellen Status werden in einer Datenbank auf den Peers gespeichert und können so effizient über den Chaincode gelesen und abgefragt werden. Bei Starter Plan-Netzen wird CouchDB als Statusdatenbank verwendet. Enterprise Plan-Netze können LevelDB oder CouchDB verwenden.

## Teilnehmer
{: #glossary-participant}
Eine Organisation, Einzelperson, Anwendung oder Einheit (Gerät), die mit dem Blockchain-Netz interagiert. Unter dem Teilnehmerbegriff werden zwei verschiedenartige Gruppierungen, nämlich Mitglieder und Benutzer, zusammengefasst.

## Transaktion
{: #glossary-transaction}
Der Mechanismus, über den Teilnehmer im Blockchain-Netz mit Assets interagieren. Eine Transaktion erstellt entweder einen neuen Chaincode oder ruft eine Operation in einem vorhanden Chaincode auf.

## Verbindungsprofil
{: #glossary-connection-profile}
Das Verbindungsprofil ist in der Anzeige "Übersicht" im Network Monitor sichtbar, wenn Sie auf die Schaltfläche **Verbindungsprofil** klicken. Die Informationen sind im JSON-Format verfügbar und enthalten die Informationen zu den API-Endpunkten sowie die Eintragungs-IDs und geheimen Schlüssel für Ihre Netzressourcen, d. h. für Peers, Anordnungsknoten und Zertifizierungsstellen (CAs). Ihre Anwendung interagiert mit Netzressourcen über diese API-Endpunkte.

## World-Status
{: #glossary-world-state}
Siehe [Aktueller Status](/docs/services/blockchain?topic=blockchain-glossary#glossary-current-state).
## Zertifizierungsstelle (CA)
{: #glossary-CA}
CA steht als Abkürzung für den englischen Begriff "Certificate Authority", zu Deutsch "Zertifizierungsstelle". Hierbei handelt es sich um die Komponente, die Zertifikate an alle teilnehmenden Mitglieder ausgibt. Diese Zertifikate stellen die Identität eines Mitglieds dar. Alle Entitäten im Netz (Peers, Anordnungsknoten, Clients usw.) müssen über eine Identität verfügen, um kommunizieren, sich authentifizieren und letztlich Transaktionen ausführen zu können. Diese Identitäten sind für jede direkte Teilnahme am Blockchain-Netz erforderlich.

