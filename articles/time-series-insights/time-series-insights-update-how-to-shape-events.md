---
title: 'Dar forma a eventos: Azure Time Series Insights | Microsoft Docs'
description: Obtenga información sobre los procedimientos recomendados y cómo dar forma a los eventos para realizar consultas en la versión preliminar de Azure Time Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/14/2020
ms.custom: seodec18
ms.openlocfilehash: e814d9be4a0db2852bd9e21f3d3c1d54a45bd268
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2020
ms.locfileid: "77368647"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Dar forma a los eventos con la versión preliminar de Azure Time Series Insights

En este artículo se proporciona información para dar forma a su archivo JSON para la ingesta y para maximizar la eficacia de las consultas de la versión preliminar de Azure Time Series Insights.

## <a name="best-practices"></a>Procedimientos recomendados

Piense en cómo envía eventos a la versión preliminar de Time Series Insights. Es decir, siempre debería:

* Enviar los datos a través de la red de la forma más eficaz posible.
* Almacenar los datos de una manera que le ayude a agregarlos de manera más adecuada para su escenario.

Para garantizar el máximo rendimiento de las consultas, haga lo siguiente:

* No envíe propiedades innecesarias. La versión preliminar de Time Series Insights le facturará en función del uso. Es mejor almacenar y procesar los datos que vaya a consultar.
* Use campos de instancia para los datos estáticos. Este procedimiento le ayudará a evitar el envío de datos estáticos a través de la red. Los campos de instancia, un componente del modelo de Time Series Insights, funcionan como datos de referencia en el servicio Time Series Insights, que tiene disponibilidad general. Para más información sobre los campos de instancia, lea [Modelo de Time Series](./time-series-insights-update-tsm.md).
* Comparta propiedades de dimensión entre dos o más eventos. Este procedimiento le ayudará a enviar datos a través de la red de manera más eficaz.
* No utilice el anidamiento profundo de matriz. La versión preliminar de Time Series Insights admite hasta dos niveles de matrices anidadas que contienen objetos. Igualmente, la versión preliminar de Time Series Insights acopla las matrices en mensajes de varios eventos con pares de valores de propiedad.
* Si solo existen algunas de las medidas para todos o la mayoría de los eventos, es mejor que envíe estas medidas como propiedades independientes dentro del mismo objeto. Al enviarlas por separado se reduce el número de eventos y podría mejorar el rendimiento de consultas, ya que se deben procesar menos eventos.

Durante la ingesta, las cargas útiles que contienen anidamiento se aplanarán, de modo que el nombre de la columna sea un valor único con un delineador. Time Series Insights (versión preliminar) usa caracteres de subrayado para la delineación. Tenga en cuenta que se trata de un cambio respecto a la versión de disponibilidad general del producto que usaba puntos. Durante la versión preliminar, hay una advertencia sobre el aplanamiento, que se muestra en el segundo ejemplo a continuación.

## <a name="examples"></a>Ejemplos

El ejemplo siguiente se basa en un escenario donde dos o más dispositivos envían señales o medidas. Las medidas o señales podrían ser *Caudal*, *Presión de aceite del motor*, *Temperatura* y *Humedad*.

En el ejemplo, hay un único mensaje de Azure IoT Hub donde la matriz externa contiene una sección compartida de valores de dimensión comunes. La matriz externa usa datos de instancia de serie temporal para aumentar la eficacia del mensaje. 

La instancia de Time Series contiene los metadatos del dispositivo. Estos metadatos no cambian con cada evento, pero que proporcionan propiedades útiles para el análisis de datos. Para ahorrar en bytes enviados a través de la conexión y hacer que el mensaje sea más eficaz, considere la posibilidad de agrupar valores de dimensiones comunes y usar metadatos de instancia de serie temporal.

### <a name="example-1"></a>Ejemplo 1:

