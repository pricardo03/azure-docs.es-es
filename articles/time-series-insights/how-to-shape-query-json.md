---
title: Procedimientos recomendados para dar forma a JSON en las consultas de Azure Time Series Insights | Microsoft Docs
description: Aprenda a mejorar la eficacia de consulta de Azure Time Series Insights.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 089285637bb740fea47f1fd07de0906dfe46662b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244455"
---
# <a name="shape-json-to-maximize-query-performance"></a>Forma de JSON para maximizar el rendimiento de las consultas 

Este artículo proporcionan instrucciones sobre la forma de JSON para maximizar la eficacia de las consultas de Azure Time Series Insights.

## <a name="video"></a>Vídeo

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Obtenga información sobre los procedimientos recomendados para dar forma a JSON para satisfacer las necesidades de almacenamiento.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Procedimientos recomendados
Piense en cómo enviar eventos a Time Series Insights. Es decir, que siempre:

1. Enviar los datos a través de la red de la forma más eficaz posible.
1. Asegúrese de que los datos se almacenan en una forma para que pueda realizar agregaciones adecuadas para su escenario.
1. Asegúrese de que no llegan a los límites de la propiedad maximum de Time Series Insights de:
   - 600 propiedades (columnas) para entornos de S1,
   - 800 propiedades (columnas) para entornos de S2.

Las siguientes instrucciones le ayuda a garantizar el máximo rendimiento posible de la consulta:

1. No use las propiedades dinámicas, como un identificador de etiqueta, como un nombre de propiedad. Este uso contribuye a alcanzar el límite máximo de las propiedades.
1. No envíe propiedades innecesarias. Si una propiedad de consulta no es necesaria, es mejor no enviarlo. De esta forma de que evitar las limitaciones de almacenamiento.
1. Use [hacen referencia a datos](time-series-insights-add-reference-data-set.md) para evitar el envío de datos estáticos a través de la red.
1. Compartir propiedades de dimensión entre varios eventos para enviar datos a través de la red de forma más eficaz.
1. No utilice el anidamiento profundo de matriz. Time Series Insights admite hasta dos niveles de matrices anidadas que contienen objetos. Time Series Insights reduce las matrices en los mensajes en varios eventos con pares de valor de propiedad.
1. Si solo existen algunas de las medidas para todos o la mayoría de los eventos, es mejor que envíe estas medidas como propiedades independientes dentro del mismo objeto. Enviarlos por separado, reduce el número de eventos y pueden mejorar el rendimiento de las consultas porque menos eventos deben procesarse. Cuando hay varias medidas, enviarlos como valores en una sola propiedad minimiza la posibilidad de alcanzar el límite máximo de propiedad.

## <a name="example-overview"></a>Información general de ejemplo

Los dos ejemplos siguientes muestran cómo enviar eventos para resaltar las recomendaciones anteriores. Después de cada ejemplo, puede ver cómo se aplican las recomendaciones.

Los ejemplos se basan en un escenario donde varios dispositivos envían señales o medidas. Pueden ser señales o medidas de flujo de velocidad, presión del aceite del motor, temperatura y humedad. En el primer ejemplo, hay algunas medidas en todos los dispositivos. El segundo ejemplo tiene muchos dispositivos y los dispositivos envían muchas medidas únicas.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Escenario uno: Existen solo algunas medidas

> [!TIP]
> Se recomienda enviar cada medida o una señal como una propiedad independiente o una columna.

En el ejemplo siguiente, hay un único mensaje IoT Hub de Azure donde la matriz externa contiene una sección compartida común de valores de dimensión. La matriz externa usa datos de referencia para aumentar la eficacia del mensaje. Datos de referencia contienen los metadatos de dispositivo que no cambian con cada evento, sino que proporciona propiedades útiles para análisis de datos. Procesamiento por lotes de los valores de dimensión común y emplear guarda datos en bytes enviados a través del cable de referencia, que permite que el mensaje más eficaz.

Carga JSON de ejemplo:

