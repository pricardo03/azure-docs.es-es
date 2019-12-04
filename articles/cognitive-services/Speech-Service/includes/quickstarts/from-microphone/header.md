---
title: 'Inicio rápido: Reconocimiento de la voz a través de un micrófono: servicio Voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: bee3b64ece3faac6258fe4d017f12833b12e370d
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2019
ms.locfileid: "74536368"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer interactivamente la voz de la entrada de micrófono y obtener la transcripción del texto del audio capturado. Es fácil integrar esta característica en las aplicaciones o dispositivos para tareas de reconocimiento comunes, como transcribir conversaciones. También se puede usar para integraciones más complejas, como el uso de Bot Framework con el SDK de Voz para crear asistentes de voz.

Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un micrófono solo son necesarios cuatro pasos:

> [!div class="checklist"]
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `SpeechRecognizer` con el objeto `SpeechConfig` anterior.
> * Con el objeto `SpeechRecognizer`, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto `SpeechRecognitionResult` devuelto.
