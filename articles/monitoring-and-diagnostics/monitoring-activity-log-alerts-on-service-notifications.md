---
title: Recepción de alertas de registro de actividad en las notificaciones del servicio de Azure
description: Reciba notificaciones por SMS, correo electrónico o webhook cuando se produzcan eventos en el servicio de Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/09/2018
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 01dc3a3c6489b694af26c78ae3b4756f3e8f00b7
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263123"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Creación de alertas del registro de actividad en notificaciones del servicio
## <a name="overview"></a>Información general
En este artículo se explica cómo configurar las alertas del registro de actividad para las notificaciones de mantenimiento de un servicio mediante Azure Portal.  

Puede recibir una alerta cuando Azure envía notificaciones de estado del servicio a la suscripción de Azure. Puede configurar la alerta en función de:

- La clase de notificación de estado del servicio (problemas de servicio, mantenimiento planificado y avisos de estado).
- La suscripción afectada.
- Los servicios afectados.
- Las regiones afectadas.

También puede configurar a quién se debe enviar la alerta:

- Seleccione un grupo de acciones existente.
- Cree un nuevo grupo de acciones (que puede usarse para futuras alertas).

Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones](monitoring-action-groups.md).

Para obtener información sobre cómo configurar las alertas de notificación de mantenimiento del servicio mediante plantillas de Azure Resource Manager, consulte [Plantillas de Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Creación de una alerta basada en una notificación de mantenimiento del servicio para un nuevo grupo de acciones con Azure Portal
1. En el [portal](https://portal.azure.com), seleccione **Estado del servicio**.

    ![El servicio "Estado del servicio"](./media/monitoring-activity-log-alerts-on-service-notifications/home-servicehealth.png)

2. En la sección **Alertas**, seleccione **Alertas de estado**.

    ![La pestaña "Alertas de estado"](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades-sh.png)

3. Seleccione **Crear alerta de estado de servicio** y rellene los campos.

    ![El comando "Crear alerta de estado de servicio"](./media/monitoring-activity-log-alerts-on-service-notifications/service-health-alert.png)

4. Seleccione la **suscripción**, los **servicios** y las **regiones** para los que desea recibir alertas.

    ![Cuadro de diálogo "Agregar alerta de registro de actividad"](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-new-ux.png)

> [!NOTE]
> Esta suscripción se usa para guardar la alerta del registro de actividad. El recurso de la alerta se implementa en esta suscripción y supervisa los eventos en el registro de actividad para dicho recurso.

5. Elija los **Tipos de evento** para los que quiere recibir alertas: *Problema de servicio*, *Mantenimiento planeado* y *Avisos de estado* 

6. Para definir los detalles de la alerta, escriba un **nombre de regla de alertas** y una **descripción**.

7. Seleccione el **grupo de recursos** donde quiere guardar la alerta.

8. Para crear un nuevo grupo de acciones, seleccione **Nuevo grupo de acciones**. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. Se hace referencia al nombre corto en las notificaciones que se envían cuando se desencadena esta alerta.

    ![Creación de un nuevo grupo de acciones](./media/monitoring-activity-log-alerts-on-service-notifications/action-group-creation.png)

9. A continuación, defina una lista de destinatarios proporcionando:

    a. **Nombre**: el nombre, alias o identificador del destinatario.

    b. **Tipo de acción**: seleccione SMS, correo electrónico, webhook, aplicación de Azure, etc.

    c. **Detalles**: según el tipo de acción elegido, proporcione un número de teléfono, una dirección de correo electrónico, un identificador URI de webhook, etc.

10. Seleccione **Aceptar** para crear el grupo de acciones y luego en **Crear regla de alertas** para completar la alerta.

En unos minutos, la alerta está activa y comienza a desencadenarse en función de las condiciones especificadas durante la creación.

Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](../service-health/service-health-alert-webhook-guide.md). Para obtener información sobre el esquema de webhook para las alertas del registro de actividad, consulte [Webhooks para alertas del registro de actividad de Azure](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>El grupo de acciones definido en estos pasos es reutilizable, como grupo de acciones existente, en todas las futuras definiciones de alertas.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Creación de una alerta basada en una notificación de mantenimiento del servicio para un grupo de acciones existente con Azure Portal

1. Siga los pasos del 1 al 7 de la sección anterior para crear la notificación de mantenimiento del servicio. 

2. En **Definir grupo de acciones**, haga clic en el botón **Seleccionar grupo de acciones**. Seleccione el grupo adecuado.

3. Seleccione **Agregar** para agregar el grupo de acciones y luego **Crear regla de alertas** para completar la alerta.

En unos minutos, la alerta está activa y comienza a desencadenarse en función de las condiciones especificadas durante la creación.

## <a name="manage-your-alerts"></a>Administración de alertas

Después de crear una alerta, es visible en la sección **Alertas** de **Supervisión**. Seleccione la alerta que desea administrar para:

* Editarla.
* Eliminarla.
* Deshabilitarla o habilitarla, si desea detener temporalmente o reanudar la recepción de notificaciones de la alerta.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información acerca de cómo [configurar notificaciones de webhook para los sistemas de administración de problemas existentes](../service-health/service-health-alert-webhook-guide.md).
- Más información acerca de las [Notificaciones del estado del servicio](monitoring-service-notifications.md).
- Más información sobre la [Limitación del número de notificaciones](monitoring-alerts-rate-limiting.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](monitoring-activity-log-alerts-webhook.md).
- Consulte la [introducción a las alertas del registro de actividad](monitoring-overview-alerts.md) y aprenda cómo puede recibir alertas. 
- Más información sobre los [grupos de acciones](monitoring-action-groups.md).
