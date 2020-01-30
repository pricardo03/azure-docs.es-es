---
title: Análisis de JSON y AVRO en Azure Stream Analytics
description: En este artículo se describe cómo trabajar con tipos de datos complejos como matrices, JSON, datos formateados CSV.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: cbfa6f8b85814f0f77234e014ade0ff757a4c4b8
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720085"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Análisis de datos JSON y AVRO en Azure Stream Analytics

Azure Stream Analytics admite eventos de procesamiento en formatos de datos CSV, JSON y Avro. Tanto los datos JSON como los datos Avro se pueden estructurar y contener algunos tipos complejos, tales como objetos anidados (registros) y matrices. 




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
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```


### <a name="access-nested-fields-in-known-schema"></a>Acceso a campos anidados en un esquema conocido
Use la notación de punto (.) para acceder fácilmente a los campos anidados directamente desde la consulta. Por ejemplo, esta consulta selecciona las coordenadas de latitud y longitud de la propiedad Location en los datos JSON anteriores. La notación de punto se puede usar para navegar por varios niveles, como se muestra a continuación.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.SensorMetadata.Version
FROM input
```

### <a name="select-all-properties"></a>Selección de todas las propiedades
Puede seleccionar todas las propiedades de un registro anidado usando el comodín "*". Considere el ejemplo siguiente:

```SQL
SELECT input.Location.*
FROM input
```

El resultado es el siguiente:

```json
{
    "Lat" : 47,
    "Long" : 122
}
```


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Acceso a los campos anidados cuando el nombre de la propiedad es una variable
Use la función [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getrecordpropertyvalue-azure-stream-analytics) si el nombre de la propiedad es una variable. 

Por ejemplo, imagine que un flujo de datos de ejemplo tiene que combinarse con los datos de referencia que contienen los umbrales para cada sensor de dispositivo. A continuación se muestra un fragmento de código de tales datos de referencia.

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
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
    -- the where statement selects the property value coming from the reference data
```

### <a name="convert-record-fields-into-separate-events"></a>Conversión de campos de registro en eventos independientes
Para convertir los campos de registro en eventos independientes, use el operador [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) con la función [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics). Por ejemplo, si el ejemplo anterior tenía varios registros para SensorReading, la consulta siguiente se podría utilizar para extraerlos en eventos diferentes:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```



## <a name="array-data-types"></a>Tipos de datos de matriz

Los tipos de datos de matriz son una colección ordenada de valores. A continuación se detallan algunas operaciones típicas en valores de matriz. En estos ejemplos se supone que los eventos de entrada tienen una propiedad denominada "arrayField" que es un tipo de datos de matriz.

Estos ejemplos utilizan las funciones [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics) y el operador [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics).

### <a name="working-with-a-specific-array-element"></a>Trabajo con un elemento de matriz específico
Selección del elemento de matriz en un índice especificado (seleccionando el primer elemento de matriz):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

### <a name="select-array-length"></a>Selección de la longitud de matriz

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

### <a name="convert-array-elements-into-separate-events"></a>Conversión de los elementos de matriz en eventos independientes
Selección de todos los elemento de matriz como eventos individuales. El operador [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) junto con la función integrada [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) extrae todos los elementos de matriz como eventos individuales:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```


## <a name="see-also"></a>Consulte también
[Tipos de datos en Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
