---
title: Docker pull para el contenedor de extracción de frases clave
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de extracción de frases clave
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: e71db5455a3d6726cd6567d568a13fcdcf9cb520
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051366"
---
## <a name="pull-the-key-phrase-extraction-container"></a>Extracción del contenedor de extracción de frases clave

Las imágenes de contenedor de Text Analytics están disponibles en Microsoft Container Registry.

| Contenedor | Container Registry/Repositorio/Nombre de imagen |
|-----------|------------|
| Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Extracción de frases clave](https://go.microsoft.com/fwlink/?linkid=2018757) en Docker Hub.

### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker pull para el contenedor de extracción de frases clave

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