```json
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
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

* Tabla de datos de referencia que tiene la propiedad de clave **deviceId**:

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | LINE\_DATA | EU |
   | FYYY | LINE\_DATA | US |

* Tabla de eventos de Series Insights de la hora después de hacerlo:

   | deviceId | messageId | deviceLocation |  timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | LINE\_DATA | US | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Notas sobre estas dos tablas:

- La columna **deviceId** actúa como el encabezado de columna para los distintos dispositivos en una línea. Hace que el valor de deviceId su propio nombre de la propiedad limita el total de dispositivos para 595 (para S1 entornos) o 795 (para entornos de S2) con las otras cinco columnas.
- Se evitan propiedades innecesarias, para obtener información de ejemplo, la marca y modelo. Dado que las propiedades no se puede consultar en el futuro, eliminándolos permite red mejor y la eficacia de almacenamiento.
- Los datos de referencia se utilizan para reducir el número de bytes transferidos a través de la red. Los dos atributos **messageId** y **deviceLocation** se unen usando la propiedad de clave **deviceId**. Estos datos se combina con los datos de telemetría en tiempo de entrada y, a continuación, se almacenan en Time Series Insights para realizar consultas.
- Se utilizan dos niveles de anidamiento, que es la cantidad máxima de anidamiento compatible con Time Series Insights. Es fundamental para evitar matrices profundamente anidadas.
- Las medidas se envían como propiedades independientes dentro del mismo objeto, porque hay algunas medidas. En este caso, **series.Flow Rate psi** y **series.Engine Oil Pressure ft3/s** son columnas únicas.

## <a name="scenario-two-several-measures-exist"></a>Escenario dos: Existen varias medidas

> [!TIP]
> Se recomienda que envíe las mediciones como un "tipo", "unidad" y "value" tuplas.

Carga JSON de ejemplo:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Tabla de datos de referencia que tiene las propiedades de clave **deviceId** y **series.tagId**:

   | deviceId | series.tagId | messageId | deviceLocation | Tipo | unit |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s |
   | FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | psi |
   | FYYY | pumpRate | LINE\_DATA | US | Caudal | ft3/s |
   | FYYY | oilPressure | LINE\_DATA | US | Presión de aceite del motor | psi |

* Tabla de eventos de Series Insights de la hora después de hacerlo:

   | deviceId | series.tagId | messageId | deviceLocation | Tipo | unit |  timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | LINE\_DATA | US | Caudal | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | LINE\_DATA | US | Presión de aceite del motor | psi | 2018-01-17T01:18:00Z | 22.2 |

Notas sobre estas dos tablas:

- Las columnas **deviceId** y **series.tagId** actuar como los encabezados de columna para los distintos dispositivos y las etiquetas de una flota. Con cada uno como su propio atributo limita la consulta a 594 (para S1 entornos) o 794 (para entornos de S2) número total de dispositivos con las seis columnas.
- se han evitado propiedades innecesarias, por la razón citada en el primer ejemplo.
- Datos de referencia se usan para reducir el número de bytes transferidos a través de la red mediante la introducción de **deviceId**, que se usa para el par único de **messageId** y **deviceLocation**. La clave compuesta **series.tagId** se usa para el par único de **tipo** y **unidad**. Permite que la clave compuesta la **deviceId** y **series.tagId** par que se usará para hacer referencia a cuatro valores: **messageId, deviceLocation, escriba,** y **unidad**. Estos datos se combina con los datos de telemetría en tiempo de entrada. A continuación, se almacena en Time Series Insights para realizar consultas.
- se usan dos niveles de anidamiento, por la razón citada en el primer ejemplo.

### <a name="for-both-scenarios"></a>Para ambos escenarios

Para una propiedad con un gran número de valores posibles, es mejor enviar como valores distintos dentro de una sola columna en lugar de crear una nueva columna para cada valor. De los dos ejemplos anteriores:

  - En el primer ejemplo, algunas propiedades tienen varios valores, por lo que es adecuada para que cada uno una propiedad independiente.
  - En el segundo ejemplo, las medidas no se especifican como propiedades individuales. En su lugar, son una matriz de valores o medidas en una propiedad común de la serie. La nueva clave **tagId** se envía, lo que crea la nueva columna **series.tagId** en la tabla plana. Las nuevas propiedades **tipo** y **unidad** se crean mediante el uso de datos de referencia para que no se alcance el límite de la propiedad.

## <a name="next-steps"></a>Pasos siguientes

- Lectura [sintaxis de consulta de Azure Time Series Insights](/rest/api/time-series-insights/ga-query-syntax) para obtener más información sobre la sintaxis de consulta para la API de REST de acceso a los datos de Time Series Insights.
- Obtenga información sobre [cómo los eventos de forma](./time-series-insights-send-events.md).
