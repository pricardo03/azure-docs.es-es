---
title: Idiomas admitidos - Translator Speech API
titlesuffix: Azure Cognitive Services
description: Vea los idiomas admitidos en Translator Speech API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 3/5/2018
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ed8f693e4dc0344a0117ae9d6992b925992ef0c4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446925"
---
# <a name="languages-supported-by-the-translator-speech-api"></a>Idiomas admitidos en Translator Speech API

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

La traducción de voz admite los siguientes idiomas. Si ambos idiomas son compatibles con la traducción de voz, estará disponible tanto la función voz a voz como la de voz a texto. Si no se admite el idioma de destino para la traducción de voz, solo estará disponible la función de voz a texto.

| Idioma de voz    |
|:----------- |
| Árabe (estándar moderno)      |
| Portugués (brasileño)     |
| Chino (mandarín)      |
| English      |
| Francés      |
| Alemán      |
| Italiano      |
| Japonés      |
| Ruso      |
| Español      |

Translator Speech API admite los siguientes idiomas como idioma de destino para la conversión de voz en texto.

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
| Kiswahili      | `sw`          |
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

Para ver la lista de idiomas admitidos mediante programación, use el recurso Languages. La lista proporciona el código de idioma, así como el nombre del idioma en inglés (o en cualquier otro idioma admitido). El servicio Translator Speech actualiza esta lista automáticamente cuando hay nuevos idiomas disponibles.

El recurso Languages devuelve la lista de idiomas admitidos para la voz, el texto y el texto a voz. El recurso Languages no requiere autenticación.

[Visite la referencia de API para probar el método de idiomas](languages-reference.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Acceso a la lista en el sitio web de Microsoft Translator

Para echar un vistazo rápido a los idiomas, el sitio web de Microsoft Translator muestra todos los idiomas admitidos por Translator Text API y Speech API. Esta lista no incluye información específica para desarrolladores, por ejemplo, los códigos de idioma.

[Ver la lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
