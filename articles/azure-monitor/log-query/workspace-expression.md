---
title: Expresión workspace() en consultas de registro de Azure Monitor | Microsoft Docs
description: La expresión workspace se usa en una consulta de registro de Azure Monitor para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.openlocfilehash: b4bc652d54150b72cc64898464b3511a860bf011
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57996308"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>Expresión workspace() en consultas de registro de Azure Monitor

La expresión `workspace` se usa en una consulta de Azure Monitor para recuperar datos de un área de trabajo específica en el mismo grupo de recursos, en otro grupo de recursos o en otra suscripción. Es útil para incluir datos de registro en una consulta de Application Insights y para consultar datos a través de varias áreas de trabajo en una consulta de registro.


## <a name="syntax"></a>Sintaxis

`workspace(`*Identificador*`)`

## <a name="arguments"></a>Argumentos

- *Identificador*: Identifica el área de trabajo mediante uno de los formatos de la tabla siguiente.

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

- Consulte la [expresión aplicación](app-expression.md) para hacer referencia a una aplicación de Application Insights.
- Obtenga información sobre cómo se almacenan los [datos de Azure Monitor](log-query-overview.md).
- Acceda a toda la documentación del [lenguaje de consulta de Kusto](/azure/kusto/query/).
