---
title: Compatibilidad con Language - servicios de voz
titleSuffix: Azure Cognitive Services
description: Los servicios de Voz de Azure admite un gran número de idiomas para la conversión de texto a voz y texto a voz, junto con la traducción por voz. En este artículo se proporciona una lista completa idiomas compatibles por servicio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9b8e12220f220bd8183675d13e25bdcab02707fd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65020855"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Compatibilidad de idioma y región para los servicios de voz

Se admiten diferentes idiomas para distintas funciones de los servicios de voz. En las tablas siguientes se resumen los idiomas admitidos.

## <a name="speech-to-text"></a>Conversión de voz en texto

Speech Recognition API de Microsoft admite los siguientes idiomas. Para cada idioma hay diferentes niveles de personalización disponibles.

  Código | Idioma | [Adaptación acústica](how-to-customize-acoustic-models.md) | [Adaptación de idioma](how-to-customize-language-model.md) | [Adaptación de pronunciación](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-EG | Árabe (Egipto), estándar moderno | No | Sí | No
 ca-ES | Catalán | No | No | No
 da-DK | Danés (Dinamarca) | No | No | No
 de-DE | Alemán (Alemania) | Sí | Sí | No
 en-AU | Inglés (Australia) | No | Sí | Sí
 en-CA | Inglés (Canadá) | No | Sí | Sí
 en-GB | Inglés (Reino Unido) | No | Sí | Sí
 en-IN | Inglés (India) | Sí | Sí | Sí
 en-NZ | Inglés (Nueva Zelanda) | No | Sí | Sí  
 es-ES | Inglés (Estados Unidos) | Sí | Sí | Sí
 es-ES | Español (España) | Sí | Sí | No
 es-MX | Español (México) | No | Sí | No
 fi-FI | Finés (Finlandia) | No | No | No
 fr-CA | Francés (Canadá) | No | Sí | No
 fr-FR | Francés (Francia) | Sí | Sí | No
 hi-IN | Hindi (India) | No | Sí | No
 it-IT | Italiano (Italia) | Sí | Sí | No
 ja-JP | Japonés (Japón) | No | Sí | No
 ko-KR | Coreano (Corea) | No | Sí | No
 nb-NO | Noruego, Bokmål (Noruego) | No | No | No
 nl-NL | Neerlandés (Países Bajos) | No | Sí | No
 pl-PL | Polaco (Polonia) | No | No | No
 pt-BR | Portugués (Brasil) | Sí | Sí | No
 pt-PT | Portugués (Portugal) | No | Sí | No
 ru-RU | Ruso (Rusia) | Sí | Sí | No
 sv-SE | Sueco (Suecia) | No | No | No
 zh-CN | Chino (mandarín, simplificado) | Sí | Sí | No
 zh-HK | Chino (cantonés, tradicional) | No | Sí | No
 zh-TW | Chino (mandarín, Taiwán) | No | Sí | No
 th-TH | Tailandés (Tailandia) | No | No | No


## <a name="text-to-speech"></a>Texto a voz

Text to Speech REST API admite las siguientes voces, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional.

> [!IMPORTANT]
> Los precios son distintos para voces estándar, personalizadas y neuronales. Consulte la página de [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

### <a name="neural-voices"></a>Voces neuronales

Texto a voz neuronal es un nuevo tipo de síntesis de voz con tecnología de redes neuronales profundas. Cuando se usa una voz neuronal, es prácticamente imposible distinguir la voz sintetizada de las grabaciones humanas.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes virtuales sean más naturales y atractivas, para convertir textos digitales, como los libros electrónicos, en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga de la escucha que aparece cuando los usuarios interactúan con sistemas de inteligencia artificial.

Para obtener una lista completa de las voces neuronales y la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

Configuración regional | Idioma | Sexo | Asignación de nombres de servicio completo | Nombre corto de voz
--------|----------|--------|---------|------------
de-DE | Alemán (Alemania) | Mujer | "Microsoft Server voz texto a voz (de-DE, KatjaNeural)" | "de-DE-KatjaNeural"
es-ES | Inglés (EU) | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)" | "en-US-GuyNeural"
es-ES | Inglés (EU) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)" | "en-US-JessaNeural"
it-IT | Italiano (Italia) | Mujer |"Microsoft Server voz texto a voz (it-IT, ElsaNeural)" | "it-IT-ElsaNeural"
zh-CN | Chino (continental) | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

