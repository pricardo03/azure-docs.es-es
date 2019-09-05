---
title: Ejemplos de Docker de contenedor de análisis de sentimiento
titleSuffix: Azure Cognitive Services
description: Ejemplos de Docker de contenedor de análisis de sentimiento
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/21/2019
ms.author: dapine
ms.openlocfilehash: 369b0e30791b38dfa2048ac9f840123daa04ecba
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051100"
---
## <a name="sentiment-analysis-container-docker-examples"></a>Ejemplos de Docker de contenedor de análisis de sentimiento

Los siguientes ejemplos de Docker son para el contenedor de análisis de sentimiento.

### <a name="basic-example"></a>Ejemplo básico 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Ejemplo de registro 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
