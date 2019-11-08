---
title: 'Inicio rápido: Reconocimiento de la voz a través de un archivo de audio: servicio Voz'
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
ms.openlocfilehash: bbd3880ea679dc5fc86c0fc1ece101ca3fca74d9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506287"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer la voz que procede de un archivo de audio. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un archivo solo son necesarios cinco pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````AudioConfig```` que especifique el nombre de archivo .WAV.
> * Cree un objeto ````SpeechRecognizer```` con los objetos ````SpeechConfig```` y ````AudioConfig```` anteriores.
> * Con el objeto ````SpeechRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````SpeechRecognitionResult```` devuelto.
