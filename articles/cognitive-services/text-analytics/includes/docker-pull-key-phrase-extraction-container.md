---
title: Docker pull para el contenedor de extracción de frases clave
titleSuffix: Azure Cognitive Services
description: Comando docker pull para el contenedor de extracción de frases clave
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 59313528e4beb343f9ac1103d3f9f2faae5dfc16
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966726"
---
#### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker pull para el contenedor de extracción de frases clave

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte el contenedor [Extracción de frases clave](https://go.microsoft.com/fwlink/?linkid=2018757) en Docker Hub.

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
