---
title: 'Ejemplo de la CLI: Agregar una base de datos única al grupo de conmutación por error en Azure SQL Database'
description: Script de ejemplo de la CLI de Azure para crear una base de datos única de Azure SQL Database, agregarla a un grupo de conmutación por error y probar la conmutación por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.openlocfilehash: 12e433a146d8dd8a6fb7e1d7c40acc72e023047b
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68736211"
---
# <a name="use-cli-to-add-an-azure-sql-database-single-database-into-a-failover-group"></a>Uso de la CLI para agregar una base de datos única de Azure SQL Database a un grupo de conmutación por error

En este script de ejemplo de PowerShell se crea una base de datos única, se crea un grupo de conmutación por error, se le agrega la base de datos y se prueba la conmutación por error. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este tema es preciso que ejecute la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add single database to failover group")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | Establece una suscripción como la suscripción activa actual. | 
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Crea un servidor de SQL Database que hospeda bases de datos únicas y grupos elásticos. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Crea las reglas de firewall de un servidor. | 
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Crea un grupo de conmutación por error. | 
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Enumera los grupos de conmutación por error de un servidor. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Establece el servidor principal del grupo de conmutación por error mediante la conmutación por error de todas las bases de datos desde el servidor principal actual. | 
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../sql-database-cli-samples.md).
