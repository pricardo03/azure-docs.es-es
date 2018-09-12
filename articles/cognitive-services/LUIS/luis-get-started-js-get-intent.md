---
title: Análisis de texto en lenguaje natural en Language Understanding (LUIS) mediante Javascript en Cognitive Services (Azure Cognitive Services) | Microsoft Docs
description: En esta guía de inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional. Con Javascript, envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación pública. En el punto de conexión, LUIS aplica el modelo de la aplicación pública para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: d787f744ff0fe7315553e9dd6f4465122f7e06b2
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159715"
---
# <a name="quickstart-analyze-text-using-javascript"></a>Guía de inicio rápido: Análisis de texto mediante Javascript

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio Code](https://code.visualstudio.com/)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Análisis de texto con el explorador

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-javascript"></a>Análisis de texto con Javascript 

Puede usar JavaScript para acceder a los mismos resultados que vio en la ventana del explorador en el paso anterior. 

1. Copie el código que sigue y guárdelo en un archivo HTML:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Abra el archivo en un explorador. Escriba la clave de punto de conexión de LUIS en el formulario y seleccione **Enviar**.

    ![Ejemplo HTML mostrado en el explorador con resultados de LUIS para una aplicación de automatización de dispositivos del hogar](./media/luis-get-started-js-get-intent/html-results.png)

    El resultado se muestra debajo del formulario. 

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el archivo de Javascript.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-javascript-add-utterance.md)
