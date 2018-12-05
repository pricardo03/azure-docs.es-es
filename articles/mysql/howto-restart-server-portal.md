---
title: Reinicio de un servidor de Azure Database for MySQL mediante Azure Portal
description: En este artículo se describe cómo reiniciar un servidor de Azure Database for MySQL mediante Azure Portal.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 11/16/2018
ms.openlocfilehash: 278b535e57dc4d3e79c1ca4d08c89f3e4d833325
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52166888"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Reinicio de un servidor de Azure Database for MySQL mediante Azure Portal
En este tema se describe cómo reiniciar un servidor de Azure Database for MySQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.

El tiempo necesario para completar un reinicio depende el proceso de recuperación de MySQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se están ejecutando en el servidor antes del reinicio.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for MySQL y una base de datos](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Realización del reinicio del servidor

Los pasos siguientes reinician el servidor de MySQL:

1. En Azure Portal, seleccione el servidor de Azure Database for MySQL.

2. En la barra de herramientas de la página de **información general** del servidor, haga clic en **Restart** (reiniciar).

   ![Azure Database for MySQL - Información general - Botón Reiniciar](./media/howto-restart-server-portal/2-server.png)

3. Haga clic en **Sí** para confirmar el reinicio del servidor. 

   ![Azure Database for MySQL - Confirmación del reinicio ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que el estado del servidor cambia a "Reiniciando".

   ![Azure Database for MySQL - Estado del reinicio ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme que el reinicio del servidor es correcto.

   ![Azure Database for MySQL - Reinicio correcto ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Creación de un servidor de Azure Database for MySQL con Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)