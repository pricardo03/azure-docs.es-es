---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: d5a4b3a07854c2664de7ec60f3677b666798a9bd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186605"
---
### <a name="standard-and-neural-voices"></a>Voces estándares y neuronales

Utilice esta tabla para determinar la disponibilidad de las voces estándar y neuronales por región o punto de conexión:

| Region | Punto de conexión | Voces estándar | Voces neuronales |
|--------|----------|-----------------|---------------|
| Este de Australia | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Centro de Canadá | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Centro de EE. UU. | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Asia oriental | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Este de EE. UU | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Este de EE. UU. 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Centro de Francia | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| India central | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Este de Japón | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Corea Central | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Centro-Norte de EE. UU | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Europa del Norte | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Centro-Sur de EE. UU | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sudeste asiático | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Sur de Reino Unido 2 | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Europa occidental | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |
| Oeste de EE. UU. | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sin |
| Oeste de EE. UU. 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Sí | Sí |

### <a name="custom-voices"></a>Voces personalizadas

Si ha creado una fuente de voz personalizada, use el punto de conexión que ha creado. También puede utilizar los puntos de conexión que se indican a continuación; para ello, reemplace `{deploymentId}` por el identificador de implementación para el modelo de voz.

| Region | Punto de conexión |
|--------|----------|
| Este de Australia | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Canadá | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de EE. UU. | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Asia oriental | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de EE. UU. 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro de Francia | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| India central | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Este de Japón | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Corea Central | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-Norte de EE. UU | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa del Norte | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Centro-Sur de EE. UU | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sudeste asiático | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Sur del Reino Unido 2 | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa occidental | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Oeste de EE. UU. 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
