---
title: 'Compatibilidad con idiomas: Speech Service'
titleSuffix: Azure Cognitive Services
description: Speech Service admite un gran número de idiomas para la conversión de texto a voz y voz a texto, junto con la traducción por voz. En este artículo se proporciona una lista completa de idiomas compatibles por servicio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: af8bb24862c05b232b7bb5d831b1eb3b1add3a7f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468808"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Compatibilidad con regiones e idiomas para los servicios de Voz

Se admiten diferentes idiomas para distintas funciones de los servicios de voz. En las tablas siguientes se resumen los idiomas admitidos.

## <a name="speech-to-text"></a>Voz a texto

El SDK de Voz de Microsoft y la API REST admiten los siguientes idiomas (configuraciones regionales). Para mejorar la precisión, se ofrece la personalización para un subconjunto de idiomas mediante la carga de audio y transcripciones con etiqueta humana o texto relacionado: Oraciones.  La personalización de la pronunciación solo está disponible actualmente para en-US y de-DE. Aprenda más sobre la personalización [aquí](how-to-custom-speech.md).

  Configuración regional | Idioma | Compatible | Personalizable
 ------|----------|---------------------|---------------------
 ar-EG | Árabe (Egipto), estándar moderno | Sí | Sí
 ar-SA | Árabe (Arabia Saudí) | Sí | Sí
 ar-AE | Árabe (Emiratos Árabes Unidos) | Sí | Sí
 ar-KW | Árabe (Kuwait) | Sí | Sí
 ar-QA | Árabe (Qatar) | Sí | Sí
 ca-ES | Catalán | Sí | Sin
 da-DK | Danés (Dinamarca) | Sí | Sin
 de-DE | Alemán (Alemania) | Sí | Sí
 en-AU | Inglés (Australia) | Sí | Sí
 en-CA | Inglés (Canadá) | Sí | Sí
 en-GB | Inglés (Reino Unido) | Sí | Sí
 en-IN | Inglés (India) | Sí | Sí
 en-NZ | Inglés (Nueva Zelanda) | Sí | Sí
 en-US | Inglés (Estados Unidos) | Sí | Sí
 es-ES | Español (España) | Sí | Sí
 es-MX | Español (México) | Sí | Sí
 fi-FI | Finés (Finlandia) | Sí | Sin
 fr-CA | Francés (Canadá) | Sí | Sí
 fr-FR | Francés (Francia) | Sí | Sí
 gu-IN | Gujarati (India) | Sí | Sí
 hi-IN | Hindi (India) | Sí | Sí
 it-IT | Italiano (Italia) | Sí | Sí
 ja-JP | Japonés (Japón) | Sí | Sí
 ko-KR | Coreano (Corea) | Sí | Sí
 mr-IN | Maratí (India) | Sí | Sí
 nb-NO | Noruego, Bokmål (Noruego) | Sí | Sin
 nl-NL | Neerlandés (Países Bajos) | Sí | Sí
 pl-PL | Polaco (Polonia) | Sí | Sin
 pt-BR | Portugués (Brasil) | Sí | Sí
 pt-PT | Portugués (Portugal) | Sí | Sí
 ru-RU | Ruso (Rusia) | Sí | Sí
 sv-SE | Sueco (Suecia) | Sí | Sin
 ta-IN | Tamil (India) | Sí | Sí
 te-IN | Telugu (India) | Sí | Sí
 zh-CN | Chino (mandarín, simplificado) | Sí | Sí
 zh-HK | Chino (cantonés, tradicional) | Sí | Sí
 zh-TW | Chino (mandarín, Taiwán) | Sí | Sí
 th-TH | Tailandés (Tailandia) | Sí | Sin
 tr-TR | Turquía | Sí | Sí |


## <a name="text-to-speech"></a>Texto a voz

Tanto el SDK de Voz de Microsoft como la API REST admiten estas voces, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional.

