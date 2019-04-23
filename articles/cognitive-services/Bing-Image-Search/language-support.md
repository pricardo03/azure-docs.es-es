---
title: 'Idiomas admitidos: Bing Image Search API'
titleSuffix: Azure Cognitive Services
description: Averigüe qué países o regiones e idiomas se admiten en Bing Image Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: a4c315d2696b081a4f2ac646d097e76ff9562380
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009487"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Compatibilidad de idiomas y regiones para Bing Image Search API

Bing Image Search API admite más de 36 países o regiones, muchos de ellos con más de un idioma. La especificación de un país o región en una consulta sirve principalmente para refinar los resultados de la búsqueda en función de los intereses en ese país o región. Además, los resultados pueden contener vínculos a Bing, y estos vínculos pueden localizar la experiencia del usuario de Bing según el país o región especificado o el idioma.

Para especificar el país o región y el idioma, establezca el parámetro de consulta `mkt` (mercado) en un código de la tabla **Mercados** siguiente. El mercado especifica un país o región y un idioma. Si el usuario prefiere ver el texto de la pantalla en un idioma diferente, establezca el parámetro de consulta `setLang`en el código de idioma adecuado.

También puede especificar el país o región utilizando el parámetro de consulta `cc`. Si especifica un país o región, también debe especificar uno o más códigos de idioma con el encabezado HTTP `Accept-Language`. Los idiomas admitidos varían según el país o región; se facilitan para cada país en la tabla Mercados.

> [!NOTE]
> Trending Images API solo se admite actualmente en los mercados siguientes:
> - en-US (inglés, Estados Unidos)
> - en-CA (inglés, Canadá)
> - en-AU (inglés, Australia)
> - zh-CN (chino, China)

## <a name="countries"></a>Países

|País/región|Código|
|-------|----|
|Argentina|AR|
|Australia|AU|
|Austria|AT|
|Bélgica|BE|
|Brasil|BR|
|Canadá|CA|
|Chile|CL|
|Dinamarca|DK|
|Finlandia|FI|
|Francia|VF|
|Alemania|DE|
|RAE de Hong Kong|HK|
|India|IN|
|Indonesia|ID|
|Italia|IT|
|Japón|JP|
|Corea|KR|
|Malasia|MY|
|México|MX|
|Países Bajos|NL|
|Nueva Zelanda|NZ|
|Noruega|NO|
|China|CN|
|Polonia|PL|
|Portugal|PT|
|Filipinas|PH|
|Rusia|RU|
|Arabia Saudí|SA|
|Sudáfrica|ZA|
|España|ES|
|Suecia|SE|
|Suiza|CH|
|Taiwán|TW|
|Turquía|TR|
|Reino Unido|GB|
|Estados Unidos|US|


## <a name="markets"></a>Mercados

|País/región|Idioma|Código de mercado|
|-------|--------|-----------|
|Argentina|Español|es-AR|
|Australia|English|en-AU|
|Austria|Alemán|de-AT|
|Bélgica|Neerlandés|nl-BE|
|Bélgica|Francés|fr-BE|
|Brasil|Portugués|pt-BR|
|Canadá|English|en-CA|
|Canadá|Francés|fr-CA|
|Chile|Español|es-CL|
|Dinamarca|Danés|da-DK|
|Finlandia|Finés|fi-FI|
|Francia|Francés|fr-FR|
|Alemania|Alemán|de-DE|
|RAE de Hong Kong|Chino tradicional|zh-HK|
|India|English|en-IN|
|Indonesia|English|en-ID|
|Italia|Italiano|it-IT|
|Japón|Japonés|ja-JP|
|Corea|Coreano|ko-KR|
|Malasia|English|en-MY|
|México|Español|es-MX|
|Países Bajos|Neerlandés|nl-NL|
|Nueva Zelanda|English|en-NZ|
|China|Chino|zh-CN|
|Polonia|Polaco|pl-PL|
|Portugal|Portugués|pt-PT|
|Filipinas|English|en-PH|
|Rusia|Ruso|ru-RU|
|Arabia Saudí|Árabe|ar-SA|
|Sudáfrica|English|en-ZA|
|España|Español|es-ES|
|Suecia|Sueco|sv-SE|
|Suiza|Francés|fr-CH|
|Suiza|Alemán|de-CH|
|Taiwán|Chino tradicional|zh-TW|
|Turquía|Turco|tr-TR|
|Reino Unido|English|en-GB|
|Estados Unidos|English|en-US|
|Estados Unidos|Español|es-US|

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los puntos de conexión de Bing News Search, consulte [News Image Search API v7 reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) (Referencia de News Image Search API v7).
