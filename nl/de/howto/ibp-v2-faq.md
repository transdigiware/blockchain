---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}

# Häufig gestellte Fragen
{: #ibp-v2-faq}

**Allgemeine Fragen**   

- [Welche Vorteile bietet die Verwendung von {{site.data.keyword.blockchainfull_notm}} Platform gegenüber dem nativen Hyperledger Fabric-Produkt?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Welche Hyperledger Fabric-Version wird mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Welche Datenbank verwenden die Peers für ihr Ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [Welche Sprachen werden für Smart Contracts unterstützt?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Wird die Verwendung von Zertifikaten unterstützt, die nicht von IBM Zertifizierungsstellen (CAs) stammen?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [Kann ich ein Upgrade von V1.0 auf die neue {{site.data.keyword.blockchainfull_notm}} Platform-Version durchführen? ](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [Welche Folgen hat die Löschung meines {{site.data.keyword.blockchainfull_notm}} Platform-Service?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Welche Regionen stehen für den Blockchain-Service unter {{site.data.keyword.cloud_notm}} zur Verfügung?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Kann ich meinen bestehenden {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster verwenden?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Besteht Zugriff auf Protokollierungsservices und welche Protokolle sind für mich verfügbar?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [Welches sind die Vorteile von {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [Welche Umgebungen werden von {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud unterstützt?](#ibp-v2-faq-icp-environments)
- [Sie sieht das Preismodell für {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud aus?](#ibp-v2-faq-icp-pricing)
- [Welche Services müssen installiert werden, damit {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud verwendet werden kann?](#ibp-v2-faq-icp-services)
- [Wie kann ich die voraussichtlichen Größenanforderungen in {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud für meine Entwicklungs-, Test- und Produktionsumgebungen ermitteln?](#ibp-v2-faq-icp-sizing)
- [Was geschieht mit meinen Blockchain-Komponenten, wenn das vorhandene Helm-Release gelöscht wird?](#ibp-v2-faq-icp-delete)

## Welche Vorteile bietet die Verwendung von {{site.data.keyword.blockchainfull_notm}} Platform gegenüber dem nativen Hyperledger Fabric-Produkt?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform ermöglicht Clients die einfache Bereitstellung eines angepassten Blockchain-Netzes. Sie können die intuitive Benutzerschnittstelle der Konsole verwenden, um Ihr Netz schnell bereitzustellen, Smart Contracts auf einfache Weise zu installieren und zu instanziieren und Ihre Transaktionen zu überwachen.

## Welche Hyperledger Fabric-Version wird mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} und {{site.data.keyword.blockchainfull_notm}} Platform for Platform for Multicloud mit Hyperledger Fabric v1.4.1

## Welche Datenbank verwenden die Peers für ihr Ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Alle mit {{site.data.keyword.blockchainfull_notm}} Platform bereitgestellten Peers verwenden CouchDB als Datenbank für das Ledger.

## Welche Sprachen werden für Smart Contracts unterstützt?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform unterstützt Smart Contracts, die in Go und Node.js geschrieben sind. Das neue Hyperledger Fabric-Programmiermodell unterstützt momentan nur Node.js, die Unterstützung weiterer Programmiersprachen ist jedoch in Kürze verfügbar. Wenn Sie Ihren vorhandenen Anwendungscode beibehalten oder Fabric-SDKs für andere Sprachen als Node.js verwenden möchten, können Sie über niedrigere Versionen der Fabric-SDK-APIs weiterhin eine Verbindung zu Ihrem {{site.data.keyword.blockchainfull_notm}} Platform-Netz herstellen.

## Wird die Verwendung von Zertifikaten unterstützt, die nicht von IBM Zertifizierungsstellen (CAs) stammen?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Ja, Sie können eigene Zertifikate bereitstellen, sofern sie von einer CA stammen, die X.509-konform ist.

## Kann ich ein Upgrade von V1.0 auf die neue {{site.data.keyword.blockchainfull_notm}} Platform-Version durchführen?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Für den Starter Plan ist kein derartiges Upgrade möglich. Sie können für Ihr System kein Upgrade von der Starter Plan-Version auf die neue Version von {{site.data.keyword.blockchainfull_notm}} Platform durchführen.
Für den Enterprise Plan wird ein Upgrade auf die neue Version von {{site.data.keyword.blockchainfull_notm}} Platform in der Zukunft jedoch möglich sein. Zur Unterstützung erhalten Sie vom {{site.data.keyword.blockchainfull_notm}} Platform-Team eine E-Mail an den Kontoeigner.

## Welche Folgen hat die Löschung meines {{site.data.keyword.blockchainfull_notm}} Platform-Service?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Wenn Sie ein {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz löschen, werden alle  Blockchain-CAs, -Peers und -Anordnungsknoten sowie der zugehörige Speicher gelöscht.

## Welche Regionen stehen für den Blockchain-Service unter {{site.data.keyword.cloud_notm}} zur Verfügung?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

Die für {{site.data.keyword.blockchainfull_notm}} Platform verfügbaren Regionen sind im Abschnitt zu den [Standorten von {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations) aufgelistet. Beachten Sie hierbei, dass Sie einen {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster in derselben Region erstellen müssen, in der sich auch der Blockchain-Service befindet, damit der Cluster erkannt werden kann. In Kürze werden weitere Regionen zur Verfügung gestellt.

## Kann ich meinen bestehenden {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster verwenden?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Ihr bestehender Kubernetes-Cluster kann mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet werden, wenn die folgenden Bedingungen erfüllt sind:
- Auf dem Cluster wird Kubernetes v1.11 oder eine höhere stabile Version des Produkts ausgeführt.
- Im Cluster sind genügend verfügbare Ressourcen vorhanden.

## Besteht Zugriff auf Protokollierungsservices und welche Protokolle sind für mich verfügbar?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Mit {{site.data.keyword.blockchainfull_notm}} Platform können Sie jetzt über das Kubernetes-Dashboard direkt auf Ihre Peer-, CA- und Anordnungsknotenprotokolle zugreifen. Sie sollten die Vorteile des {{site.data.keyword.cloud_notm}} LogDNA-Service nutzen, mit dem die Protokolle auf einfache Weise in Echtzeit analysiert werden können.

## Welche Vorteile bietet {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

Weitere Angaben hierzu finden Sie im Abschnitt zu [Möglichkeiten in {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## Welche Umgebungen werden von {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud unterstützt?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud bietet Unterstützung für alle Umgebungen, die von {{site.data.keyword.cloud_notm}} Private v3.2 unterstützt werden. Weitere Informationen finden Sie im Abschnitt zu [Unterstützten Betriebssystemen und Plattformen](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external}.

## Wie sieht das Preismodell für {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud aus?
{: #ibp-v2-faq-icp-pricing}
{: faq}

Die Lizenzierung für {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) basiert auf dem für das Produkt bereitgestellten CPU-Volumen (oder VPC-Volumen). Weitere Informationen finden Sie unter [IBM kontaktieren](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Welche Services müssen installiert werden, damit {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud verwendet werden kann?
{: #ibp-v2-faq-icp-services}
{: faq}

Sie müssen lediglich {{site.data.keyword.cloud_notm}} Private v3.2 installieren.

## Wie kann ich die voraussichtlichen Größenanforderungen in {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud für meine Entwicklungs-, Test- und Produktionsumgebungen ermitteln?
{: #ibp-v2-faq-icp-sizing}
{: faq}

Sobald Sie wissen, wie viele Zertifizierungsstellen, Peers und Anordnungsknoten erforderlich sind, können Sie anhand der [Tabelle der standardmäßigen Ressourcenzuordnungen](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) eine grobe Schätzung der für Ihr Netz benötigten CPUs (VPCs) vornehmen.

## Was geschieht mit meinen Blockchain-Komponenten, wenn das vorhandene Helm-Release gelöscht wird?
{: #ibp-v2-faq-icp-delete}
{: faq}

Wenn Sie ein Helm-Release aus Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster löschen, werden die zugehörigen Blockchain-Komponenten nicht gelöscht. Um ein Helm-Release vollständig aus Ihrem Cluster zu entfernen, sollten Sie zunächst alle Ihre Komponenten über die Blockchain-Console oder die Blockchain-APIs löschen. Danach können Sie das Helm-Diagramm löschen.  
