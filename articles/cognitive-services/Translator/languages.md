---
title: Idiomas admitidos en Microsoft Translator Text API | Microsoft Docs
description: Ver los idiomas admitidos en Microsoft Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: v-jansko
ms.openlocfilehash: 5542c1c0eee5e4c7c85c566908dc8999b118f06e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37032444"
---
# <a name="supported-languages-in-the-microsoft-translator-text-api"></a>Idiomas admitidos en Microsoft Translator Text API

Microsoft Translator Text API admite los siguientes idiomas para la conversión de texto a texto. La traducción automática neuronal (NMT) es el nuevo estándar de traducción automática de alta calidad con tecnologías de inteligencia artificial, y está disponible de forma predeterminada con la versión V3 de Translator Text API cuando hay un sistema neuronal disponible. La traducción automática neuronal está disponible en V2 con la categoría "generalnn".

[Más información sobre cómo funciona la traducción automática](https://www.microsoft.com/translator/mt.aspx)

| Idioma    | Tipo de traducción |Código de idioma |
|:----------- |:-------:|:-------------:|
| Afrikáans      | Estadística |`af`          |
| Árabe      | Neuronal | `ar`          |
| Árabe, levantino    | Neuronal | `apc`
| Bangla      | Neuronal |`bn`          |
| Bosnio (latino)      | Estadística |`bs`          |
| Búlgaro     |  Neuronal |`bg`          |
| Cantonés (tradicional)      | Estadística |`yue`          |
| Catalán      | Estadística |`ca`          |
| Chino simplificado        |  Neuronal |`zh-Hans`          | 
| Chino tradicional        |  Neuronal |`zh-Hant`          |
| Croata      | Neuronal |`hr`          |
| Checo        |  Neuronal |`cs`          |
| Danés        |  Neuronal |`da`          |
| Neerlandés        |  Neuronal |`nl`          |
| English       |  Neuronal |`en`          |
| Estonio      | Neuronal |`et`          |
| Fiyiano      | Estadística |`fj`          |
| Filipino      | Estadística |`fil`          |
| Finés      | Neuronal |`fi`          |
| Francés        |  Neuronal |`fr`          |
| Alemán       |  Neuronal |`de`          |
| Griego      | Neuronal |`el`          |
| Criollo haitiano      | Estadística |`ht`          |
| Hebreo      | Neuronal |`he`          |
| Hindi        |  Neuronal |`hi`          |
| Hmong daw      | Estadística |`mww`          |
| Húngaro      | Neuronal |`hu`          |
| Islandés      |  Neuronal |`is`           |
| Indonesio      | Estadística |`id`          |
| Italiano        |  Neuronal |`it`          |
| Japonés        |  Neuronal |`ja`          |
| Kiswahili      | Estadística |`sw`          |
| Klingon      | Estadística |`tlh`          |
| Klingon (plqaD)      | Estadística |`tlh-Qaak`          |
| Coreano        |  Neuronal |`ko`          |
| Letón      | Neuronal |`lv`          |
| Lituano      | Neuronal |`lt`          |
| Malgache      | Estadística |`mg`          |
| Malayo      | Estadística |`ms`          |
| Maltés      | Estadística |`mt`          |
| Noruego        |  Neuronal |`nb`          |
| Persa      | Estadística |`fa`          |
| Polaco        |  Neuronal |`pl`          |
| Portugués        |  Neuronal |`pt`          |
| Otomí Querétaro      | Estadística |`otq`          |
| Rumano        |  Neuronal |`ro`          |
| Ruso        |  Neuronal |`ru`          |
| Samoano      | Estadística |`sm`          |
| Serbio (cirílico)      | Estadística |`sr-Cyrl`          |
| Serbio (latino)      | Estadística |`sr-Latn`          |
| Eslovaco     | Neuronal |`sk`          |
| Esloveno      | Neuronal |`sl`          |
| Español        |  Neuronal |`es`          |
| Sueco        |  Neuronal |`sv`          |
| Tahitiano      | Estadística |`ty`          |
| Tamil      | Estadística |`ta`          |
| Tailandés      | Neuronal |`th`          |
| Tongano      | Estadística |`to`          |
| Turco       |  Neuronal |`tr`          |
| Ucraniano      | Neuronal |`uk`          |
| Urdu      | Estadística |`ur`          |
| Vietnamita      | Neuronal |`vi`          |
| Galés      | Neuronal |`cy`          |
| Maya Yucateco      | Estadística |`yua`          |

## <a name="transliteration"></a>Transliteración

El método Transliterate admite los siguientes idiomas. En "Hacia/Desde", "<-->" indica que el idioma se puede transliterar hacia o desde cualquiera de los alfabetos enumerados. "-->" indica que el idioma solo se puede transliterar de un idioma al otro.

| Idioma    | Código de idioma | Script | Hacia/Desde | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Árabe | ar | Árabe | <--> | Latín |
|Bangla  | bn | Bengalí | <--> | Latín |
| Chino (simplificado) | zh-Hans | Chino simplificado | <--> | Latín |
| Chino (simplificado) | zh-Hans | Chino simplificado | <--> | Chino tradicional |
| Chino (tradicional) | zh-Hant | Chino tradicional | <--> | Latín |
| Chino (tradicional) | zh-Hant | Chino tradicional | <--> | Chino simplificado |
| Gujarati | gu  | Gujarati | --> | Latín |
| Hebreo | he | Hebreo | <--> | Latín |
| Hindi | hi | Devanagari | <--> | Latín |
| Japonés | ja | Japonés | <--> | Latín |
| Canarés | kn | Canarés | --> | Latín |
| Malayo | ml | Malayalam | --> | Latín |
| Maratí | mr | Devanagari | --> | Latín |
| Odia | o | Odia | <--> | Latín |
| Punjabi | pa | Gurmukhi | <--> | Latín  |
| Serbio (cirílico) | sr-Cyrl | Cirílico  | --> | Latín |
| Serbio (latino) | sr-Latn | Latín | --> | Cirílico |
| Tamil | ta | Tamil | --> | Latín |
| Telugu | te | Telugu | --> | Latín |
| Tailandés | th | Tailandés | <--> | Latín |

## <a name="dictionary"></a>Diccionario

El diccionario admite los siguientes idiomas desde o hacia el inglés con los métodos Lookup y Examples.

| Idioma    | Código de idioma |
|:----------- |:-------------:|
| Afrikáans      | `af`          |
| Árabe       | `ar`          |
| Bangla      | `bn`          |
| Bosnio (latino)      | `bs`          |
| Búlgaro      | `bg`          |
| Catalán      | `ca`          |
| Chino simplificado      | `zh-Hans`          | 
| Croata      | `hr`          |
| Checo      | `cs`          |
| Danés      | `da`          |
| Neerlandés      | `nl`          |
| Estonio      | `et`          |
| Finés      | `fi`          |
| Francés      | `fr`          |
| Alemán      | `de`          |
| Griego      | `el`          |
| Criollo haitiano      | `ht`          |
| Hebreo      | `he`          |
| Hindi      | `hi`          |
| Hmong daw      | `mww`          |
| Húngaro      | `hu`          |
| Islandés    | `is`  |
| Indonesio      | `id`          |
| Italiano      | `it`          |
| Japonés      | `ja`          |
| Kiswahili      | `sw`          |
| Klingon      | `tlh`          |
| Coreano      | `ko`          |
| Letón      | `lv`          |
| Lituano      | `lt`          |
| Malayo      | `ms`          |
| Maltés      | `mt`          |
| Noruego      | `nb`          |
| Persa      | `fa`          |
| Polaco      | `pl`          |
| Portugués      | `pt`          |
| Rumano      | `ro`          |
| Ruso      | `ru`          |
| Serbio (latino)      | `sr-Latn`          |
| Eslovaco     | `sk`          |
| Esloveno      | `sl`          |
| Español      | `es`          |
| Sueco      | `sv`          |
| Tamil      | `ta`          |
| Tailandés      | `th`          |
| Turco      | `tr`          |
| Ucraniano      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Galés      | `cy`          |

## <a name="languages-detected-by-the-detect-method"></a>Idiomas detectados por el método Detect

Los siguientes idiomas pueden detectarse con el método Detect. Detect puede detectar idiomas que Microsoft Translator no puede traducir.

| Idioma    |
|:----------- |
| Afrikáans |
| Albanés |
| Árabe |
| Vasco |
| Bielorruso |
| Búlgaro |
| Catalán |
| Chino |
| Chino simplificado |
| Chino tradicional |
| Croata |
| Checo |
| Danés |
| Neerlandés |
| English |
| Esperanto |
| Estonio |
| Finés |
| Francés |
| Gallego |
| Alemán |
| Griego |
| Criollo haitiano |
| Hebreo |
| Hindi |
| Húngaro |
| Islandés |
| Indonesio |
| Irlandés |
| Italiano |
| Japonés |
| Coreano |
| Kurdo árabe |
| Kurdo latín |
| Latín |
| Letón |
| Lituano |
| Macedonio |
| Malayo |
| Maltés |
| Noruego |
| Noruego nynorsk |
| Pashto |
| Persa |
| Polaco |
| Portugués |
| Rumano |
| Ruso |
| Serbio cirílico |
| Serbio latín |
| Eslovaco |
| Esloveno |
| Somalí |
| Español |
| Swahili |
| Sueco |
| Tagalo |
| Tailandés |
| Turco |
| Ucraniano |
| Urdu |
| Uzbeko cirílico |
| Uzbeko latín |
| Vietnamita |
| Galés |
| Yidis |

## <a name="access-the-list-programmatically"></a>Acceso a la lista mediante programación

Para ver la lista de idiomas admitidos mediante programación, use la operación Languages de Text API V3.0. Puede ver la lista por característica, código de idioma o por nombre del idioma en inglés o en cualquier otro idioma admitido. El servicio Microsoft Translator actualiza esta lista automáticamente cuando hay nuevos idiomas disponibles.

[Ver la documentación de referencia de la operación Languages](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Acceso a la lista en el sitio web de Microsoft Translator

Para echar un vistazo rápido a los idiomas, el sitio web de Microsoft Translator muestra todos los idiomas admitidos por Translator Text API y Speech API. Esta lista no incluye información específica para desarrolladores, por ejemplo, los códigos de idioma.

[Ver la lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
