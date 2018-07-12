---
title: 'Revisión de las expresiones de punto de conexión para usar el aprendizaje activo en Language Understanding (LUIS): Azure | Microsoft Docs'
description: Use la característica de aprendizaje activo denominada "Review endpoint utterances" (Revisar las expresiones de punto de conexión) para mejorar las predicciones de rendimiento con más rapidez.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: b9672e8e63fb601d4411a342b7f3c00e30f9e002
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35383534"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Habilitar el aprendizaje activo mediante la revisión de expresiones de punto de conexión
El aprendizaje activo es una de las tres estrategias para mejorar la precisión de la predicción y la más fácil de implementar. 

## <a name="what-is-active-learning"></a>¿Qué es el aprendizaje activo?
El aprendizaje activo es un proceso de dos pasos. En primer lugar, LUIS selecciona las expresiones que recibe en el punto de conexión de la aplicación que necesitan validación. El segundo paso lo realiza el propietario de la aplicación o el colaborador para validar las expresiones seleccionadas para [revisión](label-suggested-utterances.md), incluida la intención correcta y todas las entidades dentro de la intención. Después de revisar las expresiones, vuelva a entrenar y publicar la aplicación. 

## <a name="which-utterances-are-on-the-review-list"></a>Expresiones incluidas en la lista de revisión
En LUIS se agregan expresiones a la lista de revisión cuando la primera intención que se desencadena tiene una puntuación baja, o bien cuando las puntuaciones de las dos primeras intenciones son demasiado próximas. 

## <a name="where-are-the-utterances-from"></a>De dónde provienen las expresiones
Las expresiones de punto de conexión se obtienen de las consultas del usuario final en el punto de conexión HTTP de la aplicación. Si la aplicación no se ha publicado o todavía no ha recibido visitas, no hay ninguna expresión para revisar. Si las visitas del punto de conexión se reciben para una intención o entidad específica, no habrá expresiones para revisar que las contengan. 

## <a name="schedule-review-periodically"></a>Programar la revisión periódicamente
No es necesario revisar las expresiones sugeridas todos los días, pero la revisión debe formar parte del mantenimiento periódico de LUIS. 

## <a name="delete-review-items-programmatically"></a>Eliminar los elementos de revisión mediante programación
Si la aplicación es grande, puede optar por revisar algunas expresiones y eliminar el resto de la lista mediante programación. Para ello, primero se [obtiene](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) la lista y, después, se [eliminan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) las expresiones por identificador.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [revisar](Label-Suggested-Utterances.md) las expresiones de punto de conexión
