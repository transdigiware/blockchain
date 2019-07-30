---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: IBM Blockchain Platform, IBM Cloud Private, AWS, Data residency, world state

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

# Residencia de datos
{: #console-icp-about-data-residency}

Debido a que las redes blockchain no conocen qué tipo de datos se procesa, en ocasiones deben realizarse pasos adicionales para mantener seguros determinados tipos de datos. El requisito más común en cuanto a residencia de datos está asociado a las leyes de determinados países, según las cuales todos los datos que se procesan y almacenan en un sistema de TI deben permanecer dentro de las fronteras del país específico. Paralelamente, algunas empresas de sectores muy regulados, como las gubernamentales, de sanidad y de servicios financieros, obligan a que los datos se almacenen tras su cortafuegos.

Las redes blockchain permiten que varias organizaciones utilicen un libro mayor distribuido para realizar transacciones y compartir datos de una manera fiable y segura. No obstante, esto implica que los datos se pueden distribuir entre los nodos de la red y las regiones donde residen dichos nodos. Las organizaciones pueden utilizar varias opciones para separar los datos del resto de la red y lograr así la residencia de datos:
1. [Recopilaciones de datos privados en un canal compartido](#console-icp-about-data-residency-fabric)
2. [Recopilaciones de datos privados en un canal independiente](#console-icp-about-data-residency-use-case)
3. [Un canal independiente con todos los nodos del canal dentro de un solo país](#console-icp-about-data-residency-use-case-channel)

Cada enfoque proporciona un mayor nivel de aislamiento y protección de los datos, pero requiere un esfuerzo adicional en su implementación y gestión. Para ayudarle a entender cómo se puede utilizar cada opción para obtener residencia de datos, se proporciona una visión general de cómo se comparten los datos dentro de una red de Hyperledger Fabric. A continuación, se presenta un caso de uso de ejemplo para ilustrar el modo en que las organizaciones de un consorcio de blockchain utilizarían cada opción para separar sus datos y evitar que salgan de su región.

## Cómo se comparten los datos dentro de una red de {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-icp-about-data-residency-fabric}

La arquitectura de Hyperledger Fabric que subyace a {{site.data.keyword.blockchainfull_notm}} Platform se centra en tres componentes clave: un servicio de ordenación (compuesto por nodos de ordenación), entidades emisoras de certificados e iguales. Además, las organizaciones envían transacciones a estos nodos desde aplicaciones cliente utilizando los
[SDK de Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}. Al pensar en la residencia de datos, es importante entender cómo interactúan estos componentes con los datos y cómo los almacenan.

Los **iguales** los utilizan los miembros del consorcio para almacenar el [libro mayor](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ledger/ledger.html){: external} de blockchain. El libro mayor de blockchain consta de dos componentes. El primero es el escenario mundial, que almacena el valor más reciente de todos los datos del libro mayor en pares de clave/valor. El segundo es el registro blockchain de cada transacción. Los iguales reciben actualizaciones de estado en forma de nuevos bloques del servicio de ordenación. A continuación, utilizan los bloques y el escenario mundial para confirmar transacciones, actualizar el escenario mundial y añadir el registro de transacciones a blockchain. El servicio de ordenación establece el orden de las transacciones para todos los iguales del [consorcio](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium) y almacena una copia de la parte de blockchain del libro mayor.

Los **canales** son un mecanismo para transmitir datos dentro de una red. No puede participar en una red blockchain sin unirse a un canal. Los miembros de la red pueden utilizar canales para crear una separación lógica entre las aplicaciones empresariales e incluso para aumentar el rendimiento limitando el tráfico. También los pueden utilizar subconjuntos de las organizaciones del consorcio para realizar transacciones de forma privada y separar los datos.

Los iguales mantienen un libro mayor independiente para cada canal al que se unen. Únicamente las organizaciones que sean miembros del canal pueden unir sus iguales al canal y recibir actualizaciones del libro mayor del servicio de ordenación. Como resultado, cada canal está vinculado a un servicio de ordenación, que almacena la parte de blockchain del libro mayor de cada canal que mantiene. Las aplicaciones cliente envían transacciones a los iguales y al servicio de ordenación de un canal determinado. Estas transacciones se añaden al registro de transacciones dentro de blockchain e incluyen un [conjunto de lectura-escritura](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, que se añade a los pares de clave/valor en el escenario mundial.

Si la residencia de datos en el país es un requisito, debe tener en cuenta la ubicación de los iguales, el servicio de ordenación y las aplicaciones cliente. También debe conocer la ubicación de los iguales que pertenecen a otras organizaciones de sus canales.  Si utiliza
{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, puede encontrar la lista de
[Regiones y ubicaciones de {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference?topic=blockchain-ibp-regions-locations#ibp-regions-locations) donde tanto usted como los miembros del consorcio pueden desplegar componentes.

## Un caso de uso para la residencia de datos
{: #console-icp-about-data-residency-use-case}

Podemos utilizar un consorcio de ejemplo para ilustrar cómo se distribuyen los datos en
{{site.data.keyword.blockchainfull_notm}} Platform y explorar cómo pueden los miembros obtener la residencia de datos. La figura siguiente contiene un consorcio con un servicio de ordenación y cuatro organizaciones. Cada organización tiene un nodo igual. Dos organizaciones, Org A y Org B, y el servicio de ordenación se encuentran en los Estados Unidos. Las otras dos organizaciones, Org C y Org D, se encuentran en Alemania. Las cuatro organizaciones son miembros del canal X y han unido sus iguales a dicho canal.

![Consorcio de ejemplo](images/data_res_use_case.svg "Consorcio de ejemplo")

Cada igual que se haya unido al canal X almacena una copia del libro mayor del canal, que se muestra en la
**Figura 1** como el libro mayor X. Debido a que se han unido al canal iguales de Estados Unidos y de Alemania, los datos del libro mayor del canal residen en ambas zonas geográficas. La parte de blockchain del libro mayor la almacena también el servicio de ordenación que se encuentra en Estados Unidos.

Si dos organizaciones del consorcio crean un segundo canal, el canal Y, se crea un segundo libro mayor y se almacena en los iguales de los miembros del canal. Únicamente las organizaciones que se hayan unido al canal tendrán una copia de los datos del canal.

![Adición de un segundo canal](images/data_res_use_case_channel.svg "Adición de un segundo canal")


En la **Figura 2**, Org B y Org D se han unido al canal Y. Los iguales de Org B y Org D almacenan ahora una copia del libro mayor Y, además del libro mayor X. Debido a que se ha utilizado el mismo servicio de ordenación para crear el canal X y el canal Y, el servicio de ordenación tiene ahora una copia de la parte de blockchain de los libros mayores de ambos canales. Tanto en la **Figura 1** como en la **Figura 2**, los datos que crean las aplicaciones en Alemania se almacenan en Estados Unidos, lo cual no es deseable si se requiere residencia de datos.

Podemos utilizar el ejemplo anterior para explorar las opciones que tienen las organizaciones para obtener la residencia de datos. Supongamos que una normativa de Alemania requiere que algunos de los datos que crean Org C y Org D permanezcan dentro del país. Las organizaciones de Alemania pueden utilizar las tres opciones para evitar que los datos se almacenen en Estados Unidos.

## Opción uno: recopilaciones de datos privados en un canal compartido
{: #console-icp-about-data-residency-use-case-private-data}

Org C y Org D pueden utilizar la
[característica de datos privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "¿Qué es una recopilación de datos privados?"){: external} de Hyperledger Fabric para evitar que los datos se distribuyan en todas las organizaciones del canal. Las recopilaciones de datos privados permiten que las organizaciones puedan compartir datos de estado de igual a igual (a través del protocolo Gossip) con otras organizaciones que estén autorizadas para leer la recopilación. Los datos se almacenan en una base de datos privada independiente en el igual. El servicio de ordenación no está implicado y no ve los datos privados. Solo se añade al libro mayor del canal un hash de los datos en la recopilación y se almacena en los iguales de otros miembros del canal y en el servicio de ordenación. Esto permite que las organizaciones puedan verificar los datos privados si desean hacer públicos los detalles de la transacción. Para obtener más información, visite el artículo sobre el concepto de [Datos privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Datos privados"){: external} en la documentación de Fabric.

![Datos privados](images/data_res_private_data.svg "Título tres")

En la **Figura 3**, Org C y Org D han creado una recopilación de datos privados, la recopilación OrgC-OrgD, que permite que las organizaciones puedan realizar transacciones sin tener que compartir datos con Org A, con Org B ni con el servicio de ordenación. Los datos de estado clave/valor de esta recopilación solo se almacenan en los iguales de Org C y Org D y no salen de Alemania. No obstante, se almacena un hash de los datos de la recopilación en el libro mayor X y se comparten con el canal más amplio. Esto implica que se almacena un hash de los datos de la recopilación OrgC-OrgD en los iguales y en el servicio de ordenación en Estados Unidos.

Al considerar los datos privados, es importante entender la diferencia entre
**datos con hash** y **datos cifrados**. El cifrado utiliza una función de dos sentidos para transformar datos en un formato que oculta su valor original, pero que se puede volver a convertir al estado original. Como ejemplo, cuando se envían datos a través de una red protegida mediante TLS, los datos se cifran utilizando un certificado TLS. A continuación, se envían a través de la red como texto criptográfico, y luego los descifra el destinatario. El texto cifrado contiene todos los datos originales, y se pueden descifrar utilizando una clave privada. No obstante, el hashing es una función de una sola dirección que utiliza datos para crear una serie exclusiva de números y letras. Los datos hash no se pueden volver a convertir al formato original utilizando el código hash. Para verificar los datos que ha creado el hash, un destinatario necesita crear un nuevo hash de los datos originales utilizando la misma función hash, y verificar que los valores hash coinciden. Un tercero no puede utilizar el hash sin una copia de los datos originales.  

Con esta opción, es importante tener en cuenta que, aunque las organizaciones Org A y Org B no pueden ver los datos reales del libro mayor porque se ha aplicado el hash, sí que pueden ver que Org C y Org D están realizando transacciones y el volumen de transacciones que se producen entre ellas.

Tenga en cuenta también que los datos de una recopilación de datos privados se pueden purgar de los iguales que los almacenan. Aunque los datos se almacenan en un canal para siempre, las recopilaciones permiten que los miembros puedan especificar cuántos bloques se confirman en un canal antes de que [se purguen los datos privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private_data_tutorial.html#pd-purge){: external}. Una vez que se eliminen los datos de la recopilación de datos privados, ya no se podrá utilizar el hash del canal para verificar la transacción que los ha creado. En la red de ejemplo de la **Figura 3**, Org C y Org D pueden utilizar una política `block to live` para garantizar que los datos que no haga falta conservar para siempre se eliminen de la red por completo dentro un periodo de tiempo especificado.

## Opción dos: recopilaciones de datos privados en un canal independiente
{: #console-icp-about-data-residency-use-case-private-data-channel}

Org C y Org D también pueden utilizar recopilaciones de datos privados en el contexto de un canal independiente para proporcionar un aislamiento adicional para sus datos. La creación de un nuevo canal, el canal Y en este caso, garantiza que el hash de los datos privados solo se comparta con el servicio de ordenación, sin hacerlo con los demás miembros del consorcio ni almacenarse en sus iguales.

![Utilización de datos privados con un canal independiente](images/data_res_private_data_channel.svg "Utilización de datos privados con un canal independiente")

En la **Figura 4**, Org C y Org D han formado un nuevo canal, el canal Y, que no contiene ningún miembro en Estados Unidos. Como resultado, el hash de los datos de la recopilación OrgC-OrgD se almacena en el libro mayor Y en lugar del libro mayor X, y no se almacena en los iguales de Estados Unidos. Debido a que el servicio de ordenación se encuentra en Estados Unidos, un hash de los datos creados en Alemania sigue saliendo del país.

La creación de un canal independiente puede evitar que se compartan los detalles de transacción con las demás organizaciones del consorcio. Si las organizaciones de Alemania han utilizado un canal compartido, Org A y Org B deberían poder ver el número de hashes de transacción que se han confirmado en el libro mayor del canal mediante la recopilación de datos privados. Esto puede proporcionar a estas organizaciones visibilidad en relación con que Org C y Org D están realizando transacciones y el volumen de transacciones generadas entre ellas. No obstante, tenga en cuenta que la formación de un nuevo canal requiere una sobrecarga de gestión adicional para la creación y actualización. La formación de un nuevo canal también hace que Org C y Org D tengan más difícil compartir datos con Org A y Org B.

## Opción tres: un canal con todos los componentes en un país
{: #console-icp-about-data-residency-use-case-channel}

Org C y Org D también pueden crear un canal con toda la infraestructura dentro del mismo país. Esto requiere que los iguales que se han unido al canal, las aplicaciones y el servicio de ordenación residan todos en la misma región. En este caso de ejemplo, ninguno de los datos almacenados en el libro mayor del canal saldrá de la región ni se almacenarán fuera del país.

![Creación de un canal independiente](images/data_res_separate_channel.svg "Creación de un canal independiente")

En **Figura 5**, Org C y Org D han creado un nuevo canal para los datos que no deben salir de Alemania. Esto requiere la creación de un nuevo servicio de ordenación ubicado en Alemania para garantizar que la copia del clasificador del libro mayor del canal se almacene dentro del país. Debido a que en este caso el servicio de ordenación, el igual de Org C y el igual de Org D se encuentran en Alemania, Org C y Org D pueden ahora mantener los datos públicos en el canal si lo desean, o podrían seguir optando por utilizar recopilaciones de datos privados para evitar que todos los datos de transacción se almacenen en el servicio de ordenación.

La creación de un canal con todos los componentes en un país garantiza que todos los datos residan dentro de una región, incluyendo los pares de clave/valor, el registro de transacciones de blockchain y el hash de los datos privados. No obstante, esta opción requiere la sobrecarga de mantener un nuevo canal y el coste asociado al mantenimiento del servicio de ordenación.

## Material de referencia
{: #console-icp-about-data-residency-reference}

Para obtener más información sobre el flujo de datos de la red de {{site.data.keyword.blockchainfull_notm}} Platform, consulte la [documentación de Fabric sobre el flujo de transacciones](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

En el futuro, Zero Knowledge Proof mejorará la capacidad de obtener una mayor residencia de datos en Hyperledger Fabric. Zero-Knowledge Proof (ZKP) permite que un “comprobador” asegure a un “verificador” que conoce un secreto sin tener que revelar el propio secreto. Es una forma de mostrar que sabes algo que satisface una declaración sin mostrar lo que sabes.

Encontrará más información acerca de las recopilaciones de datos privados y de Zero Knowledge Proof en el documento técnico sobre [Transacciones privadas y confidenciales con Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.
