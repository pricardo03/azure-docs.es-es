---
title: Ejemplo de la CLI para crear una base de datos de Azure SQL
description: Use este ejemplo de script de la CLI de Azure para crear una base de datos SQL.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: aa13e5f245a34387eee5fae48c896163cc2f7c4a
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73691707"
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Uso de la CLI para crear una base de datos de Azure SQL única y configurar una regla de firewall

Este script de ejemplo de la CLI de Azure crea una base de datos de Azure SQL y configura una regla de firewall en el nivel de servidor. Después de ejecutar el script correctamente, SQL Database es accesible desde todos los servicios de Azure y la dirección IP configurada.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh "Create SQL Database")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Crea un servidor de SQL Database que hospeda una base de datos única o un grupo elástico. |
| [az sql server firewall create](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create) | Crea una regla de firewall para permitir el acceso a todas las bases de datos únicas y grupos elásticos del servidor de SQL Database desde el intervalo de direcciones IP especificado. |
| [az sql db create](/cli/azure/sql/db#az-sql-db-create) | Crea una base de datos única o un grupo elástico. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../sql-database-cli-samples.md).
