---
title: Crecimiento automático de almacenamiento de Azure Database for MariaDB con Azure portal
description: Este artículo describe cómo se puede habilitar automáticamente aumentar el almacenamiento de Azure Database for MariaDB con Azure portal
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: bb3291b66776a5f0f6be16069b2d6a999b2d1f32
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676884"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-using-the-azure-portal"></a>Crecimiento automático de almacenamiento de Azure Database for MariaDB con Azure portal
En este artículo se describe cómo puede configurar una base de datos de Azure para el almacenamiento del servidor de MariaDB crezca sin afectar a la carga de trabajo.

Cuando un servidor alcanza el límite de almacenamiento asignado, el servidor está marcado como de solo lectura. Sin embargo, si habilita el crecimiento automático de almacenamiento, aumenta el almacenamiento del servidor para alojar los datos cada vez mayores. Para los servidores con menos de 100 GB aprovisionar almacenamiento, 5 GB aumenta el tamaño de almacenamiento aprovisionado tan pronto como el almacenamiento disponible está por debajo de la mayor de 1 GB o 10% del almacenamiento aprovisionado. Para los servidores con más de 100 GB de almacenamiento aprovisionado, 5% aumenta el tamaño de almacenamiento aprovisionado cuando el espacio de almacenamiento libre está por debajo del 5% del tamaño de almacenamiento aprovisionado. Almacenamiento máximo limita tal como se especifica [aquí](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) aplicar.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [-Azure Database for MariaDB server](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Habilitar almacenamiento auto crezca 

Siga estos pasos para establecer el crecimiento automático de almacenamiento del servidor de MariaDB:

1. En el [portal Azure](https://portal.azure.com/), seleccione el servidor Azure Database for MariaDB.

2. En la página de servidor de MariaDB en **configuración** encabezado, haga clic en **tarifa** para abrir la página de nivel de precios.

3. En la sección del crecimiento automático, seleccione **Sí** para habilitar el crecimiento automático de almacenamiento.

    ![Azure Database for MariaDB - Settings_Pricing_tier - crecimiento automático](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Haga clic en **Aceptar** para guardar los cambios.

5. Una notificación se confirmará el crecimiento automático se habilitó correctamente.

    ![Azure Database for MariaDB - correcto de crecimiento automático](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo crear alertas en métricas](howto-alert-metric.md).
