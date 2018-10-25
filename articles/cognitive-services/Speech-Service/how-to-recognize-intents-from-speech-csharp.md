---
title: 'Tutorial: Reconocimiento de intenciones a partir de contenido de voz con SDK de Voz para C#'
titleSuffix: Azure Cognitive Services
description: En este tutorial aprenderá a reconocer intenciones a partir de contenido de voz con el SDK de Voz para C#.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 64fa194225c7e9fa4c272ca8a9e95b44282ec1df
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466484"
---
# <a name="tutorial-recognize-intents-from-speech-using-the-speech-sdk-for-c"></a>Tutorial: Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para C#

[!INCLUDE [Article selector](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-selector.md)]

El [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) de Cognitive Services se integra con [Language Understanding Intelligent Service (LUIS)](https://www.luis.ai/home) para proporcionar un **reconocimiento de intenciones.** Una intención es algo que el usuario quiere hacer: reservar un vuelo, comprobar el tiempo o hacer una llamada. El usuario puede utilizar cualquier término que le parezca natural. Con el aprendizaje automático, LUIS asigna las solicitudes de los usuarios a las intenciones que se han definido.

> [!NOTE]
> Una aplicación LUIS define las intenciones y entidades que desea reconocer. Es independiente de la aplicación C# que utiliza el servicio Voz. En este artículo, con la palabra "aplicación" se indica tanto la aplicación LUIS, como el código C#.

En este tutorial, se utiliza el SDK de Voz para desarrollar una aplicación de consola C# que deriva las intenciones de las expresiones de los usuarios mediante el micrófono del dispositivo. Aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto de Visual Studio que haga referencia al paquete NuGet del SDK de Voz
> * Crear una configuración de voz y obtener un reconocedor de intenciones
> * Obtener el modelo para la aplicación LUIS y agregar las intenciones que necesita
> * Especificar el idioma para el reconocimiento de voz
> * Reconocer la voz a partir de un archivo
> * Usar el reconocimiento asincrónico, continuo y controlado por eventos

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de contar con lo siguiente antes de empezar este tutorial.

* Una cuenta de LUIS. Puede obtener una gratis mediante el [portal de LUIS](https://www.luis.ai/home).
* Visual Studio 2017 (cualquier edición).

## <a name="luis-and-speech"></a>LUIS y voz

LUIS se integra con el servicio Voz para reconocer las intenciones a partir de contenido de voz. No necesita una suscripción al servicio Voz, solo LUIS.

LUIS usa dos tipos de claves: 

|Tipo de clave|Propósito|
|--------|-------|
|creación|le permite crear y modificar mediante programación de aplicaciones LUIS|
|endpoint |autoriza el acceso a una determinada aplicación LUIS|

La clave de punto de conexión es la clave de LUIS necesaria para este tutorial. Este tutorial utiliza la aplicación de automatización del hogar de LUIS, que puede crear siguiendo [Uso de la aplicación de automatización del hogar pregenerada](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app). Si ha creado su propia aplicación LUIS, puede usarla en su lugar.

Al crear una aplicación LUIS, se genera automáticamente una clave de inicio para que pueda probar la aplicación mediante consultas de texto. Esta tecla no permite la integración del servicio Voz y no funcionará con este tutorial. Debe crear un recurso de LUIS en el panel de Azure y asignarlo a la aplicación de LUIS. Puede usar el nivel de suscripción gratis para este tutorial. 

Después de crear el recurso de LUIS en el panel de Azure, inicie sesión en el [portal de LUIS](https://www.luis.ai/home), seleccione la aplicación en la página Mis aplicaciones y, después, cambie a la página Manage (Administrar) de la aplicación. Por último, haga clic en **Keys and Endpoints** (Claves y puntos de conexión) en la barra lateral.

![Configuración de los puntos de conexión y claves del portal de LUIS](media/sdk/luis-keys-endpoints-page.png)

En la página de configuración Keys and Endpoints (Claves y puntos de conexión):

1. Desplácese hacia abajo hasta la sección Resources and Keys (Recursos y claves) y haga clic en **Assign resource** (Asignar recurso).
1. En el cuadro de diálogo **Assign a key to your app** (Asignar una clave a la aplicación), elija lo siguiente:

    * Elija Microsoft como el inquilino.
    * En Subscription Name (Nombre de suscripción), elija la suscripción de Azure que contiene el recurso de LUIS que desea usar.
    * En Key (Clave), elija el recurso de LUIS que se va a usar con la aplicación.

En unos instantes, la nueva suscripción aparecerá en la tabla en la parte inferior de la página. Haga clic en el icono situado junto a una clave para copiarla al Portapapeles. (Puede usar cualquiera de las claves).

![Claves de suscripción de la aplicación LUIS](media/sdk/luis-keys-assigned.png)

## <a name="create-a-speech-project-in-visual-studio"></a>Creación de un proyecto de contenido de voz en Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-the-code"></a>Adición del código

Abra el archivo `Program.cs` en el proyecto de Visual Studio y reemplace el bloque de sentencias `using` al principio del archivo por las siguientes declaraciones.

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

Dentro del método `Main()` proporcionado, agregue el código siguiente.

```csharp
RecognizeIntentAsync().Wait();
Console.WriteLine("Please press Enter to continue.");
Console.ReadLine();
```

Cree un método asincrónico vacío `RecognizeIntentAsync()`, tal y como se muestra aquí.

```csharp
static async Task RecognizeIntentAsync()
{
}
```

En el cuerpo de este nuevo método, agregue este código.

[!code-csharp[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

Reemplace los marcadores de posición en este método por su clave de suscripción, región e identificador de aplicación de LUIS, como se indica a continuación.

|Placeholder|Reemplazar por|
|-----------|------------|
|`YourLanguageUnderstandingSubscriptionKey`|La clave de punto de conexión de LUIS. Como se mencionó anteriormente, esta debe ser una clave obtenida en el panel de Azure, no una "tecla de inicio". Puede encontrarla en la página Keys and Endpoints (Claves y puntos de conexión) de la aplicación (en Manage [Administrar]) en el [portal de LUIS](https://www.luis.ai/home).|
|`YourLanguageUnderstandingServiceRegion`|El identificador corto para la región en la que se encuentra la suscripción a LUIS, como `westus` para Oeste de EE. UU. Consulte [Regiones](regions.md).|
|`YourLanguageUnderstandingAppId`|El id. de la aplicación LUIS. Puede encontrarlo en la página Settings (Configuración) del [portal de LUIS](https://www.luis.ai/home).|

Con estos cambios realizados, puede crear (Control-Mayús-B) y ejecutar (F5) la aplicación del tutorial. Cuando se le solicite, pruebe a decir "Apagar las luces" al micrófono del equipo. El resultado se muestra en la ventana de la consola.

Las secciones siguientes incluyen una explicación del código.


## <a name="create-an-intent-recognizer"></a>Creación de un reconocedor de intenciones

El primer paso para reconocer las intenciones en el habla es crear una configuración de voz a partir de la clave y la región del punto de conexión de LUIS. Las configuraciones de voz se pueden utilizar para crear reconocedores de las distintas funcionalidades del SDK de Voz. La configuración de voz tiene múltiples formas de especificar la suscripción que desea usar; aquí, usamos `FromSubscription`, que toma la clave de suscripción y la región.

> [!NOTE]
> Utilice la clave y la región de su suscripción a LUIS, no de su suscripción a Voz.

Después, cree un reconocedor de intenciones mediante `new IntentRecognizer(config)`. Dado que la configuración ya sabe qué suscripción utilizar, no es necesario volver a especificar la clave de suscripción y el punto de conexión al crear el reconocedor.

## <a name="import-a-luis-model-and-add-intents"></a>Importación de un modelo de LUIS y adición de intenciones

Ahora importe el modelo desde la aplicación LUIS mediante `LanguageUnderstandingModel.FromAppId()` y agregue las intenciones de LUIS que desee reconocer mediante el método `AddIntent()` del reconocedor. Estos dos pasos mejoran la precisión del reconocimiento de voz al indicar palabras que el usuario probablemente utilizará en sus solicitudes. No es necesario agregar todas las intenciones de la aplicación si no necesita reconocerlas todas en la aplicación.

La adición de intenciones requiere tres argumentos: el modelo LUIS (que se acaba de crear y se llama `model`), el nombre de la intención y una identificación de la intención. La diferencia entre el identificador y el nombre es como sigue.

|Argumento `AddIntent()`|Propósito|
|--------|-------|
|intentName |El nombre de la intención, tal como se define en la aplicación LUIS. Debe coincidir exactamente con el nombre de la intención de LUIS.|
|intentID    |Identificador asignado a una intención reconocida por el SDK de Voz. Puede ser el que quiera; no necesita corresponderse con el nombre de la intención tal y como se define en la aplicación LUIS. Si se controlan varias intenciones con el mismo código, por ejemplo, se puede utilizar el mismo identificador para ellos.|

La aplicación de automatización del hogar de LUIS tiene dos propósitos: uno, encender un dispositivo y otro, apagarlo. En las líneas siguientes se agregan estas intenciones al reconocedor; reemplace las tres líneas `AddIntent` del método `RecognizeIntentAsync()` por este código.

```csharp
recognizer.AddIntent(model, "HomeAutomation.TurnOff", "off");
recognizer.AddIntent(model, "HomeAutomation.TurnOn", "on");
```

## <a name="start-recognition"></a>Inicio del reconocimiento

Cuando haya creado el reconocedor y haya agregado las intenciones, puede empezar el reconocimiento. El SDK de Voz admite tanto el reconocimiento continuo como el de una sola emisión.

|Modo de reconocimiento|Métodos para la llamada|Resultado|
|----------------|-----------------|---------|
|Emisión única|`RecognizeOnceAsync()`|Devuelve la intención reconocida, si la hubiera, después de una expresión.|
|Continuo|`StartContinuousRecognitionAsync()`<br>`StopContinuousRecognitionAsync()`|Reconoce varias expresiones. Emite eventos (por ejemplo, `IntermediateResultReceived`) cuando los resultados están disponibles.|

La aplicación de tutorial utiliza el modo de emisión única y, por lo tanto, llama a `RecognizeOnceAsync()` para iniciar el reconocimiento. El resultado es un objeto `IntentRecognitionResult` que contiene información sobre la intención reconocida. La respuesta JSON de LUIS se extrae mediante la siguiente expresión:

```csharp
result.Properties.GetProperty(PropertyId.LanguageUnderstandingServiceResponse_JsonResult)
```

La aplicación del tutorial no analiza el resultado de JSON, solo lo muestra en la ventana de la consola.

![Resultados del reconocimiento de LUIS](media/sdk/luis-results.png)

## <a name="specify-recognition-language"></a>Especificación de un idioma de reconocimiento

De forma predeterminada, LUIS reconoce las intenciones en idioma inglés de Estados Unidos (`en-us`). Al asignar un código de configuración regional a la propiedad `SpeechRecognitionLanguage` de la configuración de voz, puede reconocer las intenciones en otros idiomas. Por ejemplo, agregue `config.SpeechRecognitionLanguage = "de-de";` en la aplicación tutorial antes de crear el reconocedor para reconocer las intenciones en idioma alemán. Consulte [Idiomas admitidos](language-support.md#speech-to-text).

## <a name="continuous-recognition-from-a-file"></a>Reconocimiento continuo desde un archivo

El código siguiente muestra dos funcionalidades adicionales de reconocimiento de intenciones mediante el SDK de Voz. La primera, mencionada anteriormente, es el reconocimiento continuo, donde el reconocedor emite eventos cuando los resultados están disponibles. Estos eventos los pueden procesar los controladores de eventos que proporcione. Con el reconocimiento continuo, se llama a `StartContinuousRecognitionAsync()` del reconocedor para iniciar el reconocimiento en lugar de `RecognizeOnceAsync()`.

La otra funcionalidad es leer el audio que contiene la voz que se va a procesar desde un archivo WAV. Esto implica la creación de una configuración de audio que se puede utilizar al crear el reconocedor de intenciones. El archivo debe ser monocanal (mono) con una frecuencia de muestreo de 16 kHz.

Para probar estas características, reemplace el cuerpo del método `RecognizeIntentAsync()` por el código siguiente. 

[!code-csharp[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

Revise el código para incluir la clave del punto de conexión de LUIS, la región y el identificador de la aplicación y para agregar las intenciones de automatización del hogar, como antes. Cambie `whatstheweatherlike.wav` por el nombre del archivo de audio. Después compílelo y ejecútelo.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque el código de este artículo en la carpeta samples/csharp/sharedcontent/console.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de voz](how-to-recognize-speech-csharp.md)
