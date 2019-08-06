---
title: 'Límites: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker tiene meta-límites para partes de la base de conocimiento y el servicio. Es importante mantener la base de conocimiento dentro de esos límites para probar y publicar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 07/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b90b4806e86ed0ba33500cf31a6ed892241ceabe
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423463"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Límites de la base de conocimiento de QnA Maker

los límites de QnA Maker que se proporcionan a continuación son una combinación de los [límites del plan de tarifa de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) y los [límites del plan de tarifa de QnA Maker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/). Debe conocer ambos conjuntos de límites para comprender el número de bases de conocimiento que puede crear por recurso y el tamaño que puede alcanzar cada base de conocimiento.

## <a name="knowledge-bases"></a>Bases de conocimiento

El número máximo de bases de conocimiento se basa en los [límites de plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity).

|**Plan de Azure Search** | **Gratis** | **Básico** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo permitido de bases de conocimiento publicadas|2|14|49|199|199|2999|

 Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 bases de conocimiento (1 índice por base de conocimiento publicada). El índice decimoquinto, `testkb`, se usa con todas las bases de conocimiento para creación y prueba. 

## <a name="extraction-limits"></a>Límites de extracción

### <a name="maximum-number-of-files"></a>Número máximo de archivos

El número máximo de archivos que se pueden extraer y tamaño máximo del archivo se basa en los **[límites del plan de tarifa de QnA Maker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** .

### <a name="maximum-number-of-deep-links-from-url"></a>Número máximo de vínculos profundos de la dirección URL

El número máximo de vínculos profundos que se pueden rastrear para la extracción de QnA desde una páginas URL es **20**.

## <a name="metadata-limits"></a>Límites de metadatos

El número máximo de campos de metadatos por base de conocimiento se basa en los **[límites de plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** .

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
* Número máximo de preguntas alternativas: 300
* Número máximo de pares de preguntas y respuestas: en función del **[plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** elegido. Un par de preguntas y respuestas se asignan a un documento en el índice de Azure Search. 
* Página HTML/URL: 1 millón de caracteres

## <a name="create-knowledge-base-call-limits"></a>Límites de llamada de creación de la base de conocimiento:
Representan los límites para cada acción de creación de base de conocimiento; es decir, al hacer clic en *Crear una base de conocimiento* o al llamar a la API CreateKnowledgeBase.
* Número máximo de preguntas alternativas por respuesta: 300
* Número máximo de direcciones URL: 10
* Número máximo de archivos: 10

## <a name="update-knowledge-base-call-limits"></a>Límites de llamadas de actualización de la base de conocimiento
Representan los límites para cada acción de actualización; es decir, al hacer clic en *Save and train* (Guardar y entrenar) o al llamar a la API UpdateKnowledgeBase.
* Longitud de cada nombre de origen: 300
* Número máximo de preguntas alternativas agregadas o eliminadas: 300
* Número máximo de campos de metadatos agregados o eliminados: 10
* Número máximo de direcciones URL que se pueden actualizar: 5

## <a name="next-steps"></a>Pasos siguientes

Aprenda cuándo y cómo cambiar los niveles de servicio:

* [QnA Maker](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): cuando necesite tener más archivos de origen o documentos más grandes en su base de conocimiento, más allá de su plan actual, actualice el plan de tarifa del servicio QnA Maker.
* [App Service](how-to/upgrade-qnamaker-service.md#upgrade-app-service): Cuando Knowledge Base deba atender más solicitudes de la aplicación cliente, actualice el plan de tarifa de App Service.
* [Azure Search](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): Si planea tener muchas instancias de Knowledge Base, actualice el plan de tarifa del servicio de Azure Search.
