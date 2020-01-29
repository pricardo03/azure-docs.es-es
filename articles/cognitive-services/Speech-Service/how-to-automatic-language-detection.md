---
title: Uso de detección automática de idioma para la conversión de voz en texto
titleSuffix: Azure Cognitive Services
description: El SDK de Voz admite la detección automática de idioma para la conversión de voz en texto. Al usar esta característica, el audio proporcionado se compara con una lista de idiomas proporcionada y se determina la coincidencia más probable. Después, el valor devuelto se puede usar para seleccionar el modelo de lenguaje usado para la conversión de voz en texto.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-seven
ms.openlocfilehash: bc438c3e606fefc10e9ffbb64c79f7167d9af062
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122056"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Detección automática de idioma para la conversión de voz en texto

La detección automática de idioma se usa para determinar la coincidencia más probable para el audio que se pasa al SDK de Voz cuando se compara con una lista de idiomas proporcionados. El valor devuelto por la detección automática de idioma se usa para seleccionar el modelo de idioma para la conversión de voz en texto, lo que le proporciona una transcripción más precisa. Para ver los idiomas que están disponibles, consulte la [compatibilidad de idioma](language-support.md).

En este artículo, aprenderá a usar `AutoDetectSourceLanguageConfig` para construir un objeto `SpeechRecognizer` y recuperar el idioma detectado.

> [!IMPORTANT]
> Esta característica solo está disponible para el SDK de Voz de C#, C++ y Java.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Detección automática de idioma con el SDK de Voz

La detección automática de idioma actualmente tiene un límite de servicio de dos idiomas por detección. Tenga en cuenta esta limitación al construir el objeto `AudoDetectSourceLanguageConfig`. En los ejemplos siguientes, creará un objeto `AutoDetectSourceLanguageConfig` y lo usará para construir un `SpeechRecognizer`.

> [!TIP]
> También puede especificar un modelo personalizado para utilizarlo al realizar la conversión de voz en texto. Para obtener más información, consulte [Uso de un modelo personalizado para la detección automática de idioma](#use-a-custom-model-for-automatic-language-detection).

En los fragmentos de código siguientes se muestra cómo usar la detección automática de idioma en las aplicaciones:

::: zone pivot="programming-language-csharp"

```csharp
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromLanguages(new string[] { "en-US", "de-DE" });
using (var recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig))
{
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.FromResult(speechRecognitionResult);
    var detectedLanguage = autoDetectSourceLanguageResult.Language;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
```

::: zone-end

## <a name="use-a-custom-model-for-automatic-language-detection"></a>Uso de un modelo personalizado para la detección automática de idioma

Además de la detección de idiomas mediante los modelos del servicio de Voz, puede especificar un modelo personalizado para obtener reconocimiento mejorado. Si no se proporciona un modelo personalizado, el servicio utilizará el modelo de lenguaje predeterminado.

En los fragmentos de código siguientes se muestra cómo especificar un modelo personalizado en la llamada al servicio de Voz. Si el idioma detectado es `en-US`, se usa el modelo predeterminado. Si el idioma detectado es `fr-FR`, se usa el punto de conexión para el modelo personalizado:

::: zone pivot="programming-language-csharp"

```csharp
var sourceLanguageConfigs = new SourceLanguageConfig[]
{
    SourceLanguageConfig.FromLanguage("en-US"),
    SourceLanguageConfig.FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR")
};
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

- [Documentación de referencia del SDK de voz](speech-sdk.md)
