---

copyright:
  years: 2019
lastupdated: "2019-05-16"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:new_window: target="_blank"}
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

- [Welche Datenbank verwenden die Peers für ihr Ledger?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [Welche Sprachen werden für Smart Contracts unterstützt?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Kann ich ein Upgrade von V1.0 auf die neue {{site.data.keyword.blockchainfull_notm}} Platform-Version durchführen? ](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [Welche Vorteile bietet die Verwendung von {{site.data.keyword.blockchainfull_notm}} Platform gegenüber dem nativen Hyperledger Fabric-Produkt?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Welche Folgen hat die Löschung meines {{site.data.keyword.blockchainfull_notm}} Platform-Service?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Welche Regionen stehen für den Blockchain-Service unter {{site.data.keyword.cloud_notm}} zur Verfügung?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Welche Hyperledger Fabric-Version wird mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Kann ich meinen bestehenden {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster verwenden?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Besteht Zugriff auf Protokollierungsservices und welche Protokolle sind für mich verfügbar?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)


## Welche Datenbank verwenden die Peers für ihr Ledger?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Alle mit {{site.data.keyword.blockchainfull_notm}} Platform bereitgestellten Peers verwenden CouchDB als Datenbank für das Ledger.

## Welche Sprachen werden für Smart Contracts unterstützt?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform unterstützt Smart Contracts, die in Go oder Node.js geschrieben sind. Das neue Hyperledger Fabric-Programmiermodell unterstützt momentan nur Node.js, die Unterstützung weiterer Programmiersprachen ist jedoch in Kürze verfügbar. Wenn Sie Ihren vorhandenen Anwendungscode beibehalten oder Fabric-SDKs für andere Sprachen als Node.js verwenden möchten, können Sie über niedrigere Versionen der Fabric-SDK-APIs weiterhin eine Verbindung zu Ihrem {{site.data.keyword.blockchainfull_notm}} Platform-Netz herstellen.

## Kann ich ein Upgrade von V1.0 auf die neue {{site.data.keyword.blockchainfull_notm}} Platform-Version durchführen?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Für den Starter Plan ist kein derartiges Upgrade möglich. Sie können für Ihr System kein Upgrade von der Starter Plan-Version auf die neue Version von {{site.data.keyword.blockchainfull_notm}} Platform durchführen. Für den Enterprise Plan wird ein Upgrade auf die neue Version von {{site.data.keyword.blockchainfull_notm}} Platform in der Zukunft jedoch möglich sein. Zur Unterstützung erhalten Sie vom {{site.data.keyword.blockchainfull_notm}} Platform-Team eine E-Mail an den Kontoeigner.

## Welche Vorteile bietet die Verwendung von {{site.data.keyword.blockchainfull_notm}} Platform gegenüber dem nativen Hyperledger Fabric-Produkt?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform ermöglicht Clients die einfache Bereitstellung eines angepassten Blockchain-Netzes. Sie können die intuitive Benutzerschnittstelle der Konsole verwenden, um Ihr Netz schnell bereitzustellen, Smart Contracts auf einfache Weise zu installieren und zu instanziieren und Ihre Transaktionen zu überwachen.

## Welche Folgen hat die Löschung meines {{site.data.keyword.blockchainfull_notm}} Platform-Service?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Wenn Sie eine {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz löschen, dann wird lediglich die Konsole gelöscht. Alle mit der Konsole erstellten Komponenten sowie der zugehörige Speicher werden hingegen nicht gelöscht. Der Kubernetes-Cluster und alle importierten Komponenten verbleiben ebenfalls auf dem System. Die Nutzung Ihrer Kubernetes-Knoten und des Speichers wird Ihnen weiterhin in Rechnung gestellt, bis Sie sie selbst über das Kubernetes-Dashboard oder mit den CLI-Befehlen löschen.

## Welche Regionen stehen für den Blockchain-Service unter {{site.data.keyword.cloud_notm}} zur Verfügung?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

Die für {{site.data.keyword.blockchainfull_notm}} Platform verfügbaren Regionen sind im Abschnitt zu den [Standorten von {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations) aufgelistet. Beachten Sie hierbei, dass Sie einen {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster in derselben Region erstellen müssen, in der sich auch der Blockchain-Service befindet, damit der Cluster erkannt werden kann. In Kürze werden weitere Regionen zur Verfügung gestellt.

## Welche Hyperledger Fabric-Version wird mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

Im Februar 2019 wurde die Betaversion mit Hyperledger Fabric 1.4.0 eingeführt.
Am 29. Mai 2019 wurde die GA-Version mit Hyperledger Fabric 1.4.1 eingeführt. Hyperledger Fabric 1.4.0 wird im GA-Produkt nicht unterstützt.

## Kann ich meinen bestehenden {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster verwenden?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Ihr bestehender Kubernetes-Cluster kann mit {{site.data.keyword.blockchainfull_notm}} Platform verwendet werden, wenn die folgenden Bedingungen erfüllt sind:
- Auf dem Cluster wird Kubernetes v1.11 oder eine höhere stabile Version des Produkts ausgeführt.
- Im Cluster sind genügend verfügbare Ressourcen vorhanden.

## Besteht Zugriff auf Protokollierungsservices und welche Protokolle sind für mich verfügbar?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Mit {{site.data.keyword.blockchainfull_notm}} Platform können Sie nun über das Kubernetes-Dashboard direkt auf Ihre Peer-, CA- und Anordnungsknotenprotokolle zugreifen. Sie sollten die Vorteile des {{site.data.keyword.cloud_notm}} LogDNA-Service nutzen, mit dem die Protokolle auf einfache Weise in Echtzeit analysiert werden können.
