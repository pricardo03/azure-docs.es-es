---
title: 'Idiomas admitidos: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Una lista de referencias culturales y lenguajes naturales admitidos por QnA Maker para la base de conocimiento. No mezcle idiomas en la misma base de conocimiento.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 4e1dbf408565e78547928047ae2ce2d37ad1a022
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105131"
---
# <a name="language-and-region-support-for-qna-maker"></a>Compatibilidad de idiomas y regiones en QnA Maker

El idioma de una base de conocimiento afecta la capacidad de QnA Maker de extraer automáticamente preguntas y respuestas de los [orígenes](../Concepts/data-sources-supported.md), así como la relevancia de los resultados que QnA Maker proporciona en respuesta a las consultas de los usuarios.

## <a name="auto-extraction"></a>Extracción automática
QnA Maker permite extraer preguntas y respuestas en cualquier página de idioma, pero la efectividad de la extracción es mucho mayor para los siguientes idiomas, ya que QnA Maker usa palabras clave para identificar preguntas.

|Idiomas admitidos| Configuración regional|
|-----|----|
|English|en-*|
|Francés|fr-*|
|Italiano|it-*|
|Alemán|de-*|
|Español|es-*|

## <a name="query-matching-and-relevance"></a>Coincidencia y relevancia de las consultas
QnA Maker depende de [analizadores de idioma](https://docs.microsoft.com/rest/api/searchservice/language-support) en la búsqueda de Azure para proporcionar resultados. Tiene disponibles características especiales que le permitirán rehacer las clasificaciones en idiomas de tipo En- *, y gracias a las cuales podrá obtener una relevancia mejor.

Aunque las funcionalidades de Azure Search están en el mismo nivel que los idiomas admitidos, QnA Maker tiene un clasificador adicional que está por encima de los resultados de búsqueda de Azure. En este modelo de clasificador, utilizamos una semántica especial y algunas características basadas en palabras con en-* que todavía no están disponibles para otros idiomas. Esos recursos no están disponibles, ya que forman parte del trabajo interno del clasificador. 

QnA Maker detecta automáticamente el idioma de la base de conocimiento durante la creación, y configura el analizador en consecuencia. Puede crear bases de conocimiento en los siguientes idiomas. Lea [esto](../How-To/language-knowledge-base.md) para obtener más detalles sobre cómo maneja los idiomas QnA Maker.


> [!Tip]
> Una vez configurados los analizadores de idiomas, no podrá modificarlos. Asimismo, el analizador de lenguaje se aplica a todas las bases de conocimiento en un [servicio de QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Si planea tener bases de conocimiento en diferentes idiomas, debe crearlas en servicios separados de QnA Maker.

|Idiomas admitidos|
|-----|
|Árabe|
|Armenio|
Bengalí|
|Vasco|
|Búlgaro|
|Catalán|
|Chino simplificado|
|Chino tradicional|
|Croata|
|Checo|
|Danés|
|Neerlandés|
|English|
|Estonio|
|Finés|
|Francés|
|Gallego|
|Alemán|
|Griego|
|Gujarati|
|Hebreo|
|Hindi|
|Húngaro|
|Islandés|
|Indonesio|
|Irlandés|
|Italiano|
|Japonés|
|Canarés|
|Coreano|
|Letón|
|Lituano|
|Malayalam|
|Malayo|
|Noruego|
|Polaco|
|Portugués|
|Punjabi|
|Rumano|
|Ruso|
|Serbio cirílico|
|Serbio latín|
|Eslovaco|
|Esloveno|
|Español|
|Sueco|
|Tamil|
|Telugu|
|Tailandés|
|Turco|
|Ucraniano|
|Urdu|
|Vietnamita|
