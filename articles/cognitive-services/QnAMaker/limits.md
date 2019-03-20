---
title: 'Límites: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker tiene meta-límites para partes de la base de conocimiento y el servicio. Es importante mantener la base de conocimiento dentro de esos límites para probar y publicar.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/26/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: fe15cafceea8128735f7241fa5e4187d4d9c47a9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960061"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Límites de la base de conocimiento de QnA Maker
Lista completa de los límites de QnA Maker.

## <a name="knowledge-bases"></a>Bases de conocimiento

* Número máximo de bases de conocimiento en función de los [límites de plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Plan de Azure Search** | **Gratis** | **Básico** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo permitido de bases de conocimiento publicadas|2|14|49|199|199|2999|

 Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 bases de conocimiento (1 índice por base de conocimiento publicada). El índice decimoquinto, `testkb`, se usa con todas las bases de conocimiento para creación y prueba. 

## <a name="extraction-limits"></a>Límites de extracción
* Número máximo de archivos que se pueden extraer y tamaño máximo del archivo: Consulte [Precios de QnAMaker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)
* Número máximo de vínculos profundos que se pueden rastrear para la extracción de QnA desde las páginas HTML de preguntas más frecuentes: 20

## <a name="metadata-limits"></a>Límites de metadatos
* Número máximo de campos de metadatos por base de conocimiento en función de los [límites de plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Plan de Azure Search** | **Gratis** | **Básico** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo de metadatos por servicio QnA Maker (en todas las KB)|1000|100*|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Límites de contenido de la base de conocimiento
Límites generales del contenido de la base de conocimiento:
* Longitud del texto de la respuesta: 25 000
* Longitud del texto de la pregunta: 1000
* Longitud del texto de clave y valor de metadatos: 100
* Caracteres admitidos para el nombre de metadatos: Alfabéticos, dígitos y _  
* Caracteres admitidos para el valor de metadatos: Todos excepto : y | 
* Longitud del nombre de archivo: 200
* Formatos de archivo admitidos: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Número máximo de preguntas alternativas: 100
* Número máximo de pares de preguntas y respuestas: Depende de la [nivel de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) elegido. Un par de preguntas y respuestas se asigna a un documento en el índice de Azure Search. 

## <a name="create-knowledge-base-call-limits"></a>Límites de llamada de creación de la base de conocimiento:
Representan los límites para cada acción de creación de base de conocimiento; es decir, al hacer clic en *Crear una base de conocimiento* o al llamar a la API CreateKnowledgeBase.
* Número máximo de preguntas alternativas por respuesta: 100
* Número máximo de direcciones URL: 10
* Número máximo de archivos: 10

## <a name="update-knowledge-base-call-limits"></a>Límites de llamadas de actualización de la base de conocimiento
Representan los límites para cada acción de actualización; es decir, al hacer clic en *Save and train* (Guardar y entrenar) o al llamar a la API UpdateKnowledgeBase.
* Longitud de cada nombre de origen: 300
* Número máximo de preguntas alternativas agregadas o eliminadas: 100
* Número máximo de campos de metadatos agregados o eliminados: 10
* Número máximo de direcciones URL que se pueden actualizar: 5

## <a name="next-steps"></a>Pasos siguientes

Aprenda cuándo y cómo cambiar los niveles de servicio:

* [QnA Maker](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): Cuando necesite tener más archivos de código fuente o documentos más grandes en la base de conocimiento, más allá de su nivel actual, actualización el servicio QnA Maker plan de tarifa.
* [Servicio de aplicación](how-to/upgrade-qnamaker-service.md#upgrade-app-service): Cuando Knowledge Base deba atender más solicitudes de la aplicación cliente, actualice el plan de tarifa de App Service.
* [Azure Search](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): Si planea tener muchas instancias de Knowledge Base, actualice el plan de tarifa del servicio de Azure Search.
