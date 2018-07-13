---
title: Idiomas admitidos en Microsoft Translator Speech API | Microsoft Docs
description: Ver los idiomas admitidos en Microsoft Translator Speech API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jansko
ms.openlocfilehash: 0d33033442a012290baa78d80f1b8bde0499b3f1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382881"
---
# <a name="languages-supported-by-the-microsoft-translator-speech-api"></a>Idiomas admitidos en Microsoft Translator Speech API
La traducción de voz admite los siguientes idiomas. Si ambos idiomas son compatibles con la traducción de voz, estará disponible tanto la función voz a voz como la de voz a texto. Si no se admite el idioma de destino para la traducción de voz, solo estará disponible la función de voz a texto. 

| Idioma de voz    |
|:----------- |
| Árabe (estándar moderno)      |
| Chino (mandarín)      |
| English      |
| Francés      |
| Alemán      |
| Italiano      |
| Japonés      |
| Portugués (Brasil)     |
| Ruso      |
| Español      | 

Microsoft Translator Speech API admite los siguientes idiomas como idioma de destino para la traducción de voz a texto. 

| Idioma de texto    | Código de idioma |
|:----------- |:-------------:|
| Afrikáans      | `af`          |
| Árabe       | `ar`          |
| Bangla      | `bn`          |
| Bosnio (latino)      | `bs`          |
| Búlgaro      | `bg`          |
| Cantonés (tradicional)      | `yue`          |
| Catalán      | `ca`          |
| Chino simplificado      | `zh-Hans`          | 
| Chino tradicional      | `zh-Hant`          |
| Croata      | `hr`          |
| Checo      | `cs`          |
| Danés      | `da`          |
| Neerlandés      | `nl`          |
| English      | `en`          |
| Estonio      | `et`          |
| Fiyiano      | `fj`          |
| Filipino      | `fil`          |
| Finés      | `fi`          |
| Francés      | `fr`          |
| Alemán      | `de`          |
| Griego      | `el`          |
| Criollo haitiano      | `ht`          |
| Hebreo      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Húngaro      | `hu`          |
|Islandés|`is`          |
| Indonesio      | `id`          |
| Italiano      | `it`          |
| Japonés      | `ja`          |
| Suajili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Coreano      | `ko`          |
| Letón      | `lv`          |
| Lituano      | `lt`          |
| Malgache      | `mg`          |
| Malayo      | `ms`          |
| Maltés      | `mt`          |
| Noruego      | `nb`          |
| Persa      | `fa`          |
| Polaco      | `pl`          |
| Portugués      | `pt`          |
| Otomí Querétaro      | `otq`          |
| Rumano      | `ro`          |
| Ruso      | `ru`          |
| Samoano      | `sm`          |
| Serbio (cirílico)      | `sr-Cyrl`          |
| Serbio (latino)      | `sr-Latn`          |
| Eslovaco     | `sk`          |
| Esloveno      | `sl`          |
| Español      | `es`          |
| Sueco      | `sv`          |
| Tahitiano      | `ty`          |
| Tamil      | `ta`          |
| Tailandés      | `th`          |
| Tongano      | `to`          |
| Turco      | `tr`          |
| Ucraniano      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Galés      | `cy`          |
| Maya Yucateco      | `yua`          |

## <a name="access-the-list-programmatically"></a>Acceso a la lista mediante programación

Para ver la lista de idiomas admitidos mediante programación, use el recurso Languages. La lista proporciona el código de idioma, así como el nombre del idioma en inglés (o en cualquier otro idioma admitido). El servicio Microsoft Translator actualiza esta lista automáticamente cuando hay nuevos idiomas disponibles.

El recurso Languages devuelve la lista de idiomas admitidos para la voz, el texto y el texto a voz. El recurso Languages no requiere autenticación.

[Visite la referencia de API para probar el método de idiomas](languages-reference.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Acceso a la lista en el sitio web de Microsoft Translator

Para echar un vistazo rápido a los idiomas, el sitio web de Microsoft Translator muestra todos los idiomas admitidos por Translator Text API y Speech API. Esta lista no incluye información específica para desarrolladores, por ejemplo, los códigos de idioma.

[Ver la lista de idiomas](https://www.microsoft.com/translator/languages.aspx) 
