---

copyright:
  years: 2017, 2019

lastupdated: "2019-06-18"

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# Problemas conocidos
{: #known-issues}

En esta página se describen los problemas conocidos que se pueden encontrar al utilizar el Plan inicial o el Plan empresarial.
{:shortdesc}

Los siguientes problemas ya se han notificado:
- **Todavía no se admite la configuración de una entidad emisora de certificados externa**. Como alternativa, puede generar y subir los certificados de administración a través del Supervisor de red. Para obtener más información, consulte la descripción del [separador "Certificados" de la pantalla "Miembros"](/docs/services/blockchain?topic=blockchain-ibp-dashboard#ibp-dashboard-members) del supervisor de red.
- En el Supervisor de red de una red de Plan inicial, al pulsar **Ver registros** en los nodos que se listan en la pantalla "Visión general", se abrirá la interfaz Registro de Kibana de {{site.data.keyword.cloud}}. **De forma predeterminada, kibana está preconfigurado para mostrar registros de los últimos 30 días de actividad**. Si no ha habido actividad en los últimos 30 días, verá un mensaje que indica que *No se han encontrado resultados*. Para ver otros registros, puede pulsar el icono de temporizador en la esquina superior derecha bajo el nombre de usuario y establecer un rango de tiempo más amplio, por ejemplo *Año hasta la fecha*.
- Los registros de la red del Plan inicial los recopila el [servicio {{site.data.keyword.cloud_notm}} Log Analysis](https://cloud.ibm.com/catalog/services/log-analysis){: external}. De forma predeterminada, los registros los recopila el Plan Lite del servicio Log Analysis. Este plan es gratuito y **solo le permite buscar los primeros 500 MB de sus registros por día**. Si los registros de la red superan los 500 MB, no podrá ver registros nuevos en Kibana. Si la red genera más de 500 MB de registros, puede actualizar a una versión de pago del servicio Log Analysis.
- Dado que el Plan inicial no es un entorno de producción, **las aplicaciones podrían no llegar inmediatamente a un recurso de red**.
  - Si esto sucede, se recomienda como primer paso aumentar los valores de tiempo de espera predeterminados en el SDK de Fabric. Para obtener más información sobre cómo establecer valores de tiempo de espera, consulte [Establecimiento de valores de tiempo de espera en los SDK de Fabric](/docs/services/blockchain?topic=blockchain-best-practices-app#best-practices-app-set-timeout-in-sdk).
  - También puede volver a intentar la solicitud a nivel de aplicación.
- **Los contenedores de código de encadenamiento pueden detenerse a veces** mediante un problema de red en segundo plano y es posible que se deban reconstruir y reiniciar después de que un usuario invoque el código de encadenamiento. Si sucede esto, su código de encadenamiento puede tardar unos minutos en responder.
- Dado a la limitación de recursos en la red del Plan inicial, es decir, 1 CPU y 4 Gi de RAM para cada igual, **puede encontrar un error `REQUEST_TIMEOUT` durante la instanciación del código de encadenamiento**. Si esto ocurre, vuelva a intentar el paso de instanciación. Si el error continúa, puede aumentar el tiempo de espera de la instanciación del código de encadenamiento. En el perfil de conexión, el tiempo de espera de la instanciación del código de encadenamiento se establece en 300 segundos.
  - Si utiliza el valor de tiempo de espera predeterminado en SDK, copie la sección **cliente** del perfil de conexión como se muestra a continuación, lo que establece el tiempo de espera en 300 segundos, y asegúrese de que el SDK lo lee. Tenga en cuenta que para el SDK de nodo, este valor de tiempo de espera del perfil de conexión afecta a todas las llamadas, tales como `invoke` y `queries`.
    ```
    "client": {
       "organization": "Org1",
       "connection": {
           "timeout": {
               "peer": {
                   "endorser": "300"
               }
           }
       }
    },
    ```
    {:codeblock}
  - Si sobrescribe el valor de tiempo de espera de los mandatos de instanciación del código de encadenamiento en los SDK, establézcalo de nuevo en el valor predeterminado o cámbielo a 300 segundos o más.
    - Si utiliza el SDK de nodo, puede cambiar el valor de tiempo de espera del método `sendInstantiateProposal(request, timeout)`. Para obtener más información, consulte [sendInstantiateProposal(request, timeout)](https://fabric-sdk-node.github.io/Channel.html#sendInstantiateProposal){: external}.
    - Si utiliza el SDK de Java, puede cambiar el valor de tiempo de espera del mandato `instantiateProposalRequest.setProposalWaitTime(DEPLOYWAITTIME)`, que se encuentra en el archivo `src/test/java/org/hyperledger/fabric/sdkintegration/End2endIT.java`.

Para obtener soporte y ayuda con la red de {{site.data.keyword.blockchainfull_notm}} Platform en {{site.data.keyword.cloud_notm}}, consulte [Obtención de soporte](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
