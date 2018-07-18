---
title: Idiomas admitidos - QnA Maker - Azure Cognitive Services | Microsoft Docs
description: Obtenga información acerca de qué idiomas se admiten en QnA Maker.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b210f59129a962046787b27d003c2872a54f6c8e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382383"
---
# <a name="supported-languages"></a>Idiomas admitidos

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
QnA Maker depende de [analizadores de idioma](https://docs.microsoft.com/en-us/rest/api/searchservice/language-support) en la búsqueda de Azure para proporcionar resultados. Tiene disponibles características especiales que le permitirán rehacer las clasificaciones en idiomas de tipo En- *, y gracias a las cuales podrá obtener una relevancia mejor.

QnA Maker detecta automáticamente el idioma de la base de conocimiento durante la creación, y configura el analizador en consecuencia. Puede crear bases de conocimiento en los siguientes idiomas. Lea [esto](../How-To/language-knowledge-base.md) para obtener más detalles sobre cómo maneja los idiomas QnA Maker.


> [!Tip]
> Una vez configurados los analizadores de idiomas, no podrá modificarlos. Asimismo, el analizador de lenguaje se aplica a todas las bases de conocimiento en un [servicio de QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Si planea tener bases de conocimiento en diferentes idiomas, debe crearlas en servicios separados de QnA Maker.

|Idiomas admitidos|
|-----|
|Árabe|
|Armenio|.
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
