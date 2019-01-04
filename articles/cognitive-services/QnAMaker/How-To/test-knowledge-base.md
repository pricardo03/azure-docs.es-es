---
title: 'Cómo probar una base de conocimiento: QnA Maker'
titlesuffix: Azure Cognitive Services
description: La prueba de la base de conocimiento de QnA Maker es una parte importante de un proceso iterativo para mejorar la precisión de las respuestas devueltas. Puede probar la base de conocimiento mediante una interfaz de chat mejorada que también permite realizar ediciones.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/17/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2ad8d4fa04801996364755d1e0270a6509912679
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581953"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Prueba de la base de conocimiento de forma interactiva en QnA Maker

La prueba de la base de conocimiento de QnA Maker es una parte importante de un proceso iterativo para mejorar la precisión de las respuestas devueltas. Puede probar la base de conocimiento mediante una interfaz de chat mejorada que también permite realizar ediciones.

## <a name="test-answer-matching"></a>Probar la coincidencia de respuestas

1.  Acceda a la base de conocimiento; para ello, seleccione su nombre en la página  **Mis bases de conocimiento**.
2.  Para acceder al panel deslizante Prueba, seleccione  **Prueba**  en el panel superior de la aplicación.

    ![Acceso al panel Prueba](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Escriba una consulta en el cuadro de texto y presione ENTRAR.

4.  Se devuelve como resultado la respuesta con mejor coincidencia de la base de conocimiento.

## <a name="clear-test-panel"></a>Borrar el panel de prueba

Para borrar las consultas de prueba especificadas y sus resultados de la consola de pruebas, seleccione  **Empezar de nuevo**  en la esquina superior izquierda del panel Prueba.

## <a name="close-test-panel"></a>Cerrar el panel Prueba

Para cerrar el panel Prueba, vuelva a seleccionar el botón  **Prueba** . Mientras el panel Prueba está abierto, no puede editar el contenido de la base de conocimiento.

## <a name="inspect-score"></a>Inspeccionar puntuación

Puede inspeccionar los detalles del resultado de las pruebas en el panel Inspeccionar.

1.  Con el panel deslizante Prueba abierto, seleccione  **Inspeccionar**  para obtener más detalles sobre dicha respuesta.

    ![Inspeccionar respuestas](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Aparece el panel Inspección. El panel incluye la intención de mayor puntuación, así como cualquier entidad identificada. El panel muestra el resultado de la expresión seleccionada.

## <a name="correct-the-top-scoring-answer"></a>Corrección de la respuesta con mayor puntuación

Si la respuesta con mayor puntuación es incorrecta, seleccione la respuesta correcta en la lista y haga clic en **Guardar y entrenar**.

![Corrección de la respuesta con mayor puntuación](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Agregar preguntas alternativas

Puede agregar formas alternativas de una pregunta a una respuesta determinada. Escriba las respuestas alternativas en el cuadro de texto y presiones ENTRAR para agregarlas. Seleccione **Guardar y entrenar** para almacenar las actualizaciones.

![Agregar preguntas alternativas](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Adición de una nueva respuesta

Puede agregar una respuesta nueva si cualquiera de las respuestas existentes coincidentes son incorrectas o la respuesta no existe en la base de conocimiento (ninguna coincidencia adecuada encontrada en la base de conocimiento). Escriba la nueva respuesta a la pregunta actual en el cuadro de texto y presione ENTRAR para agregarla. 

Seleccione **Guardar y entrenar** para persistir esta respuesta. Se ha agregado un par pregunta-respuesta nuevo a la base de conocimiento.

![Adición de un nuevo par de preguntas y respuestas](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Todas las modificaciones realizadas en la base de conocimiento solo se guardan si presiona el botón **Guardar y entrenar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Publicación de una base de conocimientos](./publish-knowledge-base.md)
