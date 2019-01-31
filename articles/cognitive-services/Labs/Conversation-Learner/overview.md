---
title: ¿Qué es Conversation Learner? - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Obtenga información sobre Conversation Learner y cómo funciona.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c46ffe2076d4b1491a3b27958dfbf5ed09115eaa
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221291"
---
# <a name="what-is-conversation-learner"></a>¿Qué es Conversation Learner?

Conversation Learner permite compilar y entrenar interfaces de conversación que aprenden de las interacciones de ejemplo. 

A diferencia de los enfoques tradicionales, Conversation Learner considera el contexto de un extremo a otro de un diálogo para mejorar las respuestas y ofrecer experiencias de usuario más atractivas. Con la extensión de un amplio conjunto de casos de uso orientados a tareas, Conversation Learner aplica el aprendizaje automático en segundo plano para que haya menos probabilidades de que los bots y los agentes inteligentes frustren a los usuarios, incurran en costes de servicio de cliente adicionales e inciten a una interacción más intuitiva.

Los desarrolladores comienzan por introducir diálogos prototípicos que desean imitar. El modelo aprende a medida que se introducen más diálogos. Una vez que el modelo funcione bien, puede implementarse el bot para los usuarios finales. Conversation Learner registra las conversaciones con los usuarios, y el desarrollador puede revisarlas. Si se detectan errores, el desarrollador puede hacer una corrección inmediata, y el modelo se conserva y se mantiene disponible para su uso inmediato.

Este enfoque reduce la codificación manual de la lógica de control de diálogos y permite a los propietarios de empresas o a los expertos en la materia contribuir a una interfaz de conversación sin un conocimiento previo de aprendizaje automático. Si se implementa como parte de un bot, de un dispositivo inteligente o de un agente inteligente, Conversation Learner puede iterar rápidamente nuevas aptitudes, comportamientos o competencias y mejorar rápidamente su calidad. 

Conversation Learner capacita a los desarrolladores para aumentar la comercialización y administrar diálogos correctos en varios canales de conversaciones a través de Microsoft Bot Framework, o bien de manera independiente con el uso de su propia infraestructura.

Resumen y aspectos destacados:

- Conversation Learner es una forma de AI en primer lugar de compilar bots orientados a tareas.

- Se basa en una red neural recurrente de un extremo a otro (LSTM) y aprende directamente de ejemplos de conversaciones de varios turnos. 

- Permite a los diseñadores, desarrolladores, usuarios empresariales y trabajadores de centros de llamadas compilar y mantener bots. 

- Proporciona la capacidad para expresar reglas de negocio y sentido común en el código.

- Durante las sesiones de aprendizaje, el modelo de red neural se usa para puntuar el siguiente conjunto de acciones esperadas de la conversación. El desarrollador del bot puede seleccionar después la acción correcta y entrenar a la red para que proporcione la respuesta apropiada.
 
- Una vez completado el entrenamiento, el desarrollador puede usar los diálogos del registro de las interacciones de usuario para realizar correcciones en las respuestas de bots y volver a entrenar el modelo. 

- Puede llamar a API específicas de dominios y de terceros para completar tareas.

