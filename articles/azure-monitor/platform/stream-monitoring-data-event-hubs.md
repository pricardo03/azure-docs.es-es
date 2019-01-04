---
title: Flujo de datos de supervisión de Azure a Event Hubs
description: Aprenda sobre el flujo de todos los datos de supervisión de Azure a un centro de eventos para que una herramienta de un asociado de Administración de eventos e información de seguridad o de análisis puedan disponer de ellos.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: a39d497c90f49f8699b9d27be175e501973804c5
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811518"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa

Azure Monitor proporciona una sola canalización para acceder a todos los datos de supervisión del entorno de Azure, lo que permite configurar fácilmente una herramienta de asociado de Administración de eventos e información de seguridad y de supervisión que consuman los datos. Este artículo le guía a través de la configuración de distintos niveles de datos en el entorno de Azure para enviarlos a un único espacio de nombres de Event Hubs o centro de eventos, donde una herramienta externa los pueda recopilar.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

## <a name="what-data-can-i-send-into-an-event-hub"></a>¿Qué datos puedo enviar a un centro de eventos?

En el entorno de Azure hay varios "niveles" de datos de supervisión, cuyo método de acceso varía ligeramente. Normalmente, estos niveles se pueden describir como:

- **Datos de supervisión de aplicaciones:** datos sobre el rendimiento y la funcionalidad del código que ha escrito y que se ejecuta en Azure. El seguimiento del rendimiento, los registros de aplicaciones y la telemetría de usuario son ejemplos de datos de supervisión de aplicaciones. Normalmente se recopilan datos de supervisión de aplicaciones de una de las maneras siguientes:
  - Mediante la instrumentación del código con un SDK como el [SDK de Application Insights](../../application-insights/app-insights-overview.md).
  - Mediante la ejecución de un agente de supervisión que escucha los registros de aplicaciones nuevos en la máquina donde se ejecuta la aplicación, como el [agente de Azure Diagnostics para Windows](./../../azure-monitor/platform/diagnostics-extension-overview.md) o el [agente de Azure Diagnostics para Linux](../../virtual-machines/extensions/diagnostics-linux.md).
