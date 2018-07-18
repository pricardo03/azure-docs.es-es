---
title: Uso del SDK de C# de Voz con LUIS - Azure | Microsoft Docs
titleSuffix: Azure
description: Utilice el ejemplo del SDK de C# de Voz para hablar al micrófono y obtener la devolución de predicciones de intenciones y de entidades de LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/26/2018
ms.author: v-geberr;
ms.openlocfilehash: b681598f953d217ca636fb5c0adc3de4ddbebd60
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031794"
---
# <a name="integrate-speech-service"></a>Integración de Speech Service
[Speech Service](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) le permite usar una única solicitud para recibir audio y devolver objetos JSON de predicción de LUIS.

En este artículo, descarga y usa un proyecto de C# en Visual Studio para decir una expresión a un micrófono y recibir información de predicción de LUIS. El proyecto utiliza el paquete [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) de Voz, que ya se incluye como referencia. 

En este artículo, necesita una cuenta de sitio web de [LUIS][LUIS] gratuita para importar la aplicación.

## <a name="create-luis-endpoint-key"></a>Creación de clave de punto de conexión de LUIS
En Azure Portal, [cree](luis-how-to-azure-subscription.md#create-luis-endpoint-key) una clave de **Language Understanding** (LUIS). 

## <a name="import-human-resources-luis-app"></a>Importación de aplicación Human Resources de LUIS
Las intenciones y expresiones de este artículo proceden de la aplicación Human Resources de LUIS disponible en el repositorio de Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples). Descargue el archivo [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json), guárdelo con la extensión *.json e [impórtelo](create-new-app.md#import-new-app) a LUIS. 

Esta aplicación tiene intenciones, entidades y expresiones relacionadas con el dominio de Human Resources. Las expresiones de ejemplo incluyen:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Incorporación de entidad pregenerada KeyPhrase
Después de importar la aplicación, seleccione **Entidades** y luego **Manage prebuilt entities** (Administrar entidades pregeneradas). Agregue la entidad **KeyPhrase**. La entidad KeyPhrase extrae el tema principal de la expresión.

## <a name="train-and-publish-the-app"></a>Entrenamiento y publicación de la aplicación
1. En la barra de navegación superior derecha, seleccione el botón **Entrenar** para entrenar la aplicación de LUIS.

2. Seleccione **Publicar** para ir a la página de publicación. 

3. En la parte inferior de la página **Publicar**, agregue la clave de LUIS creada en la sección [Creación de clave de punto de conexión de LUIS](#create-luis-endpoint-key).

4. Publique la aplicación de LUIS seleccionando el botón **Publicar** que se encuentra a la derecha del espacio Publicar. 

  En la página **Publicar**, recopile el identificador de aplicación, la región de publicación y el identificador de suscripción de la clave de LUIS creada en la sección [Creación de clave de punto de conexión de LUIS](#create-luis-endpoint-key). Debe modificar el código para utilizar estos valores posteriormente en este artículo. 

  Estos valores están todos incluidos en la dirección URL del punto de conexión en la parte inferior de la página **Publicar** para la clave que creó. 
  
  **No** utilice la clave de inicio gratuita para este ejercicio. Solo funcionará en este ejercicio una clave de **Language Understanding** creada en Azure Portal. 

  https://**REGION**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**?subscription-key=**LUISKEY**&q=

## <a name="audio-device"></a>Dispositivo de audio
En este artículo se usa el dispositivo de audio en el equipo. Pueden ser unos auriculares con micrófono o un dispositivo de audio integrado. Compruebe los niveles de entrada de audio para ver si debería hablar más alto de lo que lo hace normalmente para que el dispositivo de audio pueda detectar su voz. 

## <a name="download-the-luis-sample-project"></a>Descarga del proyecto de ejemplo de LUIS
 Clone o descargue el repositorio [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples). Abra el [proyecto de conversión de voz en intención](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) con Visual Studio y restaure los paquetes NuGet. El archivo de solución de VS es .\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln.

El SDK de Voz ya está incluido como referencia. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Captura de pantalla de Visual Studio 2017 que muestra el paquete NuGet Microsoft.CognitiveServices.Speech")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>Modificación del código de C#
Abra el archivo **LUIS_samples.cs** y cambie las siguientes variables:

|Nombre de la variable|Propósito|
|--|--|
|luisSubscriptionKey|Corresponde al valor de clave de suscripción de la dirección URL del punto de conexión de la página Publicar.|
|luisRegion|Corresponde al primer subdominio de la dirección URL del punto de conexión.|
|luisAppId|Corresponde a la ruta de la dirección URL del punto de conexión que aparece después de **apps/**.|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Captura de pantalla de Visual Studio 2017 que muestra variables de LUIS_samples.cs")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

El archivo ya tiene las intenciones de Human Resources asignadas.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Captura de pantalla de Visual Studio 2017 que muestra intenciones de LUIS_samples.cs")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Compile y ejecute la aplicación. 

## <a name="test-code-with-utterance"></a>Prueba de código con expresión
Seleccione **1** y diga al micrófono "Quién es el administrador de John Smith".

```cmd
1. Speech recognition of LUIS intent.
0. Stop.
Your choice: 1
LUIS...
Say something...
ResultId:cc83cebc9d6040d5956880bcdc5f5a98 Status:Recognized IntentId:<GetEmployeeOrgChart> Recognized text:<Who is the manager of John Smith?> Recognized Json:{"DisplayText":"Who is the manager of John Smith?","Duration":25700000,"Offset":9200000,"RecognitionStatus":"Success"}. LanguageUnderstandingJson:{
  "query": "Who is the manager of John Smith?",
  "topScoringIntent": {
    "intent": "GetEmployeeOrgChart",
    "score": 0.617331
  },
  "entities": [
    {
      "entity": "manager of john smith",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 31
    }
  ]
}

Recognition done. Your Choice:

```

La intención correcta, **GetEmployeeOrgChart**, se ha conseguido con una confiabilidad del 61 %. Se ha devuelto la entidad keyPhrase. 

El SDK de Voz devuelve toda la respuesta de LUIS. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no la necesite, elimine la aplicación HumanResources de LUIS. Para ello, seleccione el menú de tres puntos (...) a la derecha del nombre de la aplicación en la lista de aplicaciones y seleccione **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), seleccione **Ok** (Aceptar).

Recuerde eliminar el directorio LUIS-Samples cuando termine de utilizar el código de ejemplo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Integración de LUIS con un BOT](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website