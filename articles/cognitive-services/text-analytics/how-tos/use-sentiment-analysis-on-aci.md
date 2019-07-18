---
title: Ejecutar Azure Container Instances
titleSuffix: Text Analytics - Azure Cognitive Services
description: Implemente los contenedores de Text Analytics con la imagen de Análisis de sentimiento en Azure Container Instances y pruébelos en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a82b5a1cbed662289d3a406a61fdd19a3ca8861b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454789"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances-aci"></a>Implementar un contenedor de Análisis de sentimiento en Azure Container Instances (ACI)

Aprenda a implementar el contenedor de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) de Cognitive Services con la imagen de Análisis de sentimiento en [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI). Este procedimiento ejemplifica la creación de un recurso de Text Analytics y la creación de una imagen de Análisis de sentimiento asociada, así como la posibilidad de usar esta orquestación de ambas acciones desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la gestión de la infraestructura y llevarles a centrarse en el desarrollo de aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

* Use una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances (ACI)](../../containers/includes/create-aci-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Pasos siguientes 

* Uso de [Contenedores de Cognitive Services](../../cognitive-services-container-support.md)
* Usar el [servicio conectado de Text Analytics](../vs-text-connected-service.md)
