---
title: Repositorios e imágenes de contenedor
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Dos tablas que representan los registros de contenedor, los repositorios y los nombres de imagen de todas las ofertas de Cognitive Service.
ms.service: cognitive-services
ms.topic: include
ms.date: 11/15/2019
ms.author: dapine
ms.openlocfilehash: 2058dd6e52ddb417e24368b27384df9a222c378e
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74142219"
---
### <a name="container-repositories-and-images"></a>Repositorios e imágenes de contenedor

Las tablas siguientes son una lista de las imágenes de contenedor disponibles que ofrece Azure Cognitive Services. Para obtener una lista completa de todos los nombres de imagen de contenedor y sus etiquetas disponibles, consulte [Etiquetas de imágenes de contenedor de Azure Cognitive Services](../container-image-tags.md).

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>"No validado" público (registro de contenedor: `mcr.microsoft.com`)

Microsoft Container Registry sindica todos los contenedores "no validados" con disponibilidad general para Cognitive Services. También están disponibles directamente desde [Docker Hub](https://hub.docker.com/_/microsoft-azure-cognitive-services).

| Servicio | Contenedor | Container Registry/repositorio/nombre de imagen |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Text Analytics](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Versión preliminar pública "validada" (registro de contenedor: `containerpreview.azurecr.io`)

La versión preliminar del registro de contenedor hospeda todos los contenedores "validados" con disponibilidad general para Cognitive Services. Estos contenedores requieren una solicitud formal de acceso para poder consumirlos.

| Servicio | Contenedor | Container Registry/repositorio/nombre de imagen |
|--|--|--|
| [Anomaly Detector](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomaly Detector | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lectura | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Face](../../face/face-how-to-install-containers.md) | Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form Recognizer](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=stt) | Voz a texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=cstt) | Conversión de voz a texto personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=tts) | Texto a voz | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
| [Speech Service API](../../speech-service/speech-container-howto.md?tab=ctts) | Conversión de texto a voz personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` |
