---
title: Alertas de registro de actividad en Azure Monitor
description: Reciba notificaciones por SMS, webhook y correo electrónico, entre otros, cuando se produzcan determinados eventos en el registro de actividad.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 5ddf510d50f38ed9aaf742bd06c330e53ffe1391
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992803"
---
# <a name="alerts-on-activity-log"></a>Alertas de registro de actividad 

## <a name="overview"></a>Información general
Las alertas del registro de actividad son alertas que se activan cuando un nuevo evento del registro de actividad cumple las condiciones especificadas en la alerta. Son recursos de Azure, por lo que pueden crearse con una plantilla de Azure Resource Manager. También se pueden crear, actualizar o eliminar en Azure Portal. Este artículo presenta los conceptos relativos a las alertas del registro de actividad. Seguidamente, se explica cómo usar Azure Portal para configurar alertas en eventos del registro de actividad. Para obtener más información sobre el uso, consulte la sección [Creación y administración de alertas del registro de actividades](alert-activity-log.md).

> [!NOTE]
> **No se pueden** crear alertas para eventos en la categoría Alerta del registro de actividad.

Por lo general, se crean alertas del registro de actividad para recibir notificaciones cuando:

* Se produzcan operaciones específicas en los recursos de la suscripción de Azure, que abarcan normalmente grupos de recursos o recursos en particular. Por ejemplo, si quiere que le notifiquen cuando se elimine alguna máquina virtual de myProductionResourceGroup. O, podría querer recibir una notificación si se asigna algún rol nuevo a un usuario de la suscripción.
* Se produce un evento de mantenimiento del servicio. Los eventos de mantenimiento del servicio incluyen la notificación de incidentes y eventos de mantenimiento que se aplican a recursos de la suscripción.

Una simple analogía para comprender las condiciones en las que se pueden crear reglas de alertas en el registro de actividad, es explorar o filtrar eventos a través del [Registro de actividad en Azure Portal ](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). En "Azure Monitor: registro de actividad", se puede filtrar o buscar un evento necesario y crear una alerta mediante el botón **Agregar alerta de registro de actividad**.

En cualquier caso, una alerta del registro de actividad solo supervisa eventos de la suscripción en la que se ha creado la alerta.

Puede configurar una alerta del registro de actividad según las propiedades de nivel superior del objeto JSON de un evento del registro de actividad. Para más información, consulte [Información general sobre el registro de actividad de Azure](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Para más información acerca de los eventos de mantenimiento del servicio, consulte [Recibir alertas del registro de actividad con las notificaciones del servicio](./monitoring-activity-log-alerts-on-service-notifications.md). 

Las alertas del registro de actividad tienen algunas opciones en común:

- **Categoría**: Administración, Mantenimiento del servicio, Escalado automático, Seguridad, Directiva y Recomendación. 
- **Ámbito**: el recurso individual o conjunto de recursos para el que se define la alerta de registro de actividad. El ámbito de una alerta de registro de actividad se puede definir en distintos niveles:
    - Nivel de recursos: por ejemplo, para una máquina virtual específica
    - Nivel de grupo de recursos: por ejemplo, todas las máquinas virtuales en un grupo de recursos específico
    - Nivel de suscripción: por ejemplo, todas las máquinas virtuales de una suscripción (o) todos los recursos de una suscripción
- **Grupo de recursos**: de forma predeterminada, la regla de alertas se guarda en el mismo grupo de recursos que tiene el destino definido en el ámbito. El usuario también puede definir el grupo de recursos donde se debe almacenar la regla de alertas.
- **Tipo de recurso**: Resource Manager define el espacio de nombres para el destino de la alerta.

- **Nombre de la operación**: el nombre de la operación de control de acceso basado en rol de Resource Manager.
- **Nivel**: el nivel de gravedad del evento (detallado, informativo, advertencia, error o crítico).
- **Estado**: el estado del evento, normalmente Iniciado, Error o Correcto.
- **Evento iniciado por**: también se conoce como el "llamador." La dirección de correo electrónico o el identificador de Azure Active Directory del usuario que realizó la operación.

> [!NOTE]
> En una suscripción, hasta 100 reglas de alertas para la actividad del ámbito en el nivel: de un único recurso, de todos los recursos de un grupo de recursos (o) de suscripción completa.

Cuando se activa una alerta del registro de actividad, usa un grupo de acciones para generar acciones o notificaciones. Un grupo de acciones es un conjunto reutilizable de destinatarios de notificación, como direcciones de correo electrónico, direcciones URL del webhook o números de teléfono para SMS. Se puede hacer referencia a los receptores desde varias alertas para centralizar y agrupar los canales de notificación. Al definir la alerta del registro de actividad, tiene dos opciones. Puede:

* Usar un grupo de acciones existente en la alerta del registro de actividad.
* Crear un nuevo grupo de acciones.

Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones en Azure Portal](monitoring-action-groups.md).


## <a name="next-steps"></a>Pasos siguientes
- Obtener una [Introducción a las alertas](monitoring-overview-alerts.md).
- Obtenga información sobre la [creación y modificación de las alertas del registro de actividad](alert-activity-log.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](monitoring-activity-log-alerts-webhook.md).
- Más información acerca de las [Notificaciones del estado del servicio](monitoring-service-notifications.md).

