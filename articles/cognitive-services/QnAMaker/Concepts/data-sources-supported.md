---
title: 'Orígenes de datos compatibles: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker extrae automáticamente pares de preguntas y respuestas del contenido semiestructurado, como preguntas más frecuentes, manuales de productos, directrices, documentos de soporte técnico y directivas almacenadas como páginas web, archivos PDF o archivos de documento (DOC) de MS Word. El contenido también se puede agregar a la base de conocimiento de archivos de contenido de preguntas y respuestas estructurados.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/04/2019
ms.author: tulasim
ms.openlocfilehash: b30f910b7eebabe02443e5b3ce328a89a25c7a6f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213708"
---
# <a name="data-sources-for-qna-maker-content"></a>Orígenes de datos para el contenido de QnA Maker

QnA Maker extrae automáticamente pares de preguntas y respuestas del contenido semiestructurado, como preguntas más frecuentes, manuales de productos, directrices, documentos de soporte técnico y directivas almacenadas como páginas web, archivos PDF o archivos de documento (DOC) de MS Word. El contenido también se puede agregar a la base de conocimiento de archivos de contenido de preguntas y respuestas estructurados. 

En la tabla siguiente se resumen los tipos de contenido y los formatos de archivo que son compatibles con QnA Maker.

|Tipo de origen|Tipo de contenido| Ejemplos|
|--|--|--|
|URL|Preguntas más frecuentes<br> (sin formato, con secciones o con una página principal de temas)<br>Páginas de soporte técnico <br> (artículos paso a paso de una sola página, artículos de solución de problemas, etc.)|[Preguntas más frecuentes sin formato](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[preguntas más frecuentes con vínculos](https://www.microsoft.com/software-download/faq),<br> [página principal de preguntas más frecuentes por temas](https://support.microsoft.com/products/windows?os=windows-10)<br>[artículo de soporte técnico](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Preguntas más frecuentes,<br> manual de producto,<br> folletos,<br> documento,<br> directiva de folleto,<br> guía de soporte técnico,<br> archivo de preguntas y respuestas estructurado,<br> etc.|[Archivo de preguntas y respuestas estructurado.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Manual de producto de ejemplo.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Archivo semiestructurado de ejemplo.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Notas del producto de ejemplo.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)|
|Excel|Archivo de preguntas y respuestas estructurado<br> (incluye compatibilidad con RTF y HTML)|[Preguntas más frecuentes de preguntas y respuestas de ejemplo.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Archivo de preguntas y respuestas estructurado|[Charla de ejemplo.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

## <a name="data-source-locations"></a>Ubicaciones de orígenes de datos

Solo son válidas las direcciones URL públicas para todos los orígenes de datos. No envíe orígenes de datos que requieran autenticación. Puede descargar el archivo desde el sitio autenticado y luego utilizar la opción de carga de archivos para extraer preguntas y respuestas.

## <a name="faq-urls"></a>Preguntas más frecuentes sobre las direcciones URL

QnA Maker puede admitir páginas web de preguntas más frecuentes de 3 formas diferentes: Páginas de preguntas más frecuentes sin formato, páginas de preguntas más frecuentes con vínculos, páginas de preguntas más frecuentes con una página principal de temas.

### <a name="plain-faq-pages"></a>Páginas de preguntas más frecuentes sin formato

Este es el tipo más común de página de preguntas más frecuentes, en la que las respuestas siguen inmediatamente las preguntas de la misma página. 

Aquí tiene un ejemplo de una página de preguntas más frecuentes sin formato:

![Ejemplo de página de preguntas más frecuentes sin formato para una base de conocimiento](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Páginas de preguntas más frecuentes con vínculos 

En este tipo de página de preguntas más frecuentes, las preguntas se agregan juntas y se vinculan a respuestas que están en secciones diferentes de la misma página o en páginas distintas.

A continuación se muestra un ejemplo de una página de preguntas más frecuentes con vínculos en las secciones que se encuentran en la misma página:

 ![Ejemplo de página de preguntas más frecuentes con sección de vínculos para una base de conocimiento](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>Páginas de preguntas más frecuentes con una página principal de temas

Este tipo de preguntas más frecuentes tiene una página principal con los temas, donde cada tema es un vínculo a su pregunta y respuesta relevante en una página diferente. Aquí, QnA Maker rastrea todas las páginas vinculadas para extraer las preguntas y respuestas correspondientes.

A continuación se muestra un ejemplo de una página de preguntas más frecuentes en la cual una página principal de temas tiene vínculos a secciones de preguntas más frecuentes en páginas distintas. 

 ![Ejemplo de página de preguntas más frecuentes con vínculos profundos para una base de conocimiento](../media/qnamaker-concepts-datasources/topics-faq.png) 


### <a name="support-urls"></a>Direcciones URL de soporte técnico

QnA Maker puede procesar páginas web de soporte semiestructuradas, como artículos web que describan cómo realizar una tarea determinada, cómo diagnosticar y resolver un problema determinado y cuáles son las mejores prácticas para un determinado proceso. La extracción funciona mejor en contenido con una estructura clara y con encabezados jerárquicos.

> [!NOTE]
> La extracción para artículos de soporte técnico es una característica nueva que se encuentra en sus primeras etapas. Funciona mejor con páginas simples, que están bien estructuradas y no contienen encabezados ni pies de página complejos.

![QnA Maker admite la extracción en páginas web semiestructuradas con una estructura clara y con encabezados jerárquicos.](../media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)


## <a name="pdf-doc-files"></a>Archivos PDF/DOC

QnA Maker puede procesar contenido semiestructurado en un archivo PDF o de documento (DOC) y convertirlo en preguntas y respuestas. Un buen archivo que pueda extraerse correctamente es aquel en el cual el contenido está organizado de alguna forma estructurada y se representa en secciones bien definidas. Las secciones pueden dividirse adicionalmente en subsecciones o subtemas. La extracción funciona mejor en documentos que tienen una estructura clara con encabezados jerárquicos.

QnA Maker identifican las secciones y subsecciones, así como las relaciones, en el archivo según las indicaciones visuales, como tamaño de fuente, estilo de fuente, numeración, colores, etc. Archivos PDF o de documento (DOC) semiestructurados podrían ser manuales, preguntas más frecuentes, instrucciones, directivas, folletos, prospectos y muchos otros tipos de archivos. A continuación se muestran algunos tipos de ejemplos de estos archivos.

### <a name="product-manuals"></a>Manuales de productos

Un manual suele ser material de orientación que acompaña a un producto. Ayuda al usuario a configurar, usar y mantener el producto y a solucionar problemas con el mismo. Cuando QnA Maker procesa un manual, extrae los títulos y subtítulos como preguntas y el contenido ulterior como respuestas. Consulte un ejemplo [aquí](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

A continuación se muestra un ejemplo de un manual con una página de índice y contenido jerárquico

 ![Ejemplo de manual del producto para una base de conocimiento](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> La extracción funciona mejor en manuales que tienen una tabla de contenido o una página de índice y una estructura clara con títulos jerárquicos.

### <a name="brochures-guidelines-papers-and-other-files"></a>Folletos, instrucciones, documentos y otros archivos

También se pueden procesar muchos otros tipos de documentos para generar pares de preguntas y respuestas, siempre que tengan una estructura y un diseño claros. Entre ellas se incluyen las siguientes: folletos, instrucciones, informes, documentos técnicos, documentos científicos, directivas, libros, etc. Consulte un ejemplo [aquí](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

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

 ![Ejemplo de documento de QnA estructurado para una base de conocimiento](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Archivos *TXT*, *TSV* y *XLS* estructurados

Las preguntas y respuestas en forma de archivos *.txt*, *.tsv* o *.xls* estructurados también se pueden cargar en QnA Maker para crear o ampliar una base de conocimiento.  Pueden ser texto sin formato o pueden tener contenido en formato RTF o HTML. 

| Pregunta  | Respuesta  | Metadatos                |
|-----------|---------|-------------------------|
| Pregunta1 | Respuesta1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 |      `Key:Value`           |

Se ignoran las columnas adicionales del archivo de origen.

A continuación se muestra un ejemplo de archivo *.xls* de preguntas y respuestas estructurado, con contenido HTML:

 ![Ejemplo de libro Excel de QnA estructurado para una base de conocimiento](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>Formato de datos estructurado mediante importación

Al importar una base de conocimiento, se reemplaza el contenido de la base de conocimiento existente. La importación requiere un archivo .tsv estructurado que contiene información del origen de datos. Esta información ayuda a QnA Maker a agrupar los pares pregunta-respuesta y a atribuirlos a un origen de datos particular.

| Pregunta  | Respuesta  | Origen| Metadatos                |
|-----------|---------|----|---------------------|
| Pregunta1 | Respuesta1 | URL1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 | Editorial|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Adición en modo editorial a la base de conocimiento

Si no tiene contenido existente previamente para rellenar la base de conocimiento, puede agregar preguntas y respuestas a nivel editorial en la base de conocimiento de QnA Maker. Consulte [aquí](../How-To/edit-knowledge-base.md) cómo actualizar la base de conocimiento.

## <a name="formatting-considerations"></a>Consideraciones de formato

Después de importar un archivo o una dirección URL, se convierte al formato Markdown y se almacena en ese formato. Si el proceso de conversión no convierte correctamente los vínculos de los archivos y direcciones URL, debe editar las preguntas y respuestas en la página **Editar**. 

|Formato|Propósito|
|--|--|
|`\n\n`| Línea nueva|
|`\n*`|Viñeta de una lista ordenada|

## <a name="editing-your-knowledge-base-locally"></a>Edición de la base de conocimiento localmente

Una vez que se crea una base de conocimiento, es recomendable realizar modificaciones en el texto de dicha base en el [portal de QnA Maker](https://qnamaker.ai), en lugar de exportar y volver a importar a través de archivos locales. Sin embargo, puede haber ocasiones en las que necesite editar una base de conocimiento localmente. 

Exporte la base de conocimiento desde la página **Configuración** y luego edite dicha base con Microsoft Excel. Si decide utilizar otra aplicación para editar el archivo TSV exportado, la aplicación puede introducir errores de sintaxis porque no es totalmente compatible con TSV. Por lo general, los archivos TSV de Microsoft Excel no introducen errores de formato. 

Cuando termine de realizar las modificaciones, vuelva a importar el archivo TSV desde la página **Configuración**. Esto reemplazará completamente la base de conocimiento actual por la base de conocimiento importada. 

## <a name="testing-your-markdown"></a>Prueba de Markdown

Use el tutorial **[CommonMark](https://commonmark.org/help/tutorial/index.html)** para validar su Markdown. El tutorial tiene una característica **Pruébelo** para la validación rápida de copiar y pegar. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configurar un servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Otras referencias 

[Introducción de QnA Maker](../Overview/overview.md)
