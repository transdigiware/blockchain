---

copyright:
  years: 2019
lastupdated: "2019-06-21"

keywords: pricing model, hourly, per hour, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

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

# Preisstruktur
{: #ibp-saas-pricing}

In diesem Abschnitt finden Sie Informationen zur Preisstruktur von {{site.data.keyword.blockchainfull}} Platform on {{site.data.keyword.cloud_notm}} und zu den Kosten, die beim Entwickeln und Erweitern Ihres Blockchain-Netzes mit Peers, Anordnungsknoten und Komponenten für Zertifizierungsstellen entstehen, die auf Hyperledger Fabric v1.4.1 basieren.{:shortdesc}

_Dieses Preismodell gilt nur für {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}}. Wenn Sie mit einem Starter Plan- oder Enterprise Plan-System arbeiten und Fragen zur Preisstruktur haben, dann finden Sie weiterführende Informationen im Abschnitt zur [Preisstruktur](/docs/services/blockchain?topic=blockchain-ibp-pricing) für den Starter Plan und den Enterprise Plan._

{{site.data.keyword.blockchainfull_notm}} Platform verfügt nun über ein neues Preismodell mit stündlicher Abrechnung, das auf den VPC-Nutzungsdaten (VPC = Virtual Processor Core) basiert. Dieses vereinfachte Modell basiert auf dem CPU-Volumen (oder VPC-Volumen), das von Ihren {{site.data.keyword.blockchainfull_notm}} Platform-Knoten pro Stunde verbraucht wird. Hierbei gilt ein Pauschalbetrag von **0,29 USD/VPC-Stunde**.

Bei VPC (Virtual Processor Cores) handelt es sich um eine Maßeinheit, mit der die Lizenzgebühren für {{site.data.keyword.IBM_notm}} Produkte bestimmt werden können. Die Basis bildet hierbei die Anzahl der virtuellen Kerne (vCPUs), die dem Produkt zur Verfügung stehen. Eine vCPU (virtuelle CPU) ist ein virtueller Kern, der einer virtuellen Maschine oder einem physischen Prozessorkern zugeordnet wird. Zur Schätzung der für {{site.data.keyword.blockchainfull_notm}} Platform anfallenden Kosten wird von der folgenden Annahme ausgegangen: **1 VPC = 1 CPU = 1 vCPU = 1 Kern**.
{:note}

Für die Schätzung der Gesamtkosten sollten Sie berücksichtigen, dass Ihr Blockchain-Netz aus einem {{site.data.keyword.cloud_notm}} Kubernetes-Cluster besteht, der {{site.data.keyword.blockchainfull_notm}} Platform-Komponenten enthält und den von Ihnen ausgewählten Speicher verwendet. Ihr {{site.data.keyword.cloud_notm}} Kubernetes-Cluster und der von Ihnen ausgewählte Speicher verursachen separate Gebühren. Für den Cluster, unter dem die Instanz für die Betriebstools (auch als Konsole bezeichnet) ausgeführt wird, entstehen keine Kosten. Eine Darstellung zu diesem Thema finden Sie im Abschnitt mit der [Architekturreferenz](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview-architecture). Im Folgenden werden weitere Details zur Berechnung der Gebühren beschrieben.

Entwickler können für die ersten Schritte die kostenlose [Erweiterung für VS Code ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform "{{site.data.keyword.blockchainfull_notm}} Platform-Erweiterung für VS Code") verwenden. Benutzen Sie diese integrierte Entwicklerumgebung zum Schreiben, Testen, Debuggen und Paketieren von Smart Contracts auf dem lokalen System und zur Bereitstellung von {{site.data.keyword.blockchainfull_notm}} Platform sowie zum Schreiben von Clientanwendungen. Beginnen Sie dabei völlig neu oder greifen Sie auf die Lernprogramme und Beispiele zu, um sich mit den Blockchain-Grundlagen vertraut zu machen.

