---
title: Prácticas recomendadas para dar forma a JSON en las consultas de Azure Time Series Insights.
description: Aprenda a mejorar la eficacia de consulta de Time Series Insights.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/24/2018
ms.author: anshan
ms.openlocfilehash: 11bea78315ff7ebb4b0c167dbb687ce940907527
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628919"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Cómo dar forma a JSON para maximizar el rendimiento de las consultas 

Este artículo proporciona orientación para dar forma a JSON para maximizar la eficacia de las consultas de Azure Time Series Insights (TSI).

## <a name="best-practices"></a>Procedimientos recomendados

Es importante que piense en cómo envía eventos a Time Series Insights. Es decir, siempre debería:

1. enviar los datos a través de la red de la forma más eficaz posible;
2. asegurarse de que los datos se almacenan de forma que le permite realizar agregaciones adecuadas para su escenario;
3. asegurarse de no alcanza los límites máximos de propiedades de TSI de
   - 600 propiedades (columnas) para entornos de S1,
   - 800 propiedades (columnas) para entornos de S2.

Las siguientes instrucciones le ayudan a garantizar el máximo rendimiento posible de consultas:

1. No use las propiedades dinámicas, como un identificador de etiqueta como nombre de propiedad, ya que esto contribuye a llegar al límite máximo de propiedades.
2. No envíe propiedades innecesarias. Si una propiedad de consulta no es necesaria, es mejor que no la envíe y evite las limitaciones de almacenamiento.
3. Use [datos de referencia](time-series-insights-add-reference-data-set.md) para evitar el envío de datos estáticos a través de la red.
4. Comparta propiedades de dimensión entre varios eventos para enviar datos a través de la red de forma más eficaz.
5. No utilice el anidamiento profundo de matriz. TSI admite hasta dos niveles de matrices anidadas que contienen objetos. TSI acopla las matrices en los mensajes, en varios eventos con pares de valor de propiedad.
6. Si solo existen algunas de las medidas para todos o la mayoría de los eventos, es mejor que envíe estas medidas como propiedades independientes dentro del mismo objeto. Al enviarlas por separado se reduce el número de eventos y podría hacer que las consultas sean más eficientes ya que se deben procesar menos eventos. Cuando existen varias medidas, al enviarlas como valores en una sola propiedad se minimiza la posibilidad de llegar al límite máximo de propiedades.

## <a name="examples"></a>Ejemplos

Los dos ejemplos siguientes muestran los eventos de envío para resaltar las recomendaciones anteriores. Después de cada ejemplo puede ver cómo se han aplicado las recomendaciones.

Los ejemplos se basan en un escenario donde varios dispositivos envían señales o medidas. Las medidas o señales podrían ser Caudal, Presión de aceite del motor, Temperatura y Humedad. En el primer ejemplo, hay algunas medidas en todos los dispositivos. En el segundo ejemplo, hay muchos dispositivos y cada uno envía muchas medidas únicas.

### <a name="scenario-only-a-few-measurementssignals-exist"></a>Escenario: solo se existen algunas medidas o señales

**Recomendación:** Envíe cada medida como una columna o propiedad independiente.

En el ejemplo siguiente, hay un único mensaje de IoT Hub, donde la matriz externa contiene una sección compartida de valores de dimensión comunes. La matriz externa usa datos de referencia para aumentar la eficacia del mensaje. Los datos de referencia contienen metadatos del dispositivo, que no cambian con cada evento, pero proporcionan propiedades útiles para el análisis de datos. Tanto los valores de dimensión común de procesamiento por lotes y el uso de datos de referencia, ahorran bytes enviados a través de la conexión, lo que hace que el mensaje sea más eficaz.

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
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

Tabla de datos de referencia (la propiedad clave es deviceId):

| deviceId | messageId | deviceLocation |
| --- | --- | --- |
| FXXX | LINE\_DATA | EU |
| FYYY | LINE\_DATA | US |

Tabla de eventos de Time Series Insights (después del acoplamiento):

| deviceId | messageId | deviceLocation | timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
| --- | --- | --- | --- | --- | --- |
| FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
| FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
| FYYY | LINE\_DATA | US | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Tenga en cuenta lo siguiente en el ejemplo anterior:

- La columna **deviceId** actúa como el encabezado de columna para los distintos dispositivos en una línea. Si se intentase establecer el valor de deviceId como su propio nombre de propiedad, se habría limitado el número total de dispositivos a 595 (entornos S1) o 795 (entornos S2), con las otras cinco columnas.

