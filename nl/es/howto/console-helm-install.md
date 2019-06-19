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

# Instalación del diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform para {{site.data.keyword.cloud_notm}} Private se entrega como un diagrama de Helm que se puede instalar en un clúster de {{site.data.keyword.cloud_notm}} Private local. Después de instalar el diagrama de Helm, encontrará {{site.data.keyword.blockchainfull_notm}} Platform como una plataforma en el catálogo de {{site.data.keyword.cloud_notm}} Private.

El diagrama de Helm se debe adquirir a través de [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Al realizar la compra, se incluye soporte técnico para la plataforma {{site.data.keyword.blockchainfull_notm}}.

Antes de instalar {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private, revise las
[Consideraciones y limitaciones](/docs/services/blockchain/console-icp-about.html#console-icp-about-considerations). Para obtener más información sobre precios y soporte y para ver consideraciones sobre la seguridad y la residencia de los datos, consulte el apartado [Acerca de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/console-icp-about.html#console-icp-about).

## Requisitos previos para la instalación del diagrama de Helm
{: #console-helm-install-prereqs}

Antes de instalar el diagrama de Helm, debe haber configurado un clúster de {{site.data.keyword.cloud_notm}} Private y haber creado un nuevo espacio de nombres de destino enlazado a una política de seguridad de pod. Consulte las instrucciones para
[configurar un clúster de {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup). Si tiene previsto crear varias redes blockchain, por ejemplo, va a crear entornos diferentes para desarrollo, transferencia y producción, debe crear un espacio de nombres exclusivo para cada entorno.

### Requisitos de PodSecurityPolicy
{: #console-helm-install-prereqs-pod-security-requirements}

El diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} requiere que se enlace una PodSecurityPolicy con el espacio de nombres de destino antes de la instalación. Elija una política de seguridad de pod (PodSecurityPolicy) predefinida o solicite al administrador del clúster que cree una PodSecurityPolicy personalizada:
- Nombre de PodSecurityPolicy predefinida: [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- Nombre de PodSecurityPolicy personalizada:
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
- ClusterRole (rol de clúster) personalizado para la PodSecurityPolicy personalizada:
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
- ClusterRoleBinding (enlace de rol de clúster) personalizado para el ClusterRole personalizado:
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

## Importación del diagrama de Helm en {{site.data.keyword.cloud_notm}} Private
{: #console-helm-install-importing}

1. Descargue el diagrama de Helm en {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private desde [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}.

2. Si aún no lo ha hecho, inicie sesión en el clúster de {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Asegúrese de configurar la [CLI de Docker](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html). Tras configurar la CLI de Docker, acceda al registro de imágenes del clúster utilizando el mandato siguiente:
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Busque el nombre del repositorio en {{site.data.keyword.cloud_notm}} Private para cargar el diagrama de Helm utilizando el mandato siguiente:
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  Cuando este mandato finalice correctamente, podrá ver una lista de repositorios del clúster. Elija el nombre del repositorio de destino y guárdelo. Necesitará utilizarlo en el mandato siguiente.

5. Importe el diagrama de Helm utilizando la línea de mandatos. Desde el directorio en el que ha almacenado el diagrama de Helm descargado de PPA, ejecute el mandato siguiente en la CLI de {{site.data.keyword.cloud_notm}} Private para importar el diagrama de Helm en el clúster de {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Sustituya los valores siguientes:
  - `<archive-name>` por el nombre del archivo `.tgz` descargado.
  - `<cluster_CA_domain>:8500` por el dominio que utilice para iniciar sesión en el clúster de
{{site.data.keyword.cloud_notm}} Private.
  - `<repo-name>` por el repositorio de Helm donde desee cargar el diagrama. Ejecute 'cloudctl catalog repos' para ver una lista de los repositorios.

  Cuando este mandato finalice correctamente, verá algo parecido a la información siguiente:

  ```  
  Loading Helm chart
  Loaded Helm chart

  Synch charts on repo: <repo-name>
  OK
  ```  
  </details>

Pulse el botón **Catálogo** de la consola de {{site.data.keyword.cloud_notm}} Private y, a continuación, pulse **Blockchain** en el panel de navegación de la izquierda. Si la importación se realiza correctamente, se podrá ver el mosaico **ibm-blockchain-platform-prod** en la página Catálogo de {{site.data.keyword.cloud_notm}} Private.

## Siguientes pasos
{: #console-helm-install-next-steps}

Después de instalar el diagrama de Helm, puede utilizar el mosaico **ibm-blockchain-platform-prod** del catálogo de {{site.data.keyword.cloud_notm}} Private para desplegar la consola de {{site.data.keyword.blockchainfull_notm}} Platform. Debe crear un nuevo espacio de nombres de destino para el despliegue y asegurarse de que el clúster tiene suficientes recursos para los componentes de {{site.data.keyword.blockchainfull_notm}} Platform antes de completar la página de configuración. Para obtener más información, consulte [Despliegue de la consola de {{site.data.keyword.blockchainfull_notm}} en {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).
