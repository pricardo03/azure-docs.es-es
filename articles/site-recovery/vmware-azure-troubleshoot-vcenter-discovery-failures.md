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
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565198"
---
# <a name="troubleshoot-vcenter-discovery-failures"></a>Solución de problemas de detección de vCenter

Este artículo le ayuda a solucionar los problemas que se producen debido a errores de detección de VMware vCenter.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>Valores no numéricos en la propiedad maxSnapShots

En las versiones anteriores a la 9,20, vCenter se desconecta cuando recupera un valor no numérico para la propiedad `snapshot.maxSnapShots` en una máquina virtual.

Este problema se identifica mediante el identificador de error 95126.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Para resolver el problema:

- Identifique la máquina virtual y establezca el valor en un valor numérico (configuración de edición de la máquina virtual en vCenter).

o

- Actualice el servidor de configuración a la versión 9.20 o una versión posterior.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>Problemas de configuración de proxy para la conectividad de vCenter

La detección de vCenter respeta la configuración de proxy predeterminada del sistema que el usuario del sistema ha configurado. El servicio DRA respeta la configuración de proxy proporcionada por el usuario durante la instalación del servidor de configuración mediante el instalador de configuración unificado o la plantilla de OVA. 

En general, el proxy se usa para comunicarse con las redes públicas; por ejemplo para comunicarse con Azure. Si el proxy está configurado y vCenter está en un entorno local, no podrá comunicarse con DRA.

Cuando se encuentra este problema, se producen las situaciones siguientes:

- No se puede acceder a vCenter Server \<vCenter> debido al error: El servidor remoto devolvió un error: (503) Servidor no disponible
- No se puede acceder a vCenter Server \<vCenter> debido al error: El servidor remoto devolvió un error: No se puede conectar al servidor remoto.
- No se puede conectar al servidor vCenter/ESXi.

Para resolver el problema:

Descargue la [herramienta PsExec](https://aka.ms/PsExec). 

Use la herramienta de PsExec para acceder al contexto de usuario del sistema y determinar si la dirección del proxy está configurada. Después, puede agregar vCenter a la lista de omisión mediante los procedimientos siguientes.

Para la configuración de proxy de detección:

1. Abra IE en el contexto de usuario del sistema mediante la herramienta PsExec.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. Modifique la configuración del proxy en Internet Explorer para omitir la dirección IP de vCenter.
3. Reinicie el servicio tmanssvc.

Para la configuración de proxy DRA:

1. Abra un símbolo del sistema y abra la carpeta Microsoft Azure Site Recovery Provider.
 
    **cd C:\Archivos de programa\Microsoft Azure Site Recovery Provider**

3. En el símbolo del sistema, ejecute el siguiente comando.
   
   **DRCONFIGURATOR.EXE /configure /AddBypassUrls [dirección IP/FQDN de vCenter Server proporcionada en el momento de agregar vCenter]**

4. Reinicie el servicio de proveedor DRA.

## <a name="next-steps"></a>Pasos siguientes

[Administración del servidor de configuración para la recuperación ante desastres de la VM de VMware](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
