---
title: Información general sobre las alertas clásicas en Microsoft Azure y Azure Monitor
description: Las alertas permiten supervisar los registros, los eventos o las métricas de recursos de Azure y recibir una notificación cuando se cumple una condición especificada.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: a0abcbdaa7e998413efb717be6e0addc5607ec5c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114018"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>¿Qué son las alertas clásicas en Microsoft Azure?

> [!NOTE]
> En este artículo se describe cómo crear alertas de métrica clásicas más antiguas. Azure Monitor ahora es compatible con [una nueva experiencia de alertas y las alertas de métrica más recientes que casi se ejecutan en tiempo real](monitoring-overview-unified-alerts.md). 
>

Estas alertas le permiten configurar las condiciones en los datos y recibir notificaciones cuando las condiciones coinciden con los datos de supervisión más actualizados.


## <a name="alerts-on-azure-monitor-data"></a>Alertas sobre datos de Azure Monitor
Hay dos tipos de alertas clásicas disponibles: alertas de métricas y alertas del registro de actividad.

* **Alertas de métricas clásicas**: estas alertas se desencadenan cuando el valor de una métrica específica cruza un umbral que se le haya asignado. La alerta en cuestión genera una notificación cuando está "activada" (si se sobrepasa el umbral y se cumple la condición de alerta). También se genera una alerta cuando se haya "resuelto" (esto es, cuando se sobrepasa el umbral de nuevo y ya no se cumple la condición). 

* **Alertas clásicas del registro de actividad**: estas alertas del registro de streaming se desencadenan cuando se genera un evento del registro de actividad que coincida con los criterios de filtro que se hayan asignado. Estas alertas tienen solo un estado "Activado", puesto que el motor de alertas simplemente aplica los criterios del filtro a cualquier evento nuevo. Se pueden utilizar estas alertas para recibir notificaciones cuando se produzca un incidente de Service Health o cuando un usuario o una aplicación realice una operación en su suscripción como, por ejemplo, "Eliminar máquina virtual".

Para recibir los datos del registro de diagnóstico disponibles a través de Azure Monitor, enrute los datos en Log Analytics (anteriormente denominado Operations Management Suite) y utilice una alerta de consulta de Log Analytics. Log Analytics utiliza ahora el [nuevo método de alertas](monitoring-overview-unified-alerts.md). 

En el diagrama siguiente se resumen los orígenes de datos en Azure Monitor, y la manera en la que se puede alertar sobre esos datos.

![Alertas explicadas](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomía de las alertas (clásicas) de Azure Monitor
Azure utiliza los siguientes términos para describir las alertas clásicas y sus funciones:
* **Alerta**: es una definición de criterios (una o más reglas o condiciones) que se activa cuando se cumplen dichos criterios.
* **Activo**: es el estado que se establece cuando se cumplen los criterios que haya definido una alerta clásica.
* **Resuelto**: es el estado que se establece cuando ya no se cumplen los criterios que haya definido una alerta clásica, después de que se cumplieran con anterioridad.
* **Notificación**: es la acción que se realiza a partir de la activación de una alerta clásica.
* **Acción**: es la llamada específica que se envía al receptor de una notificación (por ejemplo, un correo electrónico o un mensaje en una dirección URL de webhook). Las notificaciones normalmente pueden desencadenar varias acciones.

## <a name="how-do-i-receive-notifications-from-an-azure-monitor-classic-alert"></a>¿Cómo puedo recibir notificaciones de una alerta clásica de Azure Monitor?
Históricamente, las alertas de Azure de distintos servicios usan sus propios métodos de notificación integrados. 

Azure Monitor ofrece ahora un grupo de notificaciones reutilizable denominado *Grupos de acciones*. Los grupos de acciones especifican un conjunto de receptores para una notificación. Cuando se activa una alerta que hace referencia al grupo de acción, todos los receptores reciben esa notificación. Esta funcionalidad le permite reutilizar una agrupación de receptores (por ejemplo, la lista de ingenieros de guardia) a través de muchos objetos de alerta. Asimismo, los grupos de acciones admiten el envío de notificaciones a través de diversos métodos. Estos métodos pueden incluir agregar mensajes en una dirección URL de webhook, enviar correos electrónicos, mensajes de SMS y otras acciones similares. Para obtener más información, consulte [Crear y administrar grupos de acciones en Azure Portal](monitoring-action-groups.md). 

Las anteriores alertas de registro de actividad clásicas usan grupos de acciones.

Sin embargo, las anteriores alertas de métricas aún no los utilizan. En cambio, se pueden configurar las siguientes acciones: 
* Envíe notificaciones de correo electrónico al administrador de servicios o a los coadministradores, o a las direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook, que permite iniciar acciones de automatización adicionales.

Los webhooks le permiten realizar acciones de automatización y corrección mediante los siguientes servicios:
- Runbook de Azure Automation
- Azure Functions
- Aplicación lógica de Azure
- Un servicio de terceros

## <a name="next-steps"></a>Pasos siguientes
Obtenga información acerca de las reglas de alerta y cómo configurarlas mediante la siguiente documentación:

* Obtenga más información acerca de las [métricas](monitoring-overview-metrics.md)
* Configure [alertas de métricas clásicas mediante Azure Portal](insights-alerts-portal.md)
* Configure [alertas de métricas clásicas mediante PowerShell](insights-alerts-powershell.md)
* Configure [alertas de métricas clásicas mediante la CLI de Azure](insights-alerts-command-line-interface.md)
* Configure [alertas de métricas clásicas mediante la API REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Obtenga más información sobre los [registros de actividades](monitoring-overview-activity-logs.md).
* Configure [alertas del registro de actividades mediante Azure Portal](monitoring-activity-log-alerts.md)
* Configure [alertas del registro de actividades mediante el Administrador de recursos](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Revisión del [esquema de webhook de alertas del registro de actividad](monitoring-activity-log-alerts-webhook.md)
* Más información sobre los [grupos de acciones](monitoring-action-groups.md)
* Configure las [alertas más recientes](monitor-alerts-unified-usage.md)
