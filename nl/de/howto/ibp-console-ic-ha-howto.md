---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# Bereitstellungen mit Hochverfügbarkeit in mehreren Regionen einrichten
{: #ibp-console-hadr-mr}

Die HA-Konfiguration mit mehreren Regionen bietet die größtmögliche HA-Abdeckung. Durch die übergreifende Bereitstellung von Peers in mehreren geografischen Regionen wird sichergestellt, dass im Falle der Nichtverfügbarkeit einer Region die Peers in anderen Regionen die Transaktionsverarbeitung fortsetzen können. Dabei ist zu beachten, dass für CAs und für den Anordnungsservice derzeit keine HA-Unterstützung mit mehreren Regionen verfügbar ist.

## Übersicht
{: #ibp-console-hadr-overview}

Beim Einrichten der HA-Unterstützung mit mehreren Regionen für Peers werden die folgenden Tasks ausgeführt:
- Mehrere Serviceinstanzen in {{site.data.keyword.cloud}} erstellen, die jeweils an einen Kubernetes-Cluster in einer anderen Region gebunden sind
- Blockchain-Knoten in verschiedenen Regionen erstellen
- Die Knoten mithilfe der Export-/Importfunktion für Knoten über eine einzige Konsole verwalten

## Konfigurationsschritte
{: #ibp-console-hadr-config}

Führen Sie beim Konfigurieren Ihres Blockchain-Netzes die folgenden Schritte aus, um die HA mit mehreren Regionen zu konfigurieren, indem Sie redundante Peers für jede Organisation erstellen: 

1. Erstellen Sie drei {{site.data.keyword.cloud_notm}}-Kubernetes-Cluster in den gewünschten Regionen. Diese Cluster können sich in jeder gewünschten Region befinden, sollten jedoch für eine optimale Leistung möglichst nahe beieinander positioniert werden. Beispiel: Die Regionen "USA - Ostküste", "USA - Westküste" und "Kanada" sind besser geeignet als die Regionen "USA - Westküste" und "Tokio".
2. Stellen Sie eine neue Instanz von {{site.data.keyword.blockchainfull_notm}} Platform bereit und verknüpfen Sie sie mit dem Cluster in der ersten Region. Stellen Sie anschließend eine weitere Instanz von {{site.data.keyword.blockchainfull_notm}} Platform bereit und verknüpfen Sie sie mit dem Cluster in der zweiten Region. Wiederholen Sie den Vorgang, um eine dritte Serviceinstanz mit dem Cluster in der dritten Region zu verknüpfen. Danach verfügen Sie über drei separate {{site.data.keyword.blockchainfull_notm}} Platform-Instanzen, die mit drei separaten Clustern in verschiedenen Regionen verknüpft sind, und über drei separate Konsolen.

In diesem Lernprogramm wird vorausgesetzt, dass ein Anordnungsservice mit einem definierten Kanal vorhanden ist, an dem Peers teilnehmen können.
{: important}

### Schritt 1: CA und Metadaten der Peerorganisation in Cluster 1 installieren
{: #ibp-console-hadr-peerCA}

1. Stellen Sie eine CA im ersten Cluster bereit, in dem Sie die Anweisungen im Abschnitt [Zertifizierungsstelle für Peerorganisation erstellen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA) aus dem Lernprogramm zum Erstellen eines Netzes ausführen. Notieren Sie die Werte für **Eintragungs-ID** und **geheimer Schlüssel** der CA. Diese Werte müssen Sie beim Importieren der CA in andere Cluster angeben.
2. Sobald der Statusanzeiger für die CA-Kachel grün (`Aktiv`) anzeigt, befolgen Sie die Anweisungen im Abschnitt [Identitäten bei Ihrer Zertifizierungsstelle registrieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). Mithilfe dieser Anweisungen erstellen Sie zwei Identitäten, eine für den Peer und eine andere für die Organisationsadministratoridentität des Peers.
3. [Erstellen Sie die MSP-Definition für die Peer-Organisation](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) im ersten Cluster.
4. [Erstellen Sie einen Peer](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) im ersten Cluster.
5. [Installieren Sie Ihren Smart Contract](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) auf Ihrem Peer.

Bevor Sie den Smart Contract auf dem Kanal instanziieren können, müssen Sie mit den folgenden Schritten den Peer zu dem Kanal im Anordnungsservice hinzufügen:
- [Exportieren Sie die Definition der Peerorganisation](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) und teilen Sie sie mit einem Administrator des Anordnungsservice.
- Der Administrator des Anordnungsservice muss die Schritte zum [Importieren der Peerorganisation](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) ausführen und die Organisation zum Konsortium hinzufügen. Außerdem muss er die Schritte in diesen Anweisungen zum Exportieren des Anordnungsservice in eine JSON-Datei ausführen.
- [Importieren Sie die JSON-Datei des Anordnungsservice](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) in Ihre Konsole.
- [Verknüpfen Sie den Peer mit dem Kanal](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Falls Sie die Serviceerkennung, private Daten und Peer-Gossip verwenden, muss der Administrator des Anordnungsservice abschließend auf dem Kanal [Ankerpeers konfigurieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers). Für HA wird empfohlen, jeden redundanten Peer als Ankerpeer hinzuzufügen. Dadurch kann der Gossip-Datenaustausch zwischen Organisationen fortgesetzt werden, wenn einer der Peers nicht verfügbar ist.   

Nachdem Sie den Peer mit dem Kanal verknüpft haben, können Sie in dem Kanal [Smart Contract instanziieren](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).

### Schritt 2: Metadaten und Identitäten aus Cluster 1 exportieren
{: #ibp-console-hadr-export-meta1}

1. Exportieren Sie die CA-Definition in eine JSON-Datei.
   - Öffnen Sie die Zertifizierungsstelle (CA) auf der Registerkarte **Knoten**.
   - Klicken Sie auf das Download-Symbol, um die JSON-Datei der CA über Ihre Browsersitzung zu erstellen.
2. Exportieren Sie die MSP-Definition der Peerorganisation in eine JSON-Datei.
   - Navigieren Sie zu der MSP-Definition auf der Registerkarte **Organisationen**.
   - Klicken Sie auf das Downloads-Symbol auf der Kachel.
3. Exportieren Sie die Administratoridentität der Peerorganisation aus Ihrer Wallet.
   - Navigieren Sie zur Registerkarte **Wallet**.
   - Klicken Sie auf die Administratoridentität der Peerorganisation und anschließend auf **Identität exportieren**.
   - Eine JSON-Datei mit den Administratorzertifikaten der Organisation wird erstellt. Notieren Sie den Dateinamen und schützen Sie die Datei. Sie wird beim Erstellen zusätzlicher Peers auf Ihren weiteren Clustern benötigt.

### Schritt 3: Metadaten und Identitäten in Cluster 2 und Cluster 3 importieren
{: #ibp-console-hadr-import-meta23}

1. Führen Sie die Schritte zum [Importieren der JSON-Datei der Zertifizierungsstelle](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca), die Sie aus Cluster 1 exportiert haben, in Cluster 2 und Cluster 3 aus.  
2. Nach dem Hochladen der JSON-Datei müssen Sie die Eintragungs-ID und den geheimen Schlüssel des CA-Administrators eingeben sowie die Eintragungs-ID und den geheimen Schlüssel des TLS-CA-Administrators, die Sie beim Bereitstellen der CA auf Cluster 1 verwendet haben.
2. Importieren Sie die JSON-Datei mit der MSP-Definition der Peerorganisation, die Sie aus Cluster 1 exportiert haben.
   - Klicken Sie auf der Registerkarte **Organisationen** auf **MSP-Definition importieren**.
   - Klicken Sie auf **Datei hinzufügen**, um die JSON-Datei hochzuladen.
   - Klicken Sie auf das Kontrollkästchen **Ich verfüge über eine Administratoridentität für die MSP-Definition**, damit Sie diese MSP-Definition beim Erstellen neuer Peers in anderen Zonen verwenden können.
   - Klicken Sie auf **MSP-Definition importieren**.
3. Importieren Sie die JSON-Datei mit der MSP-Definition der Identität des Organisationsadministrators, die aus Cluster 1 exportiert wurde, in die Konsolenwallet auf Cluster 2 und Cluster 3.

### Schritt 4: Neue Peers in Cluster 2 und Cluster 3 erstellen und einen Kanal hinzufügen
{: #ibp-console-hadr-create-new-peers}

1. Verwenden Sie in Cluster 2 und Cluster 3 die Zertifizierungsstelle (CA), um einen neuen Peerbenutzer zu registrieren. Führen Sie dazu die gleichen Schritte wie beim Registrieren der Peeridentität in Cluster 1 aus. Sie müssen lediglich die Peerbenutzer erstellen, ohne die Identität des Organisationsadministrators erneut zu erstellen, da diese Identität im nächsten Schritt importiert wird.
2. Erstellen Sie neue Peers mithilfe der CA, die Sie als Peer-CA aus Cluster 1 importiert haben, unter Verwendung der MSP-Definition der Peerorganisation, die Sie aus Cluster 1 importiert haben.
3. Wiederholen Sie nun in Cluster 2 und in Cluster 3 die Schritte zum Hinzufügen der Peers zum selben Kanal wie für den Peer in Cluster 1. 
4. Nachdem Sie den Smart Contract auf diesen redundanten Peers installiert haben, wird der Ledger zwischen allen Peers automatisch synchronisiert.
5. Falls Sie die Serviceerkennung, private Daten und Peer-Gossip verwenden möchten, müssen Sie jeden redundanten Peer als Ankerpeer definieren.  

Ihr Netz ist jetzt so konfiguriert, dass sich eine Störung in einer einzelnen Region nicht auf die Verarbeitung der Peer-Workload auswirkt.  

Wenn Sie die Hochverfügbarkeit noch weiter maximieren möchte, ziehen Sie die folgenden zusätzlichen Optionen in Betracht:
- Exportieren Sie die Peers, die Sie in Cluster 2 und Cluster 3 erstellt haben, und importieren Sie sie in die Konsole in Cluster 1. Danach kann das ganze Netz über einen einzigen Cluster verwaltet werden.
- Es kann jedoch vorkommen, dass Cluster 1 ausfällt. Um diesen Fall zu berücksichtigen, können Sie alle Ihre Peers in jede der drei Konsolen importieren. Wenn nun einer der Cluster ausfällt, kann das Netz weiterhin über die Konsolen der beiden anderen Cluster verwaltet werden.
