---
title: Obtención de intención con la llamada a REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: e6ae9590cee3a2ddc3b8e121161fcf84815da28a
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838549"
---
## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio Community Edition 2017](https://visualstudio.microsoft.com/vs/community/)
* Lenguaje de programación de C# (incluido con Visual Studio Community 2017)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Utilice C# para consultar el punto de conexión de predicción GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) para obtener los mismos resultados que vio en la ventana del explorador en la sección anterior. 

1. Cree una aplicación de consola en Visual Studio. 

    ![Creación de una aplicación de consola en Visual Studio](../media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. En el proyecto de Visual Studio, en el Explorador de soluciones, seleccione **Agregar referencia**, a continuación, seleccione **System.Web** en la pestaña Ensamblados.

    ![Seleccionar Agregar referencia y luego seleccionar System.Web en la pestaña Ensamblados](../media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Sobrescriba Program.cs con el código siguiente:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Reemplace el valor de `YOUR_KEY` con su clave de LUIS.

5. Compilación y ejecución de la aplicación de consola. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ![Ventana de la consola que muestra el resultado JSON de LUIS](../media/luis-get-started-cs-get-intent/console-turn-on.png)



## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con esta guía de inicio rápido, cierre el proyecto de Visual Studio y quite el directorio del proyecto desde el sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de expresiones y entrenamiento con C#](../luis-get-started-cs-add-utterance.md)