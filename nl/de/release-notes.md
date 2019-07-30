---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# Releaseinformationen
{: #release-notes-saas-20}

In diesen nach Datum gruppierten Releaseinformationen können Sie sich über die neuesten Änderungen an {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} informieren. Dieses Produkt basiert auf Hyperledger Fabric v1.4.1.
{:shortdesc}


## 24. Mai 2019
{: #05-24-2019}

**Raft-Konsensprotokoll**: Der für Produktionsnetze empfohlene Raft-Anordnungsservice mit fünf Knoten ist jetzt verfügbar. Darüber hinaus können Sie für Entwicklungs- und Testzwecke einen Raft-Anordnungsservice mit nur einem Knoten bereitstellen.

## 9. Mai 2019
{: #05-09-2019}

**Einführung der APIs für die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole**

Es werden nun APIs zur Verfügung gestellt, mit denen Knoten für Peers, Anordnungsknoten und Zertifizierungsstellen bereitgestellt, bearbeitet und gelöscht werden können, sodass die Erstellung Ihres Blockchain-Netzes über ein Script ausgeführt werden kann. In der Dokumentation im [{{site.data.keyword.cloud_notm}}-API-Dokumentationsrepository](/apidocs/blockchain#introduction){: external} finden Sie weiterführende Informationen zu den APIs und können diese testen. Darüber hinaus finden Sie im Abschnitt zur [Erstellung eines Netzes mit APIs](/docs/services/blockchain?topic=blockchain-ibp-v2-apis) Anweisungen zur Verwendung der APIs für die Erstellung Ihres Netzes.  

**Kanalgovernance**  

Die Aktualisierungen der Kanalgovernance ermöglichen Ihnen die Neukonfiguration von Richtlinien. Darüber hinaus können Sie steuern, welche Kanalmitglieder eine Kanalaktualisierung signieren müssen und Sie können die Sammlung von Signaturen koordinieren.

## 17. April 2019
{: #04-17-2019}

**Fähigkeit zur Größenänderung und Skalierung von Knotenressourcen**  

Wenn Sie einen Knoten bereitstellen, haben Sie jetzt die Möglichkeit, bei Bedarf die Menge an CPU, Hauptspeicher und Speicher für Ihre Container anzugeben. Sie können ihre Ressourcen zu einem späteren Zeitpunkt entsprechend den Nutzungsmustern nach oben oder unten skalieren. Weitere Informationen finden Sie unter [Ressourcen zuordnen](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources).

**Verwendung von {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

IAM wird verwendet, um den Benutzerzugriff auf die Konsolen-Benutzerschnittstelle zu steuern und die Aktionen, die Benutzer in der Benutzerschnittstelle ausführen können, einzuschränken.  Lesen Sie hierzu das Thema zur Vorgehensweise beim [Hinzufügen und Entfernen von Benutzern aus der Konsole](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).

## 3. April 2019
{: #04-03-2019}

**Unterstützung für externe Zertifizierungsstelle**

Wenn Sie einen Peer oder Anordnungsknoten hinzufügen, haben Sie die Möglichkeit, Zertifikate von einer externen Zertifizierungsstelle zu verwenden, die nicht von {{site.data.keyword.IBM_notm}} gehostet wird. Lesen Sie dazu dieses Thema über die [Verwendung von Zertifikaten einer externen Zertifizierungsstelle mit Ihrem Peer oder Anordnungsknoten](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca).

**Anordnungsknoten optimieren**

In der Konsole stehen neue Optimierungsparameter für Anordnungsknoten zur Verfügung, mit deren Hilfe Sie den Durchsatz und die Leistung des Anordnungsknotens besser steuern können. Anweisungen zur Konfiguration dieser Parameter finden Sie im Thema [Anordnungsknoten optimieren](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning).
