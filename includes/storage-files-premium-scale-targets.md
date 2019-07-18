---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: 368f08272173b019873dfe20e1164d6baf72ff5e
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542655"
---
#### <a name="additional-premium-file-share-level-limits"></a>Límites del nivel de recurso compartido de archivos prémium adicionales

|Ámbito  |Destino  |
|---------|---------|
|Aumento o disminución del tamaño mínimo    |1 GiB      |
|IOPS base    |1 IOPS por GiB hasta 100 000|
|Ampliación de IOPS    |3 x IOPS por GiB hasta 100 000|
|Velocidad de salida         |60 MiB/s + 0,06 * GiB aprovisionado        |
|Velocidad de entrada| 40 MiB/s + 0,04 * GiB aprovisionado |

#### <a name="file-level-limits"></a>Límites de nivel de archivo

|Ámbito  |Archivo prémium  |Archivo estándar |
|---------|---------|---------|
|Size                  |1 TiB         |1 TiB|
|IOPS máximas por archivo     |5\.000         |1000|
|Identificadores simultáneos    |2\.000         |2\.000|
|Entrada  |300 MiB/s|      Consulte los valores de rendimiento de archivo estándar|
|Salida   |200 MiB/s| Consulte los valores de rendimiento de archivo estándar|
|Throughput| Consulte los valores de entrada y salida de archivos prémium| Hasta 60 MiB/s|