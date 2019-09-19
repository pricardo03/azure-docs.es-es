---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/14/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: ee8ff3529524a63ca2e54a64327570197f363538
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "67186679"
---
| Recurso | Básica | Estándar | Premium |
|---|---|---|---|
| Almacenamiento<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Tamaño máximo de la capa de imagen | 200 GiB | 200 GiB | 200 GiB |
| Operaciones de lectura por minuto<sup>2, 3</sup> | 1000 | 3000 | 10 000 |
| Operaciones de escritura por minuto<sup>2, 4</sup> | 100 | 500 | 2\.000 |
| Ancho de banda de descarga en MBps<sup>2</sup> | 30 | 60 | 100 |
| Ancho de banda de carga en MBps<sup>2</sup> | 10 | 20 | 50 |
| webhooks | 2 | 10 | 100 |
| Replicación geográfica | N/D | N/D | [Compatible][geo-replication] |
| Confianza de contenido | N/D | N/D | [Compatible][content-trust] |

<sup>1</sup> Los límites de almacenamiento especificados son la cantidad de almacenamiento *incluido* para cada nivel. Se le cobrará una tarifa diaria adicional por GiB de almacenamiento de imágenes por encima de estos límites. Para más información, consulte [Precios de Azure Container Registry][pricing].

<sup>2</sup>*ReadOps*, *WriteOps* y *ancho de banda* son estimaciones mínimas. Azure Container Registry se esfuerza por mejorar el rendimiento adaptado a su uso.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduce en varias operaciones de lectura en función del número de capas de la imagen, además de la recuperación del manifiesto.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduce en varias operaciones de escritura, en función del número de capas que se deben insertar. Un elemento `docker push` incluye *operaciones de lectura* para recuperar un manifiesto para una imagen existente.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
