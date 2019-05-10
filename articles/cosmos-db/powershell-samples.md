---
title: Ejemplos de Azure PowerShell para Azure Cosmos DB
description: 'Ejemplos de Azure PowerShell: scripts para ayudarle a crear y administrar cuentas de Azure Cosmos DB.'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/08/2019
ms.author: mjbrown
ms.openlocfilehash: 68e845a05f4ebe2d1f25b55c00042c8925c8109e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65069296"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Ejemplos de Azure PowerShell para Azure Cosmos DB

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell de ejemplo para Azure Cosmos DB para Core (SQL) API.

| |  |
|---|---|
|**Cuentas de Azure Cosmos**||
|[Crear una cuenta](scripts/powershell/sql/ps-account-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta de Azure Cosmos SQL API. |
|[Obtención de una cuenta](scripts/powershell/sql/ps-account-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las propiedades de una cuenta de Azure Cosmos. |
|[Agregar una región](scripts/powershell/sql/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga una cuenta de Azure Cosmos y agregue una región a la lista de ubicaciones. |
|[Cambiar la prioridad de conmutación por error](scripts/powershell/sql/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambie la prioridad de conmutación por error de una cuenta de Azure Cosmos con un desencadenador de conmutación por error manual. |
|[Actualización de etiquetas](scripts/powershell/sql/ps-account-tags-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice las etiquetas de una cuenta de Azure Cosmos. |
|[Obtención de claves de cuenta](scripts/powershell/sql/ps-account-key-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las claves principal y secundaria de una cuenta de Azure Cosmos. |
|[Regeneración de claves de cuenta](scripts/powershell/sql/ps-account-key-regenerate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Regenere las claves principal y secundaria de una cuenta de Azure Cosmos. |
|[Enumeración de cadenas de conexión](scripts/powershell/sql/ps-account-connection-string-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las cadenas de conexión principal y secundaria de una cuenta de Azure Cosmos. |
|[Creación de un firewall de dirección IP](scripts/powershell/sql/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree un firewall de dirección IP para una cuenta de Azure Cosmos. |
|[Eliminación de una cuenta de Azure Cosmos](scripts/powershell/sql/ps-account-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Elimine una cuenta de Azure Cosmos. |
|**Bases de datos de Azure Cosmos**||
| [Creación de una base de datos](scripts/powershell/sql/ps-database-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree una base de datos dentro de una cuenta de Azure Cosmos.|
| [Creación de una base de datos con rendimiento en el nivel de base de datos/compatido](scripts/powershell/sql/ps-database-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree una base de datos de Azure Cosmos con rendimiento en el nivel de base de datos que se comparte con sus contenedores.|
| [Enumeración de todas las bases de datos](scripts/powershell/sql/ps-database-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Enumere todas las bases de datos de una cuenta de Azure Cosmos.|
| [Obtención de una base de datos](scripts/powershell/sql/ps-database-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtenga las propiedades de una base de datos de Azure Cosmos.|
|**Contenedores de Azure Cosmos**||
| [Creación de un contenedor](scripts/powershell/sql/ps-container-create.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con rendimiento dedicado.|
| [Creación de un contenedor con rendimiento compartido](scripts/powershell/sql/ps-container-create-shared.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con rendimiento compartido con otros contenedores de la base de datos.|
| [Creación de un contenedor con directiva de índice](scripts/powershell/sql/ps-container-create-index-custom.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con una directiva de índice personalizada.|
| [Creación de un contenedor sin directiva de índice](scripts/powershell/sql/ps-container-create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con directiva de índice desactivada.|
| [Creación de un contenedor con TTL y claves únicas](scripts/powershell/sql/ps-container-create-unique-key-ttl.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con una restricción de clave única y período de vida configurado.|
| [Creación de un contenedor con resolución de conflictos](scripts/powershell/sql/ps-container-create-conflict-policy.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cree un contenedor de Azure Cosmos con la directiva de resolución de conflictos de últimos casos en la que el último en escribir gana.|
| [Enumeración de todos los contenedores](scripts/powershell/sql/ps-container-list.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Enumere todos los contenedores de una base de datos de Azure Cosmos.|
| [Obtención de un contenedor](scripts/powershell/sql/ps-container-get.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Obtenga las propiedades de un contenedor de una base de datos de Azure Cosmos.|
| [Eliminación de un contenedor](scripts/powershell/sql/ps-container-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Elimine un contenedor de una base de datos de Azure Cosmos.|
|||