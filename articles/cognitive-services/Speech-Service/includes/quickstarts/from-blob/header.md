---
title: 'Inicio rápido: Reconocimiento de la voz almacenada en Blob Storage: servicio de voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: ef61139056ad194d89e2a67cb4ac1b1414a3380b
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828801"
---
En este inicio rápido usará la [API REST Batch Transcription](../../../batch-transcription.md) para reconocer la voz almacenada en un [blob de SAS](https://aka.ms/ignite2019/speech/placeholder). Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz mediante una API REST solo son necesarios unos cuantos pasos:
> [!div class="checklist"]
> * Envíe la solicitud JSON al servicio de voz para comenzar a transcribir la voz.
> * Compruebe el estado de la transcripción.
> * Cuando finalice, descargue los resultados de la transcripción.