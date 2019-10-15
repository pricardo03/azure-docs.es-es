---
title: Obtención de intención con la llamada a REST en Java
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 7199dfaaa476e4be27929010b76a6e0544857bdb
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838502"
---
## <a name="prerequisites"></a>Requisitos previos

* [JDK SE](https://aka.ms/azure-jdks) (Kit de desarrollo de Java, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) o su IDE favorito.
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Puede usar Java para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior. No olvide agregar las bibliotecas de Apache a su proyecto.

1. Copie el código siguiente para crear una clase en un archivo llamado `LuisGetRequest.java`:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Sustituya el valor de la variable `YOUR-KEY` por su clave de LUIS.

3. Reemplace la ruta de acceso de archivo por la suya propia y compile el programa de Java desde una línea de comandos: `javac -cp .;<FILE_PATH>\* LuisGetRequest.java`.

4. Reemplace la ruta de acceso de archivo y ejecute la aplicación desde una línea de comandos: `java -cp .;<FILE_PATH>\* LuisGetRequest.java`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ![Ventana de la consola que muestra el resultado JSON de LUIS](../media/luis-get-started-java-get-intent/console-turn-on.png)
    


## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con esta guía de inicio rápido, cierre el proyecto de Visual Studio y quite el directorio del proyecto desde el sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de expresiones y entrenamiento con Java](../luis-get-started-java-add-utterance.md)