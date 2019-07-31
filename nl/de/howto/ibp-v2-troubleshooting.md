---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Fehlerbehebung
{: #ibp-v2-troubleshooting}

Es können allgemeine Probleme auftreten, wenn Sie die Konsole verwenden, um Knoten, Kanäle oder Smart Contracts zu verwalten. In vielen Fällen können Sie diese Probleme durch Ausführen weniger einfacher Schritte beheben.
{:shortdesc}

In diesem Abschnitt werden allgemeine Probleme beschrieben, die bei der Verwendung der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole auftreten können.

- [Wenn ich den Mauszeiger über den Knoten bewege, lautet der `Status nicht verfügbar`. Was bedeutet das?](#ibp-v2-troubleshooting-status-unavailable)
- [Wenn ich den Mauszeiger über den Knoten bewege, wird der `Status nicht erkannt`. Was bedeutet das?](#ibp-v2-troubleshooting-status-undetectable)
- [Warum schlagen meine Knotenoperationen fehl, nachdem ich meinen Peer oder Anordnungsservice erstellt habe?](#ibp-console-build-network-troubleshoot-entry1)
- [Warum wird der Fehler `Unable to get system channel` ausgegeben, wenn ich meinen Anordnungsservice öffne?](#ibp-troubleshoot-ordering-service)
- [Warum kann mein Peer nicht gestartet werden?](#ibp-console-build-network-troubleshoot-entry2)
- [Warum ist die Installation, die Instanziierung oder das Upgrade für meinen Smart Contract fehlgeschlagen?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Wie kann ich meine Smart-Contract-Containerprotokolle anzeigen?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Mein Kanal, meine Smart Contracts und meine Identitäten werden in der Konsole nicht mehr angezeigt. Wie kann ich diese Informationen wieder anzeigen?](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Warum wird der Fehler `Unable to authenticate with the enroll ID and secret you provided` ausgegeben, wenn ich eine neue MSP-Definition für die Organisation erstelle?](#ibp-v2-troubleshooting-create-msp)
- [Warum wird der Fehler `An error occurred when updating channel` ausgegeben, wenn ich versuche, eine Organisation zu meinem Kanal hinzuzufügen?](#ibp-v2-troubleshooting-update-channel)
- [Mein {{site.data.keyword.cloud_notm}} Kubernetes-Cluster ist abgelaufen. Was bedeutet das?](#ibp-v2-troubleshooting-cluster-expired)
- [Weshalb schlagen die Transaktionen, die ich über den VS Code übergebe, fehl?](#ibp-v2-troubleshooting-anchor-peer)
- [Warum wird ein Fehler "401" für fehlende Autorisierung ausgegeben?](#ibp-v2-troubleshooting-console-401)
- [Warum verarbeiten die Knoten, die ich in {{site.data.keyword.cloud_notm}} Private bereitgestellt habe, keine Transaktionen und bestehen die Statusprüfungen nicht?](#ibp-v2-troubleshooting-healthchecks)

## Wenn ich den Mauszeiger über den Knoten bewege, lautet der `Status nicht verfügbar`. Was bedeutet das?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

Der Knotenstatus in der Kachel für die Zertifizierungsstelle, den Peer oder den Anordnungsknoten ist grau, d. h. der Status des Knotens ist nicht verfügbar. Idealerweise sollte der Knotenstatus `Aktiv` lauten.
{: tsSymptoms}

Dieses Problem kann auftreten, wenn der Knoten neu erstellt wurde und der Bereitstellungsprozess noch nicht abgeschlossen ist. Wenn es sich bei dem Knoten um eine Zertifizierungsstelle handelt, ist er mit einiger Wahrscheinlichkeit nicht aktiv.
Handelt es sich um einen Peer- oder Anordnungsknoten, dann tritt diese Bedingung auf, wenn das für den Peer bzw. Anordnungsknoten ausgeführte Statusprüfprogramm den Knoten nicht kontaktieren kann. Die Anforderung für den Status kann mit einem Zeitlimitfehler fehlschlagen, da der Knoten nicht innerhalb eines bestimmten Zeitraums antwortet, der Knoten inaktiv oder die Netzkonnektivität inaktiv ist.
{: tsCauses}

Wenn es sich um einen neuen Knoten handelt, warten Sie noch einige Minuten, bis die Bereitstellung abgeschlossen ist. Wenn der Knoten nicht neu erstellt wurde,
[prüfen Sie die zugeordneten Knotenprotokolle](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) auf Fehler, um die Ursache zu bestimmen.
{: tsResolve}

## Wenn ich den Mauszeiger über den Knoten bewege, wird der `Status nicht erkannt`. Was bedeutet das?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

Der Knotenstatus in der Kachel für den Peer oder Anordnungsknoten ist gelb, d. h. der Status des Knotens wird nicht erkannt. Idealerweise sollte der Knotenstatus `Aktiv` lauten.
{: tsSymptoms}

Diese Bedingung tritt nur bei Peer- und Anordnungsknoten auf, die in die Konsole *importiert* wurden und auf die das Statusprüfprogramm nicht angewendet werden kann. Dieser Status tritt auf, weil beim Import des Knotens keine `operations_url` angegeben wurde. Es ist eine Operations-URL erforderlich, damit der Prüfprogramm für den Knoten ausgeführt werden kann. Der Knoten selbst ist wahrscheinlich `Aktiv`, da aber die Operations-URL nicht angegeben wurde, kann sein Status nicht ermittelt werden.
{: tsCauses}

Sie können dieses Problem mit den folgenden Schritten beheben:
 1. Klicken Sie auf die Knotenkachel, um sie zu öffnen.
 2. Klicken Sie auf das Symbol **Einstellungen**.
 3. Klicken Sie auf **Identität zuordnen**, sehen Sie sich an, welche Identität diesem Knoten zugeordnet ist, und notieren Sie die Identität. Zum Schließen dieser Anzeige klicken Sie auf **Abbrechen**.
 4. Klicken Sie auf **Exportieren**, um eine `JSON`-Datei für den Knoten zu generieren.
 5. Bearbeiten Sie die generierte `JSON`-Datei und geben Sie die `operations_url` für den Knoten ein. Der Wert von `operations_url` ist von seiner Konfiguration sowie von verschiedenen Netzeinstellungen abhängig. Der Wert muss von dem Netzadministrator zur Verfügung gestellt werden, der den Knoten bereitgestellt hat.
 6. Klicken Sie auf **Löschen**. Mit diesem Schritt wird der importierte Knoten aus der Konsole entfernt, der tatsächliche Knoten jedoch nicht gelöscht.
 7. Klicken Sie in der Registerkarte **Knoten** auf **Peer hinzufügen** oder auf **Anordnungsservice hinzufügen** und danach auf **Vorhandenen Peer importieren** oder **Vorhandenen Anordnungsservice importieren**.
 8. Klicken Sie auf **JSON hochladen** und navigieren Sie zu der soeben bearbeiteten JSON-Datei. Klicken Sie auf **Weiter**.
 9. Ordnen Sie die Identität zu, die Sie in Schritt 3 notiert haben.
 10. Klicken Sie auf **Peer hinzufügen** oder auf **Anordnungsservice hinzufügen**.

Das Statusprüfprogramm kann jetzt für den Knoten ausgeführt werden und den Knotenstatus berichten.
{: tsResolve}

## Warum schlagen meine Knotenoperationen fehl, nachdem ich meinen Peer oder Anordnungsservice erstellt habe?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

Es ist möglich, dass bei der Verwaltung eines vorhandenen Knotens ein Fehler auftritt. In diesem Fall ist es häufig hilfreich, die Knotenprotokolle zurate zu ziehen.  

Wenn Sie z. B. versuchen, eine Aktion für den Betrieb des Knotens auszuführen, kann die Aktion fehlschlagen.
{: tsSymptoms}

Nachdem Sie einen neuen Peer oder Anordnungsservice erstellt haben, kann es je nach Clusterspeicherkonfiguration einige Minuten dauern, bis die Knoten betriebsbereit sind.
{: tsCauses}

Überprüfen Sie Ihr Kubernetes-Dashboard und stellen Sie sicher, dass der Peer oder Knoten den Status `Aktiv` aufweist. Versuchen Sie anschließend erneut, die Aktion auszuführen. Treten weiterhin Probleme auf, obwohl der Knoten betriebsbereit ist, [überprüfen Sie Ihre Knotenprotokolle ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) auf Fehler.  
{: tsResolve}

## Warum wird der Fehler `Unable to get system channel` ausgegeben, wenn ich meinen Anordnungsservice öffne?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

Nachdem Sie in Ihrer {{site.data.keyword.cloud_notm}} Private-Konsole einen Anordnungsservice erstellt haben, lautet der Status `Aktiv`. Wenn Sie den Anordnungsservice öffnen, wird jedoch der folgende Fehler angezeigt:

```
Unable to get system channel. Wenn Sie eine Identität ohne Administratorberechtigung für den Anordnungsserviceknoten
zugeordnet haben, können Sie die Details des Anordnungsservice nicht anzeigen oder verwalten.
```

{: tsSymptoms}

Diese Bedingung tritt in der Blockchain-Konsole auf, die in {{site.data.keyword.cloud_notm}} Private ausgeführt wird. Unter anderen Browsern als Chrome müssen Sie ein Zertifikat akzeptieren, damit die Konsole ordnungsgemäß mit dem Knoten kommunizieren kann.
{: tsCauses}

Dieses Problem kann mit vielen Methoden behoben werden:
1. In Ihren Helm-Releaseinformationen finden Sie an der Stelle, an der die Browser-URL für Ihre Konsole angegeben wird, auch einen Hinweis, die URL aufzurufen und das Zertifikat zu akzeptieren. Rufen Sie die angegebene URL auf und akzeptieren Sie das Zertifikat. Öffnen Sie jetzt Ihren Anordnungsservice. Der Fehler sollte nicht mehr auftreten.
2. Wenn Sie einen Chrome-Browser verwenden, öffnen Sie Ihre Blockhain-Konsole in Chrome und öffnen Sie Ihren Anordnungsservice. Der Fehler tritt nicht auf. Beachten Sie Folgendes: Sie müssen Ihre Identitäten aus Ihrer Konsolenwallet aus dem Nicht-Chrome-Browser exportieren und anschließend in die Wallet auf dem Chrome-Browser importieren, damit die ordnungsgemäße Funktion erhalten bleibt.
{: tsResolve}

## Warum kann mein Peer nicht gestartet werden?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

Dieser Fehler kann unter verschiedenen Bedingungen auftreten.

Das Peerprotokoll enthält die Angabe `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`.
{: tsSymptoms}

- Dieser Fehler kann unter den folgenden Bedingungen auftreten:
  - Beim Erstellen der MSP-Definition für die Organisation des Peer- oder Anordnungsserviceknotens haben Sie eine Kombination aus Eintragungs-ID und geheimem Schlüssel angegeben, die einer Identität des Typs `peer` und nicht des Typs `client` entspricht. Die Identität muss den Typ `client` aufweisen.
  - Beim Erstellen der MSP-Definition für die Organisation des Peer- oder Anordnungsserviceknotens haben Sie eine Kombination aus Eintragungs-ID und geheimem Schlüssel angegeben, die nicht mit der Eintragungs-ID oder dem geheimem Schlüssel der entsprechenden Organisationsadministratoridentität übereinstimmt.
  - Beim Erstellen des Peers oder Anordnungsservice haben Sie die Eintragungs-ID und den geheimen Schlüssel einer Identität angegeben, die nicht den Typ "peer" aufweist.

- Öffnen Sie den CA-Knoten für Ihren Peer oder Anordnungsservice und zeigen Sie die registrierten Identitäten an, die in der Tabelle **Registrierte Benutzer** aufgelistet werden.
- Löschen Sie den Peer oder Anordnungsservice und erstellen Sie ihn erneut. Achten Sie dabei sorgfältig darauf, die Eintragungs-ID und den geheimen Schlüssel korrekt anzugeben.
- Beachten Sie Folgendes: Vor dem Erstellen des Peers oder Anordnungsservice müssen Sie eine Organisationsadministratoridentität des Typs "client" erstellen. Stellen Sie sicher, dass beim Erstellen der MSP-Definition für die Organisation dieselbe ID als Eintragungs-ID angegeben wird. Weitere Informationen enthalten die Anweisungen in den Abschnitten [Peeridentitäten registrieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) und [Identitäten für Anordnungsknoten registrieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## Warum ist die Installation, die Instanziierung oder das Upgrade für meinen Smart Contract fehlgeschlagen?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

Es ist möglich, dass bei der Installation, Instanziierung oder beim Upgrade eines Smart Contract ein Fehler auftritt.  Beispielsweise kann beim Installieren eines Smart Contract auf einem Peer der Fehler `An error occurred when installing smart contract on peer.` ausgegeben werden.
{: tsSymptoms}

Dieser Fehler kann auftreten, wenn die betreffende Smart-Contract-Version auf dem Peer bereits vorhanden ist, oder wenn die verfügbaren Ressourcen auf dem Peer ausgeschöpft sind.
{: tsCauses}

- Öffnen Sie das Kubernetes-Dashboard und stellen Sie sicher, dass der Peer den Status `Aktiv` aufweist.  
- Öffnen Sie den Peerknoten, stellen Sie sicher, dass die Smart-Contract-Version auf dem Peer noch nicht vorhanden ist, und wiederholen Sie den Vorgang mit der geeigneten Version.
- Treten weiterhin Probleme auf, obwohl der Knoten betriebsbereit ist, [überprüfen Sie Ihre Knotenprotokolle ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) auf Fehler.  
{: tsResolve}

## Wie kann ich meine Smart-Contract-Containerprotokolle anzeigen?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

Es kann vorkommen, dass Sie die Containerprotokolle für Smart Contract oder Chaincode aufrufen müssen, um ein Problem mit Smart Contract zu beheben.
{: tsSymptoms}

Mit den folgenden Schritten können Sie die Smart Contract-Containerprotokolle anzeigen:
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs)
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs)
{: tsResolve}

## Mein Kanal, meine Smart Contracts und meine Identitäten werden in der Konsole nicht mehr angezeigt. Wie kann ich diese Informationen wieder anzeigen?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

Ihre Identitäten für Konsolenwallets umfassen jeweils ein Signierzertifikat und einen privaten Schlüssel zum Verwalten Ihrer Blockchain-Komponenten, die jedoch nur im lokalen Speicher Ihres Browsers gespeichert werden. Sie sind für den Schutz und die Verwaltung dieser Identitäten verantwortlich. Es wird empfohlen, sie nach dem Erstellen in Ihr Dateisystem zu exportieren. Bei jedem Erstellen eines neuen Knotens ordnen Sie dem Knoten eine Identität aus Ihrer Konsolenwallet zu. Diese Administratoridentität ermöglicht Ihnen, den Knoten zu verwalten. Wenn Sie den Browser wechseln oder einen Browser auf einer anderen Maschine verwenden, sind diese Identitäten nicht mehr in Ihrer Wallet enthalten. Dies bedeutet, dass Sie die Komponenten nicht verwalten können.
{: tsSymptoms}

Eine der Neuerungen in {{site.data.keyword.blockchainfull_notm}} Platform besteht darin, dass Sie jetzt für den Schutz und die Verwaltung Ihrer Zertifikate selbst verantwortlich sind. Aus diesem Grund werden die Zertifikate zum Verwalten der Komponente nur im lokalen Speicher Ihres Browsers als persistent definiert. Wenn Sie ein privates Browserfenster verwenden und dann zu einem anderen Browser oder einem nicht privaten Browserfenster wechseln, werden die von Ihnen erstellten Identitäten in der neuen Browsersitzung aus der Konsolenwallet entfernt. Aus diesem Grund ist es erforderlich, dass Sie die Identitäten aus der Konsolenwallet in Ihrer privaten Browsersitzung in Ihr Dateisystem exportieren. Bei Bedarf können Sie sie dann in Ihre nicht private Browsersitzung importieren. Sonst gibt es keine Möglichkeit, sie wiederherzustellen.
{: tsCauses}

- Jedes Mal, wenn Sie eine neue MSP-Definition für eine Organisation erstellen, generieren Sie Schlüssel für eine Identität, die zum Verwalten der Organisation berechtigt ist. Daher müssen Sie während dieses Prozesses auf die Schaltflächen **Generieren** und anschließend auf **Exportieren** klicken, um die generierte Identität in Ihrer Konsolenwallet zu speichern und anschließend als JSON-Datei in Ihrem Dateisystem zu sichern.
- Um dieses Problem in Ihrem Browser zu beheben, müssen Sie die betreffenden Identitäten importieren und dem entsprechenden Knoten zuordnen. Gehen Sie dazu wie folgt vor:
  - Klicken Sie in dem Browser, in dem das Problem auftritt, auf die Registerkarte **Wallet** und anschließend auf **Identität hinzufügen**, um die JSON-Datei in Ihre Wallet zu importieren.
  - Klicken Sie auf **JSON hochladen** und navigieren Sie mithilfe der Schaltfläche **Dateien hinzufügen** zu der JSON-Datei, die Sie exportiert haben.
  - Klicken Sie auf **Übergeben**.
  - Öffnen Sie nun den Peer- oder Anordnungsserviceknoten, dem diese Identität ursprünglich zugeordnet wurde, und klicken Sie auf das Symbol **Einstellungen**.
  - Klicken Sie auf die Schaltfläche **Identität zuordnen**.
  - Wählen Sie in der Dropdown-Liste die Identität aus, die Sie gerade in Ihre Konsolenwallet importiert haben.
  - Klicken Sie auf **Zuordnen**.
- Wiederholen Sie diesen Vorgang für jede Identität, die in der Wallet des ursprünglichen Browsers enthalten war.
{: tsResolve}

## Warum wird der Fehler `Unable to authenticate with the enroll ID and secret you provided` ausgegeben, wenn ich eine neue MSP-Definition für eine Organisation erstelle?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

Wenn Sie versuchen, auf der Registerkarte "Organisationen" eine neue MSP-Definition für eine Organisation zu erstellen, wird der Fehler `Unable to authenticate with the enroll ID and secret you provided` ausgegeben.
{: tsSymptoms}

Dieser Fehler tritt auf, wenn der Wert, den Sie als geheimen Schlüssel angegeben haben, nicht für die Eintragungs-ID gilt, die Sie im Abschnitt `Administratorzertifikat Ihrer Organisation generieren` der Anzeige angegeben haben.
{: tsCauses}

Stellen Sie sicher, dass Sie in der Dropdown-Liste für die Eintragungs-ID die richtige Administratoreintragungs-ID der Organisations ausgewählt haben und geben Sie den richtigen Wert für den zugehörigen geheimen Schlüssel ein.
{: tsResolve}

## Warum wird der Fehler `An error occurred when updating channel` ausgegeben, wenn ich versuche, eine Organisation zu meinem Kanal hinzuzufügen?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

Wenn Sie versuchen, eine andere Organisation zu einem Kanal hinzuzufügen, schlägt die Aktualisierung mit der Nachricht `An error occurred when updating channel` fehl.
{: tsSymptoms}

Dieser Fehler tritt auf, wenn die ausgewählte **MSP-ID des Kanalaktualisierungsberechtigten** in der Anzeige **Kanal aktualisieren** kein Administrator des Kanals ist.
{: tsCauses}

Blättern Sie in der Anzeige **Kanal aktualisieren** abwärts zur **MSP-ID des Kanalaktualisierungsberechtigten** und wählen Sie die MSP-ID aus, die bei der Erstellung des Kanals angegeben wurde, oder geben Sie die MSP-ID des Kanaladministrators an.
{: tsResolve}

## Mein {{site.data.keyword.cloud_notm}} Kubernetes-Cluster ist abgelaufen. Was bedeutet das?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

Ich habe eine E-Mail erhalten, dass die Gültigkeit meines {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster demnächst abläuft oder sein Status `Abgelaufen` lautet. Oder Sie können nach 30 Tagen nicht mehr auf die Konsole zugreifen.
{: tsSymptoms}

Kostenlose Kubernetes-Cluster sind nur 30 Tage lang gültig.
{: tsCauses}

Es ist nicht möglich, von einem kostenlosen Cluster auf einen gebührenpflichtigen Cluster zu migrieren. Nach 30 Tagen können Sie nicht mehr auf die Konsole zugreifen und alle Ihre Knoten und Zertifikate werden gelöscht. Informationen zu diesen Vorgängen und was Sie unternehmen können, finden Sie im Thema zum [Ablauf des Kubernetes-Clusters](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration).
{: tsResolve}

## Weshalb schlagen die Transaktionen, die ich über den VS Code übergebe, fehl?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Wenn Transaktionen fehlschlagen, die über den VS Code übergeben wurden, wird die folgende Fehlernachricht ausgegeben:
```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```
{: tsSymptoms}

Dieser Fehler tritt auf, wenn Sie die Funktion zur Fabric-Service-Erkennung verwenden, aber keine Ankerpeers in Ihrem Kanal konfiguriert haben.
{: tsCauses}

Weitere Informationen zum Konfigurieren von Ankerpeers finden Sie in Schritt 3 des Abschnitts zu [privaten Daten](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) im Lernprogramm zur Bereitstellung von Smart Contracts.

## Warum wird ein Fehler "401" für fehlende Autorisierung ausgegeben?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

Beim Anmelden an meiner Konsole kann ich die Konsole nicht in meinem Browser aufrufen. In den Browserprotokollen finde ich den Fehler "401" für fehlende Autorisierung.
{: tsSymptoms}

Die Konsolensitzung in Ihrem Browser läuft nach **8 Stunden** Inaktivität ab. Wenn eine  Sitzung inaktiv wird, kann der inaktive Benutzer keine weiteren Aktionen in der Konsole ausführen.
{: tsCauses}

Wenn Ihre Sitzung inaktiviert wurde, können Sie versuchen, die Browseranzeige zu aktualisieren. Wenn dies nicht funktioniert, schließen Sie den Browser einschließlich **aller** geöffneten Registerkarten und Fenster. Öffnen Sie die URL erneut. Melden Sie sich neu an.

Als bewährtes Verfahren sollten Sie Ihre Zertifikate und Identitäten bereits in Ihrem Dateisystem gespeichert haben. Wenn Sie ein Fenster für anonymen Zugriff verwenden, werden beim Schließen des Browsers alle Zertifikate aus dem lokalen Speicher des Browsers gelöscht. Nachdem Sie sich erneut angemeldet haben, müssen Sie Ihre Identitäten und Zertifikate wieder importieren.
{: note}

## Warum verarbeiten die Knoten, die ich in {{site.data.keyword.cloud_notm}} Private bereitgestellt habe, keine Transaktionen und bestehen die Statusprüfung nicht?
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

Meine Konsole gibt an, dass meine Peers und Anordnungsknoten weiterhin aktiv sind. Meine Transaktionen schlagen jedoch fehl. Beim Ausführen einer Aktivitäts- oder Bereitschaftsprüfung für die Knotenpods, wird kein ordnungsgemäßer Zustand für die Pods gemeldet.
{: tsSymptoms}

Nach zahlreichen Bereitstellungs-, Entfernungs- und Upgradeaktionen für Knoten in Ihrem Cluster (z. B. im Rahmen von Tests) schlägt Docker möglicherweise aufgrund eines bekannten Problems in {{site.data.keyword.cloud_notm}} Private fehl. Weitere Informationen enthalten die Angaben zu diesem Problem in der [{{site.data.keyword.cloud_notm}} Private-Dokumentation](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}.
{: tsCauses}

Um das Problem zu umgehen, entfernen Sie die fehlgeschlagenen Pods und stellen Sie Ihre Knoten erneut bereit. Sie können auch den Dockerservice im Cluster erneut starten.
