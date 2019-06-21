---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-29"

keywords: IBM Blockchain Platform, release, new features

subcollection: blockchain

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}

# Neuerungen
{: #whats-new}

## 29. Mai 2019
{: #whats-new-5-29-2019}

Das {{site.data.keyword.blockchainfull}} Platform-Produkt der zweiten Generation wird allgemein verfügbar. Es ermöglicht Ihnen die Bereitstellung, den Betrieb und die Überwachung Ihres Blockchain-Netzes. Dieses Release umfasst eine Konsole mit neuer Benutzerschnittstelle, die zum Bereitstellen und Verwalten von Blockchain-Komponenten in Ihrem {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster unter {{site.data.keyword.cloud_notm}} verwendet werden kann.

Dieses Release von {{site.data.keyword.blockchainfull_notm}} Platform enthält die folgenden Schlüsselfunktionen:

**BUILD ---- Integrierte Entwicklererfahrung**
- **Codieren Sie ganz einfach** Ihre Smart Contracts in Node.js, Golang oder Java, schreiben Sie Clientanwendungen mit der neuen {{site.data.keyword.blockchainfull_notm}}-Erweiterung für VS Code, nutzen Sie die **SDK-Integration** mit der Konsole und lernen Sie mithilfe unserer umfangreichen Lernprogramme und Beispiele.
- **Simplified DevOps** ermöglicht es Ihnen, von der Entwicklungs- in die Test- bis in die Produktionsumgebung in einer einzigen Umgebung zu wechseln, indem Sie Ihre Kubernetes-Ressourcen skalieren, um weitere Komponenten hinzuzufügen.
- **Aktuelle Fabric-Schlüsselfunktionen**. Nutzen Sie die neuesten Funktionen von Hyperledger Fabric v1.4.1:
  -  [Raft-Anordnungsservice ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft "Raft-Anordnungsservice")]
  - **{{site.data.keyword.cloud_notm}}-Serviceintegration.** Nutzen Sie die in {{site.data.keyword.cloud_notm}} integrierten Services wie z. B. {{site.data.keyword.cloud_notm}} Kubernetes Service Dashboard, {{site.data.keyword.IBM_notm}} Log Analysis with LogDNA und {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
  - [**Private Datensammlungen**](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data), die einen höheren Datenschutz gewährleisten, indem sichergestellt wird, dass die Ledger-Daten nur an berechtigte Peers über das Gossip-Protokoll freigegeben werden.
  - Die [Serviceerkennung ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html "Service discovery") ermöglicht es Ihnen, die Interaktion Ihrer Anwendung mit Ihrem Netz dynamisch zu erkennen und zu aktualisieren.
  - Die [Kanal-Zugriffssteuerungslisten ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html "Access Control Lists") ermöglichen es Ihnen, die Governance Ihrer Kanäle und Smart Contracts zusätzlich zu steuern.

**OPERATE --- Vollständige Kontrolle über Ihre Bereitstellungen**
- **Stellen Sie nur die Komponenten bereit, die Sie benötigen**. Verbinden Sie einen Peer mit mehreren Kanälen und Netzen oder hosten Sie einen Anordnungsservice, mit dem sich Geschäftspartner verbinden können.
- **Verwalten Sie Ihre vollständige Kontrolle über Ihre Identitäten**. Speichern und verwalten Sie die Schlüssel, die für die Verwaltung Ihrer Knoten verwendet werden, ohne Ihre privaten Schlüssel in {{site.data.keyword.cloud_notm}} zu speichern.
- **Zentralisierter Betrieb**. Die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole ermöglicht es Ihnen, alle Ihre Organisationen und Knoten in **einer zentralen Konsole** bereitzustellen und zu verwalten, ohne sich dabei auf {{site.data.keyword.IBM_notm}} oder andere Anbieter verlassen zu müssen, um Ihre Anordnungsknoten oder Zertifizierungsstellen zu verwalten. Sie können auch Mitglieder zu einem Blockchain-Konsortium hinzufügen oder daraus entfernen, Kanäle erstellen und Kanälen beitreten sowie Smart Contracts von Ihrer Konsole aus installieren und instanziieren.
- **Netz hosten oder am Netz teilnehmen**. Stellen Sie Peers in Ihrem Cluster mehreren Kanälen in mehreren Clouds bereit oder laden Sie andere Organisationen ein, sich an Ihrem Konsortium oder an Ihren Kanälen zu beteiligen, während die Organisationen ihre Knoten unabhängig und infrastrukturübergreifend verwalten.
- **Verwalten Sie den Zugriff** der Benutzer, die Ihre Knoten verwalten oder überwachen können.
- **Direkter Zugriff auf die Protokolle** Ihrer Knoten über {{site.data.keyword.IBM_notm}} Kubernetes Service. Verwenden Sie den {{site.data.keyword.cloud_notm}} Log Analysis-Service oder den Service eines anderen Anbieters, um Ihre Protokolle zu extrahieren und zu analysieren.
- **Interagieren Sie direkt mit Ihren Pods** und verwenden Sie dazu das Kubernetes-Dashboard. Führen Sie den Befehl Exec zum Aufrufen Ihrer Pods und Container aus, um Befehle auszuführen und Zertifikate über die Befehlszeile zu aktualisieren.
- **Dynamische Sammlung von Signaturen** zur besseren Kontrolle über die kooperative Governance bei Kanalkonfigurationen.

**GROW --- Skalierbarkeit und Flexibilität**
- **Wählen Sie die gewünschte Rechenleistung aus.**. Sie können die CPU-, Hauptspeicher- und Speicherkapazität, die Sie in Ihrem Kubernetes-Cluster bereitstellen möchten, flexibel festlegen. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zur [Interaktion von {{site.data.keyword.cloud_notm}} Kubernetes Service mit der Konsole](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction).
- **Skalieren** Sie die Ressourcen in Ihrem Kubernetes-Cluster nach oben und unten, und bezahlen Sie nur für das, was Sie benötigen. Weitere Informationen finden Sie im Abschnitt zur [Preisstruktur](/docs/services/blockchain/howto/pricing.html#ibp-pricing).
- **Disaster Recovery and Multi-Zone High Availability:** Diese Option dupliziert Ihre Kubernetes-Bereitstellung in mehreren Zonen, wodurch die Hochverfügbarkeit (HA) Ihrer Komponenten und die Disaster-Recovery (DR) aktiviert werden.
- **Anywhere ausführen** (Anweisungen werden demnächst bereitgestellt). Aufgrund der **einheitlichen Codebasis** der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole können Sie Ihre Komponenten unter {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Private und in öffentlichen Clouds anderer Anbieter ausführen.

- Weitere Informationen zur kostenlosen Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 sind in [Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) verfügbar.
- Anweisungen zum Bereitstellen der kostenlosen Betaversion 2.0 in einem {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster sind in [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks) verfügbar.
- Neue Lernprogramme für die Verwendung der kostenlosen Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 sind im Unterabschnitt zu **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** unter der Kategorie **VORGEHENSWEISE** verfügbar.
  * Das [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) führt Sie durch das Hosten eines Netzes, indem ein Anordnungsknoten und Peer erstellt werden.
  * Das [Lernprogramm zum Teilnehmen an einem Netz](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network) erläutert, wie Sie einem vorhandenen Netz beitreten können, indem Sie einen Peer erstellen und einem Kanal zuordnen.
  * Im [Lernprogramm zum Bereitstellen eines Smart Contract im Netz](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) erfahren Sie, wie Sie einen Smart Contract schreiben und in Ihrem Netz bereitstellen.
- Das kostenlose Betaangebot für {{site.data.keyword.blockchainfull_notm}} Platform 2.0 basiert auf Hyperledger Fabric v1.4.1 und unterstützt Peer-to-Peer-Gossip, die Serviceerkennung und die Verwendung privater Daten. Weitere Informationen zum Konfigurieren privater Daten in Ihrem Netz finden Sie in diesem [Abschnitt](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data).

## 9. Mai 2019
{: #whats-new-5-09-2019}

{{site.data.keyword.blockchainfull_notm}}-Releases Version 1.0.0 der {{site.data.keyword.blockchainfull_notm}} Platform-Erweiterung für Visual Studio (VS) Code. Dieses Entwicklertoolkit umfasst die folgenden wichtigen Leistungsmerkmale:

**Neu konfigurierte Benutzerschnittstelle zur einfacheren Navigation**

**Präskriptive und detaillierte Lernprogramme mit folgenden Themen:**
- Entwicklung eines Smart Contract
- Bereitstellung einer Instanz des {{site.data.keyword.blockchainfull_notm}}-Service unter {{site.data.keyword.cloud_notm}}
- Bereitstellung und Transaktion mit der {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz

**Alle häufig verwendeten Funktionen aus den Vorgängerreleases einschließlich:**
- Funktionalität zum Debuggen von Smart Contracts
- Beispielcode
- Aktualisierte Walletfunktionalität

Weitere Informationen hierzu finden Sie im Abschnitt zum [Entwickeln von Smart Contracts mit der Visual Studio Code-Erweiterung](/docs/services/blockchain/vscode-extension.html "Entwickeln von Smart Contracts mit der Visual Studio Code-Erweiterung").

## 23. April 2019
{: #whats-new-4-23-2019}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private gibt Version 1.0.2 frei, die die Hyperledger Fabric Version 1.4.0-Codebasis verwendet und die Bereitstellung unter {{site.data.keyword.cloud_notm}} Private Version 3.1.2 unterstützt.

Informationen zu einem Upgrade Ihrer vorhandenen {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private auf Version 1.0.2 finden Sie unter [Upgrade des Helm-Diagramms auf {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/helm_install_icp.html#helm-install-upgrading).

Weitere Informationen zu Hyperledger Fabric Version 1.4.0 finden Sie in der [Dokumentation zu Hyperledger Fabric ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ "Dokumentation zu Hyperledger Fabric Version 1.4"). Weitere Informationen zu {{site.data.keyword.cloud_notm}} Private finden Sie in der [Dokumentation zu {{site.data.keyword.cloud_notm}} Private Version 3.1.2 ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.2/kc_welcome_containers.html "Dokumentation zu {{site.data.keyword.cloud_notm}} Private Version 3.1.2").

## 8. Februar 2019
{: #whats-new-2-08-2019}

{{site.data.keyword.blockchainfull}} Platform gibt eine kostenlose 2.0-Betaversion aus: die nächste Generation von {{site.data.keyword.blockchainfull_notm}} Platform-Lösungen, mit denen Sie ein Blockchain-Netz bereitstellen, betreiben und überwachen können. Die kostenlose Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 beta enthält eine neue Benutzerschnittstellenkonsole, die verwendet werden kann, um Blockchain-Komponenten in Ihrem eigenen {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster in {{site.data.keyword.cloud_notm}} bereitzustellen und zu verwalten. Die kostenlose Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 ermöglicht Ihnen Folgendes:

**Erstellen Sie Ihr Netz schneller und einfacher mit einem nahtlosen Erlebnis.**

*	Reibungslose Integration zwischen Smart-Contract-Entwicklung (VS Code) und Netzmanagement
*	Simplified DevOps ermöglichen den Wechsel von einer einzigen Konsole aus von der Entwicklungs- über die Test- zur Produktionsumgebung
*	Unterstützung für das Schreiben von Smart Contracts in Javascript-, Java- und Go-Sprachen

**Betreiben und regeln Sie Netze mit maximaler Kontrolle.**

*	Bereitstellung nur der von Ihnen benötigten Blockchain-Komponenten (Peer, Anordnungsservice, Zertifizierungsstelle)
*	Überarbeitete Konsole zur Verwaltung von Netzkomponenten an einem Ort, unabhängig vom Ort ihrer Bereitstellung
*	Maximale Kontrolle über Ihre Identitäten, Ledger und Smart Contracts

**Erweitern Sie verteilte Netze problemlos mit der neuen Multi-Cloud-Flexibilität.**

*	Verbindung zu Knoten, die in einer beliebigen Umgebung (lokal, öffentlich, Hybrid-Clouds) ausgeführt werden
*	Einfache Verbindung eines einzelnen Peers mit mehreren Branchennetzen
*	Klein anfangen und je nach Wachstum für das bezahlen, was Sie verwenden, keine Vorlaufinvestitionen und einfache Upgrades durch Kubernetes

- Weitere Informationen zur kostenlosen Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 sind in [Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) verfügbar.
- Anweisungen zum Bereitstellen der kostenlosen Betaversion 2.0 in einem {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster sind in [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks) verfügbar.
- Neue Lernprogramme für die Verwendung der kostenlosen Betaversion {{site.data.keyword.blockchainfull_notm}} Platform 2.0 sind im Unterabschnitt zu **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** unter der Kategorie **VORGEHENSWEISE** verfügbar.
  * Das [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) führt Sie durch das Hosten eines Netzes, indem ein Anordnungsknoten und Peer erstellt werden.
  * Das [Lernprogramm zum Teilnehmen an einem Netz](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network) erläutert, wie Sie einem vorhandenen Netz beitreten, indem Sie einen Peer erstellen und mit einem Kanal verbinden.
  * Im [Lernprogramm zum Bereitstellen eines Smart Contract im Netz](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) erfahren Sie, wie Sie einen Smart Contract schreiben und in Ihrem Netz bereitstellen.
- Das kostenlose Beta-Angebot {{site.data.keyword.blockchainfull_notm}} Platform 2.0 basiert auf Hyperledger Fabric v1.4 und unterstützt die Peer-to-Peer-Gossip, Serviceerkennung und private Daten. Weitere Informationen zum Konfigurieren privater Daten in Ihrem Netz finden Sie in diesem [Abschnitt](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data).

- Die {{site.data.keyword.blockchainfull_notm}}-Erweiterung für Visual Studio Code ist über den Visual Studio Code Marketplace verfügbar. Entwickler können die Erweiterung verwenden, um für eine Instanz von Hyperledger Fabric einen Smart Contract zu erstellen, zu testen und bereitzustellen. Die Erweiterung ist mit Hyperledger Fabric 1.3 und höher kompatibel. Weitere Informationen zur Verwendung der VSCode-Erweiterung finden Sie unter [Tools für Smart Contracts](/docs/services/blockchain/vscode-extension.html#develop-vscode).  

Das Inhaltsverzeichnis wird erweitert, indem alle Einführungsthemen zusammen in einem Abschnitt mit dem Namen **Einführungslernprogramme** zusammengefasst werden. Darüber hinaus wird jedes der {{site.data.keyword.blockchainfull_notm}} Platform-Angebote im Unterabschnitt **Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform** des Abschnitts **LERNEN/ERKUNDEN** beschrieben.

**Hinweis:** Für Benutzer des Starter Plans sind keine kostenlosen Cloud-Punkte mehr verfügbar.

## 7. Dezember 2018
{: #whats-new-12-07-2018}

Die Abschnitte *Starter Plan-Netz betreiben* und *Enterprise Plan-Netz betreiben* wurden kombiniert und durch ein einziges Lernprogramm namens [Network Monitor verwenden](/docs/services/blockchain/v10_dashboard.html#ibp-dashboard) ersetzt.

## 27. November 2018
{: #whats-new-11-27-2018}

{{site.data.keyword.blockchainfull_notm}} Platform veröffentlicht {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.cloud_notm}} Private ist eine Anwendungsplattform für die Entwicklung und Verwaltung von containerisierten Anwendungen, die auf Kubernetes basieren, und ermöglicht Benutzern die Bereitstellung von Zertifizierungsstellen, Anordnungsknoten und Peers unter x86, LinuxONE und IBM Z. {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private basiert auf Hyperledger Fabric Version 1.2.1 und wird unter Verwendung von Kubernetes-Helm-Diagrammen bereitgestellt. Nach der Installation des Helm-Diagramms ist die Plattform als Servicepaket im {{site.data.keyword.cloud_notm}}-Katalog enthalten. Beachten Sie, dass [dieses Angebot](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) eher für erfahrene Fabric-Benutzer geeignet ist.

Weitere Informationen zu {{site.data.keyword.cloud_notm}} Private enthält die [Dokumentation zu {{site.data.keyword.cloud_notm}} Private Version 3.1.0 ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/kc_welcome_containers.html "{{site.data.keyword.cloud_notm}} Private Version 3.1.0 - Dokumentation").

Mit dem Release von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private wird die Betaversion von {{site.data.keyword.blockchainfull_notm}} Platform Remote Peer eingestellt. Ein Peer kann weiterhin in {{site.data.keyword.cloud_notm}} Private oder AWS bereitgestellt und mit einem Starter Plan- oder Enterprise Plan-Netz verbunden werden. Weitere Informationen enthalten die Abschnitte [Peer für Starter oder Enterprise Plan bereitstellen](/docs/services/blockchain/howto/peer_deploy_ibp.html#ibp-peer-deploy) und [Peer in AWS bereitstellen](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws). Diese Peers werden nicht als ferne Peers, sondern als **verteilte** Peers betrachtet, weil die Bereitstellung durch den Kunden verwaltet wird und es daher keine zentrale Position und somit auch keinen als fern definierten Standort gibt.

In diesem Release wurden auch einige Verbesserungen am Inhaltsverzeichnis für die Dokumentation vorgenommen. Als Starter- oder Enterprise-Benutzer befindet sich der für Sie relevante Inhalt weiterhin in den Abschnitten **INFORMATIONEN**, **VORGEHENSWEISE**, **REFERENZINFORMATIONEN** und  **HILFE**; auch die Links sind identisch. Der Inhalt befindet sich möglicherweise lediglich in einem anderen Unterabschnitt des Inhaltsverzeichnisses.

## 13. November 2018
{: #whats-new-11-13-2018}

{{site.data.keyword.blockchainfull_notm}} Platform gibt {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services (AWS) frei.

Mit {{site.data.keyword.blockchainfull_notm}} Platform for AWS können Sie Peers in der AWS-Cloud ausführen und mit vorhandenen Blockchain-Netzen in {{site.data.keyword.cloud_notm}} verbinden. Ihre Peers in AWS können das Verbindungsprofil und andere Blockchain-Komponenten in den vorhandenen Netzen nutzen; gleichzeitig erhalten Sie eine größere Flexibilität bei der Zusammenfassung Ihrer Peers mit anderen Anwendungen außerhalb von {{site.data.keyword.cloud_notm}}. Weitere Angaben enthält der Abschnitt [Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws).
/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws

## 4. Oktober 2018
{: #whats-new-10-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform-Upgrade für den Starter Plan von Hyperledger Fabric Version 1.1.0 auf Version 1.2.1. Weitere Informationen finden Sie unter [Informationen zum Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

**Wichtig:** Fabric Version 1.2.1 ist gegenwärtig nicht mit der Betaversion von Remote Peer kompatibel, die einen Peer von Version 1.1.0 nutzt. Starter Plan-Netze, die vor dem 4. Oktober 2018 erstellt wurden und auf Fabric Version 1.1.0 basieren, sind hiervon nicht betroffen und können weiterhin zusammen mit der Betaversion von Remote Peer eingesetzt werden. {{site.data.keyword.blockchainfull_notm}} Platform wird zu einem späteren Zeitpunkt die Betaversion von Remote Peer für die Verwendung des Peers von Version 1.2.1 aktualisieren, wodurch eine Kompatibilität mit neueren Starter-Netzen mit der Fabric-Version 1.2.1 und Enterprise-Netzen mit der Fabric-Version 1.1.0 erreicht wird. Versuchen Sie bis zur Aktualisierung der Betaversion von Remote Peer auf die Fabric-Version 1.2.1 nicht, einen fernen Peer der Version 1.1.0 mit einem neuen Starter-Netz der Version 1.2.1 bereitzustellen.

## 4. September 2018
{: #whats-new-9-04-2018}

{{site.data.keyword.blockchainfull_notm}} Platform gibt die Betaversion für das Remote-Peer-Angebot frei. Das Remote-Peer-Angebot basiert auf Hyperledger Fabric Version 1.1.0. Mithilfe von Remote Peer können Sie Peerknoten von {{site.data.keyword.blockchainfull_notm}} in Ihrer eigenen Cloudumgebung von {{site.data.keyword.cloud_notm}} Private oder Amazon Web Services (AWS) ausführen. Weitere Informationen finden Sie unter [Informationen zu fernen Peers](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws).

## 15. Juni 2018
{: #whats-new-6-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform gibt GA-Version von Starter Plan frei. Weitere Informationen finden Sie unter [Informationen zum Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

## 15. Mai 2018
{: #whats-new-5-15-2018}

{{site.data.keyword.blockchainfull_notm}} Platform-Upgrade für den Enterprise Plan von Hyperledger Fabric Version 1.0.0 auf Version 1.1.0. Weitere Angaben enthält der Abschnitt [Informationen zum Enterprise Plan](/docs/services/blockchain/enterprise_plan.html#enterprise-plan-about).

## 18. März 2018
{: #whats-new-3-18-2018}

{{site.data.keyword.blockchainfull_notm}} Platform gibt die Betaversion für den Starter Plan frei. Der Starter Plan bietet Ihnen eine Entwicklungs- und Testumgebung, in der Sie die Verwendung in einem {{site.data.keyword.blockchainfull_notm}} Platform-Netz lernen und üben können. Weitere Informationen finden Sie unter [Informationen zum Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

## 11. August 2017
{: #whats-new-8-11-2017}

{{site.data.keyword.blockchainfull_notm}} Platform ersetzt {{site.data.keyword.blockchainfull_notm}} on Cloud und gibt den Enterprise Plan frei. Weitere Angaben enthält der Abschnitt [Informationen zum Enterprise Plan](/docs/services/blockchain/enterprise_plan.html#enterprise-plan-about).
