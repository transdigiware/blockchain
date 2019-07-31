---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-21"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# {{site.data.keyword.blockchainfull_notm}}-Konsole bereitstellen
{: #console-deploy-icp}

Mithilfe der folgenden Anleitung können Sie die {{site.data.keyword.blockchainfull}} Platform-Konsole in Ihrem lokalen {{site.data.keyword.cloud_notm}} Private-Cluster bereitstellen, nachdem das {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramm installiert wurde.
{:shortdesc}

## Erforderliche Ressourcen
{: #console-deploy-icp-resources-required}

Stellen Sie sicher, dass Ihr {{site.data.keyword.cloud_notm}} Private-System die Mindestvoraussetzungen für Hardwareressourcen für die Konsole und für die Komponenten erfüllt, die Sie erstellen. Die Anzahl der erforderlichen vCPUs/CPUs kann je nach Infrastruktur, Netzdesign und Leistungsanforderungen variieren. Der ungefähre Bedarf an vCPU/CPU-Ressourcen kann anhand der [Tabelle der standardmäßigen Ressourcenzuordnungen](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default) für {{site.data.keyword.cloud_notm}} ermittelt werden, obwohl die Zuordnungen in {{site.data.keyword.cloud_notm}} Private etwas davon abweichen. Die tatsächlichen Ressourcenzuordnungen werden beim Bereitstellen eines Knotens in der Blockchain-Konsole angezeigt.

**Hinweise:**  

- Eine vCPU (virtuelle CPU) ist ein virtueller Kern, der einer virtuellen Maschine oder einem physischen Prozessorkern zugeordnet wird, wenn der Server nicht für virtuelle Maschinen partitioniert ist. Bei der Festlegung des virtuellen Prozessorkerns (VPC) für Ihre Bereitstellung in {{site.data.keyword.cloud_notm}} Private müssen Sie bestimmte vCPU-Voraussetzungen berücksichtigen. VPC ist eine Maßeinheit, mit der die Lizenzgebühren für {{site.data.keyword.IBM_notm}} Produkte bestimmt werden. Weitere Informationen zu Szenarios für die Festlegung des VPC finden Sie unter [Virtueller Prozessorkern (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- Eine vCPU entspricht einer CPU, die wiederum einem VPC entspricht.

## Speicher
{: #console-deploy-icp-storage}

Im {{site.data.keyword.blockchainfull_notm}}-Helm-Diagramm wird die dynamische Bereitstellung für den Speicher verwendet, der von der Konsole und den von Ihnen zu erstellenden Blockchain-Komponenten genutzt wird. Vor dem Bereitstellen der Konsole müssen Sie eine Speicherklasse (storageClass) mit ausreichendem Sicherungsspeicher für Ihre Konsole und Ihre Komponenten erstellen. Sie müssen den Namen der Speicherklasse angeben, die Sie beim Konfigurieren erstellt haben.

- Wenn Sie die Standardeinstellungen verwenden, erstellt das Helm-Diagramm eine neue Anforderung für einen persistenten Datenträger mit dem Namen des Helm-Release für Ihre Konsolendaten.


## Voraussetzungen für die Bereitstellung der Konsole
{: #console-deploy-icp-prerequisites}

1. Bevor Sie die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitstellen können, müssen Sie [ und {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup) und das [{{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramm installieren](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console_helm-install).

2. Sie sollten einen neuen, angepassten Namensbereich für Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Bereitstellung erstellen. In Ihrem Namensbereich muss die [erforderliche Pod-Sicherheitsrichtlinie (PodSecurityPolicy)](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-prereqs-pod-security-requirements) verwendet werden. Wenn Sie mehrere Blockchain-Netze erstellen möchten (zum Beispiel für unterschiedliche Umgebungen für Entwicklung. Staging und Produktion) sollten Sie für jede Umgebung einen eindeutigen Namensbereich erstellen. Dabei ist zu beachten, dass pro Namensbereich jeweils nur ein Helm-Diagramm bereitgestellt werden kann. Wenn mehrere Instanzen der Konsole in demselben Cluster ausgeführt werden sollen, müssen Sie daher separate Namensbereiche verwenden.

3. Rufen Sie den Wert für die Proxy-IP-Adresse des Clusters Ihrer Zertifizierungsstelle in der {{site.data.keyword.cloud_notm}} Private-Konsole ab. **Hinweis:** Sie müssen die Berechtigung eines [Clusteradministrators](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} besitzen, um auf Ihre Proxy-IP zugreifen zu können. Melden Sie sich beim {{site.data.keyword.cloud_notm}} Private-Cluster an. Klicken Sie im linken Navigationsfenster auf **Plattform** und anschließend auf **Knoten**, um die im Cluster definierten Knoten anzuzeigen. Klicken Sie auf den Knoten mit der Rolle `Proxy` und kopieren Sie den Wert, der in der Tabelle für die `Host-IP` angegeben ist. **Wichtig:** Speichern Sie diesen Wert. Sie benötigen ihn, wenn Sie das Feld für die `Proxy-IP-Adresse` des Helm-Diagramms konfigurieren.

4. Erstellen Sie eine [Imagesicherheitsrichtlinie](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-image-policy), die es Ihrer Bereitstellung ermöglicht, die erforderlichen Images aus Ihrer Cluster-Docker-Registry herunterzuladen.

5. Erstellen Sie ein Kennwort für die erste Anmeldung an der Konsole und speichern Sie es in einem Objekt für den geheimen Schlüssel in {{site.data.keyword.cloud_notm}} Private. Die Schritte zum Erstellen des geheimen Schlüssel sind im [folgenden Abschnitt](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) beschrieben.


## Voraussetzungen für Clusterimagerichtlinien
{: #console-deploy-icp-image-policy}

Die [Containerimagesicherheit](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_images/image_security.html) ist in ICP 3.2+ standardmäßig aktiviert. Daher müssen Sie die Docker Hub-Containerregistry hinzufügen, die Sie beim Installieren des Helm-Diagramms in der Liste der anerkannten Registrys angegeben haben. Andernfalls kann Ihre Konsolenbereitstellung nicht auf die Images in dieser Registry zugreifen. Führen Sie die folgenden Schritte aus, um eine neue Imagerichtlinie zu erstellen:

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}} Private-Konsole an. Klicken Sie im linken Navigationsfenster auf **Verwalten** und anschließend auf **Ressourcensicherheit**. Navigieren Sie im Menü **Ressourcensicherheit** zu **Imagerichtlinien** und klicken Sie auf **Imagerichtlinie erstellen**.

2. Füllen Sie im Abschnitt **Richtliniendetails** die folgenden Felder aus:
  - Geben Sie einen **Namen** für die neue Imagerichtlinie ein. Beispiel: `ibp-imagepolicy`.
  - Wählen Sie für **Bereich** die Option `Namensbereich` aus.
  - Wählen Sie unter **Namensbereich** den Namensbereich aus, in dem Ihr Helm-Diagramm installiert wurde.   

3. Klicken Sie im Abschnitt **Richtliniendetails** auf **Registry hinzufügen** und geben Sie die Image-Registry an, die Sie beim [Installieren des Helm-Diagramms](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install-importing) angegeben haben, gefolgt von der Angabe `/*`.
  - Als Beispiel können Sie die Registray `<cluster_CA_domain>:8500/*` oder `<cluster_CA_domain>:8500/ibp/*` und `<cluster_CA_domain>:8500/op-tools/*` angeben.

Sie können eine neue Cluster-Image-Richtlinie auch mithilfe einer YAML-Datei und mit dem Befehlszeilentool "kubectl" angeben. Ein Beispiel für eine YAML-Beispieldatei ist nachfolgend dargestellt:

```
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ClusterImagePolicy
metadata:
  name: ibp-imagepolicy
spec:
  repositories:
  - name: <cluster-CA-domäne>:8500/ibp/*
  - name: <cluster-CA-domäne>:8500/op-tools/*
```

## Geheimen Schlüssel für Konsolenkennwort erstellen
{: #console-deploy-icp-password-secret}

Bevor Sie auf Ihre Konsole zugreifen können, müssen Sie ein Standardkennwort für die erste Anmeldung bei der Konsole erstellen. Erstellen Sie einen [geheimen Kubernetes-Schlüssel](https://kubernetes.io/docs/concepts/configuration/secret/){: external} zum Speichern des Kennworts, bevor Sie die Konsole bereitstellen. Mit einem geheimen Kubernetes-Schlüssel können Sie Informationen schützen und gemeinsam nutzen, ohne die Angaben direkt an die Bereitstellung übergeben zu müssen.

1. Erstellen Sie ein Kennwort und codieren Sie es im Base64-Format. Führen Sie den folgenden Befehl in einem Terminalfenster aus und ersetzen Sie dabei `password` durch den Wert, den Sie verwenden möchten. **Speichern Sie die Ausgabe des Befehls**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Melden Sie sich bei der {{site.data.keyword.cloud_notm}} Private-Konsole an. Klicken Sie im linken Navigationsfenster auf **Konfiguration** und anschließend auf **Geheime Schlüssel**. Klicken Sie auf die Schaltfläche **Geheimen Schlüssel erstellen**, um ein Popup-Fenster zu öffnen, in dem Sie ein neues Objekt für einen geheimen Schlüssel generieren können.

3. Füllen Sie auf der Registerkarte **Allgemein** die folgenden Felder aus:
  - **Name:** Geben Sie für Ihren geheimen Schlüssel einen Namen ein, der in Ihrem Cluster eindeutig ist. Sie benötigen diesen Namen beim Bereitstellen Ihrer Konsole. Der Name darf nur aus Kleinbuchstaben bestehen.
  - **Namensbereich:** Dies ist der Namensbereich, zu dem Ihr geheimer Schlüssel hinzugefügt werden soll. Wählen Sie  den `Namensbereich` aus, in dem Sie Ihre Konsole bereitstellen möchten.
  - Geben Sie für **Typ** den Wert `generisch` an.

4. Lassen Sie die Registerkarte **Annotationen** leer.

5. Fügen Sie auf der Registerkarte **Daten** den Benutzernamen und das Kennwort als Schlüssel/Wert-Paare hinzu.
  1. Geben Sie im ersten Feld **Name** die Zeichenfolge `Kennwort` ein.
  2. Geben Sie im ersten Feld **Wert** das Ergebnis von `echo -n 'password' | base64` aus Schritt 1 ein.
  3. Klicken Sie auf **Erstellen**, um ein neues Objekt für einen geheimen Schlüssel zu generieren.

Geben Sie den Namen für den geheimen Schlüssel in das Feld `Name des geheimen Schlüssels für Konsolenadministratorkennwort` auf der Konfigurationsseite an, wenn Sie die Konsole bereitstellen. Für die erste Anmeldung bei der Konsole benötigen Sie den Wert, den Sie in Schritt 1 codiert haben. Dieser Wert wird als Standardkennwort für die Konsole verwendet, sofern er nicht von einem Konsolenadministrator geändert wird. Weitere Informationen finden Sie unter [Benutzer mit der Konsole verwalten](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users).

## Geheimen TLS-Schlüssel erstellen (Optional)
{: #console-deploy-icp-tls-secret}

In der Konsole wird TLS für die sichere Kommunikation mit Ihren Blockchain-Knoten verwendet. Die Konsole generiert standardmäßig eigene TLS-Zertifikate. Sie können jedoch optional Ihr eigenes TLS-Zertifikat mit Schlüssel angeben. Mithilfe der folgenden Schritte können Sie die Zertifikate in einem [geheimen Kubernetes-Schlüssel](https://kubernetes.io/docs/concepts/configuration/secret/) speichern. Anschließend können Sie diese Zertifikate in der Konsole bereitstellen, indem Sie den Namen des geheimen Schlüssels beim Konfigurieren im Feld für den geheimen TLS-Schlüssel angeben.

1. Wandeln Sie das TLS-Zertifikat und den privaten Schlüssel in das Base64-Format um. Sie können den Inhalt der Zertifikats- oder Schlüsseldatei in eine Base64-Zeichenfolge umwandeln, indem Sie den folgenden Befehl auf Ihrer lokalen Maschine ausführen:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <zertifikat_oder_schlüssel> | base64 $FLAG
  ```

  Führen Sie diesen Befehl für Ihr TLS-Zertifikat und für den zugehörigen TLS-Schlüssel aus. **Speichern** Sie die Ausgabe.

2. Melden Sie sich bei der {{site.data.keyword.cloud_notm}} Private-Konsole an. Klicken Sie im linken Navigationsfenster auf **Konfiguration** und anschließend auf **Geheime Schlüssel**. Klicken Sie auf die Schaltfläche **Geheimen Schlüssel erstellen**, um ein Popup-Fenster zu öffnen, in dem Sie ein neues Objekt für einen geheimen Schlüssel generieren können.

3. Füllen Sie auf der Registerkarte **Allgemein** die folgenden Felder aus:
  - **Name:** Geben Sie für Ihren geheimen Schlüssel einen Namen ein, der in Ihrem Cluster eindeutig ist. Diesen Namen verwenden Sie zum Konfigurieren Ihrer Zertifizierungsstelle. Der Name darf nur aus Kleinbuchstaben bestehen.
  - **Namensbereich:** Dies ist der Namensbereich, zu dem Ihr geheimer Schlüssel hinzugefügt werden soll. Wählen Sie den `Namensbereich` aus, in dem Sie Ihre Zertifizierungsstelle bereitstellen wollen.
  - **Typ:** Geben Sie den Wert `kubernetes.io/tls` ein.

4. Lassen Sie die Registerkarte **Annotationen** leer.

5. Fügen Sie auf der Registerkarte **Daten** den Benutzernamen und das Kennwort als Schlüssel/Wert-Paare hinzu.
  1. Geben Sie im ersten Feld **Name** die Zeichenfolge `tls.crt` ein.
  2. Geben Sie im ersten Feld **Wert** die Zeichenfolge Ihres Base64-codierten TLS-Zertifikats aus Schritt 1 ein.
  3. Klicken Sie auf die Schaltfläche **Daten hinzufügen**, um ein zweites Schlüssel/Wert-Paar hinzuzufügen.
  4. Geben Sie im ersten Feld **Name** die Zeichenfolge `tls.key` ein.
  5. Geben Sie im ersten Feld **Wert** die Zeichenfolge Ihres Base64-codierten TLS-Schlüssels aus Schritt 1 ein.
  6. Klicken Sie auf **Erstellen**, um ein neues Objekt für einen geheimen Schlüssel zu generieren.

Geben Sie beim Bereitstellen Ihrer Konsole den Namen des von Ihnen erstellten geheimen Schlüssels in das Feld `Geheimer TLS-Schlüssel` in allen Abschnitten für Parameter auf der Konfigurationsseite ein.

Geheime Schlüssel werden nicht aus Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster entfernt, wenn Sie Ihr Helm-Release löschen. Für die Verwaltung des Kennworts und der geheimen TLS-Schlüssel in Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster sind Sie selbst verantwortlich. Wenn Sie zu einem späteren Zeitpunkt eine andere Konsole bereitstellen möchten, können Sie die geheimen Schlüssel wiederverwenden. Andernfalls müssen Sie dafür sorgen, dass sie aus Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster gelöscht werden.
{:note}

## Konfiguration
{: #console-deploy-icp-configuration}

Nachdem Sie das Objekt für den geheimen TLS-Schlüssel erstellt haben, können Sie die Konsole mit den folgenden Schritten bereitstellen.

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}} Private-Konsole an und klicken Sie in der rechten oberen Ecke auf **Katalog**.
2. Klicken Sie im linken Navigationsfenster auf `Blockchain` und suchen Sie nach der Kachel namens `ibm-blockchain-platform-prod`. Klicken Sie auf die Kachel, um sie zu öffnen. Daraufhin wird eine Readme-Datei mit Informationen zur Installation und Konfiguration des Helm-Diagramms angezeigt.
3. Klicken Sie auf die Registerkarte **Konfiguration** oben in der Anzeige oder klicken Sie auf die Schaltfläche **Konfigurieren** in der rechten unteren Ecke.
4. Geben Sie die Werte für die [Sicherheitsparameter für Konfiguration und Pod](#icp-peer-deploy-configuration-parms) an und akzeptieren Sie die Lizenzvereinbarung.
5. Navigieren Sie zum Abschnitt **Parameter**:
- Sie können die Konsole allein mithilfe der [Quickstart-Parameter](#icp-peer-deploy-quickstart-parms) bereitstellen. Verwenden Sie diese Option zum Experimentieren oder als Einstieg.
- Im Abschnitt [Alle Parameter](#icp-peer-deploy-quickstart-parms) können Sie den Netzzugang, die Ressourcen und den Speicher für Ihre Konsole anpassen. Der Abschnitt **Alle Parameter** wird nur für erfahrene Kubernetes-Benutzer empfohlen.
6. Klicken Sie auf **Installieren**.

In den folgenden Tabellen werden die Felder für Konfigurationsparameter und die zugehörigen Standardwerte beschrieben.

### Parameter für Konfiguration und Pod-Sicherheit
{: #icp-peer-deploy-configuration-parms}

|  Parameter     | Beschreibung    | Standardwert  | Erforderlich |
| --------------|-----------------|-------|------- |
| `Helm-Releasename`| Dieser Name identifiziert Ihre Pod-Release-Bereitstellung. Er muss mit einem Kleinbuchstaben beginnen und darf nur aus Bindestrichen und alphanumerischen Zeichen in Kleinschreibung bestehen. | Keine | Ja |
| `Zielnamensbereich`| Geben Sie den Namensbereich an, den Sie für Ihre Konsole und Komponenten erstellt haben. Der Namensbereich muss eine Richtlinie `ibm-privileged-psp` enthalten. Andernfalls [binden Sie eine Pod-Sicherheitsrichtlinie](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) an Ihren Namensbereich. | Keine | Ja |
| `Zielcluster`| Geben Sie die Cluster an, in denen die Ressource bereitgestellt werden soll. Die verfügbaren Cluster werden nach dem ausgewählten Namensbereich und den Kubernetes-Versionsanforderungen gefiltert. | Keine | Ja |
| `Richtlinien für den Zielnamensbereich`| Für die Verwendung Ihres Zielnamensbereichs vorkonfiguriert. Die Werte müssen die Richtlinie `ibm-privileged-psp` oder `ibm-blockchain-platform-psp` enthalten. | Keine | Ja |

### Quickstart und "Alle Parameter"
{: #icp-peer-deploy-quickstart-parms}

Verwenden Sie den Quickstart-Abschnitt zum Experimentieren oder als Einstieg. Im Abschnitt "Alle Parameter" können Sie den Netzzugang, die Ressourcen und den Speicher anpassen. Dieser Abschnitt wird nur für erfahrene Kubernetes-Benutzer empfohlen.

**Klicken Sie auf die Registerkarte "Alle Parameter", um alle Konfigurationsparameter anzuzeigen.**

|  Parameter     | Beschreibung    | Standardwert  | Erforderlich |
| --------------|-----------------|-------|------- |
| **Konsoleneinstellungen** | **Verwaltung und Bereitstellung Ihrer Konsole** | | |
| `Proxy-IP` | Die IP-Adresse des Proxy-Knotens in Ihrem Cluster. | Keine| Ja |
| `E-Mail-Adresse des Konsolenadministrators` | Die E-Mail-Adresse zum Anmelden bei der Konsole. | Keine | Ja |
| `Name des geheimen Schlüssels für Konsolenadministratorkennwort` | Der Name  des geheimen Schlüssels, den Sie [zum Speichern des Kennworts erstellt](#console-deploy-icp-password-secret) haben, mit dem Sie sich bei der Konsole anmelden möchten. | Keine | Ja |
| **Netzeinstellungen** | **Netzzugriff auf Ihre Konsole** | | |
| `Hostname für Konsole` | Geben Sie denselben Wert wie die Proxy-IP ein. | Keine | Ja |
| `Port für Konsole` | Geben Sie den Port ein, den Sie verwenden möchten (im Bereich von 31210 bis 31220). Dieser Port darf von keiner anderen Anwendung verwendet werden. | Keine | Ja |
| `Proxy-Hostname` | Der Hostname des Proxy-Servers.  Geben Sie denselben Wert wie die Proxy-IP ein. | Keine | Ja |
| `Proxy-Port` | Geben Sie den Port ein, den Sie verwenden möchten (im Bereich von 31210 bis 31220). Dieser Port darf weder von einer anderen Anwendung noch von der Konsole verwendet werden. | Keine | Ja |
| **Speichereinstellungen** | **Der Speicher, der von Ihrer Konsole verwendet werden soll.** | | |
| `Speicherklassenname` | Geben Sie den Namen der Speicherklasse ein, die von der Konsole sowie den von Ihnen erstellen Komponenten verwendet werden soll.| Keine | Ja |
{: caption="Tabelle 1. Quickstart-Parameter" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Parameter     | Beschreibung    | Standardwert  | Erforderlich |
| --------------|-----------------|-------|------- |
| `Lizenz` | Geben Sie an, ob Sie die Lizenzbedingungen akzeptieren. | Nicht akzeptiert | Ja |
| `Architektur` | Wählen Sie die Architektur Ihrer Cloutplattform aus. (AMD64 oder S390x) | AMD64 | Nein |
| **Konsoleneinstellungen** | **Verwaltung und Bereitstellung Ihrer Konsole** | | |
| `Name des Servicekontos` | Das Servicekonto, das vom Operator (Bediener) verwendet werden soll. | Keine | Nein |
| `Proxy-IP` | Die IP-Adresse des Proxy-Knotens in Ihrem Cluster. | Keine | Ja |
| `E-Mail-Adresse des Konsolenadministrators` | Die E-Mail-Adresse zum Anmelden bei der Konsole. | Keine | Ja |
| `Name des geheimen Schlüssels für Konsolenadministratorkennwort` | Der Name  des geheimen Schlüssels, den Sie [zum Speichern des Kennworts erstellt](#console-deploy-icp-password-secret) haben, mit dem Sie sich bei der Konsole anmelden möchten. | Keine | Ja |
| **Docker-Image-Einstellungen** | **Mit diesen Einstellungen können Sie angeben, welche Fabric-Images von der Konsole extrahiert werden sollen.** | | |
| `Name für "imagePullSecret"` | Der geheime Schlüssel zum Abrufen von Images (imagePullSecret), der zum Herunterladen von Images verwendet werden soll. | `ibp-ibmregistry` | Nein |
| **Netzeinstellungen** | **Netzzugriff auf Ihre Konsole** | | |
| `Hostname für Konsole` | Geben Sie denselben Wert wie die Proxy-IP ein. | Keine | Ja |
| `Port für Konsole` | Geben Sie den Port ein, den Sie verwenden möchten (im Bereich von 31210 bis 31220). | Keine | Ja |
| `Proxy-Hostname` | Der Hostname des Proxy-Servers.  Geben Sie denselben Wert wie die Proxy-IP ein. | Keine | Ja |
| `Proxy-Port` | Geben Sie den Port ein, den Sie verwenden möchten (im Bereich von 31210 bis 31220). Dieser Port darf weder von einer anderen Anwendung noch von der Konsole verwendet werden. | Keine | Ja |
| `Geheimer TLS-Schlüssel` | Der Name des geheimen Schlüssels, den Sie [zum Speichern der TLS-Zertifikate erstellt](#console-deploy-icp-tls-secret) haben, die von der Konsole verwendet werden. | Keine | Nein |
| **Speichereinstellungen**| **Speicher für Konsole und Tools bereitstellen** | | |
| `Volume Claim-Größe`| Die Größe des Persistent Volume Claim, der bereitgestellt werden soll. | 10Gi  | Nein |
| `Speicherklassenname`| Der Name der Speicherklasse, die von der Konsole und den von Ihnen erstellen Komponenten verwendet werden soll. | Keine | Ja |
| `Speicherzugriffsmodus`| Geben Sie den Speicher[zugriffsmodus](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes) für den PVC an. | ReadWriteMany | Nein |
| **Ressourcenzuordnung**| **Ressourcen für die Konsole zuordnen** | | |
| `Opstools-CPU-Limit` | Maximale Anzahl CPUs, die für die Komponente "opstools" zugeordnet werden soll. | 500m | Nein |
| `Opstools-Speicherbegrenzung` | Maximale Speicherkapazität, die für die Komponente "opstools" zugeordnet werden soll. | 1000 Mi | Nein |
| `Opstools-CPU-Anforderung` | Mindestanzahl der CPUs, die der Komponente "opstools" zugeordnet werden soll. | 500m | Nein |
| `Opstools-Speicheranforderung` | Minimale Speicherkapazität, die für die Komponente "opstools" zugeordnet werden soll.| 1000 Mi | Nein |
| `Configtxlator-CPU-Limit` | Maximale Anzahl CPUs, die dem Tool "configtxlator" zugeordnet werden soll. | 25m | Nein |
| `Configtxlator-Speicherbegrenzung` | Maximale Speicherkapazität, die für das Tool "configtxlator" zugeordnet werden soll. | 50 Mi | Nein |
| `Configtxlator-CPU-Anforderung` | Mindestanzahl der CPUs, die dem Tool "configtxlator" zugeordnet werden soll.| 25m | Nein |
| `Configtxlator-Speicheranforderung` | Minimale Speicherkapazität, die für das Tool "configtxlator" zugeordnet werden soll.| 50 Mi | Nein |
| `CouchDB-CPU-Limit` | Maximale Anzahl CPUs, die für CouchDB zugeordnet werden soll. | 500m | Nein |
| `CouchDB-Speicherbegrenzung` | Maximalwert für Speicher, der für CouchDB zugeordnet werden soll. | 1000 Mi | Nein |
| `CouchDB-CPU-Anforderung` | Mindestanzahl der CPUs, die für CouchDB zugeordnet werden soll.| 500m | Nein |
| `CouchDB-Speicheranforderung` | Minimale Speicherkapazität, die für CouchDB zugeordnet werden soll.| 1000 Mi | Nein |
| `Operator-CPU-Limit` | Maximale Anzahl CPUs, die der Komponente "operator" zugeordnet werden soll. | 100 m | Nein |
| `Operator-Speicherbegrenzung` | Maximale Speicherkapazität, die der Komponente "operator" zugeordnet werden soll. | 200 Mi | Nein |
| `Operator-CPU-Anforderung` | Mindestanzahl der CPUs, die der Komponente "operator" zugeordnet werden soll. | 100 m | Nein |
| `Operator-Speicheranforderung` | Minimale Speicherkapazität, die der Komponente "operator" zugeordnet werden soll. | 200 Mi | Nein |
| `Deployer-CPU-Limit` | Maximale Anzahl CPUs, die der Komponente "deployer" zugeordnet werden soll. | 100 m | Nein |
| `Deployer-Speicherbegrenzung` | Maximale Speicherkapazität, die der Komponente "deployer" zugeordnet werden soll. | 200 Mi | Nein |
| `Deployer-CPU-Anforderung` | Mindestanzahl der CPUs, die der Komponente "deployer" zugeordnet werden soll. | 100 m | Nein |
| `Deployer-Speicheranforderung` | Minimale Speicherkapazität, die der Komponente "deployer" zugeordnet werden soll. | 200 Mi | Nein |
{: caption="Tabelle 2. Alle Parameter" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Helm-Release mit der Helm-Befehlszeile installieren
{: #console-deploy-icp-helm-cli}

Zur Installation des Helm-Release können Sie alternativ auch die CLI `helm` verwenden. Stellen Sie vor der Ausführung des Befehls `helm install` sicher, dass Sie das [Helm-Repository des Clusters zur Helm-CLI-Umgebung hinzugefügt](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external} haben.

Die für die Installation erforderlichen Parameter können Sie festlegen, indem Sie eine Datei `yaml` erstellen und sie an den folgenden Befehl `helm install` übergeben.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Hierbei gilt Folgendes:
- `<helm_release name>` steht für den Namen, den Sie für das Helm-Release vergeben möchten.
- `<helm_chart>` steht für den Namen des Helm-Diagramms, das in den Katalog importiert wird.
- `<helm_chart_version>` steht für die Version des Helm-Diagramms, das in den Katalog importiert wird.
- `<customvalues.yaml>` ist der Name der YAML-Datei, in der die Konfigurationsparameter enthalten sind.

Beispiel:
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

Sie können eine neue Datei `yaml` erstellen, indem Sie die Datei `values.yaml` bearbeiten, die in der heruntergeladenen Archivdatei enthalten ist. In dieser Datei befinden sich alle erforderlichen Parameter mit ihren Standardeinstellungen.

## Installation prüfen

Nachdem Sie die Konfigurationsparameter ausgefüllt und auf die Schaltfläche **Installieren** geklickt haben, können Sie die Bereitstellung prüfen, indem Sie auf die Schaltfläche **Helm-Release anzeigen** klicken. Wenn die Bereitstellung erfolgreich durchgeführt wurde, sollte der Wert 1 in den Feldern `GEWÜNSCHT`, `DERZEIT`, `AKTUELL` und `VERFÜGBAR` der Tabelle "Bereitstellung" angezeigt werden. Möglicherweise müssen Sie zur Aktualisierung klicken und warten, bis die Tabelle aktualisiert angezeigt wird.

Navigieren Sie zum Anzeigen der Details Ihrer Bereitstellung zur Übersichtsanzeige **Bereitstellung** und klicken Sie auf den Pod, der erstellt wurde. Für die Bereitstellung des Helm-Diagramms werden diese fünf Container in Ihrem Cluster erstellt: 
- **optools**: Die Benutzerschnittstelle der Konsole.
- **deployer**: Dieses Tool ermöglicht der Konsole die Kommunikation mit Ihren Bereitstellungen.
- **configtxlator**: Dieses Tool wird von der Konsole verwendet, um Kanalaktualisierungen zu lesen und zu erstellen.
- **couchdb**: Eine CouchDB-Instanz, in der Daten aus Ihrer Konsole sowie Ihre Berechtigungsinformationen gespeichert werden.
- **operator**: Ein Kubernetes-Operator zum Verwalten Ihrer Komponentenbereitstellungen.

## Bei der Konsole anmelden

Nach der Installation können Sie in Ihrem Browser auf die Konsole zugreifen. Die URL finden Sie im Abschnitt **Hinweise:** der Übersichtsanzeige des Helm-Release, die nach der Bereitstellung angezeigt wird. Stellen Sie sicher, dass Sie nicht die ESR-Version von Firefox verwenden. Wenn ja, wechseln Sie in einen anderen Browser, wie z. B. Chrome, und wiederholen Sie den Vorgang.

Die Anmeldeanzeige der Konsole müsste nun in Ihrem Browser angezeigt werden:
- Geben Sie für **Benutzer-ID** den Wert an, den Sie beim Konfigurieren im Feld `E-Mail-Adresse des Konsolenadministrators` angegeben haben.
- Geben Sie als **Kennwort** den Wert an, den Sie codiert, im [geheimen Kennwortschlüssel](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) gespeichert und anschließend während der Konfiguration an die Konsole übergeben haben. Dieses Kennwort wird als Standarkenntwort für die Konsole festgelegt, das alle Benutzer zum Anmelden bei der Konsole verwenden. Nach dem ersten Anmeldevorgang werden Sie aufgefordert, ein neues Anmeldekennwort für die Konsole anzugeben.

Der Administrator, von dem das Helm-Diagramm bereitgestellt wurde, kann anderen Benutzern Zugriff auf die Konsole gewähren und angeben, welche Operationen die Benutzer ausführen können. Weitere Informationen finden Sie unter [Benutzer mit der Konsole verwalten](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users).

## Nächste Schritte
{: #console-deploy-icp-next-steps}

Nachdem Sie Ihre Konsole aufgerufen haben, können Sie die Registerkarte **Knoten** der Konsolenbenutzerschnittstelle anzeigen. Über diese Anzeige können Sie Komponenten in Ihrem lokalen Cluster bereitstellen. Eine Einführung in die Verwendung der Konsole finden Sie im [Lernprogramm zum Erstellen eines Netzes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network). Sie können diese Registerkarte auch verwenden, um Knoten zu steuern, die in anderen Clouds erstellt wurden. Weitere Informationen finden Sie unter [Knoten importieren](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes).

Zum Verwalten der Benutzer, die Zugriff auf die Konsole haben, können Sie {{site.data.keyword.blockchainfull_notm}} Platform-APIs verwenden. Informationen zum Anzeigen der Protokolle für Ihre Konsole und für Blockchain-Komponenten finden Sie in [Konsole in {{site.data.keyword.cloud_notm}} Private verwalten](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage).
