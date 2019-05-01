---
title: Trabajar con bases de datos, contenedores y elementos de Azure Cosmos DB
description: En este artículo se describe la forma de crear y usar bases de datos, contenedores y elementos de Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 8eaca83b7ea89737a63fe56a18505c8df7e93fdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60889387"
---
# <a name="work-with-databases-containers-and-items"></a>Uso de bases de datos, contenedores y elementos

Después de crear un [cuenta de Azure Cosmos](account-overview.md) en su suscripción de Azure, puede administrar datos en su cuenta mediante la creación de bases de datos, los contenedores y los elementos. En este artículo se describe cada una de estas entidades: bases de datos, contenedores y elementos. En la siguiente imagen se muestra la jerarquía de diferentes entidades en una cuenta de Azure Cosmos:

![Entidades de cuenta de Azure Cosmos](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Bases de datos de Azure Cosmos

Puede crear una o varias bases de datos de Azure Cosmos en su cuenta. Una base de datos es análogo a un espacio de nombres. Es la unidad de administración para un conjunto de contenedores de Azure Cosmos. En la siguiente tabla se muestra la forma de asignar una base de datos de Azure Cosmos a varias entidades específicas de API:

| **Entidad de Azure Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Base de datos de Azure Cosmos | Base de datos | Espacio de claves | Base de datos | Base de datos | N/D |

> [!NOTE]
> Con las cuentas de Table API, al crear la primera tabla, una base de datos predeterminada se crea automáticamente dentro de la cuenta de Azure Cosmos.

### <a name="operations-on-an-azure-cosmos-database"></a>Operaciones en una base de datos de Azure Cosmos

Puede interactuar con una base de datos de Azure Cosmos con las API de Azure Cosmos como sigue:

| **operación** | **CLI de Azure**|**SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- |
|Enumerar todas las bases de datos| Sí | Sí | Sí (la base de datos está asignada a un espacio de claves). | Sí | N/D | N/D |
|Leer la base de datos| Sí | Sí | Sí (la base de datos está asignada a un espacio de claves). | Sí | N/D | N/D |
|Create new database| Sí | Sí | Sí (la base de datos está asignada a un espacio de claves). | Sí | N/D | N/D |
|Actualizar la base de datos| Sí | Sí | Sí (la base de datos está asignada a un espacio de claves). | Sí | N/D | N/D |


## <a name="azure-cosmos-containers"></a>Contenedores de Azure Cosmos

Un contenedor de Azure Cosmos es la unidad de escalabilidad de rendimiento aprovisionado y almacenamiento. Un contenedor se divide de forma horizontal y luego se replica en varias regiones. Los elementos que agregue al contenedor y el rendimiento que aprovisione en él se distribuyen automáticamente en un conjunto de particiones lógicas basadas en la clave de partición. Para obtener más información sobre la creación de particiones y las claves de partición, vea [esto](partition-data.md) artículo. 

Al crear un contenedor de Azure Cosmos, debe configurar el rendimiento de uno de los siguientes modos:

* Modo de **rendimiento aprovisionado dedicado**: El rendimiento aprovisionado en un contenedor se reserva exclusivamente para ese contenedor y está respaldado por los SLA. Para obtener más información, consulte [Aprovisionamiento del rendimiento de un contenedor de Azure Cosmos DB](how-to-provision-container-throughput.md).

* Modo de **rendimiento aprovisionado compartido**: Estos contenedores comparten el rendimiento aprovisionado con los otros contenedores en la misma base de datos (excepto los contenedores que se han configurado con rendimiento aprovisionado dedicado). En otras palabras, el rendimiento aprovisionado en la base de datos se comparte entre todos los contenedores de "rendimiento compartido". Para obtener más información, consulte [how to configure provisioned throughput on an Azure Cosmos database](how-to-provision-database-throughput.md) (Cómo configurar el rendimiento aprovisionado en una base de datos de Azure Cosmos.

Un contenedor de Azure Cosmos puede escalarse elásticamente, sin importar si crea contenedores con modos de rendimiento aprovisionado "compartido" o "dedicado".

Un contenedor de Azure Cosmos es un contenedor de elementos sin esquemas. Los elementos de un contenedor pueden tener esquemas arbitrarios. Por ejemplo, un elemento que representa una persona, un elemento que representa un automóvil puede colocarse en el *mismo contenedor*. De forma predeterminada, todos los elementos que agregue a un contenedor se indexan automáticamente sin requerir ningún índice explícito o administración de esquema. Puede personalizar el comportamiento de la indización configurando el [directiva de indexación](index-overview.md) en un contenedor. 

Puede establecer [período de vida (TTL)](time-to-live.md) en los elementos seleccionados dentro de un contenedor de Azure Cosmos o para todo el contenedor purgar correctamente los elementos del sistema. Azure Cosmos DB eliminará automáticamente los elementos cuando expiren. También garantiza que una consulta realizada en el contenedor no devuelva los elementos que expiraron en un límite fijado. Para obtener más información, consulte [How to configure TTL on your container](how-to-time-to-live.md) (Cómo configurar el TTL en su contenedor).

Mediante el uso de [fuente de cambios](change-feed.md), puede suscribirse en el registro de operaciones que se administra para cada una de las particiones lógicas de su contenedor. La fuente de cambios le proporciona el registro de todas las actualizaciones realizadas en el contenedor junto con las imágenes de antes y después de los elementos. Consulte [cómo compilar aplicaciones reactivas con fuente de cambios](serverless-computing-database.md). También puede configurar la duración de retención para la fuente de cambio mediante el uso de la directiva en el contenedor de fuente de cambios. 

Puede registrar [procedimientos almacenados, desencadenadores, funciones definidas por el usuario (UDF)](stored-procedures-triggers-udfs.md) y [mezcla procedimientos](how-to-manage-conflicts.md) con el contenedor de Azure Cosmos. 

Puede especificar un [restricción unique key](unique-keys.md) en el contenedor de Azure Cosmos. Al crear una directiva de clave única, garantiza la exclusividad de uno o varios valores por clave de partición lógica. Una vez creado un contenedor con una directiva de clave única, este impide la creación de cualquier elemento nuevo o actualizado con valores que dupliquen los valores que haya especificado la restricción de clave única. Para obtener más información, consulte [Unique key constraints](unique-keys.md) (Restricciones de clave única).

Un contenedor de Azure Cosmos está especializado en entidades específicas de API de la siguiente manera:

| **Entidad de Azure Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Contenedor de Azure Cosmos | Colección | Tabla | Colección | Grafo | Tabla |

### <a name="properties-of-an-azure-cosmos-container"></a>Propiedades de un contenedor de Azure Cosmos

Un contenedor de Azure Cosmos tiene un conjunto de propiedades definido por el sistema. Según la API que elija, es posible que algunas de ellas no se expongan directamente. En la tabla siguiente se describe la lista de propiedades definido por el sistema:

| **Propiedad que define el sistema** | **Sistema configurables por el usuario o generado** | **Propósito** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_rid | Generado por el sistema | Identificador único del contenedor | Sí | No | No | No | Sin  |
|_etag | Generado por el sistema | Etiqueta de entidad que se usa para el control de simultaneidad optimista | Sí | No | No | No | Sin  |
|_ts | Generado por el sistema | Última actualización de la marca de tiempo del contenedor | Sí | No | No | No | Sin  |
|_self | Generado por el sistema | URI direccionable del contenedor | Sí | No | No | No | Sin  |
|id | Configurable por el usuario | Nombre único que ha definido el usuario del contenedor | Sí | Sí | Sí | Sí | Sí |
|indexingPolicy | Configurable por el usuario | Proporciona la capacidad para cambiar la ruta de acceso de índice, el tipo de índice y el modo de índice. | Sí | No | No | No | Sí |
|TimeToLive | Configurable por el usuario | Proporciona la capacidad de eliminar automáticamente elementos de un contenedor después de un determinado período de tiempo. Para obtener más información, lea el artículo sobre el [período de vida](time-to-live.md). | Sí | No | No | No | Sí |
|changeFeedPolicy | Configurable por el usuario | Se usa para leer los cambios realizados en los elementos de un contenedor. Para obtener más información, consulte el [fuente de cambios](change-feed.md) artículo. | Sí | No | No | No | Sí |
|uniqueKeyPolicy | Configurable por el usuario | Se utiliza para garantizar la unicidad de uno o varios valores dentro de una partición lógica. Para obtener más información, consulte el [las restricciones de clave única](unique-keys.md) artículo. | Sí | No | No | No | Sí |

### <a name="operations-on-an-azure-cosmos-container"></a>Operaciones en un contenedor de Azure Cosmos

Un contenedor de Azure Cosmos es compatible con las siguientes operaciones si se usa cualquiera de las API de Azure Cosmos.

| **operación** | **CLI de Azure** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- |
| Enumerar los contenedores en una base de datos | Sí | Sí | Sí | Sí | N/D | N/D |
| Leer un contenedor | Sí | Sí | Sí | Sí | N/D | N/D |
| Crear un contenedor nuevo | Sí | Sí | Sí | Sí | N/D | N/D |
| Actualizar el contenedor | Sí | Sí | Sí | Sí | N/D | N/D |
| Eliminar el contenedor | Sí | Sí | Sí | Sí | N/D | N/D |

## <a name="azure-cosmos-items"></a>Elementos de Azure Cosmos

Según la elección de la API, un elemento de Azure Cosmos puede representar cualquier un documento en una colección, una fila en una tabla o un nodo o borde en un gráfico. En la tabla siguiente se muestra la asignación entre las entidades específicas de API para un elemento de Azure Cosmos:

| **Entidad de Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Elemento de Azure Cosmos | Documento | Fila | Documento | Nodo o borde | item |

### <a name="properties-of-an-item"></a>Propiedades de un elemento

Cada elemento de Azure Cosmos tiene las siguientes propiedades que define el sistema. Según la API que elija, es posible que algunas de ellas no se expongan directamente.

|**Propiedad que define el sistema** | **Sistema configurables por el usuario o generado**| **Propósito** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_id | Generado por el sistema | Identificador único del elemento | Sí | No | No | No | Sin  |
|_etag | Generado por el sistema | Etiqueta de entidad que se usa para el control de simultaneidad optimista | Sí | No | No | No | Sin  |
|_ts | Generado por el sistema | La marca de tiempo de la última actualización del elemento | Sí | No | No | No | Sin  |
|_self | Generado por el sistema | URI direccionable del elemento | Sí | No | No | No | Sin  |
|id | Es posible usar el | Nombre único que define el usuario en una partición lógica. Si el usuario no especifica el identificador, el sistema generará automáticamente uno. | Sí | Sí | Sí | Sí | Sí |
|Propiedades arbitrarias que define el usuario | Definidas por el usuario | Las propiedades que define el usuario se presentan en la representación de la API nativa (JSON, BSON, CQL, etc.). | Sí | Sí | Sí | Sí | Sí |

### <a name="operations-on-items"></a>Operaciones en elementos

Un elemento de Azure Cosmos es compatible con las siguientes operaciones que se pueden realizar si se usa cualquiera de las API de Azure Cosmos.

| **operación** | **CLI de Azure** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- |
| Insertar, reemplazar, eliminar, upsert, leer | Sin  | Sí | Sí | Sí | Sí | Sí |

## <a name="next-steps"></a>Pasos siguientes

Ahora puede continuar para obtener información sobre los conceptos siguientes:

* [Cómo configurar el rendimiento aprovisionado en una base de datos de Azure Cosmos](how-to-provision-database-throughput.md)
* [Cómo configurar el rendimiento aprovisionado en un contenedor de Azure Cosmos](how-to-provision-container-throughput.md)
* [Logical partitions](partition-data.md) (Particiones lógicas)
* [How to configure TTL on Azure Cosmos container](how-to-time-to-live.md) (Cómo configurar TTL en un contenedor de Azure Cosmos)
* [How to build reactive applications using Change Feed](change-feed.md) (Cómo crear aplicaciones reactivas con la fuente de cambios).
* [How to configure unique key constraint on your Azure Cosmos container](unique-keys.md) (Cómo configurar la restricción de clave única en un contenedor de Azure Cosmos)
