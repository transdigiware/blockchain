---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# Configuración de despliegues de alta disponibilidad de varias regiones
{: #ibp-console-hadr-mr}

La configuración de alta disponibilidad de varias regiones proporciona el mayor grado de cobertura de alta disponibilidad posible. El despliegue de iguales en varias regiones geográficas garantiza que, si una región deja de estar disponible, los iguales de las demás regiones puedan seguir realizando transacciones. Tenga en cuenta que el soporte de alta disponibilidad de varias regiones para las CA y el servicio de ordenación no está disponible actualmente.

## Visión general
{: #ibp-console-hadr-overview}

Al configurar el soporte de alta disponibilidad de varias regiones para los iguales, realizará las tareas siguientes:
- Crear varias instancias de servicio en {{site.data.keyword.cloud}}, cada una enlazada con un clúster Kubernetes en una región distinta.
- Crear los nodos de blockchain en distintas regiones.
- Utilizar la función de exportación/importación de nodos para gestionar los nodos desde una única consola.

## Pasos de configuración
{: #ibp-console-hadr-config}

Para configurar la alta disponibilidad de varias regiones mediante la creación de iguales redundantes para cada organización, realice los pasos siguientes al configurar la red blockchain:

1. Cree tres clústeres Kubernetes de {{site.data.keyword.cloud_notm}} en las regiones que prefiera. Estos clústeres pueden encontrarse en cualquier región que desee, aunque, para obtener un rendimiento alto, deben estar relativamente cerca unos de otros. Por ejemplo, las regiones Costa este de EE.UU., Costa oeste de EE.UU. y Canadá son mejores que las regiones Costa oeste de EE.UU., Londres y Tokio.
2. Despliegue una nueva instancia de {{site.data.keyword.blockchainfull_notm}} Platform y enlácela al clúster de la primera región. A continuación, despliegue otra instancia de {{site.data.keyword.blockchainfull_notm}} Platform y enlácela al clúster de la segunda región. Repita estos pasos para enlazar una tercera instancia del servicio con el clúster de la tercera región. Cuando haya terminado, tendrá tres instancias de
{{site.data.keyword.blockchainfull_notm}} Platform independientes enlazadas con tres clústeres separados, cada uno en una región distinta, y tres consolas independientes.

En esta guía de aprendizaje se presupone que existe un servicio de ordenación con un canal definido al que se pueden unir los iguales.
{: important}

### Paso uno: crear la CA de la organización del igual y los metadatos en el clúster uno
{: #ibp-console-hadr-peerCA}

1. Despliegue una CA en el primer clúster siguiendo las instrucciones de la guía de aprendizaje Crear una red para la
[Creación de la CA de la organización del igual](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA). Anote los valores de **ID de inscripción** y **secreto** de la CA, ya que necesitará proporcionar dichos valores al importar la CA en otros clústeres.
2. Una vez que el indicador de estado del mosaico de la CA sea de color verde, `Running`, siga las instrucciones para
[Registrar identidades con la CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). En dichas instrucciones, se crean dos identidades, una para el igual y otra para la identidad de administrador de la organización del igual.
3. [Cree la definición de MSP de la organización del igual](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) para la organización del igual del primer clúster.
4. [Cree un igual](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) en el primer clúster.
5. [Instale el contrato inteligente](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) en el igual.

Para poder crear una instancia del contrato inteligente en el canal, primero deberá seguir estos pasos para hacer que el igual se una al canal en el servicio de ordenación:
- [Exporte la definición de la organización del igual](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) y compártala con un administrador del servicio de ordenación.
- El administrador del servicio de ordenación debe seguir los pasos para [importar la organización del igual](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) y añadirla al consorcio. También deberá completar los pasos de dichas instrucciones para exportar el servicio de ordenación en un archivo JSON.
- [Importe el archivo JSON del servicio de ordenación](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) en la consola.
- [Una el igual al canal](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Finalmente, si utiliza el descubrimiento de servicios, datos privados y rumores de iguales, el administrador del servicio de ordenación deberá [configurar iguales de ancla](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) en el canal. Para la alta disponibilidad, se recomienda añadir cada igual redundante como igual de ancla. De esta manera, si uno de los iguales deja de estar disponible, los rumores (gossip) entre organizaciones podrán continuar.   

Ahora que ha unido el igual al canal, puede [crear una instancia del contrato inteligente](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) en el canal.

### Paso dos: exportar los metadatos e identidades del clúster uno
{: #ibp-console-hadr-export-meta1}

1. Exporte la definición de CA en un archivo JSON.
   - Abra la CA en el separador **Nodos**.
   - Pulse el icono de descarga para generar un archivo JSON de CA desde la sesión del navegador.
2. Exporte la definición de MSP de la organización del igual en un archivo JSON.
   - Vaya a la definición de MSP en el separador **Organizaciones**.
   - Pulse el icono de descarga en el mosaico.
3. Exporte la identidad de administrador de la organización del igual desde su cartera.
   - Vaya al separador **Cartera**.
   - Pulse sobre la identidad de administrador de la organización del igual y, a continuación, pulse **Exportar identidad**.
   - Se crea un archivo JSON que contiene los certificados de administración de la organización. Anote el nombre del archivo y proteja el archivo, ya que será necesario al crear iguales adicionales en los otros clústeres.

### Paso tres: importar los metadatos e identidades en los clústeres dos y tres
{: #ibp-console-hadr-import-meta23}

1. En los clústeres dos y tres, [importe el archivo JSON de la CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca) que ha exportado desde el clúster uno.  
2. Tras cargar el archivo JSON, deberá especificar el ID de inscripción y el secreto del administrador de CA, y el ID de inscripción y el secreto del administrador de CA de TLS que haya utilizado al desplegar la CA en el clúster uno.
2. Importe el archivo JSON de definición de MSP de la organización del igual que ha exportado del clúster uno.
   - En el separador **Organizaciones**, pulse **Importar definición de MSP**.
   - Pulse **Añadir archivo** para cargar el archivo JSON.
   - Pulse sobre el recuadro de selección **Tengo una identidad de administrador para la definición de MSP** para poder utilizar esta definición de MSP al crear nuevos iguales en otras zonas.
   - Pulse **Importar definición de MSP**.
3. Importe el archivo JSON de la identidad de administrador de la organización que ha exportado del clúster uno en la cartera de la consola en los clústeres dos y tres.

### Paso cuatro: crear nuevos iguales en los clústeres dos y tres y hacer que se unan a un canal
{: #ibp-console-hadr-create-new-peers}

1. En los clústeres dos y tres, utilice la CA para registrar un nuevo usuario de igual, siguiendo los mismos pasos que ha realizado para registrar la identidad del igual en el clúster uno. Solo necesita crear los usuarios de igual, pero no necesita volver a crear la identidad de administrador de la organización, ya que va a importarla en el siguiente paso.
2. Cree iguales nuevos, utilizando la CA que ha importado desde el clúster uno como CA del igual y utilizando el MSP de organización del igual que ha importado desde el clúster uno como la definición de MSP de la organización del igual.
3. En los clústeres dos y tres, ahora puede repetir los pasos que ha realizado para hacer que los iguales se unan al mismo canal que el igual del clúster uno. 
4. Tras instalar el contrato inteligente en estos iguales redundantes, el libro mayor se sincronizará automáticamente en todos los iguales.
5. Una vez más, si tiene pensado utilizar el descubrimiento de servicios, datos privados y rumores de iguales, deberá hacer que cada igual redundante sea un igual de ancla.  

Ahora, la red está configurada de manera que un error en una región individual no afecte a la carga de trabajo de igual.  

Para maximizar la alta disponibilidad aún más, plantéese utilizar las dos opciones adicionales siguientes:
- Exporte los iguales que ha creado en los clústeres dos y tres e impórtelos en la consola en el clúster uno. A continuación, todo se podrá gestionar desde un solo clúster.
- No obstante, el clúster uno puede fallar. Por lo tanto, para hacer frente a un posible fallo del clúster, puede importar todos los iguales en cada una de las tres consolas. Así, si el clúster que contiene una de las consolas falla, todo se podrá seguir gestionando desde las consolas de los otros dos clústeres.
