---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Einführung in {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform kann für öffentliche und private Clouds wie {{site.data.keyword.cloud_notm}}, Ihr Rechenzentrum und öffentliche Clouds anderer Anbeiter übergreifend bereitgestellt werden. Diese Bereitstellung für mehrere Clouds wird durch {{site.data.keyword.cloud_notm}} Private, die auf Kubernetes basierende {{site.data.keyword.IBM_notm}}-Plattform für Containerorchestrierung ermöglicht. Sie können dieses Angebot verwenden, um die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole in einer Bereitstellung von {{site.data.keyword.cloud_notm}} Private zu installieren und anschließend mithilfe der Konsole {{site.data.keyword.blockchainfull_notm}}-Komponenten in Ihrem lokalen Cluster zu erstellen. Außerdem können Sie mit der Konsole ein verteiltes Netz mit mehreren Clouds betreiben, indem Sie Knoten importieren, die in anderen {{site.data.keyword.cloud_notm}} Private-Clustern oder in {{site.data.keyword.cloud_notm}} bereitgestellt wurden.
{:shortdesc}

**Zielgruppe**: Dieser Abschnitt richtet sich an Systemadministratoren, die für die Bereitstellung von {{site.data.keyword.blockchainfull_notm}} Platform unter {{site.data.keyword.cloud_notm}} Private verantwortlich sind.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private ist ein Produktpaket für {{site.data.keyword.cloud_notm}} Private. Weitere Informationen zu {{site.data.keyword.cloud_notm}} Private finden Sie in der Dokumentation für [{{site.data.keyword.cloud_notm}} Private Version 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Schritt 1: Einen Kubernetes-Cluster unter {{site.data.keyword.cloud_notm}} Private einrichten
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

Der erste Schritt besteht darin, einen Kubernetes-Cluster unter {{site.data.keyword.cloud_notm}} Private auf der Cloudplattform Ihrer Wahl einzurichten.
Empfehlungen und eine Anleitung finden Sie in [{{site.data.keyword.cloud_notm}} Private einrichten](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

Wenn Sie ein Netz für die Verwendung in der Produktion erstellen, müssen Sie die Clusterbereitstellung und Ihr Blockchain-Netz für hohe Verfügbarkeit einrichten.

- Empfehlungen zum Konfigurieren Ihres Clusters finden Sie im Abschnitt zum [Implementieren der Hochverfügbarkeit in {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- Wenn Sie mit der Erstellung Ihres Netzes beginnen möchten, lesen Sie die Abschnitte mit [Hinweisen zur Hochverfügbarkeit für Peers](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-peers) und mit [Hinweisen zur Hochverfügbarkeit für Anordnungsservices](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-ordering-service).

## Schritt 2: {{site.data.keyword.blockchainfull_notm}} Platform installieren
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private wird als Helm-Diagramm bereitgestellt, das von Passport Advantage (PPA) heruntergeladen werden kann. Anweisungen zum Installieren des Helm-Diagramms in Ihrem lokalen Cluster finden Sie unter [{{site.data.keyword.blockchainfull_notm}} Platform für {{site.data.keyword.cloud_notm}} Private installieren ](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install).

## Schritt 3: {{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitstellen
{: #get-started-console-icp-step-three-deploy-console}

Nach dem Installieren des Helm-Diagramms können Sie auf die Kachel der {{site.data.keyword.blockchainfull_notm}} Platform-Anwendung auf der Seite "Katalog" klicken um eine {{site.data.keyword.blockchainfull_notm}} Platform-Konsole in Ihrem lokalen Cluster zu installieren. Informationen zum Konfigurieren der Konsole und der erforderlichen Ressourcen für Ihre Bockchain-Komponenten finden Sie in [{{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitstellen](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp).

## Schritt 4: Als Administrator Benutzer zur Konsole hinzufügen
{: #get-started-console-icp-step-four-add-console-admin}

Der Konsolenadministrator kann sich mit der E-Mail-Adresse und dem zugehörigen Kennwort, die während der Bereitstellung angegeben wurden, an der Konsole anmelden. Das dort angegebene Kennwort wird zum Standardkennwort für die Konsole und wird von allen neuen Benutzern für die erste Anmeldung an der Konsole verwendet. Nach dem Anmelden kann der Administrator neue Benutzer für die Konsole hinzufügen, die sich ebenfalls anmelden und mit {{site.data.keyword.blockchainfull_notm}}-Knoten arbeiten können. Außerdem kann der Administrator ein neues Standardkennwort festlegen. Weitere Informationen hierzu finden Sie im Abschnitt [Benutzer mit der Konsole verwalten](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users).

## Schritt 5: Komponenten mit der Konsole erstellen
{: #get-started-console-icp-build-network}

Nach dem Bereitstellen der Konsole können Sie mit der Konsole {{site.data.keyword.blockchainfull_notm}}-Komponenten in Ihrem lokalen Cluster erstellen, steuern und verwalten. Eine Einführung in die Verwendung der Benutzerschnittstelle der Konsole finden Sie im [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network).

## Schritt 6: Netze cloudübergreifend verbinden
{: #get-started-console-icp-import-nodes}

Mit der Konsole können Sie Komponenten betreiben, die in anderen {{site.data.keyword.cloud_notm}} Private-Clustern oder in {{site.data.keyword.cloud_notm}} ausgeführt werden. Zunächst müssen Sie die Komponenteninformationen aus der Konsole, in der die Komponente ursprünglich bereitgestellt wurde, in eine JSON-Datei exportieren. Anschließend können Sie die JSON-Knotendatei in die Konsole importieren, die in Ihrem lokalen Cluster bereitgestellt ist, und die Komponenten cloudübergreifend verwalten. Weitere Informationen finden Sie unter [Knoten importieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes).
