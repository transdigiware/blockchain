---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform console, administer a console, add users, remove users, modify a user's role, install patches, Kubernetes cluster expiration

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
{:gif: data-image-type='gif'}


# Konsole verwalten
{: #ibp-console-manage-console}

Es gibt verschiedene Aktionen, die Sie ausführen können, um das Konsolenverhalten zu verwalten. In diesem Abschnitt werden Aktionen außerhalb der Blockchain-Knotenoperationen beschrieben, die Sie bei der täglichen Verwendung der Konsole unterstützen.
{:shortdesc}

**Zielgruppe:** Dieser Abschnitt richtet sich an Netzoperatoren, die für die Erstellung, Überwachung und Verwaltung des Blockchain-Netzes verantwortlich sind.

## Benutzer in der Konsole hinzufügen und entfernen
{: #ibp-console-manage-console-add-remove}

Jedem Benutzer mit Zugriff auf die Konsole muss eine Zugriffsrichtlinie mit einer definierten {{site.data.keyword.cloud}} Identity and Access Management (IAM)-Benutzerrolle zugewiesen sein. Die Richtlinie bestimmt, welche Aktionen der Benutzer innerhalb der Konsole ausführen kann. Die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole wird mit der E-Mail-Adresse des {{site.data.keyword.cloud_notm}}-Eigners als Konsolenadministrator bereitgestellt.  Diesem {{site.data.keyword.cloud_notm}}-Benutzer wird standardmäßig die Rolle **Manager** für den {{site.data.keyword.blockchainfull_notm}} Platform-Service in IAM zugeordnet. Der Konsolenadministrator kann daraufhin anderen Benutzern über die IAM-Benutzerschnittstelle den Zugriff auf die Konsole gewähren. Weitere Informationen zu IAM finden Sie unter [Was ist IAM?](/docs/iam?topic=iam-iamoverview#iamoverview){: external}.  

Wenn Sie [mit IAM Benutzer einladen](/docs/iam?topic=iam-iamuserinv#iamuserinv){: external} müssen Sie die folgenden Schritte ausführen, um ihre Rollen und ihren Zugriff auf die Konsole zu konfigurieren:
 1. Klicken Sie in der Menüleiste auf **verwalten** > **Zugriff (IAM)** und wählen Sie dann **Benutzer** aus.
 2. Klicken Sie auf **Benutzer einladen**.
 3. Geben Sie die E-Mail-Adresse des oder der Benutzer ein.
 4. Wählen Sie in der Dropdown-Liste **Services** die Option **Blockchain Platform** aus.
 5. Blättern Sie abwärts bis zu **Rollen auswählen**.
 6. Wählen Sie unter **Zugriffsrollen für Service zuweisen** eine Rolle für den Benutzer aus. Dies kann die Rolle **Manager**, **Schreibberechtigter** oder **Leseberechtigter** sein.
 7. Klicken Sie auf **Benutzer einladen**.

| Rolle | Funktionen |
|--------|----------|
| Manager | Als Manager verfügen Sie über Berechtigungen, die über die Rolle "Schreibberechtigter" hinausgehen. Sie können alles tun, was ein Leseberechtigter und ein Schreibberechtigter tun können, sowie: <ul><li>Neue Komponenten über die Konsole oder die APIs bereitstellen.</li><li>Bereitgestellte Komponenten über die Konsole oder die APIs löschen.</li><li>Protokollierungsstufen der Konsole über die Konsole oder die APIs ändern.</li><li>Konsole über eine API neu starten.</li></ul> |
| Schreibberechtigter | Als Schreibberechtigter verfügen Sie über Berechtigungen, die über die Rolle "Leseberechtigter" hinausgehen, darunter: <ul><li>Komponenten über die Konsole oder die APIs importieren.</li><li>Importierte Komponenten über die Konsole oder die APIs entfernen.</li><li>Benutzer für eine Zertifizierungsstelle registrieren.</li><li> Benachrichtigungen über die Konsole oder die APIs hinzufügen oder entfernen.</li></ul>  |
| Leseberechtigter | Als Leseberechtigter können Sie schreibgeschützte Aktionen ausführen, darunter: <ul><li>Die Konsolen-Benutzerschnittstelle anzeigen.</li><li>Das Konsolenprotokoll anzeigen.</li><li>Komponenten exportieren.</li><li>Gewünschte GET-API ausgeben.</li></ul> | |

 Berechtigungen sind kumulativ. Wenn Sie eine **Manager**-Rolle auswählen, kann der Benutzer auch alle Aktionen des **Leseberechtigten** und **Schreibberechtigten** ausführen. Diese Rollen müssen nicht zusätzlich aktiviert werden.   Ebenso kann ein Benutzer mit der Rolle `Schreibberechtigter` alle Aktionen der Rolle des **Leseberechtigten** ausführen. Für den Konsolenzugriff müssen Sie lediglich unter **Zugriffsrollen für Service** eine Rolle auswählen. Unter **Zugriffsrollen für Plattform** muss keine Auswahl getroffen werden. Überprüfen Sie die entsprechende Rolle unter **Zugriffsrollen für Plattform zuweisen**, wenn es von Bedeutung ist, dass die Serviceinstanz im {{site.data.keyword.cloud_notm}}-Dashboard des eingeladenen Benutzers sichtbar ist.

![Benutzer hinzufügen](../images/AddICPUser.gif){: gif}


Nachdem Sie der Konsole neue Benutzer hinzugefügt haben, kann es sein, dass Benutzer nicht alle Knoten, Kanäle oder Chaincodes, die von anderen Benutzern bereitgestellt werden, anzeigen können. Um mit diesen Komponenten arbeiten zu können, müssen die einzelnen Benutzer die zugehörigen Identitäten in ihr eigenes Konsolenwalletimportieren. Weitere Informationen hierzu finden Sie unter [Identitäten in der Konsolenwalletspeichern](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

Gehen Sie wie folgt vor, wenn Sie die Rolle eines Benutzers ändern müssen:
 1. Klicken Sie in der Menüleiste auf **verwalten** > **Zugriff (IAM)** und wählen Sie dann **Benutzer** aus.
 2. Klicken Sie neben dem Benutzer, den Sie ändern möchten, auf das Aktionsmenü und wählen Sie **Zugriff zuweisen** aus.
 3. Wählen Sie die Kachel **Zugriff auf Ressourcen zuweisen** aus.
 4. Wählen Sie aus der Dropdown-Liste **Services** die Option **Blockchain Platform 2.0**.
 5. Blättern Sie abwärts bis zu **Rollen auswählen**.
 6. Wählen Sie unter **Zugriffsrollen für Service zuweisen** eine Rolle für den Benutzer aus. Dies kann die Rolle **Manager**, **Schreibberechtigter** oder **Leseberechtigter** sein.
 7. Klicken Sie auf **Zuweisen**.

Wenn Sie den Zugriff eines Benutzers auf die Konsole entfernen müssen, befolgen Sie die Anweisungen im [IAM-Thema "Benutzer entfernen"](/docs/iam?topic=iam-remove#remove){: external}.

### Zugriffsrollen für Einzelpersonen oder Benutzergruppen in IAM zuweisen
{: #ibp-console-manage-console-users-groups}

Wenn Sie {{site.data.keyword.cloud_notm}}-IAM-Richtlinien festlegen, können Sie Rollen zu einem einzelnen Benutzer oder einer Benutzergruppe zuweisen.

<dl>
<dt>Einzelne Benutzer</dt>
<dd>Möglicherweise gibt es einen bestimmten Benutzer, der mehr oder weniger Berechtigungen benötigt als der Rest Ihres Teams. Sie können die Berechtigungen individuell anpassen, sodass jede Person über die Berechtigungen verfügt, die sie zum Ausführen ihrer Tasks benötigt. Sie können jedem Benutzer mehr als eine {{site.data.keyword.cloud_notm}} IAM-Rolle zuordnen.</dd>
<dt>Mehrere Benutzer in einer Zugriffsgruppe</dt>
<dd>Sie können eine Gruppe von Benutzern erstellen und dann dieser Gruppe Berechtigungen zuordnen. Sie können z. B. alle Teamleiter gruppieren und Administratorzugriff für die Gruppe erteilen. Anschließend können Sie alle Entwickler gruppieren und dieser Gruppe nur Schreibzugriff erteilen. Sie können jeder Zugriffsgruppe mehr als eine {{site.data.keyword.cloud_notm}}-IAM-Rolle zuordnen. Wenn Sie einer Gruppe Berechtigungen zuordnen, wirkt sich dies auf jeden Benutzer aus, der dieser Gruppe hinzugefügt oder daraus entfernt wird. Wenn Sie einen Benutzer zu der Gruppe hinzufügen, hat er auch den zusätzlichen Zugriff. Wird er entfernt, so wird ihm der Zugriff entzogen.</dd>
</dl>

Bei den in IAM hinzugefügten Benutzern handelt es sich lediglich um E-Mail-Adressen von Benutzern, die sich an der Konsole anmelden können. Sie stehen in keiner Beziehung zu den **verfügbaren Organisationen** auf der Registerkarte "Organisationen" oder zu den Identitäten, die von der Konsolenwallet gespeichert werden.
{:note}

## E-Mail-Adresse des Administrators aktualisieren

Wird die Konsole von mehreren Personen oder Organisationen verwendet, so wird empfohlen, eine funktionale E-Mail-Adresse für den Zugriff auf das Netz zu erstellen. Die Verwendung einer funktionalen E-Mail ermöglicht es Ihnen, weiterhin auf die Konsole zuzugreifen, auch wenn der ursprüngliche Administrator Ihre Organisation verlässt oder seine E-Mail-Adresse ausgesetzt wird. Es kann nur eine einzige E-Mail-Adresse als Administratorkontakt verwendet werden.

Zur Aktualisierung der E-Mail-Adresse des Konsolenadministrators, die bei der Bereitstellung der Konsole konfiguriert wurde, müssen Sie als Konsolenadministrator angemeldet sein. Navigieren Sie zur Registerkarte **Benutzer** und klicken Sie in der **{{site.data.keyword.IBM_notm}} ID**-Kachel auf **Konfigurieren**. Geben Sie in der Anzeige, die geöffnet wird, eine neue E-Mail-Adresse für den Konsolenadministrator an.

## Protokolle anzeigen
{: #ibp-console-manage-logs}

Beim Arbeiten mit der {{site.data.keyword.blockchainfull_notm}} Platform-Konsole kann es erforderlich werden, Protokolle anzuzeigen, um ein Problem zu beheben.

### Konsolenprotokolle anzeigen
{: #ibp-console-manage-console-logs}

Sie können problemlos auf die Konsolenprotokolle zugreifen, wenn Sie Fehler beheben müssen, die bei der Verwendung der Konsole und der Knoten aufgetreten sind. Sie können die Anzahl der Protokolle, die von der Konsole erfasst werden, auch erhöhen oder verringern, indem Sie die Protokollierungsstufe entsprechend festlegen. Die Protokolle der Konsole werden getrennt von den [Knotenprotokollen](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) erfasst, die von {{site.data.keyword.cloud_notm}} Kubernetes Service gesammelt werden.

Navigieren Sie zu der Registerkarte **Einstellungen** im Konsolenbrowser, um die Protokollierungseinstellungen zu ändern. Die Konsolenprotokolle werden aus zwei separaten Quellen erfasst:

  * **Clientprotokollierung:** Diese Protokolle werden erfasst, wenn Befehle von Ihrem Browser an die Konsole gesendet werden.
  * **Serverprotokollierung:** Diese Protokolle werden erfasst, wenn die Konsole Befehle an Ihre Knoten und von der Konsolenimplementierung sendet. Zu diesen Protokollen gehört die Protokollausgabe von Hyperledger Fabric.

Legen Sie die Menge der zu erfassenden Protokolle in der Dropdown-Liste unter den einzelnen Protokolltypen fest. Beispiel: **Fehler** und **Warnung** erfassen die wenigsten Protokolle, während mit **Debug** und **Alle** die meisten Protokolle erfasst werden.

Sie können die Konsolenprotokolle nur anzeigen, wenn Sie als Konsolenadministrator angemeldet sind. Wenn Sie die Protokolle auf der Registerkarte **Einstellungen** anzeigen möchten, ersetzen Sie das Wort `settings` in der Browser-URL durch `api/v1/logs`. Wenn Ihre Konsolen-URL z. B. `localhost:3001/settings` lautet, können Sie Ihre Protokolle anzeigen, indem Sie zu `localhost:3001/api/v1/logs` navigieren. Client- und Serverprotokolle werden in separaten Dateien erfasst. Die neuesten Protokolle befinden sich oben auf der Seite.

### Konsolenprotokolle anzeigen
{: #ibp-console-manage-console-node-logs}

Die Protokolle Ihrer Peers, Anordnungsknoten und Zertifizierungsstellen werden von {{site.data.keyword.IBM_notm}} Kubernetes Service erfasst. Führen Sie die folgenden Schritte aus, um die Protokolle Ihrer Knoten aus dem Cluster anzuzeigen, in dem Sie Ihr {{site.data.keyword.blockchainfull_notm}} Platform-Netz bereitgestellt haben.

Zum einfacheren Lokalisieren Ihrer Knotenprotokolle wird empfohlen, nach dem Namensbereich zu filtern, der beim Bereitstellen der Knoten verwendet wurde. Um den Namensbereich zu ermitteln, öffnen Sie einen beliebigen CA-Knoten in Ihrer Konsole und klicken Sie auf das Symbol **Einstellungen**. Zeigen Sie den Wert der **Endpunkt-URL der Zertifizierungsstelle** an. Beispiel: `https://n2734d0-paorg10524.ibpv2-cluster.us-south.containers.appdomain.cloud:7054`.

Der Namensbereich ist der erste Teil der URL, der mit dem Buchstaben `n` beginnt. Danach folgt eine zufällige Zeichenfolge aus sechs alphanumerischen Zeichen. Im Beispiel oben lautet der Wert für den Namensbereich `n2734d0`.

1. Öffnen Sie das [{{site.data.keyword.cloud_notm}}-Dashboard](https://cloud.ibm.com/resources){: external} und navigieren Sie zur Übersichtsanzeige für Ihren {{site.data.keyword.IBM_notm}} Kubernetes Service-Cluster.
2. Klicken Sie oberhalb der Übersichtsanzeige für den Cluster auf **Kubernetes-Dashboard**.
3. Verwenden Sie im Kubernetes-Dashboard die Dropdown-Liste **Namensbereich** im linken Navigationsbereich, um den Namensbereich für Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Serviceinstanz zu verwenden, die Sie zuvor aufgespürt haben.
4. Klicken Sie im linken Navigationsbereich auf **Pods**, um die Liste der von Ihnen bereitgestellten Knotenpods anzuzeigen.
5. Klicken Sie auf einen Pod. Klicken Sie anschließend im oberen Menü auf **Protokolle**, um die Protokolle für Ihren Knoten zu öffnen. Über den Protokollen können Sie das Dropdown-Menü nach **Protokolle von** verwenden, um die Protokolle aus den verschiedenen Containern innerhalb des Pods anzuzeigen. Ihr Peer und die Statusdatenbank (z. B. CouchDB) werden beispielsweise in verschiedenen Containern ausgeführt und generieren daher unterschiedliche Protokolle.

Standardmäßig werden die Protokolle Ihrer Knoten lokal in Ihrem Cluster erfasst. Sie können auch die {{site.data.keyword.cloud_notm}}-Services oder den Service eines anderen Anbieters verwenden, um die Protokolle aus Ihrem Netz zu erfassen, zu speichern und zu analysieren. Weitere Informationen finden Sie im Abschnitt [Protokollierung und Überwachung für {{site.data.keyword.IBM_notm}} Kubernetes Service](/docs/containers?topic=containers-health#health){: external}. Es wird empfohlen, die Vorteile des [{{site.data.keyword.cloud_notm}} LogDNA](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube){: external}-Service zu nutzen, mit dem die Protokolle auf einfache Weise in Echtzeit analysiert werden können.

### Protokolle für Ihren Smart-Contract-Container anzeigen
{: #ibp-console-manage-console-container-logs}

Bei Problemen mit Ihrem Smart Contract können Sie die Containerprotokolle für Smart Contract oder Chaincode anzeigen, um die Probleme zu beheben:

- Öffnen Sie das Kubernetes-Dashboard, filtern Sie nach Ihrem [Namensbereich](#ibp-console-manage-console-node-logs) und klicken Sie auf den Peer-Pod, in dem der Smart Contract ausgeführt wird.
- Klicken Sie in Ihrem Dashboard auf den Link `Protokolle`. Der Link verweist standardmäßig auf den Peer-Container.
- Wechseln Sie zum Container `fluentd`, indem Sie ihn in der Dropdown-Liste auswählen.  

In diesem Fenster werden alle Ihre Smart Contract-Protokolle aufgelistet und können über das Download-Symbol in der Anzeige heruntergeladen werden.

## Programmkorrekturen für Ihre Knoten installieren
{: #ibp-console-manage-patch}

Die zugrunde liegenden {{site.data.keyword.IBM_notm}} Hyperledger Fabric-Docker-Images für den Peer, die Zertifizierungsstelle und die Anordnungsknoten müssen im Laufe der Zeit möglicherweise aktualisiert werden (z. B. mit Sicherheitsupdates oder durch ein neues Punktrelease für Fabric). Der Text **Patch verfügbar** auf einer Knotenkachel gibt an, dass eine solche Programmkorrektur (Patch) verfügbar ist und jederzeit auf dem Knoten installiert werden kann. Diese Programmkorrekturen sind optional, werden aber empfohlen.  Knoten, die in die Konsole importiert wurden, können nicht aktualisiert werden.

Programmkorrekturen werden auf Knoten nacheinander angewendet. Während der Anwendung der Programmkorrektur steht der Knoten für die Verarbeitung von Anforderungen und Transaktionen nicht zur Verfügung. Um Unterbrechungen der Serviceverfügbarkeit zu vermeiden, sollten Sie deshalb sicherstellen, einen weiteren Knoten desselben Typs verfügbar zu halten, der die Anforderungen verarbeiten kann, sofern dies möglich ist. Die Installation einer Programmkorrektur auf einem Knoten dauert ca. eine Minute. Nach Abschluss der Aktualisierung ist der Knoten wieder zur Verarbeitung von Anforderungen bereit.
{:note}

Öffnen Sie zum Anwenden einer Programmkorrektur auf einen Knoten die Kachel des betreffenden Knotens und klicken Sie dann auf die Schaltfläche **Programmkorrektur installieren**. Knoten, die Sie in die Konsole importiert haben, können nicht aktualisiert werden.

## Ablauf des Kubernetes-Clusters
{: #ibp-console-manage-console-cluster-expiration}

Wenn Sie einen kostenlosen {{site.data.keyword.cloud_notm}} Kubernetes Service-Cluster verwenden, läuft dieser nach 30 Tagen ab. In diesem Fall können Sie nicht mehr auf Ihre Konsole zugreifen. Alle zugehörigen Knoten und Zertifikate werden ebenfalls gelöscht. Sie können über nur jeweils einen kostenlosen Cluster verfügen. Daher müssen Sie, bevor Sie eine weitere Blockchain-Serviceinstanz erstellen können, sicherstellen, dass der abgelaufene Cluster und die zugehörige Serviceinstanz aus {{site.data.keyword.cloud_notm}} gelöscht wurden. Führen Sie die folgenden Schritte aus, um zu überprüfen, ob sie bereits gelöscht wurden, oder um diese Ressourcen manuell zu löschen:

1. Klicken Sie in Ihrem {{site.data.keyword.cloud_notm}}-Dashboard auf das Hamburgermenü und öffnen Sie die **Ressourcenliste**.
2. Blättern Sie zum Pfeilsymbol von **Services** und erweitern Sie es, um Ihre Serviceinstanz anzuzeigen.
3. Wenn Ihre Instanz aufgelistet ist, klicken Sie auf das Aktionsmenü für die Serviceinstanz und anschließend auf **Löschen**, um diese Serviceinstanz zu löschen.
4. Blättern Sie zum Pfeilsymbol von **Kubernetes-Clusters** und erweitern Sie es, um Ihren kostenlosen Cluster anzuzeigen.
5. Wenn Ihr kostenloser Cluster aufgelistet ist, klicken Sie auf das Aktionsmenü für den Cluster und anschließend auf **Löschen**, um diesen kostenlosen Cluster zu löschen.

Nachdem diese Aktionen abgeschlossen sind, können Sie die [ursprünglichen Schritte ](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) ausführen, um über die {{site.data.keyword.cloud_notm}}-Katalogseite einen neuen Kubernetes-Cluster und eine Blockchain-Serviceinstanz zu erstellen.
