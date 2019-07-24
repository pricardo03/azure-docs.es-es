---
title: Visualización de las notificaciones de mantenimiento del servicio mediante Azure Portal
description: Las notificaciones de mantenimiento del servicio permiten ver los mensajes de mantenimiento del servicio que publica Microsoft Azure.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: 8cf8c3eb86f55b5595ef9a1af83ea50580bf638b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069351"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Visualización de las notificaciones de mantenimiento del servicio mediante Azure Portal

Las notificaciones de mantenimiento del servicio se publican mediante la infraestructura de Azure. Estas contienen información acerca de los recursos en la suscripción. Estas notificaciones son una subclase de los eventos del registro de actividad y también se pueden encontrar en el registro de actividad. En función de la clase, las notificaciones de mantenimiento del servicio pueden ser meramente informativas o pueden requerir acciones.

Para obtener más información sobre las distintas clases de notificaciones de mantenimiento del servicio, vea [Propiedades de las notificaciones de Service Health](../../service-health/service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Visualización de las notificaciones de mantenimiento del servicio en Azure Portal

1. En [Azure Portal](https://portal.azure.com), seleccione **Supervisión**.

    ![Captura de pantalla del menú de Azure Portal, con el Monitor seleccionado](./media/service-notifications/home-monitor.png)

    Azure Monitor se abre junto con toda la configuración de supervisión y los datos en una sola vista consolidada. Primero se abre la sección **Registro de actividades** .

1. Seleccione **Alertas**.

    ![Captura de pantalla del registro de actividad del Monitor, con Alertas seleccionado](./media/service-notifications/service-health-summary.png)

1. Seleccione **+Agregar alerta del registro de actividad** y configure una alerta para asegurarse de que recibe las futuras notificaciones del servicio. Para más información, consulte [Creación de alertas del registro de actividad en notificaciones del servicio](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Pasos siguientes

* Reciba [notificaciones de alertas con cada notificación de mantenimiento del servicio](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).  
* Más información sobre las [alertas del registro de actividad](../../azure-monitor/platform/activity-log-alerts.md).
