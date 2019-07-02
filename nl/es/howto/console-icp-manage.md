---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

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
{:curl: #curl .ph data-hd-programlang='curl'}

# Administración de la consola
{: #console-icp-manage}

Después de desplegar la consola en {{site.data.keyword.cloud}} Private, puede utilizar la consola para añadir o eliminar usuarios de la consola, así como las API de acceso que le permiten trabajar en la red y controlar la consola. También puede acceder a los registros de la consola y personalizarlos.
{:shortdesc}

## Gestión de usuarios desde la consola
{: #console-icp-manage-users}

El usuario que suministra la consola de {{site.data.keyword.blockchainfull_notm}} Platform se considera el administrador de la consola. El administrador puede añadir y otorgar a otros usuarios acceso a la consola mediante el separador **Usuarios** de la consola. Cada usuario que acceda a la consola debe tener asignada una política de acceso con un rol de usuario definido. La política determina qué acciones puede realizar el usuario dentro de la consola. De forma predeterminada, al administrador de la consola se le otorga el rol de **Gestor** de la consola. Se pueden asignar otros usuarios los roles de **Gestor**, **Escritor** o **Lector** cuando un gestor de la consola los añade a la consola. Tenga en cuenta que los usuarios también se pueden gestionar mediante [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis).

| Rol | Funciones |
|--------|----------|
| Gestor | Como Gestor, tiene permisos más allá del rol Escritor. Puede hacer todo lo que puede hacer un Lector y un Escritor, además de: <ul><li>Suministrar nuevos componentes utilizando la consola o las API.</li><li>Suprimir componentes suministrados utilizando la consola o las API.</li><li>Añadir/eliminar usuarios y cambiar las políticas de acceso de los usuarios.</li><li>Cambiar los niveles de registro de la consola utilizando la consola o las API.</li><li>Reiniciar la consola utilizando una API.</li></ul> |
| Escritor | Como Escritor, tiene permisos más allá del rol Lector, incluyendo: <ul><li>Importar componentes utilizando la consola o las API.</li><li>Eliminar componentes importados utilizando la consola o las API.</li><li>Registrar usuarios en una CA.</li><li> Añadir o eliminar notificaciones utilizando la consola o las API.</li></ul>  |
| Lector | Como lector, puede realizar acciones de solo lectura, incluyendo: <ul><li>Ver la interfaz de usuario de la consola.</li><li>Ver el registro de la consola.</li><li>Exportar componentes.</li><li>Emitir cualquier API GET.</li></ul> | |

Los permisos son acumulativos. Si selecciona el rol **Gestor**, el usuario también podrá realizar todas las acciones de **Escritor** y de **Lector** sin que necesite marcar dichos roles de forma adicional. Del mismo modo, un usuario con el rol **Escritor** podrá realizar todas las acciones del rol **Lector**.

### Gestión de la contraseña de un usuario
{: #console-icp-manage-user-pw}

La contraseña que se ha almacenado en el [secreto de contraseña](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) y que se pasa a la consola durante la configuración pasa a ser la contraseña predeterminada para la consola. Todos los usuarios deben utilizar esta contraseña cuando inicien una sesión en la consola por primera vez. El gestor de la consola también puede especificar una nueva contraseña predeterminada. En el separador **Usuarios** de la consola, pulse el enlace **Actualizar configuración** bajo el nombre del mosaico del servicio de autenticación. En el panel de la derecha, puede ver o actualizar la contraseña predeterminada para los nuevos usuarios de la consola.

Debe compartir la contraseña predeterminada, o la contraseña predeterminada que restablezca, con los usuarios para que puedan iniciar la sesión en la consola. Se les exigirá que cambien la contraseña tras su primer inicio de sesión.
{:note}

Los usuarios con el rol de gestor pueden restablecer contraseñas para otros usuarios. En el separador **Usuarios** de la consola, pulse los tres puntos verticales que hay al final de la fila para un usuario específico y luego pulse **Restablecer contraseña**. La consola restablecerá la contraseña de ese usuario a la contraseña predeterminada, que se puede comprobar y confirmar en el panel derecho. Los usuarios con cualquier rol pueden cambiar su propia contraseña en cualquier momento. Pulse el icono de avatar de la esquina superior derecha y luego pulse **Cambiar contraseña**.

Tras añadir nuevos usuarios a la consola, es posible que los usuarios no puedan ver todos los nodos, canales o código de encadenamiento que despliegan otros usuarios. Para trabajar con estos componentes, cada usuario necesita importar las identidades asociadas en su propia cartera de consola. Para obtener más información, consulte [Almacenamiento de identidades en la cartera de la consola](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modificación del rol de un usuario
{: #console-icp-manage-reset-user-pw}

Un usuario con el rol de gestor puede actualizar los roles de otros usuarios de la consola y proporcionarles acceso a más o menos prestaciones de la consola. En el separador **Usuarios** de la consola, pulse los tres puntos verticales que hay al final de la fila de un usuario específico y luego pulse **Actualizar usuario autenticado**. Seleccione el nuevo rol para el usuario en el panel de la derecha. El rol del usuario se actualizará la próxima vez que inicie una sesión en la consola.

### Supresión de un usuario de la consola
{: #console-icp-manage-icp-remove-user}

Si es un usuario con el rol de gestor, puede eliminar el acceso de un usuario a la consola. En el separador **Usuarios** de la consola, pulse sobre el recuadro de selección que hay al principio de la fila de usuario específica. A continuación, pulse el icono de
**Papelera** en la parte superior de la tabla, situado junto al botón **Añadir nuevos usuarios**. Tras eliminar el usuario de la tabla, el usuario no podrá volver a iniciar sesión en la consola.

## Utilización de las API de {{site.data.keyword.blockchainfull_notm}}
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform expone API RESTful que le permiten importar, editar y eliminar nodos de blockchain de la consola. También puede utilizar las API para gestionar los valores, el registro y las notificaciones de la consola. Siga las instrucciones que se indican a continuación para utilizar las API que proporciona una consola desplegada en {{site.data.keyword.cloud_notm}} Private.

### Requisitos previos
{: #console-icp-manage-prereqs}

Para utilizar las API, tendrá que recopilar la información siguiente:

- El **Punto final de API** de la consola. Es el URL que se utiliza para acceder a la consola mediante el navegador. Encontrará este URL en la sección **Notas:** de la pantalla Visión general del release de Helm.
- Un nombre de usuario y contraseña que puede utilizar para acceder a la consola. Para crear una clave de API, debe tener una cuenta con un [rol de gestor](#console-icp-manage-users).

### Utilización de una clave de API
{: #console-icp-manage-create-api-key}

Cada consola proporciona su propia gestión de identidad y acceso. Puede utilizar el nombre de usuario y la contraseña de la consola para generar una clave de API y un secreto que pueden autorizar las llamadas de API.

Cada clave de API se asocia con un rol que controla las prestaciones que el usuario tiene permitido utilizar. Las claves de API utilizan los mismos roles de política de acceso que existen para los usuarios que inician la sesión en la consola mediante un nombre de usuario y una contraseña. Consulte la tabla de la sección [Gestión de usuarios](#console-icp-manage-users) para ver la lista de acciones que puede realizar cada rol. Puesto que solo los roles de gestor pueden crear claves de API, un administrador de la consola debe crear claves de API para los usuarios con los roles de lector y escritor. Las claves de API nunca caducan. Como resultado, el administrador de la consola debe suprimir las claves de API que no se utilicen.

Existen tres API disponibles para gestionar sus claves de API:
- [Creación de una clave de API](#console-icp-manage-create-api-key)
- [Visualización de claves de API](#console-icp-manage-view-api-keys)
- [Supresión de claves de API](#console-icp-manage-delete-api-keys)

Utilice la solicitud siguiente para crear una clave de API y un secreto. Luego puede utilizar esta clave y este secreto para utilizar las otras API. La consola no guarda el secreto de API; lo debe guardar el usuario.

#### Creación de una clave de API
{: #console-icp-manage-create-api-key-api}

| **Solicitud** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campos del cuerpo de la solicitud** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Se necesita al menos un valor </li><li>`string` opcional</li></ul>|
| **Campos del cuerpo de la respuesta** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` Guarde este valor: la clave no se guarda</li><li>`["<role>"]`</li></ul>|
| Autorización necesaria | gestor |

#### Ejemplo de solicitud curl: creación de una clave de API
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### Gestión de las claves de API de la consola
{: #console-icp-manage-api-keys}

Una vez que ha creado una clave de API y un secreto, puede utilizar las API para ver o eliminar las claves que ha creado. Las claves y los secretos no caducan hasta que se suprimen.

#### Visualización de claves de API
{: #console-icp-manage-view-api-keys}

| **Solicitud** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campos del cuerpo de la respuesta** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` Indicación de fecha y hora de Unix en milisegundos</li><li>`string`</li></ul>|
| Autorización necesaria | lector |

#### Solicitud curl de ejemplo: visualización de claves de API
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### Supresión de claves de API
{: #console-icp-manage-delete-api-keys}

| **Solicitud** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| Autorización necesaria| gestor |

#### Ejemplo de solicitud curl: supresión de una clave de API
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### Gestión de usuarios mediante las API
{: #console-icp-manage-users-apis}


También puede utilizar las API para listar, añadir o eliminar usuarios que pueden iniciar una sesión en la consola o acceder a las API de la consola. También puede editar los roles de usuario para actualizar un nivel de acceso de usuario. Están disponibles las API siguientes:

- [Listar usuarios](#console-icp-manage-list-users-api)
- [Editar usuarios](#console-icp-manage-edit-users-api)
- [Añadir usuarios](#console-icp-manage-add-users-api)
- [Eliminar usuarios](#console-icp-manage-remove-users-api)

Para las API que controlan los usuarios y los valores de la consola, y gestionan los nodos de la red, necesita añadir un distintivo
``-K`` o ``--insecure`` para eludir el requisito de un certificado TLS. También puede descargar el certificado TLS desde la consola utilizando el navegador.

#### Listar usuarios
{: #console-icp-manage-list-users-api}


| **Solicitud** |  |
|-------------|-----------|
| Path | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos del cuerpo de la respuesta** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` id de usuario</li><li>`string` dirección de correo electrónico</li><li>`["<role>"]`</li><li>`number` Indicación de fecha y hora de Unix en milisegundos</li></ul>|
| Autorización necesaria | lector |

#### Ejemplo de solicitud curl: listado de usuarios
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### Editar usuarios
{: #console-icp-manage-edit-users-api}

| **Solicitud** |  |
|-------------|-----------|
| Path | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos del cuerpo de la solicitud** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` id de usuario </li><li>`["reader", "writer", "manager"]` Se necesita al menos un valor</li></ul> |
| **Campos del cuerpo de la respuesta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` id de usuario</li></ul>|
| Autorización| gestor |

#### Ejemplo de solicitud curl: edición de un usuario
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### Añadir usuarios
{: #console-icp-manage-add-users-api}

| **Solicitud** |  |
|-------------|-----------|
| Path | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos del cuerpo de la solicitud** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Se necesita al menos un valor </li><li>`string` opcional</li></ul>|
| Autorización | gestor |

#### Ejemplo de solicitud curl: adición de un usuario
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### Eliminar usuarios
{: #console-icp-manage-remove-users-api}

| **Solicitud** |  |
|-------------|-----------|
| Path | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos del cuerpo de la solicitud** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` id de usuario</li></ul>|
| **Campos del cuerpo de la respuesta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` id de usuario</li></ul>|
| Autorización | gestor |

#### Ejemplo de solicitud curl: eliminación de un usuario
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### Utilización de las API para gestionar los componentes

Puede ver el conjunto completo de API disponibles en la
[Referencia de API de {{site.data.keyword.blockchainfull_notm}} Platform](https://test.cloud.ibm.com/apidocs/blockchain).

Puesto que utiliza las API para comunicarse con la consola en {{site.data.keyword.cloud_notm}} Private, debe utilizar la autorización que proporciona {{site.data.keyword.cloud_notm}} con la autenticación que proporciona la consola. Sustituya `Bearer Auth` en la consulta de API por `-u <api_key>:<api_secret>`. También debe añadir un distintivo `-K` o
``--insecure`` en el mandato, o descargar el certificado TLS de la consola utilizando el navegador. No puede utilizar el separador
**Pruébelo** para probar las API para las redes en {{site.data.keyword.cloud_notm}} Private.

Como ejemplo, la llamada de API siguiente devolverá información sobre todos los componentes que se ejecutan en una instancia de servicio de
{{site.data.keyword.blockchainfull_notm}} Platform para {{site.data.keyword.cloud_notm}}.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
La misma llamada de API se parecería a la solicitud siguiente para una consola desplegada en {{site.data.keyword.cloud_notm}} Private.
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

Puede utilizar las API para crear los nodos en el clúster donde se despliega la consola e importar nodos de otros clústeres o {{site.data.keyword.cloud_notm}}. Para obtener más información, visite los apartados sobre [Creación de una red mediante API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) e [Importación de una red mediante API](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Visualización de los registros
{: #icp-console-manage-logs}

Cuando utilice la consola de {{site.data.keyword.blockchainfull_notm}} Platform, es posible que necesite ver los registros para depurar un problema.

### Visualización de los registros de la consola
{: #console-icp-manage-console-logs}

Puede acceder fácilmente a los registros de la consola si tiene que depurar los problemas que encuentre al utilizar la consola o al trabajar con los nodos. También puede establecer el nivel de registro para aumentar o reducir la cantidad de registros que recopila la consola. Los registros de la consola se recopilan por separado de los [registros de nodo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs), recopilados por {{site.data.keyword.cloud_notm}} Private.

Vaya al separador **Valores** en el navegador de la consola para cambiar los valores de registro. Los registros de la consola se recopilan de dos fuentes distintas:

  * **Registros de cliente:** estos registros se recopilan cuando se envían mandatos desde el navegador a la consola.
  * **Registros de servidor:** estos registros se recopilan cuando la consola envía mandatos a los nodos y desde el despliegue de la consola. Estos registros incluyen la salida de registro de Hyperledger Fabric.

Establezca la cantidad de registros recopilados utilizando la lista desplegable para cada tipo de registro. Por ejemplo, **Error** y **Aviso** recopilan la cantidad mínima de registros, mientras que **Depuración** y **Todos** recopilan la mayor cantidad.

Solo puede ver los registros de la consola si ha iniciado la sesión como administrador de la consola. Para ver los registros desde el separador **Valores**, sustituya la palabra `settings` en el URL del navegador por `api/v1/logs`. Por ejemplo, si el url de su consola es `localhost:3001/settings`, para ver los registros debe ir a `localhost:3001/api/v1/logs`. Los registros de cliente y de servidor se recopilan en archivos independientes. Los registros más recientes se encuentran en la parte superior de la página.

### Visualización de los registros de la consola
{: #console-icp-manage-node-logs}

Los registros de componentes se pueden ver desde la línea de mandatos mediante [mandatos de CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} o mediante [Kibana](https://www.elastic.co/products/kibana){: external}, que se incluye en el clúster de {{site.data.keyword.cloud_notm}} Private.

- Utilice el mandato `kubectl logs` para ver los registros de contenedor dentro del pod. Siga las instrucciones para [Instalar la CLI de kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} si todavía no lo ha hecho. Si no está seguro de cuál es el nombre del pod, ejecute el mandato siguiente para ver la lista de pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  A continuación, ejecute el mandato siguiente para recuperar los registros correspondientes al contenedor de nodos que reside dentro del pod:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Sustituya `<pod_name>` por el nombre de su pod de la salida del mandato anterior.  
  Sustituya `<node>` por `ca`, `peer` u `orderer` para ver los registros de su nodo.  
  Sustituya `<node>` por `fluentd` para ver los registros de los contratos inteligentes.

  Para obtener más información sobre el mandato `kubectl logs`, consulte la [documentación de Kubernetes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- Como alternativa, puede ejecutar [access the logs](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} desde la consola de {{site.data.keyword.cloud_notm}} Private; esto abrirá los registros en Kibana.

  **Nota:** al visualizar los registros en Kibana, es posible que reciba la respuesta `No results found`. Esta
condición se puede producir si {{site.data.keyword.cloud_notm}} Private utiliza la dirección IP del nodo trabajador como su nombre de host. Para resolver este problema, elimine el filtro que comienza por `node.hostname.keyword` al principio del panel y los registros se volverán visibles.

### Visualización de los registros de contenedor de contratos inteligentes
{: #console-icp-manage-container-logs}

Si detecta problemas con el contrato inteligente, puede consultar los registros del contrato inteligente, o el código de encadenamiento, y del contenedor para depurar un problema. Puede ejecutar el mandato siguiente para ver los registros del contenedor de contratos inteligentes:

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

Sustituya `<peer_pod>` por el nombre del pod del igual donde se ejecuta el código de encadenamiento. Utilice el mandato
`kubectl get po` para obtener la lista de pods en ejecución.

## Instalación de parches para los nodos
{: #console-icp-manage-patch}

Es posible que sea necesario actualizar con el tiempo las imágenes de docker subyacentes de {{site.data.keyword.IBM_notm}} Hyperledger Fabric para los nodos de igual, CA y clasificador, por ejemplo, con actualizaciones de seguridad o a un nuevo release puntual de Fabric. Puede actualizar sus imágenes de Fabric al actualizar el diagrama de Helm de {{site.data.keyword.blockchainfull_notm}} Platform.

Tras actualizar el diagrama de Helm, podrá encontrar el texto **Parche disponible** en un mosaico de nodo si hay una actualización de componente disponible. Este parche se puede instalar en un nodo siempre que esté preparado. Estos parches son opcionales, pero recomendables. No puede aplicar parches a nodos que se han importado en la consola.

Los parches se aplican a los nodos de uno en uno. Mientras se aplica un parche, el nodo no estará disponible para procesar solicitudes ni transacciones. Por lo tanto, para evitar cualquier interrupción del servicio, siempre que sea posible debe asegurarse de que haya otro nodo del mismo tiempo disponible para procesar solicitudes. La instalación de parches en un nodo tarda alrededor de un minuto en completarse y, cuando la actualización haya finalizado, el nodo estará listo para procesar solicitudes.
{:note}

Para aplicar un parche a un nodo, abra el mosaico del nodo y pulse el botón **Instalar parche**. No puede aplicar parches a nodos que haya importado en la consola.
