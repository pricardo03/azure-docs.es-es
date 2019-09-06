---
title: Recomendaciones y requisitos del contenedor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 8/26/2019
ms.author: dapine
ms.openlocfilehash: 5da088bf0356dd54d98ec5393fd2db8068f9c666
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034487"
---
En la tabla siguiente se describen los núcleos de CPU y memoria mínimos y recomendados para asignar cada contenedor de Reconocer texto.

| Contenedor | Mínima | Recomendado |TPS<br>(mínimo, máximo)|
|-----------|---------|-------------|--|
|Reconocer texto|1 núcleo, 8 GB de memoria, 0,5 TPS|2 núcleos, 8 GB de memoria, 1 TPS|0,5, 1|

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TPS: transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.