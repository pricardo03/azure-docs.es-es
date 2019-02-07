---
title: 'Script de la CLI de Azure: escala de Azure Database for PostgreSQL'
description: 'Ejemplo de script de la CLI de Azure: escalado del servidor de Azure Database for PostgreSQL (Base de datos de Azure para PostgreSQL) a un nivel de rendimiento diferente después de consultar las métricas.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc
ms.topic: sample
ms.date: 04/05/2018
ms.openlocfilehash: 65958eb1e2d7cc4492e0437b24db77f00ebc7eba
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728340"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Supervisión y escalado de un solo servidor PostgreSQL mediante la CLI de Azure
Este script de la CLI de ejemplo escala un solo servidor de Azure Database for PostgreSQL (Base de datos de Azure para PostgreSQL) a un nivel de rendimiento diferente después de consultar las métricas. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Si decide ejecutar la CLI localmente, este artículo necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para comprobar la versión. Consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli) para instalar o actualizar su versión de la CLI de Azure.

## <a name="sample-script"></a>Script de ejemplo
En este script de ejemplo, va a modificar las líneas resaltadas para actualizar el nombre de usuario administrador y la contraseña a los suyos propios. Reemplace el identificador de suscripción usado en los comandos `az monitor` por su propio identificador de suscripción.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Use el comando siguiente para quitar el grupo de recursos y todos los recursos asociados a él después de ejecutarse el script de ejemplo. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Explicación del script
Este script usa los comandos que se describen en la tabla siguiente:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az postgres server create](/cli/azure/postgres/server) | Crea un servidor PostgreSQL que hospeda las bases de datos. |
| [az monitor metrics list](/cli/azure/monitor/metrics) | Especifica el valor de métrica de los recursos. |
| [az group delete](/cli/azure/group) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre la CLI de Azure, consulte: [Documentación de la CLI de Azure](/cli/azure)
- Pruebe otros scripts adicionales: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md) (Ejemplos de la CLI de Azure para Azure Database for PostgreSQL)
- Más información sobre el escalado: [Niveles de servicio](../concepts-service-tiers.md) y [Unidades de proceso y unidades de almacenamiento](../concepts-compute-unit-and-storage.md)
