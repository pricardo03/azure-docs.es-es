---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 5c1caf87f32ddd827b85263ee634d3e15d821124
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52272337"
---
Las máquinas virtuales de IaaS (máquinas virtuales) y las instancias de rol de PaaS de una red virtual reciben automáticamente una dirección IP privada de un intervalo que especifique, según la subred a la que se conecten. Esa dirección la conservan las máquinas virtuales y las instancias de rol, hasta que se retiren. Retira una máquina virtual o una instancia de rol deteniéndola desde PowerShell, la CLI de Azure o el portal de Azure. En esos casos, cuando la máquina virtual o la instancia de rol se inicia de nuevo, recibirá una dirección IP disponible de la infraestructura de Azure, que puede que no sea la misma que la que tenía anteriormente. Si apaga la VM o la instancia de rol desde el sistema operativo invitado, conserva la dirección IP que tenía.  

En determinados casos, desea que una máquina virtual o una instancia de rol tenga una dirección IP estática, por ejemplo, si la máquina virtual va a ejecutar DNS o va a ser un controlador de dominio. Puede hacerlo configurando una dirección IP privada estática.