- **Datos de supervisión del sistema operativo invitado**: datos sobre el sistema operativo en el que se ejecuta la aplicación. Ejemplos de datos de supervisión de sistema operativo invitado serían los syslog de Linux o los eventos de sistema de Windows. Para recopilar este tipo de datos, debe instalar un agente como el [agente de Azure Diagnostics para Windows](./../../azure-monitor/platform/diagnostics-extension-overview.md) o el [agente de Azure Diagnostics para Linux](../../virtual-machines/extensions/diagnostics-linux.md).
- **Supervisión de recursos con DMV**: datos sobre el funcionamiento de un recurso de Azure. Para algunos tipos de recursos de Azure, como las máquinas virtuales, hay un sistema operativo invitado y aplicaciones que supervisan el interior de ese servicio de Azure. Para otros recursos de Azure, como los grupos de seguridad de red, los datos de supervisión de recursos son el nivel más alto de datos disponible (porque no hay ningún sistema operativo invitado ni aplicación que se ejecute en esos recursos). Estos datos se pueden recopilar con la [configuración de diagnóstico de recursos](./../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
- **Datos de supervisión de la suscripción de Azure:** datos sobre el funcionamiento y la administración de una suscripción de Azure, así como sobre el estado y el funcionamiento del propio Azure. El [registro de actividad](./../../azure-monitor/platform/activity-logs-overview.md) contiene la mayoría de los datos de supervisión de suscripciones, como los incidentes de estado del servicio y las auditorías de Azure Resource Manager. Puede recopilar estos datos mediante un perfil de registro.
- **Datos de supervisión de inquilino de Azure:** datos sobre el funcionamiento de los servicios de Azure en el nivel del inquilino, como Azure Active Directory. Las auditorías y los inicios de sesión de Azure Active Directory son ejemplos de datos de supervisión de inquilino. Estos datos se pueden recopilar con la configuración de diagnóstico de inquilino.

Pueden enviarse datos desde cualquier nivel a un centro de eventos, donde pueden extraerse en una herramienta asociada. En las secciones siguientes se describe cómo configurar los datos de cada nivel para el flujo de datos a un centro de eventos. En los pasos se presupone que ya tiene recursos en ese nivel que desea supervisar.

## <a name="set-up-an-event-hubs-namespace"></a>Configuración de un espacio de nombres de Event Hubs

Antes de empezar, debe [crear un espacio de nombres de Event Hubs y un centro de eventos](../../event-hubs/event-hubs-create.md). Estos centro de eventos y espacio de nombres son el destino de todos los datos de supervisión. Un espacio de nombres de Event Hubs es una agrupación lógica de centros de eventos que comparten la misma directiva de acceso, al igual que una cuenta de almacenamiento tiene blobs individuales dentro de esa cuenta de almacenamiento. Tenga en cuenta algunos detalles sobre el espacio de nombres de los centros de eventos y los propios centros de eventos que crea:
* Se recomienda usar un espacio de nombres de Event Hubs estándar.
* Normalmente, solo se necesita una unidad de rendimiento. Si necesita escalar verticalmente a medida que el uso del registro aumenta, siempre puede aumentar manualmente el número de unidades de rendimiento para el espacio de nombres más adelante o habilitar la inflación automática.
* El número de unidades de rendimiento permite aumentar la escala de rendimiento para los centros de eventos. El número de particiones permite paralelizar el consumo entre muchos consumidores. Una sola partición puede hacer hasta 20 MBps o aproximadamente 20 000 mensajes por segundo. Dependiendo de la herramienta que consume los datos, puede o no puede admitir el consumo de varias particiones. Si no está seguro del número de particiones que se va a establecer, se recomienda empezar con cuatro particiones.
* Se recomienda establecer la retención de mensajes en el centro de eventos a 7 días. Si la herramienta de consumo deja de funcionar durante más de un día, esto garantiza que dicha herramienta puede continuar donde se quedó (para los eventos de hasta 7 días de antigüedad).
* Se recomienda utilizar el grupo de consumidores predeterminado para el centro de eventos. No es necesario para crear otros grupos de consumidores o usar un grupo de consumidores independientes a menos que piense disponer de dos herramientas diferentes que consuman los mismos datos del mismo centro de eventos.
* Para el registro de actividad de Azure, se elige un espacio de nombres de Event Hubs y Azure Monitor crea un centro de eventos dentro de ese espacio de nombres denominado "insights-logs-operationallogs". Para otros tipos de registro, puede elegir un centro de eventos existente (lo que permite reutilizar el mismo centro de eventos insights-logs-operationallogs) o hacer que Azure Monitor cree un centro de eventos por categoría de registro.
* Por lo general, los puertos 5671 y 5672 deben estar abiertos en la máquina que consume datos del centro de eventos.

Consulte también [Preguntas frecuentes sobre Event Hubs](../../event-hubs/event-hubs-faq.md).

## <a name="azure-tenant-monitoring-data"></a>Datos de supervisión de inquilino de Azure

Los datos de supervisión de inquilino actualmente solo están disponibles para Azure Active Directory. Puede usar los datos de [informes de Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md), que contienen el historial de la actividad de inicio de sesión y la traza de auditoría de los cambios realizados en un inquilino determinado.

### <a name="azure-active-directory-data"></a>Datos de Azure Active Directory

Para enviar datos desde el registro de Azure Active Directory en un espacio de nombres de Event Hubs, configure los valores de diagnóstico de inquilino en su inquilino de AAD. [Siga esta guía](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) para configurar los valores de diagnóstico de inquilino.

## <a name="azure-subscription-monitoring-data"></a>Datos de supervisión de la suscripción de Azure

Los datos de supervisión de la suscripción de Azure están disponibles en el [registro de actividad de Azure](./../../azure-monitor/platform/activity-logs-overview.md). Este registro contiene las operaciones de creación, actualización y eliminación de Resource Manager, los cambios de [estado del servicio Azure](../../service-health/service-health-overview.md) que puede afectar a los recursos de la suscripción, las transiciones de [estado de los recursos](../../service-health/resource-health-overview.md) y otros muchos tipos de eventos de nivel de suscripción. [En este artículo se detallan todas las categorías de eventos que aparecen en el registro de actividad de Azure](./../../azure-monitor/platform/activity-log-schema.md).

### <a name="activity-log-data"></a>Datos del registro de actividad

Para enviar datos del registro de actividad de Azure a un espacio de nombres de Event Hubs, configure un perfil de registro en la suscripción. [Siga esta guía](./activity-logs-stream-event-hubs.md) para configurar un perfil de registro en la suscripción. Haga esto con cada suscripción que desee supervisar.

> [!TIP]
> Un perfil de registro actualmente solo permite seleccionar un espacio de nombres de Event Hubs, en el que se crea un centro de eventos con el nombre "Insights-operational-logs". No, aún es posible especificar un nombre de centro de eventos propio en un perfil de registro.

## <a name="azure-resource-metrics-and-diagnostics-logs"></a>Métricas y registros de diagnóstico de los recursos de Azure

Los recursos de Azure emiten dos tipos de datos de supervisión:
1. [Registros de diagnóstico de recursos](./../../azure-monitor/platform/diagnostic-logs-overview.md)
2. [Métricas](../../azure-monitor/platform/data-collection.md)

Ambos tipos de datos se envían a un centro de eventos mediante la configuración de diagnóstico de recursos. [Siga esta guía](./../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) para definir una configuración de diagnóstico en un recurso concreto. Establezca una configuración de diagnóstico en cada recurso del que quiera recopilar registros.

> [!TIP]
> Puede usar Azure Policy para asegurarse de que todos los recursos de un ámbito determinado siempre se definen con una configuración de diagnóstico [con el efecto de DeployIfNotExists en la regla de directiva](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="guest-os-data"></a>Datos del sistema operativo invitado

Debe instalar un agente para enviar los datos de supervisión de sistema operativo invitado a un centro de eventos. Para Windows o Linux, especifique los datos que desea que se envíen al centro de eventos y el centro de eventos donde enviarlos en un archivo de configuración y pase ese archivo de configuración al agente que se ejecuta en la máquina virtual.

### <a name="linux-data"></a>Datos de Linux

Puede utilizarse el [agente de Azure Diagnostics para Linux](../../virtual-machines/extensions/diagnostics-linux.md) para enviar datos de supervisión de una máquina Linux a un centro de eventos. Esto se hace al agregar el centro de eventos como receptor en el archivo de configuración protegido JSON de LAD. [Consulte este artículo para más información sobre cómo agregar el centro de eventos receptor al agente de Azure Diagnostics para Linux](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings).

> [!NOTE]
> No se puede establecer el flujo de datos de supervisión de sistema operativo invitado a un centro de eventos en el portal. En su lugar, debe editar manualmente el archivo de configuración.

### <a name="windows-data"></a>Datos de Windows

Puede utilizarse el [agente de Azure Diagnostics para Windows](./../../azure-monitor/platform/diagnostics-extension-overview.md) para enviar datos de supervisión de una máquina Windows a un centro de eventos. Esto se hace al agregar el centro de eventos como receptor en la sección privateConfig del archivo de configuración WAD. [Consulte este artículo para más información sobre cómo agregar el centro de eventos receptor al agente de Azure Diagnostics para Windows](./../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md).

> [!NOTE]
> No se puede establecer el flujo de datos de supervisión de sistema operativo invitado a un centro de eventos en el portal. En su lugar, debe editar manualmente el archivo de configuración.

## <a name="application-monitoring-data"></a>Datos de supervisión de aplicaciones

Los datos de supervisión de aplicaciones requieren la instrumentación del código mediante SDK, por lo que no hay una solución general al enrutamiento de datos de supervisión de aplicaciones a un centro de eventos en Azure. Sin embargo, [Azure Application Insights](../../application-insights/app-insights-overview.md) es un servicio que puede usarse para recopilar datos de nivel de aplicación de Azure. Si usa Application Insights, puede haber un flujo de datos de supervisión a un centro de eventos al hacer lo siguiente:

1. [Configure la exportación continua](../../azure-monitor/app/export-telemetry.md) de los datos de Application Insights a una cuenta de almacenamiento.

2. Configure una aplicación lógica desencadenada por temporizador que [extraiga los datos de Blob Storage](../../connectors/connectors-create-api-azureblobstorage.md#add-action) y [los inserte como mensaje en el centro de eventos](../../connectors/connectors-create-api-azure-event-hubs.md#add-action).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>¿Qué puedo hacer con los datos de supervisión que se envían a mi centro de eventos?

El enrutamiento de los datos de supervisión a un centro de eventos con Azure Monitor facilita la integración con las herramientas de asociados de Administración de eventos e información de seguridad y supervisión. La mayoría de las herramientas requieren la cadena de conexión al centro de eventos y determinados permisos para la suscripción de Azure para leer datos desde el centro de eventos. Esta es una lista incompleta de herramientas con la integración de Azure Monitor:

* **IBM QRadar**: el módulo específico del dispositivo y el protocolo del centro de eventos de Microsoft Azure están disponibles para descarga en [el sitio web de soporte técnico de IBM](http://www.ibm.com/support). [Más información sobre la integración con Azure aquí](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk**: dependiendo de su configuración de Splunk, existen dos enfoques:
    1. [El complemento Azure Monitor para Splunk](https://splunkbase.splunk.com/app/3534/) está disponible en Splunkbase y es un proyecto de código abierto. [La documentación se encuentra aquí](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
    2. Si no puede instalar un complemento en su instancia de Splunk (p. ej., si usa un proxy o se ejecuta en Splunk Cloud), puede reenviar estos eventos al recopilador de eventos de HTTP de Splunk mediante [esta función desencadenada por nuevos mensajes en el centro de eventos](https://github.com/Microsoft/AzureFunctionforSplunkVS).
* **SumoLogic**: las instrucciones sobre cómo configurar SumoLogic para consumir datos de un centro de eventos se encuentran [disponibles aquí](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub).
* **ArcSight**: el conector inteligente ArcSight de Azure Event Hubs está disponible como parte de [esta colección de conectores inteligentes ArcSight](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852).
* **Servidor de Syslog**: si desea transmitir los datos de Azure Monitor directamente a un servidor de syslog, puede consultar [este repositorio de GitHub](https://github.com/miguelangelopereira/azuremonitor2syslog/).

## <a name="next-steps"></a>Pasos siguientes
* [(Archivado del registro de actividades en una cuenta de almacenamiento)](../../azure-monitor/platform/archive-activity-log.md)
* [Lea la información general sobre el registro de actividades de Azure](../../azure-monitor/platform/activity-logs-overview.md)
* [Configure una alerta basada en un evento del registro de actividades](../../azure-monitor/platform/alerts-log-webhook.md)

