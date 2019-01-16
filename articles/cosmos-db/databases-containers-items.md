---
title: Trabajar con bases de datos, contenedores y elementos de Azure Cosmos DB
description: En este artículo se describe la forma de crear y usar bases de datos, contenedores y elementos de Azure Cosmos DB.
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: d5714e43c9ba58cdec33ca5fd1eae31eb6a88f51
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107742"
---
# <a name="working-with-azure-cosmos-databases-containers-and-items"></a>Trabajar con bases de datos, contenedores y artículos de Azure Cosmos.

Después de crear una [cuenta de Azure Cosmos DB](account-overview.md) en su suscripción a Azure, puede administrar los datos en su cuenta mediante la creación de bases de datos, contenedores y elementos. En este artículo se describe cada una de estas entidades: bases de datos, contenedores y elementos. En la siguiente imagen se muestra la jerarquía de diferentes entidades en una cuenta de Azure Cosmos:

![Entidades de cuenta de Azure Cosmos](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Bases de datos de Azure Cosmos

Puede crear una o varias bases de datos de Azure Cosmos en su cuenta. Una base de datos es análoga a un espacio de nombres, puesto que es la unidad de administración de un conjunto de contenedores de Azure Cosmos. En la siguiente tabla se muestra la forma de asignar una base de datos de Azure Cosmos a varias entidades específicas de API:

| **Entidad de Azure Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Base de datos de Azure Cosmos | Base de datos | Espacio de claves | Base de datos | Base de datos | N/D |

> [!NOTE]
> Con las cuentas de Table API, cuando crea su primera tabla también se crea automáticamente una base de datos predeterminada dentro de su cuenta de Azure Cosmos.

### <a name="operations-on-an-azure-cosmos-database"></a>Operaciones en una base de datos de Azure Cosmos

Puede interactuar con una base de datos de Azure Cosmos mediante las siguientes API de Azure Cosmos:

| **operación** | **CLI de Azure**|**SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- |
|Enumerar todas las bases de datos| SÍ | SÍ | Sí (la base de datos está asignada a un espacio de claves). | SÍ | N/D | N/D |
|Leer la base de datos| SÍ | SÍ | Sí (la base de datos está asignada a un espacio de claves). | SÍ | N/D | N/D |
|Create new database| SÍ | SÍ | Sí (la base de datos está asignada a un espacio de claves). | SÍ | N/D | N/D |
|Actualizar la base de datos| SÍ | SÍ | Sí (la base de datos está asignada a un espacio de claves). | SÍ | N/D | N/D |


## <a name="azure-cosmos-containers"></a>Contenedores de Azure Cosmos

Un contenedor de Azure Cosmos es la unidad de escalabilidad del rendimiento aprovisionado y el almacenamiento de elementos. Un contenedor se divide de forma horizontal y luego se replica en varias regiones. Los elementos que agregue al contenedor y el rendimiento que aprovisione en él se distribuyen automáticamente en un conjunto de particiones lógicas basadas en la clave de partición. Para obtener más información sobre cómo crear particiones y la clave de partición, consulte el artículo sobre [particiones lógicas](partition-data.md). 

Al crear un contenedor de Azure Cosmos, debe configurar el rendimiento de uno de los siguientes modos:

* Modo de **rendimiento aprovisionado dedicado**: el rendimiento aprovisionado en un contenedor está reservado exclusivamente para ese contenedor y lo respaldan los contratos de nivel de servicio. Para obtener más información, consulte [Aprovisionamiento del rendimiento de un contenedor de Azure Cosmos DB](how-to-provision-container-throughput.md).

* Modo de **rendimiento aprovisionado compartido**: estos contenedores comparten el rendimiento aprovisionado con otros contenedores en la misma base de datos (excepto aquellos contenedores que han sido configurados con un rendimiento aprovisionado dedicado). En otras palabras, el rendimiento aprovisionado en la base de datos se comparte entre todos los contenedores "compartidos". Para obtener más información, consulte [how to configure provisioned throughput on an Azure Cosmos database](how-to-provision-database-throughput.md) (Cómo configurar el rendimiento aprovisionado en una base de datos de Azure Cosmos.

Un contenedor de Azure Cosmos puede escalarse elásticamente, sin importar si crea contenedores con modos de rendimiento aprovisionado "compartido" o "dedicado".

Un contenedor de Azure Cosmos es un contenedor de elementos sin esquemas. Los elementos de un contenedor pueden tener esquemas arbitrarios. Por ejemplo, se pueden colocar en el mismo contenedor un elemento que representa a una persona y otro elemento que representa un automóvil. De forma predeterminada, todos los elementos que agregue a un contenedor se indexan automáticamente sin requerir ningún índice explícito o administración de esquema. Puede personalizar el comportamiento de la indexación configurando la directiva de indexación en un contenedor. 

Asimismo, puede configurar el período de vida (TTL) en los elementos seleccionados en un contenedor de Azure Cosmos o para que todo el contenedor elimine esos elementos del sistema sin problemas. Azure Cosmos DB eliminará automáticamente los elementos cuando expiren. También garantiza que una consulta realizada en el contenedor no devuelva los elementos que expiraron en un límite fijado. Para obtener más información, consulte [How to configure TTL on your container](how-to-time-to-live.md) (Cómo configurar el TTL en su contenedor).

Al usar la fuente de cambios, puede suscribirse al registro de operaciones que se administra para cada una de las particiones lógicas del contenedor. La fuente de cambios le proporciona el registro de todas las actualizaciones realizadas en el contenedor junto con las imágenes de antes y después de los elementos. Consulte [How to build reactive applications using change feed](change-feed.md) (Cómo crear aplicaciones reactivas con la fuente de cambios). También puede configurar la duración de la retención de la fuente de cambios usando la directiva de fuentes de cambios en el contenedor. 

Puede registrar procedimientos almacenados, desencadenadores, funciones que define el usuario (UDF) y procedimientos de combinación con su contenedor de Azure Cosmos. 

Puede especificar una clave única en el contenedor de Azure Cosmos. Al crear una directiva de clave única, garantiza la exclusividad de uno o varios valores por clave de partición lógica. Una vez creado un contenedor con una directiva de clave única, este impide la creación de cualquier elemento nuevo o actualizado con valores que dupliquen los valores que haya especificado la restricción de clave única. Para obtener más información, consulte [Unique key constraints](unique-keys.md) (Restricciones de clave única).

Un contenedor de Azure Cosmos está especializado en entidades específicas de API de la siguiente manera:

| **Entidad de Azure Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Contenedor de Azure Cosmos | Colección | Tabla | Colección | Grafo | Tabla |

### <a name="properties-of-an-azure-cosmos-container"></a>Propiedades de un contenedor de Azure Cosmos

Un contenedor de Azure Cosmos tiene un conjunto de propiedades que define el sistema. Según la API que elija, es posible que algunas de ellas no se expongan directamente. En la tabla siguiente se describe la lista de propiedades admitidas que define el sistema:

| **Propiedad que define el sistema** | **Configurable por el usuario o generado por el sistema** | **Propósito** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_rid | Generado por el sistema | Identificador único del contenedor | SÍ | No | No | No | Sin  |
|_etag | Generado por el sistema | Etiqueta de entidad que se usa para el control de simultaneidad optimista | SÍ | No | No | No | Sin  |
|_ts | Generado por el sistema | Última actualización de la marca de tiempo del contenedor | SÍ | No | No | No | Sin  |
|_self | Generado por el sistema | URI direccionable del contenedor | SÍ | No | No | No | Sin  |
|id | Configurable por el usuario | Nombre único que ha definido el usuario del contenedor | SÍ | Sí | Sí | Sí | SÍ |
|indexingPolicy | Configurable por el usuario | Proporciona la capacidad para cambiar la ruta de acceso del índice, su precisión y el modelo de coherencia. | SÍ | No | No | No | SÍ |
|TimeToLive | Configurable por el usuario | Proporciona la capacidad de eliminar automáticamente elementos de un contenedor después de un determinado período de tiempo. Para obtener más información, lea el artículo sobre el [período de vida](time-to-live.md). | SÍ | No | No | No | SÍ |
|changeFeedPolicy | Configurable por el usuario | Se usa para leer los cambios realizados en los elementos de un contenedor. Para obtener más información, consulte el artículo referente a la [fuente de cambios](change-feed.md). | SÍ | No | No | No | SÍ |
|uniqueKeyPolicy | Configurable por el usuario | Con las claves únicas, se garantiza la exclusividad de uno o varios valores dentro de una partición lógica. Para obtener más información, consulte el artículo referente a las [claves únicas](unique-keys.md). | SÍ | No | No | No | SÍ |

### <a name="operations-on-an-azure-cosmos-container"></a>Operaciones en un contenedor de Azure Cosmos

Un contenedor de Azure Cosmos es compatible con las siguientes operaciones si se usa cualquiera de las API de Azure Cosmos.

| **operación** | **CLI de Azure** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Enumerar los contenedores en una base de datos | Sí* | SÍ | Sí | SÍ | N/D | N/D |
| Leer un contenedor | SÍ | Sí | Sí | SÍ | N/D | N/D |
| Crear un contenedor nuevo | SÍ | Sí | Sí | SÍ | N/D | N/D |
| Actualizar el contenedor | SÍ | Sí | Sí | SÍ | N/D | N/D |
| Eliminar el contenedor | SÍ | Sí | Sí | SÍ | N/D | N/D |

## <a name="azure-cosmos-items"></a>Elementos de Azure Cosmos

Según la elección de la API, un elemento de Azure Cosmos puede representar cualquier un documento en una colección, una fila en una tabla o un nodo o borde en un gráfico. En la tabla siguiente se muestra la asignación entre las entidades específicas de API para un elemento de Azure Cosmos:

| **Entidad de Cosmos**  | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- |
|Elemento de Azure Cosmos | Documento | Fila | Documento | Nodo o borde | item |

### <a name="properties-of-an-item"></a>Propiedades de un elemento

Cada elemento de Azure Cosmos tiene las siguientes propiedades que define el sistema. Según la API que elija, es posible que algunas de ellas no se expongan directamente.

|**Propiedad que define el sistema** | **Configurable por el usuario o generado por el sistema**| **Propósito** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
|_id | Generado por el sistema | Identificador único del elemento | SÍ | No | No | No | Sin  |
|_etag | Generado por el sistema | Etiqueta de entidad que se usa para el control de simultaneidad optimista | SÍ | No | No | No | Sin  |
|_ts | Generado por el sistema | Última actualización de la marca de tiempo del elemento | SÍ | No | No | No | Sin  |
|_self | Generado por el sistema | URI direccionable del elemento | SÍ | No | No | No | Sin  |
|id | Es posible usar el | Nombre único que define el usuario en una partición lógica. Si el usuario no especifica el id., el sistema generará automáticamente uno. | SÍ | Sí | Sí | Sí | SÍ |
|Propiedades arbitrarias que define el usuario | Definidas por el usuario | Las propiedades que define el usuario se presentan en la representación de la API nativa (JSON, BSON, CQL, etc.). | SÍ | Sí | Sí | Sí | SÍ |

### <a name="operations-on-items"></a>Operaciones en elementos

Un elemento de Azure Cosmos es compatible con las siguientes operaciones que se pueden realizar si se usa cualquiera de las API de Azure Cosmos.

| **operación** | **CLI de Azure** | **SQL API** | **Cassandra API** | **API de Azure Cosmos DB para MongoDB** | **Gremlin API** | **Table API** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Insertar, reemplazar, eliminar, upsert, leer | Sin  | SÍ | Sí | Sí | Sí | SÍ |

## <a name="next-steps"></a>Pasos siguientes

Ya está preparado para obtener información sobre cómo aprovisionar el rendimiento de la cuenta de Azure Cosmos o consultar otros conceptos:

* [How to configure provisioned throughput on an Azure Cosmos database](how-to-provision-database-throughput.md) (Cómo configurar el rendimiento aprovisionado en una base de datos de Azure Cosmos.
* [How to configure provisioned throughput on an Azure Cosmos container](how-to-provision-container-throughput.md) (Cómo configurar el rendimiento aprovisionado en un contenedor de Azure Cosmos.
* [Logical partitions](partition-data.md) (Particiones lógicas)
* [How to configure TTL on Azure Cosmos container](how-to-time-to-live.md) (Cómo configurar TTL en un contenedor de Azure Cosmos)
* [How to build reactive applications using Change Feed](change-feed.md) (Cómo crear aplicaciones reactivas con la fuente de cambios).
* [How to configure unique key constraint on your Azure Cosmos container](unique-keys.md) (Cómo configurar la restricción de clave única en un contenedor de Azure Cosmos)
