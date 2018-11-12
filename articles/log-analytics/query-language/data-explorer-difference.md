---
title: Referencia del lenguaje de Log Analytics en Azure Monitor | Microsoft Docs
description: Información de referencia del lenguaje de consulta de Data Explorer que utiliza Log Analytics. Incluye elementos adicionales específicos de Log Analytics y elementos no admitidos en las consultas de Log Analytics.
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
ms.date: 10/31/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 65f3388074ff7a84c6e0516a64665019d5883ab1
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913179"
---
# <a name="log-analytics-query-language-differences"></a>Diferencias del lenguaje de consulta de Log Analytics

Aunque [Log Analytics](../log-analytics-queries.md) se basa en [Azure Data Explorer](/azure//data-explorer) y usa el [mismo lenguaje de consulta](/azure/kusto/query), la versión del lenguaje presenta algunas diferencias. En este artículo se identifican los elementos que son diferentes entre la versión del lenguaje que se utiliza en Data Explorer y la versión que se utiliza en las consultas de Log Analytics.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Elementos de Data Explorer que no se admiten en Log Analytics
En las siguientes secciones se describen los elementos del lenguaje de consulta de Data Explorer que no son compatibles con Log Analytics.

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
Los operadores siguientes admiten características específicas de Log Analytics que no están disponibles fuera de Log Analytics.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga referencias a distintos [recursos para escribir consultas de Log Analytics](query-language.md).
- Obtenga acceso a toda la [documentación de referencia del lenguaje de consulta de Data Explorer](/azure/kusto/query/).
