---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: d5a4b3a07854c2664de7ec60f3677b666798a9bd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145404"
---
### <a name="standard-and-neural-voices"></a>Voces estándares y neuronales

Use esta tabla para determinar la disponibilidad de voces estándares y neuronales por región y punto de conexión:

| Área | Punto de conexión | Voces estándares | Voces neuronales |
|--------|----------|-----------------|---------------|
| Este de Australia | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Centro de Canadá | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Centro de EE. UU. | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Asia oriental | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Este de EE. UU | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Este de EE. UU. 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro de Francia | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| India central | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Este de Japón | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Corea Central | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro-Norte de EE. UU | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Europa del Norte | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Centro-Sur de EE. UU | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sudeste asiático | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sur de Reino Unido 2 | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Europa occidental | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Oeste de EE. UU. | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | No |
| Oeste de EE. UU. 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |

### <a name="custom-voices"></a>Voces personalizadas

Si ha creado una fuente de voz personalizados, use el punto de conexión que ha creado. También puede usar los puntos de conexión que se muestran a continuación, reemplazando el `{deploymentId}` con el identificador de implementación para el modelo de voz.

| Área | Punto de conexión |
|--------|----------|
| Este de Australia | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Canadá | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de EE. UU. | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de Asia | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU. | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU. 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Francia | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| India central | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de Japón | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Corea Central | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-Norte de EE. UU | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Norte de Europa | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-Sur de EE. UU | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sudeste asiático | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sur del Reino Unido 2 | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa occidental | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
