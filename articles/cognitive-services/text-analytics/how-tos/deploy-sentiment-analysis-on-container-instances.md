---
title: 'Ejecución de Azure Container Instances: Text Analytics'
titleSuffix: Azure Cognitive Services
description: Implemente los contenedores de Text Analytics con la imagen de Análisis de sentimiento en Azure Container Instances y pruébelos en un explorador web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9ef529c9d505e5b305602c80a8dbef906f52269c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552527"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Implementar un contenedor de Análisis de sentimiento en Azure Container Instances

Aprenda a implementar el contenedor de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) de Cognitive Services con la imagen de Análisis de sentimiento en [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Este procedimiento ejemplifica la creación de un recurso de Text Analytics y la creación de una imagen de Análisis de sentimiento asociada, así como la posibilidad de usar esta orquestación de ambas acciones desde un explorador. El uso de contenedores puede desviar la atención de los desarrolladores de la administración de la infraestructura y centrarla en el desarrollo de aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

* Use una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Pasos siguientes 

* Uso de [Contenedores de Cognitive Services](../../cognitive-services-container-support.md)
* Uso del [servicio conectado de Text Analytics](../vs-text-connected-service.md)
