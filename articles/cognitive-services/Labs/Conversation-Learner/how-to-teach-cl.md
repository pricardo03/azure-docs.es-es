---
title: 'Cómo enseñar con Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo enseñar con Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8c55bb27ce5a413c5ceae22371ad61a5acf47281
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322896"
---
# <a name="how-to-teach-with-conversation-learner"></a>Enseñar con Conversation Learner 

En este documento se explica qué señales conoce Conversation Learner y se describe cómo aprende.  

El aprendizaje puede dividirse en dos pasos distintos: extracción de entidades y selección de acciones.

## <a name="entity-extraction"></a>Extracción de entidades

En segundo plano, Conversation Learner usa [LUIS](https://www.luis.ai) para la extracción de entidades.  Si está familiarizado con LUIS, esa experiencia se aplica a la extracción de entidades en Conversation Learner.

Los modelos de extracción de entidades conocen el *contenido* y el *contexto* de la expresión de un usuario.  Por ejemplo, si la palabra "Seattle" se ha etiquetado como una ciudad en una expresión como "What's the weather in Seattle" (¿Qué tiempo hace en Seattle?), la extracción de entidades es capaz de reconocer ese mismo contenido ("Seattle") como una ciudad en otra expresión, como "Population of Seattle" (Población de Seattle), aunque las expresiones sean muy diferentes.  Por el contrario, si "Francis" se ha reconocido como un nombre en "Schedule a meeting with Francis" (Programar una reunión con Francis), entonces se puede reconocer un nuevo nombre nunca visto previamente en un contexto similar, como "Set up a meeting with Robin" (Programar una reunión con Robin).  El aprendizaje automático deduce cuándo ocuparse de contenido, de contexto o de ambos, en función de los ejemplos de entrenamiento.

En la actualidad, la extracción de entidades solo es consciente del contenido de la expresión actual.  A diferencia de la selección de acciones (a continuación), no es consciente del historial de diálogos, como los turnos anteriores del sistema, los turnos anteriores de los usuarios o las entidades reconocidas anteriormente.  Como resultado, el comportamiento de la extracción de entidades es "compartido" entre todas las expresiones.  Por ejemplo, si la expresión del usuario "I want Apple" has "Apple" (Quiero una manzana) tiene "Apple" (Manzana) como un tipo de entidad "Fruit" (Fruta) en la expresión de un usuario, el modelo de extracción de entidades esperará que esta expresión ("I want Apple") siempre tenga "Apple" etiquetado como "Fruit".

Si la extracción de entidades no se comporta según lo esperado, aquí se presentan posibles soluciones:

- Lo primero es intentar agregar más ejemplos de entrenamiento, en particular ejemplos que revelen el contexto de entidad típico (palabras relacionadas) o excepciones.
- Considere la posibilidad de agregar la propiedad "Entidad esperada" a una acción, si procede.  Consulte el tutorial sobre entidades esperadas para obtener más información.
- Aunque es posible agregar procesamiento manual a `EntityExtractionCallback` para extraer entidades mediante código, este es el enfoque menos recomendado porque no podrá aprovechar las mejoras del aprendizaje automático a medida que el sistema evoluciona.

## <a name="action-selection"></a>Selección de acciones

La selección de acciones usa una red neural recurrente, que adopta como entrada todo el historial de conversaciones.  Por tanto, la selección de acciones es un proceso con estado que conoce las expresiones de usuario anteriores, los valores de entidad y las expresiones del sistema.  

El proceso de aprendizaje naturalmente prefiere algunas señales.  En otras palabras, si Conversation Learner puede explicar una decisión de selección de acciones con señales "más preferidas", lo hará; en caso de que no pueda, usará las señales "menos preferidas".

A continuación se presenta una tabla en la que se muestran todas las señales de Conversation Learner y las que se usan en la selección de acciones.  Tenga en cuenta que se ignora el orden de palabras en las expresiones de usuario.

Señal | Preferencia (1=más preferido) | Notas
--- | --- | --- 
Acción del sistema en el turno anterior | 1 | 
Entidades presentes en el turno actual | 1 | 
Si se trata del primer turno | 1 |
Coincidencia exacta de palabras en la expresión de usuario actual | 2 | 
Palabras con significado similar en la expresión de usuario actual | 3 | 
Acciones del sistema antes del turno anterior | 4 |
Entidades presentes en turnos antes del turno actual | 4 | 
Expresiones de usuario antes del turno actual | 5 | 

> [!NOTE]
> La selección de acciones no adopta el contenido de las acciones del sistema (texto, contenido de tarjeta o nombre o comportamiento de la API), solo la identidad de la acción del sistema.  Como resultado, el cambio de contenido de una acción no alterará el comportamiento del modelo de selección de acciones.
>
> Además, el contenido o los valores de las entidades no se usan, sino solo su presencia/ausencia.

Si la selección de acciones no se comporta según lo esperado, aquí se presentan posibles soluciones:

- Agregue más diálogos de entrenamiento, en particular diálogos que ilustren a qué selección de acciones de señal hay que prestar atención.  Por ejemplo, si la selección de acciones debe preferir una señal frente a otra, proporcione ejemplos que muestren la señal preferida en el mismo estado y el resto de señales que varían.  Algunas secuencias pueden necesitar algunos diálogos de entrenamiento para aprender.
- Agregue las entidades "Required" (Obligatoria) y "Disqualifying" (Descalificadora) a las definiciones de acciones.  Esto limita la disponibilidad de las acciones y puede resultar útil para expresar reglas de negocio y algunos patrones de sentido común. 

## <a name="updates-to-models"></a>Actualización de modelos

Cada vez que agrega o edita una entidad, acción o diálogo de entrenamiento en la interfaz de usuario, genera una solicitud para volver a entrenar tanto el modelo de extracción de entidades como el de selección de acciones.  Esta solicitud se coloca en una cola, y el nuevo entrenamiento se realiza de forma asincrónica.  Cuando hay un nuevo modelo disponible, se usa a partir de ese punto para la extracción de entidades y la selección de acciones.  A menudo, este proceso de nuevo entrenamiento tarda unos 5 segundos, pero puede tardar incluso más si el modelo es complejo o si la carga del servicio de entrenamiento es alta.

Dado que el entrenamiento se realiza de forma asincrónica, es posible que las ediciones realizadas no se reflejen de inmediato.  Si la extracción de entidades o la selección de acciones no se comportan según lo esperado según los cambios realizados en los últimos 5 a 10 segundos, la causa puede ser esta.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Límites y valores predeterminados](./cl-values-and-boundaries.md)
