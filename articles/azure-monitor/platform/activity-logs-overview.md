---
title: Información general sobre el registro de actividad de Azure
description: Aprenda qué es el registro de actividad de Azure y cómo puede usarlo para comprender los eventos que se producen en su suscripción de Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: fa1737a8627fe9561a2a84e7f0ef69aefb6deb14
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70170617"
---
# <a name="overview-of-azure-activity-log"></a>Introducción al registro de actividad de Azure

El **registro de actividad de Azure** es un registro que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. El registro de actividad se conocía anteriormente como _registros de auditoría_ o _registros operativos_, ya que la categoría Administrativo notifica eventos del plano de control de las suscripciones. 

Use el registro de actividad para dar respuesta a los interrogantes _qué_, _quién_ y _cuándo_ de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes. 

El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo Clásico/RDFE.

## <a name="comparison-to-diagnostic-logs"></a>Comparación con los registros de diagnóstico
Hay un único registro de actividad para cada suscripción de Azure. Este proporciona datos sobre las operaciones en un recurso desde el exterior (el "plano de control"). Los [registros de diagnóstico](diagnostic-logs-overview.md) son emitidos por un recurso y proporcionan información sobre el funcionamiento de dicho recurso (el "plano de datos"). Debe habilitar la configuración de diagnóstico de cada recurso.

![Comparación entre registro de actividad y registro de diagnóstico](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)


> [!NOTE]
> El registro de actividad de Azure sirve principalmente para las actividades que se producen en Azure Resource Manager. No hace seguimiento de los recursos que usan el modelo Clásico/RDFE. Algunos tipos de recursos clásicos tienen un proveedor de recursos de servidor proxy en Azure Resource Manager (por ejemplo, Microsoft.ClassicCompute). Si interactúa con un tipo de recurso clásico a través de Azure Resource Manager con estos proveedores de recursos de servidor proxy, las operaciones aparecen en el registro de actividad. Si interactúa con un tipo de recurso clásico fuera de los servidores proxy de Azure Resource Manager, sus acciones solo se registran en el registro de operaciones. Se puede examinar el registro de operaciones en una sección independiente del portal.

## <a name="activity-log-retention"></a>Retención del registro de actividad
Una vez creado, el sistema no modifica o elimina las entradas del registro de actividad. Además, no se pueden cambiar en la interfaz o mediante programación. Los eventos del registro de actividad se almacenan durante 90 días. Para almacenar los datos durante períodos más largos, [recopílelos en Azure Monitor](activity-log-collect.md) o [expórtelos a Storage o Event Hubs](activity-log-export.md).

## <a name="view-the-activity-log"></a>Ver el registro de actividad
Vea el registro de actividad de todos los recursos del menú **Supervisión** de Azure Portal. Vea el registro de actividad de un recurso determinado en la opción **Registro de actividad** del menú de ese recurso. También puede recuperar las entradas del registro de actividad con PowerShell, la CLI o API REST.  Consulte [View and retrieve Azure Activity log events](activity-log-view.md) (Visualización y recuperación de eventos del registro de actividad de Azure).

![Ver el registro de actividad](./media/activity-logs-overview/view-activity-log.png)

## <a name="collect-activity-log-in-azure-monitor"></a>Recopilación del registro de actividad en Azure Monitor
Recopile el registro de actividad en un área de trabajo de Log Analytics en Azure Monitor para analizarlo junto con otros datos de supervisión y conservar los datos durante más de 90 días. Consulte [Collect and analyze Azure activity logs in Log Analytics workspace in Azure Monitor](activity-log-collect.md) (Recopilación y análisis de registros de actividad en un área de trabajo de Log Analytics en Azure Monitor).

![Consulta del registro de actividad](./media/activity-logs-overview/query-activity-log.png)

