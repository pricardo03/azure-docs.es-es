---
title: Transmisión de registros de Azure Diagnostics a un área de trabajo de Log Analytics en Azure Monitor
description: Aprenda a transmitir registros de Azure Diagnostics a un área de trabajo de Log Analytics en Azure Monitor.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: e8e6276a38f06b5c6ebb24c89f3733b9fd7220f7
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612838"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Transmisión de registros de Azure Diagnostics a un área de trabajo de Log Analytics en Azure Monitor

**[Los registros de Azure Diagnostics](diagnostic-logs-overview.md)** se pueden transmitir casi en tiempo real a un área de trabajo de Log Analytics en Azure Monitor mediante el portal, cmdlets de PowerShell o la CLI de Azure.

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>Qué puede hacer con los registros de diagnóstico en un área de trabajo de Log Analytics

Azure Monitor proporciona una herramienta flexible de consulta y análisis de registros que permite obtener una visión general de los datos de registro sin procesar generados a partir de recursos de Azure. Entre estas funcionalidades, cabe destacar:

* **Consulta de registros**: escriba consultas avanzadas sobre los datos de registro, correlacione registros de varios orígenes y genere gráficos que se pueden anclar al panel de Azure.
* **Generación de alertas**: detecte cuando uno o varios eventos coinciden con una consulta determinada y reciba notificaciones por correo electrónico o llamada de webhook mediante alertas de Azure Monitor.
* **Análisis avanzados**: aplique el aprendizaje automático y algoritmos de coincidencia de patrones para identificar posibles problemas revelados por los registros.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>Habilitación de la transmisión de registros de diagnóstico a un área de trabajo de Log Analytics

Puede habilitar el streaming de registros de diagnóstico mediante programación, a través del portal o mediante la [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). En cualquier caso, puede crear una configuración de diagnóstico en la que se especifica un área de trabajo de Log Analytics y las categorías de registro y métricas que desea enviar a dicha área de trabajo. Una **categoría de registro** de diagnóstico es un tipo de registro que puede proporcionar un recurso.

El área de trabajo de Log Analytics no tiene que estar en la misma suscripción que la del recurso que emite los registros, siempre que el usuario que configura la configuración tenga acceso RBAC adecuado a ambas suscripciones.

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Streaming de registros de diagnóstico mediante el portal
1. En el portal, desplácese a Azure Monitor y haga clic en **Configuración de diagnóstico** en el menú **Configuración**.


2. Si lo desea, filtre la lista por tipo de recurso o por grupo de recursos y, a continuación, haga clic en el recurso para el que desea establecer la configuración de diagnóstico.

