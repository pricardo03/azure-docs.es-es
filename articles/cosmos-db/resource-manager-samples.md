---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB
description: Use las plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: ecdfe89e899e0d416784ae32a0d66b335c09e2b6
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582808"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

En las tablas siguientes se incluyen vínculos a plantillas de Azure Resource Manager para Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL API (Core)

|**Plantilla**|**Descripción**|
|---|---|
|[Crear una cuenta, una base de datos y un contenedor de Azure Cosmos](manage-sql-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de API de SQL (Core) en dos regiones con dos contenedores con rendimiento de base de datos compartido y un contenedor con rendimiento dedicado. El rendimiento se puede actualizar mediante el reenvío de la plantilla con el valor de la propiedad de rendimiento actualizado. |
|[Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con un procedimiento almacenado, un desencadenador y una UDF](manage-sql-with-resource-manager.md#create-sproc) | Esta plantilla crea una cuenta de API de SQL (Core) en dos regiones con un procedimiento almacenado, un desencadenador y una UDF para un contenedor. |

## <a name="mongodb-api"></a>MongoDB API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, una base de datos y una colección de Azure Cosmos](manage-mongodb-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta con la API de Azure Cosmos DB para MongoDB en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos contenedores que compartirán el rendimiento de nivel de base de datos. |
|[Actualizar el rendimiento (RU/s) de una base de datos](manage-mongodb-with-resource-manager.md#database-ru-update) | Con esta plantilla se actualiza el rendimiento de una base de datos en una cuenta de API de MongoDB. |
|[Actualizar el rendimiento (RU/s) de una colección](manage-mongodb-with-resource-manager.md#collection-ru-update) | Con esta plantilla se actualiza el rendimiento de un contenedor en una cuenta de API de MongoDB. |

## <a name="cassandra-api"></a>Cassandra API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, un espacio de claves y una tabla de Azure Cosmos](manage-cassandra-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de Cassandra API en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos tablas que compartirán el rendimiento de nivel de espacio de claves. |
|[Actualizar el rendimiento (RU/s) de un espacio de claves](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Con esta plantilla se actualiza el rendimiento de un espacio de claves en una cuenta de Cassandra API. |
|[Actualizar el rendimiento (RU/s) de una tabla](manage-cassandra-with-resource-manager.md#table-ru-update) | Con esta plantilla se actualiza el rendimiento de una tabla en una cuenta de Cassandra API. |

## <a name="gremlin-api"></a>API de Gremlin

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta, una base de datos y un grafo de Azure Cosmos](manage-gremlin-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de API de Gremlin en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá dos grafos que compartirán el rendimiento de nivel de base de datos. |
|[Actualizar el rendimiento (RU/s) de una base de datos](manage-gremlin-with-resource-manager.md#database-ru-update) | Con esta plantilla se actualiza el rendimiento de una base de datos en una cuenta de API de Gremlin. |
|[Actualizar el rendimiento (RU/s) de un grafo](manage-gremlin-with-resource-manager.md#graph-ru-update) | Con esta plantilla se actualiza el rendimiento de un grafo en una cuenta de API de Gremlin. |

## <a name="table-api"></a>Table API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, tabla](manage-table-with-resource-manager.md#create-resource) | Con esta plantilla se crea una cuenta de Table API en dos regiones con la arquitectura multimaestro habilitada. La cuenta de Azure Cosmos tendrá una sola tabla. |
|[Actualizar el rendimiento (RU/s) de una tabla](manage-table-with-resource-manager.md#table-ru-update) | Con esta plantilla se actualiza el rendimiento de una tabla en una cuenta de Table API. |

> [!TIP]
> Para habilitar rendimiento compartido cuando se usa Table API, habilite el rendimiento de nivel de cuenta en Azure Portal.

Consulte la página [ARM reference for Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) (Referencia de ARM para Azure Cosmos DB) para obtener documentación de referencia.
