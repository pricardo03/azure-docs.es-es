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
ms.openlocfilehash: 34857108cf7f0580c380ffbd4bbcedb5cd5a807a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245022"
---
# <a name="overview-of-azure-activity-log"></a>Información general del registro de actividad de Azure

El **Azure Activity Log** proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. El registro de actividad se conocía anteriormente como _los registros de auditoría_ o _registros operativos_, ya que la categoría administrativo notifica eventos del plano de control para las suscripciones. 

Utilice el registro de actividad, para determinar el _lo_, _quién_, y _cuando_ para las operaciones (PUT, POST, DELETE) realizadas en los recursos en su suscripción de escritura. También puede conocer el estado de la operación y otras propiedades relevantes. 

El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo clásico/RDFE.

## <a name="comparison-to-diagnostic-logs"></a>Comparación con los registros de diagnóstico
Hay un único registro de actividad para cada suscripción de Azure. Proporciona datos sobre las operaciones en un recurso desde el exterior (el "plano de control"). [Los registros de diagnóstico](diagnostic-logs-overview.md) son emitidos por un recurso y proporcionan información acerca del funcionamiento de ese recurso (el "plano de datos"). Debe habilitar la configuración de diagnóstico para cada recurso.

![Registros de actividad en comparación con los registros de diagnóstico](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)


> [!NOTE]
> El registro de actividad de Azure sirve principalmente para las actividades que se producen en Azure Resource Manager. No realiza un seguimiento ugresources con el modelo clásico/RDFE. Algunos tipos de recursos clásicos tienen un proveedor de recursos de servidor proxy en Azure Resource Manager (por ejemplo, Microsoft.ClassicCompute). Si interactúa con un tipo de recurso clásico a través de Azure Resource Manager con estos proveedores de recursos de servidor proxy, las operaciones aparecen en el registro de actividad. Si interactúa con un tipo de recurso clásico fuera de los servidores proxy de Azure Resource Manager, sus acciones solo se registran en el registro de operaciones. Se puede examinar el registro de operaciones en una sección independiente del portal.

## <a name="activity-log-retention"></a>Retención de registro de actividad
Eventos de registro de actividad se almacenan durante 90 días. Para almacenar los datos durante períodos más largos, [recopilarlos en Azure Monitor](activity-log-collect.md) o [exportarlo a storage o Event Hubs](activity-log-export.md).

## <a name="view-the-activity-log"></a>Ver el registro de actividad
Ver el registro de actividad para todos los recursos de la **Monitor** menú en el portal de Azure. Ver el registro de actividad para un recurso determinado desde el **registro de actividad** opción en el menú de ese recurso. También puede recuperar las entradas del registro de actividad con PowerShell, CLI o API de REST.  Consulte [vista y actividad de Azure de recuperación registrar eventos](activity-log-view.md).

![Ver registro de actividad](./media/activity-logs-overview/view-activity-log.png)

## <a name="collect-activity-log-in-azure-monitor"></a>Recopilar el registro de actividad en Azure Monitor
Recopilar el registro de actividad en un área de trabajo de Log Analytics en Azure Monitor puede analizarlo con otros datos de supervisión y conservar los datos durante más de 90 días. Consulte [recopilar y analizar los registros de actividad de Azure en el área de trabajo de Log Analytics en Azure Monitor](activity-log-collect.md).

![Registro de actividad de consulta](./media/activity-logs-overview/query-activity-log.png)

## <a name="export-activity-log"></a>Exportar registro de actividad
Exportar el registro de actividad en Azure Storage para archivar o transmitirlo a un centro de eventos para la ingesta en un servicio de terceros o una solución de análisis personalizado. Consulte [exportar el registro de actividad de Azure](activity-log-export.md). También puede analizar los eventos de registro de actividad en Power BI mediante la [ **paquete de contenido de Power BI**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

## <a name="alert-on-activity-log"></a>Alerta de registro de actividad
Puede crear una alerta cuando se crean eventos concretos en el registro de actividad con un [alerta de registro de actividad](activity-log-alerts.md). También puede crear una alerta mediante un [consulta de registro](alerts-log-query.md) cuando el registro de actividades está conectado a un área de trabajo de Log Analytics, pero hay un costo para registrar las alertas de consulta. No hay ningún costo para las alertas de registro de actividad.

## <a name="categories-in-the-activity-log"></a>Categorías del Registro de actividad
Cada evento del registro de actividad tiene una categoría determinada que se describen en la tabla siguiente. Para obtener todos los detalles sobre los esquemas de estas categorías, consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md). 

| Categoría | DESCRIPCIÓN |
|:---|:---|
| Administrativo | Contiene el registro de todos los de creación, las operaciones de actualización, eliminación y acción realizan a través de Resource Manager. Ejemplos de eventos administrativos _crear máquina virtual_ y _eliminar el grupo de seguridad de red_.<br><br>Cada acción realizada por un usuario o una aplicación mediante Resource Manager se modela como una operación en un tipo de recurso determinado. Si el tipo de operación es _escribir_, _eliminar_, o _acción_, los registros de inicio y corrección o error de esa operación se registra en la categoría administrativa. Eventos administrativos también incluyen los cambios realizados en el control de acceso basado en roles en una suscripción. |
| Service Health | Contiene el registro de los incidentes de estado de servicio que se han producido en Azure. Un ejemplo de un evento de estado del servicio _SQL Azure este de Estados Unidos está experimentando tiempos de inactividad_. <br><br>Eventos de estado del servicio son de cinco variedades: _Acción requerida_, _recuperación asistida_, _incidente_, _mantenimiento_, _información_, o  _Seguridad_. Estos eventos se crean solo si tiene un recurso en la suscripción que se vean afectada por el evento.
| Estado de los recursos | Contiene el registro de los eventos de mantenimiento de recursos que se han producido en los recursos de Azure. Un ejemplo de un evento de estado de los recursos es _cambia de estado de mantenimiento de la máquina Virtual a disponible_.<br><br>Eventos de mantenimiento de recursos pueden representar uno de cuatro estados de mantenimiento: _Disponible_, _disponible_, _degradado_, y _desconocido_. Además, los eventos de estado de los recursos se pueden clasificar como _iniciado por la plataforma_ o _iniciada por el usuario_. |
| Alerta | Contiene el registro de activaciones de alertas de Azure. Un ejemplo de un evento de alerta es _% de CPU en myVM ha estado por encima del 80 durante los últimos cinco minutos_.|
| Escalado automático | Contiene el registro de los eventos relacionados con el funcionamiento del motor de escalado automático en función de cualquier configuración de escalado automático que haya definido en su suscripción. Un ejemplo de un evento de escalado automático es _acción de escalado automático no se pudo_. |
| Recomendación | Contiene los eventos de recomendación de Azure Advisor. |
| Seguridad | Contiene el registro de las alertas generadas por Azure Security Center. Un ejemplo de un evento de seguridad es _ejecutarlo el archivo de extensión doble sospechoso_. |
| Directiva | Contiene registros de todas las operaciones de acción de efecto realizadas por la directiva de Azure. Ejemplos de eventos de directiva _auditoría_ y _Deny_. Cada acción llevada a cabo por Azure Policy se modela como una operación en un recurso. |


## <a name="next-steps"></a>Pasos siguientes

* [Crear un perfil de registro para exportar el registro de actividad de Azure](activity-log-export.md)
* [Transmisión del registro de actividad de Azure a Event Hubs](activity-logs-stream-event-hubs.md)
* [Archivar el registro de actividad de Azure para almacenamiento](archive-activity-log.md)

