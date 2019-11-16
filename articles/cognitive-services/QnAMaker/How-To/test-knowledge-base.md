---
title: 'Cómo probar una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: La prueba de la base de conocimiento de QnA Maker es una parte importante de un proceso iterativo para mejorar la precisión de las respuestas devueltas. Puede probar la base de conocimiento mediante una interfaz de chat mejorada que también permite realizar ediciones.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c139d3a740067e3cecaff90d3171d7b0cb3d52c7
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091750"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>Prueba de una base de conocimiento en QnA Maker

La prueba de la base de conocimiento de QnA Maker es una parte importante de un proceso iterativo para mejorar la precisión de las respuestas devueltas. Puede probar la base de conocimiento mediante una interfaz de chat mejorada que también permite realizar ediciones.

## <a name="interactively-test-in-qna-maker-portal"></a>Prueba interactiva en QnA Maker Portal

1. Acceda a la base de conocimiento; para ello, seleccione su nombre en la página **Mis bases de conocimiento**.
1. Para acceder al panel deslizante Prueba, seleccione **Prueba** en el panel superior de la aplicación.
1. Escriba una consulta en el cuadro de texto y presione ENTRAR.
1. Se devuelve como resultado la respuesta con mejor coincidencia de la base de conocimiento.

### <a name="clear-test-panel"></a>Borrar el panel de prueba

Para borrar las consultas de prueba especificadas y sus resultados de la consola de pruebas, seleccione **Empezar de nuevo** en la esquina superior izquierda del panel Probar.

### <a name="close-test-panel"></a>Cerrar el panel Prueba

Para cerrar el panel Prueba, vuelva a seleccionar el botón **Prueba**. Mientras el panel Prueba está abierto, no puede editar el contenido de la base de conocimiento.

### <a name="inspect-score"></a>Inspeccionar puntuación

Puede inspeccionar los detalles del resultado de las pruebas en el panel Inspeccionar.

1.  Con el panel deslizante Prueba abierto, seleccione **Inspeccionar** para obtener más detalles sobre dicha respuesta.

    ![Inspeccionar respuestas](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Aparece el panel Inspección. El panel incluye la intención con la mayor puntuación, así como cualquier entidad identificada. El panel muestra el resultado de la expresión seleccionada.

### <a name="correct-the-top-scoring-answer"></a>Corrección de la respuesta con mayor puntuación

Si la respuesta con mayor puntuación es incorrecta, seleccione la respuesta correcta en la lista y haga clic en **Guardar y entrenar**.

![Corrección de la respuesta con mayor puntuación](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Agregar preguntas alternativas

Puede agregar formas alternativas de una pregunta a una respuesta determinada. Escriba las respuestas alternativas en el cuadro de texto y presiones ENTRAR para agregarlas. Seleccione **Guardar y entrenar** para almacenar las actualizaciones.

![Agregar preguntas alternativas](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Adición de una nueva respuesta

Puede agregar una respuesta nueva si cualquiera de las respuestas existentes coincidentes son incorrectas o la respuesta no existe en la base de conocimiento (ninguna coincidencia adecuada encontrada en la base de conocimiento). 

En la parte inferior de la lista de las respuestas, use el cuadro de texto para escribir una nueva respuesta y presione Entrar para agregarla. 

Seleccione **Guardar y entrenar** para persistir esta respuesta. Se ha agregado un par pregunta-respuesta nuevo a la base de conocimiento. 

> [!NOTE]
> Todas las modificaciones realizadas en la base de conocimiento solo se guardan si presiona el botón **Guardar y entrenar**.

### <a name="test-the-published-knowledge-base"></a>Prueba de la base de conocimiento publicada

Puede probar la versión publicada de la base de conocimiento en el panel prueba. Una vez publicada la base de conocimiento, seleccione el cuadro **Published KB** (Base de conocimiento publicada) y envíe una consulta para obtener resultados de esta.

![Prueba en una base de conocimiento publicada](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="batch-test-with-tool"></a>Pruebas por lotes con herramienta

Use la herramienta de pruebas por lotes cuando quiera:

* determinar la mejor respuesta y la puntuación de un conjunto de preguntas
* validar la respuesta esperada para el conjunto de preguntas

Las pruebas por lotes se proporcionan con la herramienta de pruebas por lotes. Esta herramienta está disponible como [ejecutable comprimido](https://aka.ms/qnamakerbatchtestingtool) para descarga o como [código fuente de C#](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting). 

La [documentación de referencia sobre la herramienta](../reference-tsv-format-batch-testing.md) incluye:

* el ejemplo de la línea de comandos de la herramienta
* el formato de los archivos de entrada y salida de TSV 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Publicación de una base de conocimientos](./publish-knowledge-base.md)
