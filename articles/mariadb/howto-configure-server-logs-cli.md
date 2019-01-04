---
title: Acceso a los registros del servidor en Azure Database for MariaDB mediante la CLI de Azure
description: En este artículo se describe cómo acceder a los registros del servidor de Azure Database for MariaDB mediante la utilidad de la línea de comandos de la CLI de Azure.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 403f111882b1bd151b26af56fd614355e7ba88e3
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539218"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configuración y acceso a los registros del servidor con la CLI de Azure
Puede descargar los registros del servidor de Azure Database for MariaDB mediante la CLI de Azure, la utilidad de la línea de comandos de Azure.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- [Servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md)
- La [CLI de Azure](/cli/azure/install-azure-cli) o Azure Cloud Shell en el explorador

## <a name="configure-logging-for-azure-database-for-mariadb"></a>Configuración del registro para Azure Database for MariaDB
Puede configurar el servidor para acceder al registro de consultas lentas de MariaDB con los pasos siguientes:
1. Active el registro estableciendo el parámetro **slow\_query\_log** en ON.
2. Ajuste otros parámetros, como **long\_query\_time**  y  **log\_slow\_admin\_statements**.

Para aprender a establecer el valor de estos parámetros mediante la CLI de Azure, consulte [Cómo configurar parámetros del servidor](howto-configure-server-parameters-cli.md).

Por ejemplo, el siguiente comando de la CLI activará el registro de consultas lentas, establecerá el tiempo de consultas largas en 10 segundos y desactivará el registro de la instrucción de administración lenta. Por último, se muestran las opciones de configuración para su revisión.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>Lista de registros del servidor de Azure Database for MariaDB
Para mostrar la lista de archivos de registro disponibles para el servidor, ejecute el comando [az mariadb server-logs list](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list).

Puede enumerar los archivos de registro del servidor **mydemoserver.mariadb.database.azure.com** en el grupo de recursos **myresourcegroup**. A continuación, dirija la lista de archivos de registro a un archivo de texto denominado **log\_files\_list.txt**.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Descarga de registros del servidor
Con el comando [az mariadb server-logs download](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download), puede descargar archivos de registro individuales del servidor.

Use el ejemplo siguiente para descargar el archivo de registro específico del servidor **mydemoserver.mariadb.database.azure.com** en el grupo de recursos **myresourcegroup** a su entorno local.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre los [registros del servidor de Azure Database for MariaDB](concepts-server-logs.md).
