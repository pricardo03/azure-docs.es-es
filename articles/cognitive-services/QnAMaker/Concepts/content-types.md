---
title: 'Tipos de contenido: QnA Maker'
description: Entre los tipos de contenido se incluyen muchos documentos estructurados estándar, como los archivos PDF, DOC y TXT.
services: cognitive-services
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: 7c78f9ea261fa636cce50b69524802d0900e9d7b
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650204"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Tipos de contenido de documento que se pueden agregar a una base de conocimiento
Entre los tipos de contenido se incluyen muchos documentos estructurados estándar, como los archivos PDF, DOC y TXT.

## <a name="file-and-url-data-types"></a>Tipos de datos de archivo y URL

En la tabla siguiente se resumen los tipos de contenido y los formatos de archivo que son compatibles con QnA Maker.

|Tipo de origen|Tipo de contenido| Ejemplos|
|--|--|--|
|URL|Preguntas más frecuentes<br> (sin formato, con secciones o con una página principal de temas)<br>Páginas de soporte técnico <br> (artículos paso a paso de una sola página, artículos de solución de problemas, etc.)|[Preguntas más frecuentes sin formato](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[preguntas más frecuentes con vínculos](https://www.microsoft.com/en-us/software-download/faq),<br> [página principal de preguntas más frecuentes por temas](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[artículo de soporte técnico](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Preguntas más frecuentes,<br> manual de producto,<br> folletos,<br> documento,<br> directiva de folleto,<br> guía de soporte técnico,<br> archivo de preguntas y respuestas estructurado,<br> etc.|**Sin varios turnos**<br>[Archivo de preguntas y respuestas estructurado.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Manual de producto de ejemplo.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Archivo semiestructurado de ejemplo.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Notas del producto de ejemplo.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**Varios turnos**:<br>[Surface Pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Ventajas de Contoso (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Ventajas de Contoso (pdf)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|*Excel|Archivo de preguntas y respuestas estructurado<br> (incluye compatibilidad con RTF y HTML)|[Preguntas más frecuentes de preguntas y respuestas de ejemplo.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|*TXT/TSV|Archivo de preguntas y respuestas estructurado|[Charla de ejemplo.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Si necesita autenticación para el origen de datos, tenga en cuenta los siguientes métodos para introducir ese contenido en QnA Maker:

* Descargar manualmente el archivo e importarlo en QnA Maker
* Agregar el archivo desde una [ubicación de SharePoint](../how-to/add-sharepoint-datasources.md) autenticada

## <a name="url-content"></a>Contenido de URL

Se pueden importar dos tipos de documentos a través de una **dirección URL** en QnA Maker:

* Preguntas más frecuentes sobre las direcciones URL
* Direcciones URL de soporte técnico

Cada tipo indica un formato esperado.

## <a name="file-based-content"></a>Contenido basado en archivos

Puede agregar archivos a una base de conocimiento desde un origen de datos público o desde el sistema de archivos local en el portal de [QnA Maker](https://www.qnamaker.ai).

## <a name="content-format-guidelines"></a>Instrucciones de formato de contenido

Obtenga más información sobre las [directrices de formato](../reference-document-format-guidelines.md) para los distintos archivos.

## <a name="next-steps"></a>Pasos siguientes

Comprenda qué información se almacena en un [conjunto de preguntas y repuestas (QnA)](question-answer-set.md).