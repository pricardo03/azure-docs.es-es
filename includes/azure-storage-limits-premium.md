---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/01/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e878ca23b9187fe3175ad0af1b4f27e59e1deef6
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509915"
---
### <a name="premium-performance-block-blob-storage"></a>Almacenamiento de blobs en bloques de rendimiento Premium

Una cuenta de almacenamiento de blob en bloques de alto rendimiento está optimizada para aplicaciones que utilizan objetos más pequeños, con un rango de kilobytes. Es ideal para aplicaciones que requieren altas tasas de transacción o almacenamiento coherente de baja latencia. El almacenamiento de blob en bloques de rendimiento prémium está diseñado para escalar con las aplicaciones. Si piensa implementar aplicaciones que requieren cientos de miles de solicitudes por segundo o petabytes de capacidad de almacenamiento, póngase en contacto con nosotros mediante una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="premium-performance-filestorage"></a>Almacenamiento de archivos de rendimiento prémium

[!INCLUDE [azure-storage-limits-filestorage](azure-storage-limits-filestorage.md)]

 Para los objetivos de escala de uso compartido de archivos prémium, consulte la sección [Objetivos de escala de archivos prémium](../articles/storage/common/storage-scalability-targets.md#premium-files-scale-targets).

### <a name="premium-performance-page-blob-storage"></a>Almacenamiento de blob en páginas de rendimiento Premium

Las cuenta de almacenamiento de rendimiento prémium, uso general v1 o v2 tienen los siguientes objetivos de escalabilidad:

| Capacidad total de la cuenta                            | Ancho de banda total de una cuenta de almacenamiento con redundancia local                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Capacidad de disco: 35 TB <br>Capacidad de instantánea: 10 TB | Hasta 50 gigabits por segundo de entrada<sup>1</sup> y salida<sup>2</sup> |

<sup>1</sup> Todos los datos (solicitudes) que se envían a una cuenta de almacenamiento

<sup>1</sup> Todos los datos (respuestas) que se reciben desde una cuenta de almacenamiento

Si usa cuentas de almacenamiento de rendimiento prémium para los discos no administrados y la aplicación supera los objetivos de escalabilidad de una cuenta de almacenamiento individual, es posible que quiera migrar a discos administrados. Si no desea migrar a Managed Disks, compile la aplicación para que use varias cuentas de almacenamiento. A continuación, divida los datos en esas cuentas de almacenamiento. Por ejemplo, si desea asociar discos de 51 TB entre varias VM, distribúyalos entre dos cuentas de almacenamiento. 35 TB es el límite de una cuenta de Premium Storage única. Asegúrese de que una sola cuenta de almacenamiento de rendimiento prémium nunca tenga más de 35 TB de discos aprovisionados.