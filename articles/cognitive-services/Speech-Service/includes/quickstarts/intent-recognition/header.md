---
title: 'Inicio rápido: Reconocimiento de voz, intenciones y entidades: servicio Voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 3b50ba8f2c3d21fb5bb0ab2c26cedb6bda0fab16
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505967"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer interactivamente la voz de los datos de audio capturada a través de un micrófono. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un micrófono solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````IntentRecognizer```` con el objeto ````SpeechConfig```` anterior.
> * Con el objeto ````IntentRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````IntentRecognitionResult```` devuelto.