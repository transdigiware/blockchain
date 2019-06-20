---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

Asegúrese de que el sistema {{site.data.keyword.cloud_notm}} Private cumple los requisitos de recursos de hardware mínimos para cada componente de tiempo de ejecución de Fabric:

| **Componente** (todos los contenedores) | vCPU  | Memoria (GB) | Almacenamiento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Consola**                    | 1,3            | 2,5                   | 10                     |
| **Igual**                       | 1,2            | 2,4                   | 200 (incluye 100 GB para el igual y 100 GB para CouchDB)|
| **CA**                         | 0,1            | 0,2                   | 20                     |
| **Clasificador**                    | 0,45           | 0,9                   | 100                    |

 **Notas:**
 - Un vCPU es un núcleo virtual que se asigna a una
máquina virtual o a un núcleo de procesador físico si el servidor no está particionado
para máquinas virtuales. Debe tener en cuenta los requisitos de vCPU cuando decida el núcleo de procesador virtual (VPC) para su despliegue en {{site.data.keyword.cloud_notm}} Private. VPC es una unidad de medida que determina el coste de licencias de los productos de {{site.data.keyword.IBM_notm}}. Para obtener más información sobre los casos de ejemplo para decidir el VPC, consulte [Núcleo de procesador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Una vCPU es equivalente a una CPU, que es equivalente a una VPC.

### Consideraciones sobre el almacenamiento
{: #icp-console-setup-storage-considerations}

* Es necesario que cree una nueva clase de almacenamiento con recursos suficientes para la consola y los componentes que cree. La clase de almacenamiento que proporcione a la consola durante la configuración también se utilizará para almacenar los datos de los componentes.
* Si utiliza los valores predeterminados, el diagrama de Helm crea una nueva reclamación de volumen persistente con el nombre del release de Helm correspondiente a los datos de la consola.
* El [suministro dinámico](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/){: external} solo está disponible para los nodos amd64 en {{site.data.keyword.cloud_notm}} Private. Por lo tanto, si el clúster incluye una combinación de nodos trabajadores s390x y amd64, el suministro dinámico no se puede utilizar.
* Si no se utiliza el suministro dinámico, se deben crear [volúmenes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} y se deben configurar con etiquetas que se puedan utilizar para definir mejor el proceso de enlace de la reclamación de volumen persistente (PVC) de Kubernetes.
* Si utiliza volúmenes persistentes de NFS v2/v3, debe habilitar el módulo **Supervisor de estado de NFS para bloqueos del sistema de archivos NFSv2/v3**, también conocido como **rpc-statd**, en el sistema host donde existe el sistema de archivos NFS. Este módulo permite que el sistema de archivos NFS pueda comprobar si existen bloqueos exclusivos en archivos que mantienen otros procesos. Ejecute los mandatos siguientes para habilitar este módulo:

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

3. Configure la política de seguridad de pod para el espacio de nombres de destino. Se proporcionan instrucciones en la
[sección siguiente](#icp-setup-psp).

Después de instalar {{site.data.keyword.cloud_notm}} Private y enlazar una política de seguridad de pod a un espacio de nombres de destino, puede continuar con la [importación del diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install) en el clúster de {{site.data.keyword.cloud_notm}} Private.

## Requisitos de PodSecurityPolicy
{: #icp-console-setup-psp}

El diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} requiere que se enlace una [PodSecurityPolicy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/){: external} con el espacio de nombres de destino antes de la instalación. Elija una política de seguridad de pod (PodSecurityPolicy) predefinida o solicite al administrador del clúster que cree una PodSecurityPolicy personalizada:
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
  {:codeblock}
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
  {:codeblock}

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
  {:codeblock}
