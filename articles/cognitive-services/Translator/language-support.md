---
title: 'Idiomas admitidos: Translator Text API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales admitidos por Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 09/25/2018
ms.author: v-jansko
ms.openlocfilehash: 0b1187083c14fc7c536f6a32f3a41957f53f299b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679722"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Compatibilidad de idiomas y regiones para Translator Text API

Translator Text API admite los siguientes idiomas para la conversión de texto a texto. La traducción automática neuronal (NMT) es el nuevo estándar de traducción automática de alta calidad con tecnologías de inteligencia artificial, y está disponible de forma predeterminada con la versión V3 de Translator Text API cuando hay un sistema neuronal disponible. 

[Más información sobre cómo funciona la traducción automática](https://www.microsoft.com/translator/mt.aspx)

**Translator API V2**

> [!NOTE]
> V2 quedó en desuso el 30 de abril de 2018 y se suspenderá el 30 de abril de 2019.

* Solo estadísticas: no hay ningún sistema neuronal disponible para este idioma.
* Traducción neuronal disponible: hay un sistema neuronal disponible. Utilice el parámetro `category=generalnn` para acceder al sistema neuronal.
* Neuronal como sistema predeterminado: el sistema de traducción neuronal es el predeterminado. Utilice el parámetro `category=smt` para acceder al sistema estadístico y utilizarlo con Microsoft Translator Hub.
* Solo neuronal: la traducción neuronal es la única que está disponible.

**Translator API V3** Translator API V3 utiliza el sistema neuronal de forma predeterminada, por lo que los sistemas estadísticos solamente están disponibles cuando no existe un sistema neuronal. La característica Traductor personalizado solo puede utilizarse con idiomas neuronales. 

|Idioma|  Código de idioma|  API V2| API V3|
|:-----|:-----:|:-----|:-----|
|Afrikáans| `af`    |Solo estadísticas|  Neuronal|
|Árabe|    `ar`    |Traducción neuronal disponible|  Neuronal|
|Árabe, levantino| `apc`   |Traducción neuronal disponible|  Neuronal|
|Bangla|    `bn`    |Traducción neuronal disponible|  Neuronal|
|Bosnio (latino)|   `bs`    |Solo estadísticas|  Estadística|
|Búlgaro| `bg`    |Traducción neuronal disponible|  Neuronal|
|Cantonés (tradicional)|   `yue`   |Solo estadísticas|  Estadística|
|Catalán|   `ca`    |Solo estadísticas|  Estadística|
|Chino simplificado|    `zh-Hans`   |Neuronal como sistema predeterminado |Neuronal|
|Chino tradicional|   `zh-Hant`   |Neuronal como sistema predeterminado |Neuronal|
|Croata|  `hr`    |Traducción neuronal disponible|  Neuronal|
|Checo| `cs`    |Traducción neuronal disponible|  Neuronal|
|Danés|    `da`    |Traducción neuronal disponible   |Neuronal|
|Neerlandés| `nl`    |Traducción neuronal disponible|  Neuronal|
|English|   `en`    |Traducción neuronal disponible|  Neuronal|
|Estonio|  `et`    |Traducción neuronal disponible|  Neuronal|
|Fiyiano|    `fj`    |Solo estadísticas|  Estadística|
|Filipino|  `fil`   |Solo estadísticas|  Estadística|
|Finés|   `fi`    |Traducción neuronal disponible|  Neuronal|
|Francés|    `fr`    |Traducción neuronal disponible|  Neuronal|
|Alemán|    `de`    |Traducción neuronal disponible|  Neuronal|
|Griego| `el`    |Traducción neuronal disponible|  Neuronal|
|Criollo haitiano|    `ht`    |Solo estadísticas   |Estadística|
|Hebreo |`he`   |Traducción neuronal disponible   |Neuronal|
|Hindi| `hi`    |Neuronal como sistema predeterminado|    Neuronal|
|Hmong Daw| `mww`   |Solo estadísticas|  Estadística|
|Húngaro| `hu`    |Traducción neuronal disponible|  Neuronal|
|Islandés| `is`    |Solo neuronal|   Neuronal|
|Indonesio|    `id`    |Solo estadísticas|  Estadística|
|Italiano|   `it`    |Traducción neuronal disponible|  Neuronal|
|Japonés|  `ja`    |Traducción neuronal disponible|  Neuronal|
|Kiswahili| `sw`    |Solo estadísticas|  Estadística|
|Klingon|   `tlh`   |Solo estadísticas|  Estadística|
|Klingon (plqaD)|   `tlh-Qaak`  |Solo estadísticas|  Estadística|
|Coreano |`ko`   |Traducción neuronal disponible|  Neuronal|
|Letón|   `lv`    |Traducción neuronal disponible|  Neuronal|
|Lituano|    `lt`    |Traducción neuronal disponible|  Neuronal|
|Malgache|  `mg`    |Solo estadísticas|  Estadística|
|Malayo| `ms`    |Solo estadísticas   |Estadística|
|Maltés|   `mt`    |Solo estadísticas|  Estadística|
|Noruego| `nb`    |Traducción neuronal disponible|  Neuronal|
|Persa|   `fa`    |Solo estadísticas|  Estadística|
|Polaco|    `pl`    |Traducción neuronal disponible|  Neuronal|
|Portugués|    `pt`    |Traducción neuronal disponible|  Neuronal|
|Otomí Querétaro|   `otq`   |Solo estadísticas|  Estadística|
|Rumano|  `ro`    |Traducción neuronal disponible|  Neuronal|
|Ruso|   `ru`    |Traducción neuronal disponible|  Neuronal|
|Samoano|    `sm`    |Solo estadísticas|  Estadística|
|Serbio (cirílico)|    `sr-Cyrl`   |Solo estadísticas|  Estadística|
|Serbio (latino)|   `sr-Latn`   |Solo estadísticas   |Estadística|
|Eslovaco|    `sk`    |Traducción neuronal disponible|  Neuronal|
|Esloveno| `sl`    |Traducción neuronal disponible|  Neuronal|
|Español|   `es`    |Traducción neuronal disponible|  Neuronal|
|Sueco|   `sv`    |Traducción neuronal disponible   |Neuronal|
|Tahitiano|  `ty`    |Solo estadísticas|  Estadística|
|Tamil| `ta`    |Solo estadísticas|  Estadística|
|Telugu|    `te`    |Solo neuronal|   Neuronal|
|Tailandés|  `th`    |Traducción neuronal disponible|  Neuronal|
|Tongano|    `to`    |Solo estadísticas|  Estadística|
|Turco|   `tr`    |Traducción neuronal disponible   |Neuronal|
|Ucraniano| `uk`    |Traducción neuronal disponible|  Neuronal|
|Urdu|  `ur`    |Solo estadísticas|  Estadística|
|Vietnamita|    `vi`    |Traducción neuronal disponible|  Neuronal|
|Galés| `cy`    |Traducción neuronal disponible|  Neuronal|
|Maya Yucateco|  `yua`   |Solo estadísticas|  Estadística|

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
| Malayalam | ml | Malayalam | --> | Latín |
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
| Hmong Daw      | `mww`          |
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
| Chino (simplificado) |
| Chino (tradicional) |
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
| Kurdo (árabe) |
| Kurdo (latino) |
| Latín |
| Letón |
| Lituano |
| Macedonio |
| Malayo |
| Maltés |
| Noruego |
| Noruego (nynorsk) |
| Pashto |
| Persa |
| Polaco |
| Portugués |
| Rumano |
| Ruso |
| Serbio (cirílico) |
| Serbio (latino) |
| Eslovaco |
| Esloveno |
| Somalí |
| Español |
| Swahili |
| Sueco |
| Tagalo |
| Telugu |
| Tailandés |
| Turco |
| Ucraniano |
| Urdu |
| Uzbeko (cirílico) |
| Uzbeko (latino) |
| Vietnamita |
| Galés |
| Yidis |

## <a name="access-the-list-programmatically"></a>Acceso a la lista mediante programación

Para ver la lista de idiomas admitidos mediante programación, use la operación Languages de Text API V3.0. Puede ver la lista por característica, código de idioma o por nombre del idioma en inglés o en cualquier otro idioma admitido. El servicio Microsoft Translator actualiza esta lista automáticamente cuando hay nuevos idiomas disponibles.

[Ver la documentación de referencia de la operación Languages](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Acceso a la lista en el sitio web de Microsoft Translator

Para echar un vistazo rápido a los idiomas, el sitio web de Microsoft Translator muestra todos los idiomas admitidos por Translator Text API y Speech API. Esta lista no incluye información específica para desarrolladores, por ejemplo, los códigos de idioma.

[Ver la lista de idiomas](https://www.microsoft.com/translator/languages.aspx)
