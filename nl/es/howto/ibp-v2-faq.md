---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}

# Preguntas más frecuentes
{: #ibp-v2-faq}

**General**   

- [¿Cuál es la ventaja de utilizar {{site.data.keyword.blockchainfull_notm}} Platform sobre Hyperledger Fabric nativo?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [¿Qué versión de Hyperledger Fabric se está utilizando con {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [¿Qué base de datos utilizan los iguales para su libro mayor?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [¿Qué lenguajes se admiten para los contratos inteligentes?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [¿Se pueden utilizar certificados de entidades emisoras de certificados (CA) que no sean de IBM?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [¿Puedo actualizar a partir de V1.0 a la nueva versión de {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [¿Qué ocurrirá cuando suprima mi servicio de {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [¿Qué regiones están disponibles para el servicio de blockchain que se ejecuta en {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [¿Puedo utilizar mi clúster existente del servicio Kubernetes de {{site.data.keyword.cloud_notm}}?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [¿Tenemos acceso a servicios de registro y qué registros tengo disponibles?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [¿Qué ventajas ofrece {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [¿A qué entornos da soporte {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-environments)
- [¿Cuál es el modelo de precios de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-pricing)
- [¿Qué servicios tengo que instalar para poder utilizar {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-services)
- [¿Cómo puedo calcular los requisitos de dimensionamiento de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud para los entornos de desarrollo, prueba y producción?](#ibp-v2-faq-icp-sizing)
- [¿Qué ocurre con mis componentes de blockchain cuando suprimo mi release de Helm?](#ibp-v2-faq-icp-delete)

## ¿Cuál es la ventaja de utilizar {{site.data.keyword.blockchainfull_notm}} Platform sobre Hyperledger Fabric nativo?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform permite que los clientes puedan desplegar con facilidad una red blockchain personalizada. Puede utilizar una interfaz de usuario de consola intuitiva para desplegar la red, instalar e instanciar contratos inteligentes con facilidad y supervisar las transacciones.

## ¿Qué versión de Hyperledger Fabric se está utilizando con {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} e {{site.data.keyword.blockchainfull_notm}} Platform for Platform for Multicloud utilizan Hyperledger Fabric v1.4.1

## ¿Qué base de datos utilizan los iguales para su libro mayor?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Todos los iguales que se despliegan con {{site.data.keyword.blockchainfull_notm}} Platform utilizan CouchDB como base de datos para el libro mayor.

## ¿Qué lenguajes se admiten para los contratos inteligentes?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform admite contratos inteligentes escritos en Go y Node.js. El nuevo modelo de programación de Hyperledger Fabric solo tiene soporte actualmente para Node.js, pero próximamente se incluirán más. Si está interesado en conservar su código de aplicación existente o en utilizar los SDK de Fabric para otros lenguajes que no sean Node.js, podrá seguir conectándose a la red de {{site.data.keyword.blockchainfull_notm}} Platform utilizando las API del SDK de Fabric de un nivel más bajo.

## ¿Se pueden utilizar certificados de entidades emisoras de certificados (CA) que no sean de IBM?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Sí, puede traer sus propios certificados, siempre que los haya emitido una CA que cumplan con X.509.

## ¿Puedo actualizar a partir de V1.0 a la nueva versión de {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

No para el Plan inicial. No puede actualizar del Plan inicial a la nueva versión de {{site.data.keyword.blockchainfull_notm}} Platform.
Para el Plan empresarial, podrá actualizar a la nueva versión de {{site.data.keyword.blockchainfull_notm}} Platform en el futuro. El propietario de la cuenta recibirá un correo electrónico del equipo de {{site.data.keyword.blockchainfull_notm}} Platform con instrucciones de ayuda.

## ¿Qué ocurrirá cuando suprima mi servicio de {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Cuando se suprime una instancia de servicio de {{site.data.keyword.blockchainfull_notm}} Platform, se suprimen todos los nodos de CA, igual y de ordenación de blockchain, junto con su almacenamiento asociado.

## ¿Qué regiones están disponibles para el servicio de blockchain que se ejecuta en {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

Las regiones disponibles para {{site.data.keyword.blockchainfull_notm}} Platform aparecen listadas en [Ubicaciones de {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations). Tenga en cuenta que debe crear un clúster de servicio Kubernetes de {{site.data.keyword.cloud_notm}} en la misma región que el servicio de blockchain para que se reconozca el clúster. Pronto estarán disponibles regiones adicionales.

## ¿Puedo utilizar mi clúster existente del servicio Kubernetes de {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Su clúster de Kubernetes funcionará con {{site.data.keyword.blockchainfull_notm}} Platform siempre que cumpla las siguientes condiciones:
- Ejecuta Kubernetes v1.11 o una versión estable superior.
- Haya suficientes recursos disponibles en el clúster.

## ¿Tenemos acceso a servicios de registro y qué registros tengo disponibles?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Con {{site.data.keyword.blockchainfull_notm}} Platform, ahora puede acceder directamente a los registros del igual, de la CA y del nodo de ordenación desde el panel de control de Kubernetes. Se recomienda que haga uso del servicio LogDNA de {{site.data.keyword.cloud_notm}} que le permite analizar fácilmente los registros en tiempo real.

## ¿Qué ventajas ofrece {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

Consulte este tema sobre [Lo que ofrece {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## ¿A qué entornos da soporte {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud da soporte a todos los entornos a los que da soporte {{site.data.keyword.cloud_notm}} Private v3.2. Consulte [Sistemas operativos y plataformas soportados](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external} para obtener más información.

## ¿Cuál es el modelo de precios de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-pricing}
{: faq}

El [sistema de licencias](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud se basa en la cantidad de CPU (VPC) disponible para el producto. Para obtener más información, [póngase en contacto con IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## ¿Qué servicios tengo que instalar para poder utilizar {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-services}
{: faq}

Solo tiene que instalar {{site.data.keyword.cloud_notm}} Private v3.2.

## ¿Cómo puedo calcular los requisitos de dimensionamiento de {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud para los entornos de desarrollo, prueba y producción?
{: #ibp-v2-faq-icp-sizing}
{: faq}

Cuando sepa cuántos nodos de CA, de igual y de ordenación necesita, puede examinar la [tabla de asignaciones predeterminadas de recursos](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) para obtener una estimación de los parques de datos de CPU (VPC) que necesita para la red.

## ¿Qué ocurre con mis componentes de blockchain cuando suprimo mi release de Helm?
{: #ibp-v2-faq-icp-delete}
{: faq}

Al suprimir un release de Helm del clúster de {{site.data.keyword.cloud_notm}} Private, los componentes de blockchain asociados no se suprimen. Para eliminar correctamente un release de Helm del clúster, debe suprimir todos los componentes utilizando la consola de blockchain o las API de blockchain en primer lugar. A continuación, puede suprimir el diagrama de Helm.  
