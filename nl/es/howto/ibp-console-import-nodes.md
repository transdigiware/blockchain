---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Importación de nodos
{: #ibp-console-import-nodes}

La consola incluye la opción de importar nodos que se crean utilizando otra consola de {{site.data.keyword.blockchainfull}} Platform.
{:shortdesc}  

**Audiencia de destino:** este tema está diseñado para los operadores de red responsables de crear, supervisar y gestionar la red blockchain.

## ¿Por qué importar un nodo?
{: #ibp-console-import-nodes-why}

Importe CA, servicios de ordenación e iguales desde otras consolas de {{site.data.keyword.blockchainfull_notm}} Platform cuando necesite utilizarlos además de los nodos ya existentes en su consola. Por ejemplo, puede utilizar la opción de importación de un igual si desea unir iguales de organizaciones externas a su consola a canales de su consola. Como los servicios de {{site.data.keyword.blockchainfull_notm}} Platform se despliegan entre varios entornos, es probable que algunas instancias de servicio solo incluyan CA e iguales, mientras que otras alojen servicios de ordenación. La importación de distintos nodos en la consola proporciona una forma de ver y trabajar con estos componentes distribuidos desde una sola interfaz de usuario para que puedan realizar transacciones conjuntamente en blockchain.

Después de importar los nodos en la consola, dispondrá de un potente conjunto de funciones operativas, como por ejemplo:
- Añadir nuevas organizaciones al consorcio
- Crear nuevos canales
- Unir iguales a canales
- Instalar contratos inteligentes en iguales, independientemente del lugar en el que se hayan desplegado
- Crear instancias de contratos inteligentes en canales
- Actualizar contratos inteligentes en canales

Cuando importe un nodo, debe proporcionar su información de conexión. Cuando importe un componente, en realidad no importa el componente físico propiamente dicho, sino que se utiliza la información de conexión para crear una representación del componente en la consola de modo que se pueda utilizar desde la consola. Del mismo modo, cuando se suprime un nodo importado de la consola, el propio nodo, que sigue en ejecución en la ubicación en el que se desplegó, no se suprime; solo se elimina de la consola en la que se ha importado.  

Después de importar un nodo en la consola, también puede modificar su información de conexión mediante el separador **Valores** del nodo.

Para poder importar nodos en la consola, deben exportarse desde la consola de {{site.data.keyword.blockchainfull_notm}} Platform donde se han creado. El operador de la red puede simplemente exportar la información de nodo desde la consola a un archivo `JSON`.
{: note}

## Limitaciones
{: #ibp-console-import-limitations}

- No puede importar nodos desde redes del Plan Inicial o del Plan de Empresa.
- Todos los nodos que se van a importar se deben haber desplegado mediante la consola de {{site.data.keyword.blockchainfull_notm}} Platform.
- No puede aplicar parches a nodos que haya importado en la consola.
- No puede suprimir nodos que haya importado en la consola desde el clúster en el que se desplegaron. Solo puede eliminar el nodo de la consola.
- Si va a importar un nodo que está desplegado en {{site.data.keyword.cloud_notm}} Private, debe asegurarse de que el puerto de proxy web gRPC utilizado por el componente se exponga externamente a la consola. Para obtener más información, consulte [Importación de nodos desde {{site.data.keyword.cloud_notm}} Private](#ibp-console-import-icp)

## Empiece aquí: recopilación de certificados o credenciales
{: #ibp-console-import-start-here}

Para poder importar un componente de blockchain existente desde otra instancia de servicio de {{site.data.keyword.blockchainfull_notm}} Platform, tiene que
importar la identidad de administrador del componente en la cartera de la consola. La importación de la identidad en la cartera de la consola simplifica la operación del nodo.  El operador de la red en la que se ha desplegado el nodo debe exportar la identidad de administrador del nodo desde la cartera a un archivo JSON que puede [importar](#ibp-console-import-nodes-admin-identities).  Normalmente, la identidad del administrador del nodo es el administrador de organización de igual o de ordenación que se especificó al crear la definición de MSP de la organización. Ya debería estar en la cartera de la consola en la que reside el nodo.

### Importación de identidades de administrador en la cartera de la consola
{: #ibp-console-import-nodes-admin-identities}

Cada componente de {{site.data.keyword.blockchainfull_notm}} Platform se despliega con su propio certificado para firmas,
el certificado de firma, de un administrador del componente incluida. Cuando el administrador realiza acciones sobre el componente, este certificado para firmas se adjunta a la transacción y se valida en la configuración del componente. Las claves permiten al administrador trabajar con sus componentes, como por ejemplo registrar nuevos usuarios, crear un nuevo canal o instalar contratos inteligentes en los iguales. Si desea utilizar la consola trabajar con un servicio de ordenación o con un igual que se ha creado utilizando otra consola, debe cargar la identidad de administrador asociada en la cartera de la consola. Luego puede asociar el nodo importado con la identidad en la cartera de la consola. Estas identidades se deben exportar desde la consola en la que se han creado y son necesarias cuando se importa el nodo.

Para importar una nueva identidad, abra el separador **Cartera** y pulse **Añadir identidad**. Pulse **Cargar JSON** para examinar el archivo de identidad JSON que ha exportado el operador de la red desde el lugar en el que se ha creado el nodo.

Después de completar el panel **Añadir identidad** y de pulsar Enviar, puede ver la nueva identidad de administrador en la pantalla de visión general de la cartera. Ahora puede hacer referencia a estas identidades cuando importe los componentes CA, igual o servicio de ordenación.

## Importación de una CA
{: #ibp-console-import-ca}

Un nodo de CA es el componente de blockchain que emite certificados a todas las entidades de la red (iguales, servicios de ordenación, clientes, etc.) para que dichas entidades se puedan comunicar, autenticar y, en última instancia, realizar transacciones. Cada organización tiene su propia CA que actúa como su raíz de confianza. Debe añadir sus organizaciones si está uniendo o creando un consorcio de blockchain. Encontrará más información sobre las CA en la [visión general de los componentes de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca).  

Hay varias razones por las que puede desear importar una CA en la consola. Después de importar la CA, puede utilizarla para registrar más usuarios iguales a la organización igual pulsando **Registrar usuario**. O bien, puede utilizar la CA para crear identidades de administrador adicionales para un igual o un servicio de ordenación.

Para importar una CA en la consola de {{site.data.keyword.blockchainfull_notm}} Platform y trabajar con la misma, el operador de red ya debe haber exportado la CA desde {{site.data.keyword.blockchainfull_notm}} Platform en que se había desplegado. El hecho de importar una CA le permite registrar nuevos usuarios e [inscribir identidades](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll).

### Antes de empezar
{: #ibp-console-import-ca-before-you-begin}

- Asegúrese de que ya ha importado el archivo JSON de identidad de administrador de la CA en la cartera de la consola, o bien de que tiene el ID de inscripción de administrador y el secreto tanto para la CA como para la CA de TLS que se especificaron cuando se desplegó originalmente la CA.
- Asegúrese de que el archivo JSON de la CA que se ha exportado desde la consola donde se ha creado está disponible.

### Cómo importar una CA  
{: #ibp-console-import-nodes-howto-ca}

La importación de una CA se realiza desde el separador **Nodos**.
1. Pulse **Añadir entidad emisora de certificados**, seguido de **Importar una entidad emisora de certificados existente** y pulse **Siguiente**.
2. Seleccione la ubicación en la que se desplegó originalmente la CA en la lista desplegable **Ubicación de la entidad emisora de certificados**.
3. Pulse **Añadir archivo** para cargar el archivo JSON de CA que se ha exportado desde la consola en la que se desplegó originalmente.
4. En el panel siguiente, puede especificar el ID de inscripción y el secreto que se utilizó cuando se desplegó la CA.
5. Por último, especifique el ID de inscripción y el secreto para la CA de TLS que se utilizó cuando se desplegó la CA. Cuando se despliega una CA, se despliegan conjuntamente una CA y una CA de TLS. El operador de la red puede utilizar el mismo ID de inscripción y secreto para ambas, o bien puede especificar ID de inscripción y secretos exclusivos para la CA y la CA de TLS cuando se despliega una CA.

Una vez que haya importado la CA en la consola, puede utilizar la CA para crear nuevas identidades y generar los certificados necesarios para trabajar con los componentes y enviar transacciones a la red. Para obtener más información, consulte [Gestión de entidades emisoras de certificados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca).

## Importación de un servicio de ordenación
{: #ibp-console-import-orderer}

Un servicio de ordenación es el componente de blockchain que recopila las transacciones de los miembros de la red, ordena las transacciones y las empaqueta en bloques. Es el enlace común de los consorcios de blockchain y se debe desplegar si va a fundar un consorcio al que se unirán otras organizaciones. Encontrará más información sobre los servicios de ordenación en la [visión general de los componentes de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer).

La importación de un servicio de ordenación en la consola le permite crear nuevos canales para que los iguales realicen transacciones en privado.

### Antes de empezar
{: #ibp-console-import-orderer-before-you-begin}

Para poder importar un servicio de ordenación, debe recopilar la siguiente información:

- Asegúrese de que ya ha [importado el archivo JSON de identidad del administrador del servicio de ordenación](#ibp-console-import-nodes-admin-identities) en la cartera de la consola.
- Asegúrese de que el archivo JSON del servicio de ordenación que se ha exportado desde la consola donde se ha creado está disponible.

### Cómo importar un servicio de ordenación
La importación de un servicio de ordenación se realiza desde el separador **Nodos**.
1. Pulse **Añadir servicio de ordenación**, seguido de **Importar un servicio de ordenación existente** y pulse **Siguiente**.
2. Seleccione la ubicación en la que se desplegó originalmente el servicio de ordenación en la lista desplegable **Ubicación de la entidad emisora de certificados**.
3. Pulse **Añadir archivo** para cargar el archivo JSON del servicio de ordenación que se ha exportado desde la consola en la que se desplegó originalmente. Tenga en cuenta que, si se trata de un servicio de ordenación Raft de cinco nodos, debería tener un solo archivo con la información de conexión de los cinco nodos de ordenación en el archivo.
4. Establezca la identidad de administrador para el servicio de ordenación pulsando **Identidad existente** y seleccionando la identidad de administrador del servicio de ordenación que ha importado en la cartera de la consola.

Una vez que haya importado el servicio de ordenación en la consola, puede añadir nuevos miembros de la organización y seleccionar el servicio de ordenación al crear nuevos canales.

## Importación de un igual
{: #ibp-console-import-peer}

Un nodo igual es el componente blockchain que mantiene un libro mayor y ejecuta un contrato inteligente para realizar las operaciones de consulta y actualización en el libro mayor. Los miembros de la organización poseen y mantienen los iguales.  Cada organización que se une a un consorcio debe desplegar al menos un igual y como mínimo dos para disponer de alta disponibilidad (HA). Encontrará más información sobre los iguales en la [visión general de los componentes de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer).

Después de importar un igual en la consola, puede instalar contratos inteligentes en el igual y unir el igual a otros canales de su blockchain.

**Nota:** si necesita añadir más iguales a la organización igual o crear identidades de administrador adicionales para un igual, tiene que importar la CA del igual y luego utilizar dicha CA para realizar dichas operaciones.

### Antes de empezar
{: #ibp-console-import-peer-before-you-begin}

Antes de importar un igual, debe recopilar la información siguiente:

- Asegúrese de que ya ha [importado el archivo JSON de identidad del administrador del igual](#ibp-console-import-nodes-admin-identities) en la cartera de la consola.
- Asegúrese de que el archivo JSON del igual que se ha exportado desde la consola donde se ha creado está disponible.

### Cómo importar un igual
{: #ibp-console-import-peer-howto}

La importación de un igual se realiza desde el separador **Nodos**.
1. Pulse **Añadir igual**, seguido de **Importar un igual existente** y luego pulse **Siguiente**.
2. Seleccione la ubicación en la que se desplegó originalmente el igual en la lista desplegable **Ubicación de la entidad emisora de certificados**.
3. Pulse **Añadir archivo** para cargar el archivo JSON del igual que se ha exportado desde la consola en la que se desplegó originalmente.
4. Establezca la identidad de administrador para el igual pulsando **Identidad existente** y seleccionando la identidad de administrador del igual que ha importado en la cartera de la consola.  

Después de importar el igual en la consola, puede instalar contratos inteligentes en el igual y unir el igual a los canales de su blockchain.

## Importación de una definición de MSP de organización
{: #ibp-console-import-msp}

Tiene que importar la definición de MSP de una organización si el MSP se ha desplegado mediante otra consola de instancia de servicio de {{site.data.keyword.blockchainfull_notm}} Platform y tiene previsto crear o actualizar un canal que utiliza dicha definición de MSP. Además, si tiene previsto crear un servicio igual o de ordenación que forme parte de un MSP de organización que se ha desplegado en otra consola, debe importar el MSP y la identidad del administrador de MSP asociada.  El operador de red que ha creado el MSP tiene que exportar la definición de MSP de la organización y la identidad de administrador correspondiente a archivos JSON y compartirlos con usted.

- Si tiene previsto crear un servicio igual o de ordenación que forme parte de la organización, importe la identidad de administrador de MSP asociada en la cartera.
- La importación de la definición de MSP de la organización se lleva a cabo desde el separador **Organizaciones**.
- Pulse **Importar definición de MSP** para cargar el archivo JSON
- (Opcional) Si ha importado la identidad de administrador de MSP en su cartera, marque el recuadro de selección `Tengo una identidad de administrador para la definición de MSP`. Si no selecciona esta opción, cuando posteriormente intente crear un servicio igual o de ordenación, esta definición MSP de organización no aparecerá en la lista desplegable de MSP.

## Importación de nodos desde {{site.data.keyword.cloud_notm}} Private
{: #ibp-console-import-icp}

Puede importar nodos que se hayan creado en {{site.data.keyword.cloud_notm}} Private en consolas que se hayan desplegado en otros clústeres de {{site.data.keyword.cloud_notm}} Private o en {{site.data.keyword.cloud_notm}}. No obstante, necesita asegurarse de que el puerto utilizado por el URL de gRPC de los nodos esté expuesto desde fuera del clúster. Si va a desplegar
{{site.data.keyword.cloud_notm}} Private detrás de un cortafuegos, debe habilitar un paso a través, mediante una lista blanca por ejemplo, para permitir que la consola fuera del clúster se pueda comunicar con los nodos.

Como ejemplo, a continuación puede ver el archivo JSON de un igual que se ha exportado desde
{{site.data.keyword.cloud_notm}} Private. Para comunicarse con el igual desde otra consola, debe asegurarse de que el puerto de
`grpcwp_url`, el puerto 32403 en este ejemplo, está abierto al tráfico externo.

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
