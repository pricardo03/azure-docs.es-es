---
title: Docker pull para el contenedor de detección de idioma
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de detección de idioma
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: da6f04f1042d1a02178f345c5fe67387ae0a7d0f
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051322"
---
## <a name="pull-the-language-detection-container"></a>Extracción del contenedor de detección de idioma

Las imágenes de contenedor de Text Analytics están disponibles en Microsoft Container Registry.

| Contenedor | Container Registry/Repositorio/Nombre de imagen |
|-----------|------------|
| Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759) en Docker Hub.


### <a name="docker-pull-for-the-language-detection-container"></a>Docker pull para el contenedor de detección de idioma

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
