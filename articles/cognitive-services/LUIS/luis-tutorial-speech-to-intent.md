---
title: Uso del SDK de C# de Voz
titleSuffix: Azure Cognitive Services
description: Speech Service le permite usar una única solicitud para recibir audio y devolver objetos JSON de predicción de LUIS. En este artículo, descarga y usa un proyecto de C# en Visual Studio para decir una expresión a un micrófono y recibir información de predicción de LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 8e664e26a2cafc2f0b32ebea0f019918426d956c
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714587"
---
# <a name="integrate-speech-service-with-your-language-understanding-app"></a>Integración del servicio Voz con la aplicación de Language Understanding
[Speech Service](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) le permite usar una única solicitud para recibir audio y devolver objetos JSON de predicción de LUIS. En este artículo, descarga y usa un proyecto de C# en Visual Studio para decir una expresión a un micrófono y recibir información de predicción de LUIS. El proyecto utiliza el paquete [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) de Voz, que ya se incluye como referencia. 

En este artículo, necesita una cuenta de sitio web de [LUIS][LUIS] gratuita para importar la aplicación.

## <a name="create-luis-endpoint-key"></a>Creación de clave de punto de conexión de LUIS
En Azure Portal, [cree](luis-how-to-azure-subscription.md#create-luis-endpoint-key) una clave de **Language Understanding** (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importación de aplicación Human Resources de LUIS
Las intenciones y expresiones de este artículo proceden de la aplicación Human Resources de LUIS disponible en el repositorio de GitHub [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding). Descargue el archivo [HumanResources.json](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources.json), guárdelo con la extensión `.json` e [impórtelo](luis-how-to-start-new-app.md#import-new-app) en LUIS. 

Esta aplicación tiene intenciones, entidades y expresiones relacionadas con el dominio de Human Resources. Las expresiones de ejemplo incluyen:

|Expresiones de ejemplo|
|--|
|¿Quién es el administrador de John Smith?|
|¿A quién administra John Smith?|
|¿Dónde está el formulario 123456?|
|¿Tengo algún permiso remunerado?|


## <a name="add-keyphrase-prebuilt-entity"></a>Incorporación de entidad pregenerada KeyPhrase
Después de importar la aplicación, seleccione **Entities** (Entidades) y luego **Add prebuilt entity** (Agregar entidad creada previamente). Agregue la entidad **KeyPhrase**. La entidad KeyPhrase extrae el tema principal de la expresión.

## <a name="train-and-publish-the-app"></a>Entrenamiento y publicación de la aplicación
1. En la barra de navegación superior derecha, seleccione el botón **Entrenar** para entrenar la aplicación de LUIS.

2. Seleccione **Administrar** en la barra de la parte superior derecha y, a continuación, seleccione **Claves y puntos de conexión** en el panel de navegación de la izquierda. 

3. En la página **Claves y puntos finales**, asigne la clave de LUIS creada en la sección [Creación de clave de punto de conexión de LUIS](#create-luis-endpoint-key).

  En esta página, recopile el identificador de aplicación, la región de publicación y el identificador de suscripción de la clave de LUIS creada en la sección [Creación de clave de punto de conexión de LUIS](#create-luis-endpoint-key). Debe modificar el código para utilizar estos valores posteriormente en este artículo. 
  
  **No** utilice la clave de inicio gratuita para este ejercicio. Solo funcionará en este ejercicio una clave de **Language Understanding** creada en Azure Portal. 

  https://**REGION**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?subscription-key=**LUISKEY**&q=


4. Publique la aplicación de LUIS seleccionando el botón **Publicar** que se encuentra en la barra superior derecha. 

## <a name="audio-device"></a>Dispositivo de audio
En este artículo se usa el dispositivo de audio en el equipo. Pueden ser unos auriculares con micrófono o un dispositivo de audio integrado. Compruebe los niveles de entrada de audio para ver si debería hablar más alto de lo que lo hace normalmente para que el dispositivo de audio pueda detectar su voz. 

## <a name="download-the-luis-sample-project"></a>Descarga del proyecto de ejemplo de LUIS
 Clone o descargue el repositorio [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding). Abra el [proyecto de conversión de voz en intención](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-speech-intent-recognition) con Visual Studio y restaure los paquetes NuGet. El archivo de solución de VS es documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

El SDK de Voz ya está incluido como referencia. 

[![Captura de pantalla de Visual Studio 2017 que muestra el paquete NuGet Microsoft.CognitiveServices.Speech](./media/luis-tutorial-speech-to-intent/nuget-package.png "Screenshot of Visual Studio 2017 displaying Microsoft.CognitiveServices.Speech NuGet package")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Modificación del código de C#
Abra el archivo `Program.cs` y cambie las variables siguientes:

|Nombre de la variable|Propósito|
|--|--|
|LUIS_assigned_endpoint_key|Corresponde al valor de clave de suscripción asignado de la dirección URL del punto de conexión de la página Publicar.|
|LUIS_endpoint_key_region|Corresponde al primer subdominio de la dirección URL del punto de conexión, por ejemplo `westus`.|
|LUIS_app_ID|Corresponde a la ruta de la dirección URL del punto de conexión que aparece después de **apps/**.|

El archivo `Program.cs` ya tiene las intenciones de Human Resources asignadas.

Compile y ejecute la aplicación. 

## <a name="test-code-with-utterance"></a>Prueba de código con expresión
Diga por el micrófono "¿Qué dentistas autorizados hay en Redmond?".

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

La intención correcta, **GetEmployeeBenefits**, se ha conseguido con una confiabilidad del 85 %. Se ha devuelto la entidad keyPhrase. 

El SDK de Voz devuelve toda la respuesta de LUIS. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no la necesite, elimine la aplicación HumanResources de LUIS. Para ello, seleccione la aplicación y, a continuación, en la barra de herramientas contextual encima de la lista, seleccione **Eliminar**. En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

Recuerde eliminar el directorio cuando termine de usar el ejemplo de código.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Integración de LUIS con un BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website