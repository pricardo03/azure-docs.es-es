---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 9f7e2760ef8bf06a2e680dce90c323672ca9d491
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66416061"
---
* La conversión requiere reiniciar la máquina virtual, por lo que debe programar la migración de las máquinas virtuales durante una ventana de mantenimiento existente previamente. 

* La conversión no es reversible. 

* Tenga en cuenta que los usuarios con el rol [Colaborador de la máquina virtual](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) no podrán cambiar el tamaño de la máquina virtual (como lo hacían antes de la conversión). El motivo es que las máquinas virtuales con discos administrados requieren que el usuario tenga el permiso de escritura/discos/Microsoft.Compute para los discos del sistema operativo.

* Asegúrese de probar la conversión. Migre una máquina virtual de prueba antes de realizar la migración en producción.

* Durante la conversión, se desasigna la VM. La VM recibe una nueva dirección IP cuando se inicia después de la conversión. Si es necesario, puede [asignar una dirección IP estática](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) a la máquina virtual.

* Revise la versión mínima del agente de máquina virtual de Azure necesario para admitir el proceso de conversión. Para más información acerca de cómo comprobar y actualizar la versión del agente, consulte [Soporte de versión mínima para los agentes de la máquina virtual en Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)