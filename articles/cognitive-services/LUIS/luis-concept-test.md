---
title: 'Probar la aplicación de LUIS: Azure | Microsoft Docs'
description: Use Language Understanding (LUIS) para trabajar continuamente en la aplicación para refinarla y mejorar la comprensión del lenguaje.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8c702d2adbadd2736eed05c7580e8aabf69affbf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266335"
---
# <a name="testing-in-luis"></a>Realización de pruebas en LUIS

La realización de pruebas es el proceso por el cual se proporcionan expresiones de ejemplo a LUIS y se obtiene una respuesta de intenciones y entidades reconocidas por LUIS. 

Puede [probar](interactive-test.md) LUIS de forma interactiva (una expresión a la vez) o proporcionar un [lote](luis-concept-batch-test.md) de expresiones. Con la realización de pruebas puede comparar el modelo [activo](luis-concept-version.md#active-version) actual con el modelo publicado. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>
## <a name="what-is-a-score-in-testing"></a>¿Qué es una puntuación en las pruebas?
Vea los conceptos de [puntuación de predicción](luis-concept-prediction-score.md) para obtener más información sobre las puntuaciones de predicción.

## <a name="interactive-testing"></a>Pruebas interactivas
Las pruebas interactivas se llevan a cabo desde el panel **Prueba** del sitio web. Puede introducir una expresión para ver cómo se identifican y puntúan las intenciones y las entidades. Si LUIS no predice en el panel de pruebas las intenciones y entidades tal como espera en una expresión, cópiela en la página **Intención** como una expresión nueva. Después, etiquete las partes de esa expresión y entrene a LUIS. 

## <a name="batch-testing"></a>Pruebas por lotes
Vea [Pruebas por lotes](luis-concept-batch-test.md) si va a probar más de una expresión a la vez.

## <a name="endpoint-testing"></a>Pruebas de punto de conexión
Puede hacer pruebas mediante el [punto de conexión](luis-glossary.md#endpoint) con un máximo de dos versiones de la aplicación. Con la versión principal o activa de la aplicación establecida como punto de conexión de **producción**, agregue una segunda versión al punto de conexión de **ensayo**. Este enfoque le proporciona tres versiones de una expresión: el modelo actual en el panel Prueba del sitio web de [LUIS][LUIS] y las dos versiones en los dos puntos de conexión diferentes. 

Todas las pruebas de punto de conexión cuentan para la cuota de uso. 

## <a name="do-not-log-tests"></a>No registrar pruebas
Si hace una prueba en un punto de conexión y no quiere que la expresión quede registrada, recuerde que debe usar la configuración de cadena de consulta `logging=false`.

## <a name="where-to-find-utterances"></a>Dónde encontrar expresiones
LUIS almacena todas las expresiones registradas en el registro de consultas, que se puede descargar en la página de la lista **Aplicaciones** del sitio web de [LUIS][LUIS], así como las [API de creación](https://aka.ms/luis-authoring-apis) de LUIS. 

Todas las expresiones de las que LUIS no esté seguro aparecerán en la página **[Revisar las expresiones de punto de conexión](label-suggested-utterances.md)** del sitio web de [LUIS][LUIS]. 

![Revisar las expresiones de punto de conexión](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>No se olvide del entrenamiento
No olvide [entrenar](luis-how-to-train.md) a LUIS después de hacer cambios en el modelo. Los cambios efectuados en la aplicación de LUIS no se ven en las pruebas hasta que se entrena la aplicación. 

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre las [pruebas](interactive-test.md) de las expresiones.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions