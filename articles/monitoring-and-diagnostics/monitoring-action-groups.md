---
title: Creación y administración de grupos de acciones en Azure Portal
description: Obtenga información acerca de cómo crear y administrar grupos de acciones en Azure Portal.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 3ce7c5111fa176bb7fa734f54084b9e14e7afbef
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016053"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Creación y administración de grupos de acciones en Azure Portal
## <a name="overview"></a>Información general ##
Un grupo de acciones es una colección de las preferencias de notificación que el usuario define. Las alertas de Azure Monitor y Service Health están configuradas para usar un grupo de acciones específico cuando se desencadena la alerta. Varias alertas pueden usar el mismo grupo de acciones o distintos grupos de acciones en función de los requisitos del usuario.

En este artículo se muestra cómo crear y administrar grupos de acciones en el portal de Azure.

Cada acción se compone de las siguientes propiedades:

* **Nombre**: un identificador único dentro del grupo de acciones.  
* **Tipo de acción**: acción que se va a realizar. El envío de llamadas de voz, mensajes de texto o correo electrónico o el desencadenamiento de varios tipos de acciones automatizadas son algunos ejemplos. Consulte los tipos más adelante en este artículo. 
* **Detalles**: detalles correspondientes que varían según el *tipo de acción*. 

Para más información sobre el uso de plantillas de Azure Resource Manager para configurar grupos de acciones, consulte [Plantillas de Resource Manager para grupos de acciones](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Creación de un grupo de acciones con Azure Portal ##
1. En el [portal](https://portal.azure.com), seleccione **Monitor**. La hoja **Supervisión** consolida todos los valores y datos de supervisión en una vista.

    ![Servicio "Monitor"](./media/monitoring-action-groups/home-monitor.png)
1. Seleccione **Alertas** y seleccione **Administrar los grupos de acciones**.

    ![Botón Administrar los grupos de acciones](./media/monitoring-action-groups/manage-action-groups.png)
1. Seleccione **Agregar grupo de acciones** y rellene los campos.

    ![Comando "Agregar grupo de acciones"](./media/monitoring-action-groups/add-action-group.png)
1. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

      ![Cuadro de diálogo "Agregar grupo de acciones"](./media/monitoring-action-groups/action-group-define.png)

1. El cuadro de texto **Suscripción** se rellena automáticamente con la suscripción actual. La suscripción es aquella en la que se guarda el grupo de acciones.

1. Seleccione el **Grupo de recursos** en el que se guarda el grupo de acciones.

1. Defina una lista de acciones proporcionando los siguientes valores para cada acción:

    a. **Nombre**: escriba un identificador único para esta acción.

    b. **Tipo de acción**: seleccionar correo electrónico, SMS, notificación push, voz, aplicación lógica, webhook, ITSM o runbook de Automation.

    c. **Detalles**: según el tipo de acción, proporcione un número de teléfono, una dirección de correo electrónico o un identificador URI de webhook, una aplicación de Azure, una conexión de ITSM o un runbook de Automation. Para la acción de ITSM, especifique además **Elemento de trabajo** y otros campos que requiera la herramienta ITSM.

1. Seleccione **Aceptar** para crear el grupo de acciones.

## <a name="manage-your-action-groups"></a>Administración de los grupos de acciones ##
Después de crear un grupo de acciones, está visible en la sección **Grupos de acciones** de la hoja **Supervisión**. Seleccione el grupo de acciones que desea administrar para:

* Agregar, editar o quitar acciones.
* Eliminar el grupo de acciones.

## <a name="action-specific-information"></a>Información específica de la acción
**Notificaciones push de aplicaciones de Azure**: en un grupo de acciones puede tener hasta 10 acciones de aplicación de Azure. En este momento, la acción de aplicación de Azure solo admite alertas de ServiceHealth. Los demás momentos de alerta se omitirán. Consulte el artículo acerca de la [configuración de alertas siempre que se publique una notificación de estado de un servicio](monitoring-activity-log-alerts-on-service-notifications.md).

**Correo electrónico**: se enviarán mensajes de correo electrónico desde las siguientes direcciones de correo electrónico. Asegúrese de que el filtrado de correo electrónico esté configurado correctamente.
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

En un grupo de acciones puede tener hasta 1000 acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./monitoring-alerts-rate-limiting.md).

**ITSM**: puede tener hasta 10 acciones de administración de servicios de TI en un grupo de acciones, que necesitan conexión para ITSM. Aprenda cómo crear una [conexión de ITSM](../log-analytics/log-analytics-itsmc-overview.md).

**Aplicación lógica**: en un grupo de acciones puede tener hasta 10 acciones de aplicación lógica.

**Runbook**: puede tener hasta 10 acciones de runbook en un grupo de acciones. Consulte los [límites de servicio de suscripción de Azure](../azure-subscription-service-limits.md) para más información sobre las cargas de runbook

**SMS**: puede tener hasta 10 acciones de SMS en un grupo de acciones. Consulte el artículo de [información sobre las limitaciones](./monitoring-alerts-rate-limiting.md). Consulte el artículo [Comportamiento de alertas por SMS](monitoring-sms-alert-behavior.md)

**Voz**: en un grupo de acciones puede tener hasta 10 acciones de voz.</dd>
Consulte el artículo de [información sobre las limitaciones](./monitoring-alerts-rate-limiting.md).</dd>

**Webhook**: en un grupo de acciones puede tener hasta 10 acciones de webhook. Lógica de reintento: el tiempo de espera para una respuesta es de 10 segundos. Se volverá a intentar la llamada de webhook un máximo de 2 veces cuando se devuelvan los siguientes códigos de estado HTTP: 408, 429, 503, 504 o el punto de conexión HTTP no responda. El primer reintento se produce transcurridos 10 segundos. El segundo y último reintento se produce transcurridos 100 segundos.

Rangos de direcciones IP de origen
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Para recibir actualizaciones sobre los cambios a estas direcciones IP, se recomienda configurar una [alerta de estado del servicio](./monitoring-service-notifications.md) que supervise las notificaciones informativas sobre el servicio de grupos de acciones.


## <a name="next-steps"></a>Pasos siguientes ##
* Más información sobre el [comportamiento de las alertas por SMS](monitoring-sms-alert-behavior.md).  
* [Comprender el esquema de webhook de alertas del registro de actividad](monitoring-activity-log-alerts-webhook.md).  
* Obtenga más información sobre [Conector de ITSM](../log-analytics/log-analytics-itsmc-overview.md)
* Más información sobre la [limitación de velocidad](monitoring-alerts-rate-limiting.md) en las alertas.
* Consulte la [introducción a las alertas del registro de actividad](monitoring-overview-alerts.md) y aprenda cómo puede recibir alertas.  
* Aprenda a [configurar alertas siempre que se publique una notificación de mantenimiento de un servicio](monitoring-activity-log-alerts-on-service-notifications.md).
