---
title: 'Obtención de intenciones con Java: LUIS'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido de Java, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 7a80aefd7a88727cf3a2261115c076853bd45b3f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563816"
---
# <a name="quickstart-get-intent-using-java"></a>Inicio rápido: Obtención de intención con Java

En esta guía de inicio rápido, pasará expresiones a un punto de conexión de LUIS y obtendrá entidades e intenciones.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* [JDK SE](https://aka.ms/azure-jdks) (Kit de desarrollo de Java, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) o su IDE favorito.
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obtención de la intención con el explorador

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Puede usar Java para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior. No olvide agregar las bibliotecas de Apache a su proyecto.

1. Copie el código siguiente para crear una clase en un archivo llamado `LuisGetRequest.java`:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Sustituya el valor de la variable `YOUR-KEY` por su clave de LUIS.

3. Reemplace la ruta de acceso de archivo por la suya propia y compile el programa de Java desde una línea de comandos: `javac -cp .;<FILE_PATH>\* LuisGetRequest.java`.

4. Reemplace la ruta de acceso de archivo y ejecute la aplicación desde una línea de comandos: `java -cp .;<FILE_PATH>\* LuisGetRequest.java`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ![Ventana de la consola que muestra el resultado JSON de LUIS](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine la carpeta del archivo o proyecto de Java.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-java-add-utterance.md)
