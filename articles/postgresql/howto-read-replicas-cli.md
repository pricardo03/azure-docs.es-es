---
title: 'Administración de réplicas de lectura desde la CLI de Azure para Azure Database for PostgreSQL: servidor único'
description: 'Obtenga información sobre cómo administrar réplicas de lectura desde la CLI de Azure en Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/28/2019
ms.openlocfilehash: 9a6a1a744a8441d2f082d4d14a3aba8aa1cfc09e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306036"
---
# <a name="create-and-manage-read-replicas-from-the-azure-cli"></a>Creación y administración de réplicas de lectura desde la CLI de Azure

En este artículo, obtendrá información sobre cómo crear y administrar réplicas de lectura en Azure Database for PostgreSQL desde la CLI de Azure. Para más información acerca de las réplicas de lectura, consulte la [introducción](concepts-read-replicas.md).

> [!IMPORTANT]
> Puede crear una réplica de lectura en la misma región que el servidor maestro o en cualquier otra región de Azure que prefiera. La replicación entre regiones se encuentra actualmente en versión preliminar pública.

## <a name="prerequisites"></a>Requisitos previos
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-up-azure-cli.md) que se usará como servidor maestro.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli). 


## <a name="prepare-the-master-server"></a>Preparación del servidor maestro
Estos pasos se deben utilizar para preparar un servidor maestro en los niveles de uso general u optimizado para memoria.

El parámetro `azure.replication_support` debe establecerse en **REPLICA** en el servidor maestro. Cuando se cambia este parámetro estático, es necesario reiniciar el servidor para que el cambio surta efecto.

1. Establezca `azure.replication_support` en REPLICA (réplica).

   ```azurecli-interactive
   az postgres server configuration set --resource-group myresourcegroup --server-name mydemoserver --name azure.replication_support --value REPLICA
   ```

2. Reinicie para aplicar el cambio en el servidor.

   ```azurecli-interactive
   az postgres server restart --name mydemoserver --resource-group myresourcegroup
   ```

## <a name="create-a-read-replica"></a>Creación de una réplica de lectura

El comando [az postgres server replica create](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-create) requiere los siguientes parámetros:

| Configuración | Valor de ejemplo | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group | myresourcegroup |  Grupo de recursos donde se creará el servidor de réplica.  |
| Nombre | mydemoserver-replica | Nombre del nuevo servidor de réplica que se crea. |
| source-server | mydemoserver | Nombre o identificador de recurso del servidor maestro existente desde el que replicar. |

En el siguiente ejemplo de la CLI, la réplica se crea en la misma región que el maestro.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup
```

Para crear una réplica de lectura entre regiones, use el parámetro `--location`. El siguiente ejemplo de la CLI crea la réplica en Oeste de EE. UU.

```azurecli-interactive
az postgres server replica create --name mydemoserver-replica --source-server mydemoserver --resource-group myresourcegroup --location westus
```

Si no ha establecido el parámetro `azure.replication_support` en **REPLICA** en un servidor maestro de uso general u optimizado para memoria y no ha reiniciado el servidor, recibirá un error. Complete estos dos pasos antes de crear una réplica.

Las réplicas se crean con la misma configuración de servidor que el servidor maestro. Después de crear una réplica, se pueden cambiar varias configuraciones independientemente del servidor maestro: generación de proceso, núcleos virtuales, almacenamiento y período de retención de copia de seguridad. El plan de tarifa también se puede modificar independientemente, excepto desde el nivel Básico.

> [!IMPORTANT]
> Antes de actualizar la configuración de un servidor maestro con nuevos valores, actualice la configuración de las réplicas a valores iguales o mayores. Esta acción garantiza que la réplica puede hacer frente a los cambios realizados en el servidor maestro.

## <a name="list-replicas"></a>Lista de réplicas
Puede ver la lista de réplicas de un servidor maestro con el comando [az postgres server replica list](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-list).

```azurecli-interactive
az postgres server replica list --server-name mydemoserver --resource-group myresourcegroup 
```

## <a name="stop-replication-to-a-replica-server"></a>Detención de la replicación en un servidor de réplica
Puede detener la replicación entre un servidor maestro y una réplica de lectura mediante el uso del comando [az postgres server replica stop](/cli/azure/postgres/server/replica?view=azure-cli-latest#az-postgres-server-replica-stop).

Después de detener la replicación en un servidor maestro y en una réplica de lectura, este proceso no se puede deshacer. La réplica de lectura se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor independiente no puede volver a convertirse en una réplica.

```azurecli-interactive
az postgres server replica stop --name mydemoserver-replica --resource-group myresourcegroup 
```

## <a name="delete-a-master-or-replica-server"></a>Eliminación de un servidor maestro o de réplica
Para eliminar un servidor maestro o de réplica, use el comando [az postgres server delete](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-delete).

Cuando se elimina un servidor maestro, la replicación se detiene en todas las réplicas de lectura. Las réplicas de lectura se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

```azurecli-interactive
az postgres server delete --name myserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes
Más información sobre las [réplicas de lectura en Azure Database for PostgreSQL](concepts-read-replicas.md).