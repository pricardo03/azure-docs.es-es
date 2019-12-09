---
title: 'Inicio rápido: Reconocimiento de la voz de un archivo de audio: servicio de voz'
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
ms.openlocfilehash: 2f7ec24667514fb131af29321a53f97210e58fc9
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74819423"
---
En este inicio rápido usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para reconocer la voz que procede de un archivo de audio. Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz a través de un archivo solo son necesarios cinco pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````AudioConfig```` que especifique el nombre de archivo .WAV.
> * Cree un objeto ````SpeechRecognizer```` con los objetos ````SpeechConfig```` y ````AudioConfig```` anteriores.
> * Con el objeto ````SpeechRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````SpeechRecognitionResult```` devuelto.
