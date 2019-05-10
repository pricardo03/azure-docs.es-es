---
title: Direcciones IP compartidas en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo Azure DevTest Labs usa direcciones IP compartidas para minimizar las direcciones IP públicas necesarias para acceder a las máquinas virtuales de su laboratorio.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2019
ms.author: spelluru
ms.openlocfilehash: f7c9feedddab1aea031cb3a8879e868aae04df00
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236876"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Direcciones IP compartidas en Azure DevTest Labs

Las máquinas virtuales de laboratorio de Azure DevTest Labs comparten la misma dirección para minimizar el número de direcciones IP públicas necesarias para acceder a estas máquinas virtuales.  En este artículo se describe cómo funcionan las direcciones IP compartidas y sus opciones de configuración relacionadas.

## <a name="shared-ip-setting"></a>Configuración de IP compartidas

Cuando se crea un laboratorio, se crea en una subred de una red virtual.  De forma predeterminada, esta subred se crea con la opción **Habilitar IP pública compartida** establecida en *Sí*.  Esta configuración crea una dirección IP pública para toda la subred.  Para obtener más información sobre cómo configurar redes virtuales y subredes, vea [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Nueva subred de laboratorio](media/devtest-lab-shared-ip/lab-subnet.png)

Para habilitar esta opción en laboratorios existentes, seleccione **Configuration and policies (Directivas y configuración) > Redes virtuales**. Después, seleccione una red virtual de la lista y elija **HABILITAR IP PÚBLICA COMPARTIDA**  para la subred seleccionada. También puede deshabilitar esta opción en cualquier laboratorio si no desea compartir una dirección IP pública en máquinas virtuales de laboratorio.

Las máquinas virtuales creadas en este laboratorio usarán una dirección IP compartida de forma predeterminada.  Al crear la máquina virtual, esta configuración se puede observar en la **configuración avanzada** página **configuración de dirección IP**.

![Nueva máquina virtual](media/devtest-lab-shared-ip/new-vm.png)

- **Compartido:** Todas las máquinas virtuales creadas como **Shared** se colocan en un grupo de recursos (RG). Se asigna una sola dirección IP al grupo de recursos y todas las máquinas virtuales usarán esa dirección IP.
- **Pública:** Cada máquina virtual que crea tiene su propia dirección IP y se crea en su propio grupo de recursos.
- **Privada:** Cada máquina virtual que crea usa una dirección IP privada. No se puede conectar a esta máquina virtual directamente desde internet con Escritorio remoto.

Cuando una máquina virtual con dirección IP compartida habilitada se agregue a la subred, DevTest Labs agrega automáticamente la máquina virtual a un equilibrador de carga y asigna a un número de puerto TCP en la dirección IP pública, reenvía el puerto RDP en la máquina virtual.  

## <a name="using-the-shared-ip"></a>Uso de la dirección IP compartida

- **Usuarios de Linux:** SSH en la máquina virtual con la dirección IP o nombre de dominio completo, seguido de dos puntos, seguido por el puerto. Por ejemplo, en la imagen siguiente, la dirección RDP para conectarse a la máquina virtual es `mydevtestlab597975021002.eastus.cloudapp.azure.com:50661`.

  ![Ejemplo de máquina virtual](media/devtest-lab-shared-ip/vm-info.png)

- **Usuarios de Windows:** Seleccione el **Connect** botón en el portal de Azure para descargar un archivo RDP preconfigurado y acceder a la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

* [Definición de directivas de laboratorio en Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Configuración de una red virtual en Azure DevTest Labs](devtest-lab-configure-vnet.md)





