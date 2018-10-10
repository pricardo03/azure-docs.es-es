---
title: Regiones del servicio Speech
description: Referencia de las regiones del servicio Voz.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8485caeff3a7c96ed8f7403befac0026fae16e90
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987567"
---
# <a name="regions-of-the-speech-service"></a>Regiones del servicio Voz

El servicio Voz está disponible en distintas regiones.
Al crear una suscripción, puede seleccionar una región disponible según sus necesidades.

Al utilizar la suscripción, debe tener en cuenta la región que ha seleccionado.

## <a name="rest-api"></a>API DE REST

Use la API REST para seleccionar los puntos de conexión específicos de la región correctos.
Consulte las [API REST](rest-apis.md) para más información.

## <a name="speech-sdk"></a>SDK de voz

En las regiones de [SDK de Voz](speech-sdk.md), las regiones se especifican como una cadena (por ejemplo, como un parámetro `SpeechConfig.FromSubscription` en el SDK de Voz para C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regiones con reconocimiento y traducción de voz

En la tabla siguiente se enumeran las regiones disponibles para el **reconocimiento de voz** y la **traducción**.

  Region | Parámetro del SDK de Voz | Portal
 ------|-------|--------
 Oeste de EE. UU. | `westus` | https://westus.cris.ai
 Oeste de EE. UU. 2 | `westus2` | https://westus2.cris.ai 
 Este de EE. UU | `eastus` | https://eastus.cris.ai
 Este de EE. UU. 2 | `eastus2` | https://eastus2.cris.ai
 Asia oriental | `eastasia` | https://eastasia.cris.ai
 Sudeste de Asia | `southeastasia` | https://southeastasia.cris.ai
 Europa del Norte | `northeurope` | https://northeurope.cris.ai
 Europa occidental | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>Regiones con reconocimiento de intenciones

Las regiones disponibles para el **reconocimiento de la intención** mediante el SDK de Voz se enumeran en la [página de la región del servicio Language Understanding](/azure/cognitive-services/luis/luis-reference-regions).
Para cada región de publicación enumerada, el parámetro de región del SDK de Voz correspondiente se determina como la primera parte del nombre de dominio del punto de conexión.
Por ejemplo, use `westus` para especificar la región de publicación Oeste de EE. UU.
