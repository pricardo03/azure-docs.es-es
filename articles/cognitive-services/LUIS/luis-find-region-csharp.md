---
title: Región del punto de conexión, C#
titleSuffix: Language Understanding - Azure Cognitive Services
description: Con C#, busque la región de publicación con la clave de punto de conexión y el identificador de la aplicación para LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: f95dec8a539a92a0397421fbde411f646eeca3ca
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720091"
---
# <a name="programmatically-find-endpoint-region-with-c"></a>Búsqueda mediante programación de la región de punto de conexión con C# 
Si tiene los identificadores de aplicación y suscripción de LUIS, puede buscar la región que se va a usar para las consultas de punto de conexión.

> [!NOTE] 
> La solución completa de C# está disponible en el repositorio [**Azure-Samples** de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/find-region/csharp/).

## <a name="luis-endpoint-query-strategy"></a>Estrategia de consulta de punto de conexión de LUIS
Cada consulta de punto de conexión de LUIS requiere lo siguiente:

* Una clave de punto de conexión
* Un id. de la aplicación
* Una región

Si la consulta de punto de conexión de LUIS usa la clave de punto de conexión y el id. de la aplicación correctos pero la región incorrecta, el código de respuesta es 401. La solicitud 401 no se cuenta para la cuota de suscripción. Convierta esta solicitud en una estrategia para sondear todas las regiones para buscar la región correcta. La región correcta es la única solicitud que devuelve un código de estado 2xx. 

|Response code|Parámetros|
|--|--|
|2xx|clave de punto de conexión correcta<br>Id. de la aplicación correcto<br>Región de host correcta|
|401|clave de punto de conexión correcta<br>Id. de la aplicación correcto<br>Región de host _incorrecta_|

## <a name="c-class-code-to-find-region"></a>Código de clase de C# para buscar la región
La aplicación de consola toma el id. de la aplicación de LUIS y la clave de punto de conexión y devuelve todas las regiones asociadas con ella. En la actualidad, una clave de punto de conexión se crea por región, por lo que solo se debe devolver una región.

Incluya las dependencias de biblioteca de .NET:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Agregue esta clase de LUIS personalizada creada para buscar la región. Reemplace los valores de variable para de `luisAppId` y `luisSubscriptionKey` por valores propios. Todas las regiones que devuelven 401 se escribirán en la consola de depuración. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Este es un ejemplo de una llamada a la clase LUIS personalizada en el método Main de la aplicación de consola:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Cuando se ejecuta la aplicación, en la consola se muestra la región para el id. de la aplicación.

![Captura de pantalla de la aplicación de consola en la que se muestra la región de LUIS](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [regiones](luis-reference-regions.md) de LUIS.
