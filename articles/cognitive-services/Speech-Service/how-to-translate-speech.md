---
title: Traducción de voz mediante los servicios de voz | Microsoft Docs
description: Obtenga información sobre cómo usar traducción de voz en el servicio Voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 6acfcf0ae8ab4c63e6cc943f93da6b947f3d118c
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071103"
---
# <a name="translate-speech-using-speech-service"></a>Traducción de voz mediante el servicio Voz

El [SDK de Voz](speech-sdk.md) es la manera más sencilla de usar la traducción de voz en la aplicación. El SDK proporciona toda la funcionalidad del servicio. El proceso básico para llevar a cabo la traducción de voz incluye los pasos siguientes:

1. Cree un generador de voz y proporcione una clave de suscripción al servicio Voz y una [región](regions.md) o un token de autorización. En este momento también se configuran los idiomas de traducción de origen y de destino, y se especifica si quiere salida de texto o de voz.

2. Obtenga un reconocedor del generador. Para la traducción, seleccione un reconocedor de traducción. (Los otros identificadores son para *Conversión de voz en texto*). Hay distintas versiones de reconocedor de traducción en función del origen de audio que se use.

4. Asocie eventos para la operación asincrónica, si quiere. El reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales. En caso contrario, la aplicación recibe un resultado de traducción final.

5. Inicie el reconocimiento y la traducción.

# <a name="sdk-samples"></a>Ejemplos del SDK

Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos del SDK de Voz de Cognitive Services](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconocer la voz en C#](quickstart-csharp-dotnet-windows.md)
