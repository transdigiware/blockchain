---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# Acerca de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform se puede desplegar en nubes públicas y privadas, como {{site.data.keyword.cloud_notm}}, su propio centro de datos y nubes públicas de terceros. Este despliegue multinube es posible a través de {{site.data.keyword.cloud_notm}} Private, la plataforma de organización de contenedores basada en Kubernetes de IBM. Este release proporciona una interfaz de usuario de consola de blockchain que puede utilizar para desplegar y gestionar componentes de blockchain en un clúster de {{site.data.keyword.cloud_notm}} Private. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private utiliza la base de código de Hyperledger Fabric v1.4.1 y permite el despliegue en {{site.data.keyword.cloud_notm}} Private v3.2.0.
{:shortdesc}

## Qué ofrece {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-offers}

Puede utilizar esta oferta para instalar la consola de {{site.data.keyword.blockchainfull_notm}} Platform en un despliegue de {{site.data.keyword.cloud_notm}} Private. Luego puede utilizar la consola para crear todos los componentes fundamentales de un blockchain de Fabric Manager Hyperledger, una entidad emisora de certificados, un servicio de ordenación e iguales, en el clúster local. Para obtener más información sobre los bloques de creación de las redes de Hyperledger Fabric, consulte [Visión general de componentes de Blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview). También puede utilizar la consola para trabajar con una red de varias nubes distribuidas mediante la importación de nodos desplegados en otros clústeres de {{site.data.keyword.cloud_notm}} Private o en {{site.data.keyword.cloud_notm}}.

Este release de {{site.data.keyword.blockchainfull_notm}} Platform incluye las siguientes características principales:

