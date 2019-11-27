---
title: 'Uso compartido de vistas personalizadas con direcciones URL con parámetros: Azure Time Series Insights | Microsoft Docs'
description: Aprenda a desarrollar direcciones URL con parámetros en Azure Time Series Insights para compartir fácilmente vistas personalizadas.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/18/2019
ms.custom: seodec18
ms.openlocfilehash: 145af35f8c36d7f4659c3937209cb0d4d5b221a3
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74006370"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Uso compartido de una vista personalizada mediante una dirección URL con parámetros

Para compartir una vista personalizada en el Explorador de Time Series Insights, puede crear una dirección URL con parámetros de la vista personalizada mediante programación.

El Explorador de Time Series Insights admite parámetros de consulta de dirección URL para especificar vistas para compartir directamente desde la dirección URL. Por ejemplo, con solo la dirección URL, puede especificar un entorno de destino, un predicado de búsqueda y un intervalo de tiempo deseado. Cuando un usuario selecciona en la dirección URL personalizada, la interfaz proporciona un vínculo directo a dicho recurso en el portal de Time Series Insights. Se aplican directivas de acceso a datos.

> [!TIP]
> * Vea la [demostración gratuita de Time Series Insights](https://insights.timeseries.azure.com/samples).
> * Lea la documentación complementaria sobre el [Explorador de Time Series Insights](./time-series-insights-explorer.md).

## <a name="environment-id"></a>Identificador de entorno

El parámetro `environmentId=<guid>` especifica el identificador de entorno de destino. Se trata de un componente del FQDN de acceso a datos y se puede encontrar en la esquina superior derecha de la información general del entorno en Azure Portal. Es todo lo que va delante de `env.timeseries.azure.com`.

Un ejemplo de un parámetro de identificador de entorno es `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Hora

Puede especificar valores de tiempos absolutos o relativos con una dirección URL con parámetros.

### <a name="absolute-time-values"></a>Valores de tiempo absolutos

Con los valores de tiempo absolutos, use los parámetros `from=<integer>` y `to=<integer>`.

* `from=<integer>` es un valor de JavaScript en milisegundos de la hora de inicio del intervalo de búsqueda.
* `to=<integer>` es un valor de JavaScript en milisegundos de la hora de finalización del intervalo de búsqueda.

Para identificar los milisegundos de JavaScript para una fecha, consulte [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html) (Convertidor de marca de tiempo de Epoch y Unix).

### <a name="relative-time-values"></a>Valores de tiempo relativos

Con valores de tiempo relativos, use `relativeMillis=<value>`, donde *value* son los milisegundos de JavaScript a partir de los datos más recientes en el back-end.

Por ejemplo, `&relativeMillis=3600000` muestra los 60 minutos de datos más recientes.

Los valores aceptados corresponden al menú de **tiempo rápido** del explorador de Time Series Insights, e incluyen:

* `1800000` (Últimos 30 minutos)
* `3600000` (Últimos 60 minutos)
* `10800000` (Últimas 3 horas)
* `21600000` (Últimas 6 horas)
* `43200000` (Últimas 12 horas)
* `86400000` (Últimas 24 horas)
* `604800000` (Últimos 7 días)
* `2592000000` (Últimas 30 horas)

### <a name="optional-parameters"></a>Parámetros opcionales

El parámetro `timeSeriesDefinitions=<collection of term objects>` especifica los términos de una vista de Time Series Insights:

| Parámetro | Elemento de URL | DESCRIPCIÓN |
| --- | --- | --- |
| **name** | `\<string>` | El nombre del *término*. |
| **splitBy** | `\<string>` | El nombre de columna para *dividido por*. |
| **measureName** | `\<string>` | El nombre de columna de *medida*. |
| **predicate** | `\<string>` | La cláusula *where* para el filtrado en el lado del servidor. |
| **useSum** | `true` | Un parámetro opcional que especifica el uso de sum para la medida. </br>  Tenga en cuenta que si `Events` es la medida seleccionada, se selecciona de forma predeterminada el recuento.  </br>  Si `Events` no se selecciona, se selecciona la media de forma predeterminada. |

* El par clave-valor `multiChartStack=<true/false>` permite el apilamiento en el gráfico.
* El par clave-valor `multiChartSameScale=<true/false>` permite la misma escala del eje Y por los términos de un parámetro opcional.  
* `timeBucketUnit=<Unit>&timeBucketSize=<integer>` permite ajustar el control deslizante de intervalos para proporcionar una vista más pormenorizada y agregada del gráfico.  
* El parámetro `timezoneOffset=<integer>` permite establecer la zona horaria del gráfico que se va a ver como un desplazamiento a la hora UTC.

| Par(es) | DESCRIPCIÓN |
| --- | --- |
| `multiChartStack=false` | `true` está habilitado de forma predeterminada, así que pase `false` a la pila. |
| `multiChartStack=false&multiChartSameScale=true` | Para usar la misma escala del eje Y en todos los términos es preciso habilitar el apilamiento.  De forma predeterminada es `false`, por lo que al pasar "true" se habilita esta funcionalidad. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Unidades = días, horas, minutos, segundos y milisegundos.  La unidad siempre debe escribirse en mayúsculas. </br> Para definir el número de unidades, pase el entero que desea a timeBucketSize.  Tenga en cuenta que puede agregar hasta de 7 días.  |
| `timezoneOffset=-<integer>` | El entero siempre está en milisegundos. </br> Tenga en cuenta que esta funcionalidad no es exactamente igual que la que se habilita en el explorador de Time Series Insights, donde se le permite elegir entre local (hora del explorador) y UTC. |

### <a name="examples"></a>Ejemplos

Para agregar definiciones de series temporales a un entorno de Time Series Insights como un parámetro de URL, agregue:

```plaintext
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Use los ejemplos de definiciones de series temporales para:

* El identificador de entorno
* Los últimos 60 minutos de datos
* Los términos (F1PressureID, F2TempStation y F3VibrationPL) que componen los parámetros opcionales

Puede construir la siguiente dirección URL con parámetros para una vista:

```plaintext
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

> [!TIP]
> Vea en directo el Explorador [mediante la URL](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]).

La URL anterior describe y genera la vista del Explorador de Time Series Insights:

[![Términos del explorador de Time Series Insights](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

La vista completa (incluido el gráfico):

[![Vista de gráfico](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [consultar datos con C#](time-series-insights-query-data-csharp.md).

* Obtenga más información sobre el [Explorador de Time Series Insights](./time-series-insights-explorer.md).
