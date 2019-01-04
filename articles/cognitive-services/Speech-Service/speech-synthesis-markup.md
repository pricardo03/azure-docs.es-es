---
title: 'Lenguaje de marcado de síntesis de voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Uso del lenguaje de marcado de síntesis de voz para controlar la pronunciación y la prosodia en la conversión de texto a voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: f6a2d6a35200bc4dec169aae72415c1c2904c465
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341140"
---
# <a name="speech-synthesis-markup-language"></a>Lenguaje de marcado de síntesis de voz

El lenguaje de marcado de síntesis de voz (SSML) es un lenguaje de marcado basado en XML que proporciona una manera de controlar la pronunciación y la *prosodia* de la conversión de texto a voz. El término "prosodia" hace referencia al ritmo y al tono de voz (su música, si lo prefiere). Puede especificar palabras fonéticamente, proporcionar sugerencias para interpretar números, insertar pausas, controlar el tono, el volumen y la velocidad, etc. Para más información, consulte [Speech Synthesis Markup Language (SSML) Version 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/) [Lenguaje de marcado de síntesis de voz (SSML) versión 1.0].

Para obtener una lista completa de los idiomas compatibles, las configuraciones regionales y las voces (neuronales y estándar), consulte [compatibilidad con idiomas](language-support.md#text-to-speech).

En las secciones siguientes encontrará ejemplos de tareas de síntesis de voz comunes.

>[!IMPORTANT]
> Actualmente, el etiquetado de la prosodia solo está disponible para voces estándar.

## <a name="add-a-break"></a>Adición de una pausa
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Cambio de la velocidad de habla
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Pronunciación
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Cambio de volumen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Cambio de tono
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Cambio de curva melódica
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Uso de varias voces
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Pasos siguientes

* [Compatibilidad de idiomas: voces, configuraciones regionales e idiomas](language-support.md)
