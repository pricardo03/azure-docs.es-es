---
title: 'Análisis de texto en lenguaje natural en Language Understanding (LUIS) mediante C#: Azure Cognitive Services | Microsoft Docs'
description: En esta guía de inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional. Con C#, envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación pública. En el punto de conexión, LUIS aplica el modelo de la aplicación pública para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 676546a215bbb8964f1cb2d26ae0fb9fd2ed9289
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771657"
---
# <a name="quickstart-analyze-text-using-c"></a>Guía de inicio rápido: análisis de texto con C#

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio Community Edition 2017](https://visualstudio.microsoft.com/vs/community/)
* Lenguaje de programación de C# (incluido con Visual Studio Community 2017)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Análisis de texto con el explorador

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Análisis de texto con C# 

Utilice C# para consultar el punto de conexión de predicción GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) para obtener los mismos resultados que vio en la ventana del explorador en la sección anterior. 

1. Cree una aplicación de consola en Visual Studio. 

    ![Acceso al menú de configuración de usuario de LUIS](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. En el proyecto de Visual Studio, en el Explorador de soluciones, seleccione **Agregar referencia**, a continuación, seleccione **System.Web** en la pestaña Ensamblados.

    ![Acceso al menú de configuración de usuario de LUIS](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Sobrescriba Program.cs con el código siguiente:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Reemplace el valor de `YOUR_KEY` con su clave de LUIS.

5. Compilación y ejecución de la aplicación de consola. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ![Ventana de la consola que muestra el resultado JSON de LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>Claves de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con esta guía de inicio rápido, cierre el proyecto de Visual Studio y quite el directorio del proyecto desde el sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de expresiones y entrenamiento con C#](luis-get-started-cs-add-utterance.md)