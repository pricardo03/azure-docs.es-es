---
title: Cambio de la API heredada de alertas de Log Analytics a la nueva API de Alertas de Azure
description: Información general sobre la retirada de savedSearch heredada basadas en la API de alertas de Log Analytics y proceso para cambiar las reglas de alerta a la nueva API ScheduledQueryRules, con detalles que abordar las inquietudes comunes de los clientes.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 6af1c5347a522f7e42feecb6722dfbb64439d086
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341018"
---
# <a name="switch-api-preference-for-log-alerts"></a>Cambio de la preferencia de API para las alertas de registro

> [!NOTE]
> Contenido que **no** se aplica a los usuarios de Azure GovCloud y únicamente a los usuarios de nube pública de Azure.  

Hasta hace poco, las reglas de alertas se administraban en el portal de Microsoft Operations Management Suite. La nueva experiencia de alertas se ha integrado con diversos servicios en Microsoft Azure, incluido Log Analytics y hemos pedido [ampliar las reglas de alertas del portal de OMS a Azure](alerts-extend.md). Pero para asegura una interrupción mínima para los clientes, el proceso no modificó la interfaz de programación para su consumo: [API de alertas de Log Analytics](api-alerts.md) basada en SavedSearch.

Pero ahora anuncia para los usuarios de alertas de Log Analytics una verdadera alternativa de programación de Azure, [API ScheduledQueryRules de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), que también se refleja en su [facturación de Azure para alertas de registro](alerts-unified-log.md#pricing-and-billing-of-log-alerts). Para más información sobre cómo administrar las alertas de registro mediante la API, consulte [cómo administrar las alertas de registro mediante la plantilla de recursos de Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) y [Administración de alertas de registro con PowerShell, la CLI o la API](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).

## <a name="benefits-of-switching-to-new-azure-api"></a>Ventajas de cambiar a la nueva API de Azure

Hay varias ventajas de crear y administrar alertas mediante la [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) frente a la [API heredada de alertas de Log Analytics](api-alerts.md); a continuación se enumeran algunas de las principales:

- Capacidad de [buscar en registros entre varias áreas de trabajo](../log-query/cross-workspace-query.md) en las reglas de alerta y abarcar recursos externos, como áreas de trabajo de Log Analytics o incluso aplicaciones de Application Insights
- Cuando se usan varios campos para agrupar en la consulta, al usar la [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) el usuario puede especificar en qué campo agregar en Azure Portal
- Si registra las alertas creadas mediante [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), puede obtener un período definido de hasta 48 horas y recuperar datos durante un período más largo que el anterior.
- Las reglas de alertas se crean de una sola vez como único recurso sin necesidad de crear tres niveles de recursos, como con la [API heredada de alertas de Log Analytics](api-alerts.md)
- Interfaz de programación única para todas las variantes de las alertas de registro basadas en consultas en Azure; la nueva [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) puede usarse para administrar las reglas para Log Analytics, así como para Application Insights
- Toda nueva funcionalidad de alertas de registro y los desarrollos futuros estarán disponible solo a través de la nueva [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Proceso de cambio de la API heredada de alertas de registro

El proceso de trasladar las reglas de alertas de la [API heredada de alertas de Log Analytics](api-alerts.md) no implica cambiar de ningún modo la configuración, la consulta ni la definición de las alertas. Los usuarios tienen libertad para usar la [API heredada de alertas de Log Analytics](api-alerts.md) o la nueva [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Las reglas de alertas creadas por cualquiera de las API se *podrán administrar mediante la misma API únicamente*, así como desde Azure Portal. De forma predeterminada, Azure Monitor seguirá usando la [API heredada de alertas de Log Analytics](api-alerts.md) para crear cualquier nueva regla de alertas desde Azure Portal.

A continuación, se compilan los impactos del cambio de preferencia a la API scheduledQueryRules:

- Todas las interacciones para administrar las alertas de registro a través de interfaces programáticas ahora deben hacerse con [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en su lugar. Para más información, consulte el [ejemplo de uso a través de la plantilla de recursos de Azure](alerts-log.md#managing-log-alerts-using-azure-resource-template) y el [ejemplo de uso mediante la CLI de Azure y PowerShell](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).
- Toda nueva regla de alerta de registro creada en Azure Portal se creará usando [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) únicamente y permitirá, además, que los usuarios usen la [funcionalidad adicional de la nueva API](#Benefits-of-switching-to-new-Azure-API) a través de Azure Portal.

Cualquier cliente que quiera cambiar voluntariamente a la nueva [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) y bloquee el uso de la [API heredada de alertas de Log Analytics](api-alerts.md) puede hacerlo mediante una llamada PUT en la API siguiente para cambiar todas las reglas de alertas reglas asociadas con el área de trabajo de Log Analytics específica.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Con el cuerpo de solicitud que contiene el siguiente código JSON.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

También puede obtener acceso a la API desde una línea de comandos de PowerShell mediante [ARMClient](https://github.com/projectkudu/ARMClient), una herramienta de línea de comandos de código abierto que simplifica la tarea de invocar a la API de Azure Resource Manager. Tal como se muestra a continuación, el ejemplo de llamada PUT usa la herramienta ARMclient para cambiar todas las reglas de alerta asociadas al área de trabajo de Log Analytics específica.

```PowerShell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Si el cambio de todas las reglas de alertas en el área de trabajo de Log Analytics para usar la nueva [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) es correcto, se proporcionará la respuesta siguiente.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Además, los usuarios pueden comprobar el estado actual del área de trabajo de Log Analytics y ver si ha cambiado o no al uso de [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) únicamente. Para comprobarlo, los usuarios pueden hacer una llamada GET en la API siguiente.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Para ejecutar los pasos anteriores mediante la línea de comandos de PowerShell con la herramienta [ARMClient](https://github.com/projectkudu/ARMClient), consulte el ejemplo siguiente.

```PowerShell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Si el área de trabajo de Log Analytics especificada se ha cambiado para usar [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) únicamente, la respuesta JSON será como se muestra a continuación.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
De lo contrario, si el área de trabajo de Log Analytics especificada aún no se ha cambiado para usar [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) únicamente, la respuesta JSON será como se muestra a continuación.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [Alertas de registro en Azure Monitor](alerts-unified-log.md).
- Aprenda a crear [alertas de registro en Alertas de Azure](alerts-log.md).
- Obtenga más información sobre la experiencia de [Alertas de Azure](../../azure-monitor/platform/alerts-overview.md).
