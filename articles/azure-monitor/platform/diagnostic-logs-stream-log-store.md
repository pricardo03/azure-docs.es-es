---
title: Registros de diagnóstico de Azure Stream al área de trabajo de Log Analytics en Azure Monitor
description: Aprenda a transmitir registros de diagnóstico de Azure a un área de trabajo de Log Analytics en Azure Monitor.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: b17978da3195b364f868d33ab7ad9faa1544e9ec
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006801"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Registros de diagnóstico de Azure Stream al área de trabajo de Log Analytics en Azure Monitor

**[Registros de diagnóstico de Azure](diagnostic-logs-overview.md)**  se pueden transmitir casi en tiempo real a un área de trabajo de Log Analytics en Azure Monitor mediante el portal, cmdlets de PowerShell o CLI de Azure.

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>¿Qué puede hacer con los diagnósticos de registros en un área de trabajo de Log Analytics

Azure Monitor proporciona una herramienta de consulta y análisis de registros flexible que le permite obtener información sobre los datos de registro sin procesar generados a partir de los recursos de Azure. Entre estas funcionalidades, cabe destacar:

* **Consulta de registro** -escritura de consultas avanzadas sobre los datos de registro, correlacione registros desde varios orígenes y generan gráficos que se pueden anclar al panel de Azure.
* **Alertas** -detectar cuando uno o varios eventos coinciden con una consulta determinada y reciba notificaciones por una llamada de correo electrónico o webhook mediante Azure Monitor genera una alerta.
* **Análisis avanzados**: aplique el aprendizaje automático y algoritmos de coincidencia de patrones para identificar posibles problemas revelados por los registros.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>Habilitar el streaming de registros de diagnóstico al área de trabajo de Log Analytics

Puede habilitar el streaming de registros de diagnóstico mediante programación, a través del portal o mediante la [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). En cualquier caso, puede crear una configuración de diagnóstico en la que se especifica un área de trabajo de Log Analytics y las categorías de registro y métricas que desea enviar a dicha área de trabajo. Una **categoría de registro** de diagnóstico es un tipo de registro que puede proporcionar un recurso.

El área de trabajo de Log Analytics no tiene que estar en la misma suscripción que la del recurso que emite los registros, siempre que el usuario que configura la configuración tenga acceso RBAC adecuado a ambas suscripciones.

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Streaming de registros de diagnóstico mediante el portal
1. En el portal, vaya a Azure Monitor y haga clic en **configuración de diagnóstico** en el **configuración** menú.


2. Si lo desea, filtre la lista por tipo de recurso o por grupo de recursos y, a continuación, haga clic en el recurso para el que desea establecer la configuración de diagnóstico.

