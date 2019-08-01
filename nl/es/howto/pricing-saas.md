---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: pricing model, hourly, per hour, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost, billing

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:gif: data-image-type='gif'}
{:pre: .pre}

# Precios de {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}
{: #ibp-saas-pricing}

Esta guía le ayuda a comprender el modelo de precios de los planes de suscripción a {{site.data.keyword.blockchainfull}} Platform para {{site.data.keyword.cloud_notm}}, y cuánto pagará cuando se desarrolle y crezca la red blockchain de componentes de iguales, nodos de ordenación y entidades emisoras de certificados, que se basan en Hyperledger Fabric v1.4.1.
{:shortdesc}

_Este modelo de precios es solo para {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}. Si utiliza el Plan inicial o el Plan empresarial y tiene preguntas acerca del precio, consulte los [precios](/docs/services/blockchain?topic=blockchain-ibp-pricing) del Plan inicial y el Plan empresarial._

## Modelo de precios
{: #ibp-saas-pricing-model}

{{site.data.keyword.blockchainfull_notm}} Platform presenta un nuevo modelo de precios por hora que se basa en el uso de núcleos de procesador virtual (VPC). Este modelo simplificado se basa en la cantidad de CPU (o VPC) que consumen los nodos de {{site.data.keyword.blockchainfull_notm}} Platform en cada hora, con una tarifa plana de **$0,29 USD/VPC-hora**.

Una VPC es una unidad de medida que se utiliza para determinar el coste de la licencia de los productos de {{site.data.keyword.IBM_notm}}. Se basa en el número de núcleos virtuales (vCPU) que están disponibles para el producto. Una vCPU es un núcleo virtual que se asigna a una máquina virtual o a un núcleo de procesador físico. Para fines de estimación del coste de {{site.data.keyword.blockchainfull_notm}} Platform, **1 VPC = 1 CPU = 1 vCPU = 1 Núcleo**.
{:note}

Para obtener una estimación del coste total, recuerde que su red blockchain consta de un clúster Kubernetes de {{site.data.keyword.cloud_notm}} que contiene componentes de {{site.data.keyword.blockchainfull_notm}} Platform y utiliza el almacenamiento que haya elegido. El clúster Kubernetes de {{site.data.keyword.cloud_notm}} y el almacenamiento que elija incurrirán en cargos independientes. No se efectuarán cargos debido al clúster en el que se ejecuta la instancia de Herramientas operativas, también conocida como consola. Consulte el tema [Referencia de la arquitectura](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview-architecture) para ver una ilustración. A continuación se ofrecen más detalles sobre cómo calcular los cargos.

### ¿Es usted un desarrollador?
{: #ibp-saas-pricing-developer}

Los desarrolladores pueden comenzar con nuestra [extensión para VS Code](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} gratuita. Utilice este entorno de desarrollador integrado para escribir, probar, depurar y empaquetar contratos inteligentes de manera local y para el despliegue de {{site.data.keyword.blockchainfull_notm}} Platform, así como para escribir aplicaciones de cliente. Empiece desde cero o acceda a guías de aprendizaje y ejemplos para aprender los aspectos básicos de blockchain. A continuación, vuelva y enlace una instancia de servicio de {{site.data.keyword.blockchainfull_notm}} Platform con el clúster de Kubernetes para que pueda crear la red blockchain mediante la consola.

## Ventajas del nuevo modelo de precios
{: #ibp-saas-pricing-benefits}

- **Sin cuotas de pertenencia**: quedar libre de cuotas de pertenencia implica que puede invertir directamente en los componentes de blockchain.
- **Claridad en la estimación**: un modelo de precios por horas simple hace que la estimación del coste sea más clara y sencilla, utilizando la herramienta de estimación de costes disponible en el panel de control de {{site.data.keyword.cloud_notm}}.
- **Sin cuota mínima necesaria**: pague solo por lo que use, sin que sea necesario un paquete por horas de VPC mínimo, lo que hace que el inicio sea muy económico.
- **Escalabilidad de cálculo**: dispone de la opción de aumentar la escala del cálculo durante periodos de uso máximo o de reducir la escala a una fracción de minuto de la capacidad cuando no se necesite cálculo para minimizar el gasto.  

En resumen, estas características eliminan la complejidad de tener en cuenta las limitaciones de pertenencia o adquirir potencia de cálculo anticipándose a sus necesidades.

## Elementos clave del coste
{: #ibp-saas-pricing-elements}

Debido a que la red blockchain consta de un clúster Kubernetes de {{site.data.keyword.cloud_notm}} que contiene componentes de {{site.data.keyword.blockchainfull_notm}} Platform y utiliza el almacenamiento que haya elegido, cada uno de los elementos siguientes componen el coste total:

- Tarifa plana de **{{site.data.keyword.blockchainfull_notm}} Platform** de $0.29 USD/VPC por hora. Esta tarifa representa la carga de los componentes de blockchain en el clúster de Kubernetes.
- Precios por niveles del clúster del **servicio Kubernetes de {{site.data.keyword.cloud_notm}}**, visibles en {{site.data.keyword.cloud_notm}} al suministrar el clúster de pago. Esto incluye los cargos del cálculo, es decir, CPU y memoria. Se establece el precio de los servicios Kubernetes de {{site.data.keyword.cloud_notm}} sobre un modelo por niveles que se basa en el número de horas de uso al mes. Por lo tanto, al examinar los planes de precios, tenga en cuenta que un uso de 24x7 es equivalente a 720 horas al mes. Consulte la tabla de la [página del catálogo de servicios de Kubernetes](https://cloud.ibm.com/kubernetes/catalog/cluster){: external} para obtener más detalles sobre los precios del clúster.
- Elija el plan de **almacenamiento** que se ajuste a sus necesidades. Consulte el tema [Conceptos básicos del almacenamiento de Kubernetes](/docs/containers?topic=containers-kube_concepts#kube_concepts) para obtener más información sobre las opciones de clase de almacenamiento y cuánto [cuestan](https://www.ibm.com/cloud/file-storage/pricing){: external}. Los nodos de {{site.data.keyword.blockchainfull_notm}} Platform utilizan la clase de almacenamiento predeterminada para el clúster. Al suministrar un clúster Kubernetes en {{site.data.keyword.cloud_notm}}, viene preconfigurado con [almacenamiento de archivos de nivel Bronce](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) como plugin de almacenamiento persistente.

## Ejemplos de precios
{: #ibp-saas-pricing-scenarios}

La tabla siguiente proporciona dos ejemplos de precios con [asignaciones de recursos predeterminadas]( #ibp-saas-pricing-default), a menos que se indique lo contrario.
- El caso de ejemplo **Red de prueba** es adecuado para empezar y para probar contratos inteligentes.
- El caso de ejemplo **Unirse a una red de producción** incluye dos iguales, lo cual se recomienda para la alta disponibilidad, y una entidad emisora de certificados (CA) necesaria para la pertenencia a la organización.
   - Estos iguales se pueden unir a una red de producción de {{site.data.keyword.blockchainfull_notm}} Platform alojada en cualquier otro lugar.
   - Los nodos siempre pueden volver a un estado de utilización mínima (0,001 CPU) cuando no estén en uso para [reducir el coste](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).
   - Debido a que este caso de ejemplo está pensado para un entorno de **producción**:
     - Los recursos de cálculo predeterminados se han duplicado para proporcionar una mayor capacidad.
     - Se ha elegido la clase de almacenamiento [Plata](/docs/containers?topic=containers-file_storage#file_silver){: external} para aumentar el rendimiento.

| Opciones de precios** (1 VPC = 1 CPU)| **Red de prueba** | **Unirse a una red de producción** |
|-|------------|-----------------------------|
| **Asignación de CPU** |  1,65 CPU <br> Incluye: <br> - 1 igual <br> - 2 CA <br> - 1 nodo de ordenación| 4,5 CPU <br> Incluye: <br> - 2 iguales (para HA) <br> **(el doble del cálculo predeterminado)** <br>- 1 CA <br>  |
| **Coste por hora: {{site.data.keyword.blockchainfull_notm}} Platform** | $0,48 USD <br> (1,65 CPU x $0,29 USD/VPC-hora) | $1,31 USD <br> (4,5 CPU x $0,29 USD/VPC-hora) |
| **Coste por hora: clúster Kubernetes de {{site.data.keyword.cloud_notm}}**    | $0,12 USD <br> (Cálculo: 2 x 4 niveles) <br> (Asignación de IP: $16 USD/mes) | $0,46 USD <br> (Cálculo: 8 x 32 niveles) <br> (Asignación de IP: $16 USD/mes) |
| **Coste por hora: almacenamiento** | $0,07 USD <br> 340GB  <br> [Bronce](https://www.ibm.com/cloud/file-storage/pricing){: external} <br>  2 IOPS/GB | $0,13 USD <br> 420GB <br> [Plata](https://www.ibm.com/cloud/file-storage/pricing){: external} <br> 4 IOPS/GB  |
| **Coste total por hora** | **$0,67 USD** | **$1,90 USD**| |
** Obtenga una vista previa de {{site.data.keyword.blockchainfull_notm}} Platform sin coste alguno durante 30 días al enlazar su instancia de servicio de {{site.data.keyword.blockchainfull_notm}} Platform con un clúster Kubernetes gratuito de {{site.data.keyword.cloud_notm}}. El rendimiento estará limitado por el uso, almacenamiento y funcionalidad. {{site.data.keyword.cloud_notm}} suprimirá el clúster Kubernetes después de 30 días y no se podrán migrar los nodos ni los datos de un clúster gratuito a un clúster de pago.  

El coste real variará en función de factores adicionales como la tasa de transacciones, el número de canales que necesite, el tamaño de la carga útil en las transacciones y el número de transacciones simultáneas. Los ejemplos de precios anteriores se basan únicamente en un clúster Kubernetes de una sola zona de {{site.data.keyword.cloud_notm}}.  Si ha elegido un clúster de varias zonas, existen cargos adicionales para las zonas adicionales y el equilibrador de carga de varias zonas adicional.
{:note}

No hay límite en el número de instancias de servicio que puede suministrar y asociar con un clúster Kubernetes individual, pero debe asegurarse de que los recursos adecuados estén disponibles mediante la supervisión del uso de CPU, memoria y almacenamiento para evitar así la interrupción del servicio. Los nodos de {{site.data.keyword.blockchainfull_notm}} Platform no tienen que estar en su propio clúster. Puede tener otros servicios de {{site.data.keyword.cloud_notm}} en ejecución en el clúster en el que se ejecutan los componentes de blockchain, pero deberá asegurarse una vez más de tener los recursos de cálculo y almacenamiento adecuados para satisfacer todos los requisitos de todas las instancias de servicio.  

**¿Está preparado para empezar?** Consulte [Iniciación a {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-get-started-ibp) para ver las opciones. 

## Asignaciones de recursos predeterminadas
{: #ibp-saas-pricing-default}

Los valores de la tabla siguiente resulta útiles para estimar el coste por hora de su red personalizada según la CPU, cálculo y almacenamiento. Estos valores mínimos recomendados son suficientes para comenzar. A medida que supervisa el uso de la red, puede encontrar que los requisitos de recursos y los costes reales variarán en función de cada caso y de los requisitos de seguridad y de disponibilidad.  

| **Componente** (todos los contenedores) | CPU  | Memoria (GB) | Almacenamiento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Igual**                       | 1,1            | 2,4                   | 200 (incluye 100 GB para el igual y 100 GB para CouchDB)|
| **CA**                         | 0,1            | 0,2                    | 20                     |
| **Nodo de ordenación**                    | 0,35           | 0,9                    | 100                    |


## Facturación
{: #ibp-saas-pricing-billing}

La información de facturación y uso de su cuenta de **Pago según uso** está disponible en el panel de control de {{site.data.keyword.cloud_notm}} en el mosaico de [uso](https://cloud.ibm.com/billing/usage). Un servicio de medición toma instantáneas por hora del uso de VPC total de {{site.data.keyword.blockchainfull_notm}} Platform para que la cantidad de uso mensual acumulada se refleje en el mosaico **Uso**.

Al crear un nodo nuevo, es posible que el uso de VPC tarde hasta una hora en reflejarse en el mosaico **Uso** del panel de control de {{site.data.keyword.cloud_notm}}.
{:note}

### Supervisión del uso
{: #ibp-saas-pricing-usage}

Antes de obtener una factura, puede supervisar el coste del clúster Kubernetes y de {{site.data.keyword.blockchainfull_notm}} Platform desde el mosaico **Uso** del panel de control de {{site.data.keyword.cloud_notm}}. El uso de VPC de {{site.data.keyword.blockchainfull_notm}} Platform se evalúa cada hora.  **Estos costes son solo estimaciones.** Los costes reales se reflejan en su factura mensual.

#### Uso de {{site.data.keyword.blockchainfull_notm}} Platform y el servicio Kubernetes

<!--This clip provides a simple example of how to view your charges for an {{site.data.keyword.blockchainfull_notm}} Platform that includes a single CA node.

![Monitoring your usage](../images/usage_monitoring.gif){: gif}
-->

Vaya a **Gestionar** en la parte superior del panel de control de {{site.data.keyword.cloud_notm}}, pulse **Facturación y uso** y, a continuación, pulse **Uso** en el menú de la izquierda. El diagrama circular situado bajo la subsección **Servicios** le ofrece un desglose del coste total por los tipos de ofertas de servicio que haya utilizado y consumido este mes. Utilice este diagrama para entender cómo contribuyen {{site.data.keyword.blockchainfull_notm}} Platform, el servicio Kubernetes y el almacenamiento al coste total.

Al desplazarse hacia abajo, puede ver un desglose similar por **Tipo** y **Coste** en una vista de lista. Puede encontrar "Servicio Kubernetes" y "Blockchain Platform" junto con los otros servicios que haya suministrado en el clúster. Pulse **Ver planes** junto a cada uno de estos elementos para entender el desglose de costes por métrica. Por ejemplo, `VIRTUAL_PROCESSOR_CORE_HOURS` representa el número total de horas de VPC que se han evaluado hasta ahora.  Utilice este valor para ver cuánto se le va a facturar en función de la métrica de precios de **$0,29 USD/VPC-hora**.

#### Cargos de asignación de IP

Cuando suministre un clúster Kubernetes en {{site.data.keyword.cloud_notm}}, se evalúa una tarifa plana mensual para la asignación de IP. Esta tarifa se carga por zona, por lo que, si suministra tres zonas en el clúster, puede multiplicar este cargo por tres. El ejemplo siguiente muestra el cargo para una zona individual.

![Cargos de asignación de IP](../images/ip_allocation_charge.png "Cargos de asignación de IP del clúster Kubernetes")

Este cargo es visible en el separador **Facturas** del mosaico Uso. Pulse sobre el enlace bajo **Siguiente factura recurrente** para ver los cargos de la asignación de IP.

#### Uso de almacenamiento

Si utiliza el almacenamiento de archivos de {{site.data.keyword.cloud_notm}}, el coste se evalúa de manera mensual, por lo que una estimación de los costes de almacenamiento no será visible hasta el final del mes. No obstante, el almacenamiento que ha suministrado durante todo el mes aparecerá como elementos de línea en el mosaico Uso en **Ventas** > **Pedidos**. Mire la columna **Elementos** para ver una descripción del almacenamiento que se ha suministrado dinámicamente al desplegar un nodo de igual, de CA o de ordenación.

## Optimización del coste de los nodos
{: #ibp-saas-pricing-shutdown}

Una de las ventajas principales del modelo de precios de {{site.data.keyword.blockchainfull_notm}} Platform es la posibilidad de minimizar o suprimir los recursos cuando no son necesarios.

- **Cambiar los nodos al estado de utilización mínima**  
  Se puede reducir la escala de la CPU en los nodos individuales a 0,001 CPU para minimizar los cargos por completo. La realización de estas acciones hace que el nodo deje de ser funcional. Cuando se necesite el recurso de cálculo más adelante, puede utilizar la opción de reasignación en la consola de {{site.data.keyword.blockchainfull_notm}} Platform para aumentar la escala hasta el punto que sea necesario. Para obtener más información sobre cómo se pueden reasignar los recursos, consulte [Reasignación de recursos](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources).

- **Suprimir un igual sin utilizar y desplegar uno nuevo cuando sea necesario**  
  Debido a que el libro mayor se almacena en el nodo de ordenación, al desplegar un nuevo igual y hacer que se una a un canal, el igual recibirá una copia del libro mayor distribuido. El inconveniente de este método es que necesita generar nuevos certificados y hacer que el igual se una de nuevo a los canales.

  No se recomienda suprimir nunca un nodo de CA, ya que los datos que incluye nunca se podrán recuperar. Del mismo modo, si solo tiene un único nodo de ordenación, no debe suprimirlo nunca.  
  {:important}

- **Supervisar y ajustar la asignación de recursos en función de sus necesidades**  
  Al supervisar el uso de recursos a lo largo del tiempo, es posible que decida que puede reducir la escala de los recursos asignados a un nodo mientras se sigue garantizando un rendimiento adecuado. Al seguir las instrucciones de [reasignación de recursos](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-reallocate-resources) en la consola, los efectos en el VPC total del nodo se actualizarán y se podrán utilizar para estimar los costes mensuales revisados.
