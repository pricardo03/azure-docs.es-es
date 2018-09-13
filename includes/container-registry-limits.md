---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 08/30/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: bddb17a5163333a5aeb86b296a21867f0611d12f
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43303910"
---
| Recurso | Básica | Estándar | Premium |
|---|---|---|---|---|
| Almacenamiento<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Tamaño máximo de la capa de imagen | 20 GiB | 20 GiB | 50 GiB |
| Operaciones de lectura por minuto<sup>2, 3</sup> | 1000 | 3000 | 10 000 |
| Operaciones de escritura por minuto<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Ancho de banda de descarga en MBps<sup>2</sup> | 30 | 60 | 100 |
| Ancho de banda de carga en MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 100 |
| Replicación geográfica | N/D | N/D | [Admitido][geo-replication] |
| Confianza en el contenido (versión preliminar) | N/D | N/D | [Admitido][content-trust] |

<sup>1</sup> Los límites de almacenamiento especificados son la cantidad de almacenamiento *incluido* para cada nivel. Se le cobrará una tarifa diaria adicional por GiB de almacenamiento de imágenes por encima de estos límites. Para más información, consulte [Precios de Container Registry][pricing].

<sup>2</sup> Las *operaciones de lectura*, las *operaciones de escritura* y el *ancho de banda* son estimaciones mínimas. ACR trata de mejorar el rendimiento que el uso requiere.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduce en varias operaciones de lectura en función del número de capas de la imagen, además de la recuperación de manifiesto.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduce en varias operaciones de escritura, en función del número de capas que se deben insertar. Un elemento `docker push` incluye *operaciones de lectura* para recuperar un manifiesto para una imagen existente.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md