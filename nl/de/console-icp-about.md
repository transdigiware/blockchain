---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform kann für öffentliche und private Clouds wie {{site.data.keyword.cloud_notm}}, Ihr Rechenzentrum und öffentliche Cluds anderer Anbeiter übergreifend bereitgestellt werden. Diese Bereitstellung für mehrere Clouds wird durch {{site.data.keyword.cloud_notm}} Private, die auf Kubernetes basierende IBM Plattform für Containerorchestrierung, ermöglicht. Dieses Release stellt eine Benutzerschnittstelle für die Blockchain-Konsole bereit, die Sie zum Bereitstellen und Verwalten von Blockchain-Komponenten in einem {{site.data.keyword.cloud_notm}} Private-Cluster verwenden können. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private verwendet die Codebasis von Hyperledger Fabric v1.4.1 und unterstützt die Bereitstellung in {{site.data.keyword.cloud_notm}} Private v3.2.0.
{:shortdesc}

## Vorzüge von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-offers}

Sie können dieses Angebot verwenden, um die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole in einer Bereitstellung von {{site.data.keyword.cloud_notm}} Private zu installieren. Anschließend können Sie die Konsole verwenden, um alle grundlegenden Komponenten einer Hyperledger Fabric-Blockchain (eine Zertifizierungsinstanz, einen Anordnungsservice und Peers) in Ihrem lokalen Cluster zu erstellen. Weitere Informationen zu den Bausteinen von Hyperledger Fabric-Netzen finden Sie unter [Blockchain-Komponenten im Überblick](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview). Außerdem können Sie mit der Konsole ein verteiltes Netz mit mehreren Clouds betreiben, indem Sie Knoten importieren, die in anderen {{site.data.keyword.cloud_notm}} Private-Clustern oder in {{site.data.keyword.cloud_notm}} bereitgestellt wurden.

Dieses Release von {{site.data.keyword.blockchainfull_notm}} Platform enthält die folgenden Schlüsselfunktionen:

**BUILD ---- Integrierte Entwicklererfahrung**
- **Codieren Sie schnell und einfach** Ihre Smart Contracts in Node.js, Golang oder Java, schreiben Sie Clientanwendungen mit der neuen {{site.data.keyword.blockchainfull_notm}}-Erweiterung für  VS Code, nutzen Sie die **SDK-Integration** mit der Benutzerschnittstellenkonsole und lernen Sie mithilfe unserer anschaulichen Lernprogramme und Beispiele.
- **Simplified DevOps** ermöglicht es Ihnen, von der Entwicklungs- in die Test- bis in die Produktionsumgebung in einer einzigen Umgebung zu wechseln, indem Sie Ihre Kubernetes-Ressourcen skalieren, um weitere Komponenten hinzuzufügen.
- **Kubernetes-Serviceintegration.** Nutzen Sie Services wie Grafana und Prometheus für die Protokollierung und Kibana für die Überwachung.
- **Aktuelle Fabric-Schlüsselfunktionen**. Nutzen Sie die neuesten Funktionen von Hyperledger Fabric v1.4.1:
  - [Raft-Anordnungsservice](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Private Datensammlungen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data), die einen höheren Datenschutz gewährleisten, indem sichergestellt wird, dass die Ledger-Daten nur für berechtigte Peers über das Gossip-Protokoll freigegeben werden.
  - Die [Serviceerkennung](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} ermöglicht Ihnen, die Interaktion Ihrer Anwendung mit Ihrem Netz zu erkennen und zu aktualisieren.
  - [Steuerungslisten für Kanalzugriff](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} ermöglichen Ihnen, die Governance Ihrer Kanäle und Smart Contracts genauer zu steuern.

