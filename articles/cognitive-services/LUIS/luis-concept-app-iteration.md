---
title: 'Diseño de aplicaciones iterativo: LUIS'
titleSuffix: Azure Cognitive Services
description: LUIS aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 4356d9e1cd3d6f1a924603f7405d612814d35859
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70256925"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Ciclo de creación de una aplicación de LUIS
LUIS aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión. 

![Ciclo de creación](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Generar un modelo de LUIS
El propósito del modelo es averiguar qué pide el usuario (la intención) y qué partes de la pregunta aportan datos (entidades) que permitan determinar la respuesta. 

El modelo debe ser específico del dominio de aplicación para poder determinar palabras y frases que sean relevantes, así como el orden habitual de las palabras. 

El modelo requiere intenciones y _debe tener_ entidades. 

## <a name="add-training-examples"></a>Agregar ejemplos de entrenamiento
LUIS necesita expresiones de ejemplo en las intenciones. Los ejemplos necesitan una variación amplia de palabras y del orden de estas para poder determinar qué intención tiene la expresión. Cada expresión de ejemplo debe tener los datos necesarios etiquetados como entidades. 

Debe indicar a LUIS que ignore las expresiones que no sean pertinentes para el dominio de la aplicación. Para ello, debe asignar la expresión a la intención **None** (Ninguno). No es necesario etiquetar todas las palabras o frases que no necesite extraer de una expresión. No hay ninguna etiqueta para las palabras o frases que se vayan a ignorar. 

## <a name="train-and-publish-the-app"></a>Entrenamiento y publicación de la aplicación
Una vez que tenga entre 15 y 30 expresiones diferentes en cada intención, con las entidades necesarias etiquetadas, tendrá que [entrenar](luis-how-to-train.md) y, a continuación, [publicar](luis-how-to-publish-app.md). Desde la notificación de éxito de publicación, use el vínculo para obtener los puntos de conexión. Asegúrese de crear y publicar la aplicación de manera que esté disponible en las [regiones de punto de conexión](luis-reference-regions.md) que necesite. 

## <a name="https-prediction-endpoint-testing"></a>Pruebas de punto de conexión de predicción HTTPS
Puede probar la aplicación de LUIS desde el punto de conexión de predicción HTTPS. La realización de pruebas desde el punto de conexión de predicción permite a LUIS elegir cualquier expresión con una confianza baja para la [revisión](luis-how-to-review-endpoint-utterances.md).  

## <a name="recycle"></a>Reciclar

Cuando haya terminado un ciclo de creación, puede empezar de nuevo. Empiece [revisando las expresiones del punto de conexión de predicción](luis-how-to-review-endpoint-utterances.md) que LUIS haya marcado con una confianza baja. Compruebe la intención y la entidad de estas expresiones. Una vez revisadas las expresiones, la lista de revisión debería estar vacía.  

Considere la posibilidad de [clonar](luis-concept-version.md#clone-a-version) la versión actual en una nueva versión y, a continuación, comience a crear los cambios en la nueva versión. 

## <a name="batch-testing"></a>Pruebas por lotes

Las [pruebas por lotes](luis-concept-batch-test.md) son una manera de ver cuántas expresiones de ejemplo puntúa LUIS. Los ejemplos deben ser nuevos para LUIS y deben estar correctamente etiquetados con la intención y las entidades que quiere que busque LUIS. Los resultados de la prueba indican el grado de rendimiento que tendría LUIS en ese conjunto de expresiones. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre conceptos relativos a la [colaboración](luis-concept-keys.md).
