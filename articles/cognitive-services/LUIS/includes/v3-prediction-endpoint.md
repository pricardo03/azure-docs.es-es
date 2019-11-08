---
title: Obtención del punto de conexión de predicción de V3
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: bc6e43faca47e360daa8214e6b9f6e9df4a2f130
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495240"
---
1. En el portal de LUIS, vaya a la sección Manage (Administrar) del menú superior derecho y, en la página Keys and endpoints (Claves y puntos de conexión) del menú de la izquierda, seleccione la dirección URL del punto de conexión al final de la página.

    Esta acción abre una pestaña del explorador con la dirección URL del punto de conexión en la barra de direcciones.

    La dirección URL tiene el identificador de la aplicación, la clave y el nombre de la ranura. La dirección URL del punto de conexión de predicción de V3 tiene el siguiente aspecto:

    `https://REGION.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-KEY&<optional-name-value-pairs>&query=UTTERANCE-TEXT.`