```JSON
[
  {
    "deviceId":"FXXX",
    "timestamp":"2018-01-17T01:17:00Z",
    "series":[
      {
        "Flow Rate ft3/s":1.0172575712203979,
        "Engine Oil Pressure psi ":34.7
      },
      {
        "Flow Rate ft3/s":2.445906400680542,
        "Engine Oil Pressure psi ":49.2
      }
    ]
  },
  {
    "deviceId":"FYYY",
    "timestamp":"2018-01-17T01:18:00Z",
    "series":[
      {
        "Flow Rate ft3/s":0.58015072345733643,
        "Engine Oil Pressure psi ":22.2
      }
    ]
  }
]
```

### <a name="time-series-instance"></a>Instancia de serie temporal 

> [!NOTE]
> El id. de serie temporal es *deviceId*.

```JSON
[
  {
    "timeSeriesId":[
      "FXXX"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"REVOLT SIMULATOR",
      "L2":"Battery System"
    }
  },
  {
    "timeSeriesId":[
      "FYYY"
    ],
    "typeId":"17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds":[
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description":null,
    "instanceFields":{
      "L1":"COMMON SIMULATOR",
      "L2":"Battery System"
    }
  }
]
```

La versión preliminar de Time Series Insights une una tabla (después del acoplamiento) durante el tiempo de consulta. La tabla incluye columnas adicionales, como **Type**. En el ejemplo siguiente se demuestra cómo puede [dar forma](./time-series-insights-send-events.md#supported-json-shapes) a los datos de telemetría.

| deviceId  | Tipo | L1 | L2 | timestamp | series_Flow Rate ft3/s | series_Engine Oil Pressure psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULADOR | Sistema de batería | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34.7 |
| `FXXX` | Default_Type | SIMULADOR |   Sistema de batería |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA    COMMON | SIMULADOR |    Sistema de batería |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

En el ejemplo anterior, tenga en cuenta lo siguiente:

* Las propiedades estáticas se almacenan en la versión preliminar de Time Series Insights para optimizar los datos enviados a través de la red.
* Los datos de la versión preliminar de Time Series Insights se combinan cuando se realiza una consulta a través del identificador de Time Series definido en la instancia.
* Se utilizan dos niveles de anidamiento. Este número es el más compatible con la versión preliminar de Time Series Insights. Es fundamental para evitar matrices profundamente anidadas.
* Como hay pocas medidas, estas se envían como propiedades independientes en el mismo objeto. En este ejemplo, **series_Flow Rate psi**, **series_Engine Oil Pressure psi** y **series_Flow Rate ft3/s** son columnas únicas.

>[!IMPORTANT]
> Los campos de instancia no se almacenan con telemetría. Se almacenan con metadatos en el modelo de Time Series.
> La tabla anterior representa la vista de consulta.

### <a name="example-2"></a>Ejemplo 2:

Considere el siguiente JSON:

```JSON
{
  "deviceId": "FXXX",
  "timestamp": "2019-01-18T01:17:00Z",
  "data": {
        "flow": 1.0172575712203979,
    },
  "data_flow" : 1.76435072345733643
}
```
En el ejemplo anterior, la propiedad de `data_flow` aplanada presentaría una colisión de nombres con la propiedad `data_flow`. En este caso, el valor de la propiedad *latest* sobrescribiría a la anterior. Si este comportamiento presenta un desafío para sus escenarios empresariales, póngase en contacto con el equipo de Time Series Insights.

> [!WARNING] 
> En los casos en los que las propiedades duplicadas están presentes en la misma carga del evento debido al acoplamiento o a otro mecanismo, se almacena el valor de propiedad más reciente, sobrescribiendo todos los valores anteriores.


## <a name="next-steps"></a>Pasos siguientes

Para poner en práctica estas pautas, consulte [Sintaxis de consultas de la versión preliminar de Azure Time Series Insights](./time-series-insights-query-data-csharp.md). Obtendrá más información sobre la sintaxis de consultas para la API REST de acceso a datos de la versión preliminar de Time Series Insights.
