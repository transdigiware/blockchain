---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, install, Helm chart, PodSecurityPolicy

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

# {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramm installieren
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private wird als Helm-Diagramm bereitgestellt, das in einem lokalen {{site.data.keyword.cloud_notm}} Private-Cluster installiert werden kann. Nachdem Sie das Helm-Diagramm installiert haben, finden Sie {{site.data.keyword.blockchainfull_notm}} Platform als Anwendung im {{site.data.keyword.cloud_notm}} Private-Katalog.

Das Helm-Diagramm muss über [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external} erworben werden. Die technische Unterstützung für {{site.data.keyword.blockchainfull_notm}} Platform ist im Kauf enthalten.

Lesen Sie vor der Installation von {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private die [Hinweise und Einschränkungen](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations). Weitere Informationen zu Preisstruktur, Support, Sicherheit und Datenspeicherort finden Sie im Abschnitt [Informationen zu {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about).

## Voraussetzungen für die Installation des Helm-Diagramms
{: #console-helm-install-prereqs}

Vor der Installation des Helm-Diagramms müssen Sie einen {{site.data.keyword.cloud_notm}} Private-Cluster konfiguriert und einen neuen Zielnamensbereich erstellt haben, der an eine Pod-Sicherheitsrichtlinie gebunden ist. Lesen Sie hierzu die Anweisungen im Abschnitt über die [Einrichtung und Konfiguration eines {{site.data.keyword.cloud_notm}} Private-Clusters](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup). Sie können nur eine Konsole pro Namensbereich installieren. Wenn Sie mehrere Blockchain-Netze erstellen möchten (zum Beispiel für unterschiedliche Umgebungen für Entwicklung. Staging und Produktion) sollten Sie für jede Umgebung einen eindeutigen Namensbereich erstellen. 

### Voraussetzungen für Podsicherheitsrichtlinie (PodSecurityPolicy)
{: #console-helm-install-prereqs-pod-security-requirements}

Vor der Installation des {{site.data.keyword.blockchainfull_notm}} Platform-Helm-Diagramms müssen bestimmte Sicherheits- und Zugriffsrichtlinien an den Zielnamensbereich gebunden werden. Führen Sie vor dem Konfigurieren des Helm-Diagramms die folgenden Schritte aus, um die Richtlinien zu konfigurieren:

1. Wählen Sie entweder eine vordefinierte Pod-Sicherheitsrichtlinie für Ihren Namensbereich aus oder lassen Sie von Ihrem Clusteradministrator eine angepasste Pod-Sicherheitsrichtlinie erstellen:
  - Sie können die vordefinierte Pod-Sicherheitsrichtlinie [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp) verwenden.
  - Sie können auch mit dem nachfolgenden YAML-Code eine angepasste Definition für die Pod-Sicherheitsrichtlinie erstellen:

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
      volumes:
      - '*'
    ```
    {:codeblock}

2. Erstellen Sie eine Clusterrolle für die Pod-Sicherheitsrichtlinie.
  - Wenn Sie eine angepasste Sicherheitsrichtlinie erstellt haben, können Sie mithilfe der nachfolgenden YAML-Datei eine Clusterrolle erstellen:

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
      - ""
      resources:
      - secrets
      verbs:
      - create
      - delete
      - get
      - list
      - patch
      - update
      - watch
    ```
    {:codeblock}

  - Wenn Sie eine vordefinierte Pod-Sicherheitsrichtlinie verwenden, benötigen Sie zum Erstellen einer Clusterrolle nur den zweiten Abschnitt "apiGroups":

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
      rules:
      - apiGroups:
      - ""
      resources:
      - secrets
      verbs:
      - create
      - delete
      - get
      - list
      - patch
      - update
      - watch
    ```
    {:codeblock}

3. Erstellen Sie eine angepasste Clusterrollenbindung (ClusterRoleBinding). Wenn Sie den Namen des Servicekontos in der nachfolgenden Datei ändern möchten, müssen Sie den Namen im Feld `Name des Servicekontos` im Abschnitt **Alle Parameter** der Konfigurationsseite beim Bereitstellen des Helm-Diagramms angeben.

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

Sie können die folgenden Schritte ausführen, wenn Sie mithilfe von YAML-Dateien Sicherheits- und Zugriffsrichtlinien an Ihren Namensbereich binden möchten:

1. Speichern Sie die YAML-Datei in Ihrem lokalen System.

2. Melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster an und wählen Sie den Zielnamensbereich Ihrer Bereitstellung aus.

  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

3. Verwenden Sie den folgenden Befehl, um die Richtlinie auf den Zielnamensbereich anzuwenden:

  ```
  kubectl apply -f <dateiname>.yaml
  ```
  {:codeblock}

## Helm-Diagramm in {{site.data.keyword.cloud_notm}} Private importieren
{: #console-helm-install-importing}

1. Laden Sie das Helm-Diagramm für {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private von [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external} herunter.

2. Wenn Sie sich noch nicht angemeldet haben, melden Sie sich bei Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster an.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Stellen Sie sicher, dass die  [Docker-CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html)  konfiguriert ist. Greifen Sie nach dem Konfigurieren der Docker-CLI mit dem folgenden Befehl auf die Image-Registry in Ihrem Cluster zu:
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Suchen Sie mit dem folgenden Befehl nach dem Namen des Repositorys in {{site.data.keyword.cloud_notm}} Private, um das Helm-Diagramm hochzuladen:
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  Wenn dieser Befehl erfolgreich ausgeführt wurde, wird eine Liste der Repositorys in Ihrem Cluster angezeigt. Wählen Sie den Namen des Zielrepositorys aus und speichern Sie ihn. Sie benötigen ihn im folgenden Befehl.

5. Importieren Sie das Helm-Diagramm über die Befehlszeile. Führen Sie den folgenden Befehl in der {{site.data.keyword.cloud_notm}} Private-CLI ausgehend von dem Verzeichnis aus, in dem Sie das bei PPA heruntergeladene Helm-Diagrammpaket gespeichert haben, um das Helm-Diagramm in Ihren {{site.data.keyword.cloud_notm}} Private-Cluster zu importieren.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Ersetzen Sie hierbei die folgenden Werte:
  - `<archive-name>` durch den Namen der heruntergeladenen Datei `.tgz`
  - `<cluster_CA_domain>:8500` durch die Domäne, die Sie zur Anmeldung bei Ihrem {{site.data.keyword.cloud_notm}} Private-Cluster verwenden.
  - `<repo-name>` durch das Helm-Repository, in das Sie das Diagramm hochladen wollen ( mit dem Befehl "cloudctl catalog repos" können Sie Ihre Repositorys auflisten)

  Wenn dieser Befehl erfolgreich ausgeführt wird, werden Informationen ähnlich den Folgenden angezeigt:

  ```  
  Uploading Helm chart(s)
   Processing chart: charts/ibm-blockchain-platform-prod-1.1.0.tgz
   Updating chart values.yaml
   Uploading chart
  Loaded Helm chart
  OK

  Synch charts
  OK

  Archive finished processing
  ```  
  </details>

Klicken Sie auf die Schaltfläche **Katalog** in der {{site.data.keyword.cloud_notm}} Private-Konsole und dann im linken Navigationsfenster auf **Blockchain**. Wenn der Import erfolgreich ausgeführt wurde, wird die Kachel **ibm-blockchain-platform-prod** auf der {{site.data.keyword.cloud_notm}} Private-Seite "Katalog" angezeigt.

## Nächste Schritte
{: #console-helm-install-next-steps}

Nach der Installation des Helm-Diagramms können Sie die Kachel **ibm-blockchain-platform-prod** im {{site.data.keyword.cloud_notm}} Private-Katalog verwenden, um die {{site.data.keyword.blockchainfull_notm}} Platform-Konsole bereitzustellen. Sie müssen einen neuen Zielnamensbereich für die Implementierung erstellen und sicherstellen, dass Ihr Cluster über ausreichende Ressourcen für Ihre {{site.data.keyword.blockchainfull_notm}} Plattform-Komponenten verfügt, bevor Sie die Konfigurationsseite ausfüllen. Weitere Informationen finden Sie unter [{{site.data.keyword.blockchainfull_notm}}-Konsole in {{site.data.keyword.cloud_notm}} Private bereitstellen](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp).
