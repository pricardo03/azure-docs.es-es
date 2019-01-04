---
title: 'Límites: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker tiene meta-límites para partes de la base de conocimiento y el servicio. Es importante mantener la base de conocimiento dentro de esos límites para probar y publicar.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 772153040ac76f4b7bbee55c48527a841fc69037
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084795"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Límites de la base de conocimiento de QnA Maker
Lista completa de los límites de QnA Maker.

## <a name="knowledge-bases"></a>Bases de conocimiento

* Número máximo de bases de conocimiento en función de los [límites de plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Plan de Azure Search** | **Gratis** | **Básico** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo de bases de conocimiento publicadas permitido (Máximo de índices: 1) (reservado para las pruebas)|2|14|49|199|199|2999|

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
* Número máximo de pares de preguntas y respuestas: en función del [plan de Azure Search](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) elegido 

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
