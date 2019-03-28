---
title: Solución de problemas de conmutación por recuperación al entorno local durante la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describen formas de solucionar los problemas de conmutación por recuperación y reprotección durante la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery.
author: vDonGlover
manager: JarrettRenshaw
ms.service: site-recovery
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: v-doglov
ms.openlocfilehash: c598c5e238458c010500579c5371622b85e71de0
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540812"
---
# <a name="troubleshoot-vcenter-discovery-failures"></a>Solución de problemas de detección de vCenter

En este artículo le ayuda a solucionar los problemas que se producen debido a errores de detección de vCenter de VMware.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>Valores no numéricos en la propiedad maxSnapShots

En las versiones anteriores 9,20, vCenter se desconecta cuando recupera un valor no numérico para la propiedad `snapshot.maxSnapShots` propiedad en una máquina virtual.

Este problema se identifica mediante el identificador de error 95126.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Para resolver el problema:

- Identifique la máquina virtual y establezca el valor en un valor numérico (configuración de edición de la máquina virtual en vCenter).

o

- Actualice el servidor de configuración a la versión 9,20 o posterior.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>Problemas de configuración de proxy para la conectividad de vCenter

vCenter detección respeta la configuración de proxy del sistema predeterminada configurada por el usuario del sistema. El servicio DRA respeta la configuración de proxy proporcionada por el usuario durante la instalación del servidor de configuración mediante el instalador unificado o la plantilla de OVA. 

En general, se usa el proxy para comunicarse con las redes públicas; Por ejemplo, la comunicación con Azure. Si el proxy está configurado y vCenter está en un entorno local, no podrá comunicarse con DRA.

Cuando se encuentra este problema, se producen las situaciones siguientes:

- El servidor vCenter \<vCenter > no es accesible debido al error: El servidor remoto devolvió un error: Servidor (503) no está disponible
- El servidor vCenter \<vCenter > no es accesible debido al error: El servidor remoto devolvió un error: No se puede conectar al servidor remoto.
- No se puede conectar al servidor vCenter/ESXi.

Para resolver el problema:

Descargue el [herramienta PsExec](https://aka.ms/PsExec). 

Usar la herramienta de PsExec para tener acceso al contexto de usuario del sistema y determinar si la dirección del proxy está configurada. A continuación, puede agregar vCenter a la lista de omisión mediante los procedimientos siguientes.

Para la configuración de proxy de detección:

1. IE abierto en el contexto de usuario del sistema mediante la herramienta de PsExec.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. Modificar la configuración de proxy en Internet Explorer para omitir la dirección IP de vCenter.
3. Reinicie el servicio tmanssvc.

Para la configuración de proxy DRA:

1. Abra un símbolo del sistema y abra la carpeta de proveedor de Microsoft Azure Site Recovery.
 
    **Proveedor de CD C:\Program Files\Microsoft Azure Site Recovery**

3. Desde el símbolo del sistema, ejecute el siguiente comando.
   
   **DRCONFIGURATOR. EXE / configure /AddBypassUrls [dirección IP o FQDN de vCenter Server proporcionada en el momento de agregar vCenter]**

4. Reinicie el servicio de proveedor DRA.

## <a name="next-steps"></a>Pasos siguientes

[Administrar el servidor de configuración para la recuperación ante desastres de VM de VMware](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
