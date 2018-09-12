---
title: 'Análisis de texto en lenguaje natural en Language Understanding (LUIS) mediante Node.js en Cognitive Services: Azure Cognitive Services | Microsoft Docs'
description: En esta guía de inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional. Con Node.js, envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación pública. En el punto de conexión, LUIS aplica el modelo de la aplicación pública para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: d7067041ae8e413675de3c95ca4ba0c1b987f47a
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771696"
---
# <a name="quickstart-analyze-text-using-nodejs"></a>Guía de inicio rápido: análisis de texto con Node.js

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* Lenguaje de programación [Node.js](https://nodejs.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> La solución completa de Node.js está disponible en el repositorio [**LUIS-Samples** de GitHub](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/analyze-text/node).

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Análisis de texto con el explorador

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-nodejs"></a>Análisis de texto con Node.js

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

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el archivo de Node.js.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-node-add-utterance.md)