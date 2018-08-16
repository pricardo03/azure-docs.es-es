---
title: Documentación de Speech SDK de Cognitive Services
description: 'Notas de la versión: qué ha cambiado en las versiones más recientes'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: wolfma
ms.openlocfilehash: f346241e1d10d16eae08e389296f4be9149ec086
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502465"
---
# <a name="release-notes"></a>Notas de la versión

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de Voz de Cognitive Services 0.5.0: versión de julio de 2018

**Nuevas características:**

* Compatibilidad con la plataforma Android (API 23: Android 6.0 Marshmallow o superior).
  Consulte el [inicio rápido de Android](quickstart-java-android.md).
* Compatibilidad con .NET Standard 2.0 en Windows.
  Consulte el [inicio rápido de .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Experimental: compatibilidad con UWP en Windows (versión 1709 o posterior)
  * Consulte nuestro [inicio rápido de UWP](quickstart-csharp-uwp.md).
  * Nota: Las aplicaciones de UWP creadas con el SDK de Voz no pasan aún el Kit para la certificación de aplicaciones en Windows (WACK).
* Compatibilidad con reconocimiento de ejecución prolongada con reconexión automática.

**Cambios funcionales**

* `StartContinuousRecognitionAsync()` admite reconocimiento de ejecución prolongada
* El resultado del reconocimiento contiene más campos: desplazamiento desde el principio del audio y duración (ambos en tics) del texto reconocido, valores adicionales que representan el estado de reconocimiento, p. ej., `InitialSilenceTimeout`, `InitialBabbleTimeout`, etc.
* Compatibilidad con AuthorizationToken para la creación de instancias de fábrica.

**Cambios importantes**

* Eventos de reconocimiento: el tipo de evento NoMatch se combina con el evento Error.
* SpeechOutputFormat en C# se llama ahora OutputFormat para concordar con C++.
* El tipo de valor devuelto de algunos métodos de la interfaz `AudioInputStream` se ha modificado ligeramente:
   * En Java, el método `read` ahora devuelve `long` en lugar de `int`.
   * En C#, el método `Read` ahora devuelve `uint` en lugar de `int`.
   * En C++, los métodos `Read` y `GetFormat` ahora devuelven `size_t` en lugar de `int`.
* C++: las instancias de flujos de entrada de audio ahora solo se pueden pasar como `shared_ptr`.

**Correcciones de errores**

* Se han corregido los valores devueltos incorrectos cuando se agota el tiempo de espera de `RecognizeAsync()`.
* Se ha quitado la dependencia en las bibliotecas de Media Foundation en Windows. El SDK ahora usa las API de Core Audio.
* Corrección de documentación: se ha agregado una página de regiones para describir cuáles son las regiones admitidas.

**Problemas conocidos**

* El SDK de Voz para Android no informa de los resultados de síntesis de voz para la conversión.
  Este problema se solucionará en la próxima versión.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de Voz 0.4.0 de Cognitive Services: versión de junio de 2018

**Cambios funcionales**

- AudioInputStream

  Ahora un reconocedor puede usar una secuencia como el origen del audio. Para obtener información detallada, vea la [guía de procedimientos](how-to-use-audio-input-streams.md) relacionada.

- Formato de salida detallado

  Al crear un `SpeechRecognizer`, puede solicitar el formato de salida `Detailed` o `Simple`. `DetailedSpeechRecognitionResult` contiene una puntuación de confianza, texto reconocido, formato léxico sin formato, formato normalizado y formato normalizado con palabras soeces enmascaradas.

**Cambio importante**

- En C# se cambia de `SpeechRecognitionResult.RecognizedText` a `SpeechRecognitionResult.Text`.

**Correcciones de errores**

- Se ha corregido un posible problema de devolución de llamada en la capa USP durante el cierre.

- Si un reconocedor usaba un archivo de entrada de audio, mantenía el identificador de archivo más tiempo del necesario.

- Se han quitado varios interbloqueos entre el suministro de mensajes y el reconocedor.

- Se desencadena un resultado `NoMatch` cuando se agota la respuesta del servicio.

- Las bibliotecas de Media Foundation en Windows son de carga retrasada. Esta biblioteca solo es necesaria para la entrada del micrófono.

- La velocidad de carga de los datos de audio se limita al doble de la velocidad de audio original.

- En Windows, los ensamblados .NET de C# son ahora de nombre seguro.

- Corrección de la documentación: `Region` necesita información para crear un reconocedor.

Se han agregado más ejemplos y se actualizan constantemente. Para obtener el conjunto más reciente de ejemplos, vea el [repositorio de GitHub de ejemplos del SDK de Voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de Voz de Cognitive Services 0.2.12733: versión de mayo de 2018

La primera versión preliminar pública del SDK de Voz de Cognitive Services.
