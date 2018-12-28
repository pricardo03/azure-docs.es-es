---
title: 'Opción para dar forma a los eventos de la versión preliminar de Azure Time Series Insights: dar forma a los eventos con la versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Obtenga información acerca de cómo dar forma a los eventos en la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: b0ce3b5bbd58c594147f64de9f06f248f4a87fc3
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269080"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Dar forma a los eventos con la versión preliminar de Azure Time Series Insights

En este artículo se proporciona información para dar forma a su archivo JSON para así maximizar la eficacia de las consultas de la versión preliminar de Azure Time Series Insights.

## <a name="best-practices"></a>Procedimientos recomendados

Es importante que piense en cómo envía eventos a la versión preliminar de Time Series Insights. Es decir, siempre debería:

* Enviar los datos a través de la red de la forma más eficaz posible.
* Almacenar los datos de una manera que le ayude a agregarlos de manera más adecuada para su escenario.

Para garantizar el máximo rendimiento posible de las consultas, haga lo siguiente:

* No envíe propiedades innecesarias. La versión preliminar de Time Series Insights le facturará en función del uso. Es mejor almacenar y procesar los datos que vaya a consultar.
* Use campos de instancia para los datos estáticos. Este procedimiento le ayudará a evitar el envío de datos estáticos a través de la red. Los campos de instancia, que son un componente del modelo de serie temporal, funcionan como datos de referencia en el servicio GA de Time Series Insights. Para obtener más información sobre los campos de instancia, consulte los [modelos de serie temporal](./time-series-insights-update-tsm.md).
* Comparta propiedades de dimensión entre dos o más eventos. Este procedimiento le ayudará a enviar datos a través de la red de manera más eficaz.
* No utilice el anidamiento profundo de matriz. La versión preliminar de Time Series Insights admite hasta dos niveles de matrices anidadas que contengan objetos. Igualmente, la versión preliminar de Time Series Insights acopla las matrices en mensajes de varios eventos con pares de valores de propiedad.
* Si solo existen algunas de las medidas para todos o la mayoría de los eventos, es mejor que envíe estas medidas como propiedades independientes dentro del mismo objeto. Al enviarlas por separado se reduce el número de eventos y, como hay que procesar menos eventos, las consultas son más eficaces.

## <a name="example"></a>Ejemplo

El ejemplo siguiente se basa en un escenario donde dos o más dispositivos envían señales o medidas. Las medidas o señales podrían ser *Caudal*, *Presión de aceite del motor*, *Temperatura* y *Humedad*.

En el ejemplo siguiente, hay un único mensaje de IoT Hub donde la matriz externa contiene una sección compartida de valores de dimensión comunes. La matriz externa usa datos de instancia de serie temporal para aumentar la eficacia del mensaje. La instancia de serie temporal contiene metadatos del dispositivo, que no cambian con cada evento, pero que proporcionan propiedades útiles para el análisis de datos. Para ahorrar en bytes enviados a través de la conexión y hacer que el mensaje sea más eficaz, considere la posibilidad de agrupar valores de dimensiones comunes y emplear metadatos de instancia de serie temporal.

### <a name="example-json-payload"></a>Carga JSON de ejemplo

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

### <a name="time-series-instance"></a>Instancia de serie temporal 
> [!NOTE]
> El id. de serie temporal es *deviceId*.

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

La versión preliminar de Time Series Insights une una tabla (después del acoplamiento) durante el tiempo de consulta. La tabla incluye columnas adicionales, como **Type**. En el ejemplo siguiente se demuestra cómo puede dar [forma](./time-series-insights-send-events.md#json) a los datos de telemetría:

| deviceId  | Escriba | L1 | L2 |  timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULADOR REVOLT | Sistema de batería | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34.7 |
| `FXXX` | LINE_DATA REVOLT | SIMULADOR |    Sistema de batería |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA    COMMON | SIMULADOR |    Sistema de batería |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

En el ejemplo anterior, tenga en cuenta lo siguiente:

* Las propiedades estáticas se almacenan en la versión preliminar de Time Series Insights para optimizar los datos enviados a través de la red.
* Los datos de la versión preliminar de Time Series Insights se combinan cuando se realiza una consulta mediante el valor de *timeSeriesId* que está definido en la instancia.
* Se usan dos capas de anidamiento, que es la opción más compatible con la versión preliminar de Time Series Insights. Es fundamental para evitar matrices profundamente anidadas.
* Como hay pocas medidas, estas se envían como propiedades independientes en el mismo objeto. En este ejemplo, **series.Flow Rate psi**, **series.Engine Oil Pressure psi** y **series.Flow Rate ft3/s** son columnas únicas.

>[!IMPORTANT]
> Los campos de instancia no se almacenan con telemetría. Se almacenan con metadatos en el **modelo de serie temporal**.
> La tabla anterior representa la vista de consulta.

## <a name="next-steps"></a>Pasos siguientes

Para poner en práctica estas pautas, consulte [Azure Time Series Insights Preview query syntax](./time-series-insights-query-data-csharp.md) (Sintaxis de consultas de la versión preliminar de Azure Time Series Insights). Obtendrá más información sobre la sintaxis de consultas para la API REST de acceso a datos de la versión preliminar de Time Series Insights.

Para obtener información sobre las formas JSON admitidas, consulte [Supported JSON shapes](./time-series-insights-send-events.md#json) (Formas JSON admitidas).