**OPERATE --- Vollständige Kontrolle über Ihre Bereitstellungen**
- **Stellen Sie nur die Komponenten bereit, die Sie benötigen**. Verbinden Sie einen Peer mit mehreren Kanälen und Netzen oder hosten Sie einen Anordnungsservice, mit dem sich Geschäftspartner verbinden können.
- **Verwalten Sie Ihre vollständige Kontrolle über Ihre Identitäten**. Speichern und verwalten Sie die Schlüssel, die zum Verwalten Ihrer Knoten in Ihrer eigenen sicheren Umgebung verwendet werden.
- **Zentralisierter Betrieb**. Die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole ermöglicht Ihnen,  alle Ihre Organisationen und Knoten in **einer Konsole** bereitzustellen und zu verwalten, ohne bei der Verwaltung der Anordnungsknoten oder der Zertifizierungsstelle allein auf {{site.data.keyword.IBM_notm}} oder andere Anbieter angewiesen zu sein. Sie können auch Mitglieder zu einem Blockchain-Konsortium hinzufügen oder daraus entfernen, Kanäle erstellen und Kanälen beitreten sowie Smart Contracts von Ihrer Konsole aus installieren und instanziieren.
- **Netz hosten oder am Netz teilnehmen**. Stellen Sie Peers in Ihrem Cluster mehreren Kanälen in mehreren Clouds bereit oder laden Sie andere Organisationen ein, sich an Ihrem Konsortium oder an Ihren Kanälen zu beteiligen, während die Organisationen ihre Knoten unabhängig und infrastrukturübergreifend verwalten.
- **Verwalten Sie den Zugriff** der Benutzer, die Ihre Knoten verwalten oder überwachen können.
- **Direkter Zugriff auf die Protokolle** Ihrer Knoten über Ihren Kubernetes-Service. Verwenden Sie jeden beliebigen unterstützten Service eines anderen Anbieters zum Extrahieren und Analysieren Ihrer Protokolle.
- **Direkte Interaktion mit Ihren Pods** über Ihren Kubernetes-Service.
- **Dynamische Sammlung von Signaturen** zur besseren Kontrolle über die kooperative Governance bei Kanalkonfigurationen.

