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
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 64f084f58ae4b12d92d6dee343a59dd56b1c351e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505567"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer interactivamente la voz de los datos de audio capturada a través de un micrófono. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un micrófono solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````SpeechRecognizer```` con el objeto ````SpeechConfig```` anterior.
> * Con el objeto ````SpeechRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````SpeechRecognitionResult```` devuelto.
