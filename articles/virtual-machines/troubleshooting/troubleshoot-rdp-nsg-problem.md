---
title: No se puede conectar a VM de Azure porque el puerto RDP no está habilitado en NSG | Microsoft Docs
description: Obtenga información sobre cómo solucionar un problema por el que se produce un error de RDP debido a la configuración de NSG en Azure Portal | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: cb9058d4f68b2dc202edeeaa6cafb2eefa82470b
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284734"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>No puede conectarse de forma remota a una VM porque el puerto RDP no está habilitado en NSG

En este artículo se explica cómo resolver un problema por el que no puede conectarse a una máquina virtual (VM) Windows de Azure porque el puerto del protocolo de escritorio remoto (RDP) no está habilitado en el grupo de seguridad de red (NSG).


> [!NOTE] 
> Azure cuenta con dos modelos de implementación para crear recursos y trabajar con ellos: [Resource Manager y el modelo clásico](../../azure-resource-manager/resource-manager-deployment-model.md). Se recomienda usar el modelo de implementación de Resource Manager, en lugar del modelo de implementación clásica para las nuevas implementaciones. 

## <a name="symptom"></a>Síntoma

No puede establecer una conexión RDP a una VM en Azure porque el puerto RDP no está abierto en el grupo de seguridad de red.

## <a name="solution"></a>Solución 

Cuando se crea una VM, todo el tráfico de Internet se bloquea de forma predeterminada. 

Para habilitar el puerto RDP en un NSG, siga estos pasos:
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En **Máquinas virtuales**, seleccione la VM que tiene el problema. 
3. En **Configuración**, seleccione **Redes**. 
4. En **Reglas de puerto de entrada**, compruebe si el puerto de RDP se estableció correctamente. El siguiente es un ejemplo de la configuración: 

    **Prioridad**: 300 </br>
    **Puerto**: 3389 </br>
    **Nombre**: Port_3389 </br>
    **Puerto**: 3389 </br>
    **Protocolo**: TCP </br>
    **Origen**: Cualquiera </br>
    **Destinos**: Cualquiera </br>
    **Acción**: Permitir </br>

Si especifica la dirección IP de origen, esta opción permite el tráfico solo desde una dirección IP específica o intervalo de direcciones IP para conectarse a la VM. Asegúrese de que el equipo que use para iniciar la sesión de RDP esté dentro del intervalo.

Para más información sobre NSG, consulte [Grupos de seguridad de red](../../virtual-network/security-overview.md).

> [!NOTE]
> El puerto 3389 de RDP se expone a Internet. Por lo tanto, se recomienda usar este puerto solo para pruebas. Para entornos de producción, se recomienda usar una VPN o conexión privada.

## <a name="next-steps"></a>Pasos siguientes

Si el puerto RDP ya está habilitado en NSG, consulte [Solución de problemas de un error general de RDP en una máquina virtual Windows en Azure](./troubleshoot-rdp-general-error.md).



