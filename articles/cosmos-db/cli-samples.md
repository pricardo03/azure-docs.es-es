---
title: Ejemplos de la CLI de Azure para Azure Cosmos DB
description: 'Ejemplos de la CLI de Azure: creación y administración de cuentas, bases de datos, contenedores, regiones y firewalls de Azure Cosmos DB.'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 48beb93fbc5952951fff1ed31e5f8625faf78ccd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850404"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Ejemplos de la CLI de Azure para Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de ejemplo de la CLI de Azure para Azure Cosmos DB. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](/cli/azure/cosmosdb).

| |  |
|---|---|
|**Creación de cuentas, bases de datos y contenedores de Azure Cosmos DB**||
| [Creación de una cuenta de API de SQL](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una única cuenta, base de datos y contenedor de SQL API para Azure Cosmos DB. |
| [Creación de una cuenta de API de MongoDB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Creación de una única cuenta, base de datos y colección de API de MongoDB para Azure Cosmos DB. |
| [Creación de una cuenta de API de Gremlin](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una única cuenta, base de datos y grafo de Gremlin API para Azure Cosmos DB. |
| [Creación de una cuenta de Cassandra API](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una sola cuenta y base de datos de Cassandra API para Azure Cosmos DB. |
| [Creación de una cuenta de Table API](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Crea una única cuenta, base de datos y tabla de Table API para Azure Cosmos DB. |
|**Escalado de Azure Cosmos DB**||
| [Escalado del rendimiento del contenedor](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cambia el rendimiento aprovisionada en un contenedor.|
| [Replicación de la cuenta de base de datos de Azure Cosmos DB en varias regiones y configuración de prioridades de conmutación por error](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replicación global de datos de la cuenta en varias regiones con una prioridad específica de conmutación por error.|
|**Protección de Azure Cosmos DB**||
| [Obtención de las claves de la cuenta](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtención de las claves de escritura maestras primaria y secundaria y de las claves de solo lectura primaria y secundaria de la cuenta.|
| [Obtención de la cadena de conexión de MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtención de la cadena de conexión para conectar su aplicación de MongoDB a su cuenta de Azure Cosmos DB.|
| [Regeneración de claves de cuenta](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Vuelva a generar las claves de la cuenta.|
| [Creación de un firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una directiva de control de acceso de IP de entrada para limitar el acceso a la cuenta desde un conjunto aprobado de máquinas o servicios en la nube.|
|**Alta disponibilidad, recuperación ante desastres, copia de seguridad y restauración**||
| [Configuración de la directiva de conmutación por error](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Establecimiento de la prioridad de conmutación por error de cada región en la que se replica la cuenta.|
|**Conexión de Azure Cosmos DB a los recursos**||
| [Conexión de una aplicación web a Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Creación y conexión a una base de datos de Azure Cosmos DB y creación de una aplicación web de Azure.|
|||
