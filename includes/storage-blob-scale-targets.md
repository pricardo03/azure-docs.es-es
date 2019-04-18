---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/11/2019
ms.author: tamram
ms.openlocfilehash: b3e2f018a3f1ba2563ba8cf2df6dfd4959be592e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737287"
---
| Recurso | Destino        |
|----------|---------------|
| Tamaño máximo del contenedor de blob único | Igual que la capacidad de la cuenta de almacenamiento máximo |
| Número máximo de bloques en un bloque de blobs o blob en anexos | 50 000 bloques |
| Tamaño máximo de un bloque en un blob en bloques | 100 MiB |
| Tamaño máximo de un blob en bloques | 50 000 x 100 MiB (4,75 TiB aproximadamente) |
| Tamaño máximo de un bloque en un blob en anexos | 4 MiB |
| Tamaño máximo de un blob en anexos | 50 000 x 4 MiB (195 GiB aproximadamente) |
| Tamaño máximo de un blob en páginas | 8 TiB |
| Número máximo de directivas de acceso almacenadas por contenedor de blobs | 5 |
|Rendimiento de un blob |Hasta los límites de entrada/salida de cuenta de almacenamiento<sup>1</sup> |

<sup>1</sup> rendimiento único objeto depende de varios factores, incluidos, pero sin limitarse a: simultaneidad, el tamaño de la operación, nivel de rendimiento, velocidad de origen para las cargas y de destino para las descargas.