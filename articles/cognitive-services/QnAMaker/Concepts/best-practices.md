---
title: 'Procedimientos recomendados: QnA Maker'
titlesuffix: Azure Cognitive Services
description: Siga estos procedimientos recomendados para mejorar la base de conocimiento y ofrecer mejores resultados a los usuarios finales del bot de chat o de la aplicación.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 63025a299f6b7b48936be9702522a832a1d77330
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074815"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Procedimientos recomendados para una base de conocimiento de QnA Maker
El artículo sobre el [ciclo de vida de desarrollo de una base de conocimiento](../Concepts/development-lifecycle-knowledge-base.md) indica cómo administrar una base de conocimiento de principio a fin. Siga estos procedimientos recomendados para mejorar la base de conocimiento y ofrecer mejores resultados a los usuarios finales del bot de chat o de la aplicación.

## <a name="extraction"></a>Extracción
El servicio QnA Maker mejora continuamente los algoritmos que extraen preguntas y respuestas del contenido y amplía la lista de formatos HTML y de archivos que admite. Siga las [directrices](../Concepts/data-sources-supported.md) para la extracción de datos según su tipo de documento. 

En general, las páginas de preguntas más frecuentes deben ser independientes y no combinarse con otra información. Los manuales de productos deben tener encabezados claros y, preferiblemente, una página de índice. 

## <a name="chit-chat"></a>Charla
Agregue una charla a su bot para que sea más conversacional y atractivo fácilmente. Puede agregar fácilmente conjuntos de datos de charla para tres personalidades predefinidas al crear la base de conocimiento y cambiarlos en cualquier momento. Obtenga información sobre cómo [agregar una charla a la base de conocimiento](../How-To/chit-chat-knowledge-base.md). 

### <a name="choosing-a-personality"></a>Selección de una personalidad
La charla es compatible con tres personalidades predefinidas: 

|Personalidades|
|--|
|Profesional|
|El amigo|
|Cómico|

El rango de respuestas va de formal a informal e irreverente. Debe seleccionar la personalidad que sea más acorde con el tono que quiera para el bot. Puede ver los conjuntos de datos, elegir uno que sirva como base para el bot y, a continuación, personalizar las respuestas. 

### <a name="edit-bot-specific-questions"></a>Edición de preguntas específicas de bot
Hay algunas preguntas específicas de bot que forman parte del conjunto de datos de charla y que se han rellenado con respuestas genéricas. Cambie estas respuestas para reflejar mejor los detalles del bot. 

Se recomienda hacer que las siguientes preguntas y respuestas de charla sean más específicas:

* ¿Quién es usted?
* ¿Qué puede hacer?
* ¿Cuántos años tiene?
* ¿Quién le ha creado?
* Hola
   

## <a name="rankingscoring"></a>Clasificación y puntuación
Asegúrese de aprovechar al máximo las características de clasificación que QnA Maker admite. Eso aumentará las probabilidades de que una determinada consulta de usuario se responda con una respuesta correcta.

### <a name="choosing-a-threshold"></a>Selección de un umbral
La puntuación de confianza predeterminada que se usa como umbral es de 50, pero puede cambiarla para su base de conocimiento según sus necesidades. Dado que cada base de conocimiento es diferente, conviene probar y elegir el umbral que mejor se adapte a cada caso. Obtenga más información sobre la [puntuación de confianza](../Concepts/confidence-score.md). 


### <a name="add-alternate-questions"></a>Agregar preguntas alternativas
[Alternar las preguntas](../How-To/edit-knowledge-base.md) aumenta las probabilidades de encontrar una coincidencia con una consulta de usuario. Las preguntas alternativas son útiles cuando hay varias maneras de plantear la misma pregunta. Esto puede incluir cambios en la estructura de la frase y el estilo de redacción.

|Consulta original|Consultas alternativas|Change| 
|--|--|--|
|¿Hay estacionamiento disponible?|¿Dispone de estacionamiento?|estructura de la frase|
 |Hola|Buenas<br>¿Cómo estamos?|estilo de redacción o jerga|

### <a name="use-metadata-filters"></a>Usar filtros de metadatos
Los [metadatos](../How-To/edit-knowledge-base.md) permiten usar filtros para reducir los resultados de una consulta de usuario. La respuesta de la base de conocimiento puede diferir según la etiqueta de metadatos, aunque la consulta sea la misma. Por ejemplo, *¿Dónde está ubicado el estacionamiento?* puede tener otra respuesta si la ubicación de la sucursal del restaurante es diferente; es decir, los metadatos son *ubicación: Seattle* frente a *ubicación: Redmond*.

### <a name="use-synonyms"></a>Usar sinónimos
Aunque hay cierta compatibilidad con los sinónimos en inglés, use [modificaciones de palabras](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) para agregar sinónimos a las palabras clave que tengan una forma diferente. Los sinónimos deben agregarse en el nivel del servicio QnA Maker y todas las bases de conocimiento del servicio deben compartirlos.

|Palabra original|Sinónimos|
|--|--|
|comprar|compra<br>banca por internet<br>banca electrónica|

### <a name="use-distinct-words-to-differentiate-questions"></a>Usar diferentes palabras para diferenciar preguntas
Los algoritmos de coincidencia y clasificación de QnA Maker que hacen coincidir una consulta de usuario con una pregunta en la base de conocimiento funcionan mejor si cada pregunta aborda una necesidad distinta. La repetición del mismo conjunto de palabras en diferentes preguntas reduce la probabilidad de que se seleccione la respuesta correcta para una consulta de usuario determinado con esas palabras. 

Por ejemplo, puede tener dos preguntas y respuestas independientes con las siguientes preguntas:

|Preguntas y respuestas|
|--|
|dónde está *ubicado* el estacionamiento|
|dónde está *ubicado* el cajero automático|

Puesto que estas dos preguntas y respuestas están expresadas con palabras muy similares, esta similitud podría provocar que las puntuaciones sean muy parecidas para muchas de las consultas de usuario que están expresadas como *dónde está ubicado `<x>`*. Como alternativa, intente diferenciarlas claramente con consultas como *dónde está el estacionamiento* y *dónde está el cajero automático*, evitando palabras como "ubicado", que pueden estar en muchas preguntas en la base de conocimiento. 


## <a name="collaborate"></a>Colaborar
QnA Maker permite a los usuarios [colaborar](../How-to/collaborate-knowledge-base.md) en una base de conocimiento. Los usuarios necesitan acceso al grupo de recursos de QnA Maker en Azure para tener acceso a las bases de conocimiento. Algunas organizaciones quizás quieran externalizar la edición y el mantenimiento de la base de conocimiento, sin dejar de proteger el acceso a sus recursos de Azure. Para realizar este modelo de editor-aprobador, se configuran dos [servicios QnA Maker](../How-to/set-up-qnamaker-service-azure.md) idénticos en suscripciones distintas y seleccionar uno para el ciclo de pruebas de edición. Una vez finalizada la prueba, se transfiere el contenido de la base de conocimiento con un proceso de [importación y exportación](../Tutorials/migrate-knowledge-base.md) al servicio QnA Maker del aprobador que, finalmente, publicará la base de conocimiento y actualizará el punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-to/edit-knowledge-base.md)
