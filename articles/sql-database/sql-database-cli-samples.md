---
title: Ejemplos de script de la CLI de Azure para SQL Database | Microsoft Docs
description: Ejemplos de script de la CLI de Azure para crear y administrar servidores de Azure SQL Database, grupos elásticos, bases de datos y firewalls.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 02/03/2019
ms.openlocfilehash: 7a1132b5857cf6c54d0566ca29bb76ce1ef88513
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59359434"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Ejemplos de la CLI de Azure para Azure SQL Database

Azure SQL Database se puede configurar con la <a href="/cli/azure">CLI de Azure</a>.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="single-database--elastic-pools"></a>Grupos elásticos y base de datos única

En la tabla siguiente se incluyen vínculos a ejemplos de script de la CLI de Azure para Azure SQL Database.

| |  |
|---|---|
|**Creación de una base de datos única y un grupo de bases de datos elásticas**||
| [Creación de una base de datos única y configuración de una regla de firewall](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Este ejemplo de la CLI crea una instancia de Azure SQL Database y configura una regla de firewall en el nivel de servidor. |
| [Creación de grupos de bases de datos elásticas y movimiento de bases de datos agrupadas](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Este ejemplo de script de la CLI crea grupos elásticos de SQL, traslada las bases de datos agrupadas de Azure SQL Database y cambia los tamaños de proceso.|
|**Escalado de una base de datos única y un grupo de bases de datos elásticas**||
| [Escalado de una base de datos única](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Este ejemplo de script de la CLI escala una sola base de datos de Azure SQL a un tamaño de proceso distinto después de consultar la información del tamaño de la base de datos. |
| [Escalado de un grupo elástico](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Este ejemplo de script de la CLI escala un grupo elástico de SQL a un tamaño de proceso distinto.  |
|||

Obtenga más información sobre la [API de la CLI de Azure de la base de datos única](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Instancia administrada

En la tabla siguiente se incluyen vínculos a ejemplos de script de la CLI de Azure para la instancia administrada de Azure SQL Database.

| |  |
|---|---|
| [Creación de una instancia administrada](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) | Este script de la CLI muestra cómo crear una Instancia administrada. |
| [Actualización de una instancia administrada](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/) | Este script de la CLI muestra cómo crear una Instancia administrada. |
| [Movimiento de una base de datos a otra Instancia administrada](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | Este script de la CLI muestra cómo restaurar una copia de seguridad de una base de datos de una instancia a otra. |
|||

Obtenga más información sobre la [API de la CLI de Azure de la instancia administrada](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances) y encuentre [más ejemplos aquí](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
