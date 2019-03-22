---
title: Requisitos del servidor de configuración para realizar la recuperación ante desastres de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describen el soporte y los requisitos al implementar el servidor de configuración para realizar la recuperación ante desastres de VMware en Azure con Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 4f13c1beb23687b46074671fe01a0f42b26c8c03
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312927"
---
# <a name="configuration-server-requirements-for-vmware-disaster-recovery-to-azure"></a>Requisitos del servidor de configuración para la recuperación ante desastres de VMware en Azure

Cuando se usa [Azure Site Recovery](site-recovery-overview.md) para realizar la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware en Azure, se implementa un servidor de configuración local.

- El servidor de configuración coordina la comunicación entre Azure y VMware local. También administra la replicación de datos.
- El servidor de configuración coordina la comunicación entre Azure y VMware local. También administra la replicación de datos.
- [Más información](vmware-azure-architecture.md) acerca de los componentes y procesos del servidor de configuración.

## <a name="configuration-server-deployment"></a>Implementación del servidor de configuración

Para la recuperación ante desastres de máquinas virtuales de VMware en Azure, implemente el servidor de configuración como una VM de VMware.

- Site Recovery proporciona una plantilla OVA que puede descargar desde Azure Portal y, después, importar en vCenter Server para configurar la máquina virtual del servidor de configuración.
- Al implementar el servidor de configuración con la plantilla OVA, la máquina virtual cumplirá automáticamente con los requisitos que se enumeran en este artículo.
- Se recomienda que use la plantilla OVA para configurar el servidor de configuración. Sin embargo, si está configurando la recuperación ante desastres para máquinas virtuales de VMware y no puede usar la plantilla OVA, puede implementar el servidor de configuración con [las instrucciones que se proporcionan](physical-azure-set-up-source.md).
- Si va a implementar el servidor de configuración para la recuperación ante desastres de máquinas físicas locales en Azure, siga las instrucciones que se incluyen [en este artículo](physical-azure-set-up-source.md). 


## <a name="hardware-requirements"></a>Requisitos de hardware

**Componente** | **Requisito** 
--- | ---
Núcleos de CPU | 8 
RAM | 16 GB
Número de discos | 3, incluidos el disco del sistema operativo, el disco de memoria caché del servidor de procesos, la unidad de retención para la conmutación por recuperación 
Espacio libre en el disco (caché del servidor de procesos) | 600 GB
Espacio libre en el disco (disco de retención) | 600 GB

## <a name="software-requirements"></a>Requisitos de software

**Componente** | **Requisito** 
--- | ---
Sistema operativo | Windows Server 2012 R2 <br> Windows Server 2016
Configuración regional del sistema operativo | Español (es-es)
Roles de Windows Server | No habilite estos roles: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Directivas de grupo | No habilite estas directivas de grupo: <br> - Impedir el acceso al símbolo del sistema. <br> - Impedir el acceso a herramientas de edición del Registro. <br> - Confiar en la lógica de datos adjuntos de archivos. <br> - Activar la ejecución de scripts. <br> [Más información](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Ningún sitio web predeterminado debe existir previamente <br> - Ningún sitio web o aplicación que escuche en el puerto 443 deben existir previamente <br>- Habilitar la [autenticación anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Habilitar la configuración de [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 

## <a name="network-requirements"></a>Requisitos de red

**Componente** | **Requisito** 
--- | --- 
Tipo de dirección IP | estática 
Acceso a Internet | El servidor necesita acceder a estas direcciones URL (directamente o mediante proxy): <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> - https:\//management.azure.com <br> - *.services.visualstudio.com <br> - time.nist.gov <br> - time.windows.com <br> OVF también necesita acceder a las siguientes direcciones URL: <br> - https:\//login.microsoftonline.com <br> - https:\//secure.aadcdn.microsoftonline-p.com <br> - https:\//login.live.com  <br> - https:\//auth.gfx.ms <br> - https:\//graph.windows.net <br> - https:\//login.windows.net <br> - https:\//www.live.com <br> - https:\//www.microsoft.com <br> - https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi 
Puertos | 443 (orquestación del canal de control)<br>9443 (Transporte de datos) 
Tipo de NIC | VMXNET3 (si el servidor de configuración es una máquina virtual de VMware)

## <a name="required-software"></a>Requisitos de software

**Componente** | **Requisito** 
--- | ---
VMware vSphere PowerCLI | Se debe instalar [PowerCLI versión 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) si el servidor de configuración se ejecuta en una máquina virtual de VMware.
MYSQL | Se debe instalar MySQL. Lo puede instalar manualmente o Site Recovery puede instalarlo.

## <a name="sizing-and-capacity-requirements"></a>Requisitos de capacidad y tamaño

En la tabla siguiente se resumen los requisitos de capacidad del servidor de configuración. Si va a replicar varias máquinas virtuales de VMware, consulte las [consideraciones de planeamiento de capacidad](site-recovery-plan-capacity-vmware.md) y ejecute la herramienta [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md) para la replicación de VMWare. 

**Componente** | **Requisito** 
--- | ---

| **CPU** | **Memoria** | **Disco de caché** | **Frecuencia de cambio de datos** | **Máquinas replicadas** |
| --- | --- | --- | --- | --- |
| 8 vCPU<br/><br/> 2 sockets * 4 núcleos \@ 2,5 GHz | 16 GB | < 300 GB | 500 GB o menos | Menos de 100 máquinas |
| 12 vCPU<br/><br/> 2 socks * 6 núcleos a 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | De 100 a 150 máquinas |
| 16 vCPU<br/><br/> 2 socks * 8 núcleos \@ 2,5 GHz | 32 GB | 1 TB | 1 - 2 TB | De 150 a 200 máquinas | 



## <a name="next-steps"></a>Pasos siguientes
Configure la recuperación ante desastres en Azure para [máquinas virtuales VMware](vmware-azure-tutorial.md).
