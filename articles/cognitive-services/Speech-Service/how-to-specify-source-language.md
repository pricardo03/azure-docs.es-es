---
title: Especificación del idioma de origen para la conversión de voz en texto
titleSuffix: Azure Cognitive Services
description: El SDK de Voz permite especificar el lenguaje fuente al convertir voz en texto. En este artículo se describe cómo usar los métodos FromConfig y SourceLanguageConfig para permitir que el servicio de voz conozca el lenguaje de origen y proporcione un destino de modelo personalizado.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/26/2019
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 94b8fb026b61b52e8096cf54e1db30a6c260c04b
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74109958"
---
# <a name="specify-source-language-for-speech-to-text"></a>Especificación del lenguaje fuente para la conversión de voz en texto

En este artículo, obtendrá información sobre cómo especificar el lenguaje fuente de una entrada de audio que se pasa al SDK de Voz para el reconocimiento de voz. Además, se proporciona código de ejemplo para especificar un modelo de voz personalizado para el reconocimiento mejorado.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>Cómo especificar el lenguaje fuente en C#

El primer paso consiste en crear un `SpeechConfig`:

```csharp
var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

A continuación, especifique el lenguaje fuente del audio con `SpeechRecognitionLanguage`:

```csharp
speechConfig.SpeechRecognitionLanguage = "de-DE";
```

Si utiliza un modelo personalizado para el reconocimiento, puede especificar el punto de conexión con `EndpointId`:

```csharp
speechConfig.EndpointId = "The Endpoint ID for your custom model.";
```

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>Cómo especificar el lenguaje fuente en C++

En este ejemplo, el lenguaje de origen se proporciona explícitamente como un parámetro mediante el método `FromConfig`.

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

En este ejemplo, el lenguaje de origen se proporciona mediante `SourceLanguageConfig`. A continuación, se pasa el método `sourceLanguageConfig` como un parámetro a `FromConfig` al crear el elemento `recognizer`.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

En este ejemplo, el lenguaje de origen y el punto de conexión personalizado se proporcionan mediante `SourceLanguageConfig`. Se pasa el método `sourceLanguageConfig` como un parámetro a `FromConfig` al crear el elemento `recognizer`.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage` y `SetEndpointId` son métodos en desuso de la clase `SpeechConfig` en C++ y Java. No se recomienda el uso de estos métodos y no deben usarse al construir un `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Cómo especificar el lenguaje fuente en Java

En este ejemplo, el lenguaje de origen se proporciona explícitamente cuando se crea un nuevo elemento `SpeechRecognizer`.

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

En este ejemplo, el lenguaje de origen se proporciona mediante `SourceLanguageConfig`. A continuación, se pasa el método `sourceLanguageConfig` como parámetro al crear el elemento `SpeechRecognizer`.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

En este ejemplo, el lenguaje de origen y el punto de conexión personalizado se proporcionan mediante `SourceLanguageConfig`. A continuación, se pasa el método `sourceLanguageConfig` como parámetro al crear el elemento `SpeechRecognizer`.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage` y `setEndpointId` son métodos en desuso de la clase `SpeechConfig` en C++ y Java. No se recomienda el uso de estos métodos y no deben usarse al construir un `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Cómo especificar el lenguaje fuente en Python

El primer paso consiste en crear un `speech_config`:

```Python
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

A continuación, especifique el lenguaje fuente del audio con `speech_recognition_language`:

```Python
speech_config.speech_recognition_language="de-DE"
```

Si utiliza un modelo personalizado para el reconocimiento, puede especificar el punto de conexión con `endpoint_id`:

```Python
speech_config.endpoint_id = "The Endpoint ID for your custom model."
```

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>Cómo especificar el lenguaje fuente en JavaScript

El primer paso consiste en crear un `SpeechConfig`:

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

A continuación, especifique el lenguaje fuente del audio con `speechRecognitionLanguage`:

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Si utiliza un modelo personalizado para el reconocimiento, puede especificar el punto de conexión con `endpointId`:

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Cómo especificar el lenguaje fuente en Objective-C

El primer paso consiste en crear un `speechConfig`:

```Objective-C
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithSubscription:@"YourSubscriptionkey" region:@"YourRegion"];
```

A continuación, especifique el lenguaje fuente del audio con `speechRecognitionLanguage`:

```Objective-C
speechConfig.speechRecognitionLanguage = @"de-DE";
```

Si utiliza un modelo personalizado para el reconocimiento, puede especificar el punto de conexión con `endpointId`:

```Objective-C
speechConfig.endpointId = @"The Endpoint ID for your custom model.";
```

::: zone-end

## <a name="see-also"></a>Otras referencias

* Para obtener una lista de idiomas y configuraciones regionales admitidos para la conversión de voz en texto, consulte [compatibilidad de idioma](language-support.md).

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia del SDK de voz](speech-sdk.md)
