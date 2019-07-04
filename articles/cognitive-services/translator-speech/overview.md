---
title: ¿Qué es el servicio Translator Speech?
titleSuffix: Azure Cognitive Services
description: Use la API del servicio Translator Speech para la conversión de voz en voz y de voz en texto en sus aplicaciones.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: overview
ms.date: 3/5/2018
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ed425cd6545d85cdf3b2f76038b381a0f09045f9
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449650"
---
# <a name="what-is-translator-speech-api"></a>¿Qué es Translator Speech API?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Translator Speech API se puede utilizar para agregar conversión de voz en tiempo real a las aplicaciones, herramientas o cualquier solución que requiera conversión de voz en varios idiomas, independientemente del sistema operativo de destino o de los lenguajes de desarrollo. La API puede usarse tanto para traducción de voz a voz como de voz a texto.

Translator Text API es un servicio de Azure que forma parte de [la colección de API Microsoft Cognitive Services](https://docs.microsoft.com/azure/), una colección de algoritmos de aprendizaje automático y de inteligencia artificial en la nube, que se pueden consumir fácilmente en los proyectos de desarrollo.

Con Translator Speech API, las aplicaciones cliente transmiten el audio de voz al servicio y reciben unos resultados de texto y audio, que incluyen el texto reconocido en el idioma de origen y su conversión al idioma de destino. Para producir los resultados, se aplica el reconocimiento de voz automático (ASR) con la tecnología de redes neuronales profundas a la secuencia de audio entrante. La salida sin formato del reconocimiento de voz automático se mejora aún más mediante una técnica nueva denominada TrueText, para reflejar mejor la intención del usuario. Por ejemplo, TrueText quita las disfluencias (hum o tos), palabras repetidas y restablece la puntuación y el uso adecuado de mayúsculas y minúsculas. También existe la posibilidad de enmascarar o excluir palabras soeces. Los motores de reconocimiento y traducción están entrenados específicamente para controlar la voz conversacional.

El servicio Translator Speech utiliza la detección de silencios para determinar el final de una expresión. Después de una pausa en la actividad de voz, el servicio devuelve el resultado final de la expresión completa. El servicio también puede devolver resultados parciales, lo que proporciona los reconocimientos y las traducciones intermedios de una expresión en curso.

Para la traducción de voz a voz, el servicio permite sintetizar la voz (texto a voz) del texto hablado en los idiomas de destino. El audio de texto a voz se crea en el formato especificado por el cliente. Están disponibles los formatos WAV y MP3.

Translator Speech API usa el protocolo WebSocket para proporcionar un canal de comunicación dúplex completo entre el cliente y el servidor.

## <a name="about-microsoft-translator"></a>Acerca de Microsoft Translator
Microsoft Translator es un servicio de traducción automática basado en la nube. En el centro de este servicio están Translator Text API y [Translator Speech API](https://www.microsoft.com/en-us/translator/translatorapi.aspx), que dan servicio a diversos productos y servicios de Microsoft y que utilizan miles de empresas en todo el mundo en sus aplicaciones y flujos de trabajo para que su contenido llegue a una audiencia mundial.

Obtenga más información sobre el [servicio Microsoft Translator](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="microsoft-translator-neural-machine-translation-nmt"></a>Traducción automática neuronal de Microsoft Translator (NMT)
Microsoft Translator Speech API usa la traducción automática estadística (SMT) y la traducción automática neuronal (NMT) más reciente para las conversiones.

La traducción automática estadística ha alcanzado un nivel estable en cuanto a la mejora de su rendimiento. La calidad de la traducción ya no mejora de forma significativa en sistemas genéricos con SMT. Ahora, está tomando impulso una nueva tecnología de traducción basada en inteligencia artificial que utiliza redes neuronales (NN).

NMT proporciona mejores traducciones, no solo desde un punto de vista de la calidad de la traducción sino también porque suena más fluida y más humana que las traducciones SMT.
El motivo principal de esta fluidez es que NMT usa el contexto completo de una frase para traducir las palabras. SMT solo toma el contexto inmediato de unas pocas palabras antes y después de cada palabra.

Los modelos NMT son el núcleo de la API y no son visibles para los usuarios finales. Las únicas diferencias notables son:
* La mejor calidad de la traducción, especialmente en idiomas como chino, japonés y árabe.
* La incompatibilidad con las características de personalización de centros existentes (para su uso con Microsoft Translator Text API).

Todos los idiomas de traducción de voz admitidos utilizan NMT. Por lo tanto, todas las traducciones de voz a voz utilizan NMT.

La traducción de voz a texto puede usar una combinación de NMT y SMT según el par de idiomas. Si el idioma de destino admite NMT, la traducción completa se realiza con NMT. Si el idioma de destino no admite NMT, la traducción es un híbrido de NMT y SMT, con el idioma inglés como "enlace" entre los dos idiomas.

Consulte los idiomas admitidos en [Microsoft.com](https://www.microsoft.com/en-us/translator/languages.aspx).

Más información sobre [el funcionamiento de NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Sign up](translator-speech-how-to-signup.md)

> [!div class="nextstepaction"]
> [Empezar con la codificación](quickstarts/csharp.md)

## <a name="see-also"></a>Otras referencias
- [Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/)
- [Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
- [Información de precios y soluciones](https://www.microsoft.com/en-us/translator/home.aspx)
