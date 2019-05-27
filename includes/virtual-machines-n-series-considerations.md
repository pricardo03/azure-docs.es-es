---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1de7221f100077e07a2211bdb94e0198b35cb77c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170404"
---
## <a name="deployment-considerations"></a>Consideraciones de la implementación

* Para ver la disponibilidad de máquinas virtuales de la serie N, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).

* Las máquinas virtuales de la serie N solo se pueden implementar en el modelo de implementación de Resource Manager.

* Las máquinas virtuales de serie N difieren en el tipo de Azure Storage que admiten en sus discos. Las máquinas virtuales NC y NV solo admiten discos de máquina virtual respaldados por Disk Storage (HDD) estándar. Las máquinas virtuales NCv2, NCv3, ND, NDv2 y NVv2 solo admiten discos de máquina virtual respaldados por Disk Storage (SSD) Premium.

* Si desea implementar más de un pequeño número de máquinas virtuales de la serie N, considere la posibilidad de usar una suscripción de pago por uso u otras opciones de compra. Si usa una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), solo puede usar un número limitado de núcleos de proceso de Azure.

* Es posible que necesite aumentar la cuota de núcleos (por región) de la suscripción de Azure y la cuota independiente para los núcleos NC, NCv2, NCv3, ND, NDv2, NV o NVv2. Para solicitar un aumento de cuota, [abra una solicitud de soporte técnico al cliente en línea](../articles/azure-supportability/how-to-create-azure-support-request.md) sin cargo alguno. Los límites predeterminados pueden variar según la categoría de suscripción.




