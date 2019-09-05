---
title: Docker pull para el contenedor de análisis de sentimiento
titleSuffix: Azure Cognitive Services
description: Comando pull de Docker para el contenedor de análisis de sentimiento
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: 0954ef88f6917159156fbb73103b7b7af3283c00
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051283"
---
## <a name="pull-the-sentiment-analysis-container"></a>Uso del comando pull con el contenedor de análisis de sentimiento

Las imágenes de contenedor de Text Analytics están disponibles en Microsoft Container Registry.

| Contenedor | Container Registry/repositorio/nombre de imagen |
|-----------|------------|
| Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor de [análisis de sentimiento](https://go.microsoft.com/fwlink/?linkid=2018654) en Docker Hub.

### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Docker pull para el contenedor de análisis de sentimiento

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
