---
title: 'Azure VMware Solutions (AVS): introducción a las máquinas virtuales'
description: Obtenga información sobre las máquinas virtuales de AVS y sus ventajas.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0e6cc6d51e37cea98cc553a236295311d21f595a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024915"
---
# <a name="avs-virtual-machines-overview"></a>Introducción a las máquinas virtuales de AVS

AVS le permite administrar máquinas virtuales (VM) de VMware desde Azure Portal. Un clúster o un grupo de recursos del clúster de vSphere se administra a través de Azure y se asigna a la suscripción.

Para crear una máquina virtual de AVS desde Azure, debe existir una plantilla de VM en la instancia de vCenter de la nube privada de AVS. La plantilla se usa para personalizar el sistema operativo y las aplicaciones. La máquina virtual con plantilla se puede reforzar para cumplir las directivas de seguridad empresarial. Puede usar la plantilla para crear VM y consumirlas desde Azure Portal con un modelo de autoservicio.

## <a name="benefits"></a>Ventajas

Las máquinas virtuales de AVS de Azure Portal proporcionan un mecanismo de autoservicio para que los usuarios creen y administren las máquinas virtuales de VMware.

* Creación de una VM de AVS en la instancia de vCenter de la nube privada de AVS
* Administración de las propiedades de la máquina virtual
  * Adición o retirada de discos
  * Adición o retirada de NIC
* Operaciones de encendido de la VM de AVS
  * Encendido y apagado
  * Restablecimiento de la máquina virtual
* Eliminación de la máquina virtual

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre el [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Obtenga información sobre la [Asignación de la suscripción a Azure](azure-subscription-mapping.md)
