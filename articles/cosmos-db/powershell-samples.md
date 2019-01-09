---
title: Ejemplos de Azure PowerShell para Azure Cosmos DB
description: 'Ejemplos de Azure PowerShell: scripts para ayudarle a crear y administrar cuentas de Azure Cosmos DB.'
services: cosmos-db
author: SnehaGunda
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 480bccd73a049d2f181d5acf906882b2f9b6ccdb
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53808407"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Ejemplos de Azure PowerShell para Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell de ejemplo para Azure Cosmos DB. En este momento, solo puede administrar la cuenta de Azure Cosmos DB mediante PowerShell. Los demás recursos como bases de datos y contenedores no se pueden administrar con PowerShell.

| |  |
|---|---|
|**Creación de una cuenta de Azure Cosmos DB**||
|[Creación y configuración de una cuenta de Cosmos con la API de SQL](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una sola cuenta de Azure Cosmos DB para usarla con la API de SQL. |
|[Creación y configuración de una cuenta de Cosmos con la API de Azure Cosmos DB para MongoDB](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una única cuenta de Cosmos con la API de Azure Cosmos DB para MongoDB. |
|[Creación y configuración de una cuenta de Cosmos con la API de Gremlin](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una sola cuenta de Azure Cosmos DB para usarla con la API de Gremlin. |
|[Creación y configuración de una cuenta de Cosmos con Cassandra API](scripts/create-and-configure-cassandra-database.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una sola cuenta de Azure Cosmos DB para usarla con Cassandra API. |
|[Creación y configuración de una cuenta de Cosmos con Table API](scripts/create-table-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una sola cuenta de Azure Cosmos DB para usarla con Table API. |
|**Escalado de Azure Cosmos DB**||
|[Replicación de la cuenta de Azure Cosmos DB en varias regiones y configuración de prioridades de conmutación por error](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Replicación global de datos de la cuenta en varias regiones con una prioridad específica de conmutación por error.|
|**Protección de Azure Cosmos DB**||
| [Obtención de las claves de la cuenta](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtención de las claves de escritura maestras primaria y secundaria y de las claves de solo lectura primaria y secundaria de la cuenta.|
| [Obtención de la cadena de conexión de MongoDB](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtención de la cadena de conexión para conectar su aplicación de MongoDB a su cuenta de Azure Cosmos DB.|
|[Regeneración de claves de cuenta](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Regeneración de la clave maestra o de solo lectura de la cuenta.|
|[Creación de un firewall](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Creación de una directiva de control de acceso de IP de entrada para limitar el acceso a la cuenta desde un conjunto aprobado de máquinas o servicios en la nube.|
|**Alta disponibilidad, recuperación ante desastres, copia de seguridad y restauración**||
|[Configuración de la directiva de conmutación por error](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Establecimiento de la prioridad de conmutación por error de cada región en la que se replica la cuenta.|
|||
