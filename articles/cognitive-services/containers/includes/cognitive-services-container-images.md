---
title: Repositorios e imágenes de contenedor
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Dos tablas que representan los registros de contenedor, los repositorios y los nombres de imagen de todas las ofertas de Cognitive Service.
ms.service: cognitive-services
ms.topic: include
ms.date: 09/06/2019
ms.author: dapine
ms.openlocfilehash: 600c01fc2a273141b06996369231c2389c5d6d57
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775897"
---
### <a name="container-repositories-and-images"></a>Repositorios e imágenes de contenedor

Las tablas siguientes son una lista completa de las imágenes de contenedor disponibles que ofrece Azure Cognitive Services.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>"No validado" público (registro de contenedor: `mcr.microsoft.com`)

Microsoft Container Registry hospeda todos los contenedores "no validados" con disponibilidad general (GA) para Cognitive Services.

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
| [Computer Vision](../../Computer-vision/computer-vision-how-to-install-containers.md) | Reconocer texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Face](../../face/face-how-to-install-containers.md) | Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Form Recognizer](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Form Recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Speech Service API](../../speech-service/speech-container-howto.md) | Voz a texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Speech Service API](../../speech-service/speech-container-howto.md) | Texto a voz | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
