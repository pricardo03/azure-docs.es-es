---
title: 'Inicio rápido: Traducción de voz a varios idiomas: servicio de voz'
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
ms.openlocfilehash: bc35c1efcad2ca9ebb5eaf23dd84f7189858a159
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74817319"
---
En este inicio rápido, va a usar el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para traducir interactivamente la voz de un idioma a voz en otro idioma. Una vez que se cumplen los requisitos previos, para la traducción de voz a texto en varios idiomas solo son necesarios seis pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechTranslationConfig```` a partir de la clave y la región de suscripción.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar el idioma de origen del reconocimiento de voz.
> * Actualice el objeto ````SpeechTranslationConfig```` para especificar varios idiomas de destino para la traducción.
> * Cree un objeto ````TranslationRecognizer```` con el objeto ````SpeechTranslationConfig```` anterior.
> * Con el objeto ````TranslationRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````TranslationRecognitionResult```` devuelto.
