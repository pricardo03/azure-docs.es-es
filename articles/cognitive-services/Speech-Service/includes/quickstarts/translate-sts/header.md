---
title: 'Inicio rápido: Traducción de voz a voz: servicio de voz'
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
ms.openlocfilehash: 805193c08e57ddc18dfca0a78c6b58cf895baec1
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74817452"
---
En este inicio rápido, va a usar el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para traducir interactivamente la voz de un idioma a voz en otro idioma. Una vez que se cumplen los requisitos previos, para la traducción de voz a voz solo son necesarios seis pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechTranslationConfig```` a partir de la clave y la región de suscripción.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar los idiomas de origen y destino.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar el nombre de la voz de la salida de voz.
> * Cree un objeto ````TranslationRecognizer```` con el objeto ````SpeechTranslationConfig```` anterior.
> * Con el objeto ````TranslationRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````TranslationRecognitionResult```` devuelto.
