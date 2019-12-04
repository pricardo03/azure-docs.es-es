---
title: 'Inicio rápido: Reconocimiento de voz, intenciones y entidades en C#: servicio Voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 599f59df1a18dfe77d66c497c52a05d7d2e3b6d6
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280391"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
>
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha creado una aplicación de Language Understanding LUIS y ha obtenido una clave de punto de conexión](../../../../quickstarts/create-luis.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Abra el proyecto en Visual Studio.

El primer paso es asegurarse de que tiene el proyecto abierto en Visual Studio.

1. Inicie Visual Studio 2019.
2. Cargue el proyecto y abra `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto. Tenga en cuenta que ha creado un método asincrónico llamado `RecognizeIntentAsync()`.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-17,77-86)]

## <a name="create-a-speech-configuration"></a>Creación de una configuración de Voz

Antes de inicializar un objeto `IntentRecognizer`, debe crear una configuración que use la clave y la región del punto de conexión de LUIS. Inserte este código en el método `RecognizeIntentAsync()`.

En este ejemplo se usa el método `FromSubscription()` para compilar la clase `SpeechConfig`. Para ver una lista completa de los métodos disponibles, consulte [Clase SpeechConfig](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

> [!NOTE]
> Es importante usar la clave del punto de conexión de LUIS y no las claves de inicio o creación, ya que solo la clave del punto de conexión es válida para el reconocimiento de la conversión de voz en intención. Consulte [Creación de una aplicación de LUIS y obtención de una clave de punto de conexión](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md) para instrucciones sobre cómo conseguir la clave correcta.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=26)]

## <a name="initialize-an-intentrecognizer"></a>Inicialización de IntentRecognizer

Ahora, vamos a crear un objeto `IntentRecognizer`. Este objeto se crea dentro de una instrucción using para garantizar la versión correcta de los recursos no administrados. Inserte este código en el método `RecognizeIntentAsync()`, justo debajo de la configuración de Voz.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=28-30,76)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Adición de un objeto LanguageUnderstandingModel e intenciones

Ahora debe asociar un objeto `LanguageUnderstandingModel` con el reconocedor de intenciones y agregar las intenciones que quiera que se reconozcan.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=31-35)]

## <a name="recognize-an-intent"></a>Reconocimiento de una intención

En el objeto `IntentRecognizer`, va a llamar al método `RecognizeOnceAsync()`. Este método permite que el servicio Voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz.

Dentro de la instrucción using, agregue este código: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=46)]

## <a name="display-the-recognition-results-or-errors"></a>Visualización de los resultados (o errores) del reconocimiento

Cuando el servicio Voz devuelva el resultado del reconocimiento, querrá hacer algo con él. Vamos a hacer algo tan sencillo como imprimir el resultado en la consola.

Dentro de la instrucción using, debajo de `RecognizeOnceAsync()`, agregue este código: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=48-75)]

## <a name="check-your-code"></a>Comprobación del código

En este momento, el código debe tener esta apariencia:  
(Se han agregado algunos comentarios a esta versión) [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=5-86)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

1. **Compile el código**: en la barra de menús de Visual Studio, elija **Compilar** > **Compilar solución**.
2. **Inicie la aplicación**: en la barra de menús, elija **Depurar** > **Iniciar depuración** o presione **F5**.
3. **Inicie el reconocimiento**: se le pedirá que diga una frase en inglés. La voz se envía al servicio Voz, se transcribe como texto y se representa en la consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