3. Si no existe ninguna configuración en el recurso que ha seleccionado, se le pide que cree una. Haga clic en "Activar diagnóstico".

   ![Agregar configuración de diagnóstico: sin configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   Si hay una configuración actual en el recurso, verá una lista de opciones ya configuradas en este recurso. Haga clic en "Agregar configuración de diagnóstico".

   ![Agregar configuración de diagnóstico: configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. Asigne un nombre de su configuración y active la casilla **Enviar a Log Analytics** y, después, seleccione un área de trabajo de Log Analytics.

   ![Agregar configuración de diagnóstico: configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. Haga clic en **Save**(Guardar).

Transcurridos unos instantes, la nueva opción de configuración aparece en la lista de opciones para este recurso y los registros de diagnóstico se transmiten en esa área de trabajo en cuanto se generan nuevos datos de eventos. Tenga en cuenta que pueden pasar hasta quince minutos entre cuando se emite un evento y cuando aparece en Log Analytics.

### <a name="via-powershell-cmdlets"></a>Mediante cmdlets de PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para habilitar el streaming mediante [cmdlets de Azure PowerShell](../../azure-monitor/platform/powershell-quickstart-samples.md), puede utilizar el cmdlet `Set-AzDiagnosticSetting` con estos parámetros:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Tenga en cuenta que la propiedad workspaceID toma el identificador de recurso completo de Azure del área de trabajo, no el identificador/clave del área de trabajo que se muestra en el portal de Log Analytics.

### <a name="via-azure-cli"></a>Mediante la CLI de Azure

Para habilitar el streaming a través de la [CLI de Azure](../../azure-monitor/platform/cli-samples.md), puede usar el comando [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Puede agregar categorías adicionales al registro de diagnóstico mediante la incorporación de diccionarios a la matriz JSON que se pasa como el parámetro `--logs`.

El argumento `--resource-group` solo es obligatorio si `--workspace` no es un identificador de objeto.

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>¿Cómo se puede consultar los datos de un área de trabajo de Log Analytics?

En la hoja de registros en el portal de Azure Monitor, puede consultar los registros de diagnóstico como parte de la solución de administración de registros en la tabla AzureDiagnostics. También hay [varias soluciones de supervisión para recursos de Azure](../../azure-monitor/insights/solutions.md) puede instalar para obtener información inmediata sobre los datos de registro va a enviar a Azure Monitor.

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Limitación conocida: límite de la columna en AzureDiagnostics
Dado que muchos recursos enviar todos los tipos de datos se envían a la misma tabla (_AzureDiagnostics_), el esquema de esta tabla es el conjunto de superusuarios de los esquemas de todos los tipos de datos diferentes que se recopilan. Por ejemplo, si ha creado la configuración de diagnóstico de la colección de los siguientes tipos de datos, todos envían a la misma área de trabajo:
- Auditoría de registros de recurso de 1 (con un esquema que consta de las columnas A, B y C)  
- Registros de errores de recurso 2 (con un esquema que consta de columnas D, E y F)  
- Registros de flujo de datos de 3 recursos (con un esquema que consta de columnas G, H y me)  
 
La tabla AzureDiagnostics será como sigue, con algunos datos de ejemplo:  
 
| ResourceProvider | Category | Una  | b | C | D | E | F | G | H | I |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
| Microsoft.Resource2 | ErrorLogs | | | | q1 | W1 | e1 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | j1 | k1 | l1|
| Microsoft.Resource2 | ErrorLogs | | | | q2 | W2 | e2 |
| Microsoft.Resource3 | DataFlowLogs | | | | | | | j3 | k3 | l3|
| Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
| ... |
 
Hay un límite explícito de cualquier tabla de registro de Azure determinada no tener más de 500 columnas. Una vez alcanzado, se eliminarán las filas que contengan datos con cualquier columna fuera de los primeros 500 en tiempo de ingesta. La tabla AzureDiagnostics es especialmente sensibles ser afectado este límite. Esto suele ocurrir porque una gran variedad de orígenes de datos se envían a la misma área de trabajo, o varios de los orígenes de datos muy detallado que se envían a la misma área de trabajo. 
 
#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory, debido a un conjunto muy detallado de los registros, es un recurso que se conoce a especialmente verse afectadas por este límite. En particular:  
- *Parámetros de usuario definidos en relación con cualquier actividad en la canalización*: habrá una nueva columna que se crea para cada parámetro de forma unívoca con nombre de usuario en cualquier actividad. 
- *Actividad entradas y salidas*: estas varían de actividad a otra y generar una gran cantidad de columnas debido a su naturaleza detallado. 
 
Como con las propuestas más amplias de solución alternativa siguientes, se recomienda para aislar los registros ADF en su propia área de trabajo para minimizar la posibilidad de que estos registros que afectan a otros tipos de registro que se recopilan en las áreas de trabajo. Esperamos que haya reunido registros para Azure Data Factory disponibles pronto.
 
#### <a name="workarounds"></a>Soluciones alternativas
A corto plazo, hasta que se vuelve a definir el límite de 500 columnas, se recomienda separar los tipos de datos detallados en las áreas de trabajo independientes para reducir las posibilidades de alcanzar el límite.
 
A largo plazo, diagnósticos de Azure va a mover fuera de un esquema unificado, disperso en tablas individuales por cada tipo de datos; se empareja con compatibilidad con tipos dinámicos, esto mejorará enormemente la facilidad de uso de datos que entran en los registros de Azure a través del mecanismo de diagnósticos de Azure. Ya puede ver esto para seleccionar los tipos de recursos de Azure, por ejemplo [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) o [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor) registros. Busque noticias acerca de nuevos tipos de recursos de Azure que admiten estos registros protegidos en el [las actualizaciones de Azure](https://azure.microsoft.com/updates/) blog!


## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de Diagnósticos de Azure](diagnostic-logs-overview.md)

