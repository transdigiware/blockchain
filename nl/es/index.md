---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Iniciación a {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform proporciona una oferta de blockchain-as-a-service (BaaS) gestionada y de pila completa que le permite desplegar componentes de blockchain en los entornos que elija. El entorno puede ser {{site.data.keyword.cloud_notm}}, local a través de {{site.data.keyword.cloud_notm}} Private y nubes de terceros, como Amazon Web Services (AWS). En esta guía de aprendizaje se describe el proceso general para configurar una red de blockchain básica con {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

Antes de utilizar una oferta de {{site.data.keyword.blockchainfull_notm}}, debe leer la información técnica y de soporte de la sección [Descargo de responsabilidad](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer).
{: important}


## Antes de empezar
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform proporciona distintas ofertas para ayudar a todos los tipos de usuarios a iniciar su viaje en blockchain y mover sus aplicaciones a producción. Debe decidir el entorno y la oferta que va a utilizar. En la figura 1 se muestran las capacidades, las audiencias de destino, los precios y las plataformas de nube para distintas ofertas. Para obtener más información sobre cada oferta, pulse la oferta en la tabla siguiente.

| **Ofertas** | **Qué se incluye** | **Política de facturación** | **Plataforma de nube** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**Extensión de {{site.data.keyword.blockchainfull_notm}} Platform para VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | Los desarrolladores pueden comenzar con el IDE que proporciona un explorador y mandatos accesibles desde la paleta de mandatos para desarrollar contratos inteligentes rápidamente. | Gratuito | Se ejecuta en la máquina local |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) | Consola y API de {{site.data.keyword.blockchainfull_notm}} Platform que se pueden utilizar para desplegar y gestionar componentes de blockchain en el clúster Kubernetes de {{site.data.keyword.cloud_notm}}. | [Precio de VPC de $0,29 USD/VPC-hora](/docs/services/blockchain/howto?topic=blockchain-ibp-saas-pricing) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about) | Consola de {{site.data.keyword.blockchainfull_notm}} Platform desplegada en un clúster de {{site.data.keyword.cloud_notm}} Private mediante un diagrama de Helm de Kubernetes y las API para suministrar y gestionar componentes de blockchain. | [Precios de VPC](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform para AWS**](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws-about#remote-peer-aws-about) | Plantilla de inicio rápido de AWS para desplegar iguales remotos que están fuera de {{site.data.keyword.cloud_notm}}.| Gratuito | AWS |

*Figura 1. Ofertas de {{site.data.keyword.blockchainfull_notm}} Platform*


## Paso uno: obtener una oferta
{: #get-started-ibp-step1}

Asegúrese de que tiene la cuenta de nube o la licencia PPA para obtener una oferta de {{site.data.keyword.blockchainfull_notm}} Platform.

* **Extensión de {{site.data.keyword.blockchainfull_notm}} Platform para VS Code**

  Esta extensión de VS Code está disponible de forma gratuita en el [mercado de Visual Studio](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} y se puede utilizar para desarrollar, depurar y probar contratos inteligentes para su posible despliegue en {{site.data.keyword.blockchainfull_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Esta oferta está disponible en el [panel de control del catálogo de {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog){: external} de {{site.data.keyword.cloud_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Esta oferta se entrega como un diagrama de Helm desplegable a través de [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html).

* **{{site.data.keyword.blockchainfull_notm}} Platform para AWS**

  Esta oferta está disponible en AWS y puede desplegar un igual de blockchain en AWS mediante la [plantilla de inicio rápido](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Paso dos: desplegar {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Inicie la sesión en {{site.data.keyword.cloud_notm}} y cree una instancia de servicio con la oferta. Siga el asistente para completar la configuración inicial de la red. Para obtener más información, consulte [Iniciación al servicio Kubernetes de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Antes de desplegar una red, debe instalar el diagrama de Helm en un clúster de {{site.data.keyword.cloud_notm}} Private. A continuación, puede desplegar la consola de {{site.data.keyword.blockchainfull_notm}} Platform y utilizarla para desplegar y trabajar con los componentes de blockchain en el clúster local. Para obtener más información, consulte [Iniciación a {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp).

* **{{site.data.keyword.blockchainfull_notm}} Platform para AWS**

  Esta oferta está disponible en AWS y puede desplegar un igual de blockchain en AWS mediante la [plantilla de inicio rápido](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Siguientes pasos
{: #get-started-ibp-next-steps}

Después de desplegar {{site.data.keyword.blockchainfull_notm}} Platform en el entorno que elija, puede configurar la red con consorcio, nodos, canales y contratos inteligentes e iniciar las transacciones en la misma. Para obtener más información, consulte los temas de la tarea bajo la sección **CÓMO SE HACE** en el panel de navegación de la izquierda de esta documentación.

## Obtención de soporte
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}} ofrece diversas opciones de soporte en las soluciones de blockchain implementadas por {{site.data.keyword.IBM_notm}}. Para obtener más información sobre el soporte de {{site.data.keyword.blockchainfull_notm}} Platform, consulte [Obtención de soporte](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).
