---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

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

# Einführung in {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform bietet einen verwalteten und umfassenden Stack-Blockchain-as-a-Service (BaaS) an, mit dem Sie Blockchain-Komponenten in Umgebungen Ihrer Wahl bereitstellen können. Die Umgebung kann {{site.data.keyword.cloud_notm}}, lokal über {{site.data.keyword.cloud_notm}} Private und Drittanbieter-Clouds, wie z. B. Amazon Web Services (AWS), sein. In diesem Lernprogramm führen wir Sie durch den allgemeinen Prozess, wie Sie ein grundlegendes Blockchain-Netz mit {{site.data.keyword.blockchainfull_notm}} Platform konfigurieren.
{:shortdesc}

Lesen Sie vor der Verwendung eines {{site.data.keyword.blockchainfull_notm}} Platform-Angebots die technischen Angaben und Unterstützungsinformationen im Abschnitt zum [Haftungsausschluss](/docs/services/blockchain/needtoknow.html#disclaimer).
{: important}


## Vorbemerkungen
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform bietet unterschiedliche Angebote, die die verschiedenen Benutzertypen beim Einstieg in die Arbeit mit Blockchains unterstützen und den Benutzern helfen, ihre Anwendungen in die Produktionsphase zu bringen. Sie müssen Ihre Umgebung und das Angebot festlegen, die Sie verwenden möchten. In Abbildung 1 sind die Funktionen, Zielgruppen, Preisangaben und Cloudplattformen für verschiedene Angebote aufgelistet. Klicken Sie in der folgenden Tabelle auf das jeweilige Angebot, um weitere Informationen zum Angebot zu erhalten.

| **Angebote** | **Inhalt** | **Abrechnungsmodell** | **Cloudplattform** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**{{site.data.keyword.blockchainfull_notm}} Platform-Erweiterung für VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | Entwickler können mit der IDE beginnen, die einen Explorer und Befehle bereitstellt, die für die schnelle Entwicklung von Smart Contracts über die Befehlspalette zugänglich sind. | Kostenlos | Wird auf dem lokalen System ausgeführt |
| [**{{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) | {{site.data.keyword.blockchainfull_notm}} Platform-Konsole und APIs, die zum Bereitstellen und Verwalten von Blockchain-Komponenten in Ihrem {{site.data.keyword.cloud_notm}} Kubernetes-Cluster verwendet werden können | [VPC-Preisstruktur 0,29 USD/VPC-Stunde](/docs/services/blockchain/howto/pricing-saas.html) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private**](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) | Bereitstellbare Helm-Diagramme für Zertifizierungsstellen, Anordnungsknoten und Peers | [VPC-Preisstruktur](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about-pricing) und kostenlose Community Edition | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto/remote_peer.html#remote-peer-aws-about) | AWS-Schnelleinstiegsvorlage zum Bereitstellen von fernen Peers außerhalb {{site.data.keyword.cloud_notm}} | Kostenlos | AWS |

*Abbildung 1: {{site.data.keyword.blockchainfull_notm}} Platform-Angebote*


## Schritt 1: Holen Sie ein Angebot ein
{: #get-started-ibp-step1}

Stellen Sie sicher, dass Sie über das Cloud-Konto oder die PPA-Lizenz verfügen, um ein {{site.data.keyword.blockchainfull_notm}} Platform-Angebot zu erhalten.

* **{{site.data.keyword.blockchainfull_notm}} Platform-Erweiterung für VS Code**

  Diese VS Code-Erweiterung steht unter [Visual Studio Marketplace ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform "{{site.data.keyword.blockchainfull_notm}} Platform-Erweiterung für VS Code") kostenlos zur Verfügung und kann zur Entwicklung, zum Debugging und zum Testen von Smart Contracts für die Bereitstellung in {{site.data.keyword.blockchainfull_notm}} verwendet werden.

* **{{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}**

  Dieses Angebot steht im [{{site.data.keyword.cloud_notm}}-Katalogdashboard ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog "Katalog") von {{site.data.keyword.cloud_notm}} zur Verfügung.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private**

  Dieses Angebot wird als bereitstellbares Helm-Diagramm geliefert und hat sowohl eine gebührenpflichtige Edition als auch eine kostenlose Community-Edition. Sie können die Enterprise Edition von [Passport Advantage Online![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/software/passportadvantage/pao_customer.html) oder die kostenlose Community-Edition von [GitHub ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://github.com/IBM/charts/blob/master/repo/stable/ibm-blockchain-platform-dev-1.0.2.tgz) herunterladen.

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Dieses Angebot ist in AWS verfügbar und Sie können einen Blockchain-Peer in AWS mithilfe der [Schnelleinstiegsvorlage ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/) bereitstellen.

## Schritt 2: Stellen Sie {{site.data.keyword.blockchainfull_notm}} Platform bereit
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}**

  Melden Sie sich bei {{site.data.keyword.cloud_notm}} an und erstellen Sie eine Serviceinstanz mit dem Angebot. Folgen Sie dem Assistenten, um die Erstkonfiguration Ihres Netzes durchzuführen. Weitere Informationen hierzu finden Sie in der [Einführung zu {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private**

  Bevor Sie ein Netz bereitstellen, müssen Sie das [Helm-Diagramm in {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/helm_install_icp.html#helm-install) importieren. Anschließend können Sie [von einer Zertifizierungsstelle aus Ihre Netzkomponenten bereitstellen](/docs/services/blockchain/ibp_for_icp_deployment_guide.html#step-three-set-up-your-cas), um so Ihr Netz aufzubauen.

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Dieses Angebot ist in AWS verfügbar und Sie können einen Blockchain-Peer in AWS mithilfe der [Schnelleinstiegsvorlage ![Symbol für externen Link](images/external_link.svg "Symbol für externen Link")](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/) bereitstellen.

## Nächste Schritte
{: #get-started-ibp-next-steps}

Nachdem Sie {{site.data.keyword.blockchainfull_notm}} Platform in der Umgebung Ihrer Wahl bereitgestellt haben, können Sie das Netz mit Konsortium, Knoten, Kanälen und Smart Contracts einrichten und Transaktionen starten. Weitere Informationen finden Sie im linken Navigationsbereich in dieser Dokumentation in den Aufgabenthemen im Abschnitt **VORGEHENSWEISE**.

## Support anfordern
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} bietet verschiedene Supportoptionen für von {{site.data.keyword.IBM_notm}} bereitgestellte Blockchain-Lösungen an. Weitere Informationen zum {{site.data.keyword.blockchainfull_notm}} Platform-Support finden Sie unter [Support anfordern](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).
