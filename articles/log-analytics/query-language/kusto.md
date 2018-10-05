---
title: Referencia del lenguaje de Log Analytics en Azure Monitor | Microsoft Docs
description: Información de referencia de lenguaje de Kusto utilizado por Log Analytics. Incluye elementos adicionales específicos de Log Analytics y elementos no admitidos en las consultas de Log Analytics.
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
ms.date: 09/25/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 5173790436a29fa9947346d711da1a2ddb32bf62
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451075"
---
# <a name="log-analytics-query-language-reference"></a>Referencia de lenguaje de consulta de Log Analytics
[Las consultas de Log Analytics](../log-analytics-queries.md) utilizan el mismo lenguaje y motor de consultas utilizado por el [Explorador de datos de Azure](/azure/data-explorer/). Puede acceder a la referencia de lenguaje y a otros detalles sobre el lenguaje desde la siguiente ubicación: [Referencia de lenguaje de Kusto](/azure/kusto/query)



## <a name="kusto-elements-not-support-in-log-analytics"></a>Elementos de Kusto que no se admiten en Log Analytics
Mientras que las consultas de Log Analytics utilizan una implementación de Kusto, hay algunos elementos de Kusto que no se admiten, tal como se describe en las siguientes secciones.

### <a name="statements-not-supported-in-log-analytics"></a>Instrucciones que no se admiten en Log Analytics

* [Alias](/kusto/query/aliasstatement)
* [Parámetros de consulta](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Funciones que no se admiten en Log Analytics

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Operadores que no se admiten en Log Analytics

* [Cross-Cluster Join](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Complementos que no se admiten en Log Analytics

* [sql_request plugin](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Operadores adiciones de Log Analytics
Con el fin de admitir características específicas de Log Analytics, se proporcionan los siguientes operadores adicionales de Kusto que no están disponibles fuera de Log Analytics. 

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Pasos siguientes

- Lea sobre consultas en [Log Analytics](../log-analytics-queries.md).
- Repase una lección sobre cómo escribir en una [consulta de Log Analytics](/log-analytics/query-language/get-started-queries.md).
- Acceda a la [documentación de referencia completa de Kusto](/azure/kusto/query/).