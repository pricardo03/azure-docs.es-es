---
title: 'Idiomas admitidos: QnA Maker'
titleSuffix: Azure Cognitive Services
description: El idioma de una base de conocimiento afecta a la capacidad de QnA Maker de extraer automáticamente preguntas y respuestas de los orígenes, así como la relevancia de los resultados que QnA Maker proporciona en respuesta a las consultas de los usuarios. Una lista de referencias culturales y lenguajes naturales admitidos por QnA Maker para la base de conocimiento. No mezcle idiomas en la misma base de conocimiento.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: bec09e5cd053774902083c6dd042c1ed4b293d20
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066680"
---
# <a name="language-support-for-qna-maker"></a>Idiomas admitidos para QnA Maker

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

## <a name="primary-language-detection"></a>Detección del idioma principal

El idioma principal que se usa para la detección se establece para el recurso de QnA Maker, y para todas las bases de conocimiento creadas en ese recurso, cuando se agrega el primer documento o dirección URL a la primera base de conocimiento. No se puede cambiar el idioma. 

Si el usuario tiene previsto admitir varios idiomas, deben tener un nuevo recurso de QnA Maker para cada uno. Aprenda a [crear una base de conocimiento de QnA Maker basada en el idioma](../how-to/language-knowledge-base.md).  

Compruebe el idioma principal con los pasos siguientes:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).  
1. Busque y seleccione el recurso de Azure Search creado como parte del recurso de QnA Maker. El nombre del recurso de Azure Search comenzará con el mismo nombre que el recurso de QnA Maker y tendrá el tipo **servicio de búsqueda**. Tenga en cuenta que solo se puede vincular un recurso de QnA Maker a un recurso de Azure Search.
1. En la página **Información general** del recurso de búsqueda, seleccione **Índices**. 
1. Seleccione el índice **testkb**.
1. Seleccione la pestaña **Campos**. 
1. Vea la columna **Analizador** de los campos de **preguntas** y **respuestas**. 


## <a name="query-matching-and-relevance"></a>Coincidencia y relevancia de las consultas
QnA Maker depende de [analizadores de idioma](https://docs.microsoft.com/rest/api/searchservice/language-support) en la búsqueda de Azure para proporcionar resultados. Tiene disponibles características especiales que le permitirán rehacer las clasificaciones en idiomas de tipo En- *, y gracias a las cuales podrá obtener una relevancia mejor.

Aunque las funcionalidades de Azure Search están en el mismo nivel que los idiomas admitidos, QnA Maker tiene un clasificador adicional que está por encima de los resultados de búsqueda de Azure. En este modelo de clasificador, usamos una semántica especial y algunas características basadas en palabras con en-* que todavía no están disponibles para otros idiomas. Esos recursos no están disponibles, ya que forman parte del trabajo interno del clasificador de QnA Maker. 

QnA Maker [detecta automáticamente el idioma de la base de conocimiento](#primary-language-detection) durante la creación, y configura el analizador en consecuencia. Puede crear bases de conocimiento en los siguientes idiomas. 

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
