---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: key features, build, operate, grow, architecture, multizone clusters

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

# Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}
{: #ibp-console-overview}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} ist die nächste Generation der {{site.data.keyword.blockchainfull_notm}} Platform-Angebote, mit denen Sie die vollständige Kontrolle über Ihre Bereitstellungen, Zertifikate und privaten Schlüssel haben. Das Produkt enthält die neue {{site.data.keyword.blockchainfull_notm}} Platform-Konsole, d. h. eine Benutzerschnittstelle, die den Prozess der Bereitstellung von Komponenten in einem von Ihnen verwalteten und gesteuerten {{site.data.keyword.cloud_notm}} Kubernetes Service vereinfachen und beschleunigen kann. Weitere Informationen zu Kubernetes und {{site.data.keyword.cloud_notm}} Kubernetes Service finden Sie unter [Kubernetes](/docs/services/blockchain/reference?topic=blockchain-k8s-overview).
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} basiert auf Hyperledger Fabric 1.4.1. Weitere Informationen zu den neuen Features von Hyperledger Fabric 1.4.1 finden Sie in [Neuerungen in 1.4](https://hyperledger-fabric.readthedocs.io/en/release-1.4/whatsnew.html){: external}.

## Angebot des neuen Release
{: #ibp-console-overview-capabilities}

Dieses neueste Release ist auf erfahrene Benutzer von {{site.data.keyword.blockchainfull_notm}} und Hyperledger Fabric zugeschnitten und ermöglicht diesen Nutzern, {{site.data.keyword.blockchainfull_notm}}-Netze zu hosten und den Netzen beizutreten. Wenn Sie bereits Starter Plan- oder Enterprise Plan-Kunde sind, muss nicht {{site.data.keyword.IBM_notm}} Ihr Netz verwalten, sondern Sie haben jetzt die vollständige Kontrolle und die Möglichkeit, Ihre Komponenten in Ihrem eigenen Kubernetes-Cluster bereitzustellen, zu überwachen und zu verwalten.

Dieses Release von {{site.data.keyword.blockchainfull_notm}} Platform enthält die folgenden Schlüsselfunktionen:

**BUILD ---- Integrierte Entwicklererfahrung**
- **Codieren Sie ganz einfach** Ihre Smart Contracts in Node.js, Golang oder Java, schreiben Sie Clientanwendungen mit der neuen {{site.data.keyword.blockchainfull_notm}}-Erweiterung für VS Code, nutzen Sie die **SDK-Integration** mit der Konsole und lernen Sie mithilfe unserer umfangreichen Lernprogramme und Beispiele.
- **Simplified DevOps** ermöglicht es Ihnen, von der Entwicklungs- in die Test- bis in die Produktionsumgebung in einer einzigen Umgebung zu wechseln, indem Sie Ihre Kubernetes-Ressourcen skalieren, um weitere Komponenten hinzuzufügen.
- **Aktuelle Fabric-Schlüsselfunktionen**. Nutzen Sie die neuesten Funktionen von Hyperledger Fabric v1.4.1:
  -  [Raft-Anordnungsservice](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [**Private Datensammlungen**](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data), die einen höheren Datenschutz gewährleisten, indem sichergestellt wird, dass die Ledger-Daten nur für berechtigte Peers über das Gossip-Protokoll freigegeben werden.
  - Die [Serviceerkennung](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} ermöglicht Ihnen, die Interaktion Ihrer Anwendung mit Ihrem Netz dynamisch zu erkennen und zu aktualisieren.
  - Die [Steuerungslisten für Kanalzugriff](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} ermöglichen Ihnen, die Governance Ihrer Kanäle und Smart Contracts genauer zu steuern.
- {{site.data.keyword.cloud_notm}}-Serviceintegration. Nutzen Sie die integrierten [{{site.data.keyword.cloud_notm}}-Services](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks-integrations) wie {{site.data.keyword.cloud_notm}} Kubernetes Service Dashboard, {{site.data.keyword.IBM_notm}} Log Analysis with LogDNA und {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).


**OPERATE --- Vollständige Kontrolle über Ihre Bereitstellungen**
- **Stellen Sie nur die Komponenten bereit, die Sie benötigen**. Verbinden Sie einen Peer mit mehreren Kanälen und Netzen oder hosten Sie einen Anordnungsservice, mit dem sich Geschäftspartner verbinden können.
- **Verwalten Sie Ihre vollständige Kontrolle über Ihre Identitäten**. Speichern und verwalten Sie die Schlüssel, die für die Verwaltung Ihrer Knoten verwendet werden, ohne Ihre privaten Schlüssel in {{site.data.keyword.cloud_notm}} zu speichern.
- **Zentralisierter Betrieb**. Die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole ermöglicht es Ihnen, alle Ihre Organisationen und Knoten in **einer zentralen Konsole** bereitzustellen und zu verwalten, ohne sich dabei auf {{site.data.keyword.IBM_notm}} oder andere Anbieter verlassen zu müssen, um Ihre Anordnungsknoten oder Zertifizierungsstellen zu verwalten. Sie können auch Mitglieder zu einem Blockchain-Konsortium hinzufügen oder daraus entfernen, Kanäle erstellen und Kanälen beitreten sowie Smart Contracts von Ihrer Konsole aus installieren und instanziieren.
- **Netz hosten oder am Netz teilnehmen**. Stellen Sie Peers in Ihrem Cluster mehreren Kanälen in mehreren Clouds bereit oder laden Sie andere Organisationen ein, sich an Ihrem Konsortium oder an Ihren Kanälen zu beteiligen, während die Organisationen ihre Knoten unabhängig und infrastrukturübergreifend verwalten.
- **Verwalten Sie den Zugriff** der Benutzer, die Ihre Knoten verwalten oder überwachen können.
- **Direkter Zugriff auf die Protokolle** Ihrer Knoten über {{site.data.keyword.IBM_notm}} Kubernetes Service. Verwenden Sie den {{site.data.keyword.cloud_notm}} Log Analysis-Service oder einen Service eines anderen Anbieters, um Ihre Protokolle zu extrahieren und zu analysieren.
- **Interagieren Sie direkt mit Ihren Pods** und verwenden Sie dazu das Kubernetes-Dashboard. Führen Sie den Befehl Exec zum Aufrufen Ihrer Pods und Container aus, um Befehle auszuführen und Zertifikate über die Befehlszeile zu aktualisieren.
- **Dynamische Sammlung von Signaturen** zur besseren Kontrolle über die kooperative Governance bei Kanalkonfigurationen.

**GROW --- Skalierbarkeit und Flexibilität**
- **Wählen Sie Ihre Rechenleistung aus.**. Sie können die CPU-, Hauptspeicher- und Speicherkapazität, die Sie in Ihrem Kubernetes-Cluster bereitstellen möchten, flexibel festlegen. Weitere Informationen zu diesem Thema finden Sie im Abschnitt zur [Interaktion von {{site.data.keyword.cloud_notm}} Kubernetes Service mit der Konsole](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Skalieren** Sie die Ressourcen in Ihrem Kubernetes-Cluster nach oben und unten, und bezahlen Sie nur für das, was Sie benötigen. Weitere Informationen finden Sie im Abschnitt zur [Preisstruktur](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Disaster Recovery and Hochverfügbarkeit in mehreren Regionen.** Diese Option dupliziert Ihre Kubernetes-Bereitstellung in mehreren Regionen, wodurch die Hochverfügbarkeit (HA) Ihrer Komponenten und die Disaster-Recovery (DR) aktiviert werden.
- **Anywhere ausführen** (Anweisungen werden demnächst bereitgestellt). Aufgrund der **einheitlichen Codebasis** der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole können Sie Ihre Komponenten unter {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Private und in öffentlichen Clouds anderer Anbieter ausführen.

Informationen zu den nächsten Schritten beim Bereitstellen von Blockchain für standortunabhängige Unternehmen finden Sie [in diesem Blog](https://www.ibm.com/blogs/blockchain/2019/02/taking-the-next-step-towards-deploying-blockchain-anywhere){: external}.

Dieses Angebot ist für erfahrene Fabric-Benutzer ausgelegt, die eigene Netze erstellen und verwalten möchten.

## Wichtige Hinweise
{: #ibp-console-overview-considerations}

Stellen Sie vor der Bereitstellung der Konsole sicher, dass Sie die folgenden Aspekte verstehen:

- Da sich die Verfügbarkeit der Betatestversion und des allgemein verfügbaren Release (GA-Release) von {{site.data.keyword.blockchainfull_notm}} Platform überschneiden, sollten Sie unbedingt die Version von {{site.data.keyword.blockchainfull_notm}} Platform ermitteln, mit der Sie momentan arbeiten. Neue Funktionen und Fixes werden nicht in die Betaversion übernommen, stehen jedoch in der GA-Version von {{site.data.keyword.blockchainfull_notm}} Platform zur Verfügung. Wenn Sie die Betaversion von {{site.data.keyword.blockchainfull_notm}} Platform verwenden, dann stimmen wahrscheinlich bestimmte Anzeigen in Ihrer Konsole nicht mit den Darstellungen in der aktuellen Dokumentation überein, die stets auf den Stand der allgemein verfügbaren Serviceinstanz aktualisiert wird. Um alle Vorteile der neuesten Funktionalität nutzen zu können, sollten Sie eine neue GA-Serviceinstanz bereitstellen. Informationen zu diesem Arbeitsschritt finden Sie in der [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
- Alle Peers, die mit diesem Release bereitgestellt werden, verwenden CouchDB als Statusdatenbank.
- Sie sind für das Management der Statusüberwachung, der Sicherheit und der Protokollierung Ihres Kubernetes-Cluster verantwortlich. Hier finden Sie [weitere Informationen](/docs/containers?topic=containers-responsibilities_iks#your-responsibilities-by-using-ibm-cloud-kubernetes-service){: external} dazu, welche Aspekte von {{site.data.keyword.cloud_notm}} verwaltet werden und für welche Aspekte Sie verantwortlich sind.
- Sie sind auch für die Überwachung der Ressourcennutzung Ihres Kubernetes-Clusters verantwortlich. Zur Überwachung Ihrer Kubernetes-Ressourcen wird empfohlen, das Tool [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} in Kombination mit Ihrem {{site.data.keyword.cloud_notm}} Kubernetes-Dashboard zu verwenden. Wenn Sie die Speicherkapazität oder die Leistung Ihres Clusters erhöhen müssen, lesen Sie diese Informationen zum [Ändern des vorhandenen Datenträgers](/docs/containers?topic=containers-file_storage#change_storage_configuration){: external}.
- Sie sind für die Verwaltung und Sicherung Ihrer Zertifikate und privaten Schlüssel verantwortlich. {{site.data.keyword.IBM_notm}} speichert Ihre Zertifikate nicht im Kubernetes-Cluster und nicht in der Konsole. Sie sind nur im lokalen Speicher Ihres Browsers gespeichert. Wenn Sie den Browser wechseln, müssen Sie Ihre erstellten Identitäten in diesen Browser importieren.
- {{site.data.keyword.blockchainfull_notm}} Platform steht in ausgewählten Regionen zur Verfügung. Eine aktualisierte Liste hierzu finden Sie im Abschnitt zu den [Standorten von {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto?topic=blockchain-ibp-regions-locations).
- {{site.data.keyword.blockchainfull_notm}} Platform kann nicht in OpenShift-Clustern bereitgestellt werden, die mit dem {{site.data.keyword.IBM_notm}} Kubernetes-Service erstellt wurden.
- Kubernetes muss in Ihrem {{site.data.keyword.cloud_notm}} Kubernetes-Cluster die Version 1.11 oder höher aufweisen. Führen Sie die folgenden Anweisungen aus, um [ein Upgrade für Ihre neuen und vorhandenen Cluster](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks-updating-kubernetes) auf diese Version zu aktualisieren.
- Wenn Sie bei der Bereitstellung eines Kubernetes-Clusters in {{site.data.keyword.cloud_notm}} nicht den vorausgewählten Standarddateispeicher der Bronzestufe verwenden möchten, können Sie den gewünschten Speicher bereitstellen. Weitere Angaben hierzu finden Sie unter [Hinweise zum persistenten Speicher](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage).
- Wenn Sie sich entschließen, in Ihrem Kubernetes-Cluster {{site.data.keyword.cloud_notm}}-Unterstützung für mehrere Zonen einzubinden, müssen Sie eigenen Speicher bereitstellen. Weitere Angaben hierzu finden Sie unter [Cluster mit mehreren Zonen (MZR) für {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-mzr) verwenden.
- Sie können {{site.data.keyword.blockchainfull_notm}} Platform für einen Zeitraum von 30 Tagen kostenlos testen, indem Sie Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz mit einem kostenlosen {{site.data.keyword.cloud_notm}} Kubernetes-Cluster verbinden.  Die Leistung dieser Testversion ist jedoch in Bezug auf Durchsatz, Speicherkapazität und Funktionalität beschränkt. {{site.data.keyword.cloud_notm}} löscht Ihren Cluster nach 30 Tagen. Eine Migration von Knoten oder Daten aus einem kostenlosen Cluster auf einen gebührenpflichtigen Cluster ist hierbei nicht möglich. Während die Betatestversion von {{site.data.keyword.blockchainfull_notm}} Platform kostenlos ist, fallen bei Auswahl des gebührenpflichtigen Kubernetes-Clusters anstelle des kostenlosen Clusters Gebühren für Kubernetes Service an, die Ihrem {{site.data.keyword.cloud_notm}}-Konto belastet werden.
- VRF (Virtual Routing and Forwarding) wird nicht unterstützt. Der {site.data.keyword.blockchainfull_notm}} Platform-Service ist nicht kompatibel mit Konten, die für automatische globale Weiterleitung zwischen IP-Teilnetzblöcken aktiviert sind. Kubernetes-Cluster, die mit privaten VLANs konfiguriert sind, werden ebenfalls nicht unterstützt.

## Migration
{: #ibp-console-overview-migration}

Momentan ist keine Migration von einem der {{site.data.keyword.blockchainfull_notm}} Platform-Angebote auf {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} möglich.

Keine der {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanzen der Betatestversion kann auf das allgemein verfügbare GA-Release migriert werden.

## Lizenz und Preisstruktur
{: #ibp-console-overview-license-and-pricing}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} verfügt nun über ein neues Preismodell mit stündlicher Abrechnung, das auf der VPC-Nutzung basiert (VPC = Virtual Processor Core). Das vereinfachte Modell basiert auf dem CPU-Volumen (oder VPC-Volumen), das von Ihren {{site.data.keyword.blockchainfull_notm}} Platform-Knoten pro Stunde verbraucht wird. Hierbei gilt ein Pauschalbetrag von **0,29 USD/VPC-Stunde**, wobei **1 VPC = 1 CPU** ist. Weitere Einzelheiten hierzu finden Sie im Abschnitt zur [Preisstruktur](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing).

## Einführung
{: #ibp-console-overview-deploy}

Informationen zur Vorgehensweise bei der Bereitstellung von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} finden Sie in der [Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

Weitere Informationen zur Verwendung der Konsole, um Knoten bereitzustellen und Konsortien zu erstellen, finden Sie im Lernprogramm [Build a network](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) (Ein Netz erstellen). Dieses Lernprogramm führt Sie durch den Prozess, wie mithilfe der Konsole ein Beispielnetz mit drei Organisationen, einer Anordnungsorganisation, zwei Peerorganisationen und einem Kanal mit zwei beigetretenen Peers erstellt wird. Sie können dieses Beispielnetz verwenden, um Demos oder Machbarkeitsnachweise bereitzustellen. Sie können die Schritte im Lernprogramm anpassen und erweitern, um eine eigene angepasste Blockchain-Konfiguration zu erstellen.

## Architekturreferenz
{: #ibp-console-overview-architecture}

In der folgenden Abbildung sind die Komponenten Ihres Blockchain-Netzes dargestellt und es wird gezeigt, wie sie interagieren.
![Beispiel für die Netzstruktur](../images/IBP_V2_Architecture.svg "Architekturreferenz")

Beachten Sie, wie eine einzelne Instanz der Konsole, die auch als "Betriebstools" bezeichnet wird, für jede {{site.data.keyword.blockchainfull_notm}} Platform Service-Instanz erstellt wird. Wenn ein Peer-, Anordnungs- oder Zertifizierungsstellenknoten mit der Konsole bereitgestellt wird, wird der Knoten in der **Kubernetes-Cluster-Serviceinstanz** bereitgestellt.

| **{{site.data.keyword.blockchainfull_notm}} Platform-Kubernetes-Cluster** | **Beschreibung** |
| ------------------------- |-----------|
| Betriebstools | Wird auch als `Konsole` bezeichnet; dies ist Ihre zentrale Benutzerschnittstelle für den Betrieb all Ihrer Blockchain-Komponenten. Mit dieser Konsole können Sie jetzt Zertifizierungsstellen-, Peer- und Anordnungsknoten erstellen, Kanäle erstellen und Smart Contracts installieren und instanziieren, die mit der Erweiterung für VS Code von Hyperledger Fabric v1.4 entwickelt wurden. Die Konsole wird in einem {{site.data.keyword.IBM_notm}}-eigenen Cluster bereitgestellt. Für dieses Tool oder den Kubernetes-Cluster, in dem er ausgeführt wird, ist keine Gebühr zu berechnen.|


| **Kubernetes-Cluster-Serviceinstanz** | **Beschreibung** |
| ------------------------- |-----------|-----------|-----------|
| **Tiller** | Teil des [Helm-Tooling](https://docs.helm.sh/glossary/#tiller){: external}. Der Tiller wird innerhalb des Kubernetes-Clusters ausgeführt, um die Installationen Ihrer Helm-Diagramme für Peer, Zertifizierungsstelle und Anordnungsknoten zu verwalten. |
| **Ingress** | Ein [Kubernetes-Objekt](https://kubernetes.io/docs/concepts/services-networking/ingress/){: external}, das den Zugriff auf die Clusterressourcen von außerhalb des Clusters ermöglicht. |
| **Proxy** | Der {{site.data.keyword.blockchainfull_notm}} Platform-Proxy ist für die Weiterleitung des Datenverkehrs an die richtigen Peer-, Zertifizierungsstellen- und Anordnungsknoten unter Verwendung des Host-Header-Routings verantwortlich. |
| **Peers, Zertifizierungsstellen und Anordnungen** | Dies sind die Knoten, die durch die Bereitstellung der zugrunde liegenden Helm-Diagramme erstellt werden. Hinweis: Diese Knoten können auch aus anderen Kubernetes-Cluster-Serviceinstanzen importiert werden. Da die Schlüssel von {{site.data.keyword.IBM_notm}} niemals gespeichert werden, enthält jeder Peer- und Anordnungsknoten einen gRPC-Web-Proxy, der es der Konsole ermöglicht, mit jedem Knoten zu kommunizieren, indem die Schlüssel in der Konsolenwallet verwendet werden. |
| **RBAC** | Rollenbasierte Zugriffssteuerung.  {{site.data.keyword.blockchainfull_notm}} Platform konfiguriert [rollenbasierte Kubernetes-Zugriffssteuerung](https://kubernetes.io/docs/reference/access-authn-authz/rbac/){: external} in dem Cluster, die für die Verwaltung der Blockchain-Komponenten im Cluster erforderlich ist.

## Support anfordern
{: #ibp-console-overview-support}

Weitere Informationen, wie Sie Support für {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} anfordern und kostenlose Blockchain-Entwicklerressourcen und entsprechende Unterstützungsforen abrufen können, die Sie zum Beheben von Problemen verwenden können, finden Sie im Abschnitt [Support anfordern](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Die Unterstützung für die Betatestversion von {{site.data.keyword.blockchainfull_notm}} Platform ist für die Betaphase eingeschränkt, die am 3. August 2019 endet.
