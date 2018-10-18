---
title: 'Idiomas admitidos: Speech Service API'
description: Una lista de los idiomas naturales admitidos por Speech Service.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/25/2018
ms.author: erhopf
ms.openlocfilehash: 5bf680d22f990e6a93c622271bbb8ccd4c19437e
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166923"
---
# <a name="language-and-region-support-for-speech-service-api"></a>Idiomas y regiones admitidos para Speech Service API

Se admiten diferentes idiomas para distintas funciones del servicio Voz. En las tablas siguientes se resumen los idiomas admitidos.

## <a name="speech-to-text"></a>Speech to Text

Speech Recognition API de Microsoft admite los siguientes idiomas. Para cada idioma hay diferentes niveles de personalización disponibles.

  Código | Idioma | [Adaptación acústica](how-to-customize-acoustic-models.md) | [Adaptación de idioma](how-to-customize-language-model.md) | [Adaptación de pronunciación](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-EG | Árabe (Egipto), estándar moderno | Sin  | Sí | Sin 
 ca-ES | Catalán (España) | Sin  | No | Sin 
 da-DK | Danés (Dinamarca) | Sin  | No | Sin 
 de-DE | Alemán (Alemania) | SÍ | SÍ | Sin 
 en-AU | Inglés (Australia) | Sin  | SÍ | SÍ
 en-CA | Inglés (Canadá) | Sin  | SÍ | SÍ
 en-GB | Inglés (Reino Unido) | Sin  | SÍ | SÍ
 en-IN | Inglés (India) | SÍ | Sí | SÍ
 en-NZ | Inglés (Nueva Zelanda) | Sin  | SÍ | SÍ  
 es-ES | Inglés (Estados Unidos) | SÍ | Sí | SÍ
 es-ES | Español (España) | Sin  | Sí | Sin 
 es-MX | Español (México) | Sin  | Sí | Sin 
 fi-FI | Finés (Finlandia) | Sin  | No | Sin 
 fr-CA | Francés (Canadá) | Sin  | Sí | Sin 
 fr-FR | Francés (Francia) | SÍ | SÍ | Sin 
 hi-IN | Hindi (India) | Sin  | Sí | Sin 
 it-IT | Italiano (Italia) | SÍ | SÍ | Sin 
 ja-JP | Japonés (Japón) | Sin  | Sí | Sin 
 ko-KR | Coreano (Corea) | Sin  | Sí | Sin 
 nb-NO | Noruego, Bokmål (Noruego) | Sin  | No | Sin 
 nl-NL | Neerlandés (Países Bajos) | Sin  | Sí | Sin 
 pl-PL | Polaco (Polonia) | Sin  | No | Sin 
 pt-BR | Portugués (Brasil) | Sin  | Sí | Sin 
 pt-PT | Portugués (Portugal) | Sin  | Sí | Sin 
 ru-RU | Ruso (Rusia) | SÍ | SÍ | Sin 
 sv-SE | Sueco (Suecia) | Sin  | No | Sin 
 zh-CN | Chino (mandarín, simplificado) | SÍ | SÍ | Sin 
 zh-HK | Chino (mandarín, tradicional) | Sin  | Sí | Sin 
 zh-TW | Chino (mandarín, Taiwán) | Sin  | Sí | Sin 
 th-TH | Tailandés (Tailandia) | Sin  | No | Sin 


## <a name="text-to-speech"></a>Texto a voz

La API de síntesis de voz ofrece las voces siguientes, cada una de las cuales admite un idioma y un dialecto específicos, identificados mediante la configuración regional.

Configuración regional | Idioma | Sexo | Asignación de nombre de servicio
-------|----------|---------|--------------------
ar-EG\* | Árabe (Egipto) | Mujer | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)"
ar-SA | Árabe (Arabia Saudí) | Hombre | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)"
bg-BG | Búlgaro | Hombre | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)"
ca-ES | Catalán (España) | Mujer | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)"
cs-CZ | Checo | Hombre | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)"
cs-CZ | Checo | Hombre | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Vit)"
da-DK | Danés | Mujer | "Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)"
de-AT | Alemán (Austria) | Hombre | "Microsoft Server Speech Text to Speech Voice (de-AT, Michael)"
de-CH | Alemán (Suiza) | Hombre | "Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)"
de-DE | Alemán (Alemania) | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)"
el-GR | Griego | Hombre | "Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)"
en-AU | Inglés (Australia) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)"
en-CA | Inglés (Canadá) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, Linda)"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)"
en-GB | English (Reino Unido) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)"
en-IE | Inglés (Irlanda) |Hombre | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)"
en-IE | Inglés (Irlanda) |Hombre | "Microsoft Server Speech Text to Speech Voice (en-IE, Shaun)"
en-IN | Inglés (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)"
es-ES | Inglés (EE. UU.) |Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)"
es-ES | Español (España) |Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)"
es-MX | Español (México) | Mujer | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)"
fi-FI | Finés | Mujer | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)"
fr-CA | Francés (Canadá) |Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)"
fr-CH | Francés (Suiza)|Hombre | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)"
fr-FR | Francés (Francia)|Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)"
he-IL| Hebreo (Israel) | Hombre| "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)"
hi-IN | Hindi (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)"
hr-HR | Croata | Hombre | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)"
hu-HU | Húngaro | Hombre | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)"
id-ID | Indonesio| Hombre | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)"
it-IT | Italiano |Hombre | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)"
ja-JP | Japonés |Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)"
ko-KR | Coreano |Mujer | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)"
ms-MY | Malayo | Hombre | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)"
nb-NO | Noruego | Mujer | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)"
nl-NL | Neerlandés | Mujer | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)"
pl-PL | Polaco | Mujer | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)"
pt-BR | Portugués (Brasil) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)"
pt-PT | Portugués (Portugal) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)"
ro-RO | Rumano | Hombre | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)"
ru-RU |Ruso| Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)"
sk-SK | Eslovaco|Hombre | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)"
sl-SI | Esloveno|Hombre | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)"
sv-SE | Sueco|Mujer | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)"
ta-IN | Tamil (India) |Hombre | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)"
te-IN | Telugu (India) |Mujer | "Microsoft Server Speech Text to Speech Voice (te-IN, Chitra)"
th-TH | Tailandés|Hombre | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)"
tr-TR |Turco| Mujer | "Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)"
vi-VN | Vietnamita|Hombre | "Microsoft Server Speech Text to Speech Voice (vi-VN, An)"
zh-CN | Chino (continental)|Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)"
| | |Hombre | "Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)"
zh-HK | Chino (Hong Kong)|Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)"
| || Hombre | "Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)"
zh-TW | Chino (Taiwán)|Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)"
| || Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)"
| || Hombre | "Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)"

