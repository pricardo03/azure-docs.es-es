---
title: 'Descripción de los conceptos de modificación de datos en LUIS: Azure | Microsoft Docs'
description: Obtenga información sobre cómo se pueden modificar los datos antes de las predicciones en Language Understanding (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: v-geberr
ms.openlocfilehash: 4fb1a5542bb56bd853984e66198ebfbd189451f8
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266872"
---
# <a name="data-alterations"></a>Modificaciones de datos
LUIS proporciona distintos métodos para manipular la expresión antes o durante la predicción. 

## <a name="correct-spelling-errors-in-utterance"></a>Corregir errores de ortografía de una expresión
LUIS usa [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corregir los errores de ortografía de la expresión. LUIS necesita la clave asociada a ese servicio. Cree la clave y agréguela como parámetro de cadena de consulta en el [punto de conexión](https://aka.ms/luis-endpoint-apis). 

También puede corregir los errores de ortografía en el panel **Prueba** [introduciendo la clave](interactive-test.md#view-bing-spell-check-corrections-in-test-panel). La clave se conserva como variable de sesión en el explorador del panel Prueba. Agregue la clave al panel Prueba en todas las sesiones del explorador en las que quiera que se corrija la ortografía. 

El uso de la clave en el panel Prueba y en el punto de conexión cuentan para la cuota de [uso de claves](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/). LUIS implementa límites de Bing Spell Check para la longitud del texto. 

El punto de conexión requiere dos parámetros para que las correcciones ortográficas funcionen:

|Parámetro|Valor|
|--|--|
|`spellCheck`|boolean|
|`bing-spell-check-subscription-key`|Clave de suscripción de [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/)|

Cuando [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecta un error, se devuelven la expresión original y la expresión corregida junto con las predicciones del punto de conexión.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Cambiar la zona horaria de la entidad datetimeV2 creada previamente
Cuando una aplicación de LUIS usa la entidad datetimeV2 creada previamente, se puede devolver un valor de fecha y hora en la respuesta de la predicción. La zona horaria de la solicitud se usa para determinar la fecha y hora correctas que se van a devolver. Si la solicitud procede de un bot o de otra aplicación centralizada antes de llegar a LUIS, corrija la zona horaria que usa LUIS. 

### <a name="endpoint-querystring-parameter"></a>Parámetro de cadena de consulta de punto de conexión
La zona horaria se puede corregir agregando la zona horaria del usuario al [punto de conexión](https://aka.ms/luis-endpoint-apis) mediante el parámetro `timezoneOffset`. El valor de `timezoneOffset` debe ser un número positivo o negativo (en minutos) para modificar la hora.  

|Parámetro|Valor|
|--|--|
|`timezoneOffset`|número positivo o negativo (en minutos)|

### <a name="daylight-savings-example"></a>Ejemplo de horario de verano
Si necesita que la entidad datetimeV2 creada previamente que se ha devuelto se ajuste al horario de verano, debe usar el parámetro de cadena de consulta `timezoneOffset` con un valor +/- en minutos para la consulta del [punto de conexión](https://aka.ms/luis-endpoint-apis).

Agregar 60 minutos: 

https://{región}.api.cognitive.microsoft.com/luis/v2.0/apps/{IdAplicación}?q=Turn the lights on?**timezoneOffset=60**&verbose={booleano}&spellCheck={booleano}&staging={booleano}&bing-spell-check-subscription-key={cadena}&log={booleano}

Quitar 60 minutos: 

https://{región}.api.cognitive.microsoft.com/luis/v2.0/apps/{IdAplicación}?q=Turn the lights on?**timezoneOffset=-60**&verbose={booleano}&spellCheck={booleano}&staging={booleano}&bing-spell-check-subscription-key={cadena}&log={booleano}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>El código de C# determina el valor correcto de timezoneOffset
El siguiente código de C# usa el método [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) de la clase [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) para determinar el `timezoneOffset` correcto según la hora del sistema:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Corregir errores de ortografía con este tutorial](luis-tutorial-bing-spellcheck.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions