---
title: Recomendaciones y requisitos del contenedor
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/18/2019
ms.author: dapine
ms.openlocfilehash: 47c889a428ec2916dc8cad73e30ef5ff630372a7
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129820"
---
> [!NOTE]
> Los requisitos y las recomendaciones se basan en pruebas comparativas con una única solicitud por segundo, con una imagen de 8 MB de una carta comercial digitalizada que contiene 29 líneas y un total de 803 caracteres.

#### <a name="readtabread"></a>[Lectura](#tab/read)

En la tabla siguiente se describe la asignación mínima y recomendada de recursos para cada contenedor de lectura.

| Contenedor | Mínima | Recomendado |TPS<br>(mínimo, máximo)|
|-----------|---------|-------------|--|
| Lectura | 1 núcleo, 8 GB de memoria, 0,24 TPS | 8 núcleos, 16 GB de memoria, 1,17 TPS | 0,24, 1,17 |

#### <a name="recognize-texttabrecognize-text"></a>[Reconocer texto](#tab/recognize-text)

En la tabla siguiente se describe la asignación mínima y recomendada de recursos para cada contenedor Reconocer texto.

| Contenedor | Mínima | Recomendado |TPS<br>(mínimo, máximo)|
|-----------|---------|-------------|--|
| Reconocer texto | 1 núcleo, 8 GB de memoria, 0,12 TPS | 8 núcleos, 16 GB de memoria, 0,60 TPS | 0,12, 0,60 |

***

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TPS: transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.
