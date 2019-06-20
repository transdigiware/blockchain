---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Despliegue de la consola de {{site.data.keyword.blockchainfull_notm}}
{: #console-deploy-icp}

Siga las instrucciones siguientes para desplegar la consola de {{site.data.keyword.blockchainfull}} Platform en el clúster de local {{site.data.keyword.cloud_notm}} Private después de haber instalado el diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

## Recursos necesarios
{: ##console-deploy-icp-resources-required}

Asegúrese de que el sistema {{site.data.keyword.cloud_notm}} Private cumple los requisitos mínimos de recursos de hardware para la consola y para los componentes que cree:

| **Componente** (todos los contenedores) | CPU  | Memoria (GB) | Almacenamiento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Consola** | 1,3 | 2,5 | 10 |
| **Igual** | 1,2 | 2,4 | 200 (incluye 100 GB para el igual y 100 GB para CouchDB)|
| **CA** | 0,1 | 0,2  | 20 |
| **Clasificador** | 0,45 | 0,9 | 100 |

 **Notas:**
 - Un vCPU es un núcleo virtual que se asigna a una
máquina virtual o a un núcleo de procesador físico si el servidor no está particionado
para máquinas virtuales. Debe tener en cuenta los requisitos de vCPU cuando decida el núcleo de procesador virtual (VPC) para su despliegue en {{site.data.keyword.cloud_notm}} Private. VPC es una unidad de medida que determina el coste de licencias de los productos de {{site.data.keyword.IBM_notm}}. Para obtener más información sobre los casos de ejemplo para decidir el VPC, consulte [Núcleo de procesador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Una vCPU es equivalente a una CPU, que es equivalente a una VPC.

## Almacenamiento
{: #console-deploy-icp-storage}

Asegúrese de que ha creado una storageClass con una cantidad suficiente de almacenamiento de respaldo para la consola y para los componentes de blockchain.

- Es necesario que cree una nueva clase de almacenamiento con recursos suficientes para la consola y los componentes que cree. La clase de almacenamiento que proporcione a la consola durante la configuración también se utilizará para almacenar los datos de los componentes.
- Si utiliza los valores predeterminados, el diagrama de Helm creará una nueva reclamación de volumen persistente con el nombre del release de Helm correspondiente a los datos de la consola.
- Si no se utiliza el suministro dinámico, se deben crear [volúmenes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} y se deben configurar con etiquetas que se puedan utilizar para definir mejor el proceso de enlace de la reclamación de volumen persistente (PVC) de Kubernetes.

## Requisitos previos para desplegar la consola
{: #console-deploy-icp-prerequisites}

1. Para poder desplegar la consola de {{site.data.keyword.blockchainfull_notm}} Platform, debe [instalar {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup) e [instalar el diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-helm-install.html#console_helm-install).

2. Debe crear un espacio de nombres nuevo y personalizado para el despliegue de {{site.data.keyword.blockchainfull_notm}} Platform. El espacio de nombres necesita utilizar la [política PodSecurityPolicy necesaria](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install-prereqs-pod-security-requirements). Si tiene previsto crear varias redes blockchain, por ejemplo, va a crear entornos diferentes para desarrollo, transferencia y producción, debe crear un espacio de nombres exclusivo para cada entorno.

3. Recupere el valor de la dirección IP de proxy de clúster de la CA desde la consola de {{site.data.keyword.cloud_notm}} Private. **Nota:** tendrá que ser un [administrador del clúster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} para acceder a su IP de proxy. Inicie sesión en el clúster de {{site.data.keyword.cloud_notm}} Private. En el panel de navegación de la izquierda, pulse **Plataforma** y, a continuación, pulse **Nodos** para ver los nodos que están definidos en el clúster. Pulse sobre el nodo que tenga el rol `proxy` y, a continuación, copie el valor de `Host IP` de la tabla. **Importante:** guarde este valor, ya que lo utilizará cuando configure el campo `Proxy IP` del diagrama de Helm.


Guarde este valor, ya que lo utilizará cuando configure el campo `Proxy IP` del diagrama de Helm.
{:important}

4. Cree una contraseña que utilizará para iniciar la sesión en la consola por primera vez y guárdela dentro de un objeto secreto en {{site.data.keyword.cloud_notm}} Private. Encontrará los pasos para crear el secreto en la [siguiente sección](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp-password-secret).

## Creación de un secreto de contraseña de la consola
{: #console-deploy-icp-password-secret}

Para poder acceder a la consola, debe crear una contraseña predeterminada que utilizará para iniciar la sesión en la consola por primera vez. Cree un [secreto de Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/){: external} para almacenar la contraseña antes de desplegar la consola. Un secreto de Kubernetes le permite proteger y compartir información sin tener que pasarla directamente al despliegue.

1. Cree una contraseña y codifíquela en formato base64. Ejecute el mandato siguiente en una ventana de terminal y sustituya el valor de `password` por el valor que desea utilizar. **Guarde la salida de este mandato**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} Private. En el panel de navegación de la izquierda, pulse **Configuración** y, a continuación, pulse **Secretos**. Pulse el botón **Crear secreto** para abrir un panel emergente que le permite generar un nuevo objeto de secreto.

3. En el separador **General**, complete los campos siguientes:
  - **Nombre:** proporcione un nombre exclusivo dentro del clúster para el secreto. Utilizará este nombre cuando despliegue la consola. El nombre debe estar en minúsculas.
  - **Espacio de nombres:** el espacio de nombres para añadir el secreto. Seleccione el `espacio de nombres` en el que desee desplegar la consola.
  - **Tipo:** especifique el valor `generic`.

4. Deje el separador **Anotaciones** en blanco.

5. En el separador **Datos**, añada el nombre de usuario y la contraseña como pares de clave-valor.
  1. En el primer campo **Nombre**, especifique `password`.
  2. En el primer campo **Valor**, especifique el resultado de `echo -n 'password' | base64` del paso 1.
  3. Pulse **Crear** para construir un nuevo objeto de secreto.

Especifique el nombre del secreto en el campo `Nombre secreto de contraseña de administrador de consola` de la página de configuración cuando despliegue la consola. Tendrá que utilizar el valor que ha codificado en el paso uno para iniciar una sesión en la consola por primera vez. Este valor se convertirá en la contraseña predeterminada de la consola a menos que lo modifique un administrador de la consola. Para obtener más información, consulte [Gestión de usuarios desde la consola](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Creación de un secreto TLS (opcional)
{: #console-deploy-icp-tls-secret}

La consola utiliza TLS para proteger la comunicación entre la consola y los nodos de blockchain. De forma predeterminada, la consola genera sus propios certificados TLS. Sin embargo, tiene la opción de proporcionar su propio certificado TLS y clave. Siga los pasos que se indican a continuación para almacenar los certificados en un [secreto de Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/). A continuación, puede proporcionar estos certificados a la consola especificando el nombre del secreto en el campo de secreto TLS durante la configuración.

1. Convierta el certificado TLS y la clave privada al formato base64. Puede convertir el contenido del certificado o del archivo de claves en una serie base64 ejecutando el mandato siguiente en la máquina local:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  Ejecute este mandato para el certificado TLS y para la clave TLS que lo acompaña. **Guarde** la salida.

2. Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} Private. En el panel de navegación de la izquierda, pulse **Configuración** y, a continuación, pulse **Secretos**. Pulse el botón **Crear secreto** para abrir un panel emergente que le permite generar un nuevo objeto de secreto.

3. En el separador **General**, complete los campos siguientes:
  - **Nombre:** proporcione un nombre exclusivo dentro del clúster para el secreto. Utilizará este nombre para configurar la CA. El nombre debe estar en minúsculas.
  - **Espacio de nombres:** el espacio de nombres para añadir el secreto. Seleccione el `espacio de nombres` en el que desee desplegar la CA.
  - **Tipo:** especifique el valor `kubernetes.io/tls`.

4. Deje el separador **Anotaciones** en blanco.

5. En el separador **Datos**, añada el nombre de usuario y la contraseña como pares de clave-valor.
  1. En el primer campo **Nombre**, especifique `tls.crt`.
  2. En el primer campo **Valor**, especifique la serie del certificado TLS codificado en base64 del paso 1.
  3. Pulse el botón **Añadir datos** para añadir un segundo par de clave-valor.
  4. En el primer campo **Nombre**, especifique `tls.key`.
  5. En el primer campo **Valor**, especifique la serie de la clave TLS codificada en base64 del paso 1.
  6. Pulse **Crear** para construir un nuevo objeto de secreto.

Especifique el nombre del secreto que cree en el campo `Secreto TLS` en la sección de todos los parámetros de la página de configuración cuando despliegue la consola.

Los secretos que no se eliminan del clúster de {{site.data.keyword.cloud_notm}} Private cuando se suprime el release de Helm. Usted es responsable de gestionar las contraseñas y los secretos TLS de su clúster de {{site.data.keyword.cloud_notm}} Private. Si tiene previsto desplegar otra consola en el futuro, puede reutilizar los secretos. De lo contrario, deberá suprimirlos del clúster de {{site.data.keyword.cloud_notm}} Private.
{:note}

## Configuración
{: #console-deploy-icp-configuration}

Después de crear el objeto de secreto TLS, puede desplegar la consola siguiendo los pasos siguientes.

1. Inicie una sesión en la consola de {{site.data.keyword.cloud_notm}} Private y pulse **Catálogo** en la esquina superior derecha.
2. Pulse `Blockchain` en el panel de navegación de la izquierda y localizar el mosaico llamado `ibm-blockchain-platform-prod`. Pulse sobre el mosaico para abrirlo. Debería ver un archivo Readme que incluye información sobre la instalación y la configuración del diagrama de Helm.
3. Pulse el separador **Configuración** de la parte superior del panel o pulse el botón **Configurar** de la esquina inferior derecha.
4. Especifique los valores para los [parámetros de configuración y de seguridad de pod](#icp-peer-deploy-configuration-parms) y acepte el acuerdo de licencia.
5. Vaya a la sección **Parámetros**:
- Puede desplegar la consola utilizando solo los [Parámetros de inicio rápido](#icp-peer-deploy-quickstart-parms). Utilice esta opción si está experimentando o se está poniendo en marcha.
- Puede utilizar la sección [Todos los parámetros](#icp-peer-deploy-quickstart-parms) para personalizar el acceso a la red, los recursos y el almacenamiento utilizado por la consola. La sección **Todos los parámetros** solo se recomienda a los usuarios de Kubernetes más experimentados.
6. Pulse **Instalar**.

En las tablas siguientes se describen los campos de parámetros de configuración y sus valores predeterminados.

### Parámetros de configuración y de seguridad de pod
{: #icp-peer-deploy-configuration-parms}

|  Parámetro     | Descripción    | Valor predeterminado  | Obligatorio |
| --------------|-----------------|-------|------- |
| `Nombre de release de Helm`| Nombre que identifica el despliegue del release de Helm. Debe comenzar por una letra minúscula, terminar por cualquier carácter alfanumérico y solo debe contener guiones y caracteres alfanuméricos en minúsculas. | Ninguno | Sí |
| `Espacio de nombres de destino`| Especifique el espacio de nombres que ha creado para la consola y para los componentes. El espacio de nombres debe incluir una política `ibm-privilegi-psp`. De lo contrario,
[enlace una PodSecurityPolicy](https://cloud.ibm.com/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) con el espacio de nombres. | Ninguna | Sí |
| `Clúster de destino`| Especifique los clústeres en los que desea desplegar el recurso. Los clústeres disponibles están filtrados por el espacio de nombres seleccionado y por los requisitos de versión de Kubernetes. | Ninguno | Sí |
| `Políticas de espacio de nombres de destino`| Se ha configurado previamente para que utilice el espacio de nombres de destino. Los valores deben incluir la política `ibm-privilegi-psp` o `ibm-blockchain-platform-psp`. | Ninguno | Sí |

### Inicio rápido y todos los parámetros
{: #icp-peer-deploy-quickstart-parms}

Utilice la sección de inicio rápido si está experimentando o se está poniendo en marcha. La sección Todos los parámetros le permite personalizar el acceso a la red, los recursos y el almacenamiento solo se recomienda a los usuarios de Kubernetes más experimentados.

**Pulse el separador Todos los parámetros para ver todos los parámetros de configuración.**

|  Parámetro     | Descripción    | Valor predeterminado  | Obligatorio |
| --------------|-----------------|-------|------- |
| **Valores de consola** | **Administración y despliegue de la consola** | | |
| `IP de proxy` | Dirección IP del nodo proxy del clúster. | Ninguno| Sí |
| `Correo electrónico del administrador de la consola` | Correo electrónico utilizado para iniciar la sesión en la consola. | Ninguno | Sí |
| `Nombre del secreto de contraseña del administrador de la consola` | Nombre del secreto que [ha creado para almacenar la contraseña](#console-deploy-icp-password-secret) y que utilizará para iniciar la sesión en la consola. | Ninguno | Sí|
| **Valores de red** | **Acceso de red a la consola** | | |
| `Nombre de host de la consola` | Especifique el mismo valor que la IP del proxy. | Ninguno | Sí |
| `Puerto de consola` | Especifique el puerto que desea utilizar, comprendido entre 31210 y 31220. Ninguna otra aplicación puede utilizar este puerto. | Ninguno | Sí |
| **Valores de almacenamiento** | **Almacenamiento que va a utilizar la consola** | | |
| `Nombre de clase de almacenamiento` | Especifique el nombre de la clase de almacenamiento que va a utilizar la consola y los componentes que cree. | Ninguno | Sí |
{: caption="Tabla 1. Parámetros de inicio rápido" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Parámetro     | Descripción    | Valor predeterminado  | Obligatorio |
| --------------|-----------------|-------|------- |
| `Arquitectura` | Seleccione la arquitectura de la plataforma de la nube. (AMD64 o S390x) | AMD64 | No |
| **Valores de consola** | **Administración y despliegue de la consola** | | |
| `Nombre de cuenta de servicio` | Cuenta de servicio que utilizará el operador. | Ninguno | No |
| `IP de proxy` | Dirección IP del nodo proxy en el clúster. | Ninguno | Sí|
| `Correo electrónico del administrador de la consola` | Correo electrónico utilizado para iniciar sesión en la consola.  | Ninguno | Sí |
| `Nombre del secreto de contraseña del administrador de la consola` | Nombre del secreto que [ha creado para almacenar la contraseña](#console-deploy-icp-password-secret) y que utilizará para iniciar la sesión en la consola. | Ninguno | Sí|
| **Valores de imagen de Docker** | **Utilice estos valores para personalizar las imágenes de Fabric que va a extraer la consola** | | |
| `Nombre de imagePullSecret` | imagePullSecret que se utilizará para descargar imágenes. | `ibp-ibmregistry` | No |
| **Valores de red** | **Acceso de red a la consola** | | |
| `Nombre de host de la consola` | Especifique el mismo valor que la IP del proxy. | Ninguno | Sí |
| `Puerto de consola` | Especifique el puerto que desea utilizar, comprendido entre 31210 y 31220. | Ninguno | Sí |
| `Nombre de host de proxy` | Nombre de host del servidor proxy. Especifique el mismo valor que la IP del proxy. | Ninguno | No |
| `Puerto de proxy` | Especifique el puerto que desea utilizar, comprendido entre 31210 y 31220. Ninguna otra aplicación ni la consola pueden utilizar este puerto. | Ninguno | No |
| `Nombre de host de configtxlator` | Especifique el mismo valor que la IP del proxy. | Ninguno | No |
| `Puerto de configtxlator` | Especifique el puerto que desea utilizar, comprendido entre 31210 y 31220. Ninguna otra aplicación ni la consola pueden utilizar este puerto. | Ninguno | No |
| `Secreto TLS` | Nombre del secreto que [ha creado para almacenar los certificados TLS](#console-deploy-icp-tls-secret) que utilizará la consola. | Ninguno | No |
| **Persistencia de datos**| **Habilitación de la persistencia de datos y del suministro dinámico** | | |
| `Habilitar persistencia de datos`| Marque esta opción para habilitar la posibilidad de guardar permanentemente los datos después de que el clúster se reinicie o falle. Consulte [almacenamiento en Kubernetes](https://kubernetes.io/docs/concepts/storage/) para obtener más información. *Si no se establece en true, todos los datos se perderán cuando se produzca una migración tras error o un reinicio de pod.* | Habilitado | No |
| `Habilitar suministro dinámico`| Marque esta opción para habilitar el suministro dinámico para volúmenes de almacenamiento. | Habilitado | No |
| **Valores de almacenamiento**| **Suministro de almacenamiento para la consola y para las herramientas** | | |
| `Tamaño de reclamación de volumen`| Tamaño de la reclamación de volumen persistente que se va a suministrar. | 10 Gi  | No |
| `Nombre de clase de almacenamiento`| Nombre de la clase de almacenamiento que va a utilizar la consola y los componentes que cree. | Ninguno | Sí |
| `Reclamación de volumen existente`| Especifique el nombre de una reclamación de volumen existente y deje todos los demás campos en blanco. | Ninguno | No |
| `Etiqueta de selector`| [Etiqueta de selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) para la PVC| Ninguno | No |
| `Valor de selector`| [Valor de selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) para la PVC | Ninguno | No |
| `Modalidad de acceso al almacenamiento`| Especifique la [modalidad de acceso](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes){: external} al almacenamiento para la PVC.  | ReadWriteMany | No |
| `Nombre de PVC`| Solo para una reclamación nueva. Especifique un nombre para la nueva PVC. | El valor predeterminado es el nombre del release. | No |
| **Asignar recursos**| **Asignación de recursos a la consola** | | |
| `Límite de CPU de opstools` | Número máximo de CPU que se asignarán al componente opstools. | 500 m | No |
| `Límite de memoria de opstools` | Cantidad máxima de memoria que se asignará al componente opstools. | 1000 Mi | No |
| `Solicitud de CPU de opstools` | Número mínimo de CPU que se asignarán al componente opstools. | 500 m | No |
| `Solicitud de memoria de opstools` | Cantidad mínima de memoria que se asignará al componente opstools.| 1000 Mi | No |
| `Límite de CPU de configtxlator` | Número máximo de CPU que se asignarán a la herramienta configtxlator. | 25 m | No |
| `Límite de memoria de configtxlator` | Cantidad máxima de memoria que se asignará a la herramienta configtxlator. | 50 Mi | No |
| `Solicitud de CPU de configtxlator` | Número mínimo de CPU que se asignarán a la herramienta configtxlator.| 25 m | No |
| `Solicitud de memoria de configtxlator` | Cantidad mínima de memoria que se asignará a la herramienta configtxlator.| 50 Mi | No |
| `Límite de CPU de CouchDB` | Número máximo de CPU que se asignarán a CouchDB. | 500 m | No |
| `Límite de memoria de CouchDB` | Cantidad máxima de memoria que se asignará a CouchDB. | 1000 Mi | No |
| `Solicitud de CPU de CouchDB` | Número mínimo de CPU que se asignarán a CouchDB.| 500 m | 500 m |
| `Solicitud de memoria de CouchDB` | Cantidad mínima de memoria que se asignará a CouchDB.| 1000 Mi | No |
| `Límite de CPU de operator` | Número máximo de CPU que se asignará al componente operator. | 100 m | No |
| `Límite de memoria de operator` | Cantidad máxima de memoria que se asignará al componente de operator. | 200 Mi | No |
| `Solicitud de CPU de operator` | Número mínimo de CPU que se asignará al componente operator. | 100 m | No |
| `Solicitud de memoria de operator` | Cantidad mínima de memoria que se asignará al componente de operator. | 200 Mi | No |
| `Límite de CPU de deployer` | Número máximo de CPU que se asignará al componente deployer. | 100 m | No |
| `Límite de memoria de deployer` | Cantidad máxima de memoria que se asignará al componente deployer. | 200 Mi | No |
| `Solicitud de CPU de deployer` | Número mínimo de CPU que se asignará al componente deployer. | 100 m | No |
| `Solicitud de memoria de deployer` | Cantidad mínima de memoria que se asignará al componente deployer. | 200 Mi | No |
{: caption="Tabla 2. Todos los parámetros" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Utilización de la línea de mandatos de Helm para instalar el release de Helm
{: #console-deploy-icp-helm-cli}

Como alternativa, puede utilizar la CLI de `helm` para instalar el release de Helm. Antes de ejecutar el mandato `helm install`, asegúrese de [añadir el repositorio Helm del clúster al entorno de CLI de Helm](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}.

Puede establecer los parámetros necesarios para la instalación creando un archivo `yaml` y pasándolo al mandato `helm install` siguiente.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Donde:
- `<helm_release name>` representa el nombre que desea que tenga el release de Helm.
- `<helm_chart>` representa el nombre del diagrama de Helm importado en el catálogo.
- `<helm_chart_version>` representa la versión del diagrama de Helm importado en el catálogo.
- `<customvalues.yaml>` es el nombre del archivo yaml que contiene los parámetros de configuración.

Por ejemplo:
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

Puede crear un nuevo archivo `yaml` editando el archivo `values.yaml` incluido en el archivo de archivado descargado, que incluye todos los parámetros necesarios con sus valores predeterminados.

## Verificación de la instalación

Tras completar los parámetros de configuración y pulsar el botón **Instalar**, pulse el botón **Ver release de Helm** para ver el despliegue. Si se ha realizado correctamente, debe aparecer el valor 1 en los campos `DESIRED`, `CURRENT`, `UP TO DATE` y `AVAILABLE` de la tabla Despliegue. Es posible que tenga que pulsar Renovar y esperar a que se actualice la tabla.

Para ver los detalles de su despliegue, vaya a la pantalla de visión general del **Despliegue** y pulse el pod que se ha creado. El despliegue del diagrama de Helm crea cinco contenedores en el clúster:
- **opstools**: la interfaz de usuario de la consola.
- **deployer**: una herramienta que permite que la consola se comunique con los despliegues.
- **configtxlator**: una herramienta que utiliza la consola para leer y crear actualizaciones de canal.
- **couchdb**: una instancia de CouchDB que almacena los datos de la consola, incluida información sobre autorización.
- **operator**: un operador de kubernetes que gestiona los despliegues de componentes.

## Inicio de sesión en la consola

Puede utilizar el navegador para acceder a la consola después de la instalación. Encontrará el URL en la sección **Notas:** de la pantalla de visión general del release de Helm que se abre después del despliegue. Asegúrese de que no está utilizando la versión ESR de Firefox. Si es así, cambie a otro navegador, como Chrome, y vuelva a intentarlo.

En el navegador, debería poder ver la pantalla de inicio de sesión de la consola:
- Como **ID de usuario**, utilice el valor que ha especificado en el campo `Correo electrónico del administrador de la consola` durante la configuración.
- Como **Contraseña**, utilice el valor que ha codificado y guardado en el [secreto de contraseña](#console-deploy-icp-password-secret) y que luego ha pasado a la consola durante la configuración. Esta contraseña se convertirá en la contraseña predeterminada de la consola que utilizan todos los nuevos usuarios para iniciar la sesión en la consola. Después de iniciar una sesión por primera vez, se le solicitará que proporcione una contraseña nueva que puede utilizar para iniciar la sesión en la consola.

El administrador que ha suministrado el diagrama de Helm puede otorgar a otros usuarios acceso a la consola y especificar las operaciones que pueden realizar. Para obtener más información, consulte [Gestión de usuarios desde la consola](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage-users).

## Siguientes pasos
{: #console-deploy-icp-next-steps}

Después de acceder a la consola, verá el separador **nodos** de la interfaz de usuario de la consola. Puede utilizar esta pantalla para desplegar componentes en el clúster local. Consulte la [Guía de aprendizaje de creación de red](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) para empezar a utilizar la consola. También puede utilizar este separador para trabajar con nodos que se han creado en otras nubes. Para obtener más información, consulte [Importación de nodos](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).

Para aprender a gestionar los usuarios que pueden acceder a la consola, a utilizar las API de {{site.data.keyword.blockchainfull_notm}} Platform y a ver los registros de los componentes de la consola y de blockchain, consulte [Administración de la consola en {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage).
