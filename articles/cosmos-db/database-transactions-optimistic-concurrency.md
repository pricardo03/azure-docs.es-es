---
title: Transacciones entre bases de datos y control de simultaneidad optimista en Azure Cosmos DB
description: En este artículo se describen las transacciones entre bases de datos y el control de simultaneidad optimista en Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 568f47aacf39793d4c2da46798682abc002ca33b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59279518"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>Transacciones y control de simultaneidad optimista

Las transacciones entre bases de datos proporcionan un modelo de programación seguro y predecible para tratar los cambios simultáneos que se producen en los datos. Bases de datos relacionales tradicionales, como SQL Server, le permite escribir la lógica de negocios mediante procedimientos almacenados o desencadenadores, envían al servidor para su ejecución directamente en el motor de base de datos. Con las bases de datos relacionales tradicionales, es necesarios para tratar con dos diferentes lenguajes de programación de la aplicación (no transaccional) programación de lenguaje como JavaScript, Python, C#, Java, etc., y la transaccional (lenguaje de programación Por ejemplo, Transact-SQL) que se ejecuta nativamente por la base de datos.

El motor de base de datos en Azure Cosmos DB es totalmente compatible con transacciones ACID (atomicidad, coherencia, aislamiento, durabilidad) con aislamiento de instantáneas. Todas las operaciones de base de datos dentro del ámbito de un contenedor [partición lógica](partition-data.md) se ejecuta de manera transaccional dentro del motor de base de datos que se hospeda la réplica de la partición. Estas operaciones incluyen operaciones de escritura (actualización de uno o varios elementos de la partición lógica) y operaciones de lectura. La siguiente tabla muestra los diferentes tipos de operaciones y transacciones:

| **operación**  | **Tipo de operación** | **Transacción de uno o varios elementos** |
|---------|---------|---------|
| Inserción (sin necesidad de un desencadenador previo o posterior) | Escritura | Transacción de un elemento único |
| Inserción (con un desencadenador previo o posterior) | Escritura y lectura | Transacción de varios elementos |
| Reemplazo (sin necesidad de un desencadenador previo o posterior) | Escritura | Transacción de un elemento único |
| Reemplazo (con un desencadenador previo o posterior) | Escritura y lectura | Transacción de varios elementos |
| Actualización e inserción (sin necesidad de un desencadenador previo o posterior) | Escritura | Transacción de un elemento único |
| Actualización e inserción (con un desencadenador previo o posterior) | Escritura y lectura | Transacción de varios elementos |
| Eliminación (sin necesidad de un desencadenador previo o posterior) | Escritura | Transacción de un elemento único |
| Eliminación (con un desencadenador previo o posterior) | Escritura y lectura | Transacción de varios elementos |
| Ejecutar procedimiento almacenado | Escritura y lectura | Transacción de varios elementos |
| El sistema inició la ejecución de un procedimiento de combinación | Escritura | Transacción de varios elementos |
| El sistema inició la ejecución de la eliminación de elementos basados en la expiración (TTL) de un elemento. | Escritura | Transacción de varios elementos |
| Lectura | Lectura | Transacción de un solo elemento |
| Fuente de cambios | Lectura | Transacción de varios elementos |
| Lectura paginada | Lectura | Transacción de varios elementos |
| Consulta paginada | Lectura | Transacción de varios elementos |
| Ejecución de UDF como parte de la consulta paginada | Lectura | Transacción de varios elementos |

## <a name="multi-item-transactions"></a>Transacciones de varios elementos

Azure Cosmos DB le permite escribir [procedimientos almacenados, desencadenadores anteriores y posteriores,-funciones definidas por con usuario (UDF)](stored-procedures-triggers-udfs.md) y combinar los procedimientos en JavaScript. Azure Cosmos DB admite de forma nativa la ejecución de JavaScript dentro de su motor de base de datos. Puede registrar los procedimientos almacenados, los desencadenadores previos o posteriores, las funciones definidas por el usuario (UDF) y los procedimientos de combinación en un contenedor y ejecutarlos posteriormente de forma transaccional en el motor de base de datos de Azure Cosmos. La escritura de la lógica de la aplicación en JavaScript permite la expresión de forma natural del flujo de control, el ámbito de las variables, la asignación y la integración de primitivos de control de excepciones en transacciones de bases de datos directamente en el lenguaje JavaScript.

Los procedimientos almacenados basados en JavaScript, los desencadenadores, las UDF y los procedimientos de combinación se encapsulan en una transacción ACID ambiental con aislamiento de instantáneas en todos los elementos de la partición lógica. Durante el transcurso de esta ejecución, si el programa de JavaScript lanza una excepción, entonces se cancela y se revierte toda la transacción. El modelo de programación resultante es sencillo, pero eficaz. Los desarrolladores de JavaScript obtienen un modelo de programación duradero al tiempo que siguen usando sus construcciones de lenguaje y primitivos de biblioteca que ya conocen.

