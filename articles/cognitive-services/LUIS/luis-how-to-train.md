---
title: 'Entrenamiento de aplicaciones: LUIS'
titleSuffix: Azure Cognitive Services
description: El entrenamiento es el proceso de enseñar a la versión de la aplicación de Language Understanding (LUIS) a mejorar su comprensión del lenguaje natural. Entrene la aplicación de LUIS después de realizar actualizaciones en el modelo, como la adición, la edición, el etiquetado o la eliminación de entidades, intenciones o expresiones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 259ea23c05f0c0a138ad54b6efd11aad2061cf7a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500233"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Entrenamiento de la versión activa de la aplicación de LUIS 

El entrenamiento es el proceso de enseñar a la aplicación Language Understanding (LUIS) para mejorar su comprensión del lenguaje natural. Entrene la aplicación de LUIS después de realizar actualizaciones en el modelo, como la adición, la edición, el etiquetado o la eliminación de entidades, intenciones o expresiones. 

El entrenamiento y la [prueba](luis-concept-test.md) de una aplicación es un proceso iterativo. Tras entrenar la aplicación de LUIS, pruébela con expresiones de ejemplo para ver si las entidades e intenciones se reconocen correctamente. En caso contrario, realice actualizaciones en la aplicación de LUIS, entrénela y pruébela de nuevo. 

El entrenamiento se aplica a la versión activa en el portal de LUIS. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="how-to-train-interactively"></a>Cómo entrenar interactivamente

Para iniciar el proceso iterativo en el [portal de LUIS](https://www.luis.ai), primero debe entrenar la aplicación de LUIS al menos una vez. Asegúrese de que cada intención tiene al menos una expresión antes del entrenamiento.

1. Acceda a la aplicación mediante la selección de su nombre en la página **Mis aplicaciones**. 

2. En la aplicación, seleccione **Entrenar** en el panel superior. 

3. Una vez completado el entrenamiento, aparece una barra de notificación verde en la parte superior del explorador.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Si tiene uno o varios intentos en la aplicación que no contienen expresiones de ejemplo, no podrá entrenar la aplicación. Agregue expresiones para todas las intenciones. Para más información, consulte [Add example utterances](luis-how-to-add-example-utterances.md) (Adición de expresiones de ejemplo).

## <a name="training-date-and-time"></a>Fecha y hora del entrenamiento

La fecha y la hora de entrenamiento son GMT + 2. 

## <a name="train-with-all-data"></a>Entrenamiento con todos los datos

El entrenamiento usa un pequeño porcentaje de muestreo negativo. 

Si quiere utilizar todos los datos en lugar del muestreo negativo pequeño, use la [API](#version-settings-api-use-of-usealltrainingdata).

<!--

 or the [LUIS portal setting](#luis-portal-setting-to-use-all-training-data)

### LUIS portal setting to use all training data

!!!IGNITE


-->

### <a name="version-settings-api-use-of-usealltrainingdata"></a>Uso de UseAllTrainingData por parte de la API de configuración de versión

Use la [API de configuración de versión](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) con el elemento `UseAllTrainingData` establecido en true para desactivar esta característica. 

## <a name="unnecessary-training"></a>Entrenamiento innecesario

No es necesario entrenar después de realizar un cambio. El entrenamiento debe realizarse después de que se aplique un grupo de cambios al modelo y el siguiente paso que debe hacer es probar o publicar. Si no necesita probar ni publicar, el entrenamiento no es necesario. 

## <a name="training-with-the-rest-apis"></a>Entrenamiento con las API REST

Para el entrenamiento en el portal de LUIS, solo debe pulsar el botón **Entrenar**. El proceso de entrenamiento con las API REST consta de dos pasos. El primero consiste en [solicitar entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) con HTTP POST. A continuación, debe solicitar el [estado del entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) con HTTP Get. 

Para saber cuando concluirá el entrenamiento, debe sondear el estado hasta que todos los modelos estén completamente entrenados. 

## <a name="next-steps"></a>Pasos siguientes

* [Label suggested utterances with LUIS](luis-how-to-review-endpoint-utterances.md) (Etiquetado de expresiones sugeridas con LUIS) 
* [Use features to improve your LUIS app's performance](luis-how-to-add-features.md) (Uso de características para mejorar el rendimiento de la aplicación de LUIS) 
