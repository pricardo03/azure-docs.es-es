---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 202bffb03a73acde67f0d4a03b31aa9a6fbf9824
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901951"
---
## <a name="deployment-considerations"></a>Consideraciones de la implementación

* Para ver la disponibilidad de máquinas virtuales de la serie N, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).

* Las máquinas virtuales de la serie N solo se pueden implementar en el modelo de implementación de Resource Manager.

* Las máquinas virtuales de serie N difieren en el tipo de Azure Storage que admiten en sus discos. Las máquinas virtuales NC y NV solo admiten discos de máquina virtual respaldados por Disk Storage (HDD) estándar. Las máquinas virtuales NCv2, NCv3, ND, NDv2 y NVv2 solo admiten discos de máquina virtual respaldados por Disk Storage (SSD) Premium.

* Si desea implementar más de un pequeño número de máquinas virtuales de la serie N, considere la posibilidad de usar una suscripción de pago por uso u otras opciones de compra. Si usa una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), solo puede usar un número limitado de núcleos de proceso de Azure.

* Es posible que necesite aumentar la cuota de núcleos (por región) de la suscripción de Azure y la cuota independiente para los núcleos NC, NCv2, NCv3, ND, NDv2, NV o NVv2. Para solicitar un aumento de cuota, [abra una solicitud de soporte técnico al cliente en línea](../articles/azure-portal/supportability/how-to-create-azure-support-request.md) sin cargo alguno. Los límites predeterminados pueden variar según la categoría de suscripción.




