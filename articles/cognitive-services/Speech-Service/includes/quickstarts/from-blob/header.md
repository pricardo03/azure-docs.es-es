---
title: 'Inicio rápido: Reconocimiento de la voz almacenada en Blob Storage: servicio Voz'
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
ms.openlocfilehash: 195c4c94f7ab00a8740c9dd14aad0d0fd0daa9b7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506103"
---
En este inicio rápido usará la [API REST Batch Transcription](../../../batch-transcription.md) para reconocer la voz almacenada en un [blob de SAS](https://aka.ms/ignite2019/speech/placeholder). Una vez que se cumplen los requisitos previos, para realizar el reconocimiento de voz mediante una API REST solo son necesarios unos cuantos pasos:
> [!div class="checklist"]
> * Envíe la solicitud JSON al servicio Voz para comenzar a transcribir la voz.
> * Compruebe el estado de la transcripción.
> * Cuando finalice, descargue los resultados de la transcripción.