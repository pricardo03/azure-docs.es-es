---
title: Análisis de JSON y AVRO en Azure Stream Analytics
description: En este artículo se describe cómo trabajar con tipos de datos complejos como matrices, JSON, datos formateados CSV.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ad30d363c8e3ea0264ba79db5417e572614a6739
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496837"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Análisis de datos JSON y AVRO en Azure Stream Analytics

Azure Stream Analytics admite eventos de procesamiento en formatos de datos CSV, JSON y Avro. Tanto los datos JSON como los datos Avro pueden contener tipos complejos como objetos anidados (registros) y matrices.

## <a name="array-data-types"></a>Tipos de datos de matriz

Los tipos de datos de matriz son una colección ordenada de valores. A continuación se detallan algunas operaciones típicas en valores de matriz. En estos ejemplos se supone que los eventos de entrada tienen una propiedad denominada "arrayField" que es un tipo de datos de matriz.

Estos ejemplos utilizan las funciones [GetArrayElement](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelement-azure-stream-analytics), [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics), [GetArrayLength](https://msdn.microsoft.com/azure/stream-analytics/reference/getarraylength-azure-stream-analytics) y el operador [APPLY](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics).

## <a name="examples"></a>Ejemplos
Selección del elemento de matriz en un índice especificado (seleccionando el primer elemento de matriz):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

Selección de la longitud de matriz:

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

Selección de todos los elemento de matriz como eventos individuales. El operador [APPLY](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) junto con la función integrada [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics) extrae todos los elementos de matriz como eventos individuales:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```

## <a name="record-data-types"></a>Tipos de datos de registro
Los tipos de datos de registro se utilizan para representar las matrices JSON y Avro cuando se usan los formatos correspondientes en los flujos de datos de entrada. Estos ejemplos muestran un sensor de ejemplo, que lee los eventos de entrada en formato JSON. Este es el ejemplo de un solo evento:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99
    }
}
```

## <a name="examples"></a>Ejemplos
Utilice la notación de puntos (.) para acceder a los campos anidados. Por ejemplo, esta consulta selecciona las coordenadas de latitud y longitud en la propiedad de ubicación de los datos JSON anteriores:

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long
FROM input
```

Use la función [GetRecordPropertyValue](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordpropertyvalue-azure-stream-analytics) si se desconoce el nombre de la propiedad. Por ejemplo, imagine que un flujo de datos de ejemplo tiene que combinarse con los datos de referencia que contienen los umbrales para cada sensor de dispositivo:

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input
JOIN thresholds
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

Para convertir los campos de registro en eventos independientes, use el operador [APPLY](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) con la función [GetRecordProperties](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordproperties-azure-stream-analytics). Por ejemplo, para convertir una secuencia de ejemplo en un flujo de eventos con lecturas de sensor individuales, se puede utilizar esta consulta:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

Puede seleccionar todas las propiedades de un registro anidado usando el comodín "*". Considere el ejemplo siguiente:

```SQL
SELECT input.SensorReadings.*
FROM input
```

El resultado es el siguiente:

```json
{
    "Temperature" : 80,
    "Humidity" : 70,
    "CustomSensor01" : 5,
    "CustomSensor022" : 99
}
```

## <a name="see-also"></a>Vea también
[Tipos de datos en Azure Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/data-types-azure-stream-analytics)
