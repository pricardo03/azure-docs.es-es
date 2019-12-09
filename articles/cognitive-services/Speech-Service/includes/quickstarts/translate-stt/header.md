---
title: 'Inicio rápido: Traducción de voz en texto: servicio de voz'
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
ms.openlocfilehash: cd45657beacd04eb2376af3e4297eae051157778
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816934"
---
En este inicio rápido, va a usar el SDK de [Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para traducir interactivamente la voz de un idioma a texto en otro idioma. Una vez que se cumplen los requisitos previos, para la traducción de voz a texto solo son necesarios cinco pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Actualice el objeto ````SpeechConfig```` para especificar los idiomas de origen y destino.
> * Cree un objeto ````TranslationRecognizer```` con el objeto ````SpeechConfig```` anterior.
> * Con el objeto ````TranslationRecognizer````, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto ````TranslationRecognitionResult```` devuelto.
