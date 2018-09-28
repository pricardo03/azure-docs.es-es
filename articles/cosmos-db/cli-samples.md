---
title: Ejemplos de CLI de Azure para Azure Cosmos DB | Microsoft Docs
description: 'Ejemplos de la CLI de Azure: creación y administración de cuentas, bases de datos, contenedores, regiones y firewalls de Azure Cosmos DB.'
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.date: 11/29/2017
ms.author: sngun
ms.openlocfilehash: a6c6f61f9aa3e821ec048c1516c0e433c3c64e30
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963975"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Ejemplos de la CLI de Azure para Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de ejemplo de la CLI de Azure para Azure Cosmos DB. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Creación de cuentas, bases de datos y contenedores de Azure Cosmos DB**||
|[Creación de una cuenta de API de SQL](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta, base de datos y contenedor de API de Azure Cosmos DB para usarlos con la API de SQL. |
| [Creación de una cuenta de API de MongoDB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Creación de una única cuenta, base de datos y colección de API de MongoDB para Azure Cosmos DB. |
| [Creación de una cuenta de API de Gremlin](scripts/create-graph-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Creación de una única cuenta, base de datos y contenedor de API de Gremlin para Azure Cosmos DB. |
|**Escalado de Azure Cosmos DB**||
| [Escalado del rendimiento del contenedor](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cambia el rendimiento aprovisionada en un contenedor.|
|[Replicación de la cuenta de base de datos de Azure Cosmos DB en varias regiones y configuración de prioridades de conmutación por error](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replicación global de datos de la cuenta en varias regiones con una prioridad específica de conmutación por error.|
|**Protección de Azure Cosmos DB**||
| [Obtención de claves de cuenta](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtención de las claves de escritura maestras primaria y secundaria y de las claves de solo lectura primaria y secundaria de la cuenta.|
| [Obtención de la cadena de conexión de MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtención de la cadena de conexión para conectar su aplicación de MongoDB a su cuenta de Azure Cosmos DB.|
|[Regeneración de claves de cuenta](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Regeneración de la clave maestra o de solo lectura de la cuenta.|
|[Creación de un firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Creación de una directiva de control de acceso de IP de entrada para limitar el acceso a la cuenta desde un conjunto aprobado de máquinas o servicios en la nube.|
|**Alta disponibilidad, recuperación ante desastres, copia de seguridad y restauración**||
|[Configuración de la directiva de conmutación por error](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Establecimiento de la prioridad de conmutación por error de cada región en la que se replica la cuenta.|
|**Conexión de Azure Cosmos DB a los recursos**||
|[Conexión de una aplicación web a Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Creación y conexión a una base de datos de Azure Cosmos DB y creación de una aplicación web de Azure.|
|||
