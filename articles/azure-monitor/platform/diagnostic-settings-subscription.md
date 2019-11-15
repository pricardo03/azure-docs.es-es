---
title: 'Recopilación del registro de actividad de Azure con configuración de diagnóstico (versión preliminar): Azure Monitor | Microsoft Docs'
description: Use la configuración de diagnóstico para reenviar registros de actividad de Azure a los registros de Azure Monitor, a Azure Storage o a Azure Event Hubs.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 10/31/2019
ms.openlocfilehash: 9f8783dc6d3c14b086364639b60273dbae626cee
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586993"
---
# <a name="collect-azure-activity-log-with-diagnostic-settings-preview"></a>Recopilación del registro de actividad de Azure con configuración de diagnóstico (versión preliminar)
El [Registro de actividad de Azure](activity-logs-overview.md) es un [registro de plataforma](platform-logs-overview.md) que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. Hasta ahora, ha creado un perfil de registro para enviar las entradas del registro de actividad a [un centro de eventos o una cuenta de almacenamiento](activity-log-export.md), y ha usado un conector para recopilarlos en un [área de trabajo Log Analytics](activity-log-collect.md).

Ahora puede configurar la recopilación del registro de actividad de Azure con la misma [configuración de diagnóstico](diagnostic-settings.md) que se usa para recopilar [registros de recursos](resource-logs-overview.md). El uso de la configuración de diagnóstico tiene las ventajas siguientes con respecto a los métodos actuales:

- Método coherente para recopilar todos los registros de la plataforma.
- Recopilación del registro de actividad en varias suscripciones e inquilinos.
- Filtrado de la colección para recopilar solo los registros de determinadas categorías.

## <a name="considerations"></a>Consideraciones
Tenga en cuenta los detalles siguientes de la recopilación del registro de actividad con la configuración de diagnóstico antes de habilitar esta característica.

- Debe deshabilitar la recopilación existente de la actividad antes de habilitarla con la configuración de diagnóstico. Si las dos están habilitadas, se pueden producir datos duplicados.
- Se ha quitado la configuración de retención para recopilar el registro de actividad en Azure Storage, lo que significa que los datos se almacenarán de forma indefinida hasta que los quite.
- Puede enviar el registro de actividad desde varias suscripciones a una única área de trabajo de Log Analytics. Puede enviar el registro de actividad desde cualquier suscripción única hasta un máximo de cinco áreas de trabajo de Log Analytics.

## <a name="configure-diagnostic-settings"></a>Configuración de valores de diagnóstico
Use el procedimiento siguiente para crear una configuración de diagnóstico en Azure Portal para recopilar el registro de actividad de Azure. Actualmente no se puede crear una configuración de diagnóstico de nivel de suscripción con otros métodos.

1. Deshabilite la colección de áreas de trabajo de Log Analytics existentes para el registro de actividad:
   1. Abra el menú **Áreas de trabajo de Log Analytics** en Azure Portal y seleccione el área de trabajo para recopilar el registro de actividad.
   2. En la sección **Orígenes de datos del área de trabajo** del menú del área de trabajo, seleccione **Registro de actividad de Azure**.
   3. Haga clic en la suscripción que quiera desconectar.
   4. Haga clic en **Desconectar** y después en **Sí** cuando se le pida que confirme la elección.
2. En el menú **Azure Monitor** de Azure Portal, seleccione **Registro de actividad**.
3. Haga clic en **Configuración de diagnóstico**.
   
   ![Configuración de diagnóstico](media/diagnostic-settings-subscription/diagnostic-settings.png)
   
4. Haga clic en el banner de color púrpura de la experiencia heredada y deshabilite cualquier colección actual en almacenamiento o centros de eventos. 

    ![Experiencia heredada](media/diagnostic-settings-subscription/legacy-experience.png)

5. Siga los procedimientos de [Creación de configuraciones de diagnóstico en Azure Portal](diagnostic-settings.md#create-diagnostic-settings-in-azure-portal) para crear una configuración de diagnóstico. Vea [Categorías del Registro de actividad](activity-logs-overview.md#categories-in-the-activity-log) para obtener una explicación de las categorías que puede usar para filtrar los eventos del registro de actividad. 


## <a name="differences-in-data"></a>Diferencias en los datos
La configuración de diagnóstico recopila los mismos datos que los métodos anteriores usados para recopilar el registro de actividad con las siguientes diferencias actuales:

Se han quitado las propiedades siguientes:

- ActivityStatus
- ActivitySubstatus
- OperationName
- ResourceProvider 

Se han agregado las propiedades siguientes:

- Authorization_d
- Claims_d
- Properties_d

## <a name="activity-log-monitoring-solution"></a>Solución de supervisión del registro de actividad
La solución de supervisión de Azure Log Analytics incluye varias vistas y consultas de registros para analizar las entradas del registro de actividad en el área de trabajo de Log Analytics. Esta solución usa los datos de registro recopilados en un área de trabajo de Log Analytics y continuará funcionando sin ningún cambio si recopila el registro de actividad mediante la configuración de diagnóstico. Para obtener más información sobre esta solución, vea [Solución de supervisión de Activity Log Analytics](activity-log-collect.md#activity-logs-analytics-monitoring-solution).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el registro de actividad](../../azure-resource-manager/resource-group-audit.md)
* [Más información sobre la configuración de diagnóstico](diagnostic-settings.md)