> [!IMPORTANT]
> Los precios son distintos para voces estándar, personalizadas y neuronales. Consulte la página de [precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para más información.

### <a name="neural-voices"></a>Voces neuronales

Texto a voz neuronal es un nuevo tipo de síntesis de voz con tecnología de redes neuronales profundas. Cuando se usa una voz neuronal, es prácticamente imposible distinguir la voz sintetizada de las grabaciones humanas.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes de voz sean más naturales y atractivas, para convertir textos digitales (por ejemplo, los libros electrónicos) en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con su prosodia natural similar a la humana y la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga de la escucha que aparece cuando los usuarios interactúan con sistemas de inteligencia artificial.

Para obtener una lista completa de las voces neuronales y la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

Configuración regional | Idioma | Sexo | Asignación de nombre de servicio completo | Nombre corto de voz
--------|----------|--------|---------|------------
de-DE | Alemán (Alemania) | Mujer | "Microsoft Server Speech Text to Speech Voice (de-DE, KatjaNeural)" | "de-DE-KatjaNeural"
en-US | Inglés (EE. UU.) | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)" | "en-US-GuyNeural"
en-US | Inglés (EE. UU.) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)" | "en-US-JessaNeural"
it-IT | Italiano (Italia) | Mujer |"Microsoft Server Speech Text to Speech Voice (it-IT, ElsaNeural)" | "it-IT-ElsaNeural"
zh-CN | Chino (continental) | Mujer | "Microsoft Server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

> [!NOTE]
> Puede usar la asignación de nombre de servicio completo o el nombre corto de voz en las solicitudes de síntesis de voz.

### <a name="standard-voices"></a>Voces estándar

Hay más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales, lo que le permite convertir texto en voz sintetizada. Para más información acerca de la disponibilidad regional, consulte las [regiones](regions.md#standard-and-neural-voices).

Configuración regional | Idioma | Sexo | Asignación de nombre de servicio completo | Nombre corto de voz
-------|----------|---------|----------|----------
ar-EG\* | Árabe (Egipto) | Mujer | "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)" | "ar-EG-Hoda"
ar-SA | Árabe (Arabia Saudí) | Hombre | "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)" | "ar-SA-Naayf"
bg-BG | Búlgaro | Hombre | "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)" | "bg-BG-Ivan"
ca-ES | Catalán (España) | Mujer | "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS"
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
en-GB | English (Reino Unido) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-GB, George, Apollo)" | "en-GB-George-Apollo"
en-IE | Inglés (Irlanda) | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IE, Sean)" | "en-IE-Sean"
en-IN | Inglés (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, Heera, Apollo)" | "en-IN-Heera-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-IN, PriyaRUS)" | "en-IN-PriyaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo"
en-US | Inglés (EE. UU.) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
es-ES | Español (España) |Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (es-ES, HelenaRUS)" | "es-ES-HelenaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo"
es-MX | Español (México) | Mujer | "Microsoft Server Speech Text to Speech Voice (es-MX, HildaRUS)" | "es-MX-HildaRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (es-MX, Raul, Apollo)" | "es-MX-Raul-Apollo"
fi-FI | Finés | Mujer | "Microsoft Server Speech Text to Speech Voice (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS"
fr-CA | Francés (Canadá) |Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, Caroline)" | "fr-CA-Caroline"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS"
fr-CH | Francés (Suiza)| Hombre | "Microsoft Server Speech Text to Speech Voice (fr-CH, Guillaume)" | "fr-CH-Guillaume"
fr-FR | Francés (Francia)| Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo"
he-IL| Hebreo (Israel) | Hombre| "Microsoft Server Speech Text to Speech Voice (he-IL, Asaf)" | "he-IL-Asaf"
hi-IN | Hindi (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana, Apollo)" | "hi-IN-Kalpana-Apollo"
| | |Mujer | "Microsoft Server Speech Text to Speech Voice (hi-IN, Kalpana)" | "hi-IN-Kalpana"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (hi-IN, Hemant)" | "hi-IN-Hemant"
hr-HR | Croata | Hombre | "Microsoft Server Speech Text to Speech Voice (hr-HR, Matej)" | "hr-HR-Matej"
hu-HU | Húngaro | Hombre | "Microsoft Server Speech Text to Speech Voice (hu-HU, Szabolcs)" | "hu-HU-Szabolcs"
id-ID | Indonesio| Hombre | "Microsoft Server Speech Text to Speech Voice (id-ID, Andika)" | "id-ID-Andika"
it-IT | Italiano | Hombre | "Microsoft Server Speech Text to Speech Voice (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (it-IT, LuciaRUS)" | "it-IT-LuciaRUS"
ja-JP | Japonés | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
ko-KR | Coreano | Mujer | "Microsoft Server Speech Text to Speech Voice (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS"
ms-MY | Malayo | Hombre | "Microsoft Server Speech Text to Speech Voice (ms-MY, Rizwan)" | "ms-MY-Rizwan"
nb-NO | Noruego | Mujer | "Microsoft Server Speech Text to Speech Voice (nb-NO, HuldaRUS)" | "nb-NO-HuldaRUS"
nl-NL | Neerlandés | Mujer | "Microsoft Server Speech Text to Speech Voice (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
pl-PL | Polaco | Mujer | "Microsoft Server Speech Text to Speech Voice (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
pt-BR | Portugués (Brasil) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS"
| | | Hombre |"Microsoft Server Speech Text to Speech Voice (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo"
pt-PT | Portugués (Portugal) | Mujer | "Microsoft Server Speech Text to Speech Voice (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS"
ro-RO | Rumano | Hombre | "Microsoft Server Speech Text to Speech Voice (ro-RO, Andrei)" | "ro-RO-Andrei"
ru-RU |Ruso| Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Hombre | "Microsoft Server Speech Text to Speech Voice (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo"
| | | Mujer | "Microsoft Server Speech Text to Speech Voice (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS
sk-SK | Eslovaco | Hombre | "Microsoft Server Speech Text to Speech Voice (sk-SK, Filip)" | "sk-SK-Filip"
sl-SI | Esloveno | Hombre | "Microsoft Server Speech Text to Speech Voice (sl-SI, Lado)" | "sl-SI-Lado"
sv-SE | Sueco | Mujer | "Microsoft Server Speech Text to Speech Voice (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS"
ta-IN | Tamil (India) | Hombre | "Microsoft Server Speech Text to Speech Voice (ta-IN, Valluvar)" | "ta-IN-Valluvar"
te-IN | Telugu (India) | Mujer | "Microsoft Server Speech Text to Speech Voice (te-IN, Chitra)" | "te-IN-Chitra"
th-TH | Tailandés | Hombre | "Microsoft Server Speech Text to Speech Voice (th-TH, Pattara)" | "th-TH-Pattara"
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

