---
title: Implementación de hosts dedicados de Azure mediante Azure Portal
description: Implementación de máquinas virtuales en hosts dedicados mediante Azure Portal.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/26/2019
ms.author: cynthn
ms.openlocfilehash: aa19c343e003bf1cd55e3d12b18e595113a7189e
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75833949"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-portal"></a>implementación de máquinas virtuales en hosts dedicados mediante el portal

En este artículo se ofrecen instrucciones para crear un [host dedicado](dedicated-hosts.md) de Azure en el que se pueden hospedar máquinas virtuales (VM). 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>Crear una VM

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. En la página **Nuevo**, en **Popular**, seleccione **Windows Server 2016 Datacenter**.
1. En la pestaña **Aspectos básicos**, en **Detalles del proyecto**, asegúrese de que esté seleccionada la suscripción correcta y luego seleccione *myDedicatedHostsRG* como **grupo de recursos**. 
1. En **Detalles de instancia**, escriba *myVM* en **Nombre de máquina virtual** y elija *Este de EE. UU.* como **Ubicación**.
1. En **Opciones de disponibilidad**, seleccione **Zona de disponibilidad** y seleccione *1* en la lista desplegable.
1. En cuanto al tamaño, seleccione **Cambiar tamaño**. En la lista de tamaños disponibles, elija uno de la serie Esv3, como **Standard E2s v3**. Es posible que tenga que borrar el filtro para poder ver todos los tamaños disponibles.
1. En **Cuenta de administrador**, proporcione un nombre de usuario, como *azureuser*, y una contraseña. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](faq.md#what-are-the-password-requirements-when-creating-a-vm).
1. En **Reglas de puerto de entrada**, elija **Permitir los puertos seleccionados** y luego seleccione **RDP (3389)** en la lista desplegable.
1. En la parte superior de la página, seleccione la pestaña **Opciones avanzadas** y, en la sección **Host**, seleccione *myHostGroup* en **Grupo host** y *myHost* en **Host**. 
    ![Selección del grupo host y el host](./media/dedicated-hosts-portal/advanced.png)
1. Deje los valores predeterminados restantes y luego seleccione el botón **Revisar + crear** en la parte inferior de la página.
1. Cuando vea el mensaje en el que se indica que la validación se ha realizado correctamente, seleccione **Crear**.


## <a name="next-steps"></a>Pasos siguientes

- Para obtener más detalles, consulte la introducción a los [hosts dedicados](dedicated-hosts.md). 

- [Aquí](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) encontrará una plantilla de ejemplo en la que se usan zonas y dominios de error para obtener la máxima resistencia en una región.

- Asimismo, puede implementar un host dedicado mediante [Azure PowerShell](dedicated-hosts-powershell.md).