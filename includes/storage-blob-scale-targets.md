---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 0fda881b805eb3a967cf3b05f6c6df8c65d20730
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905798"
---
| Resource | Destino        |
|----------|---------------|
| Tamaño máximo de contenedor de un blob | Igual que la capacidad máxima de la cuenta de almacenamiento |
| Número máximo de bloques en un blob en bloques o blob en anexos | 50 000 bloques |
| Tamaño máximo de un bloque en un blob en bloques | 100 MiB |
| Tamaño máximo de un blob en bloques | 50 000 x 100 MiB (4,75 TiB, aproximadamente) |
| Tamaño máximo de un bloque en un blob en anexos | 4 MiB |
| Tamaño máximo de un blob en anexos | 50 000 x 4 MiB (195 GiB, aproximadamente) |
| Tamaño máximo de un blob en páginas | 8 TiB |
| Número máximo de directivas de acceso almacenadas por contenedor de blobs | 5 |
|Velocidad de solicitudes de destino para un solo blob | Hasta 500 solicitudes por segundo |
|Rendimiento de destino para un blob en una sola página | Hasta 60 MiB por segundo |
|Rendimiento de destino para un blob en un solo bloque |Hasta los límites de entrada/salida de cuenta de almacenamiento<sup>1</sup> |

<sup>1</sup> El rendimiento de un único objeto depende de varios factores, incluidos, pero sin limitarse a: simultaneidad, tamaño de la solicitud, nivel de rendimiento, velocidad de origen para las cargas y destino para las descargas. Para aprovechar las ventajas de las mejoras de rendimiento del [blob en bloques de alto rendimiento](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/), utilice un tamaño de la solicitud Put Blob o Put Block de > 4 MiB (> 256 KiB para el almacenamiento de blobs en bloques de rendimiento premium o para Data Lake Storage Gen2).
