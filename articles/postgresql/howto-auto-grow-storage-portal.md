---
title: Crecimiento automático con el portal de Azure en Azure Database for PostgreSQL - único servidor de almacenamiento
description: Este artículo describe cómo se puede habilitar automáticamente aumentar el almacenamiento mediante Azure portal en la base de datos de Azure para PostegreSQL - servidor único
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 9f88fe3e8a30dd80c5331bd6c8ea7aec401c6fb9
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676764"
---
# <a name="auto-grow-storage-using-the-azure-portal-in-azure-database-for-postgresql---single-server"></a>Crecimiento automático con el portal de Azure en Azure Database for PostgreSQL - único servidor de almacenamiento
En este artículo se describe cómo puede configurar una base de datos de Azure para PostgreSQL con almacenamiento de servidor para que crezca sin que afecte a la carga de trabajo.

Cuando un servidor alcanza el límite de almacenamiento asignado, el servidor está marcado como de solo lectura. Sin embargo, si habilita el crecimiento automático de almacenamiento, aumenta el almacenamiento del servidor para alojar los datos cada vez mayores. Para los servidores con menos de 100 GB aprovisionar almacenamiento, 5 GB aumenta el tamaño de almacenamiento aprovisionado tan pronto como el almacenamiento disponible está por debajo de la mayor de 1 GB o 10% del almacenamiento aprovisionado. Para los servidores con más de 100 GB de almacenamiento aprovisionado, 5% aumenta el tamaño de almacenamiento aprovisionado cuando el espacio de almacenamiento libre está por debajo del 5% del tamaño de almacenamiento aprovisionado. Almacenamiento máximo limita tal como se especifica [aquí](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) aplicar.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md).

## <a name="enable-storage-auto-grow"></a>Habilitar almacenamiento auto crezca 

Siga estos pasos para establecer el crecimiento automático de almacenamiento del servidor de PostgreSQL:

1. En el [portal Azure](https://portal.azure.com/), seleccione el servidor Azure Database for PostgreSQL.

2. En la página de servidor de PostgreSQL en **configuración**, haga clic en **tarifa** para abrir la página de nivel de precios.

3. En el **crecimiento automático** sección, seleccione **Sí** para habilitar el crecimiento automático de almacenamiento.

    ![Azure Database for PostgreSQL - Settings_Pricing_tier - crecimiento automático](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Haga clic en **Aceptar** para guardar los cambios.

5. Una notificación se confirmará el crecimiento automático se habilitó correctamente.

    ![Azure Database for PostgreSQL: éxito de crecimiento automático](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo crear alertas en métricas](howto-alert-on-metric.md).
