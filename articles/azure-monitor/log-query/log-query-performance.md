---
title: Escritura de consultas de registro eficaces en Azure Monitor | Microsoft Docs
description: Referencias a recursos para aprender a escribir consultas en Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: bwren
ms.openlocfilehash: 25d6b582ed4d4e24df3841f4191471296e25abd8
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436339"
---
# <a name="writing-efficient-log-queries-in-azure-monitor"></a>Escritura de consultas de registro eficaces en Azure Monitor
Este artículo ofrece recomendaciones sobre cómo escribir consultas de registro eficaces en Azure Monitor. Con estas estrategias puede garantizar que las consultas se ejecutarán rápidamente y con una mínima sobrecarga.

## <a name="scope-your-query"></a>Ámbito de la consulta
Si el proceso de consulta tiene más datos de los que realmente necesita, esto puede generar una consulta de larga duración y demasiados datos en los resultados para analizarlos de manera eficaz. En casos extremos, la consulta podría agotar el tiempo de espera y se producirá un error.

### <a name="specify-your-data-source"></a>Especificación del origen de datos
El primer paso a la hora de escribir una consulta eficaz es limitar su ámbito a los orígenes de datos necesarios. Siempre es preferible especificar una tabla en lugar de ejecutar una búsqueda de texto amplia como, por ejemplo, `search *`. Para consultar una tabla específica, inicie la consulta con el nombre de tabla del ejemplo siguiente:

``` Kusto
requests | ...
```

Puede usar [search](/azure/kusto/query/searchoperator) para buscar un valor en varias columnas de tablas concretas mediante una consulta parecida a la siguiente:

``` Kusto
search in (exceptions) "The server was not found"

search in (exceptions, customEvents) "timeout"
```

Use [union](/azure/kusto/query/unionoperator) para consultar varias tablas como en el siguiente ejemplo:

``` Kusto
union requests, traces | ...
```

### <a name="specify-a-time-range"></a>Especificar un intervalo de tiempo
También debe limitar la consulta al intervalo de tiempo de los datos que necesite. De forma predeterminada, la consulta incluirá los datos recopilados en las últimas 24 horas. Puede cambiar esa opción en el [selector de intervalos de tiempo](get-started-portal.md#select-a-time-range) o agregarlo explícitamente a la consulta. Es mejor agregar el filtro de tiempo inmediatamente después del nombre de la tabla para que el resto de la consulta solo procese los datos que están dentro de ese intervalo:

``` Kusto
requests | where timestamp > ago(1h)

requests | where timestamp between (ago(1h) .. ago(30m))
```
   
### <a name="get-only-the-latest-records"></a>Obtención solo de los registros más recientes

Para que se devuelvan solo las entradas más recientes, use el operador *top* como se muestra en la siguiente consulta que devuelve los 10 registros más recientes registrados en la tabla *traces*:

``` Kusto
traces | top 10 by timestamp
```

   
### <a name="filter-records"></a>Filtrado de registros
Para revisar solo aquellos registros que coinciden con una condición determinada, use el operador *where* como se muestra en la siguiente consulta que devuelve solo los registros en los que el valor _severityLevel_ es mayor que 0:

``` Kusto
traces | where severityLevel > 0
```



## <a name="string-comparisons"></a>Comparaciones de cadenas
Al [evaluar cadenas](/azure/kusto/query/datatypes-string-operators), debe usar `has` en lugar de `contains` cuando busque tokens completos. `has` es más eficaz, ya que no tiene que buscar subcadenas.

## <a name="returned-columns"></a>Columnas devueltas

Use [project](/azure/kusto/query/projectoperator) para restringir el conjunto de columnas que se procesan a solo aquellas que necesite:

``` Kusto
traces 
| project timestamp, severityLevel, client_City 
| ...
```

Aunque puede usar [extend](/azure/kusto/query/extendoperator) para calcular los valores y crear sus propias columnas, normalmente es más eficaz filtrar en una columna de tabla.

Por ejemplo, la primera consulta que aparece a continuación filtra por _operation\_Name_ y es más eficaz que la segunda, que crea una nueva columna _subscription_ y filtra en ella:

``` Kusto
customEvents 
| where split(operation_Name, "/")[2] == "acb"

customEvents 
| extend subscription = split(operation_Name, "/")[2] 
| where subscription == "acb"
```

## <a name="using-joins"></a>Uso de uniones
Si va a usar el operador [join](/azure/kusto/query/joinoperator), elija la tabla con menos filas para que esté en el lado izquierdo de la consulta.


## <a name="next-steps"></a>Pasos siguientes
Para más información, consulte [Procedimientos recomendados sobre las consultas](/azure/kusto/query/best-practices).