---
title: 'Orígenes de datos admitidos: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Orígenes de datos admitidos
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: b888846056fd60f37cdb1da85904fa14ffe79a39
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381826"
---
# <a name="data-sources"></a>Orígenes de datos 
QnA Maker puede extraer automáticamente pares pregunta-respuesta de formatos de contenido semiestructurado comunes, como preguntas más frecuentes y manuales de productos. El contenido también se puede agregar a la base de conocimiento de archivos estructurados.

## <a name="plain-faq-pages"></a>Páginas de preguntas más frecuentes sin formato
Este es el tipo más común de página de preguntas más frecuentes, en la que las respuestas siguen inmediatamente las preguntas de la misma página. 

![Páginas de preguntas más frecuentes sin formato](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Páginas de preguntas más frecuentes con vínculos de sección 
En este tipo de página de preguntas más frecuentes, las preguntas se agregan juntas y se vinculan a respuestas que están en secciones diferentes de la misma página.

 ![Página de preguntas más frecuentes con vínculos de sección](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Páginas de preguntas más frecuentes con vínculos a distintas páginas 
Este tipo de página de preguntas más frecuentes es similar a una página de preguntas más frecuentes con vínculos de sección, salvo que los vínculos redireccionan a una página diferente. QnA Maker rastrea todas las páginas vinculadas para extraer las respuestas correspondientes.

 ![Página de preguntas más frecuentes con vínculo profundo](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Manuales de productos

Un manual suele ser material de orientación que acompaña a un producto. Ayuda al usuario a configurar, usar y mantener el producto y a solucionar problemas con el mismo. Cuando QnA Maker procesa un manual, extrae los títulos y subtítulos como preguntas y el contenido ulterior como respuestas. Consulte un ejemplo [aquí](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> La extracción funciona mejor en manuales que tienen una tabla de contenido o una página de índice y una estructura clara con títulos jerárquicos.


## <a name="structured-data-format-through-file-upload"></a>Formato de datos estructurado mediante la carga de archivos

Los archivos estructurados como .tsv o .xlsx con columnas con formato también se pueden cargar en QnA Maker durante la creación de la base de conocimiento. También puede cargar archivos desde la pestaña **Configuración** de una base de conocimiento.

| Pregunta  | Respuesta  | Metadatos                |
|-----------|---------|-------------------------|
| Pregunta1 | Respuesta1 | `Key1:Value1\|Key2:Value2` |
| Pregunta2 | Respuesta2 |      `Key:Value`           |
Se ignoran las columnas adicionales del archivo de origen.

## <a name="structured-data-format-through-import"></a>Formato de datos estructurado mediante importación
Al importar una base de conocimiento, se reemplaza el contenido de la base de conocimiento existente. La importación requiere un archivo .tsv estructurado que contiene información del origen de datos. Esta información ayuda a QnA Maker a agrupar los pares pregunta-respuesta y a atribuirlos a un origen de datos particular.

| Pregunta  | Respuesta  | Origen| Metadatos                |
|-----------|---------|----|---------------------|
| Pregunta1 | Respuesta1 | URL1|`Key1:Value1\|Key2:Value2` |
| Pregunta2 | Respuesta2 | Editorial|    `Key:Value`       |

## <a name="editorial"></a>Editorial
Si no tiene contenido existente previamente para rellenar la base de conocimiento, puede agregarla a nivel editorial a la base de conocimiento de QnA Maker. Consulte [aquí](../How-To/edit-knowledge-base.md) cómo actualizar la base de conocimiento.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configurar un servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Otras referencias 

[Información general de QnA Maker](../Overview/overview.md)