---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: bc156b8c18f46cccf6fc775b82f76383b8c43861
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242118"
---
Aquí se proporciona una lista de los tipos de cuentas de almacenamiento compatibles y de almacenamiento para el dispositivo Data Box. Para una lista completa de todos los tipos distintos de cuentas de almacenamiento y todas sus funcionalidades, consulte [Tipos de cuentas de almacenamiento](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas*** |**Archivos de Azure** |**Notas**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y |
| Estándar de uso general v1  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v2  |  |Y | | |
| Estándar de Blob Storage |Y | | |Se admiten frecuentes y esporádicos. |

\* *-Los datos cargados para blobs en páginas deben ser de 512 bytes alineados como discos duros virtuales.*

>[!NOTE]
> No se admiten las cuentas de Azure Data Lake Storage Gen 2.