La personalización de voz está disponible para de-DE, en-GB, en-IN, en-US, es-MX, fr-FR, it-IT, pt-BR y zh-CN. Seleccione la configuración regional adecuada que coincida con los datos de entrenamiento que tiene para entrenar un modelo de voz personalizado. Por ejemplo, si los datos de grabación que tiene se hablan en inglés con acento British, seleccione en-GB.  

> [!NOTE]
> No se admite el entrenamiento de modelos bilingües en la funcionalidad de voz personalizada, excepto en el caso de chino-inglés bilingüe. Seleccione la opción de chino-inglés bilingüe si quiere entrenar una voz china que pueda hablar también inglés. El entrenamiento de voz de todas las configuraciones regionales se inicia con un conjunto de datos de más de 2000 expresiones, excepto en el caso de en-US y zh-CN, que puede comenzar con cualquier tamaño de datos de entrenamiento.

## <a name="speech-translation"></a>Traducción de voz

**Speech Translation** API admite varios idiomas para la traducción de voz a voz y de texto a voz. El idioma de origen debe incluirse siempre en la siguiente tabla de conversión de voz en texto. Los idiomas de destino admitidos dependen de si el destino de traducción es voz o texto. Puede traducir la voz entrante en más de [60 idiomas](https://www.microsoft.com/translator/business/languages/). Un subconjunto de estos idiomas está disponible para [síntesis de voz](language-support.md#text-languages).

### <a name="text-languages"></a>Idiomas de texto

| Idioma de texto    | Código de idioma |
|:----------- |:-------------:|
| Afrikáans      | `af`          |
| Árabe       | `ar`          |
| Bengalí      | `bn`          |
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

* [Obtenga su suscripción de evaluación gratuita de Speech Service](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp)
