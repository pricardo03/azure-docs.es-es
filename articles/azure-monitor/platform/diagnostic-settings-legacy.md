---
title: 'Recopilación del registro de actividad de Azure con configuración de diagnóstico (versión preliminar): Azure Monitor | Microsoft Docs'
description: Use la configuración de diagnóstico para reenviar registros de actividad de Azure a los registros de Azure Monitor, a Azure Storage o a Azure Event Hubs.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 02/04/2020
ms.openlocfilehash: fcdcef5d63163b24fe5de0f547dc2dde00cd674f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77016262"
---
# <a name="update-to-azure-activity-log-collection-and-export"></a>Actualización de la recopilación del registro de actividad de Azure y exportación
El [Registro de actividad de Azure](platform-logs-overview.md) es un [registro de plataforma](platform-logs-overview.md) que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. El método para enviar entradas del registro de actividad a [un centro de eventos o una cuenta de almacenamiento](activity-log-export.md) o a un [área de trabajo de Log Analytics](activity-log-collect.md) ha cambiado para usar la [configuración de diagnóstico](diagnostic-settings.md). En este artículo se describe la diferencia entre los métodos y cómo borrar la configuración heredada como preparación para la configuración de diagnóstico.


## <a name="differences-between-methods"></a>Diferencias entre los métodos

### <a name="advantages"></a>Ventajas
El uso de la configuración de diagnóstico tiene las ventajas siguientes con respecto a los métodos actuales:

- Método coherente para recopilar todos los registros de la plataforma.
- Recopilación del registro de actividad en varias suscripciones e inquilinos.
- Filtrado de la colección para recopilar solo los registros de determinadas categorías.
- Recopilar todas las categorías de registro de actividad. Algunas categorías no se recopilan con el método heredado.
- Latencia más rápida para la ingesta de registros. El método anterior tiene aproximadamente 15 minutos de latencia mientras que la configuración de diagnóstico solo agrega aproximadamente 1 minuto.

### <a name="considerations"></a>Consideraciones
Tenga en cuenta los detalles siguientes de la recopilación del registro de actividad con la configuración de diagnóstico antes de habilitar esta característica.

- Se ha quitado la configuración de retención para recopilar el registro de actividad en Azure Storage, lo que significa que los datos se almacenarán de forma indefinida hasta que los quite.
- Actualmente solo puede crear una configuración de diagnóstico de nivel de suscripción con Azure Portal. Para usar otros métodos, como PowerShell o CLI, puede crear una plantilla de Resource Manager.


### <a name="differences-in-data"></a>Diferencias en los datos
La configuración de diagnóstico recopila los mismos datos que los métodos anteriores usados para recopilar el registro de actividad con las siguientes diferencias actuales:

Se han quitado las siguientes columnas. Las columnas de reemplazo están en un formato diferente de las anteriores, por lo que es posible que tenga que modificar las consultas de registro que las utilizan. Todavía puede ver las columnas eliminadas en el esquema, pero no se rellenarán con datos.

| Columna quitada | Columna de reemplazo |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| OperationName     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

Se han agregado las siguientes columnas:

- Authorization_d
- Claims_d
- Properties_d

> [!IMPORTANT]
> En algunos casos, los valores de estas columnas pueden estar en mayúsculas. Si tiene una consulta que incluye estas columnas, debe usar el [operador =~](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators) para realizar una comparación sin distinguir entre mayúsculas y minúsculas.

## <a name="work-with-legacy-settings"></a>Trabajar con la configuración heredada
La configuración heredada para recopilar el registro de actividad seguirá funcionando si no opta reemplazar por una configuración de diagnóstico. Use el método siguiente para administrar el perfil de registro de una suscripción.

1. En el menú **Azure Monitor** de Azure Portal, seleccione **Registro de actividad**.
3. Haga clic en **Configuración de diagnóstico**.

   ![Configuración de diagnóstico](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Haga clic en el banner de color púrpura de la experiencia heredada.

    ![Experiencia heredada](media/diagnostic-settings-subscription/legacy-experience.png)


Consulte los artículos siguientes para obtener más información sobre el uso de los métodos de recopilación heredados.

- [Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics en Azure Monitor](activity-log-collect.md)
- [Recopilación de registros de actividad de Azure en Azure Monitor en inquilinos de Azure Active Directory](activity-log-collect-tenants.md)
- [Exportación del registro de actividad de Azure al almacenamiento o a Azure Event Hubs](activity-log-export.md)

## <a name="disable-existing-settings"></a>Deshabilitar la configuración existente
Debe deshabilitar la recopilación existente de la actividad antes de habilitarla con la configuración de diagnóstico. Si las dos están habilitadas, se pueden producir datos duplicados.

### <a name="disable-collection-into-log-analytics-workspace"></a>Deshabilitar la recopilación en el área de trabajo Log Analytics

1. Abra el menú **Áreas de trabajo de Log Analytics** en Azure Portal y seleccione el área de trabajo para recopilar el registro de actividad.
2. En la sección **Orígenes de datos del área de trabajo** del menú del área de trabajo, seleccione **Registro de actividad de Azure**.
3. Haga clic en la suscripción que quiera desconectar.
4. Haga clic en **Desconectar** y después en **Sí** cuando se le pida que confirme la elección.

### <a name="disable-log-profile"></a>Deshabilitar un perfil de registro

1. Siga el procedimiento descrito en [Trabajar con la configuración heredada](#work-with-legacy-settings) para abrir la configuración heredada.
2. Deshabilite cualquier colección actual en el almacenamiento o Event Hubs.



## <a name="activity-log-monitoring-solution"></a>Solución de supervisión del registro de actividad
La solución de supervisión de Azure Log Analytics incluye varias vistas y consultas de registros para analizar las entradas del registro de actividad en el área de trabajo de Log Analytics. Esta solución usa los datos de registro recopilados en un área de trabajo de Log Analytics y continuará funcionando sin ningún cambio si recopila el registro de actividad mediante la configuración de diagnóstico. Para obtener más información sobre esta solución, vea [Solución de supervisión de Activity Log Analytics](activity-log-collect.md#activity-logs-analytics-monitoring-solution).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el registro de actividad](../../azure-resource-manager/management/view-activity-logs.md)
* [Más información sobre la configuración de diagnóstico](diagnostic-settings.md)