> [!NOTE]
> Puede usar la asignación de nombre de servicio completo o el nombre corto de voz en las solicitudes de síntesis de voz.

### <a name="standard-voices"></a>Voces estándar

Hay más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales, lo que le permite convertir texto en voz sintetizada. Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

Configuración regional | Idioma | Sexo | Asignación de nombres de servicio completo | Nombre corto de voz
-------|----------|---------|----------|----------
ar-EG\* | Árabe (Egipto) | Mujer | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)" | "ar-EG-Hoda"
ar-SA | Árabe (Arabia Saudí) | Hombre | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)" | "ar-SA-Naayf"
bg-BG | Búlgaro | Hombre | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)" | "bg-BG-Ivan"
ca-ES | Catalán (catalán) | Mujer | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)" | "ES-ca-HerenaRUS"
cs-CZ | Checo | Hombre | "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)" | "cs-CZ-Jakub"
da-DK | Danés | Mujer | "Microsoft Server Speech Text to Speech Voice (da-DK, HelleRUS)" | "da-DK-HelleRUS"
de-AT | Alemán (Austria) | Hombre | "Microsoft Server Speech Text to Speech Voice (de-AT, Michael)" | "de-AT-Michael"
de-CH | Alemán (Suiza) | Hombre | "Microsoft Server Speech Text to Speech Voice (de-CH, Karsten)" | "de-CH-Karsten"
de-DE | Alemán (Alemania) | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, Hedda)" | "de-DE-Hedda"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, HeddaRUS)" | "de-DE-HeddaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo"
el-GR | Griego | Hombre | "Microsoft Server Speech Text to Speech Voice (el-GR, Stefanos)" | "el-GR-Stefanos"
en-AU | Inglés (Australia) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, Catherine)" | "en-AU-Catherine"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-AU, HayleyRUS)" | "en-AU-HayleyRUS"
en-CA | Inglés (Canadá) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, Linda)" | "en-CA-Linda"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-CA, HeatherRUS)" | "en-CA-HeatherRUS"
en-GB | Inglés (GB) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)" | "en-GB-Apollo de George"
en-IE | Inglés (Irlanda) | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)" | "en-IE-Juan"
en-IN | Inglés (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)" | "en-IN-Heera-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)" | "en-IN-PriyaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo"
es-ES | Inglés (EU) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
es-ES | Español (España) |Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)" | "es-ES HelenaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo"
es-MX | Español (México) | Mujer | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)" | "es-MX-HildaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)" | "es-MX-Raúl-Apollo"
fi-FI | Finlandés | Mujer | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS"
fr-CA | Francés (Canadá) |Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)" | "fr-CA-Caroline"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS"
fr-CH | Francés (Suiza)| Hombre | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)" | "fr-CH-Guillaume"
fr-FR | Francés (Francia)| Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)" | "fr-FR-Apollo de Paul"
he-IL| Hebreo (Israel) | Hombre| "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)" | "he-IL-Asaf"
hi-IN | Hindi (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)" | "hi-IN-Kalpana-Apollo"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)" | "hi-IN-Kalpana"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)" | "hi-IN-Hemant"
hr-HR | Croata | Hombre | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)" | "hr-HR-Matej"
hu-HU | Húngaro | Hombre | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)" | "hu-HU-Szabolcs"
id-ID | Indonesio| Hombre | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)" | "Andika de Id. de Id."
it-IT | Italiano | Hombre | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)" | "it-IT-LuciaRUS"
ja-JP | Japonés | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
ko-KR | Coreano | Mujer | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS"
ms-MY | Malayo | Hombre | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)" | "ms-MY-Rizwan"
nb-NO | Noruego (Bokmal) | Mujer | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)" | "nb-NO-HuldaRUS"
nl-NL | Holandés | Mujer | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
pl-PL | Polaco | Mujer | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
pt-BR | Portugués (Brasil) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS"
| | | Hombre |"Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo"
pt-PT | Portugués (Portugal) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS"
ro-RO | Rumano | Hombre | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)" | "ro-RO-Andrei"
ru-RU |Ruso| Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS
sk-SK | Eslovaco | Hombre | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)" | "sk-SK-Filip"
sl-SI | Esloveno | Hombre | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)" | "sl-SI Lado"
sv-SE | Sueco | Mujer | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS"
ta-IN | Tamil (India) | Hombre | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)" | "ta-IN-Valluvar"
te-IN | Telugu (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (te-IN, Chitra)" | "te-IN-Chitra"
th-TH | Tailandés | Hombre | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)" | "n-TH Pattara"
tr-TR | Turco | Mujer | "Microsoft Server Speech Text to Speech Voice (tr-TR, SedaRUS)" | "tr-TR-SedaRUS"
vi-VN | Vietnamita | Hombre | "Microsoft Server Speech Text to Speech Voice (vi-VN, An)" | "vi-VN-An"
zh-CN | Chino (continental) | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo"
zh-HK | Chino (RAE de Hong Kong) | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, Tracy, Apollo)" | "zh-HK-Tracy-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-HK, TracyRUS)" | "zh-HK-TracyRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-HK, Danny, Apollo)" | "zh-HK-Danny-Apollo"
zh-TW | Chino (Taiwán) | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, Yating, Apollo)" | "zh-TW-Yating-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo"

