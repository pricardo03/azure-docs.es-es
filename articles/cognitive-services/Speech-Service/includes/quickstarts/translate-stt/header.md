---
title: 'Inicio rápido: Traducción de voz en texto: servicio Voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: c45e75038d1731c933ccf8bf26f9de573e409292
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504743"
---
En este inicio rápido, va a usar el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para traducir interactivamente la voz de un idioma a texto en otro idioma. Una vez que se cumplen los requisitos previos, para la traducción de voz a texto solo son necesarios cinco pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Actualice el objeto ````SpeechConfig```` para especificar los idiomas de origen y destino.
> * Cree un objeto ````TranslationRecognizer```` con el objeto ````SpeechConfig```` anterior.
> * Con el objeto ````TranslationRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````TranslationRecognitionResult```` devuelto.
