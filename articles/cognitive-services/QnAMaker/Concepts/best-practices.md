---
title: Procedimientos recomendados - QnA Maker - Microsoft Cognitive Services | Microsoft Docs
description: Siga estos procedimientos recomendados para mejorar la base de conocimiento y ofrecer mejores resultados a los usuarios finales del bot de chat o de la aplicación.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 7a85ebbc3892a90e98e73a73425c1f8ec1de0b35
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "35383150"
---
# <a name="best-practices"></a>Procedimientos recomendados
El artículo sobre el [ciclo de vida de desarrollo de una base de conocimiento](../Concepts/development-lifecycle-knowledge-base.md) indica cómo administrar una base de conocimiento de principio a fin. Siga estos procedimientos recomendados para mejorar la base de conocimiento y ofrecer mejores resultados a los usuarios finales del bot de chat o de la aplicación.

## <a name="extraction"></a>Extracción
QnA Maker mejora continuamente los algoritmos que extraen preguntas y respuestas del contenido y amplía la lista de formatos de archivos y página HTML que admite. Siga las [directrices](../Concepts/data-sources-supported.md) de extracción basada en el tipo de documento del que se está extrayendo. 

En general, las páginas de preguntas más frecuentes deben ser independientes y no combinarse con otra información. Los manuales de productos deben tener encabezados claros y, preferiblemente, una página de índice. 

## <a name="rankingmatching"></a>Clasificación/coincidencia
Asegúrese de aprovechar al máximo las características de clasificación que QnA Maker admite. Eso aumentará las probabilidades de que una determinada consulta de usuario se responda con una respuesta correcta.

### <a name="add-alternate-questions"></a>Agregar preguntas alternativas
[Alternar las preguntas](../How-To/edit-knowledge-base.md) aumenta las probabilidades de encontrar una coincidencia con una consulta de usuario. Las preguntas alternativas son útiles cuando hay varias maneras de plantear la misma pregunta. Esto puede incluir cambios en la estructura de la frase (por ejemplo, *"¿Hay estacionamiento disponible?"* frente a *"¿Tiene aparcamiento?"* ) o los cambios en el registro (por ejemplo, *"Hola"* frente a *"Eh"*, *"¡Qué hay!"* ).

### <a name="use-metadata-filters"></a>Usar filtros de metadatos
Los [metadatos](../How-To/edit-knowledge-base.md) permiten usar filtros para reducir los resultados de una consulta de usuario. La respuesta de la base de conocimiento puede diferir según la etiqueta de metadatos, aunque la consulta sea la misma. Por ejemplo, *"¿Dónde está ubicado el estacionamiento?"* puede tener otra respuesta si la ubicación del restaurante es diferente; es decir, los metadatos son *ubicación: Seattle* frente a *ubicación: Redmond*).

### <a name="use-synonyms"></a>Usar sinónimos
Aunque hay cierta compatibilidad con los sinónimos en inglés, use [modificaciones de palabras](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) para agregar sinónimos a las palabras clave que tienen una forma diferente (ejemplo: *comprar* -> *adquirir* o *banca online* -> *banca en línea*. Los sinónimos deben agregarse en el nivel del servicio QnA Maker y todas las bases de conocimiento del servicio deben compartirlos.

### <a name="use-distinct-words-to-differentiate-questions"></a>Usar diferentes palabras para diferenciar preguntas
Los algoritmos de coincidencia y clasificación de QnA Maker que hacen coindicir una consulta de usuario con una pregunta en la base de conocimiento funcionan mejor si cada pregunta aborda una necesidad distinta. La repetición del mismo conjunto de palabras en diferentes preguntas reduce la probabilidad de que se seleccione la respuesta correcta para una consulta de usuario determinado con esas palabras.

## <a name="collaborate"></a>Colaborar
QnA Maker permite a los usuarios [colaborar](../How-to/collaborate-knowledge-base.md) en una base de conocimiento. Los usuarios necesitan acceso al grupo de recursos de QnA Maker en Azure para tener acceso a las bases de conocimiento. Algunas organizaciones quizás quieran externalizar la edición y el mantenimiento de la base de conocimiento, sin dejar de proteger el acceso a sus recursos de Azure. Para realizar este modelo de editor-aprobador, se pueden configurar dos [servicios QnA Maker](../How-to/set-up-qnamaker-service-azure.md) idénticos en suscripciones distintas y designar uno para el ciclo de pruebas de edición. Una vez completada la prueba, se puede transferir el contenido de la base de conocimiento con un proceso de [importación y exportación](../Tutorials/migrate-knowledge-base.md) al servicio QnA Maker del aprobador que, finalmente, publicará la base de conocimiento y actualizará el punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Editar una base de conocimiento](../How-to/edit-knowledge-base.md)

