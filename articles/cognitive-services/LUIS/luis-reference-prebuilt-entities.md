---
title: 'Entidades pregeneradas: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene listas de las entidades precompiladas que se incluyen en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 6e38718f14fa5c7cb8455d6c6e7bd5421d70b553
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034492"
---
# <a name="entities-per-culture"></a>Entidades por referencia cultural

Language Understanding (LUIS) proporciona entidades precompiladas. Cuando una entidad precompilada se incluye en la aplicación, LUIS incluye la predicción de entidad correspondiente en la respuesta del punto de conexión. Todas las expresiones de ejemplo también se etiquetan con la entidad. El comportamiento de las entidades predeterminadas **no puede** modificarse. A menos que se indique lo contrario, las entidades precompiladas están disponibles en todas las configuraciones locales de aplicación LUIS (referencias culturales). En la siguiente tabla se muestran las entidades precompiladas que se admiten para cada referencia cultural.

Entidad precompilada   |   Inglés (Estados Unidos)<br>```En-us```   |   Francés (Francia)<br>```fr-FR```   |   Italiano (Italia)<br>```it-IT```   |   Español (España)<br>```es-ES```   |   Chino<br>```zh-CN```   |   Alemán<br>```de-DE```   |   Portugués (Brasil)<br>```pt-BR```   |   Japonés (Japón)<br>```ja-JP```   |   Coreano (Corea)<br>```ko-kr```   | Francés (Canadá)<br>```fr-CA```   |   Español (México)<br>```es-MX```   |   Neerlandés (Países Bajos)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Edad](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Moneda](luis-reference-prebuilt-currency.md):<br>dólar<br>unidad fraccional (por ejemplo: penique)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>fecha<br>daterange<br>Twitter en tiempo<br>intervalo de tiempo   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Dimensión](luis-reference-prebuilt-dimension.md):<br>volumen<br>área<br>peso<br>información (p. ej.: bits/bytes)<br>longitud (p. ej.: medidor)<br>velocidad (p. ej.: millas por hora)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Correo electrónico](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Number](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Porcentaje](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   |    -   |    -   |    -   |    ✔   |    -   |    -   |    -   |   -   |   -   |   -   |   -   |
[Número de teléfono](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Temperatura](luis-reference-prebuilt-temperature.md):<br>grados Fahrenheit<br>grados Kelvin<br>grados Rankine<br>grados Delisle<br>grados centígrados   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Vea las notas en [Deprecated prebuilt entities](luis-reference-prebuilt-deprecated.md) (Entidades precompiladas en desuso).

KeyPhrase no está disponible en todas las referencias culturales secundarias de portugués (Brasil): ```pt-BR```.

<!--
## Examples of prebuilt entities in en-us culture
The following table lists prebuilt entities with example data and the return values.

Prebuilt entity   |   Example utterance   |   JSON
------|------|------|
 ```builtin.age```   |   ```100 year old```   |```{ "type": "builtin.age", "entity": "100 year old" }```|  
 ```builtin.age```   |   ```19 years old```   |```{ "type": "builtin.age", "entity": "19 years old" }```|
 ```builtin.currency```     |   ```1000.00 US dollars```   |```{ "type": "builtin.currency", "entity": "1000.00 us dollars" }```
 ```builtin.currency```     |   ```$ 67.5 B```   |```{ "type": "builtin.currency", "entity": "$ 67.5" }```|
 ```builtin.datetimeV2``` | See [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) | See [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) |
 ```builtin.dimension```     |   ```2 miles```   |```{ "type": "builtin.dimension", "entity": "2 miles" }```|
 ```builtin.dimension```     |  ```650 square kilometers```   |```{ "type": "builtin.dimension", "entity": "650 square kilometers" }```|
 ```builtin.email```     |  ```patti.owens@microsoft.com```   |```{ "type": "builtin.email", "entity": "patti.owens@microsoft.com" }```|
 ```builtin.number```     |  ```ten```   |``` { "type": "builtin.number", "entity": "ten" } ```|
 ```builtin.number```     |   ```3.1415```   |```  { "type": "builtin.number", "entity": "3 . 1415" }``` |
 ```builtin.ordinal```     |   ```first```   |```{ "type": "builtin.ordinal", "entity": "first" }``` |
 ```builtin.ordinal```     |   ```10th```   | ```{ "type": "builtin.ordinal", "entity": "10th" }``` |  
 ```builtin.percentage```   |   ```The stock price increase by 7 $ this year```   |```{ "type": "builtin.percentage", "entity": "7 %" }```|
 ```builtin.phonenumber```   |   ```my mobile is 00 44 161 1234567```   |```{ "type": "builtin.phonenumber", "entity": "00 44 161 1234567" }```|
 ```builtin.temperature```     |   ```10 degrees celsius```   | ```{ "type": "builtin.temperature", "entity": "10 degrees celcius" }```|   
 ```builtin.temperature```     |   ```78 F```   |```{ "type": "builtin.temperature", "entity": "78 f" }```|
 ```builtin.url```     |   ```http://www.luis.ai is a great cognitive service```   |```{ "type": "builtin.url", "entity": "http://www.luis.ai" }```|
-->

## <a name="contribute-to-prebuilt-entity-cultures"></a>Contribuir a las referencias culturales de las entidades precompiladas
Las entidades precompiladas se desarrollan en el proyecto de código abierto de Recognizers-Text. [Contribuya](https://github.com/Microsoft/Recognizers-Text) al proyecto. Este proyecto incluye ejemplos de moneda por cada referencia cultural. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [number](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) y [currency](luis-reference-prebuilt-currency.md). 
