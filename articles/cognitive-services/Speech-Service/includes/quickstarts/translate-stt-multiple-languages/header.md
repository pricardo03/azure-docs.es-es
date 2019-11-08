---
title: 'Inicio rápido: Traducción de voz a varios idiomas: Servicio Voz'
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
ms.openlocfilehash: 6c65fd9a504b5f399afa99280ca2a75b476c750c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504775"
---
En este inicio rápido, va a usar el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para traducir interactivamente la voz de un idioma a voz en otro idioma. Una vez que se cumplen los requisitos previos, para la traducción de voz a texto en varios idiomas solo son necesarios seis pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechTranslationConfig```` a partir de la clave y la región de suscripción.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar el idioma de origen del reconocimiento de voz.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar varios idiomas de destino para la traducción.
> * Cree un objeto ````TranslationRecognizer```` con el objeto ````SpeechTranslationConfig```` anterior.
> * Con el objeto ````TranslationRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````TranslationRecognitionResult```` devuelto.
