---
title: 'Inicio rápido: Obtención de respuestas de la base de conocimiento en QnA Maker con REST y C#'
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido basada en REST de C# le guiará a través del proceso para obtener una respuesta de una base de conocimiento mediante programación.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: f2b5089f1ec334f80ccb4ce80ba9b512f3305859
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861126"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-c"></a>Obtención de respuestas a una pregunta desde una base de conocimiento con C#

Esta guía de inicio rápido le conduce por la obtención mediante programación de una respuesta de una base de conocimiento de QnA Maker publicada. QnA Maker extrae automáticamente preguntas y respuestas a partir de contenido semiestructurado, como las preguntas frecuentes, y de [orígenes de datos](../Concepts/data-sources-supported.md). La pregunta, en formato JSON, se envía en el cuerpo de la solicitud de la API. 


## <a name="prerequisites"></a>Requisitos previos

* Versión de [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) más reciente.
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar la clave, seleccione **Claves** en **Administración de recursos** en el panel de Azure del recurso de QnA Maker. 
* Configuración de la página **Publicar**. Si no tiene una base de conocimiento publicada, cree una vacía, importe una base de conocimiento en la página **Configuración** y, después, publíquela. Puede descargar y usar [esta base de conocimiento básica](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    La configuración de la página de publicación incluye el valor de la ruta de POST, el valor de Host y el valor de EndpointKey. 

    ![Configuración Publicar](../media/qnamaker-quickstart-get-answer/publish-settings.png)

El código de esta guía de inicio rápido se encuentra en el repositorio [https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-knowledge-base-project"></a>Creación de un proyecto de base de conocimiento

1. Abra Visual Studio 2017 Community Edition.
1. Cree un nuevo proyecto de aplicación de consola (.Net Core) y asígnele el nombre QnaMakerQuickstart. Acepte los valores predeterminados del resto de la configuración.

## <a name="add-the-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior del archivo program.cs, reemplace la instrucción using por las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=1-3 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Incorporación de las constantes necesarias

Al principio de la clase `Program`, en `Main`, agregue las constantes necesarias para acceder a QnA Maker. Estos valores se encuentran en la página **Publicar** después de publicar la base de conocimiento. 

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=14-30 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Incorporación de una solicitud POST para enviar una pregunta y recibir una respuesta

El código siguiente realiza una solicitud HTTPS a QnA Maker API para enviar la pregunta a la base de conocimiento y recibe la respuesta:

[!code-csharp[Add a POST request to send question to knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=32-57 "Add a POST request to send question to knowledge base")]

El valor del encabezado `Authorization` incluye la cadena `EndpointKey `. 

## <a name="build-and-run-the-program"></a>Compilar y ejecutar el programa

Compile y ejecute el programa desde Visual Studio. Este enviará automáticamente la solicitud a QnA Maker API y luego la imprimirá en la ventana de la consola.

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
