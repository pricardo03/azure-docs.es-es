---
title: Introducción a Custom Speech Service en Azure | Microsoft Docs
description: Custom Speech Service es un servicio basado en la nube que permite a los usuarios personalizar los modelos de voz para la transcripción de voz a texto.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: a6139283a555f8f97371c02f9d1f3d53dc6f15d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381026"
---
# <a name="what-is-custom-speech-service"></a>¿Qué es Custom Speech Service?

Custom Speech Service es un servicio basado en la nube que ofrece a los usuarios la posibilidad de personalizar los modelos de voz para la transcripción de voz a texto.
Para usar Custom Speech Service, consulte el [portal de Custom Speech Service](https://cris.ai).

Custom Speech Service le permite crear modelos de lenguaje y modelos acústicos personalizados adaptados a sus aplicaciones y usuarios. Al cargar datos de texto o voz específicos en Custom Speech Service, puede crear modelos personalizados que se pueden usar en combinación con los modelos de voz avanzados existentes de Microsoft.

Por ejemplo, si va a agregar interacción de voz a un teléfono móvil, tableta o aplicación de PC, puede crear un modelo de lenguaje personalizado que se puede combinar con el modelo acústico de Microsoft para crear un punto de conexión de voz a texto diseñado especialmente para la aplicación. Si la aplicación está diseñada para su uso en un entorno determinado o para una población de usuarios determinada, también puede crear e implementar un modelo acústico personalizado con este servicio.


## <a name="how-do-speech-recognition-systems-work"></a>¿Cómo funcionan los sistemas de reconocimiento de voz?
Los sistemas de reconocimiento de voz constan de varios componentes que funcionan conjuntamente. Dos de los componentes más importantes son el modelo acústico y el modelo de lenguaje.

El modelo acústico es un clasificador que etiqueta fragmentos cortos de audio en uno de una serie de fonemas, o unidades de sonido, en un idioma determinado. Por ejemplo, la palabra "voz" se compone de tres fonemas: "b o z". Estas clasificaciones se llevan a cabo a razón de 100 veces por segundo.

El modelo de lenguaje es una distribución de probabilidad en secuencias de palabras. Ayuda al sistema a decidir entre secuencias de palabras que suenan de forma parecida, en función de la probabilidad de las propias secuencias de palabras. Por ejemplo, "bolsa de patatas fritas" y "bolsa de batatas fritas" suenan muy parecido, pero es mucho más probable la primera que la segunda y, por tanto, el modelo de lenguaje le asignará una puntuación más alta.

Los modelos acústico y de lenguaje son modelos estadísticos aprendidos de los datos de entrenamiento. Como resultado, funcionan mejor cuando la voz que encuentran cuando se usan en las aplicaciones es parecida a los datos observados durante el entrenamiento. Los modelos acústico y de lenguaje del motor Speech to Text de Microsoft se han entrenado en una enorme colección de voz y texto y proporcionan un rendimiento avanzado en los escenarios de uso más comunes, como la interacción con Cortana en un smartphone, tableta o PC, la búsqueda en la Web mediante voz o el dictado de mensajes de texto a un amigo.

## <a name="why-use-the-custom-speech-service"></a>¿Por qué usar Custom Speech Service?
Aunque el motor Speech To Text de Microsoft es de primera clase, va dirigido a los escenarios descritos anteriormente. No obstante, si cree que las consultas de voz que va a recibir su aplicación contendrán términos específicos, como nombres de producto o jerga que no suelen utilizarse en una conversación normal, es probable que obtenga un rendimiento mayor si personaliza el modelo de lenguaje.

Por ejemplo, si fuera a crear una aplicación que realiza búsquedas en MSDN mediante voz, es probable que términos como "orientado a objetos", "espacio de nombres" o "punto net" aparezcan con más frecuencia que en aplicaciones de voz normales. Al personalizar el modelo de lenguaje, se consigue que el sistema aprenda esto.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar Custom Speech Service, consulte el [portal de Custom Speech Service] (https://cris.ai).

* [Primeros pasos](cognitive-services-custom-speech-get-started.md)
* [P+F](cognitive-services-custom-speech-faq.md)
* [Glosario](cognitive-services-custom-speech-glossary.md)
