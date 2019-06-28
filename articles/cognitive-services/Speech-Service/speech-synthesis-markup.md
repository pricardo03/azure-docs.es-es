---
title: 'Lenguaje de marcado de síntesis de voz (SSML): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Uso del lenguaje de marcado de síntesis de voz para controlar la pronunciación y la prosodia en la conversión de texto a voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9871e0106ee6caf11c5a1e24459fbd2044f5f3d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65021434"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Lenguaje de marcado de síntesis de voz (SSML)

El lenguaje de marcado de síntesis de voz (SSML) es un lenguaje de marcado basado en XML que proporciona una manera de controlar la pronunciación y la *prosodia* de la conversión de texto a voz. El término "prosodia" hace referencia al ritmo y al tono de voz (su música, si lo prefiere). Puede especificar palabras fonéticamente, proporcionar sugerencias para interpretar números, insertar pausas, controlar el tono, el volumen y la velocidad, etc. Para más información, consulte [Speech Synthesis Markup Language (SSML) Version 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/) [Lenguaje de marcado de síntesis de voz (SSML) versión 1.0]. 

Para obtener una lista completa de los idiomas compatibles, las configuraciones regionales y las voces (neuronales y estándar), consulte [compatibilidad con idiomas](language-support.md#text-to-speech).

En las secciones siguientes encontrará ejemplos de tareas de síntesis de voz comunes.

## <a name="adjust-speaking-style-for-neural-voices"></a>Ajuste del estilo de habla para voces neuronales

Puede usar SSSL para ajustar el estilo de habla al usar una de las voces neuronales.

De forma predeterminada, el servicio de texto a voz sintetiza el texto en un estilo neutro. Las voces neuronales amplían SSML con un elemento `<mstts:express-as>` que convierte el texto a voz sintetizada en distintos estilos de habla. Actualmente, las etiquetas de estilo solo son compatibles con estas voces:

* `en-US-JessaNeural` 
* `zh-CN-XiaoxiaoNeural`.

Los cambios de estilo de habla se pueden aplicar en el nivel de oración. Los estilos varían según la voz. Si no se admite un tipo de estilo, el servicio devuelve la voz sintetizada como el estilo neutro predeterminado.

| Voz | Estilo | DESCRIPCIÓN | 
|-----------|-----------------|----------|
| `en-US-JessaNeural` | tipo =`cheerful` | Expresa una emoción que es positiva y feliz. |
| | tipo =`empathy` | Expresa un sentimiento de cuidado y comprensión. |
| `zh-CN-XiaoxiaoNeural` | tipo =`newscast` | Expresa un tono formal, similar a las retransmisiones de noticias. |
| | tipo =`sentiment ` | Transmite un mensaje conmovedor o una historia. |

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
<voice name='en-US-JessaNeural'>
<mstts:express-as type="cheerful"> 
    That'd be just amazing! 
</mstts:express-as></voice></speak>
```

## <a name="add-a-break"></a>Adición de una pausa
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Cambio de la velocidad de habla

La velocidad de habla puede aplicarse a voces estándares en el nivel de palabra o frase. Sin embargo, solo puede aplicarse a voces neuronales en el nivel de oración.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Guy24kRUS'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Pronunciación
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Cambio de volumen

Los cambios de volumen pueden aplicarse a voces estándar en el nivel de palabra o frase. Sin embargo, solo pueden aplicarse a voces neuronales en el nivel de frase.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Cambio de tono

Los cambios de tono pueden aplicarse a voces estándar en el nivel de palabra o frase. Sin embargo, solo pueden aplicarse a voces neuronales en el nivel de frase.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='en-US-Guy24kRUS'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Cambio de curva melódica

> [!IMPORTANT]
> Los cambios de curva melódica no se admiten con voces neuronales.

```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Uso de varias voces
```xml
<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='en-US-Jessa24kRUS'>
    Good morning!
</voice>
<voice  name='en-US-Guy24kRUS'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Pasos siguientes

* [Compatibilidad de idiomas: voces, configuraciones regionales e idiomas](language-support.md)