\* *ar-EG admite el árabe estándar moderno (MSA).*

### <a name="customization"></a>Personalización

La personalización de la voz está disponible para inglés de Estados Unidos (en-US), chino continental (zh-CN) e italiano (it-IT).

> [!NOTE]
> El entrenamiento de voz en italiano comienza con un conjunto de datos de más de 2000 expresiones. Los modelos bilingües chino-inglés también se admiten con un conjunto de datos inicial de más de 2000 expresiones.

## <a name="speech-translation"></a>Speech Translation

**Speech Translation** API admite varios idiomas para la traducción de voz a voz y de texto a voz. El idioma de origen siempre debe ser de la tabla de idiomas de Voz siguiente. Los idiomas de destino admitidos dependen de si el destino de traducción es voz o texto.

### <a name="speech-languages"></a>Idiomas de voz

| Idioma de voz   | Código de idioma |
|:----------- |-|
| Árabe (estándar moderno)      | `ar` |
| Chino (mandarín)      | `zh` |
| English      | `en` |
| Francés      | `fr` |
| Alemán      | `de` |
| Italiano      | `it` |
| Japonés      | `jp` |
| Portugués (Brasil)     | `pt` |
| Ruso      | `ru` |
| Español      |  `es` |

### <a name="text-languages"></a>Idiomas de texto

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


## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
