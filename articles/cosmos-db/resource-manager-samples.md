---
title: Plantillas de Azure Resource Manager para Azure Cosmos DB
description: Usar plantillas de Azure Resource Manager para crear y configurar Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: c907de019b64a6a9d03206514a1147fd43c78106
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078466"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Plantillas de Azure Resource Manager para Azure Cosmos DB

En la tabla siguiente incluye vínculos a las plantillas de Azure Resource Manager para Azure Cosmos DB:

|**Tipo de API** | **vínculo al ejemplo**| **Descripción** |
|---|---| ---|
|API de núcleo (SQL)| [Crear una cuenta de Azure Cosmos DB (varios maestros)](manage-sql-with-resource-manager.md) | Esta plantilla crea una cuenta de API de SQL en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos contenedores que compartan el rendimiento de nivel de base de datos. |
| MongoDB API | [Crear una cuenta de Azure Cosmos DB (varios maestros)](manage-mongodb-with-resource-manager.md) | Esta plantilla crea una cuenta con la API de Azure Cosmos DB para MongoDB en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos contenedores que compartan el rendimiento de nivel de base de datos. |
| Cassandra API | [Crear una cuenta de Azure Cosmos DB (varios maestros)](manage-cassandra-with-resource-manager.md) | Esta plantilla crea una cuenta de API de Cassandra en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos tablas que compartan el rendimiento de nivel de espacio de claves. |
| API de Gremlin| [Crear una cuenta de Azure Cosmos DB (varios maestros)](manage-gremlin-with-resource-manager.md) | Esta plantilla crea una cuenta de API de Gremlin en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá dos gráficos que compartan el rendimiento de nivel de base de datos. |
| Table API | [Crear una cuenta de Azure Cosmos DB (varios maestros)](manage-table-with-resource-manager.md) | Esta plantilla crea una cuenta de Table API en dos regiones con varios maestros habilitados. La cuenta de Azure Cosmos tendrá una sola tabla. |

> [!TIP]
> Para habilitar rendimiento compartido cuando se usa la API de tabla, habilite el rendimiento de nivel de cuenta en el Portal de Azure.

Consulte [referencia ARM para Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) página de la documentación de referencia.