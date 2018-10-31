---
title: Documentación del SDK del servicio Voz
titlesuffix: Azure Cognitive Services
description: 'Notas de la versión: qué ha cambiado en las versiones más recientes'
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: wolfma
ms.openlocfilehash: b5e4d239121b2449a45dfce826c99765f1c3f4be
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471148"
---
# <a name="release-notes"></a>Notas de la versión

## <a name="speech-service-sdk-101"></a>SDK del servicio Voz 1.0.1

Mejoras en la confiabilidad y correcciones de errores:

* Corrección de un potencial error grave debido a una condición de carrera al desechar un reconocedor
* Corrección de un potencial error grave en el caso de propiedades sin establecer.
* Comprobación adicional de errores y parámetros.
* Objective-C: corrección de posibles errores graves causados por la invalidación de nombres en NSString.
* Objective-C: ajuste de visibilidad en la API.
* JavaScript: corrección con respecto a los eventos y sus cargas.
* Mejoras en la documentación.

Se ha agregado un nuevo ejemplo de Javascript en nuestro [repositorio de ejemplos](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>SDK 1.0.0 de Voz de Cognitive Services: versión de septiembre de 2018

**Nuevas características:**

* Compatibilidad con Objective-C en iOS. Consulte la [Guía de inicio rápido de Objective-C para iOS](quickstart-objectivec-ios.md).
* Se admite JavaScript en el explorador. Consulte la [Guía de inicio rápido de JavaScript](quickstart-js-browser.md).

**Cambios importantes**

* Con esta versión se presentan una serie de cambios importantes.
  Consulte [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para más información.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>SDK de Voz de Cognitive Services 0.6.0: versión de agosto de 2018

**Nuevas características:**

* Ahora, las aplicaciones de UWP creadas con SDK de Voz superan el Kit para la certificación de aplicaciones en Windows (WACK).
  Consulte la [Guía de inicio rápido de UWP](quickstart-csharp-uwp.md).
* Compatibilidad con .NET Standard 2.0 en Linux (Ubuntu 16.04 x64).
* Experimental: Compatibilidad con Java 8 en Windows (64 bits) y Linux (Ubuntu 16.04 x 64).
  Consulte la [Guía de inicio rápido de Java Runtime Environment](quickstart-java-jre.md).

**Cambios funcionales**

* Se expone más información detallada sobre los errores de conexión.

**Cambios importantes**

* En Java (Android), la función `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` ya no requiere un parámetro de ruta de acceso. Ahora, la ruta de acceso se detecta automáticamente en todas las plataformas compatibles.
* En Java y C#, se ha quitado el descriptor de acceso get- de la propiedad `EndpointUrl`.

**Correcciones de errores**

* En Java, se implementa ahora el resultado de la síntesis de audio en el reconocedor de traducción.
* Se ha corregido un error que podía provocar subprocesos inactivos y un mayor número de sockets abiertos y sin usar.
* Se ha corregido un problema por el que un proceso de reconocimiento de larga ejecución podía terminar en mitad de la transmisión.
* Se ha corregido una condición de carrera en el proceso de apagado del reconocedor.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de Voz de Cognitive Services 0.5.0: versión de julio de 2018

**Nuevas características:**

* Compatibilidad con la plataforma Android (API 23: Android 6.0 Marshmallow o superior). Consulte el [inicio rápido de Android](quickstart-java-android.md).
* Compatibilidad con .NET Standard 2.0 en Windows. Consulte el [inicio rápido de .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Experimental: compatibilidad con UWP en Windows (versión 1709 o posterior).
  * Consulte la [Guía de inicio rápido de UWP](quickstart-csharp-uwp.md).
  * Nota: Las aplicaciones de UWP creadas con el SDK de Voz no pasan aún el Kit para la certificación de aplicaciones en Windows (WACK).
* Compatibilidad con el reconocimiento de ejecución prolongada con reconexión automática.

**Cambios funcionales**

* `StartContinuousRecognitionAsync()` admite reconocimiento de ejecución prolongada.
* El resultado del reconocimiento contiene más campos. Tienen un desplazamiento desde el principio del audio y la duración (ambos en tics) del texto reconocido y valores adicionales que representan el estado de reconocimiento, por ejemplo, `InitialSilenceTimeout` e `InitialBabbleTimeout`.
* Compatibilidad con AuthorizationToken para la creación de instancias de fábrica.

**Cambios importantes**

* Eventos de reconocimiento: el tipo de evento NoMatch se combina con el evento Error.
* SpeechOutputFormat en C# se llama ahora OutputFormat para concordar con C++.
* El tipo de valor devuelto de algunos métodos de la interfaz `AudioInputStream` se ha modificado ligeramente:
   * En Java, el método `read` ahora devuelve `long` en lugar de `int`.
   * En C#, el método `Read` ahora devuelve `uint` en lugar de `int`.
   * En C++, los métodos `Read` y `GetFormat` ahora devuelven `size_t` en lugar de `int`.
* C++: las instancias de secuencias de entrada de audio ahora solo se pueden pasar como un valor `shared_ptr`.

**Correcciones de errores**

* Se han corregido los valores devueltos incorrectos cuando se agota el tiempo de espera de `RecognizeAsync()`.
* Se ha eliminado la dependencia de las bibliotecas de Media Foundation en Windows. El SDK ahora usa las API de audio básicas.
* Corrección de documentación: se ha agregado una página de [regiones](regions.md) para describir cuáles son las regiones admitidas.

**Problema conocido**

* SDK de Voz para Android no informa de los resultados de la síntesis de voz para la traducción. Este problema se solucionará en la próxima versión.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de Voz 0.4.0 de Cognitive Services: versión de junio de 2018

**Cambios funcionales**

- AudioInputStream

  Un reconocedor ahora puede consumir una secuencia como origen de audio. Para más información, consulte la [guía de procedimientos](how-to-use-audio-input-streams.md) relacionada.

- Formato de salida detallado

  Al crear un elemento `SpeechRecognizer`, puede solicitar el formato de salida `Detailed` o `Simple`. `DetailedSpeechRecognitionResult` contiene una puntuación de confianza, texto reconocido, formato léxico sin formato, formato normalizado y formato normalizado con palabras soeces enmascaradas.

**Cambio importante**

- En C# se cambia de `SpeechRecognitionResult.RecognizedText` a `SpeechRecognitionResult.Text`.

**Correcciones de errores**

- Se ha corregido un posible problema de devolución de llamada en la capa USP durante el apagado.

- Si un reconocedor usaba un archivo de entrada de audio, mantenía el identificador de archivo más tiempo del necesario.

- Se han eliminado varios interbloqueos entre el suministro de mensajes y el reconocedor.

- Se desencadena un resultado `NoMatch` cuando se agota la respuesta del servicio.

- Las bibliotecas de Media Foundation en Windows son de carga retrasada. Esta biblioteca solo es necesaria para la entrada del micrófono.

- La velocidad de carga de los datos de audio se limita al doble de la velocidad de audio original.

- En Windows, los ensamblados .NET de C# ahora son de nombre seguro.

- Corrección de la documentación: `Region` necesita información para crear un reconocedor.

Se han agregado más ejemplos y se actualizan constantemente. Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos de SDK de Voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de Voz de Cognitive Services 0.2.12733: versión de mayo de 2018

Esta versión es la primera versión preliminar pública de SDK de Voz de Cognitive Services.
