---
title: Expresión workspace() en una consulta de Azure Log Analytics | Microsoft Docs
description: La expresión workspace se usa en una consulta de Log Analytics para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 8b83650acfee638ae2a667cd0925fd91d44b80b2
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282703"
---
# <a name="workspace-expression-in-log-analytics-query"></a>Expresión workspace() en una consulta de Log Analytics

La expresión `workspace` se usa en una consulta de Log Analytics para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción. Es útil para incluir datos de registro en una consulta de Application Insights y para consultar datos a través de varias áreas de trabajo en una consulta de registro.


## <a name="syntax"></a>Sintaxis

`workspace(`*Identificador*`)`

## <a name="arguments"></a>Argumentos

- *Identificador*: identifica el área de trabajo mediante uno de los formatos de la tabla siguiente.

| Identificador | DESCRIPCIÓN | Ejemplo
|:---|:---|:---|
| Nombre de recurso | Nombre legible del área de trabajo (también denominado "nombre del componente") | workspace("contosoretail") |
| Nombre completo | Nombre completo del área de trabajo con el siguiente formato: "subscriptionName/resourceGroup/componentName" | workspace("Contoso/ContosoResource/ContosoWorkspace") |
| ID | GUID del área de trabajo | workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Id. de recurso de Azure | Identificador del recurso de Azure | workspace("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Notas

* Debe tener acceso de lectura al área de trabajo.
* Una expresión relacionada es `app`, que le permite realizar consultas en todas las aplicaciones de Application Insights.

## <a name="examples"></a>Ejemplos

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [expresión de aplicación](workspace-expression.md) para hacer referencia a la aplicación de Application Insights.
- Obtenga más información acerca de cómo se almacenan los [datos de Log Analytics](../../log-analytics/log-analytics-queries.md).