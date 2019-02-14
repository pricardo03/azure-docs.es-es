---
title: Publicación de la base de conocimiento, REST, C#
titleSuffix: QnA Maker- Azure Cognitive Services
description: En este inicio rápido basado en REST de C# se explica el proceso de publicación de la base de conocimiento que le permite insertar la versión más reciente probada en un índice de Azure Search dedicado que representa la base de conocimiento publicada. También se crea un punto de conexión al que puede llamar en su aplicación o bot de chat.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/18/2018
ms.author: diberry
ms.openlocfilehash: 587661236e28b9e0fdf7d527b762897d417d0d98
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866855"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Inicio rápido: Publicación de una base de conocimiento en QnA Maker con C#

En esta guía de inicio rápido basada en REST se describe la publicación mediante programación de una base de conocimiento (KB). La publicación inserta la versión más reciente de la base de conocimiento en un índice de Azure Search dedicado y crea un punto de conexión que se puede llamar en su aplicación o bot de chat.

En esta guía de inicio rápido se llama a las siguientes API de QnA Maker:
* [Publicar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): esta API no requiere ninguna información en el cuerpo de la solicitud.

## <a name="prerequisites"></a>Requisitos previos

* Versión de [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) más reciente.
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar la clave, seleccione **Claves** en **Administración de recursos** en el panel. 
* Identificador de la base de conocimiento (KB) de QnA Maker encontrado en la dirección URL en el parámetro de cadena de consulta de KBID, como se muestra a continuación.

    ![Identificador de base de conocimiento de QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Si aún no tiene una base de conocimiento, puede crear una de ejemplo para usar en este inicio rápido: [Creación de una base de conocimiento](create-new-kb-csharp.md)

> [!NOTE] 
> Los archivos de la solución completa están disponibles en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-knowledge-base-project"></a>Creación del proyecto de base de conocimiento

1. Abra Visual Studio 2017 Community Edition.
1. Cree un nuevo proyecto de **aplicación de consola (.Net Core)** y asígnele el nombre `QnaMakerQuickstart`. Acepte los valores predeterminados del resto de la configuración.

## <a name="add-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior de Program.cs, reemplace la instrucción using por las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Incorporación de las constantes necesarias

En el método **Main**, agregue las constantes necesarias para acceder a QnA Maker. Reemplace los valores por los suyos.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=11-14 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Incorporación de una solicitud POST para publicar la base de conocimiento

Tras las constantes requeridas, agregue el siguiente código, que realiza una solicitud HTTPS a QnA Maker API para publicar una base de conocimiento y recibe la respuesta:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=16-29&dedent=8 "Add HTTP Post request and response")]

La llamada API devuelve un estado 204 en caso de publicación correcta sin ningún contenido en el cuerpo de la respuesta. 
 
## <a name="build-and-run-the-program"></a>Compilar y ejecutar el programa

Compile y ejecute el programa. Este enviará automáticamente la solicitud a QnA Maker API para publicar la base de conocimiento y, después, la respuesta se imprimirá en la ventana de la consola.

Una vez publicada la base de conocimiento, puede realizar consultas en ella desde el punto de conexión con una aplicación cliente o un bot de chat. 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Pasos siguientes

Después de publicar la base de conocimiento, es preciso que la dirección URL del punto de conexión [ genere una respuesta](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
