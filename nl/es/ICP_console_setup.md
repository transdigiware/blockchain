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

# Configuración de {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup}

Antes de desplegar componentes de {{site.data.keyword.blockchainfull}} Platform y crear la red blockchain en
{{site.data.keyword.cloud_notm}} Private, debe configurar {{site.data.keyword.cloud_notm}} Private en su propio entorno.
{:shortdesc}

## Requisitos previos
{: #icp-console-setup-prerequisites}

Complete los requisitos previos siguientes y prepare el entorno para instalar {{site.data.keyword.cloud_notm}} Private.

### Docker
{{site.data.keyword.cloud_notm}} Private requiere que Docker esté instalado. Siga las instrucciones relacionadas en el tema [Instalación de {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} para instalar Docker.

### Valores de {{site.data.keyword.cloud_notm}} Private
Antes de instalar {{site.data.keyword.cloud_notm}} Private, las sugerencias siguientes le resultarán útiles para preparar los nodos para la instalación de {{site.data.keyword.cloud_notm}} Private. Encontrará más requisitos previos de {{site.data.keyword.cloud_notm}} Private en la [documentación de {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Actualización del valor de `vm.max_map_count`
{{site.data.keyword.cloud_notm}} Private utiliza Elastic Search para el registro y la calibración. Para evitar excepciones de falta de memoria, Elastic Search requiere que se configure la propiedad del sistema `vm.max_map_count`. Antes de instalar {{site.data.keyword.cloud_notm}} Private, consulte las [instrucciones de configuración de Elastic Search](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} para configurar esta propiedad en cada nodo. Puede utilizar los mandatos siguientes para establecer esta propiedad de forma permanente:

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Configuración del archivo `/etc/hosts` en cada nodo del clúster

- {{site.data.keyword.cloud_notm}} Private utiliza [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} para gestionar las aplicaciones contenerizadas. El servidor de nombres de dominio (DNS) de Kubernetes fallará si no se configuran nombres de hosts en el archivo
`/etc/hosts` de cada nodo. [Inserte la dirección IP, el nombre de host y el nombre abreviado de cada nodo del clúster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} en el archivo `/etc/hosts` en cada nodo.

- [IPv6 no recibe soporte de {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. Para evitar problemas con el servicio DNS en un clúster de {{site.data.keyword.cloud_notm}} Private, inhabilite los valores de IPv6 en el archivo `/etc/hosts` de cada nodo comentando la línea siguiente con un signo `#` al principio de la línea:
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Recursos necesarios
{: #icp-console-setup-resources}

Asegúrese de que el sistema {{site.data.keyword.cloud_notm}} Private cumple los requisitos mínimos de recursos de hardware para la consola y para los componentes que cree. El número de vCPU/CPU necesarias puede variar en función de la infraestructura, del diseño de la red y de los requisitos de rendimiento. Se puede realizar una aproximación de los requisitos de vCPU/CPU examinando la
[tabla de asignaciones de recursos predeterminadas](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default) para {{site.data.keyword.cloud_notm}}, aunque las asignaciones serán ligeramente distintas en
{{site.data.keyword.cloud_notm}} Private. Sus asignaciones de recursos reales serán visibles en la consola de blockchain al desplegar un nodo.

**Notas:**  

- Un vCPU es un núcleo virtual que se asigna a una
máquina virtual o a un núcleo de procesador físico si el servidor no está particionado
para máquinas virtuales. Debe tener en cuenta los requisitos de vCPU cuando decida el núcleo de procesador virtual (VPC) para su despliegue en {{site.data.keyword.cloud_notm}} Private. VPC es una unidad de medida que determina el coste de licencias de los productos de {{site.data.keyword.IBM_notm}}. Para obtener más información sobre los casos de ejemplo para decidir el VPC, consulte [Núcleo de procesador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- Una vCPU es equivalente a una CPU, que es equivalente a una VPC.

### Consideraciones sobre el almacenamiento
{: #icp-console-setup-storage-considerations}

El diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} utiliza suministro dinámico para suministrar el almacenamiento que utilizará la consola y los componentes de blockchain que va a crear. Antes de desplegar la consola, deberá crear una clase de almacenamiento (storageClass) con una cantidad suficiente de almacenamiento de respaldo para la consola y sus componentes. Necesita proporcionar el nombre de la clase de almacenamiento que ha creado durante la configuración.

- Si utiliza los valores predeterminados, el diagrama de Helm creará una nueva reclamación de volumen persistente con el nombre del release de Helm correspondiente a los datos de la consola.
- Si utiliza volúmenes persistentes de NFS v2/v3, debe habilitar el módulo **Supervisor de estado de NFS para bloqueos del sistema de archivos NFSv2/v3**, también conocido como **rpc-statd**, en el sistema host donde existe el sistema de archivos NFS. Este módulo permite que el sistema de archivos NFS pueda comprobar si existen bloqueos exclusivos en archivos que mantienen otros procesos. Ejecute los mandatos siguientes para habilitar este módulo:

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## Instalación de {{site.data.keyword.cloud_notm}} Private
{: #icp-console-setup-install}

Realice los pasos siguientes para instalar y configurar {{site.data.keyword.cloud_notm}} Private en el entorno.

1. Instale un clúster de [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} de la v3.2.0.

2. Instale la CLI de {{site.data.keyword.cloud_notm}} Private [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} para instalar el diagrama de Helm.

3. Cree un espacio de nombres nuevo y personalizado para el despliegue de {{site.data.keyword.blockchainfull_notm}} Platform. Tenga en cuenta que solo puede desplegar un diagrama de Helm por espacio de nombres, por lo que, si desea que se ejecuten varias instancias de la consola en el mismo clúster, debe asegurarse de utilizar espacios de nombres independientes.

4. Configure las políticas de seguridad y de acceso para el espacio de nombres de destino. Se proporcionan instrucciones en la
[sección siguiente](#icp-console-setup-psp).

Después de instalar {{site.data.keyword.cloud_notm}} Private y enlazar una política de seguridad de pod a un espacio de nombres de destino, puede continuar con la [importación del diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install) en el clúster de {{site.data.keyword.cloud_notm}} Private.

## Requisitos de PodSecurityPolicy
{: #icp-console-setup-psp}

El diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} requiere que se enlacen políticas de seguridad y de acceso específicas
con el espacio de nombres de destino antes de la instalación. Se proporcionan los archivos YAML que definen las políticas en los pasos siguientes. Puede guardar estos archivos en su sistema local y, a continuación, enlazarlos a su espacio de nombres utilizando la CLI de
{{site.data.keyword.cloud_notm}} Private. Siga los pasos siguientes antes de desplegar el diagrama de Helm de
{{site.data.keyword.blockchainfull_notm}}.

1. Guarde el archivo siguiente que define la política de seguridad de pod (PodSecurityPolicy) de
{{site.data.keyword.blockchainfull_notm}} Platform como
`ibm-blockchain-platform-psp.yaml` en el sistema local:

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

2. Guarde el archivo siguiente que define el rol de clúster (ClusterRole) necesario para la política de seguridad de pod como
`ibm-blockchain-platform-clusterrole.yaml`:

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

3. Guarde el archivo siguiente que define el enlace de rol de clúster (ClusterRoleBinding) como
`ibm-blockchain-platform-clusterrolebinding.yaml`. Si decide cambiar el nombre de cuenta de servicio en el archivo siguiente, debe proporcionar el nombre en el campo `Nombre de cuenta de servicio` de la sección
**Todos los parámetros** de la página de configuración al desplegar el diagrama de Helm.

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

Una vez que haya guardado los archivos YAML PodSecurityPolicy, ClusterRole y ClusterRoleBinding en su sistema local, un administrador de clúster deberá utilizar la CLI de {{site.data.keyword.cloud_notm}} Private para enlazar las políticas con el espacio de nombres.

1. Inicie sesión en el clúster de {{site.data.keyword.cloud_notm}} y seleccione el espacio de nombres de destino de su despliegue.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Inicie sesión en el registro de imágenes de Docker del clúster:

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Utilice los mandatos siguientes para aplicar las políticas a su espacio de nombres de destino:

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. Tras aplicar las políticas, debe otorgar a su cuenta de servicio el nivel necesario de permisos para desplegar la consola. Ejecute el mandato siguiente con el nombre del espacio de nombres de destino:

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
