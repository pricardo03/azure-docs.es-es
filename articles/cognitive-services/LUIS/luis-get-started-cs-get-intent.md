---
title: Obtener intención, C#
titleSuffix: Language Understanding - Azure Cognitive Services
description: En este inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 6d72299e4350c472ef552fa6afd96e48fd1869b9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55876971"
---
# <a name="quickstart-get-intent-using-c"></a>Inicio rápido: Obtención de la intención con C#

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio Community Edition 2017](https://visualstudio.microsoft.com/vs/community/)
* Lenguaje de programación de C# (incluido con Visual Studio Community 2017)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obtención de la intención con el explorador

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Utilice C# para consultar el punto de conexión de predicción GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) para obtener los mismos resultados que vio en la ventana del explorador en la sección anterior. 

1. Cree una aplicación de consola en Visual Studio. 

    ![Creación de una aplicación de consola en Visual Studio](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. En el proyecto de Visual Studio, en el Explorador de soluciones, seleccione **Agregar referencia**, a continuación, seleccione **System.Web** en la pestaña Ensamblados.

    ![Seleccionar Agregar referencia y luego seleccionar System.Web en la pestaña Ensamblados](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Sobrescriba Program.cs con el código siguiente:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Reemplace el valor de `YOUR_KEY` con su clave de LUIS.

5. Compilación y ejecución de la aplicación de consola. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ![Ventana de la consola que muestra el resultado JSON de LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con esta guía de inicio rápido, cierre el proyecto de Visual Studio y quite el directorio del proyecto desde el sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de expresiones y entrenamiento con C#](luis-get-started-cs-add-utterance.md)
