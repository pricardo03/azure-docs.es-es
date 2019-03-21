---
title: Reinicio de un servidor de Azure Database for PostgreSQL mediante Azure Portal
description: En este artículo se describe cómo puede reiniciar un servidor Azure Database for PostgreSQL mediante Azure portal.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/18/2019
ms.openlocfilehash: 83947571ed835c53c6cf8da6e73deb8dceabdd62
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58168782"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-portal"></a>Reiniciar la base de datos de Azure para el servidor de PostgreSQL mediante Azure portal
En este tema se describe cómo reiniciar un servidor de Azure Database for PostgreSQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.
 
El tiempo necesario para completar un reinicio depende del proceso de recuperación de PostgreSQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor y una base de datos de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md).

## <a name="perform-server-restart"></a>Realización del reinicio del servidor

Los pasos siguientes reinician el servidor de PostgreSQL:

1. En el [portal Azure](https://portal.azure.com/), seleccione el servidor Azure Database for PostgreSQL.

2. En la barra de herramientas de la página de **información general** del servidor, haga clic en **Restart** (reiniciar).

   ![Azure Database for PostgreSQL - Información general - Botón Reiniciar](./media/howto-restart-server-portal/2-server.png)

3. Haga clic en **Sí** para confirmar el reinicio del servidor.

   ![Azure Database for PostgreSQL - Confirmación del reinicio](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que el estado del servidor cambia a "Reiniciando".

   ![Azure Database for PostgreSQL - Estado del reinicio](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme que el reinicio del servidor es correcto.

   ![Azure Database for PostgreSQL - Reinicio correcto](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo establecer los parámetros en la base de datos de Azure Database for PostgreSQL](howto-configure-server-parameters-using-portal.md)