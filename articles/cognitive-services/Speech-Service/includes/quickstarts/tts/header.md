---
title: 'Inicio rápido: Síntesis de voz: servicio de voz'
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
ms.openlocfilehash: d19f779f67cc0dea8cc7f06aa275885d75c3092e
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818352"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para convertir texto en voz sintetizada. Una vez que se cumplen los requisitos previos, para la representación de voz sintetizada en los altavoces predeterminados solo son necesarios cuatro pasos:
> [!div class="checklist"]
> * Cree un objeto ````SpeechConfig```` a partir de la clave y la región de suscripción.
> * Cree un objeto ````SpeechSynthesizer```` con el objeto ````SpeechConfig```` anterior.
> * Use el objeto ````SpeechSynthesizer```` para pronunciar el texto.
> * Compruebe el objeto ````SpeechSynthesisResult```` devuelto en caso de errores.