3. Si no existe ninguna configuración en el recurso que ha seleccionado, se le pide que cree una. Haga clic en "Activar diagnóstico".

   ![Agregar configuración de diagnóstico: sin configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   Si hay una configuración actual en el recurso, verá una lista de opciones ya configuradas en este recurso. Haga clic en "Agregar configuración de diagnóstico".

   ![Agregar configuración de diagnóstico: configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. Asigne un nombre de su configuración y active la casilla **Enviar a Log Analytics** y, después, seleccione un área de trabajo de Log Analytics.

   ![Agregar configuración de diagnóstico: configuración actual](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. Haga clic en **Save**(Guardar).

Transcurridos unos instantes, la nueva opción de configuración aparece en la lista de opciones para este recurso y los registros de diagnóstico se transmiten en esa área de trabajo en cuanto se generan nuevos datos de eventos. Pueden pasar hasta quince minutos entre el momento en que se emite un evento y el momento en que aparece en Log Analytics.

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

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>¿Cómo se consultan los datos de un área de trabajo de Log Analytics?

En la hoja Registros del portal de Azure Monitor, puede consultar los registros de diagnóstico como parte de la solución Administración de registros en la tabla AzureDiagnostics. También hay [varias soluciones de supervisión para recursos de Azure](../../azure-monitor/insights/solutions.md) que se pueden instalar para obtener información inmediata sobre los datos del registro que va a enviar a Azure Monitor.

### <a name="examples"></a>Ejemplos

```Kusto
// Resources that collect diagnostic logs into this Log Analytics workspace, using Diagnostic Settings
AzureDiagnostics
| distinct _ResourceId
```
```Kusto
// Resource providers collecting diagnostic logs into this Log Analytics worksapce, with log volume per category
AzureDiagnostics
| summarize count() by ResourceProvider, Category
```
```Kusto
// Resource types collecting diagnostic logs into this Log Analytics workspace, with number of resources onboarded
AzureDiagnostics
| summarize ResourcesOnboarded=dcount(_ResourceId) by ResourceType
```
```Kusto
// Operations logged by specific resource provider, in this example - KeyVault
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.KEYVAULT"
| distinct OperationName
```

## <a name="azure-diagnostics-vs-resource-specific"></a>Azure Diagnostics frente a recursos específicos  
Cuando un destino de Log Analytics se habilita en una configuración de Azure Diagnostics, hay dos maneras diferentes que los datos se mostrarán en el área de trabajo:  
- **Azure Diagnostics**: este es el método heredado que usa la mayoría de los servicios de Azure en la actualidad. En este modo, todos los datos de cualquier valor de diagnóstico que señala a un área de trabajo determinada terminarán en la tabla _AzureDiagnostics_. 
<br><br>Dado que muchos recursos envían datos a la misma tabla (_AzureDiagnostics_), el esquema de esta tabla es el superconjunto de los esquemas de todos los tipos de datos diferentes que se recopilan. Por ejemplo, si creó la configuración de diagnóstico para la colección de los siguientes tipos de datos, todos se envían a la misma área de trabajo:
    - Registros de auditoría de Recurso 1 (con un esquema que consta de las columnas A, B y C)  
    - Registros de errores de Recurso 2 (con un esquema que consta de las columnas D, E y F)  
    - Registros de flujo de datos de Recurso 3 (con un esquema que consta de las columnas G, H e I)  

    La tabla AzureDiagnostics tendrá el siguiente aspecto, con algunos datos de ejemplo:  

    | ResourceProvider | Categoría | Una | b | C | D | E | F | G | H | I |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
    | Microsoft.Resource2 | ErrorLogs | | | | q1 | w1 | e1 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j1 | k1 | l1|
    | Microsoft.Resource2 | ErrorLogs | | | | q2 | w2 | e2 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j3 | k3 | l3|
    | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
    | ... |

- **Específico del recurso**: en este modo, se crean tablas individuales en el área de trabajo seleccionada por cada categoría seleccionada en la configuración de diagnósticos. Con este método más reciente, resulta mucho más fácil encontrar exactamente lo que busca a través de la separación explícita de preocupaciones: una tabla para cada categoría. Además, ofrece ventajas en relación con su compatibilidad con tipos dinámicos. Ya puede ver este modo para determinados tipos de recursos de Azure, por ejemplo, los registros de [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics) o [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor). En última instancia, se espera que cada tipo de datos migrará al modo de específico del recurso. 

    En el ejemplo anterior, esto daría lugar a la creación de tres tablas: 
    - Tabla _AuditLogs_ como sigue:

        | ResourceProvider | Categoría | Una | b | C |
        | -- | -- | -- | -- | -- |
        | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
        | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
        | ... |

    - Tabla _ErrorLogs_ como sigue:  

        | ResourceProvider | Categoría | D | E | F |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource2 | ErrorLogs | q1 | w1 | e1 |
        | Microsoft.Resource2 | ErrorLogs | q2 | w2 | e2 |
        | ... |

    - Tabla _DataFlowLogs_ como sigue:  

        | ResourceProvider | Categoría | G | H | I |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource3 | DataFlowLogs | j1 | k1 | l1|
        | Microsoft.Resource3 | DataFlowLogs | j3 | k3 | l3|
        | ... |

    Entre otras ventajas de usar el modo específico del recurso se incluye un mejor rendimiento entre los tiempos de latencia de ingesta y consulta, una mejor capacidad de detección de los esquemas y su estructura, la capacidad para conceder derechos RBAC en una tabla específica y mucho más.

### <a name="selecting-azure-diagnostic-vs-resource-specific-mode"></a>Selección del modo Azure Diagnostic frente al modo específico del recurso
Para la mayoría de los recursos de Azure, no tendrá la opción de usar el modo Azure Diagnostic o el modo específico del recurso; los datos pasarán de forma automática por el método que ha seleccionado usar el recurso. Consulte la documentación que proporcione el recurso que haya habilitado para enviar datos a Log Analytics para obtener más información sobre qué modo se está usando. 

Tal como se indicó en la sección anterior, en última instancia es el objetivo de Azure Monitor hacer que todos los servicios en Azure usen el modo específico del recurso. Para facilitar esta transición y asegurarse de que no se pierdan datos como parte del proceso, en el momento de la incorporación a Log Analytics, algunos servicios de Azure le proporcionarán una selección del modo:  
   ![Selector del modo de configuración de diagnósticos](media/diagnostic-logs-stream-log-store/diagnostic-settings-mode-selector.png)

Recomendamos **encarecidamente** que, para evitar migraciones potencialmente difíciles potencialmente más adelante, toda configuración de diagnóstico que crea use el modo centrado en el recurso.  

Para la configuración de diagnóstico existente, una vez habilitada por un determinado recurso de Azure, podrá cambiar de manera retroactiva desde el modo Azure Diagnostic al modo específico del recurso. Los datos ingeridos anteriormente seguirán disponibles en la tabla _AzureDiagnostics_ hasta que quede anticuado según la configuración de retención en el área de trabajo, pero los datos nuevos se enviarán a la tabla dedicada. Esto significa que para las consultas que tengan que abarcar tanto los datos antiguos y como los nuevos (hasta que los datos antiguos queden anticuados completamente), se necesitará un operador [unión](https://docs.microsoft.com/azure/kusto/query/unionoperator) en las consultas para combinar los dos conjuntos de datos.

Consulte las novedades sobre los servicios de Azure que admiten registros en el modo específico del recurso en el blog de [actualizaciones de Azure](https://azure.microsoft.com/updates/).

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Limitación conocida: límite de columnas en AzureDiagnostics
Hay un límite explícito de cualquier tabla de registros de Azure determinada de no tener más de 500 columnas. Una vez alcanzado, cualquier fila que contenga datos con cualquier columna fuera de las primeras 500 se eliminará en el momento de la ingestión. La tabla AzureDiagnostics es especialmente susceptible de verse afectada por este límite. Esto suele ocurrir porque una gran variedad de orígenes de datos se envía a la misma área de trabajo o porque se envían varios orígenes de datos detallados a la misma área de trabajo. 

#### <a name="azure-data-factory"></a>Azure Data Factory  
Se sabe que Azure Data Factory, debido a un conjunto de registros muy detallado, es un recurso al que afecta especialmente este límite. En concreto, para cualquier configuración de diagnóstico establecida antes de que el modo específico del recurso se haya habilitado o para elegir explícitamente usar el modo específico del recurso por motivos de compatibilidad inversa:  
- *Parámetros de usuario definidos en cualquier actividad en la canalización*: se creará una nueva columna para cada parámetro de usuario con nombre único en cualquier actividad. 
- *Entradas y salidas de actividad*: varían de una actividad a otra y generan un gran número de columnas debido a su naturaleza detallada. 
 
Como con las propuestas más amplias de solución alternativa siguientes, se recomienda migrar los registros para usar el modo específico del recurso tan pronto como sea posible. Si no puede hacerlo inmediatamente, es una solución alternativa provisional es aislar los registros de ADF en su propia área de trabajo para minimizar la posibilidad de que estos registros afecten a otros tipos de registro que se recopilan en las áreas de trabajo. 
 
#### <a name="workarounds"></a>Soluciones alternativas
A corto plazo, hasta que todos los servicios de Azure se hayan habilitado en el modo específico del recurso, para todos los servicios que aún no admite este modo se recomienda separar los tipos de datos detallados publicados por estos servicios en áreas de trabajo independientes para reducir el posibilidad de llegar al límite.  
 
A largo plazo, Azure Diagnostics hará que todos los servicios de Azure admitan el modo específico del recurso. Se recomienda pasar a este modo tan pronto como sea posible para reducir la posibilidad de que se vea afectado por esta limitación de 500 columnas.  


## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los registros de Diagnósticos de Azure](diagnostic-logs-overview.md)

