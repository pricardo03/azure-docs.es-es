---
title: Información general sobre los registros de diagnóstico de Azure
description: Obtenga información sobre los registros de diagnóstico de Azure en Azure Monitor y sobre cómo puede usarlos para entender los eventos que se producen en un recurso de Azure.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 8902e29baa5802e3416bcda97ca59a5576d41829
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244877"
---
# <a name="overview-of-azure-diagnostic-logs"></a>Información general sobre los registros de diagnóstico de Azure

Los **registros de diagnóstico** proporcionan datos exhaustivos y frecuentes acerca del funcionamiento de un recurso de Azure. Azure Monitor pone a disposición dos tipos de registros de diagnóstico:

* **Registros de inquilino**: provienen de servicios de nivel de inquilino que existen fuera de una suscripción de Azure, como los registros de Azure Active Directory.
* **Registros de recursos**: proceden de servicios de Azure que implementan recursos dentro de una suscripción de Azure, como grupos de seguridad de red o cuentas de almacenamiento.

    ![Comparación de los registros de diagnóstico de recursos y otros tipos de registros](media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

El contenido de estos registros varía según el servicio de Azure y el tipo de recurso. Por ejemplo, los contadores de regla de grupo de seguridad de red y las auditorías de Key Vault son dos tipos de registros de diagnóstico.

Estos registros de diagnóstico son distintos del [registro de actividad](activity-logs-overview.md). El registro de actividad proporciona una visión general de las operaciones que se realizaron en los recursos de la suscripción con Resource Manager, por ejemplo, crear una máquina virtual o eliminar una aplicación lógica. El registro de actividad es un registro de nivel de suscripción. Los registros de diagnóstico de nivel de recursos proporcionan una visión general de las operaciones realizadas dentro del mismo recurso, por ejemplo, obtener un secreto de un almacén de claves.

Estos registros también difieren de los registros de diagnóstico de nivel de sistema operativo invitado. Estos son los recopilados por un agente que se ejecuta dentro de una máquina virtual u otro tipo de recurso admitido. Los registros de diagnóstico de nivel de recursos no requieren ningún agente y capturan datos específicos de recurso de la plataforma Azure, mientras que los registros de diagnóstico de nivel de sistema operativo invitado capturan los datos desde el sistema operativo y las aplicaciones que se ejecutan en una máquina virtual.

No todos los servicios admiten los registros de diagnóstico que se describen aquí. [En este artículo se incluye una sección en la que se muestran los servicios que admiten los registros de diagnóstico](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>Qué se puede hacer con los registros de diagnóstico
Estas son algunas de las cosas que puede hacer con los registros de diagnóstico:

![Ubicación lógica de registros de diagnóstico](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Guardarlos en una [**cuenta de almacenamiento**](../../azure-monitor/platform/archive-diagnostic-logs.md) para archivarlos o inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días) usando la **configuración de diagnóstico de recursos**.
* [Transmitirlos a **Event Hubs**](diagnostic-logs-stream-event-hubs.md) para la ingesta en un servicio de terceros o una solución de análisis personalizado como Power BI.
* Analícelos con [Azure Monitor](../../azure-monitor/platform/collect-azure-metrics-logs.md), donde los datos se escriben inmediatamente en Azure Monitor sin necesidad de escribir primero los datos en el almacenamiento.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Puede usar una cuenta de almacenamiento o un espacio de nombres de Event Hubs que no esté en la misma suscripción que el que emite los registros. El usuario que configura los ajustes debe tener el acceso de RBAC adecuado a ambas suscripciones.

> [!NOTE]
>  Actualmente, no puede archivar registros de flujo de red en una cuenta de almacenamiento que está detrás de una red virtual protegida.

## <a name="diagnostic-settings"></a>Configuración de diagnóstico

Los registros de diagnóstico de recursos se configuran mediante la configuración de diagnóstico de recursos. Los registros de diagnóstico de inquilino se configuran mediante la configuración de diagnóstico de inquilino. **Configuración de diagnóstico** para un control de servicio:

* Dónde se envían los registros de diagnóstico y las métricas (cuenta de almacenamiento, Event Hubs o Azure Monitor).
* Qué categorías de registro se envían y si se envían también datos de métrica.
* Cuánto tiempo se debe conservar cada categoría de registro en una cuenta de almacenamiento.
    - Una retención de cero días significa que los registros se conservan de forma indefinida. De lo contrario, el valor puede ser cualquier número de días comprendido entre 1 y 365.
    - Si se establecen directivas de retención, pero el almacenamiento de registros en una cuenta de almacenamiento está deshabilitado (por ejemplo, si solo se han seleccionado las opciones Event Hubs o Log Analytics), las directivas de retención no surten ningún efecto.
    - Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas.

Estas opciones se ajustan en la configuración de los diagnósticos en el portal, con los comandos de Azure PowerShell y de la CLI, o mediante la [API REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Servicios admitidos, categorías y esquemas para los registros de diagnóstico

[Consulte este artículo](../../azure-monitor/platform/diagnostic-logs-schema.md) para obtener una lista completa de los servicios admitidos y las categorías de registro y los esquemas utilizados por esos servicios.

## <a name="next-steps"></a>Pasos siguientes

* [Transmisión de registros de diagnóstico a **Event Hubs**](diagnostic-logs-stream-event-hubs.md)
* [Cambio de la configuración de diagnóstico de recursos con la API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/)
* [Análisis de registros desde Azure Storage con Azure Monitor](collect-azure-metrics-logs.md)
