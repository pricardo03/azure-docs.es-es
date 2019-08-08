---
title: 'VMware Solution by CloudSimple: introducción a las máquinas virtuales de Azure'
description: Obtenga información sobre las máquinas virtuales de CloudSimple y sus ventajas.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0f4967bbd12107bf6a04cb80537d4425c75c5f46
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812489"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Introducción a las máquinas virtuales de CloudSimple

CloudSimple le permite administrar máquinas virtuales de VMware desde Azure Portal.  Un clúster o un grupo de recursos del clúster de vSphere se administra a través de Azure y se asigna a la suscripción.  La máquina virtual de CloudSimple ofrece administración autoservicio de las máquinas virtuales de VMware desde Azure Portal.  

Para crear una máquina virtual de CloudSimple desde Azure, debe existir una plantilla de máquina virtual en vCenter de la nube privada.  La plantilla se usa para personalizar el sistema operativo y las aplicaciones.  La máquina virtual con plantilla se puede reforzar para cumplir las directivas de seguridad empresarial.  Puede usar la plantilla para crear máquinas virtuales y consumirlas desde Azure Portal con un modelo de autoservicio.

## <a name="benefits"></a>Ventajas

Las máquinas virtuales de CloudSimple de Azure Portal proporcionan un mecanismo de autoservicio para que los usuarios creen y administren las máquinas virtuales de VMware.

* Creación de una máquina virtual de CloudSimple en vCenter de la nube privada
* Administración de las propiedades de la máquina virtual
  * Adición o retirada de discos
  * Adición o retirada de NIC
* Operaciones de encendido de la máquina virtual de CloudSimple
  * Encendido y apagado
  * Restablecimiento de la máquina virtual
* Eliminación de la máquina virtual

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre el [Uso de máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Obtenga información sobre la [Asignación de la suscripción a Azure](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)