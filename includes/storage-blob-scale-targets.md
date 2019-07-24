---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 4/20/2019
ms.author: tamram
ms.openlocfilehash: aab17966862c57a52f252b3c4e9b757673078b0a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186527"
---
| Recurso | Destino        |
|----------|---------------|
| Tamaño máximo de contenedor de un blob | Igual que la capacidad máxima de la cuenta de almacenamiento |
| Número máximo de bloques en un blob en bloques o blob en anexos | 50 000 bloques |
| Tamaño máximo de un bloque en un blob en bloques | 100 MiB |
| Tamaño máximo de un blob en bloques | 50 000 x 100 MiB (4,75 TiB, aproximadamente) |
| Tamaño máximo de un bloque en un blob en anexos | 4 MiB |
| Tamaño máximo de un blob en anexos | 50 000 x 4 MiB (195 GiB, aproximadamente) |
| Tamaño máximo de un blob en páginas | 8 TiB |
| Número máximo de directivas de acceso almacenadas por contenedor de blobs | 5 |
|Rendimiento de un blob |Hasta los límites de entrada/salida de cuenta de almacenamiento<sup>1</sup> |

<sup>1</sup> El rendimiento de un único objeto depende de varios factores, incluidos, pero sin limitarse a: simultaneidad, tamaño de la solicitud, nivel de rendimiento, velocidad de origen para las cargas y destino para las descargas. Para aprovechar las ventajas de las mejoras de rendimiento del [blob en bloques de alto rendimiento](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/), utilice un tamaño de la solicitud Put Blob o Put Block de > 4 MiB (> 256 KiB para el almacenamiento de blobs en bloques de rendimiento premium o para Data Lake Storage Gen2).