**GROW --- Skalierbarkeit und Flexibilität**
- **Gewünschte Rechenleistung auswählen**. Sie können die CPU-, Hauptspeicher- und Speicherkapazität, die Sie in Ihrem Kubernetes-Cluster bereitstellen möchten, flexibel festlegen. Weitere Informationen finden Sie im Abschnitt zur [Interaktion der Konsole mit dem Kubernetes-Cluster](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Skalieren** Sie die Ressourcen in Ihrem Kubernetes-Cluster nach oben und unten, und bezahlen Sie nur für das, was Sie benötigen. Weitere Informationen finden Sie im Abschnitt zur [Preisstruktur](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Disaster-Recovery und Hochverfügbarkeit mit mehreren Zonen.** Diese Option dupliziert Ihre Kubernetes-Bereitstellung in mehreren Zonen und ermöglicht dadurch die Hochverfügbarkeit (High Availability, HA) Ihrer Komponenten sowie die Disaster-Recovery (DR).
- **Standortunabhängig ausführen**. Aufgrund der **einheitlichen Codebasis** der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole, können Ihre Komponenten in jeder von {{site.data.keyword.cloud_notm}} Private unterstützten Umgebung ausgeführt werden.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private ist ein Produktpaket für {{site.data.keyword.cloud_notm}} Private und wird als Kubernetes-Helm-Diagramm bereitgestellt. Weitere Informationen zu {{site.data.keyword.cloud_notm}} Private finden Sie in der Dokumentation für [{{site.data.keyword.cloud_notm}} Private Version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Eignung von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private für Ihre Zwecke
{: #console-icp-about-suitable}

Die Ausführung von {{site.data.keyword.blockchainfull_notm}} Platform außerhalb von {{site.data.keyword.cloud_notm}} bietet ein höheres Maß an Flexibilität in Bezug auf die Vergrößerung und bei der Teilnahme an einem Blockchain-Netz. Netzinitiatoren werden bei der Vergrößerung ihrer eigenen Netze unterstützt, indem neue Mitglieder unter Verwendung der Plattform ihrer Wahl teilnehmen können. Organisationen, die an der Teilnahme an Blockchain-Netzen Interesse haben, können ihre Peers mit ihren vorhandenen Anwendungen zusammenfassen oder bei ihren Systems of Record integrieren.

Benutzer dieses Angebots verwalten ihre Sicherheit und Infrastruktur selbst. {{site.data.keyword.cloud_notm}} stellt diese Services nicht zur Verfügung. Lesen Sie vor Beginn die [Hinweise und Einschränkungen](#console-icp-about-considerations) im nächsten Abschnitt.

## Hinweise und Einschränkungen
{: #console-icp-about-considerations}

Machen Sie sich vor Beginn mit den folgenden Einschränkungen vertraut:

- Für das Management der Statusüberwachung, der Sicherheits- und Protokollierungsfunktionen sowie der Ressourcennutzung Ihrer Blockchain-Komponenten sind Sie selbst verantwortlich.
- Mit der Konsole können nur Komponenten erstellt und gesteuert werden, die auf Hyperledger Fabric v1.4.1 oder höher basieren.
- In jedem Kubernetes-Namensbereich kann jeweils nur eine Konsole bereitgestellt werden. Wenn Sie mehrere Blockchain-Netze erstellen möchten (zum Beispiel für unterschiedliche Umgebungen für Entwicklung. Staging und Produktion) sollten Sie für jede Umgebung einen eindeutigen Namensbereich erstellen.
- Sie können nur Knoten importieren, die aus anderen {{site.data.keyword.blockchainfull_notm}} Platform-Konsolen exportiert wurden. Damit ein importierter Peer oder Anordnungsknoten über die Konsole gesteuert werden kann, müssen Sie auch die zugehörige MSP-Definition der Organisation und die Administratoridentität in Ihre Konsole importieren.
- {{site.data.keyword.blockchainfull_notm}} Platform wird nur unter {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition unterstützt.  {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition wird nicht unterstützt.
- {{site.data.keyword.IBM_notm}} Multicloud Manager kann nicht verwendet werden, um das {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramm zu installieren.

Einen Einblick in die von {{site.data.keyword.cloud_notm}} Private unterstützten Umgebungen finden Sie in [Unterstützte Umgebungen](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Systemvoraussetzungen
{: #console-icp-about-prerequisites}

In [Systemvoraussetzungen](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} sind die Hardware- und Softwarevoraussetzungen für {{site.data.keyword.cloud_notm}} Private aufgelistet. Dabei ist jedoch zu beachten, dass {{site.data.keyword.blockchainfull_notm}} Platform auf POWER8-Systemen unter Linux on Power (ppc64le) nicht unterstützt wird.

Die Ausführung des Helm-Diagramms von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} wurde in Clustern von {{site.data.keyword.cloud_notm}} Private Version 3.2 unter Ubuntu Linux bei Verwendung der folgenden Workerknoten und des folgenden Sicherungsspeichers validiert:

- **LinuxONE und {{site.data.keyword.IBM_notm}} Z**: z/VM und KVM mit Verwendung von NFS.
- **x86**: Linux 64-Bit mit Verwendung von GlusterFS.

## Lizenz
{: #console-icp-about-license}

Weitere Informationen zur Lizenzierung und Preisstruktur finden Sie unter [Preisstruktur für {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing).

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private installieren
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private wird als Helm-Diagrammdatei geliefert, die in einem lokalen {{site.data.keyword.cloud_notm}} Private-Cluster installiert werden kann. Nachdem Sie das Helm-Diagramm installiert haben, finden Sie {{site.data.keyword.blockchainfull_notm}} Platform als Anwendung im {{site.data.keyword.cloud_notm}} Private-Katalog. Anweisungen zum Installieren des Helm-Diagramms und der erforderlichen Voraussetzungen enthält der Abschnitt [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private installieren](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install).

Falls Sie {{site.data.keyword.cloud_notm}} Private noch nicht verwendet haben und Tipps zur Installation und Bereitstellung von {{site.data.keyword.cloud_notm}} Private benötigen, lesen Sie den Abschnitt [{{site.data.keyword.cloud_notm}} Private einrichten](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

Sie können {{site.data.keyword.blockchainfull_notm}} Platform ohne Zugriff auf das öffentliche Internet hinter einer Firewall installieren. Das heruntergeladene Paket für das Helm-Diagramm enthält alle Docker-Images für die Fabric-Komponenten, die von {{site.data.keyword.blockchainfull_notm}} Platform verwendet werden; die Komponenten werden nicht während der Bereitstellung aus Docker Hub extrahiert.

## Hinweise zur Sicherheit
{: #console-icp-about-security}

Da diese Komponenten in Ihrer eigenen Infrastruktur bereitgestellt werden, sind Sie für das Management ihrer Sicherheit verantwortlich. Hierzu gehören auch wichtige Bereiche der Sicherheit wie beispielsweise das Schlüsselmanagement und die Datenverschlüsselung. Machen Sie sich mit den folgenden Themen vertraut, wenn Sie sich mit der Sicherheit für Ihre Komponenten befassen.

### Datensicherheit
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} verwendet die Verschlüsselung ganzer Platten, die auf der [Verschlüsselung mit symmetrischen Schlüsseln](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} basiert, um alle von der Serviceinstanz erstellten Daten zu schützen. In Ihrer eigenen Umgebung müssen Sie zum Schutz Ihrer Peerdaten ähnliche Schritte unternehmen.

Die Daten in Ihrer Statusdatenbank werden nicht verschlüsselt; hierbei ist es ohne Belang, ob Sie LevelDB oder CouchDB verwenden. Mithilfe der Verschlüsselung auf Anwendungsebene können Sie ruhende Daten in Ihrer Statusdatenbank schützen.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### Schlüsselmanagement
{: #console-icp-about-security-key-management}

Das Schlüsselmanagement stellt einen kritischen Aspekt für die Sicherheit dar. Wenn ein privater Schlüssel manipuliert wird oder verloren geht, dann können feindliche Akteure möglicherweise auf Ihre Daten und Funktionalität zugreifen. {{site.data.keyword.IBM_notm}} verwendet als [Hardwaresicherheitsmodule](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) bezeichnete physische Einheiten, um die privaten Schlüssel von {{site.data.keyword.blockchainfull_notm}} Platform Enterprise Plan-Netzen zu speichern.

Sie sind für die Verwaltung Ihrer privaten Schlüssel verantwortlich. Sie können die Benutzerschnittstelle der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole verwenden, um private Schlüssel zu generieren. Die generierten Schlüssel werden jedoch weder von der Konsole noch in {{site.data.keyword.cloud_notm}} Private gespeichert. Ihre Schlüssel müssen unbedingt sicher verwahrt werden, damit sie nicht manipuliert werden können.

Sie können Key Escrow verwenden, um verloren gegangene private Schlüssel wiederherzustellen. Der entsprechende Arbeitsschritt muss ausgeführt werden, bevor Sie einen Schlüssel verlieren. Wenn ein privater Schlüssel nicht wiederhergestellt werden kann, dann müssen Sie neue private Schlüssel anfordern. Hierzu müssen Sie eine neue Identität bei Ihrer Zertifizierungsstelle registrieren. Außerdem sollten Sie in diesem Fall das signCert-Zertifikat von allen Kanälen entfernen, denen Sie beigetreten sind, oder es auf diesen Kanälen ersetzen.

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) ist in das Trust-Modell von Hyperledger Fabric eingebettet. Alle Komponenten von {{site.data.keyword.blockchainfull_notm}} Platform verwenden TLS zur Authentifizierung und Kommunikation mit anderen Komponenten. Aus diesem Grund müssen die Knoten unter {{site.data.keyword.cloud_notm}} Private in der Lage sein, den TLS-Handshake mit anderen Komponenten und mit Ihren Anwendungen auszuführen. Dies hat u. a. zur Folge, dass Sie den Durchgriff von den Client-Apps zu Ihrem Peer in Ihrer Firewall (z. B. über Whitelisting) aktivieren müssen.

### Anwendungssicherheit
{: #console-icp-about-security-appl}

Da alle Chaincode-Aufrufe signiert werden, verwaltet Fabric die Anwendungssicherheit. Zusätzlich hierzu umfasst Fabric auch ACL-basierte Überprüfungen der Anwendungsebene.

## Support anfordern
{: #console-icp-about-support}

Weitere Informationen, wie Sie Support für {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} anfordern und kostenlose Blockchain-Entwicklerressourcen und entsprechende Unterstützungsforen abrufen können, die Sie zum Beheben von Problemen verwenden können, finden Sie im Abschnitt [Support anfordern](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Falls Sie eine Lizenz für {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private erworben haben und sich mit der Kundenunterstützung in Verbindung setzen möchten, finden Sie weitere Informationen unter [Zugreifen auf {{site.data.keyword.IBM_notm}} Support Community und Öffnen eines Support-Tickets](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
