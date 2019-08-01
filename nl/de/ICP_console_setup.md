---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-16"


keywords: IBM Cloud Private, data storage CA, cluster ICP, configuration

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

# {{site.data.keyword.cloud_notm}} Private einrichten
{: #icp-console-setup}

Bevor Sie Komponenten von {{site.data.keyword.blockchainfull}} Platform bereitstellen und ein Blockchain-Netz in {{site.data.keyword.cloud_notm}} Private (ICP) aufbauen, müssen Sie {{site.data.keyword.cloud_notm}} Private in Ihrer eigenen Umgebung einrichten.
{:shortdesc}

## Voraussetzungen
{: #icp-console-setup-prerequisites}

Führen Sie die folgenden vorausgesetzten Schritte aus und bereiten Sie Ihre Umgebung für die Installation von {{site.data.keyword.cloud_notm}} Private vor.

### Docker
Für {{site.data.keyword.cloud_notm}} Private muss Docker installiert sein. Befolgen Sie zur Installation von Docker die zugehörigen Anweisungen in [Installing {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external}.

### Einstellungen von {{site.data.keyword.cloud_notm}} Private
Vor der Installation von {{site.data.keyword.cloud_notm}} Private helfen Ihnen die folgenden Tipps bei der Vorbereitung der Knoten für die {{site.data.keyword.cloud_notm}} Private-Installation. Weitere Voraussetzungen für {{site.data.keyword.cloud_notm}} Private finden Sie in der [{{site.data.keyword.cloud_notm}} Private-Dokumentation](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Einstellung `vm.max_map_count` aktualisieren
{{site.data.keyword.cloud_notm}} verwendet Elastic Search zur Protokollierung und Messung. Zur Vermeidung von Ausnahmebedingungen durch unzureichenden Speicher ist für Elastic Search die Konfiguration der Systemeigenschaft `vm.max_map_count` erforderlich. Lesen Sie vor der Installation von {{site.data.keyword.cloud_notm}} Private die [Konfigurationsanweisungen für Elastic Search](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external}, um diese Eigenschaft auf jedem Knoten zu konfigurieren. Mit den folgenden Befehlen können Sie diese Eigenschaft permanent festlegen:

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Datei `/etc/hosts` auf jeden Knoten im Cluster konfigurieren

- {{site.data.keyword.cloud_notm}} Private verwendet [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} zum Verwalten containerbasierter Anweisungen. Der Domänennamensserver (DNS) von Kubernetes schlägt fehl, falls nicht auf jedem Knoten in der Datei `/etc/hosts` Hostnamen konfiguriert sind. [Fügen Sie die IP-Adresse, den Hostnamen und den Kurznamen jedes Knotens in Ihrem Cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} auf jedem Knoten in der Datei `/etc/hosts` ein.

- [IPv6 wird von {{site.data.keyword.cloud_notm}} Private nicht unterstützt](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. Inaktivieren Sie zur Vermeidung von Problemen mit dem DNS-Service in einem {{site.data.keyword.cloud_notm}} Private-Cluster auf jedem Knoten die IPv6-Einstellungen in der Datei `/etc/hosts`, indem Sie die folgende Zeile durch Eingabe des Zeichens `#` am Beginn der Zeile auf Kommentar setzen:
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Erforderliche Ressourcen
{: #icp-console-setup-resources}

Stellen Sie sicher, dass Ihr {{site.data.keyword.cloud_notm}} Private-System die Mindestvoraussetzungen für Hardwareressourcen für die Konsole und für die Komponenten erfüllt, die Sie erstellen. Die Anzahl der erforderlichen vCPUs/CPUs kann je nach Infrastruktur, Netzdesign und Leistungsanforderungen variieren. Der ungefähre Bedarf an vCPU/CPU-Ressourcen kann anhand der [Tabelle der standardmäßigen Ressourcenzuordnungen](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default) für {{site.data.keyword.cloud_notm}} ermittelt werden, obwohl die Zuordnungen in {{site.data.keyword.cloud_notm}} Private etwas davon abweichen. Die tatsächlichen Ressourcenzuordnungen werden beim Bereitstellen eines Knotens in der Blockchain-Konsole angezeigt.

**Hinweise:**  

- Eine vCPU (virtuelle CPU) ist ein virtueller Kern, der einer virtuellen Maschine oder einem physischen Prozessorkern zugeordnet wird, wenn der Server nicht für virtuelle Maschinen partitioniert ist. Bei der Festlegung des virtuellen Prozessorkerns (VPC) für Ihre Bereitstellung in {{site.data.keyword.cloud_notm}} Private müssen Sie bestimmte vCPU-Voraussetzungen berücksichtigen. VPC ist eine Maßeinheit, mit der die Lizenzgebühren für {{site.data.keyword.IBM_notm}} Produkte bestimmt werden. Weitere Informationen zu Szenarios für die Festlegung des VPC finden Sie unter [Virtueller Prozessorkern (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- Eine vCPU entspricht einer CPU, die wiederum einem VPC entspricht.

### Hinweise zum Speicher
{: #icp-console-setup-storage-considerations}

Im {{site.data.keyword.blockchainfull_notm}}-Helm-Diagramm wird die dynamische Bereitstellung für den Speicher verwendet, der von der Konsole und den von Ihnen zu erstellenden Blockchain-Komponenten genutzt wird. Vor dem Bereitstellen der Konsole müssen Sie eine Speicherklasse (storageClass) mit ausreichendem Sicherungsspeicher für Ihre Konsole und Ihre Komponenten erstellen. Sie müssen den Namen der Speicherklasse angeben, die Sie beim Konfigurieren erstellt haben.

- Wenn Sie die Standardeinstellungen verwenden, erstellt das Helm-Diagramm eine neue Anforderung für einen persistenten Datenträger mit dem Namen des Helm-Release für Ihre Konsolendaten.
- Falls Sie persistente Datenträger von NFS v2/v3 verwenden, müssen Sie auf dem Hostsystem, auf dem sich das NFS-Dateisystem befindet, das Modul **NFS status monitor for NFSv2/v3 Filesystem Locks** aktivieren, das auch unter der Bezeichnung **rpc-statd** bekannt ist. Dieses Modul ermöglicht es Ihrem NFS-Dateisystem, die exklusiven Sperren für Dateien zu überprüfen, die andere
Prozesse halten. Führen Sie zum Aktivieren dieses Moduls die folgenden Befehle aus:

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## {{site.data.keyword.cloud_notm}} Private installieren
{: #icp-console-setup-install}

Führen Sie die folgenden Schritte aus, um {{site.data.keyword.cloud_notm}} Private in Ihrer Umgebung zu installieren und einzurichten.

1. Installieren Sie einen [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external}-Cluster mit Version 3.2.0.

2. Installieren Sie die {{site.data.keyword.cloud_notm}} Private-CLI [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external}, um das Helm-Diagramm zu installieren.

3. Erstellen Sie einen neuen, angepassten Namensbereich für Ihre {{site.data.keyword.blockchainfull_notm}} Platform-Bereitstellung. Dabei ist zu beachten, dass pro Namensbereich jeweils nur ein Helm-Diagramm bereitgestellt werden kann. Wenn mehrere Instanzen der Konsole in demselben Cluster ausgeführt werden sollen, müssen Sie daher separate Namensbereiche verwenden.

4. Richten Sie die Sicherheits-und Zugriffsrichtlinien für den Zielnamensbereich ein. Anweisungen finden Sie im [nächsten Abschnitt](#icp-console-setup-psp).

Nach der Installation von {{site.data.keyword.cloud_notm}} Private und dem Binden einer Pod-Sicherheitsrichtlinie an einen Zielnamensbereich können Sie mit dem [Import des {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private-Helm-Diagramms](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install) in Ihren {{site.data.keyword.cloud_notm}} Private-Cluster fortfahren.

## Voraussetzungen für Podsicherheitsrichtlinie (PodSecurityPolicy)
{: #icp-console-setup-psp}

Vor der Installation des {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramms müssen bestimmte Sicherheits- und Zugriffsrichtlinien an den Zielnamensbereich gebunden werden. In den nachfolgenden Schritten werden YAML-Dateien bereitgestellt, die die Richtlinien definieren. Diese Dateien können Sie mithilfe der {{site.data.keyword.cloud_notm}} Private-CLI in Ihrem lokalen System speichern und an Ihren Namensbereich binden. Führen Sie diese Schritte aus, bevor Sie das {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramm bereitstellen.

1. Speichern Sie die nachfolgende Datei, in der die Richtlinie "PodSecurityPolicy" für {{site.data.keyword.blockchainfull_notm}} Platform definiert wird, als Datei `ibm-blockchain-platform-psp.yaml` in Ihrem lokalen System:

    ```
    apiVersion: extensions/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: ibm-blockchain-platform-psp
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      privileged: true
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      seLinux:
        rule: RunAsAny
      supplementalGroups:
        rule: RunAsAny
      runAsUser:
        rule: RunAsAny
      fsGroup:
        rule: RunAsAny
      requiredDropCapabilities:
      - ALL
      allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - FOWNER
      volumes:
      - '*'
    ```
    {:codeblock}

2. Speichern Sie die nachfolgende Datei, in der die erforderliche Clusterrolle (ClusterRole) für "PodSecurityPolicy" definiert ist, als Datei `ibm-blockchain-platform-clusterrole.yaml`:

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
    rules:
    - apiGroups:
      - extensions
      resourceNames:
      - ibm-blockchain-platform-psp
      resources:
      - podsecuritypolicies
      verbs:
      - use
    - apiGroups:
      - "*"
      resources:
      - pods
      - services
      - endpoints
      - persistentvolumeclaims
      - persistentvolumes
      - events
      - configmaps
      - secrets
      - ingresses
      - roles
      - rolebindings
      - serviceaccounts
      verbs:
      - '*'
    - apiGroups:
      - apiextensions.k8s.io
      resources:
      - persistentvolumeclaims
      - persistentvolumes
      - customresourcedefinitions
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      - ibpservices
      - ibpcas
      - ibppeers
      - ibpfabproxies
      - ibporderers
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      verbs:
      - '*'
    - apiGroups:
      - apps
      resources:
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
      verbs:
      - '*'
    ```
    {:codeblock}

3. Speichern Sie die nachfolgende Datei, in der die Clusterrollenbindung (ClusterRoleBinding) definiert ist, als Datei `ibm-blockchain-platform-clusterrolebinding.yaml`. Wenn Sie den Namen des Servicekontos (ServiceAccount) in der nachfolgenden Datei ändern möchten, müssen Sie den gewünschten Namen im Feld `Name des Servicekontos` im Abschnitt **Alle Parameter** der Konfigurationsseite beim Bereitstellen des Helm-Diagramms angeben.

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
   name: ibm-blockchain-platform-clusterrolebinding
  roleRef:
   apiGroup: rbac.authorization.k8s.io
   kind: ClusterRole
   name: ibm-blockchain-platform-clusterrole
  subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
  ```
  {:codeblock}

Nachdem Sie die YAML-Dateien für Podsicherheitsrichtlinie, Clusterrolle und Clusterrollenbindung in Ihrem lokalen System gespeichert haben, müssen die Richtlinien von einem Clusteradministrator mithilfe der {{site.data.keyword.cloud_notm}} Private-CLI an Ihren Namensbereich gebunden werden.

1. Melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster an und wählen Sie den Zielnamensbereich Ihrer Bereitstellung aus.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Melden Sie sich bei der Docker-Image-Registry für Ihren Cluster an:

  ```
  docker login <cluster-CA-domäne>:8500
  ```
   {:codeblock}

3. Führen Sie die folgenden Befehle aus, um die Richtlinien auf Ihren Zielnamensbereich anzuwenden:

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. Nach dem Anwenden der Richtlinien müssen Sie Ihrem Servicekonto die erforderliche Berechtigungsstufe zum Bereitstellen Ihrer Konsole erteilen. Führen Sie den folgenden Befehl mit Angabe des Zielnamensbereichs aus:

  ```
  kubectl -n <namensbereich> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namensbereich>
  ```
