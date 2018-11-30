---
title: 'Referencia de datetimeV2 de entidades de LUIS precompiladas: Azure | Microsoft Docs'
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada datetimeV2 en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: bd28981ae0c5b4d6ccff3168f92f0f99be768b10
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335783"
---
# <a name="datetimev2-entity"></a>Entidad DatetimeV2

La entidad precompilada **datetimeV2** extrae los valores de fecha y hora. Estos valores se resuelven en un formato estandarizado para que los programas de cliente los consuman. Cuando una expresión tiene una fecha u hora que no está completa, LUIS incluye _los valores pasados y futuros_ en la respuesta de punto de conexión. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad datetimeV2 para las intenciones de la aplicación. 

## <a name="types-of-datetimev2"></a>Tipos de datetimeV2
DatetimeV2 se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml).

## <a name="example-json"></a>JSON de ejemplo 
La siguiente respuesta de JSON de ejemplo tiene una entidad `datetimeV2` con un subtipo de `datetime`. Para obtener ejemplos de otros tipos de entidades datetimeV2, consulte [Subtypes of datetimeV2](#subtypes-of-datetimev2)</a> (Subtipos de datetimeV2).

```JSON
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 15,
    "endIndex": 30,
    "resolution": {
      "values": [
        {
          "timex": "2017-05-02T08",
          "type": "datetime",
          "value": "2017-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>Descripciones de la propiedad JSON

|Nombre de propiedad |Tipo de propiedad y descripción|
|---|---|
|Entidad|**cadena**: texto extraído de la expresión con el tipo de fecha, hora, intervalo de fechas o el intervalo de tiempo.|
|Tipo|**cadena**: uno de los [subtipos de datetimeV2](#subtypes-of-datetimev2)
|startIndex|**entero**: índice en la expresión en la que comienza la entidad.|
|endIndex|**entero**: índice en la expresión en la que termina la entidad.|
|resolución|Tiene una matriz `values` con uno, dos o cuatro [valores de resolución](#values-of-resolution).|
|end|El valor final de un intervalo de tiempo o fechas, en el mismo formato que `value`. Solo se utiliza si `type` es `daterange`, `timerange` o `datetimerange`.|

## <a name="subtypes-of-datetimev2"></a>Subtipos de datetimeV2

La entidad precompilada **datetimeV2** tiene los siguientes subtipos. Se proporcionan ejemplos de cada uno en la siguiente tabla:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Valores de resolución
* La matriz tiene un elemento si la fecha u hora de la expresión se especifica completamente y no es ambigua.
* Si el valor de datetimeV2 es ambiguo, la matriz tiene dos elementos. La ambigüedad existe cuando falta un año, una hora o un intervalo de tiempo concretos. Consulte la sección [Fechas ambiguas](#ambiguous-dates) para obtener ejemplos. Cuando la hora es ambigua para a. m. o p. m., se incluyen ambos valores.
* Si la expresión tiene dos elementos con ambigüedad, la matriz tiene cuatro elementos. Esta ambigüedad incluye elementos que tienen:
  * Una fecha o un intervalo de fechas que es ambiguo en cuanto al año
  * Una hora o intervalo de tiempo que es ambiguo en cuanto a a. m. o p. m. Por ejemplo, 3:00 3 de abril.

Cada elemento de la matriz `values` puede tener los siguientes campos: 

|Nombre de propiedad|Descripción de la propiedad|
|--|--|
|timex|Hora, fecha o intervalo de fechas que se expresa en formato TIMEX según el [estándar ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) y los atributos de TIMEX3 para la anotación con el idioma de TimeML. Esta anotación se describe en las [instrucciones de TIMEX](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|Tipo|Subtipo, que puede ser uno de los siguientes elementos: datetime, date, time, daterange, timerange, datetimerange, duration, set.|
|value|**Opcional.** Objeto datetime con el formato aaaa:MM:dd (fecha), HH:mm:ss (hora) o aaaa:MM:dd HH:mm:ss (fecha y hora). Si `type` es `duration`, el valor es el número de segundos (duración) <br/> Solo se utiliza si `type` es `datetime` o `date`, `time` o "duración".|

## <a name="valid-date-values"></a>Valores de fecha válidos

La entidad **datetimeV2** admite fechas entre los siguientes intervalos:

| Min | max |
|----------|-------------|
| 1 de enero de 1900   | 31 de diciembre de 2099 |

## <a name="ambiguous-dates"></a>Fechas ambiguas

Si la fecha puede estar en el pasado o futuro, LUIS proporciona ambos valores. Este sería el caso de una expresión que incluyera el mes y la fecha sin el año.  

Por ejemplo, dada la expresión "May 2nd":
* Si hoy es 3 de mayo de 2017, LUIS proporciona "2017-05-02" y "2018-05-02" como valores. 
* Si hoy es 1 de mayo de 2017, LUIS proporciona "2016-05-02" y "2017-05-02" como valores.

En el siguiente ejemplo, se muestra la resolución de la entidad "may 2nd". En esta resolución, se da por supuesto que la fecha de hoy es una fecha comprendida entre el 2 de mayo de 2017 y el 1 de mayo de 2018.
Los campos con `X` en el campo `timex` forman parte de la fecha i no se especifican explícitamente en la expresión.

```JSON
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2017-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2018-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Ejemplos de resolución de intervalo de fechas para una fecha numérica

La entidad `datetimeV2` extrae los intervalos de fecha y hora. Los campos `start` y `end` especifican el principio y al final del intervalo. Para la expresión "May 2nd to May 5th", LUIS proporciona valores **daterange** valores para el año actual y el próximo año. En el campo `timex`, los valores `XXXX` indican la ambigüedad del año. `P3D` indica que el período de tiempo es de tres días de duración.

```JSON
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2017-05-02",
            "end": "2017-05-05"
          },
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2018-05-02",
            "end": "2018-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Ejemplos de resolución de intervalo de fechas para un día de la semana

En el siguiente ejemplo, se muestra cómo LUIS utiliza la entidad **datetimeV2** para resolver la expresión "Tuesday to Thursday" ("martes a jueves"). En este ejemplo, se supone que la fecha actual es el 19 de junio. LUIS incluye valores **daterange** para los dos intervalos de fechas que preceden y siguen la fecha actual.

```JSON
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-13",
            "end": "2017-06-15"
          },
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-20",
            "end": "2017-06-22"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Hora ambigua
La matriz de valores tiene dos elementos de tiempo si la hora o el intervalo de tiempo son ambiguos. Cuando hay una hora ambigua, los valores tienen tanto las horas a.m. cono p.m. .

## <a name="time-range-resolution-example"></a>Ejemplo de resolución de intervalo de tiempo

En el siguiente ejemplo, se muestra cómo LUIS utiliza la entidad **datetimeV2** para resolver la expresión que tiene el intervalo de tiempo.

```
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

## <a name="deprecated-prebuilt-datetime"></a>Intervalo de tiempo precompilado en desuso

La entidad precompilada `datetime` está en desuso y se reemplaza por  **datetimeV2**. 

Para reemplazar `datetime` por `datetimeV2` en la aplicación de LUIS, complete los pasos siguientes:

1. Abra el panel **Entities** (Entidades) de la interfaz web de LUIS. 
2. Elimine la entidad precompilada **datetime**.
3. Haga clic en **Add prebuilt entity** (Agregar entidad precompilada)
4. Seleccione **datetimeV2** y haga clic en **Save** (Guardar).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [dimensión](luis-reference-prebuilt-dimension.md), [correo electrónico](luis-reference-prebuilt-email.md) y [número](luis-reference-prebuilt-number.md). 

