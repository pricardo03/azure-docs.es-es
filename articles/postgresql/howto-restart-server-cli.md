---
title: Reiniciar una instancia de Azure Database for PostgreSQL con un único servidor con la CLI de Azure
description: En este artículo se describe cómo reiniciar una instancia de Azure Database for PostgreSQL con un único servidor mediante la CLI de Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 0a7cd815724fcebd6311860576e620eb9273523b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068974"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Reiniciar una instancia de Azure Database for PostgreSQL con un único servidor con la CLI de Azure
En este tema se describe cómo reiniciar un servidor de Azure Database for PostgreSQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.
 
El tiempo necesario para completar un reinicio depende del proceso de recuperación de PostgreSQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-up-azure-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Esta guía de procedimientos requiere el uso de la CLI de Azure versión 2.0 o posterior. Para confirmar la versión, en el símbolo del sistema de la CLI de Azure, escriba `az --version`. Para la instalación o la actualización, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Reinicio del servidor

Reinicie el servidor con el siguiente comando:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo establecer parámetros en Azure Database for PostgreSQL](howto-configure-server-parameters-using-cli.md)