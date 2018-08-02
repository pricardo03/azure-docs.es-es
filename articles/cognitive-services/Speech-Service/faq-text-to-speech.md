---
title: Preguntas más frecuentes sobre el servicio de conversión de voz en texto en Azure | Microsoft Docs
description: Estas son las respuestas a las preguntas más habituales sobre la conversión de voz en texto.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 4a29435c0ace79fc3a5d3a5a42a0e91bdbc8da5e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082831"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Preguntas más frecuentes sobre Texto a voz

Si no puede encontrar las respuestas a sus preguntas en estas preguntas más frecuentes, formúlelas en la comunidad de Custom Speech Service en [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) y [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>General

**Pregunta**: ¿Cuál es la diferencia entre los modelos de voz estándar y los modelos de voz personalizados?

**Respuesta**: Los modelos de voz estándar (también conocidos como fuentes de voz) se han entrenado con datos propiedad de Microsoft y ya están implementados en la nube. Los modelos de voz personalizados permiten que el usuario adapte un modelo promedio y transfiera la forma de expresión y el timbre de acuerdo con el estilo de voz del orador, o bien o entrene un modelo completo nuevo en función de los datos de entrenamiento que preparó el usuario. Actualmente son cada vez más los clientes que quieren una voz única y distintiva para sus bots. La plataforma de compilación de voz personalizada es la opción correcta para eso.

**Pregunta**: ¿Dónde empiezo si quiero usar un modelo de voz estándar?

**Respuesta**: Hay más de 80 modelos de voz estándar en más de 45 idiomas disponibles a través de solicitudes HTTP. En primer lugar, es necesario obtener una [clave de suscripción](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Para realizar llamadas CALL a los modelos de voz implementados previamente, consulte [los detalles aquí](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Pregunta**: Si quiero usar un modelo de voz personalizado, ¿es la misma API que para las voces estándar?

**Respuesta**: Cuando se crea e implementa el modelo de voz personalizado, recibirá un punto de conexión único para el modelo. Deberá especificar el punto de conexión en las solicitudes HTTP con el fin de usar la voz para hablar en las aplicaciones. La misma funcionalidad disponible a través de la API de REST para el servicio Texto a voz también está disponible para el punto de conexión personalizado. Vea cómo [crear y usar el punto de conexión personalizado](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Pregunta**: ¿Es necesario preparar los datos de entrenamiento para crear modelos de voz personalizados por mi cuenta?

**Respuesta**: Deberá preparar los datos de entrenamiento por su cuenta. Se requiere una colección de datos de voz para crear un modelo de voz personalizado. Esta colección se compone de un conjunto de archivos de audio de las grabaciones de voz y un archivo de texto de la transcripción de cada uno de los archivos de audio. El resultado de su voz digital se basa principalmente en la calidad de los datos de entrenamiento. Para generar una buena voz para Texto a voz, es importante que las grabaciones se realicen en una sala silenciosa con un micrófono de pedestal de alta calidad. El volumen constante, la velocidad de la conversación, el tono al hablar e incluso la coherencia en las particularidades expresivas del habla son esenciales para compilar una gran voz digital. Se recomienda grabar las voces en un estudio de grabación.
Por el momento, no proporcionamos compatibilidad con la grabación en línea ni tenemos ninguna recomendación sobre estudios de grabación. Si quiere conocer el requisito de formato, consulte [cómo preparar las grabaciones y transcripciones](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts).
 
**Pregunta**: ¿Qué guiones debo usar para grabar los datos de voz para el entrenamiento de voz personalizado? 

**Respuesta**: No ponemos límite a los guiones para las grabaciones de voz. Puede usar sus propios guiones para grabar el discurso. Solo debe asegurarse de tener la cobertura fonética suficiente en los datos de voz. Para entrenar una voz personalizada, puede empezar con un volumen pequeño de datos de voz, que podrían ser 50 frases distintas (unos 3 a 5 minutos de habla). Cuantos más datos proporcione, más natural será su voz. Para empezar a entrenar una fuente de voz completa, puede proporcionar grabaciones de más de 2000 frases (unas 3 a 4 horas de habla). Para obtener una voz completa de alta calidad, deberá preparar grabaciones de más de 6000 frases (unas 8 a 10 horas de habla).  
Proporcionamos servicios adicionales para ayudarlo a preparar guiones para la grabación. Póngase en contacto con la [asistencia al cliente de Voz personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) en caso de consultas.

**Pregunta**: ¿Qué pasa si necesito más simultaneidad que el valor predeterminado o la que se ofrece en el portal?

**Respuesta**: Se puede ampliar el modelo en incrementos de 20 solicitudes simultáneas. Póngase en contacto con la [asistencia al cliente de Voz personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) en caso de consultas sobre un mayor escalamiento.

**Pregunta**: ¿Puedo descargar mi modelo y ejecutarlo localmente?

**Respuesta**: Los modelos no se pueden descargar y ejecutar localmente.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas](troubleshooting.md)
* [Notas de la versión](releasenotes.md)
