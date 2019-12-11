---
title: Reinicio de un servidor mediante Azure Portal en Azure Database for MySQL
description: En este artículo se describe cómo reiniciar un servidor de Azure Database for MySQL mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 1857c67a77b9600f3d8f7c222a8e06f899fba728
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774058"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Reinicio de un servidor de Azure Database for MySQL mediante Azure Portal
En este tema se describe cómo reiniciar un servidor de Azure Database for MySQL. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.

El tiempo necesario para completar un reinicio depende el proceso de recuperación de MySQL. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio.

## <a name="prerequisites"></a>Requisitos previos
Para completar esta guía, necesita:
- Un [servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Realización del reinicio del servidor

Los pasos siguientes reinician el servidor de MySQL:

1. En Azure Portal, seleccione el servidor de Azure Database for MySQL.

2. En la barra de herramientas de la página de **información general** del servidor, haga clic en **Restart** (reiniciar).

   ![Azure Database for MySQL - Información general - Botón Reiniciar](./media/howto-restart-server-portal/2-server.png)

3. Haga clic en **Sí** para confirmar el reinicio del servidor.

   ![Azure Database for MySQL - Confirmación del reinicio](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Observe que el estado del servidor cambia a "Reiniciando".

   ![Azure Database for MySQL - Estado del reinicio](./media/howto-restart-server-portal/4-restarting-status.png)

5. Confirme que el reinicio del servidor es correcto.

   ![Azure Database for MySQL - Reinicio correcto](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Creación de un servidor de Azure Database for MySQL mediante Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
