---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB
description: Usar plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 4a32b0497d2457a740e9c082f990bb9112208bfd
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969185"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

Las tablas siguientes incluyen vínculos a las plantillas de Azure Resource Manager para Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL API (Core)

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, base de datos, contenedor](manage-sql-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de API de SQL (Core) en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos contenedores que compartan el rendimiento de nivel de base de datos. |
|[Actualización de procesamiento (RU/s) para una base de datos](manage-sql-with-resource-manager.md#database-ru-update) | Esta plantilla actualiza el rendimiento de una base de datos en una cuenta de API de SQL (Core). |
|[Actualización de procesamiento (RU/s) para un contenedor](manage-sql-with-resource-manager.md#container-ru-update) | Esta plantilla actualiza el rendimiento de un contenedor en una cuenta de API de SQL (Core). |

## <a name="mongodb-api"></a>MongoDB API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, base de datos, colección](manage-mongodb-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta con la API de Azure Cosmos DB para MongoDB en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos contenedores que compartan el rendimiento de nivel de base de datos. |
|[Actualización de procesamiento (RU/s) para una base de datos](manage-mongodb-with-resource-manager.md#database-ru-update) | Esta plantilla actualiza el rendimiento de una base de datos en una cuenta de API de MongoDB. |
|[Actualización de procesamiento (RU/s) para una colección](manage-mongodb-with-resource-manager.md#collection-ru-update) | Esta plantilla actualiza el rendimiento de un contenedor en una cuenta de API de MongoDB. |

## <a name="cassandra-api"></a>Cassandra API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, el espacio de claves, la tabla](manage-cassandra-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de API de Cassandra en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos tablas que compartan el rendimiento de nivel de espacio de claves. |
|[Actualización de procesamiento (RU/s) para un espacio de claves](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Esta plantilla actualiza el rendimiento de un espacio de claves en una cuenta de API de Cassandra. |
|[Actualización de procesamiento (RU/s) para una tabla](manage-cassandra-with-resource-manager.md#table-ru-update) | Esta plantilla actualiza el rendimiento de una tabla en una cuenta de API de Cassandra. |

## <a name="gremlin-api"></a>API de Gremlin

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, base de datos, gráfico](manage-gremlin-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de API de Gremlin en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos gráficos que compartan el rendimiento de nivel de base de datos. |
|[Actualización de procesamiento (RU/s) para una base de datos](manage-gremlin-with-resource-manager.md#database-ru-update) | Esta plantilla actualiza el rendimiento de una base de datos en una cuenta de API de Gremlin. |
|[Actualización de procesamiento (RU/s) para un gráfico](manage-gremlin-with-resource-manager.md#graph-ru-update) | Esta plantilla actualiza el rendimiento de un gráfico en una cuenta de API de Gremlin. |

## <a name="table-api"></a>Table API

|**Plantilla**|**Descripción**|
|---| ---|
|[Crear una cuenta de Azure Cosmos, tabla](manage-table-with-resource-manager.md#create-resource) | Esta plantilla crea una cuenta de Table API en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá una sola tabla. |
|[Actualización de procesamiento (RU/s) para una tabla](manage-table-with-resource-manager.md#table-ru-update) | Esta plantilla actualiza el rendimiento de una tabla en una cuenta de Table API. |

> [!TIP]
> Para habilitar rendimiento compartido cuando se usa la API de tabla, habilite el rendimiento de nivel de cuenta en el Portal de Azure.

Consulte [referencia ARM para Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) página de la documentación de referencia.