La capacidad de ejecutar JavaScript directamente en el motor de base de datos proporciona rendimiento y la ejecución transaccional de las operaciones de base de datos en los elementos de un contenedor. Además, debido a que el motor de base de datos de Azure Cosmos DB admite de forma nativa JSON y JavaScript, no hay ningún error de coincidencia de impedancia entre los sistemas tipo de la aplicación y la base de datos.

## <a name="optimistic-concurrency-control"></a>Control de concurrencia optimista 

El control de simultaneidad optimista le permite evitar la pérdida de actualizaciones y eliminaciones. Las operaciones simultáneas en conflicto están sujetas al bloqueo pesimista normal del motor de base de datos que hospeda la partición lógica que posee el elemento. Cuando dos operaciones simultáneas intentan actualizar la versión más reciente de un elemento de una partición lógica, uno de ellos tendrá prioridad y en el otro se producirá un error. No obstante, si una o dos operaciones que están intentando actualizar simultáneamente el mismo elemento habían leído anteriormente un valor más antiguo de este, la base de datos no sabrá si el valor leído anteriormente por cualquiera de las operaciones en conflicto, o por ambas, era realmente el valor más reciente de ese elemento. Afortunadamente, se puede detectar esta situación con la **Control de simultaneidad optimista (OCC)** antes de permitir que las dos operaciones escriba el límite de la transacción dentro del motor de base de datos. El control de simultaneidad optimista protege los datos frente a cambios accidentales por sobrescritura realizados por otros. También impide que otros usuarios sobrescriban accidentalmente sus propios cambios.

Las actualizaciones simultáneas de un elemento están sujetas al control de simultaneidad optimista mediante la capa del protocolo de comunicación de Azure Cosmos DB Azure Cosmos DB garantiza que la versión del elemento en el lado del cliente que está actualizando (o eliminando) es la misma que la versión del elemento en el contenedor de Azure Cosmos. Esto garantiza que las escrituras están protegidas frente a la sobrescritura accidental por escrituras de otros y viceversa. En un entorno multiusuario, el control de simultaneidad optimista le protege de eliminar o actualizar accidentalmente la versión equivocada de un elemento. Por tanto, los elementos están protegidos contra los desastrosos problemas de "Actualización perdida" o "Eliminación perdida".

Todos los elementos almacenados en un contenedor de Azure Cosmos tienen una propiedad `_etag` definida por el sistema. El servidor genera y actualiza automáticamente el valor de `_etag` cada vez que se actualiza el elemento. `_etag` se puede usar con lo proporcionados por el cliente `if-match` encabezado de solicitud para permitir que el servidor decida si se puede actualizar condicionalmente un elemento. El valor de la `if-match` encabezado coincide con el valor de la `_etag` en el servidor, a continuación, se actualiza el elemento. Si el valor de la `if-match` encabezado de solicitud ya no es actual, el servidor rechaza la operación con un "HTTP 412 Error de condición previa" mensaje de respuesta. El cliente, a continuación, volver a puede recuperar el elemento para adquirir la versión actual del elemento en el servidor o invalidar la versión del elemento en el servidor con su propia `_etag` valor para el elemento. Además, `_etag` puede utilizarse con el `if-none-match` encabezado para determinar si un volver a obtener de un recurso necesario. 

El elemento `_etag` cambia el valor de cada vez que se actualiza el elemento. Para las operaciones de elemento de reemplazo, `if-match` se debe expresar explícitamente como parte de las opciones de solicitud. Para ver un ejemplo, consulte el código de ejemplo de [GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs#L398-L446). `_etag` implícitamente se comprueban los valores para todos los elementos escritos usados por el procedimiento almacenado. Si se detecta ningún conflicto, el procedimiento almacenado se revierta la transacción y producir una excepción. Con este método, se aplicarán de forma atómica todas las operaciones de escritura del procedimiento almacenado o ninguna de ellas. Esto constituye una señal para que la aplicación vuelva a aplicar las actualizaciones y reintente la solicitud original del cliente.

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar más información sobre las transacciones entre bases de datos y el control de simultaneidad optimista en los siguientes artículos:

- [Uso de bases de datos, contenedores y elementos de Azure Cosmos](databases-containers-items.md)
- [Niveles de coherencia](consistency-levels.md)
- [Tipos de conflicto y directivas de resolución de conflictos](conflict-resolution-policies.md)
- [Procedimientos almacenados, desencadenadores y funciones definidas por el usuario](stored-procedures-triggers-udfs.md)
