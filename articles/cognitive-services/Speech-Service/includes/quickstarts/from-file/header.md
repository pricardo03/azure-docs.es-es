---
title: 'Inicio rápido: Reconocimiento de la voz de un archivo de audio: servicio de voz'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: e333b156eaf9c4b6e09e631513ea099018f0691b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75469617"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer la voz que procede de un archivo de audio. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un archivo solo son necesarios cinco pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````AudioConfig```` que especifique el nombre de archivo .WAV.
> * Cree un objeto ````SpeechRecognizer```` con los objetos ````SpeechConfig```` y ````AudioConfig```` anteriores.
> * Con el objeto ````SpeechRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````SpeechRecognitionResult```` devuelto.
