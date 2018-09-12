---
title: Entrenamiento de la aplicación de LUIS
titleSuffix: Azure Cognitive Services
description: El entrenamiento es el proceso de enseñar a la aplicación Language Understanding (LUIS) para mejorar su comprensión del lenguaje natural. Entrene la aplicación de LUIS después de realizar actualizaciones en el modelo, como la adición, la edición, el etiquetado o la eliminación de entidades, intenciones o expresiones.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 42cff3dd8237598da5aa71ed1a4d6462c5b4c25d
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44049138"
---
# <a name="train-your-luis-app"></a>Entrenamiento de la aplicación de LUIS

El entrenamiento es el proceso de enseñar a la aplicación Language Understanding (LUIS) para mejorar su comprensión del lenguaje natural. Entrene la aplicación de LUIS después de realizar actualizaciones en el modelo, como la adición, la edición, el etiquetado o la eliminación de entidades, intenciones o expresiones. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

El entrenamiento y la [prueba](luis-concept-test.md) de una aplicación es un proceso iterativo. Tras entrenar la aplicación de LUIS, pruébela con expresiones de ejemplo para ver si las entidades e intenciones se reconocen correctamente. En caso contrario, realice actualizaciones en la aplicación de LUIS, entrénela y pruébela de nuevo. 

## <a name="train-your-app"></a>Entrenamiento de la aplicación
Para iniciar el proceso iterativo, primero debe entrenar la aplicación de LUIS al menos una vez. Asegúrese de que cada intención tiene al menos una expresión antes del entrenamiento.

1. Acceda a la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**. 

2. En la aplicación, seleccione **Entrenar** en el panel superior. 

3. Una vez completado el entrenamiento, aparece una barra de notificación verde en la parte superior del explorador.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Si tiene uno o varios intentos en la aplicación que no contienen expresiones de ejemplo, no podrá entrenar la aplicación. Agregue expresiones para todas las intenciones. Para más información, consulte [Add example utterances](luis-how-to-add-example-utterances.md) (Adición de expresiones de ejemplo).

## <a name="next-steps"></a>Pasos siguientes

* [Label suggested utterances with LUIS](luis-how-to-review-endoint-utt.md) (Etiquetado de expresiones sugeridas con LUIS) 
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS) 