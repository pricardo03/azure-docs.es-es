---
title: Niveles de coherencia y API de Azure Cosmos DB | Microsoft Docs
description: Descripción de los niveles de coherencia entre las API de Azure Cosmos DB.
keywords: coherencia, azure cosmos db, azure, modelos, mongodb, cassandra, gráfico, tabla, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 974531cd5907e4f69e7d064125d3e51fa4974949
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50956390"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Niveles de coherencia y API de Azure Cosmos DB

Los cinco modelos de coherencia que ofrece Azure Cosmos DB se admiten de forma nativa por SQL API de Cosmos DB, que es la API predeterminada cuando se utiliza Cosmos DB. Además de la API de SQL, Cosmos DB también proporciona compatibilidad nativa para las API compatibles de protocolo de transferencia para bases de datos populares como MongoDB, Apache Cassandra, Gremlin y Azure Tables. Estas bases de datos no ofrecen modelos de coherencia definidos con precisión ni las garantías de respaldo SLA para los niveles de coherencia. Estas bases de datos suelen proporcionan únicamente un subconjunto de los cinco modelos de coherencia que ofrece Cosmos DB. Para SQL API, Gremlin API y Table API, se utiliza el nivel de coherencia predeterminado configurado en la cuenta de Cosmos.

En las secciones siguientes se muestra la asignación entre la coherencia de datos solicitada por un controlador cliente OSS para Apache Cassandra 4.x y MongoDB 3.4 cuando se usa Cassandra API y MongoDB API, respectivamente y los niveles de coherencia de Cosmos DB correspondientes.

## <a id="cassandra-mapping"></a>Asignación de niveles de coherencia entre Apache Cassandra y Cosmos DB

En la tabla siguiente se muestra la asignación para la "coherencia de lectura" entre el cliente de Apache Cassandra 4.x y el nivel de coherencia predeterminado de Cosmos DB para una implementación de varias regiones y una sola región.

| **Apache Cassandra 4.x** | **Cosmos DB (varias regiones)** | **Cosmos DB (región única)** |
| - | - | - |
| ONE, TWO, THREE | De prefijo coherente | De prefijo coherente |
| LOCAL_ONE | De prefijo coherente | De prefijo coherente |
| QUORUM, ALL, SERIAL | Obsolescencia limitada (valor predeterminado) o alta (en versión preliminar privada) | Alta |
| LOCAL_QUORUM | De obsolescencia entrelazada | Alta |
| LOCAL_SERIAL | De obsolescencia entrelazada | Alta |

## <a id="mongo-mapping"></a>Asignación entre los niveles de coherencia de MongoDB 3.4 y Cosmos DB

En la tabla siguiente se muestra la asignación de "read concerns" de MongoDB 3.4 con el nivel de coherencia predeterminado de Cosmos DB para una implementación de varias regiones o de una sola región.

| **MongoDB 3.4** | **Cosmos DB (varias regiones)** | **Cosmos DB (región única)** |
| - | - | - |
| Linearizable | Alta | Alta |
| Mayoría | De obsolescencia entrelazada | Alta |
| Local | De prefijo coherente | De prefijo coherente |

## <a name="next-steps"></a>Pasos siguientes

Lea más acerca de los niveles de coherencia y la compatibilidad entre las API de Cosmos DB con las API de código abierto en los siguientes artículos:

* [Availability and performance tradeoffs for various consistency levels](consistency-levels-tradeoffs.md) (Compromisos entre rendimiento y disponibilidad en los distintos niveles de coherencia)
* [Compatibilidad de Cosmos DB MongoDB API con las características de MongoDB](mongodb-feature-support.md)
* [Características de Apache Cassandra admitidas por Cassandra API de Cosmos DB](cassandra-support.md)