---
ms.openlocfilehash: fe9879f9574fe1496ebdf20ac76fe5b759d4ea6a
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051238"
---
### <a name="container-repositories-and-images"></a>Repositorios e imágenes de contenedor

Las tablas siguientes son una lista completa de las imágenes de contenedor disponibles que ofrece Azure Cognitive Services.

#### <a name="public-container-registry-mcrmicrosoftcom"></a>Público (registro de contenedor: `mcr.microsoft.com`)

Microsoft Container Registry hospeda todos los contenedores de disponibilidad general (GA) para Cognitive Services.

| Servicio | Contenedor | Container Registry/repositorio/nombre de imagen |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-preview-container-registry-containerpreviewazurecrio"></a>Versión preliminar pública (registro de contenedor: `containerpreview.azurecr.io`)

El registro de la versión preliminar del contenedor hospeda todos los contenedores de "versión preliminar pública" para Cognitive Services que aún no hayan cambiado a disponibilidad general (GA). Estos contenedores requieren una solicitud formal de acceso para poder consumirlos.

| Servicio | Contenedor | Container Registry/repositorio/nombre de imagen |
|--|--|--|
| [Anomaly Detector](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomaly Detector | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Reconocer texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Face](../../face/face-how-to-install-containers.md) | Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form Recognizer](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Speech Service API](../../speech-service/speech-container-howto.md) | Voz a texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md) | Texto a voz | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