## Vorteile des neuen Preismodells
{: #ibp-saas-pricing-benefits}

- **Keine Mitgliedsgebühren**: Da keine Mitgliedsgebühren anfallen, können Sie direkt in Ihre Blockchain-Komponenten investieren.
- **Transparente Schätzung**: Durch das einfache Preismodell auf Stundenbasis ist die Kostenschätzung transparent und einfach. Verwenden Sie hierzu das Kostenschätzertool, das im {{site.data.keyword.cloud_notm}}-Dashboard zur Verfügung steht.
- **Keine Mindestgebühr erforderlich**: Bezahlen Sie nur für die Ressourcen, die Sie auch benutzt haben. Sie benötigen kein VPC-Mindestpaket auf Stundenbasis, wodurch die ersten Schritte mit dem Produkt sehr kostengünstig sind.
- **Skalierbarkeit der Rechenleistung**: Sie können die Rechenleistung in Zeitperioden mit hoher Auslastung nach oben oder bei geringer Auslastung bis auf einen Minutenbruchteil nach unten skalieren, um so die Ausgaben auf ein Minimum zu beschränken.  

Zusammenfassend bedeutet dies, dass diese Funktionen die Komplexität der Abrechnung beseitigen, die in Zusammenhang mit Einschränkungen der Mitgliedschaft oder durch den Kauf von Rechenleistung im Voraus auftreten kann.

## Schlüsselelemente für Kosten
{: #ibp-saas-pricing-elements}

Da Ihr Blockchain-Netz aus einem {{site.data.keyword.cloud_notm}} Kubernetes-Cluster besteht, der {{site.data.keyword.blockchainfull_notm}} Platform-Komponenten enthält und Speicher Ihrer Wahl verwendet, gibt jeder der folgenden Posten Ihre jeweiligen Gesamtkosten an:

- **{{site.data.keyword.blockchainfull_notm}} Platform**-Pauschalbetrag von 0,29 USD/VPC-Stunde.
- Gestaffelte Preisstruktur des Clusters für **{{site.data.keyword.cloud_notm}} Kubernetes Service**, die in {{site.data.keyword.cloud_notm}} sichtbar ist, wenn Sie Ihren gebührenpflichtigen Cluster bereitstellen. Dieser Betrag enthält die Gebühren für Ihre genutzte Rechenleistung (d. h. CPU und Hauptspeicher). Die Kosten für {{site.data.keyword.cloud_notm}} Kubernetes Service werden auf Basis eines gestaffelten Modells berechnet, das auf der Anzahl der Stunden basiert, die pro Monat genutzt werden. Aus diesem Grund sollten Sie bei der Überprüfung der Preisstrukturpläne berücksichtigen, dass eine ununterbrochene Nutzung (24 Stunden pro Tag, 7 Tage pro Woche) einer monatlichen Anzahl von 720 Stunden entspricht. In der Tabelle auf der [Katalogseite für Kubernetes Service ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://cloud.ibm.com/kubernetes/catalog/cluster "Kubernetes Service") finden Sie weiterführende Details zur Preisstruktur für Cluster.
- Wählen Sie den **Speicherplan** aus, der sich am besten für Ihre individuellen Anforderungen eignet. Im Abschnitt mit den [Grundlagen des Kubernetes-Speichers](/docs/containers?topic=containers-kube_concepts#kube_concepts) finden Sie weitere Informationen zu den Speicherklassenoptionen und den dabei anfallenden [Kosten ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/cloud/file-storage/pricing). Die {{site.data.keyword.blockchainfull_notm}} Platform-Knoten verwenden die Standardspeicherklasse für den Cluster. Wenn Sie einen Kubernetes-Cluster in {{site.data.keyword.cloud_notm}} bereitstellen, dann wird dieser Cluster mit dem [Dateispeicher der Bronzestufe](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) als Plug-in für persistenten Speicher vorkonfiguriert.

## Preisbeispiele
{: #ibp-saas-pricing-scenarios}

In der folgenden Tabelle werden zwei Beispiele für die Preisstruktur mit [standardmäßigen Ressourcenzuordnungen]( #ibp-saas-pricing-default) dargestellt, sofern keine andere Angabe gemacht wird.
- Das Szenario für das **Testnetz** eignet sich für die ersten Schritte und zum Testen von Smart Contracts.
- Das Szenario für den **Beitritt zu einem Produktionsnetz** umfasst zwei Peers zur Sicherstellung der Hochverfügbarkeit und eine Zertifizierungsstelle (CA), die für die Organisationsmitgliedschaft erforderlich ist.
   - Diese Peers können einem {{site.data.keyword.blockchainfull_notm}} Platform-Produktionsnetz beitreten, das an einem anderen Standort gehostet wird.
   - Knoten können immer auf den Mindestnutzungsstatus (0,001 CPU) reduziert werden, wenn sie momentan nicht verwendet werden, um so die [Kosten zu reduzieren](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).
   - Da dieses Szenario für eine **Produktionsumgebung** vorgesehen ist, gilt Folgendes:
     - Die standardmäßigen Rechenressourcen wurden verdoppelt, um eine höhere Kapazität bereitzustellen.
     - Zur Bereitstellung einer höheren Leistung kann die Speicherklasse [Silber ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](/docs/containers?topic=containers-file_storage#file_silver) ausgewählt werden.

| Preisoptionen** (1 VPC = 1 CPU)| **Testnetz** | **Beitritt zu einem Produktionsnetz** |
|-|------------|-----------------------------|
| **CPU-Zuordnung** |  1,85 CPU <br> Umfasst Folgendes: <br> - 1 Peer <br> - 2 CAs <br> - 1 Anordnungsknoten| 4,9 CPU <br> Umfasst Folgendes: <br> - 2 Peers (für HA) <br> **(2x Standardrechenleistung)** <br>- 1 CA <br>  |
| **Kosten pro Stunde: {{site.data.keyword.blockchainfull_notm}} Platform** | 0,54 USD <br> (1,85 CPU x 0,29 USD/VPC-Std.) | 1,42 USD <br> (4,9 CPU x 0,29 USD/VPC-Std. ) |
| **Kosten pro Stunde: {{site.data.keyword.cloud_notm}} Kubernetes-Cluster**    | 0,12 USD <br> (Rechenleistung: 2 x 4 Tier) <br> (IP-Zuordnung: 16 USD/Monat) | 0,46 USD <br> (Rechenleistung: 8 x 32 Tier) <br> (IP-Zuordnung: 16 USD/Monat) |
| **Kosten pro Stunde: Speicher** | 0,07 USD <br> 340 GB  <br> [Bronze ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/cloud/file-storage/pricing) <br>  2 IOPS/GB | 0,13 USD <br> 420 GB <br> [Silber ![Symbol für externen Link](../images/external_link.svg "Symbol für externen Link")](https://www.ibm.com/cloud/file-storage/pricing) <br> 4 IOPS/GB  |
| **Gesamtkosten pro Stunde** | **0,73 USD** | **2,01 USD**| |
** Testen Sie {{site.data.keyword.blockchainfull_notm}} Platform für einen Zeitraum von 30 Tagen kostenlos, indem Sie Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz mit einem kostenlosen {{site.data.keyword.cloud_notm}} Kubernetes-Cluster verbinden. Die Leistung dieser Testversion ist jedoch in Bezug auf Durchsatz, Speicherkapazität und Funktionalität beschränkt. {{site.data.keyword.cloud_notm}} löscht Ihren Kubernetes-Cluster nach 30 Tagen. Eine Migration von Knoten oder Daten aus einem kostenlosen Cluster auf einen gebührenpflichtigen Cluster ist hierbei nicht möglich.  

Ihre tatsächlichen Kosten können abhängig von zusätzlichen Faktoren wie beispielsweise der Transaktionsrate, der Anzahl der erforderlichen Kanäle, dem Nutzdatenvolumen der Transaktionen sowie der maximalen Anzahl gleichzeitig ausgeführter Transaktionen variieren.
{:note}

Die Anzahl der Serviceinstanzen, die bereitgestellt und einem einzelnen Kubernetes-Cluster zugeordnet werden können, unterliegen keiner Beschränkung. Sie müssen jedoch sicherstellen, dass ausreichende Ressourcen verfügbar sind, indem Sie die Nutzung von CPU, Hauptspeicher und Speichereinheiten überwachen, um Serviceunterbrechungen zu vermeiden. Die {{site.data.keyword.blockchainfull_notm}} Platform-Knoten müssen nicht in einem eigenen Cluster bereitgestellt werden. In dem Cluster, der zur Ausführung Ihrer Blockchain-Komponenten benutzt wird, können weitere {{site.data.keyword.cloud_notm}}-Services ausgeführt werden. Allerdings muss auch hierbei sichergestellt werden, dass genügend Rechen- und Speicherkapazitäten verfügbar sind, um die Anforderungen aller Serviceinstanzen zu erfüllen.

## Standardmäßige Ressourcenzuordnungen
{: #ibp-saas-pricing-default}

Die Werte in der folgenden Tabelle sind hilfreich, um die Kosten pro Stunde für Ihr angepasstes Netz auf Basis von CPU-Auslastung, Rechenleistung und Speicherbedarf zu schätzen.

| **Komponente** (alle Container) | CPU  | Hauptspeicher (GB) | Speicher (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       |  1,2          | 2,4                   | 200 (umfasst 100 GB für Peer und 100 GB für CouchDB)|
| **Zertifizierungsstelle**                         | 0,1            | 0,2                    | 20                     |
| **Anordnungsknoten**                    | 0,45           | 0,9                    | 100                    |

## Abrechnung
{: #ibp-saas-pricing-billing}

Ihre Abrechnungs- und Nutzungsinformationen für Ihr **nutzungsabhängiges** Konto stehen im {{site.data.keyword.cloud_notm}}-Dashboard auf der Kachel [Nutzung](https://cloud.ibm.com/billing/usage) zur Verfügung. Ein Messservice erstellt stündlich Snapshots der gesamten VPC-Nutzung für {{site.data.keyword.blockchainfull_notm}} Platform, sodass auf der Kachel **Nutzung** der monatlich kumulierte Nutzungsbetrag angegeben werden kann.

Wenn Sie einen neuen Knoten erstellen, dann kann die Aktualisierung der VPC-Nutzung auf der Kachel **Nutzung** des {{site.data.keyword.cloud_notm}}-Dashboards bis zu einer Stunde dauern.
{:note}

Navigieren Sie zur Option **Verwalten** oben im {{site.data.keyword.cloud_notm}}-Dashboard, klicken Sie auf **Abrechnung und Nutzung** und dann im linken Menü auf **Nutzung**. Das Kreisdiagramm im Unterabschnitt **Services** enthält eine Aufgliederung der Gesamtkosten nach den Serviceangebotstypen, die von Ihnen im aktuellen Monat genutzt und verbraucht wurden. Verwenden Sie dieses Diagramm, um sich einen Überblick darüber zu verschaffen, wie {{site.data.keyword.blockchainfull_notm}} Platform, Kubernetes Service und Ihr Speicherbedarf zu den Gesamtkosten beitragen.

<!--
![Usage Tile](../images/pricing1.png "Usage Tile")  
*Figure 1. View your Usage on the dashboard*-->

Wenn Sie nach unten blättern, dann wird eine ähnliche Aufgliederung nach **Typ** und **Kosten** in einer Listenansicht dargestellt. Neben anderen Angaben finden Sie hier die Informationen zu "Kubernetes Service", "Block Storage for VPC" oder "File Storage for VPC" und "{{site.data.keyword.blockchainfull_notm}} Platform". Klicken Sie neben diesen Elementen auf **Pläne anzeigen**, um Ihre Kosten nach Metrik aufzugliedern. Beispiel: Mit `VIRTUAL_PROCESSOR_CORE_HOURS` können Sie die Gesamtanzahl der Stunden ermitteln, die die VPCs genutzt wurden, und welche Kosten dabei entstanden sind. Verwenden Sie diese Daten, um die Gebühren zu ermitteln, die Ihnen anhand der verschiedenen Preismetriken berechnet werden.

<!--
![View Plans](../images/pricing2.png "View Plans")  
*Figure 2. Find out how much cost you're incurring on Blockchain Service, Storage and more*

![Breakdown by Metrics](../images/pricing3.png "Breakdown by Metrics")  
*Figure 3. Track how many VPC hours you're utilizing, and more*
-->

## Kosten Ihrer Knoten optimieren
{: #ibp-saas-pricing-shutdown}

Einer der Hauptvorteile des {{site.data.keyword.blockchainfull_notm}} Platform-Preismodells besteht in der Möglichkeit, Ressourcen zu reduzieren oder zu löschen, wenn sie  nicht mehr oder nicht mehr im vorherigen Umfang benötigt werden.

- **Knoten in Mindestauslastungsstatus versetzen**  
  Die CPU-Nutzung einzelner Knoten kann auf einen Wert von 0,001 CPU abgesenkt werden, um die Gebühren auf ein Minimum zu beschränken. Durch diese Aktion wird der Knoten in einen nicht funktionsbereiten Status versetzt. Wird später wieder mehr Rechenleistung benötigt, können Sie die Option für die erneute Zuordnung in der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole benutzen, um die Ressourcen wieder auf den erforderlichen Wert zu erhöhen. Weitere Informationen zur Vorgehensweise bei der erneuten Zuordnung von Ressourcen finden Sie im Abschnitt zur [Neuzuordnung von Ressourcen](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).

- **Nicht benutzten Peer löschen und neuen Peer bei Bedarf bereitstellen**  
  Da das Ledger auf dem Anordnungsknoten gespeichert wird, erhält ein neuer Peer bei seiner Bereitstellung und beim Beitritt zu einem Kanal eine Kopie des verteilten Ledgers. Der Nachteil dieses Ansatzes besteht darin, dass Sie neue Zertifikate generieren und den Peer erneut den Kanälen zuordnen müssen.

  Es wird davon abgeraten, einen CA-Knoten jemals zu löschen, da die auf diesem Knoten vorhandenen Daten nicht wiederhergestellt werden können. Ebenso sollten Sie einen Anordnungsknoten nicht löschen, wenn es sich dabei um Ihren einzigen Anordnungsknoten handelt.  
  {:important}

- **Ressourcenzuordnung auf Basis Ihrer Anforderungen überwachen und anpassen**  
  Wenn Sie Ihre Ressourcennutzung im Zeitverlauf überwachen, dann geben Ihnen die dabei gewonnenen Daten die Möglichkeit, die zugeordneten Ressourcen eines Knotens zu reduzieren und dabei gleichzeitig eine ausreichende Leistung sicherzustellen. Wenn Sie die Anweisungen zur [Neuzuordnung von Ressourcen](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources) an der Konsole befolgen, dann werden die Auswirkungen auf den VPC-Gesamtwert für den Knoten aktualisiert und die entsprechenden Daten können verwendet werden, um die überarbeiteten monatlichen Kosten zu schätzen.
