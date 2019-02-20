---
title: Niveles de coherencia y API de Azure Cosmos DB
description: Descripción de los niveles de coherencia entre las API de Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: b620ca76cfea296e504afffd91852308a01575db
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002020"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Niveles de coherencia y API de Azure Cosmos DB

Se admiten cinco modelos de coherencia que ofrece Azure Cosmos DB de forma nativa en SQL API. Cuando se usa Azure Cosmos DB, SQL API es la predeterminada. 

Azure Cosmos DB también proporciona compatibilidad nativa con API compatibles con el protocolo de conexión para bases de datos conocidas. Las bases de datos incluyen MongoDB, Apache Cassandra, Gremlin y Azure Table Storage. Estas bases de datos no ofrecen modelos de coherencia definidos con precisión ni garantías con respaldo de SLA para niveles de coherencia. Suelen proporcionar únicamente un subconjunto de los cinco modelos de coherencia que ofrece Azure Cosmos DB. Para SQL API, Gremlin API y Table API, se usa el nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos. 

En las secciones siguientes se muestra la asignación entre la coherencia de datos solicitada por un controlador cliente OSS para Apache Cassandra y MongoDB, y los niveles de coherencia correspondientes de Azure Cosmos DB.

## <a id="cassandra-mapping"></a>Asignación entre niveles de coherencia de Apache Cassandra y Azure Cosmos DB

En la tabla siguiente se describen las distintas combinaciones de coherencia que se pueden usar con la API de Cassandra y la asignación del nivel de coherencia nativa equivalente de Cosmos DB. Cosmos DB admite de forma nativa todas las combinaciones de los modos de lectura y escritura de Apache Cassandra. En todas las combinaciones del modelo de coherencia de lectura y escritura de Apache Cassandra, Cosmos DB le proporcionará garantías de coherencia iguales o mayores que Apache Cassandra. Además, Cosmos DB proporciona mayores garantías de durabilidad que Apache Cassandra, incluso en el modo de escritura más débil.

En la tabla siguiente se muestra la **asignación de coherencia de escritura** entre Azure Cosmos DB y Cassandra:

| Cassandra | Azure Cosmos DB | Garantía |
| - | - | - |
|ALL|Alta  | Linealidad |
| EACH_QUORUM   | Alta    | Linealidad | 
| QUORUM, SERIAL |  Alta |    Linealidad |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | De prefijo coherente |Prefijo coherente global |
| EACH_QUORUM   | Alta    | Linealidad |
| QUORUM, SERIAL |  Alta |    Linealidad |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | De prefijo coherente | Prefijo coherente global |
| QUORUM, SERIAL | Alta   | Linealidad |
| LOCAL_QUORUM, THREE, TWO, ONE, LOCAL_ONE, ANY | De prefijo coherente | Prefijo coherente global |
| LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE    | De obsolescencia entrelazada | <ul><li>Obsolescencia limitada.</li><li>Como máximo, versiones K o retraso t.</li><li>Lea el último valor confirmado en la región.</li></ul> |
| ONE, LOCAL_ONE, ANY   | De prefijo coherente | Prefijo coherente por región |

En la tabla siguiente se muestra la **asignación de coherencia de lectura** entre Azure Cosmos DB y Cassandra:

| Cassandra | Azure Cosmos DB | Garantía |
| - | - | - |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE | Alta  | Linealidad|
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Alta |   Linealidad |
|LOCAL_ONE, ONE | De prefijo coherente | Prefijo coherente global |
| ALL, QUORUM, SERIAL   | Alta    | Linealidad |
| LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  De prefijo coherente   | Prefijo coherente global |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM |    De prefijo coherente   | Prefijo coherente global |
| ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO   |Alta |   Linealidad |
| LOCAL_ONE, ONE    | De prefijo coherente | Prefijo coherente global|
| ALL, QUORUM, SERIAL   Linealidad fuerte
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |De prefijo coherente  | Prefijo coherente global |
|ALL    |Alta |Linealidad |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  |De prefijo coherente  |Prefijo coherente global|
|ALL, QUORUM, SERIAL    Linealidad fuerte
LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE  |De prefijo coherente  |Prefijo coherente global |
|ALL    |Alta | Linealidad |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | De prefijo coherente | Prefijo coherente global |
| QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE |  De obsolescencia entrelazada   | <ul><li>Obsolescencia limitada.</li><li>Como máximo, versiones K o retraso t. </li><li>Lea el último valor confirmado en la región.</li></ul>
| LOCAL_ONE, ONE |De prefijo coherente | Prefijo coherente por región |
| LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM  | De prefijo coherente | Prefijo coherente por región |


## <a id="mongo-mapping"></a>Asignación entre los niveles de coherencia de MongoDB 3.4 y Azure Cosmos DB

En la tabla siguiente se muestra la asignación de "problemas de lectura" entre MongoDB 3.4 y el nivel de coherencia predeterminado de Azure Cosmos DB. También se muestran las implementaciones de varias regiones y una sola región.

| **MongoDB 3.4** | **Azure Cosmos DB (varias regiones)** | **Azure Cosmos DB (región única)** |
| - | - | - |
| Linearizable | Alta | Alta |
| Mayoría | Uso vinculado | Alta |
| Local | Prefijo coherente | Prefijo coherente |

## <a name="next-steps"></a>Pasos siguientes

Lea más acerca de los niveles de coherencia y la compatibilidad entre las API de Azure Cosmos DB con las API de código abierto: Consulte los artículos siguientes:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Características de MongoDB que admite la API de Azure Cosmos DB para MongoDB](mongodb-feature-support.md)
* [Características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB](cassandra-support.md)