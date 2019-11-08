---
title: 'Inicio rápido: Síntesis de voz: Servicio Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a sintetizar la voz mediante el SDK de Voz.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 263a8e98c4c029728272c020efe00e82be20f5e2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505079"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para convertir texto en voz sintetizada. Una vez que se cumplen los requisitos previos, para la representación de voz sintetizada en los altavoces predeterminados solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````SpeechSynthesizer```` con el objeto ````SpeechConfig```` anterior.
> * Use el objeto ````SpeechSynthesizer```` para pronunciar el texto.
> * Compruebe el objeto ````SpeechSynthesisResult```` devuelto en caso de errores.
