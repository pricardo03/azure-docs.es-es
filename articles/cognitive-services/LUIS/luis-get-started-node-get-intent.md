---
title: Obtener intención, Node.js
titleSuffix: Language Understanding - Azure Cognitive Services
description: En esta guía de inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional. Con Node.js, envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación pública.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 01/17/2019
ms.author: diberry
ms.openlocfilehash: 161056aa2cfe43375d5e555197b57f85685ca219
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65594107"
---
# <a name="quickstart-get-intent-using-nodejs"></a>Inicio rápido: Obtención de la intención con Node.js

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* Lenguaje de programación [Node.js](https://nodejs.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> La solución completa de Node.js está disponible en el repositorio [**Azure-Samples** de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/analyze-text/node).

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obtención de la intención con el explorador

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Puede usar Node.js para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior.

1. Copie el siguiente fragmento de código:

   [!code-nodejs[Console app code that calls a LUIS endpoint for Node.js](~/samples-luis/documentation-samples/quickstarts/analyze-text/node/call-endpoint.js)]

2. Cree el archivo `.env` con el siguiente texto o establezca estas variables en el entorno del sistema:

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. Establezca la variable de entorno `LUIS_ENDPOINT_KEY` en su clave.

4. Instale las dependencias ejecutando el siguiente comando en la línea de comandos: `npm install`.

5. Ejecute el código con `npm start`. Muestra los mismos valores que vio anteriormente en la ventana del explorador.

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el archivo de Node.js.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-node-add-utterance.md)
