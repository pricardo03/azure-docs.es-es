---
title: Solución CloudSimple - información general de máquinas virtuales de VMware
description: Obtenga información sobre las máquinas virtuales de CloudSimple y sus ventajas.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 59f5438bbedea2ff7793a5df95f1d3df58b9bba6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576991"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Información general de virtual machines CloudSimple

CloudSimple le permite administrar máquinas virtuales de VMware desde Azure portal.  Un clúster o un grupo de recursos del clúster de vSphere se administra a través de Azure asignándola a la suscripción.  Máquina virtual de CloudSimple ofrece administración autoservicio de máquinas virtuales de VMware desde Azure portal.  

Para crear una VM CloudSimple de Azure, debe existir una plantilla de máquina virtual en el nube privada de vCenter.  La plantilla se usa para personalizar el sistema operativo y aplicaciones.  La plantilla de máquina virtual puede reforzar para cumplir las directivas de seguridad empresarial.  Puede usar la plantilla para crear máquinas virtuales y consumirlos desde Azure portal con un modelo de autoservicio.

## <a name="benefits"></a>Ventajas

Las máquinas virtuales de CloudSimple desde el portal de Azure proporcionan un mecanismo de autoservicio para que los usuarios crear y administrar máquinas virtuales de VMware.

* Crear una VM CloudSimple en el nube privada de vCenter
* Administrar las propiedades de la máquina virtual
  * Agregar o quitar discos
  * Agregar o quitar NIC
* Operaciones de energía de la VM CloudSimple
  * Encendido y apagado
  * Restablecer VM
* Eliminar la máquina virtual

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [consumen las máquinas virtuales de VMware en Azure](quickstart-create-vmware-virtual-machine.md)
* Obtenga información sobre cómo [asignar su suscripción de Azure](https://docs.azure.cloudsimple.com/azuresubscriptionmapping/)