**CREAR ---- Experiencia integrada del desarrollador**
- **Codifique fácilmente** sus contratos inteligentes en Node.js, Golang o Java, escriba aplicaciones cliente con la nueva extensión de VS Code de {{site.data.keyword.blockchainfull_notm}}, aproveche la **integración de SDK** con la consola de la interfaz y aprenda con nuestras completas guías de aprendizaje y ejemplos.
- **DevOps simplificado** le permite pasar de la fase de desarrollo a la de prueba y producción en un solo entorno mediante la ampliación de los recursos de Kubernetes para añadir más componentes.
- **Integración del servicio Kubernetes.** Aproveche servicios como Grafana y Prometheus para el registro y Kibana para la supervisión.
- **Características principales de Fabric actualizadas**. Aproveche las características más recientes de Hyperledger Fabric v1.4.1:
  - [Servicio de ordenación Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Recopilaciones de datos privados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) que mejoran la privacidad de los datos al garantizar que los datos del libro mayor solo se comparten entre iguales autorizados mediante el protocolo gossip.
  - [Service Discovery](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, que le permite descubrir y actualizar de forma dinámica la forma en que la aplicación interactúa con la red.
  - [Listas de control de acceso de canal](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external}, que le ofrecen un control adicional sobre los canales y los contratos inteligentes.

**OPERAR --- Control total de los despliegues**
- **Despliegue solo los componentes que necesite**. Conecte un igual a varios canales y redes, o aloje un servicio de ordenación al que pueden conectarse los socios de la empresa.
- **Mantenga un control completo de sus identidades**. Almacene y gestione las claves que se utilizan para administrar los nodos en su propio entorno seguro.
- **Operaciones unificadas**. La consola de {{site.data.keyword.blockchainfull_notm}} Platform le permite desplegar y gestionar todas las organizaciones y nodos en **una consola** sin tener que depender de {{site.data.keyword.IBM_notm}} ni de otros proveedores para gestionar los nodos de ordenación o la entidad emisora de certificados. También puede añadir o eliminar miembros de un consorcio de blockchain, crear y unir canales e instalar y crear instancias de contratos inteligentes desde la consola.
- **Aloje o únase a una red**. Despliegue iguales alojados en el clúster en varios canales en varias nubes, o invite a otras organizaciones a unirse a su consorcio o canales mientras las organizaciones gestionan sus nodos de forma independiente entre infraestructuras.
- **Gestione el acceso** de los usuarios que pueden administrar o supervisar los nodos.
- **Acceda directamente a los registros** de los nodos desde el servicio Kubernetes. Utilice cualquier servicio de terceros admitido para extraer y analizar los registros.
- **Interactúe directamente con los pods** mediante el servicio Kubernetes.
- **Recopilación de firmas dinámica**, que le permite un mejor control sobre la gestión de la colaboración a través de configuraciones de canal.

**CRECER --- Escalabilidad y flexibilidad**
- **Elija su capacidad de cálculo**. Tiene flexibilidad para decidir la cantidad de CPU, de memoria y de almacenamiento que desea suministrar en el clúster de Kubernetes. Para obtener más información, consulte [Cómo interactúa la consola con el clúster de Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Escale** al alza o a la baja los recursos del clúster de Kubernetes y pague solo lo que necesite. Para obtener más información, consulte [Tarifas](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Recuperación tras desastre y alta disponibilidad multizona.** Esta prestación duplica el despliegue de Kubernetes entre zonas, ofreciendo alta disponibilidad (HA) de sus componentes y recuperación tras desastre (DR).
- **Ejecución en cualquier lugar**. Gracias al **código base unificado** de la consola de {{site.data.keyword.blockchainfull_notm}} Platform, es posible ejecutar los componentes en cualquier entorno al que dé soporte {{site.data.keyword.cloud_notm}} Private.

{{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private es un producto empaquetado para {{site.data.keyword.cloud_notm}} Private y se suministra como un diagrama de Helm de Kubernetes. Para obtener más información acerca de {{site.data.keyword.cloud_notm}} Private, consulte la documentación de [{{site.data.keyword.cloud_notm}} Private versión 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## ¿Resulta {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private adecuada para usted?
{: #console-icp-about-suitable}

La ejecución de {{site.data.keyword.blockchainfull_notm}} Platform fuera de {{site.data.keyword.cloud_notm}} le ofrece una mayor flexibilidad para crecer o unirse a una red blockchain. Resulta de ayuda para que los iniciadores de red hagan crecer sus redes al permitir la unión de nuevos miembros mientras utilizan la plataforma que han elegido. Permitirá que las organizaciones interesadas en unirse a redes blockchain coloquen sus iguales con sus aplicaciones existentes o los integren con sus sistemas de registro.

Los usuarios de esta oferta gestionarán su propia seguridad e infraestructura. {{site.data.keyword.cloud_notm}} no ofrece estos servicios. Revise las [Consideraciones y limitaciones](#console-icp-about-considerations) de la sección siguiente antes de comenzar.

## Consideraciones y limitaciones
{: #console-icp-about-considerations}

Antes de empezar, asegúrese de que comprende las limitaciones siguientes:

- El usuario es responsable de gestionar la supervisión del estado, la seguridad, el registro y el uso de recursos de los componentes de blockchain.
- La consola solo se puede utilizar para crear y controlar componentes basados en Hyperledger Fabric v1.4.1 o superior.
- Solo puede desplegar una consola por espacio de nombres de Kubernetes. Si tiene previsto crear varias redes blockchain, por ejemplo, va a crear entornos diferentes para desarrollo, transferencia y producción, debe crear un espacio de nombres exclusivo para cada entorno.
- Solo puede importar nodos que se hayan exportado de otras consolas de {{site.data.keyword.blockchainfull_notm}} Platform. Para poder utilizar un igual o un nodo de ordenación importado desde la consola, también debe importar la definición de MSP de la organización y la identidad de administrador de la organización del nodo asociado en la consola.
- {{site.data.keyword.blockchainfull_notm}} Platform solo se admite en {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition.  No hay soporte para {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition.
- No puede utilizar {{site.data.keyword.IBM_notm}} Multicloud Manager para instalar el diagrama de Helm de
{{site.data.keyword.blockchainfull_notm}}.

Para ver los entornos a los que da soporte {{site.data.keyword.cloud_notm}} Private, consulte [Entornos soportados](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Requisitos previos del sistema
{: #console-icp-about-prerequisites}

Consulte [Requisitos del sistema](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} para ver la lista de requisitos previos de hardware y software para
{{site.data.keyword.cloud_notm}} Private. No obstante, tenga en cuenta que no hay soporte para
{{site.data.keyword.blockchainfull_notm}} Platform en sistemas Linux en Power (ppc64le) POWER8.

El diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private se ha validado para su ejecución en clústeres de {{site.data.keyword.cloud_notm}} Private v3.2 en Ubuntu Linux utilizando los nodos trabajadores y el almacenamiento de reserva siguientes:

- **LinuxONE e {{site.data.keyword.IBM_notm}} Z**: z/VM y KVM, que utilizan NFS.
- **x86**: Linux de 64 bits que utiliza GlusterFS.

## Licencia
{: #console-icp-about-license}

Consulte [Precios de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) para obtener más información sobre licencias y precios.

## Instalación de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private se entrega como un archivo de diagrama de Helm que se puede instalar en un clúster de {{site.data.keyword.cloud_notm}} Private local. Después de instalar el diagrama de Helm, encontrará {{site.data.keyword.blockchainfull_notm}} Platform como una plataforma en el catálogo de {{site.data.keyword.cloud_notm}} Private. Para obtener instrucciones sobre cómo instalar el diagrama de Helm y los requisitos previos necesarios, consulte [Instalación de {{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install).

Si es un usuario nuevo de {{site.data.keyword.cloud_notm}} Private y desea información y sugerencias sobre cómo instalar y desplegar {{site.data.keyword.cloud_notm}} Private, consulte [Configuración de {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

Puede desplegar {{site.data.keyword.blockchainfull_notm}} Platform detrás de un cortafuegos, sin tener que acceder a Internet público. El paquete del diagrama de Helm descargado incluye todas las imágenes de Docker de los componentes de Fabric que utiliza la plataforma {{site.data.keyword.blockchainfull_notm}}, sin obtenerlos de DockerHub durante el despliegue.

## Consideraciones sobre seguridad
{: #console-icp-about-security}

Debido a que estos componentes se despliegan en su propia infraestructura, tendrá la responsabilidad de gestionar su seguridad. Esto incluye áreas importantes de seguridad, como la gestión de claves y el cifrado de datos. Examine los temas siguientes cuando tenga en cuenta la seguridad para los componentes.

### Seguridad de datos
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} utiliza el cifrado de disco completo basado en el [cifrado de claves simétricas](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} para proteger todos los datos creados por cada instancia de servicio. Debe seguir pasos similares en su propio entorno para proteger los datos de los iguales.

Los datos de la base de datos de estado, independientemente de si utiliza LevelDB o CouchDB, no se cifran. Puede utilizar un cifrado a nivel de aplicación para proteger los datos en reposo en la base de datos de estado.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### Gestión de claves
{: #console-icp-about-security-key-management}

La gestión de claves es un aspecto crítico de la seguridad. Si una clave privada se ve comprometida o se pierde, es posible que usuarios hostiles accedan a los datos y funciones. {{site.data.keyword.IBM_notm}} utiliza dispositivos físicos conocidos como [Módulos de seguridad de hardware](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) para almacenar las claves privadas de las redes del Plan empresarial de la plataforma {{site.data.keyword.blockchainfull_notm}}.

Usted tiene la responsabilidad de gestionar sus claves privadas. Aunque puede utilizar la interfaz de usuario de la consola de
{{site.data.keyword.blockchainfull_notm}} Platform para generar claves privadas, la consola no almacena dichas claves, ni tampoco se almacenan dentro de {{site.data.keyword.cloud_notm}} Private. Es de vital importancia que almacene sus claves de forma segura para que no se vean comprometidas.

Puede utilizar Key Escrow para recuperar claves privadas perdidas. Esto hay que hacerlo antes de perder ninguna clave. Si una clave privada no se puede recuperar, tiene que obtener nuevas claves privadas registrando una nueva identidad con la entidad emisora de certificados. También debe eliminar y sustituir signCert de cualquier canal al que se haya unido.

### TLS
{: #console-icp-about-security-tls}

[Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) está incorporada en el modelo de confianza de Hyperledger Fabric. Todos los componentes de
{{site.data.keyword.blockchainfull_notm}} Platform utilizan TLS para autenticarse y comunicarse entre sí. Por lo tanto, los nodos de
{{site.data.keyword.cloud_notm}} Private debe poder ser capaces de completar un reconocimiento TLS con otros componentes y con sus aplicaciones. Una implicación de este enfoque es que necesita activar el paso a través, usando por ejemplo una lista blanca, en el cortafuegos de las apps cliente a sus nodos.

### Seguridad de las aplicaciones
{: #console-icp-about-security-appl}

Puesto que todas las invocaciones de código de encadenamiento están firmadas, Fabric gestiona la seguridad de las aplicaciones. Además, Fabric también incluye comprobaciones de nivel de aplicación basadas en ACL.

## Obtención de soporte
{: #console-icp-about-support}

Para obtener más información sobre cómo obtener soporte en {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, recursos gratuitos del desarrollador de blockchain y foros de soporte que puede utilizar para solucionar problemas, consulte [Obtención de soporte](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Si ha adquirido una licencia de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private y desea ponerse en contacto con el servicio de atención al cliente, consulte la información sobre [acceso a la comunidad de soporte de {{site.data.keyword.IBM_notm}} y apertura de una incidencia de soporte](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
