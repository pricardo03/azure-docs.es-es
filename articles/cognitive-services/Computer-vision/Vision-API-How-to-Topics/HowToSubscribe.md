---
title: 'Obtención de las claves de suscripción: Computer Vision'
titlesuffix: Azure Cognitive Services
description: Vea cómo se obtienen las claves de suscripción para las llamadas a Computer Vision API en Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: c4c7ee1ab756f458c6e9fed4ff5ee54dfaba919e
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222242"
---
# <a name="how-to-obtain-subscription-keys"></a>Cómo obtener las claves de suscripción

Los servicios de Computer Vision requieren una clave de suscripción especial. Cada llamada a Computer Vision API requiere una clave de suscripción. Esta clave se debe pasar a través de un parámetro de cadena de consulta o se debe especificar en el encabezado de la solicitud.

Para registrarse y obtener claves de suscripción, consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). El registro es gratis. El precio por los servicios está sujeto a cambios.

>[!NOTE]
Sus claves de suscripción son válidas únicamente para estas [regiones de Microsoft Azure](https://azure.microsoft.com/regions/). 

| Region | Dirección |
|---|---|
| Oeste de EE. UU. | westus.api.cognitive.microsoft.com |
| Este de EE. UU. 2 | eastus2.api.cognitive.microsoft.com |
| Centro occidental de EE.UU. | westcentralus.api.cognitive.microsoft.com |
| Europa occidental | westeurope.api.cognitive.microsoft.com |
| Sudeste asiático | southeastasia.api.cognitive.microsoft.com |

Si se registra usando la versión de evaluación de Computer Vision, las claves de suscripción serán válidas para la región **westcentral** (`https://westcentralus.api.cognitive.microsoft.com/`). Ese es el caso más común. Aún así, si se registra en Computer Vision con su cuenta de Microsoft Azure a través del sitio [https://azure.microsoft.com/](https://azure.microsoft.com/), especificará la región de su versión de evaluación mediante la lista de regiones antes mencionada.

Por ejemplo, si se registra en Computer Vision con su cuenta de Microsoft Azure y especifica `westus` para su región, deberá usar la región `westus` para sus llamadas a la API de REST (`https://westus.api.cognitive.microsoft.com/`).

Si olvida la región de su clave de suscripción después de obtener la clave de evaluación, puede encontrar su región en [https://azure.microsoft.com/try/cognitive-services/my-apis/](https://azure.microsoft.com/try/cognitive-services/my-apis/).

### <a name="related-links"></a>Vínculos relacionados:

* [Detalles de precios de Azure Cognitive Services APIs](https://azure.microsoft.com/pricing/details/cognitive-services/)
