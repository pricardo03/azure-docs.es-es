---
title: Administración de réplicas de lectura para Azure Database for PostgreSQL desde la CLI de Azure
description: Obtenga información sobre cómo administrar Azure Database for PostgreSQL leer las réplicas de la CLI de Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: b5e0336a290090ed6bd7f5af508e691677780a80
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59789434"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Crear y administrar réplicas de lectura desde la CLI de Azure

En este artículo, aprenderá a crear y administrar las réplicas de lectura en la base de datos de Azure Database for PostgreSQL desde la CLI de Azure. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).

## <a name="prerequisites"></a>Requisitos previos
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-up-azure-cli.md) que se usará como servidor maestro.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli). 


## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Estos pasos se deben utilizar para preparar un servidor maestro en los niveles de uso general u optimizado para memoria.

El parámetro `azure.replication_support` debe establecerse en **REPLICA** en el servidor maestro. Cuando se cambia este parámetro estático, reiniciar el servidor es necesario para que el cambio surta efecto.

1. Establecer `azure.replication_support` a la réplica.

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Reiniciar para aplicar el cambio en el servidor.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Creación de una réplica de lectura

El [az postgres server réplica crear](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) comando requiere los siguientes parámetros:

| Configuración | Valor de ejemplo | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group | myresourcegroup |  El grupo de recursos donde se creará el servidor de réplica.  |
| Nombre | mydemoserver-replica | Nombre del nuevo servidor de réplica que se crea. |
| source-server | mydemoserver | El nombre o identificador de recurso del servidor principal existente para replicar desde. |

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Si no ha establecido la `azure.replication_support` parámetro **réplica** en un uso General u optimizadas para memoria de servidor maestro y reinicie el servidor, recibirá un error. Complete estos dos pasos antes de crear una réplica.

Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede modificar independientemente, excepto desde el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

## <a name="list-replicas"></a>Réplicas de la lista
Puede ver la lista de réplicas de un servidor maestro mediante [lista de réplicas de az postgres server](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list) comando.

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Detención de la replicación en un servidor de réplica
Puede detener la replicación entre un servidor principal y una réplica de lectura mediante el uso de [stop de réplica de az postgres server](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop) comando.

Después de detener la replicación en un servidor maestro y en una réplica de lectura, este proceso no se puede deshacer. La réplica de lectura se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Eliminación de un servidor principal o de réplica
Para eliminar un servidor principal o de réplica, usa el [eliminación de az postgres server](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete) comando.

Cuando se elimina un servidor maestro, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes
Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).