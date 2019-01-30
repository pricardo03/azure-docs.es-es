---
title: Niveles de coherencia y API de Azure Cosmos DB
description: Descripción de los niveles de coherencia entre las API de Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: 4d2994ea6ab6d6472ec56f0f2e378062590c8920
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54807004"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Niveles de coherencia y API de Azure Cosmos DB

Se admiten cinco modelos de coherencia que ofrece Azure Cosmos DB de forma nativa en SQL API de Azure Cosmos DB. Cuando se usa Azure Cosmos DB, SQL API es la predeterminada. 

Azure Cosmos DB también proporciona compatibilidad nativa con API compatibles con el protocolo de conexión para bases de datos conocidas. Las bases de datos incluyen MongoDB, Apache Cassandra, Gremlin y Azure Table Storage. Estas bases de datos no ofrecen modelos de coherencia definidos con precisión ni garantías con respaldo de SLA para niveles de coherencia. Suelen proporcionar únicamente un subconjunto de los cinco modelos de coherencia que ofrece Azure Cosmos DB. Para SQL API, Gremlin API y Table API, se usa el nivel de coherencia predeterminado configurado en la cuenta de Azure Cosmos DB. 

En las secciones siguientes se muestra la asignación entre la coherencia de datos solicitada por un controlador cliente de OSS para Apache Cassandra 4.x y MongoDB 3.4. El documento también muestra los niveles de coherencia de Azure Cosmos DB correspondientes para Apache Cassandra y MongoDB.

## <a id="cassandra-mapping"></a>Asignación entre niveles de coherencia de Apache Cassandra y Azure Cosmos DB

En esta tabla se muestra la asignación de coherencia entre Apache Cassandra y el nivel de coherencia en Azure Cosmos DB. Para cada nivel de coherencia de lectura y escritura de Cassandra, el correspondiente nivel de coherencia de Cosmos DB proporciona garantías más seguras, es decir, más estrictas.

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