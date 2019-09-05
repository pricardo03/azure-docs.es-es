---
title: Creación y administración de réplicas de lectura en Azure Database for MySQL
description: En este artículo se describe cómo configurar y administrar réplicas de lectura en Azure Database for MySQL mediante la CLI de Azure.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 8a9366005ceef5a99fd66f0ec0cd53c98b481ee1
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309335"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli"></a>Procedimiento para crear y administrar réplicas de lectura en Azure Database for MySQL mediante la CLI de Azure

En este artículo, obtendrá información sobre cómo crear y administrar las réplicas de lectura en la misma región de Azure que el servidor maestro del servicio Azure Database for MySQL mediante la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

- [Instalación de la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Un [servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md) que se usará como servidor maestro. 

> [!IMPORTANT]
> La característica de réplica de lectura solo está disponible para servidores de Azure Database for MySQL en los planes de tarifa De uso general u Optimizada para memoria. Asegúrese de que el servidor maestro está en uno de estos planes de tarifa.

## <a name="create-a-read-replica"></a>Creación de una réplica de lectura

Un servidor de réplica de lectura se puede crear mediante el comando siguiente:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

El comando `az mysql server replica create` requiere los siguientes parámetros:

| Configuración | Valor de ejemplo | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupo de recursos donde se creará el servidor de réplica.  |
| Nombre | mydemoreplicaserver | Nombre del nuevo servidor de réplica que se crea. |
| source-server | mydemoserver | Nombre o identificador del servidor principal existente desde el que replicar. |

Para crear una réplica de lectura entre regiones, use el parámetro `--location`. El siguiente ejemplo de la CLI crea la réplica en Oeste de EE. UU.

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Para más información sobre las regiones en las que puede crear una réplica, consulte el [artículo sobre los conceptos de la réplica de lectura](concepts-read-replicas.md). 

> [!NOTE]
> Las réplicas de lectura se crean con la misma configuración de servidor que el servidor maestro. Una vez creado, se puede cambiar la configuración del servidor de réplica. Se recomienda mantener la configuración del servidor de réplica con valores iguales o mayores que el maestro para asegurarse de que la réplica trabajar al mismo nivel que el servidor maestro.

## <a name="stop-replication-to-a-replica-server"></a>Detención de la replicación en un servidor de réplica

> [!IMPORTANT]
> La detención la replicación en un servidor es irreversible. Una vez que detenida, la replicación entre un servidor maestro y una réplica no se puede deshacer. Después, el servidor de réplica se convierte en un servidor independiente que admite operaciones de lectura y escritura. Este servidor no puede volver a convertirse en una réplica.

La replicación de un servidor de réplica de lectura se puede detener mediante el comando siguiente:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

El comando `az mysql server replica stop` requiere los siguientes parámetros:

| Configuración | Valor de ejemplo | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupo de recursos donde se encuentra el servidor de réplica.  |
| Nombre | mydemoreplicaserver | Nombre del servidor de réplica para el que desea detener la replicación. |

## <a name="delete-a-replica-server"></a>Eliminación de un servidor de réplica

La eliminación de un servidor de réplica de lectura se puede realizar mediante la ejecución del comando **[az mysql server delete](/cli/azure/mysql/server)** .

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

## <a name="delete-a-master-server"></a>Eliminación de un servidor maestro

> [!IMPORTANT]
> Al eliminar un servidor maestro, se detiene la replicación en todos los servidores de réplica y se elimina el propio servidor maestro. Los servidores de réplica se convierten en servidores independientes que ahora admiten tanto lectura como escritura.

Para eliminar un servidor maestro, puede ejecutar el comando **[az mysql server delete](/cli/azure/mysql/server)** .

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="list-replicas-for-a-master-server"></a>Lista de réplicas de un servidor maestro

Para ver todas las réplicas de un determinado servidor maestro, ejecute el siguiente comando: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

El comando `az mysql server replica list` requiere los siguientes parámetros:

| Configuración | Valor de ejemplo | DESCRIPCIÓN  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupo de recursos donde se creará el servidor de réplica.  |
| server-name | mydemoserver | Nombre o identificador del servidor maestro. |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [las réplicas de lectura](concepts-read-replicas.md)
