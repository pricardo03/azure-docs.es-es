---
title: 'Orígenes de datos compatibles: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker extrae automáticamente pares de preguntas y respuestas del contenido semiestructurado, como preguntas más frecuentes, manuales de productos, directrices, documentos de soporte técnico y directivas almacenadas como páginas web, archivos PDF o archivos de documento (DOC) de MS Word. El contenido también se puede agregar a la base de conocimiento de archivos de contenido de preguntas y respuestas estructurados.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 36d74c7fba472956d57344977de79a4cbfd2cf02
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648520"
---
# <a name="data-sources-for-qna-maker-content"></a>Orígenes de datos para el contenido de QnA Maker

QnA Maker extrae automáticamente pares de preguntas y respuestas del contenido semiestructurado, como preguntas más frecuentes, manuales de productos, directrices, documentos de soporte técnico y directivas almacenadas como páginas web, archivos PDF o archivos de documento (DOC) de MS Word. El contenido también se puede agregar a la base de conocimiento de archivos de contenido de preguntas y respuestas estructurados. 

En la tabla siguiente se resumen los tipos de contenido y los formatos de archivo que son compatibles con QnA Maker.

|Tipo de origen|Tipo de contenido| Ejemplos|
|--|--|--|
|URL|Preguntas más frecuentes (sin formato, con secciones o con una página principal de temas)|[Preguntas más frecuentes sin formato](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), [Preguntas más frecuentes con vínculos](https://www.microsoft.com/software-download/faq), [Preguntas más frecuentes con página principal de temas](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF/DOC|Preguntas más frecuentes, manual de producto, prospectos, documento, folleto, directiva, guía de soporte técnico, preguntas y respuestas estructuradas, etc.|[Preguntas y respuestas estructuradas.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Bot%20Service%20Sample%20FAQ.docx), [Manual de producto de ejemplo.pdf](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf), [Ejemplo semiestructurado.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx), [Documento técnico de ejemplo.pdf](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-stack-wortmann-bring-the-power-of-the-public-cloud-into-your-data-center/Azure_Stack_Wortmann_Bring_the_Power_of_the_Public_Cloud_into_Your_Data_Center.pdf)|
|Excel|Archivo de preguntas y respuestas estructurado (incluyendo compatibilidad con HTML, RTF)|[Preguntas más frecuentes de preguntas y respuestas de ejemplo.xls](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Archivo de preguntas y respuestas estructurado|[Charla de ejemplo.tsv](https://raw.githubusercontent.com/Microsoft/BotBuilder-PersonalityChat/master/CSharp/Datasets/Queries_Responses_Friendly_QnAMaker.tsv)|

## <a name="faq-urls"></a>Preguntas más frecuentes sobre las direcciones URL

QnA Maker puede admitir páginas web de preguntas más frecuentes de tres formas diferentes: preguntas más frecuentes sin formato, páginas de preguntas más frecuentes con vínculos, páginas de preguntas más frecuentes con una página principal de temas.

### <a name="plain-faq-pages"></a>Páginas de preguntas más frecuentes sin formato

Este es el tipo más común de página de preguntas más frecuentes, en la que las respuestas siguen inmediatamente las preguntas de la misma página. 

Aquí tiene un ejemplo de una página de preguntas más frecuentes sin formato:

![Páginas de preguntas más frecuentes sin formato](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Páginas de preguntas más frecuentes con vínculos 

En este tipo de página de preguntas más frecuentes, las preguntas se agregan juntas y se vinculan a respuestas que están en secciones diferentes de la misma página o en páginas distintas.

A continuación se muestra un ejemplo de una página de preguntas más frecuentes con vínculos en las secciones que se encuentran en la misma página:

 ![Página de preguntas más frecuentes con vínculos de sección](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>Páginas de preguntas más frecuentes con una página principal de temas

Este tipo de preguntas más frecuentes tiene una página principal con los temas, donde cada tema es un vínculo a su pregunta y respuesta relevante en una página diferente. Aquí, QnA Maker rastrea todas las páginas vinculadas para extraer las preguntas y respuestas correspondientes.

A continuación se muestra un ejemplo de una página de preguntas más frecuentes en la cual una página principal de temas tiene vínculos a secciones de preguntas más frecuentes en páginas distintas. 

 ![Página de preguntas más frecuentes con vínculo profundo](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>Archivos PDF/DOC

QnA Maker puede procesar contenido semiestructurado en un archivo PDF o de documento (DOC) y convertirlo en preguntas y respuestas. Un buen archivo que pueda extraerse correctamente es aquel en el cual el contenido está organizado de alguna forma estructurada y se representa en secciones bien definidas. Las secciones pueden dividirse adicionalmente en subsecciones o subtemas. La extracción funciona mejor en documentos que tienen una estructura clara con encabezados jerárquicos.

QnA Maker identifican las secciones y subsecciones, así como las relaciones, en el archivo según las indicaciones visuales, como tamaño de fuente, estilo de fuente, numeración, colores, etc. Archivos PDF o de documento (DOC) semiestructurados podrían ser manuales, preguntas más frecuentes, instrucciones, directivas, folletos, prospectos y muchos otros tipos de archivos. A continuación se muestran algunos tipos de ejemplos de estos archivos.

### <a name="product-manuals"></a>Manuales de productos

Un manual suele ser material de orientación que acompaña a un producto. Ayuda al usuario a configurar, usar y mantener el producto y a solucionar problemas con el mismo. Cuando QnA Maker procesa un manual, extrae los títulos y subtítulos como preguntas y el contenido ulterior como respuestas. Consulte un ejemplo [aquí](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

A continuación se muestra un ejemplo de un manual con una página de índice y contenido jerárquico

 ![Ejemplo de manual de producto](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> La extracción funciona mejor en manuales que tienen una tabla de contenido o una página de índice y una estructura clara con títulos jerárquicos.

### <a name="brochures-guidelines-papers-and-other-files"></a>Folletos, instrucciones, documentos y otros archivos

También se pueden procesar muchos otros tipos de documentos para generar pares de preguntas y respuestas, siempre que tengan una estructura y un diseño claros. Entre ellos se incluyen: folletos, instrucciones, informes, documentos técnicos, documentos científicos, directivas, libros, etc. Consulte un ejemplo [aquí](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

A continuación se muestra un ejemplo de documento semiestructurado, sin un índice:

 ![Documento semiestructurado de Azure Blob Storage](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Documento de preguntas y respuestas estructurado

El formato de tipo pregunta-respuestas estructurado en los archivos de documento (DOC) consiste en alternar preguntas y respuestas por línea, una pregunta por línea seguida de su respuesta en la línea siguiente, tal como se muestra a continuación: 

```text
Question1

Answer1

Question2

Answer2
```

A continuación se muestra un ejemplo de un documento de Word de preguntas y respuestas estructurado:

 ![Documento de preguntas y respuestas estructurado](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Archivos *TXT*, *TSV* y *XLS* estructurados

Las preguntas y respuestas en forma de archivos *.txt*, *.tsv* o *.xls* estructurados también se pueden cargar en QnA Maker para crear o ampliar una base de conocimiento.  Pueden ser texto sin formato o pueden tener contenido en formato RTF o HTML. 

| Pregunta  | Respuesta  | Metadatos                |
|-----------|---------|-------------------------|
| Pregunta1 | Respuesta1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 |      `Key:Value`           |

Se ignoran las columnas adicionales del archivo de origen.

A continuación se muestra un ejemplo de archivo *.xls* de preguntas y respuestas estructurado, con contenido HTML:

 ![Excel de preguntas y respuestas estructurado](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>Formato de datos estructurado mediante importación

Al importar una base de conocimiento, se reemplaza el contenido de la base de conocimiento existente. La importación requiere un archivo .tsv estructurado que contiene información del origen de datos. Esta información ayuda a QnA Maker a agrupar los pares pregunta-respuesta y a atribuirlos a un origen de datos particular.

| Pregunta  | Respuesta  | Origen| Metadatos                |
|-----------|---------|----|---------------------|
| Pregunta1 | Respuesta1 | URL1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 | Editorial|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Adición en modo editorial a la base de conocimiento

Si no tiene contenido existente previamente para rellenar la base de conocimiento, puede agregar preguntas y respuestas a nivel editorial en la base de conocimiento de QnA Maker. Consulte [aquí](../How-To/edit-knowledge-base.md) cómo actualizar la base de conocimiento.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configurar un servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Otras referencias 

[Introducción de QnA Maker](../Overview/overview.md)
