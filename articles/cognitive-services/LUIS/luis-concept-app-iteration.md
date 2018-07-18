---
title: 'Descripción del diseño de aplicaciones iterativas de LUIS: Azure | Microsoft Docs'
description: Las aplicaciones de LUIS requieren iteraciones de diseño para entrenar a LUIS para obtener la mejor extracción de datos.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: e0467e4c41209c937f548edc0c40c05cae588f4c
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888255"
---
# <a name="authoring-cycle"></a>Ciclo de creación
LUIS aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión. 

![Ciclo de creación](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Generar un modelo de LUIS
El propósito del modelo es averiguar qué pide el usuario (la intención) y qué partes de la pregunta aportan datos (entidades) que permitan determinar la respuesta. 

El modelo debe ser específico del dominio de aplicación para poder determinar palabras y frases que sean relevantes, así como el orden habitual de las palabras. 

El modelo incluye intenciones y entidades. 

## <a name="add-training-examples"></a>Agregar ejemplos de entrenamiento
LUIS necesita expresiones de ejemplo en las intenciones. Los ejemplos necesitan una variación amplia de palabras y del orden de estas para poder determinar qué intención tiene la expresión. Cada expresión de ejemplo debe tener los datos necesarios etiquetados como entidades. 

Debe indicar a LUIS que ignore las expresiones que no sean pertinentes para el dominio de la aplicación. Para ello, debe asignar la expresión a la intención **None** (Ninguno). No es necesario etiquetar todas las palabras o frases que no necesite extraer de una expresión. No hay ninguna etiqueta para las palabras o frases que se vayan a ignorar. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Entrenar y publicar la aplicación
Una vez que tenga entre 10 y 15 expresiones diferentes en cada intención, con las entidades necesarias etiquetadas, debe entrenar a LUIS y efectuar la publicación para obtener los puntos de conexión. Asegúrese de crear y publicar la aplicación de manera que esté disponible en las [regiones de punto de conexión](luis-reference-regions.md) que necesite. 

## <a name="https-endpoint-testing"></a>Pruebas de puntos de conexión HTTPS
Puede probar la aplicación de LUIS desde el punto de conexión HTTPS que aparece en la página **[Publish](luis-how-to-publish-app.md)** (Publicar). La realización de pruebas desde el punto de conexión permite a LUIS elegir cualquier expresión con una confianza baja para la revisión.  

## <a name="recycle"></a>Reciclar
Cuando haya terminado un ciclo de creación, puede empezar de nuevo. Empiece revisando las expresiones de punto de conexión que LUIS ha marcado con una confianza baja. Compruebe la intención y la entidad de estas expresiones. Una vez revisadas las expresiones, la lista de revisión debería estar vacía.  

## <a name="batch-testing"></a>Pruebas por lotes
Las pruebas por lotes son una manera de ver cuántas expresiones de ejemplo puntúa LUIS. Los ejemplos deben ser nuevos para LUIS y deben estar correctamente etiquetados con la intención y las entidades que quiere que busque LUIS. Los resultados de la prueba indican el grado de rendimiento que tendría LUIS en ese conjunto de expresiones. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre conceptos relativos a la [colaboración](luis-concept-collaborator.md).