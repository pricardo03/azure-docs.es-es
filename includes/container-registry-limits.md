---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 844e4a0d2715799b808f2c7630c201f6e792bd63
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124419"
---
| Recurso | Básica | Estándar | Premium |
|---|---|---|---|
| Almacenamiento<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Tamaño máximo de la imagen de la capa | 20 GiB | 20 GiB | 50 GiB |
| Operaciones de lectura por minuto<sup>2, 3</sup> | 1000 | 3000 | 10 000 |
| Operaciones de escritura por minuto<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Ancho de banda de descarga en MBps<sup>2</sup> | 30 | 60 | 100 |
| Ancho de banda de carga en MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Replicación geográfica | N/D | N/D | [Admitido][geo-replication] |
| Confianza en el contenido (versión preliminar) | N/D | N/D | [Admitido][content-trust] |

<sup>1</sup>los límites de almacenamiento especificada son la cantidad de *incluyen* almacenamiento para cada nivel. Se le cobrará una tarifa diaria adicional por GiB de almacenamiento de imágenes por encima de estos límites. Para información de tarifas, consulte [precios de Azure Container Registry][pricing].

<sup>2</sup>*operaciones de lectura*, *operaciones de escritura*, y *ancho de banda* son estimaciones mínimas. Se esfuerza por mejorar el rendimiento que requiere el uso de Azure Container Registry.

<sup>3</sup>A [extracción de docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduce en varias operaciones de lectura en función del número de capas en la imagen, además de la recuperación de manifiesto.

<sup>4</sup>A [inserción de docker](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduce en varias operaciones de escritura, en función del número de capas que se deben insertar. Un elemento `docker push` incluye *operaciones de lectura* para recuperar un manifiesto para una imagen existente.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
