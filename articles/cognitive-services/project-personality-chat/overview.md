---
title: ¿Qué es Project Personality Chat?
titlesuffix: Azure Cognitive Services
description: Este artículo es una introducción a Project Personality Chat de Azure, una API basada en la nube para mejorar las funcionalidades de conversación del bot.
services: cognitive-services
author: noellelacharite
manager: nitinme
ms.service: cognitive-services
ms.subservice: personality-chat
ms.topic: overview
ms.date: 05/07/2018
ms.author: nolachar
comment: As a bot developer, I want my bot to be able to handle small talk in a consistent tone so that my bot appears more complete and conversational.
ms.openlocfilehash: c7f7a8c65717acd5a19e92b7e0437dc4b8628909
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "62103641"
---
# <a name="what-is-project-personality-chat"></a>¿Qué es Project Personality Chat?

Project Personality Chat mejora las funcionalidades de conversación del bot controlando la conversación en línea con una personalidad elegida y única. Personality Chat utiliza clasificadores de intención para identificar las intenciones de conversación comunes y genera respuestas coherentes con una personalidad. Según la intención y las puntuaciones de confianza, el bot elige la mejor respuesta de una base editorial protegida o genera una en tiempo real mediante redes neuronales profundas (DNN). Puede elegir entre tres roles predeterminados. El modelo de rol devuelve las respuestas que son más parecidas a la personalidad elegida.

Está disponible una editorial personalizable establecida para las consultas de conversación comunes. Se puede integrar fácilmente con el SDK de Microsoft Bot Framework. Este SDK le ahorra el tiempo y el costo de escribirlos desde cero.

## <a name="getting-started-with-project-personality-chat"></a>Introducción a Project Personality Chat

Puede visitar la página de laboratorios de Project Personality Chat y chatear con la demostración disponible, así como solicitar acceso anticipado cuando el servicio esté disponible.
Actualmente, también puede integrar la biblioteca de solo editorial personalizable con los bots a través del SDK de Microsoft Bot Framework. <br>
[Ejemplos: Integración de Personality Chat en un bot](https://github.com/Microsoft/BotBuilder-PersonalityChat/) <br>
[Pruebe la biblioteca de Personality Chat](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp)

## <a name="generating-responses-using-neural-networks"></a>Generación de respuestas mediante redes neuronales

Personality Chat usa modelos de aprendizaje profundo para aprender patrones generales de conversación y generar respuestas. El modelo de generación de respuestas es una red neuronal recurrente (RNN). Este modelo se entrena con millones de conversaciones, a través de las que comprende y aprende patrones de interacciones humanas. Con los patrones de estructura de frases aprendidas, se crean nuevas consultas y se generan respuestas. Cuando se genera esta nueva respuesta, el modelo busca a través de un "vocabulario de escritura" de palabras y elige las primeras mejores palabras para la respuesta. A través de la búsqueda de transferencia, sigue buscando las segundas mejores palabras para cada una de las primeras palabras generadas. Una respuesta se considera completada cuando el modelo elige la palabra "Final de la frase". Una vez que dispone de todas las respuestas, elige las mejores según la puntuación de probabilidad para la respuesta completa.

El modelo aprende a generar contextualmente giros adecuados que tienen la "estructura" correcta. Por ejemplo, una pregunta como "¿Quiere hablar ahora?" proporciona un poco de información sobre la estructura de una respuesta razonable: probablemente empezará con algunas paráfrasis de "Sí" o "No" y, posiblemente, el pronombre "yo". La respuesta "No" es más probable que incluya una explicación formal, y así sucesivamente.

El sistema intenta aprender un modelo de lenguaje para la estructura básica de la conversación. Esta estructura debe permitir que restricciones externas, como temas, personalidad, etc., influyan en parte en las respuestas.  Dado que estas restricciones se extraen de los patrones generales, son adecuadas, entre otras, para la aplicación de conversación.

![Modelo de secuencia a secuencia para la generación de respuestas](./media/overview/sequence-to-sequence-model.png)

## <a name="personality-modeling"></a>Modelado de personalidad

 Con cualquier modelo conversacional controlado por datos, existe el desafío de proporcionar una "personalidad" coherente. ROL se define como el carácter experimentado durante las interacciones de conversación. Un rol puede estar compuesto por elementos de identidad, el comportamiento del idioma y el estilo de interacción. En la versión actual de Personality Chat, el foco está en el estilo de interacción y el comportamiento del idioma.

Este modelo representa cada orador como un vector o inserción. Codifica la información del orador que influye en el contenido y en el estilo de sus respuestas. El modelo aprende las representaciones del orador en función del contenido conversacional proporcionado por los diferentes oradores. Los oradores con respuestas similares tienden a tener inserciones similares, ocupando posiciones cercanas en el espacio del vector. De este modo, los datos de entrenamiento de oradores cercanos en el espacio de vector ayudan a aumentar la capacidad de generalización del modelo del orador. El modelo crea de forma eficaz clústeres de orador que tienen representaciones similares en el espacio del vector para formar un clúster de rol con un "identificador de rol".

Para los roles predeterminados, los atributos y las respuestas protegidas se usan para encontrar el clúster de orador coincidente más cercano. Este clúster se elige como el identificador de rol para cada una de las personalidades predeterminadas. Se puede realizar la personalización continua de cualquier tipo de personalidad mediante un conjunto de respuestas de bot o marca. Entonces, se crean conversaciones para que emulen con precisión el rol del individuo, como el comportamiento de respuesta lingüística y otras características principales.

![Modelado de rol mediante clústeres de orador](./media/overview/persona-modeling.png)

## <a name="small-talk-intent-understanding"></a>Comprensión de la intención de conversación

Personality Chat tiene clasificadores de intención para garantizar una respuesta para las intenciones de conversación. Estos clasificadores no tomarán la forma de tareas o consultas de información. Un clasificador de chat de alto nivel determina si la consulta es de intención de charla o conversación. Toma su decisión mediante el uso de clasificadores basados en léxico y semántica y la combinación de sus puntuaciones. Estas intenciones se entrenan con datos conversacionales (como ejemplos de intenciones positivas) y consultas de búsqueda y tareas (para ejemplos de intenciones negativas).

Los demás clasificadores de subintención se usan para identificar las subclases de conversación para restringir los tipos de conversación a los que responde el servicio, como saludos, cortesía, opiniones, etc. Estos clasificadores de aprendizaje profundo, mediante el modelo semántico de estructura profunda de circunvolución (CDSSM), convierten todas las consultas en vectores. Se entrenan con decenas de miles de consultas protegidas para las subintenciones. El clasificador hace coincidir una consulta con las clases de intenciones identificadas existentes mediante la búsqueda de la coincidencia más cercana en el espacio del vector.

Se han establecido una serie de controles para ayudar a prevenir respuestas desfavorables, basándose en el conocimiento de agentes inteligentes como Zo. De forma predeterminada, Project Personality Chat está configurado para responder únicamente a intenciones de usuario reconocidas. Es posible que desee probar si Project Personality Chat es adecuado para sus circunstancias. Sus comentarios son bienvenidos si ve algo que necesite entrenamiento adicional.
