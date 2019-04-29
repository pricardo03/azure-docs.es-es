---
title: Diferencias en el lenguaje de consulta de los registros de Azure Monitor | Microsoft Docs
description: Información de referencia del lenguaje de consulta de Kusto que utiliza Azure Monitor. Incluye elementos adicionales específicos de Azure Monitor y elementos no admitidos en las consultas del registro de Azure Monitor.
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
ms.date: 10/31/2018
ms.author: bwren
ms.openlocfilehash: 1185f3f96fd39f168d138d7dbf66e7780884b1fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589270"
---
# <a name="azure-monitor-log-query-language-differences"></a>Diferencias en el lenguaje de consulta de los registros de Azure Monitor

Aunque los [registros de Azure Monitor](log-query-overview.md) se basan en [Azure Data Explorer](/azure/data-explorer) y usa el mismo [lenguaje de consulta de Kusto](/azure/kusto/query), la versión del lenguaje presenta algunas diferencias. En este artículo se identifican los elementos que son diferentes entre la versión del lenguaje que se utiliza en Data Explorer y la versión que se utiliza en las consultas del registro de Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>Elementos de KQL que no se admiten en Azure Monitor
En las siguientes secciones se describen los elementos del lenguaje de consulta de Kusto que no son compatibles con Azure Monitor.

### <a name="statements-not-supported-in-azure-monitor"></a>Instrucciones que no se admiten en Azure Monitor

* [Alias](/azure/kusto/query/aliasstatement)
* [Parámetros de consulta](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Funciones que no se admiten en Azure Monitor

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Operadores que no se admiten en Azure Monitor

* [Cross-Cluster Join](/azure/kusto/query/joincrosscluster)
* [externaldata operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-azure-monitor"></a>Complementos que no se admiten en Azure Monitor

* [sql_request plugin](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Operadores adicionales de Azure Monitor
Los operadores siguientes admiten características específicas de Azure Monitor que no están disponibles fuera de Azure Monitor.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga referencias a distintos [recursos para escribir consultas de Azure Monitor](query-language.md).
- Acceda a la [documentación de referencia completa del lenguaje de consulta de Kusto](/azure/kusto/query/).