\* *ar-EG admite el árabe estándar moderno (MSA).*

> [!NOTE]
> Puede usar la asignación de nombre de servicio completo o el nombre corto de voz en las solicitudes de síntesis de voz.

### <a name="customization"></a>Personalización

Personalización de voz está disponible para de-DE, en-GB, en India, en-US, es-MX, fr-FR, it-IT, pt-BR y zh-CN. Seleccione la configuración regional adecuada que coincida con los datos de entrenamiento que tiene para entrenar un modelo de voz personalizados. Por ejemplo, si los datos de la grabación que tiene se hablan en inglés con un acento British, seleccione en-GB.  

> [!NOTE]
> No admitimos entrenamiento del modelo de bi-lingüe de voz personalizados, excepto bilingües chino en inglés. Seleccione a 'Bilingües chino inglés' Si desea entrenar una voz chino que puede habla a inglés también. Entrenamiento de voz en todas las configuraciones regionales se inicia con un conjunto de datos de más de 2.000 grabaciones de voz, excepto el de en-US y zh-CN, donde puede empezar con cualquier tamaño de datos de entrenamiento.

## <a name="speech-translation"></a>Traducción de voz

**Speech Translation** API admite varios idiomas para la traducción de voz a voz y de texto a voz. El idioma de origen debe incluirse siempre en la siguiente tabla de conversión de voz en texto. Los idiomas de destino admitidos dependen de si el destino de traducción es voz o texto. Puede traducir la voz entrante en más de [60 idiomas](https://www.microsoft.com/translator/business/languages/). Un subconjunto de estos idiomas está disponible para [síntesis de voz](language-support.md#text-languages).

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
| Holandés      | `nl`          |
| Inglés      | `en`          |
| Estonio      | `et`          |
| Fiyiano      | `fj`          |
| Filipino      | `fil`          |
| Finlandés      | `fi`          |
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
| Noruego (Bokmal)      | `nb`          |
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

* [Obtenga su suscripción de evaluación gratuita de Speech Service](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
