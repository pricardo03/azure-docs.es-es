---
title: Ejemplos de Azure PowerShell para Azure Cosmos DB
description: 'Ejemplos de Azure PowerShell: scripts para ayudarle a crear y administrar cuentas de Azure Cosmos DB.'
author: SnehaGunda
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 3498ac6a2a4aaa1682d7b5bc5aae5383866d5bcd
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873804"
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
