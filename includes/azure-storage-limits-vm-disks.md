---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261464"
---
Puede adjuntar un número de discos de datos a una máquina virtual de Azure. Según los objetivos de escalabilidad y rendimiento para discos de datos de la máquina virtual, puede determinar el número y tipo de disco que necesita para satisfacer los requisitos de capacidad y rendimiento.

> [!IMPORTANT]
> Para obtener un rendimiento óptimo, limite el número de discos muy usados que se conectan a la máquina virtual para evitar una posible limitación. Si todos los discos conectados no se usan mucho al mismo tiempo, la máquina virtual puede admitir un mayor número de discos.

**Discos administrados de Azure:**

En la tabla siguiente se muestra el valor predeterminado y los límites máximos del número de recursos por región y suscripción

> | Recurso | Límite predeterminado  | Límite máximo |
> | --- | --- | --- |
> | Discos administrados estándar | 25 000 | 50.000 |
> | Discos administrados de SSD estándar | 25 000 | 50.000 |
> | Managed Disks Premium | 25 000 | 50.000 |
> | Standard_LRS instantáneas | 25 000 | 50.000 |
> | Standard_ZRS instantáneas | 25 000 | 50.000 |
> | Imagen administrada | 25 000 | 50.000 |

* **Para las cuentas de almacenamiento estándar:** Una cuenta de almacenamiento estándar tiene una tasa de solicitud total máximo de 20 000 IOPS. Número total de IOPS en todos los discos de máquina virtual en una cuenta de almacenamiento estándar no debe superar este límite.
  
    Puede calcular aproximadamente el número de discos muy usados que admite una cuenta de almacenamiento estándar único basada en el límite de tasa de solicitud. Por ejemplo, para una máquina virtual de nivel básico, el número máximo de discos muy usados está alrededor de 66, que es de 20 000/300 IOPS por disco. El número máximo de discos muy usados para una máquina virtual de nivel estándar es aproximadamente de 40, que es de 20 000/500 IOPS por disco. 

* **Para las cuentas de almacenamiento Premium:** Una cuenta de almacenamiento Premium tiene una tasa de rendimiento total máximo de 50 Gbps. La capacidad total de proceso en todos los discos de la máquina virtual no debe superar este límite.