- Se evitan las propiedades innecesarias, por ejemplo, información de marca y modelo, etc. Puesto que no se consultarán en el futuro, al eliminarlas se permite que la eficacia de red y almacenamiento sea mejor.

- Los datos de referencia se utilizan para reducir el número de bytes transferidos a través de la red. Se combinan dos atributos, **messageId** y **deviceLocation**, mediante la propiedad clave, **deviceId**. Estos datos se combinan con los datos de telemetría en el momento de entrada y posteriormente se almacenan en TSI para las consultas.

- Se utilizan dos niveles de anidamiento, que es la cantidad máxima de anidamiento admitido en TSI. Es fundamental para evitar matrices profundamente anidadas.

- Las medidas se envían como propiedades independientes en el mismo objeto, dado que hay varias medidas. En este caso, **series.Flow Rate psi** y **series.Engine Oil Pressure ft3/s** son columnas únicas.

### <a name="scenario-several-measures-exist"></a>Escenario: existen varias medidas

**Recomendación:** Envíe las mediciones como tuplas de "tipo", "unidad", "valor".

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

Datos de referencia (las propiedades clave son deviceId y series.tagId):

| deviceId | series.tagId | messageId | deviceLocation | Tipo | unit |
| --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s |
| FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | psi |
| FYYY | pumpRate | LINE\_DATA | US | Caudal | ft3/s |
| FYYY | oilPressure | LINE\_DATA | US | Presión de aceite del motor | psi |

Tabla de eventos de Time Series Insights (después del acoplamiento):

| deviceId | series.tagId | messageId | deviceLocation | Tipo | unit | timestamp | series.value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 |
| FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | psi | 2018-01-17T01:17:00Z | 34.7 |
| FXXX | pumpRate | LINE\_DATA | EU | Caudal | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 |
| FXXX | oilPressure | LINE\_DATA | EU | Presión de aceite del motor | Psi | 2018-01-17T01:17:00Z | 49.2 |
| FYYY | pumpRate | LINE\_DATA | US | Caudal | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
| FYYY | oilPressure | LINE\_DATA | US | Presión de aceite del motor | psi | 2018-01-17T01:18:00Z | 22.2 |

Tenga en cuenta lo siguiente en el ejemplo anterior, al igual que en el primer ejemplo:

- Las columnas **deviceId** y **series.tagId** actúan como encabezados de columna para los distintos dispositivos y etiquetas en una línea. Al usar cada una como su propio atributo, se limitaría la consulta a 594 (entornos S1) o 794 (entornos S2) dispositivos en total, con las otras seis columnas.

- Se han evitado las propiedades innecesarias, por la razón indicada en el primer ejemplo.

- Los datos de referencia se utilizan para reducir el número de bytes transferidos a través de la red mediante la introducción de **deviceId**, para un par único de **messageId** y **deviceLocation**. Se utiliza una clave compuesta, **series.tagId**, para el par único de **tipo** y **unidad**. La clave compuesta permite usar el par **deviceId** y **series.tagId** para hacer referencia a cuatro valores: **messageId, deviceLocation, tipo** y **unidad**. Estos datos se combinan con los datos de telemetría en el momento de entrada y posteriormente se almacenan en TSI para las consultas.

- Se utilizan dos niveles de anidamiento, por la razón indicada en el primer ejemplo.

### <a name="for-both-scenarios"></a>Para ambos escenarios

Si tiene una propiedad con un gran número de valores posibles, es mejor que enviarlos como valores distintos dentro de una sola columna, en lugar de crear una nueva columna para cada valor. De los dos ejemplos anteriores:
  - En el primer ejemplo, hay algunas propiedades que tienen varios valores, por lo que es adecuado hacer que cada uno sea una propiedad independiente. 
  - Pero, en el segundo ejemplo, puede ver que las medidas no se especifican como propiedades individuales, sino como una matriz de valores y medidas en una propiedad de la serie común. Se envía una nueva clave, **tagId**, que crea una nueva columna, **series.tagId** en la tabla acoplada. Se crean nuevas propiedades, **tipo** y **unidad**, con los datos de referencia, lo que impide que se alcance el límite de propiedades.

## <a name="next-steps"></a>Pasos siguientes

Para poner estas instrucciones en práctica, consulte [Azure Time Series Insights query syntax](/rest/api/time-series-insights/time-series-insights-reference-query-syntax) (Sintaxis de consulta de Azure Time Series Insights) para obtener más información sobre la sintaxis de consulta para la API de REST de acceso a datos de TSI.