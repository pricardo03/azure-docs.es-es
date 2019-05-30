---
title: 'Analytics: la eficaz herramienta de búsqueda y consulta de Azure Application Insights | Microsoft Docs'
description: 'Información general de Analytics, la eficaz herramienta de búsqueda de Application Insights. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/02/2019
ms.author: mbullwin
ms.openlocfilehash: f5819194e7967b5921f34223cad299752460de30
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255637"
---
# <a name="analytics-in-application-insights"></a>Analytics de Application Insights
Analytics es la herramienta eficaz de búsqueda y consulta de [Application Insights](app-insights-overview.md). Analytics es una herramienta web por lo que no requiere ninguna configuración.
Si ya ha configurado Application Insights para una de las aplicaciones, a continuación, puede analizar los datos de la aplicación abriendo Analytics desde la hoja de información general de la aplicación.

![Abra portal.azure.com, abra su recurso de Application Insights y haga clic en Análisis.](./media/analytics/001.png)

También puede usar el [sitio de prueba de Analytics](https://go.microsoft.com/fwlink/?linkid=859557), que es un entorno de demostración gratuito con una gran cantidad de datos de ejemplo.

## <a name="relation-to-azure-monitor-logs"></a>Relación con los registros de Azure Monitor
El análisis de Application Insights se basa en [Azure Data Explorer](/azure/data-explorer), como los registros de Azure Monitor, y también usa el [lenguaje de consulta de Kusto](/azure/kusto/query). Usa el mismo [portal de Log Analytics](../log-query/get-started-portal.md) que los registros de Azure Monitor, aunque sus datos se almacenan en una partición independiente.

No puede acceder directamente a los datos en un área de trabajo de Log Analytics de análisis de Application Insights, ni puede acceder directamente a los datos de la aplicación de Log Analytics. Para consultar conjuntamente ambos conjuntos de datos, escriba una [consulta en Log Analytics](../log-query/log-query-overview.md) y use la expresión de [app()](../log-query/app-expression.md) para acceder a los datos de la aplicación.


## <a name="query-data-in-analytics"></a>Consulta de datos en Analytics
Una consulta típica empieza con un nombre de tabla seguido de una serie de *operadores* separados por `|`.
Por ejemplo, vamos a averiguar cuántas solicitudes ha recibido desde diferentes países o regiones, durante las últimas tres horas nuestra aplicación:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

Empezaremos con el nombre de tabla *requests* y agregaremos elementos canalizados según sea necesario.  En primer lugar, definimos un filtro de tiempo para revisar solo los registros de las últimas tres horas.
A continuación, contamos el número de registros por país (esos datos se encuentran en la columna *client_CountryOrRegion*). Por último, representamos los resultados en un gráfico circular.
<br>

![Resultados de la consulta](./media/analytics/030.png)

El lenguaje tiene muchas características atractivas:

* [Filtre](/azure/kusto/query/whereoperator) la telemetría de la aplicación sin procesar por cualquier campo, incluidas sus métricas y propiedades personalizadas.
* [Una](/azure/kusto/query/joinoperator) varias tablas: ponga en correlación las solicitudes con vistas de página, llamadas de dependencia, excepciones y seguimiento de registros.
* [Agregaciones](/azure/kusto/query/summarizeoperator)estadísticas de gran eficacia.
* Visualizaciones inmediatas y potentes.
* [API de REST](https://dev.applicationinsights.io/) que puede usar para ejecutar consultas mediante programación, por ejemplo de Powershell.

La [referencia del lenguaje completa](https://go.microsoft.com/fwlink/?linkid=856079) detalla todos los comandos admitidos y se actualiza periódicamente.

## <a name="next-steps"></a>Pasos siguientes
* Introducción al [portal de Analytics](https://go.microsoft.com/fwlink/?linkid=856587)
* Introducción a la [escritura de consultas](https://go.microsoft.com/fwlink/?linkid=856078)
* Consulte la [hoja de referencia rápida de usuarios de SQL](https://aka.ms/sql-analytics) para ver las traducciones de las expresiones más comunes.
* Use una versión de prueba de Analytics en nuestro [sitio de prueba](https://analytics.applicationinsights.io/demo) si su aplicación aún no envía datos a Application Insights.
* Vea el [vídeo de introducción](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).