## <a name="export-activity-log"></a>Exportación del registro de actividad
Exporte el registro de actividad a Azure Storage para su almacenamiento o transmítalo a un centro de eventos para su ingesta por parte de un servicio de terceros o una solución de análisis personalizados. Consulte [Export the Azure Activity Log](activity-log-export.md) (Exportación del registro de actividad de Azure). También puede analizar los eventos del registro de actividad en Power BI mediante el [**paquete de contenido de Power BI**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

## <a name="alert-on-activity-log"></a>Alertas del registro de actividad
Para determinados eventos del registro de actividad puede crear una [alerta](activity-log-alerts.md). También puede crear una alerta mediante una [consulta de registro](alerts-log-query.md) si el registro de actividad está conectado a un área de trabajo de Log Analytics, pero las alertas de consulta de registro tienen un costo. Las alertas de registro de actividad son gratuitas.

## <a name="categories-in-the-activity-log"></a>Categorías del Registro de actividad
Cada evento del registro de actividad tiene una categoría determinada que se describe en la tabla siguiente. Para obtener todos los detalles sobre los esquemas de estas categorías, consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md). 

| Categoría | DESCRIPCIÓN |
|:---|:---|
| Administrativo | Contiene el registro de todas las operaciones de creación, actualización, eliminación y acción realizadas mediante Resource Manager. Algunos ejemplos de eventos administrativos incluyen la _creación de una máquina virtual_ y la _eliminación de un grupo de seguridad de red_.<br><br>Cada acción realizada por un usuario o aplicación mediante Resource Manager se modela como una operación en un tipo de recurso determinado. Si el tipo de operación es _Write_, _Delete_ o _Action_, los registros de inicio y corrección o error de esa operación se registran en la categoría Administrativo. Los eventos de la categoría Administrativo también incluyen los cambios realizados en el control de acceso basado en rol de una suscripción. |
| Service Health | Contiene el registro de los incidentes de estado del servicio que se han producido en Azure. Ejemplo de un evento de Service Health: _SQL Azure está experimentando un tiempo de inactividad en la región Este de EE. UU._ . <br><br>Los eventos de Service Health pueden encuadrarse dentro de seis variedades: _Acción requerida_, _Recuperación asistida_, _Incidente_, _Mantenimiento_, _Información_ o _Seguridad_. Estos eventos solo se crean si tiene un recurso en la suscripción que se puede ver afectado por el evento.
| Estado de los recursos | Contiene el registro de los eventos de estado de los recursos que se han producido en los recursos de Azure. Ejemplo de un evento de Resource Health: _Cambio del estado de mantenimiento de una máquina virtual a No disponible_.<br><br>Los eventos de Resource Health pueden representar uno de los cuatro estados de mantenimiento siguientes: _Disponible_, _No disponible_, _Degradado_ y _Desconocido_. Además, los eventos de Resource Health se pueden clasificar como _iniciados por la plataforma_ o _por el usuario_. |
| Alerta | Contiene el registro de activaciones de alertas de Azure. Ejemplo de un evento de alerta: _% de CPU en myVM ha estado por encima de 80 durante los últimos 5 minutos_.|
| Escalado automático | Contiene el registro de los eventos relacionados con el funcionamiento del motor de escalado automático en función de cualquier configuración de escalado automático que haya definido en la suscripción. Ejemplo de un evento de escalado automático: _Error durante la acción de escalado vertical_. |
| Recomendación | Contiene eventos de recomendación de Azure Advisor. |
| Seguridad | Contiene el registro de todas las alertas generadas por Azure Security Center. Ejemplo de un evento de seguridad: _Se ha ejecutado un archivo de extensión doble_. |
| Directiva | Contiene registros de todas las operaciones de acción de efecto realizadas por Azure Policy. Ejemplos de eventos de directiva: _Auditar_ y _Denegar_. Cada acción llevada a cabo por Azure Policy se modela como una operación en un recurso. |


## <a name="next-steps"></a>Pasos siguientes

* [Creación de un perfil de registro para exportar el registro de actividad de Azure](activity-log-export.md)
* [Transmisión del registro de actividad de Azure a Event Hubs](activity-logs-stream-event-hubs.md)
* [Archivo del registro de actividad de Azure en Storage](archive-activity-log.md)

