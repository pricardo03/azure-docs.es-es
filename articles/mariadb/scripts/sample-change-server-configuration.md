---
title: 'Script de la CLI de Azure: cambio de configuraciones del servidor'
description: Este script de la CLI de ejemplo enumera todas las configuraciones del servidor disponibles y actualiza el valor de innodb_lock_wait_timeout.
services: mariadb
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: a2104b4a2a80dc7ca3f76edb6757a59fcc6c7a4f
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585263"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Enumeración y actualización de configuraciones de un servidor de Azure Database for MariaDB mediante la CLI de Azure
Este script de la CLI de ejemplo enumera todos los parámetros de configuración disponibles, así como sus valores permitidos para el servidor de Azure Database for MariaDB y establece *innodb_lock_wait_timeout* en un valor distinto del predeterminado.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Si decide ejecutar la CLI localmente, este artículo necesita la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para comprobar la versión. Consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli) para instalar o actualizar su versión de la CLI de Azure. 

## <a name="sample-script"></a>Script de ejemplo
En este script de ejemplo, va a modificar las líneas resaltadas para actualizar el nombre de usuario administrador y la contraseña a los suyos propios.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Use el comando siguiente para quitar el grupo de recursos y todos los recursos asociados a él después de ejecutarse el script de ejemplo.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicación del script
Este script usa los comandos que se describen en la tabla siguiente:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create) | Crea un servidor de MariaDB que hospeda las bases de datos. |
| [az mariadb server configuration list](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | Enumere las configuraciones de un servidor de Azure Database for MariaDB. |
| [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | Actualice la configuración de un servidor de Azure Database for MariaDB. |
| [az mariadb server configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) | Muestre la configuración de un servidor de Azure Database for MariaDB. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre la CLI de Azure: [Documentación de la CLI de Azure](/cli/azure).
- Pruebe otros scripts: [Ejemplos de la CLI de Azure para Azure Database for MariaDB](../sample-scripts-azure-cli.md).
- Para más información acerca de los parámetros del servidor, consulte [Configuración de parámetros del servidor en Azure Database for MariaDB](../howto-server-parameters.md).
