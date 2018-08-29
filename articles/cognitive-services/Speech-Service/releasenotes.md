---
title: Documentación del SDK de Voz de Cognitive Services | Microsoft Docs
description: 'Notas de la versión: qué ha cambiado en las versiones más recientes'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 08/16/2018
ms.author: wolfma
ms.openlocfilehash: 0900588f818855d72e415678338c96fb6505318d
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41929925"
---
# <a name="release-notes"></a>Notas de la versión

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>SDK de Voz de Cognitive Services 0.6.0: versión de agosto de 2018

**Nuevas características:**

* Ahora, las aplicaciones de UWP creadas con el SDK de Voz superan el Kit para la certificación de aplicaciones en Windows (WACK).
  Consulte nuestro [inicio rápido de UWP](quickstart-csharp-uwp.md).
* Compatibilidad con .NET Standard 2.0 en Linux (Ubuntu 16.04 x64).
* Experimental: Compatibilidad con Java 8 en Windows (64 bits) y Linux (Ubuntu 16.04 x 64).
  Consulte la [guía de inicio rápido del entorno en tiempo de ejecución de Java](quickstart-java-jre.md)

**Cambios funcionales**

* Se expone más información detallada sobre los errores de conexión.

**Cambios importantes**

* En Java (Android), la función `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` ya no requiere un parámetro de ruta de acceso. Ahora, la ruta de acceso se detecta automáticamente en todas las plataformas compatibles.
* En Java y C#, se ha quitado el descriptor de acceso get- de la propiedad `EndpointUrl`.

**Correcciones de errores**

* En Java, se ha implementado la síntesis de audio como resultado del reconocedor de traducción.
* Se ha corregido un error que podía provocar subprocesos inactivos y un mayor número de sockets abiertos y sin usar.
* Se ha corregido un problema por el que un proceso de reconocimiento de larga ejecución podía terminar en mitad de la transmisión.
* Se ha corregido una condición de carrera en el proceso de apagado del reconocedor.

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
