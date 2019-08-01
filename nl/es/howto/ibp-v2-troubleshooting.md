---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Resolución de problemas
{: #ibp-v2-troubleshooting}

Es posible que se produzcan problemas generales al utilizar la consola para gestionar nodos, canales o contratos inteligentes. En muchos de los casos, puede solucionar estos problemas siguiendo unos sencillos pasos.
{:shortdesc}

En este tema se describen problemas comunes que se pueden producir al utilizar la consola de
{{site.data.keyword.blockchainfull_notm}} Platform.

- [Cuando paso el puntero del ratón sobre mi nodo, el estado es
Estado no disponible, ¿qué significa esto?](#ibp-v2-troubleshooting-status-unavailable)
- [Cuando paso el puntero del ratón sobre mi nodo, el estado es
Estado no detectable, ¿qué significa esto?](#ibp-v2-troubleshooting-status-undetectable)
- [¿Por qué fallan mis operaciones de nodo después de crear el igual o el servicio de ordenación?](#ibp-console-build-network-troubleshoot-entry1)
- [¿Por qué obtengo el error `No se puede obtener el canal del sistema` cuando abro mi servicio de ordenación?](#ibp-troubleshoot-ordering-service)
- [¿Por qué no se inicia mi igual?](#ibp-console-build-network-troubleshoot-entry2)
- [¿Por qué ha fallado la instalación, la creación de una instancia o la actualización de mi contrato inteligente?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [¿Por qué el contrato inteligente que he instalado en el igual no aparece en la interfaz de usuario?](#ibp-console-build-network-troubleshoot-missing-sc)
- [¿Cómo puedo ver los registros del contenedor del contrato inteligente?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Mi canal, los contratos inteligentes y las identidades han desaparecido de la consola. ¿Cómo puedo recuperarlos?](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [¿Por qué recibo el error `No se puede autenticar con el ID y el secreto de inscripción que ha especificado` cuando creo una nueva definición de MSP de organización?](#ibp-v2-troubleshooting-create-msp)
- [¿Por qué recibo el error `Se ha producido un error al actualizar el canal` al intentar añadir una organización a mi canal?](#ibp-v2-troubleshooting-update-channel)
- [Mi clúster Kubernetes de {{site.data.keyword.cloud_notm}} ha caducado. ¿Qué quiere decir esto?](#ibp-v2-troubleshooting-cluster-expired)
- [¿Por qué fallan las transacciones que envío desde VS Code?](#ibp-v2-troubleshooting-anchor-peer)
- [Cuando inicio sesión en mi consola, ¿por qué obtengo un error 401 No autorizado?](#ibp-v2-troubleshooting-console-401)
- [¿Por qué los nodos que he desplegado en {{site.data.keyword.cloud_notm}} Private no procesan transacciones y fallan las comprobaciones de estado?](#ibp-v2-troubleshooting-healthchecks)
- [¿Por qué devuelven mis transacciones un error de política de aprobación: el conjunto de firmas no cumple la política?](#ibp-v2-troubleshooting-endorsement-sig-failure)

## Cuando paso el puntero del ratón sobre mi nodo, el estado es
`Estado no disponible`, ¿qué significa esto?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

El estado de nodo en el mosaico para el nodo de CA, de igual o de ordenación está en gris, lo que significa que el estado del nodo no está disponible. Idealmente, al pasar el puntero del ratón sobre cualquier nodo, el estado del nodo debe ser `En ejecución`.
{: tsSymptoms}

Este problema se puede producir si se acaba de crear el nodo y el proceso de despliegue no se ha completado. Si el nodo es una CA, es probable que el nodo no esté en ejecución.
Si el nodo es un igual o un nodo de ordenación, esta condición se produce cuando el comprobador de estado que se ejecuta en los nodos igual o de ordenación no puede contactar con el nodo.  La solicitud de estado puede fallar con un error de agotamiento del tiempo de espera, debido a que el nodo no haya respondido dentro de un periodo de tiempo específico, que el nodo esté inactivo, o que no haya conectividad de red.
{: tsCauses}

Si se trata de un nodo nuevo, espere algunos minutos más para que finalice el despliegue. Si el nodo no es nuevo,
[examine los registros de nodo asociados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) en busca de errores para determinar la causa.
{: tsResolve}

## Cuando paso el puntero del ratón sobre mi nodo, el estado es
`Estado no detectable`, ¿qué significa esto?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

El estado de nodo en el mosaico del nodo igual o de ordenación está en amarillo, lo que implica que el estado del nodo no se puede detectar. Idealmente, al pasar el puntero del ratón sobre cualquier nodo, el estado del nodo debe ser `En ejecución`.
{: tsSymptoms}

Esta condición solo se produce en nodos de igual o de ordenación que se hayan *importado* en la consola y en los que no se puede ejecutar el comprobador de estado. Este estado se produce debido a que no se ha especificado un `operations_url` al importar el nodo. Se necesita un URL de operaciones para que se pueda ejecutar el comprobador de estado del nodo. El propio nodo es probable que esté `En ejecución`, pero, debido a que no se ha especificado un URL de operaciones, no se puede determinar su estado.
{: tsCauses}

Puede resolver este problema realizando los pasos siguientes:
 1. Pulse sobre el mosaico de nodo para abrirlo.
 2. Pulse el icono **Valores**.
 3. Pulse **Asociar identidad** y vea y anote la identidad asociada a este nodo. Pulse **Cancelar** para cerrar este panel.
 4. Pulse **Exportar** para generar un archivo `JSON` para el nodo.
 5. Edite el archivo `JSON` generado y especifique el `operations_url` para el nodo. El valor del
`operations_url` dependerá de cómo se haya configurado y de diversos valores de red. Este valor deberá proporcionarlo el administrador de red que haya desplegado el nodo.
 6. Pulse **Suprimir**. Este paso elimina el nodo importado de la consola, pero no suprime el nodo real.
 7. En el separador **Nodos**, pulse **Añadir igual** o **Añadir servicio de ordenación** seguido de
**Importar un igual existente** o **Importar un servicio de ordenación existente**.
 8. Pulse **Cargar JSON** y explore para seleccionar el archivo JSON que acaba de editar. Pulse **Siguiente**.
 9. Asocie la misma identidad que ha anotado en el paso tres.
 10. Pulse **Añadir igual** o **Añadir servicio de ordenación**.

El comprobador de estado ahora se puede ejecutar en el nodo y notificar el estado del nodo.
{: tsResolve}

## ¿Por qué fallan mis operaciones de nodo después de crear el igual o el servicio de ordenación?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

Es posible que reciba un error al gestionar un nodo existente. Cuando esto ocurre, suele resultar útil consultar los registros del nodo.  

Por ejemplo, cuando intenta trabajar con el nodo, la acción puede fallar.
{: tsSymptoms}

Después de crear un nuevo igual o servicio de ordenación, en función de la configuración del almacenamiento del clúster, se puede tardar unos minutos en que los nodos estén listos para su funcionamiento.
{: tsCauses}

Compruebe el panel de control de Kubernetes y asegúrese de que el estado del igual o del nodo sea `En ejecución`. A continuación, intente de nuevo la acción. Si sigue teniendo problemas después de que se active el nodo, [consulte los registros del nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) para ver si hay errores.  
{: tsResolve}

## ¿Por qué obtengo el error `No se puede obtener el canal del sistema` cuando abro mi servicio de ordenación?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

Después de crear un servicio de ordenación en la consola de {{site.data.keyword.cloud_notm}} Private, el estado es `Running`. Si embargo, al abrir el servicio de ordenación, aparece el error:

```
No se puede obtener el canal del sistema. Si ha asociado una identidad sin privilegios de administración en el nodo del servicio de ordenación, no podrá ver ni gestionar los detalles del servicio de ordenación.
```

{: tsSymptoms}

Esta condición se produce en la consola de blockchain que se ejecuta en {{site.data.keyword.cloud_notm}} Private. En navegadores que no sean Chrome, deberá aceptar un certificado para que la consola se pueda comunicar correctamente con el nodo.
{: tsCauses}

Existen distintas formas de resolver este problema:
1. En las notas del release de Helm, donde se proporciona el URL del navegador de la consola, también hay una nota para acceder a un URL y aceptar el certificado. Vaya a dicho URL y acepte el certificado. Ahora, abra el servicio de ordenación. Ya no debería producirse el error.
2. Si puede utilizar un navegador Chrome, abra la consola de blockchain en Chrome y abra el servicio de ordenación. No se produce el error. Tenga en cuenta que deberá exportar sus identidades desde la cartera de la consola en su navegador que no sea Chrome y luego importarlas en la cartera en el navegador Chrome para que todo siga funcionando.
{: tsResolve}

## ¿Por qué no se inicia mi igual?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

Es posible que experimente este error bajo diversas condiciones.

El registro del igual incluye `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 No se puede ejecutar el igual porque no se puede iniciar la criptografía, la carpeta “/certs/msp” no existe`
{: tsSymptoms}

- Se puede
producir este error bajo las siguientes condiciones:
  - Al crear la definición de MSP de la organización del igual o del servicio de ordenación, ha especificado un ID y un secreto de inscripción que corresponden a una identidad de tipo `peer` y no `client`. Debe ser de tipo `client`.
  - Al crear la definición de MSP de la organización del igual o del servicio de ordenación, ha especificado un ID y un secreto de inscripción que no coinciden con el ID y el secreto de inscripción de la identidad de administración de la organización correspondiente.
  - Al crear el igual o el servicio de ordenación, ha especificado el ID y el secreto de inscripción de una identidad que no es de tipo 'peer'.

- Abra el nodo de CA del igual o del servicio de ordenación y visualice las identidades registradas que se muestran en la tabla **Usuarios registrados**.
- Suprima el igual o el servicio de ordenación y vuelva a crearlo, especificando el ID y el secreto de inscripción correctos.
- Tenga en cuenta que, antes de crear el igual o el servicio de ordenación, debe crear un id de administrador de la organización de tipo 'client'. Asegúrese de especificar el mismo ID que el ID de inscripción al crear la definición de MSP de la organización. Consulte estas instrucciones para [registrar identidades del igual](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) y estas instrucciones para [registrar identidades del clasificador](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## ¿Por qué ha fallado la instalación, la creación de una instancia o la actualización de mi contrato inteligente?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

Es posible que reciba un error al instalar, al crear una instancia o al actualizar un contrato inteligente.  Por ejemplo, cuando intenta instalar un contrato inteligente en un igual, falla con el error `Se ha producido un error al instalar el contrato inteligente en el igual`.
{: tsSymptoms}

Puede recibir este error si esta versión del contrato inteligente ya existe en el igual, o si el igual se ha quedado sin recursos.
{: tsCauses}

- Abra el panel de control de Kubernetes y asegúrese de que el estado del igual sea `En ejecución`.  
- Abra el nodo igual y asegúrese de que la versión del contrato inteligente no exista aún en el igual y vuelva a intentarlo con la versión adecuada.
- Si sigue teniendo problemas después de que se active el nodo, [consulte los registros del nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) para ver si hay errores.  
{: tsResolve}

## ¿Por qué el contrato inteligente que he instalado en el igual no aparece en la interfaz de usuario?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

Se ha instalado un contrato inteligente en un igual pero, al pulsar el separador
**Contratos inteligentes**, no aparece.
{: tsSymptoms}

Este problema puede producirse cuando algún otro usuario o aplicación instala el contrato inteligente en el igual y no tiene la identidad de administrador del igual en la cartera del navegador.
{: tsCauses}

Para poder ver los contratos inteligentes instalados en un igual, debe ser un administrador de igual. Asegúrese de que la identidad de administrador de igual existe en la cartera del navegador. Si no es así, deberá importarla en la cartera de la consola.
{: tsResolve}

## ¿Cómo puedo ver los registros del contenedor del contrato inteligente?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

Es posible que tenga que ver el contrato inteligente, o el código de encadenamiento, y los registros del contenedor para depurar un problema del contrato inteligente.
{: tsSymptoms}

Siga estas instrucciones para ver los registros del contenedor de contratos inteligentes en:
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs).
{: tsResolve}

## Mi canal, los contratos inteligentes y las identidades han desaparecido de la consola. ¿Cómo puedo recuperarlos?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

Las identidades de la cartera de la consola constan de un certificado para firmas y una clave privada que le permiten gestionar los componentes de blockchain, pero solo se almacenan en el almacenamiento local del navegador. Usted es el responsable de proteger y gestionar estas identidades. Le recomendamos que las exporte al sistema de archivos después de crearlas. Siempre que crea un nodo nuevo, asocia una identidad de la cartera de la consola al nodo. Esta identidad de administrador es lo que le permite gestionar el nodo. Cuando cambia de navegador navegadores o cambia a un navegador en otra máquina, estas identidades ya no se encuentran en la cartera. Por lo tanto, no puede gestionar los componentes.
{: tsSymptoms}

Una de las características nuevas de {{site.data.keyword.blockchainfull_notm}} Platform es que ahora usted es el responsable de proteger y gestionar los certificados. Por lo tanto, solo se mantienen en el almacenamiento local del navegador para permitirle gestionar el componente. Si utiliza una ventana de navegador privada y cambia luego a otro navegador o a una ventana de navegador que no sea privada, las identidades que haya creado desaparecerán de la cartera de la consola en la nueva sesión de navegador. Por lo tanto, es necesario que exporte las identidades de la cartera de la consola de la sesión de navegador privada en el sistema de archivos. A continuación, puede importarlas en la sesión de navegador que no es privada si es necesario. De lo contrario, no hay forma de recuperarlas.
{: tsCauses}

- Cada vez que cree una nueva definición de MSP de la organización, genera claves para una identidad que tiene permiso para administrar la organización. Por lo tanto, durante este proceso debe pulsar los botones **Generar** y luego **Exportar** para almacenar la identidad generada en la cartera de la consola y, a continuación, guardarla en el sistema de archivos como un archivo JSON.
- Para resolver este problema en el navegador, tiene que importar dichas identidades y asociarlas con el nodo correspondiente:
  - En el navegador donde está experimentando el problema, pulse el separador **Cartera** seguido de **Añadir identidad** para importar el archivo JSON en la cartera.
  - Pulse **Cargar JSON** y examine el archivo JSON que ha exportado utilizando el botón **Añadir archivos**.
  - Pulse **Enviar**.
  - Ahora abra el nodo de igual o del servicio de ordenación al que estaba asociada originalmente esta identidad y pulse el icono **Valores**.
  - Pulse el botón **Asociar identidad**.
  - Seleccione la identidad que acaba de importar en la cartera de la consola desde la lista desplegable.
  - Pulse **Asociar**.
- Repita este proceso para cada identidad que había en la cartera del navegador original.
{: tsResolve}

## ¿Por qué recibo el error `No se puede autenticar con el ID y el secreto de inscripción que ha especificado` cuando creo una nueva definición de MSP de organización?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

Cuando intenta crear una nueva definición de MSP de organización desde el separador Organizaciones, recibe el error `No se puede autenticar con el ID y el secreto de inscripción que ha especificado`.
{: tsSymptoms}

Este error se produce cuando el valor que ha especificado para el secreto de inscripción no es válido para el ID de inscripción que ha seleccionado en la sección `Generar certificado de administrador de organización` del panel.
{: tsCauses}

Verifique que ha seleccionado el ID de inscripción de administración de organización correcto en la lista desplegable de ID de inscripción y especifique el valor correcto para el secreto de inscripción.
{: tsResolve}

## ¿Por qué recibo el error `Se ha producido un error al actualizar el canal` al intentar añadir una organización a mi canal?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

Cuando intenta añadir otra organización a un canal, la actualización falla con el mensaje `Se ha producido un error al actualizar el canal`.
{: tsSymptoms}

Este error se produce cuando el **ID de MSP del actualizador del canal** seleccionado en el panel **Actualizar canal** no es un administrador del canal.
{: tsCauses}

En el panel **Actualizar canal**, desplácese hacia abajo hasta el **ID de MSP del actualizador del canal**
y seleccione el ID de MSP que se ha especificado al crear el canal o especifique el ID de MSP del administrador del canal.
{: tsResolve}

## Mi clúster Kubernetes de {{site.data.keyword.cloud_notm}} ha caducado. ¿Qué quiere decir esto?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

He recibido un correo electrónico que indica que mi clúster de servicio {{site.data.keyword.IBM_notm}} Kubernetes está a punto de caducar o que su estado es `Caducado`. O bien, no puedo acceder a la consola después de 30 días.
{: tsSymptoms}

Los clústeres gratuitos de Kubernetes solo son válidos durante 30 días.
{: tsCauses}

No es posible migrar de un clúster gratuito a un clúster de pago. Después de 30 días, no podrá acceder a la consola y se suprimirán todos los nodos y certificados. Consulte este tema sobre la
[Caducidad de clústeres de Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration) para obtener información sobre lo que ocurre y lo que puede hacer al respecto.
{: tsResolve}

## ¿Por qué fallan las transacciones que envío desde VS Code?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

Las transacciones enviadas desde VS Code fallan con un error similar a:
```
Error al enviar la transacción; no hay ningún plan de aprobación disponible para {"chaincodes":[{"name":"hello-world"}]}
```
{: tsSymptoms}

Este error se produce si utiliza la característica Fabric Service Discovery, pero no ha configurado ningún igual de ancla en su canal.
{: tsCauses}

Siga el paso tres del [tema sobre datos privados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) de la guía de aprendizaje sobre cómo desplegar un contrato inteligente para configurar sus iguales de ancla.

## Cuando inicio sesión en mi consola, ¿por qué obtengo un error 401 No autorizado?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

Cuando intento iniciar una sesión en mi consola, no puedo acceder a la consola desde mi navegador. Si consulto los registros del navegador, encuentro el error 401 No autorizado.
{: tsSymptoms}

La sesión de la consola del navegador excede el tiempo de espera después de **8 horas** de inactividad. Si una sesión pasa a estar inactiva, la consola impide que el usuario inactivo realice ninguna acción.
{: tsCauses}

Si la sesión ha pasado a estar inactiva, puede intentar simplemente renovar el navegador. Si no funciona, cierre el navegador, incluidos **todos** los separadores y todas las ventanas. Vuelva a abrir el URL. Se le solicitará que inicie la sesión.

Como práctica recomendada, ya debería haber almacenado los certificados y las identidades en el sistema de archivos. Si utiliza una ventana de incógnito, todos los certificados se suprimen del almacenamiento local del navegador cuando se cierra el navegador. Después de volver a iniciar la sesión, tendrá que volver a importar las identidades y los certificados.
{: note}

## ¿Por qué los nodos que he desplegado en {{site.data.keyword.cloud_notm}} Private no procesan transacciones y fallan las comprobaciones de estado?
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

Mi consola indica que mis iguales y nodos de ordenación siguen en ejecución. Sin embargo, mis transacciones fallan. Cuando ejecuto una comprobación de actividad o de preparación en los pods del nodo, la comprobación indica que los pods están en mal estado.
{: tsSymptoms}

Si ha desplegado, eliminado y actualizado nodos en el clúster muchas veces, es posible que, como resultado de las pruebas, Docker pueda fallar debido a un error conocido con {{site.data.keyword.cloud_notm}} Private. Para obtener más información, consulte este problema en la
[Documentación de {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}.
{: tsCauses}

Para abordar este problema, elimine los pods que han fallado y vuelva a desplegar los nodos. También puede reiniciar el servicio de Docker en el clúster.

## ¿Por qué devuelven mis transacciones un error de política de aprobación: el conjunto de firmas no cumple la política?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

Cuando invoco un contrato inteligente para enviar una transacción, la transacción devuelve el error de política de aprobación siguiente:
```
returned error: VSCC error: endorsement policy failure, err: signature set did not satisfy policy
```
{: tsSymptoms}

Si se ha unido recientemente a un canal y ha instalado el contrato inteligente, este error se produce si no ha añadido su organización a la política de aprobación. Debido a que su organización no está en la lista de organizaciones que pueden aprobar una transacción de un contrato inteligente, el canal rechazará la aprobación por parte de sus iguales. Si detecta este problema, puede cambiar la política de aprobación actualizando el contrato inteligente. Para obtener más información, consulte
[Especificación de una política de aprobación](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) y [Actualización de un contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}
