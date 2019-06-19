---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Iniciación a {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform se puede desplegar en nubes públicas y privadas, como {{site.data.keyword.cloud_notm}}, su propio centro de datos y nubes públicas de terceros. Este despliegue multinube es posible a través de {{site.data.keyword.cloud_notm}} Private, la plataforma de organización de contenedores basada en Kubernetes de {{site.data.keyword.IBM_notm}}. Puede utilizar esta oferta para instalar la consola de {{site.data.keyword.blockchainfull_notm}} Platform en un despliegue de {{site.data.keyword.cloud_notm}} Private y luego utilizar la consola para crear componentes de {{site.data.keyword.blockchainfull_notm}} en el clúster local. También puede utilizar la consola para trabajar con una red de varias nubes distribuidas mediante la importación de nodos desplegados en otros clústeres de {{site.data.keyword.cloud_notm}} Private o en {{site.data.keyword.cloud_notm}}.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}} Private es un producto empaquetado para {{site.data.keyword.cloud_notm}} Private. Para obtener más información acerca de {{site.data.keyword.cloud_notm}} Private, consulte la documentación de [{{site.data.keyword.cloud_notm}} Private versión 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Paso uno: Configurar un clúster de Kubernetes en {{site.data.keyword.cloud_notm}} Private
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

El primer paso consiste en configurar un clúster de Kubernetes en {{site.data.keyword.cloud_notm}} Private en la plataforma de nube que elija.
Consulte [Configuración de {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_setup.html#icp-setup) para ver sugerencias y recomendaciones.

Si va a crear una red que se utilizará en producción, tiene que configurar el despliegue del clúster y la cadena blockchain con alta disponibilidad:

- Para ver recomendaciones sobre cómo configurar el clúster, visite el tema sobre [Implementación de alta disponibilidad en {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- Cuando esté listo para empezar a crear su red, visite [Consideraciones sobre la alta disponibilidad para los iguales](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-peers) y [Consideraciones sobre la alta disponibilidad para servicios de ordenación](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-ordering-service).

## Paso dos: Instalar {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private se suministra como un diagrama de Helm que se puede descargar desde Passport Advantage (PPA). Para obtener información sobre cómo instalar el diagrama de Helm en el clúster local, visite [Instalación de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install).

## Paso tres: Desplegar la consola de {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-three-deploy-console}

Después de instalar el diagrama de Helm, puede pulsar el mosaico de la aplicación {{site.data.keyword.blockchainfull_notm}} Platform en la página Catálogo para instalar una consola de {{site.data.keyword.blockchainfull_notm}} Platform en el clúster local. Para obtener información sobre cómo configurar la consola, así como los recursos necesarios para los componentes de blockchain, consulte [Despliegue de la consola de {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).

## Paso cuatro: Añadir usuarios a la consola como administrador
{: #get-started-console-icp-step-four-add-console-admin}

El administrador de la consola puede iniciar una sesión en la consola utilizando la dirección de correo electrónico y la contraseña que se proporcionaron durante el despliegue. La contraseña proporcionada se convierte en la contraseña predeterminada de la consola y la utilizan todos los usuarios nuevos para iniciar una sesión en la consola por primera vez. Luego el administrador puede añadir nuevos usuarios a la consola, permitiendo que otros inicien una sesión y comiencen a trabajar con los nodos de {{site.data.keyword.blockchainfull_notm}}. El administrador también puede establecer una nueva contraseña predeterminada. Para obtener más información, consulte [Gestión de usuarios desde la consola](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Paso cinco: Utilizar la consola para crear los componentes
{: #get-started-console-icp-build-network}

Cuando haya desplegado la consola, puede utilizarla para crear, hacer funcionar y controlar los componentes de {{site.data.keyword.blockchainfull_notm}} en el clúster local. Para empezar a utilizar la interfaz de usuario de la consola, consulte la [Guía de aprendizaje sobre creación de una red](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network).

## Paso seis: Conectar redes entre nubes
{: #get-started-console-icp-import-nodes}

Puede utilizar la consola para trabajar con los componentes que se ejecutan en otros clústeres de {{site.data.keyword.cloud_notm}} Private o en {{site.data.keyword.cloud_notm}}. En primer lugar, tendrá que exportar la información de los componentes a un archivo JSON desde la consola en la que se desplegó originalmente el componente. A continuación, puede importar el archivo JSON de nodo en la consola desplegada en el clúster local y gestionar los componentes entre nubes. Para obtener más información, consulte [Importación de nodos](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).
