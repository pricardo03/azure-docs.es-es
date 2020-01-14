---
title: 'Inicio rápido: Reconocimiento de voz, intenciones y entidades: servicio de voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 1/02/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a833d39ab91cd803f066d707306a6ff648d37e8f
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660558"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) y el servicio Language Understanding (LUIS) para reconocer los intentos de los datos de audio capturados de un micrófono. En concreto, usará el SDK de Voz para capturar la voz y un dominio predefinido de LUIS para identificar las intenciones de automatización doméstica, como encender y apagar una luz. 

Tras cumplir algunos requisitos previos, no es preciso seguir muchos pasos para el reconocimiento de voz y la identificación de intenciones a través de un micrófono:

> [!div class="checklist"]
>
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````IntentRecognizer```` con el objeto ````SpeechConfig```` anterior.
> * Con el objeto ````IntentRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````IntentRecognitionResult```` devuelto.
