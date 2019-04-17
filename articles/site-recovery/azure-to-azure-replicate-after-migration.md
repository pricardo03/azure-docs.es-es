---
title: Configuración de la recuperación ante desastres de máquinas virtuales de Azure después de migrar a Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo preparar máquinas para configurar la recuperación ante desastres entre regiones de Azure después de la migración a Azure mediante Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 04/16/2019
ms.author: raynew
ms.openlocfilehash: 019c6ec776277a9102cb95cd685bbae0fc660d66
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59615921"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Configuración de la recuperación ante desastres de máquinas virtuales de Azure tras la migración a Azure 


Siga este artículo si ha [migrar máquinas locales a máquinas virtuales de Azure](tutorial-migrate-on-premises-to-azure.md) utilizando el [Site Recovery](site-recovery-overview.md) service y ahora desea obtener las máquinas virtuales configurada para la recuperación ante desastres en una región secundaria de Azure. El artículo describe cómo asegurarse de que está instalado el agente de máquina virtual de Azure en las máquinas virtuales migradas y cómo quitar el servicio Site Recovery Mobility que ya no se necesita después de la migración.



## <a name="verify-migration"></a>Comprobar la migración

Antes de configurar la recuperación ante desastres, asegúrese de que la migración se haya completado según lo previsto. Para completar una migración correctamente después de una conmutación por error, debe seleccionar la opción **Completar la migración** en cada máquina que desea migrar. 

## <a name="verify-the-azure-vm-agent"></a>Compruebe que el agente de máquina virtual de Azure

Cada máquina virtual de Azure debe tener la [agente de máquina virtual de Azure](../virtual-machines/extensions/agent-windows.md) instalado. Para replicar máquinas virtuales de Azure, Site Recovery instala una extensión del agente.

- Si el equipo está ejecutando la versión 9.7.0.0 o posterior del servicio Site Recovery Mobility, el agente de máquina virtual de Azure se instala automáticamente el servicio de movilidad en máquinas virtuales de Windows. En versiones anteriores de Mobility service, deberá instalar al agente de forma automática.
- Las máquinas virtuales de Linux, debe instalar manualmente el agente de máquina virtual de Azure. Deberá instalar el agente de máquina virtual de Azure si el servicio de movilidad instalado en la máquina migrada es 9.6 o anterior.


### <a name="install-the-agent-on-windows-vms"></a>Instalar al agente en máquinas virtuales de Windows

Si está ejecutando una versión de Site Recovery mobility service anterior a 9.7.0.0 o tiene alguna necesidad instalar al agente manualmente, haga lo siguiente:  

1. Asegúrese de que tiene permisos de administrador en la máquina virtual.
2. Descargue el [instalador del agente de máquina virtual](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
3. Ejecute el archivo de instalador.

#### <a name="validate-the-installation"></a>Validación de la instalación
Para comprobar que el agente está instalado:

1. En la máquina virtual de Azure, en la carpeta C:\WindowsAzure\Package, debería ver el archivo WaAppAgent.exe.
2. Haga clic con el botón derecho en el archivo y, en **Propiedades**, seleccione la pestaña **Detalles**.
3. Verifique que en el campo **Versión del producto** aparece el valor 2.6.1198.718 o una versión posterior.

[Obtenga más información](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) acerca de la instalación del agente para Windows.

### <a name="install-the-agent-on-linux-vms"></a>Instalar al agente en máquinas virtuales Linux

Instalar el [VM Linux de Azure](../virtual-machines/extensions/agent-linux.md) agente manualmente como sigue:

1. Asegúrese de que tiene permisos de administrador en el equipo.
2. Recomendamos encarecidamente que instale al agente de VM de Linux mediante un paquete de DEB o RPM del repositorio de paquetes de su distribución. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios.
    - Se recomienda encarecidamente que actualice al agente solo a través de un repositorio de distribución.
    - No se recomienda instalar al agente de VM de Linux directamente desde GitHub y actualizarlo.
    -  Si el último agente no está disponible para su distribución, póngase en contacto con el soporte técnico de distribución para obtener instrucciones sobre cómo instalarlo. 

#### <a name="validate-the-installation"></a>Validación de la instalación 

1. Ejecute este comando: **ps -e** para asegurarse de que se está ejecutando el agente de Azure en la VM de Linux.
2. Si el proceso no se está ejecutando, reinícielo mediante los siguientes comandos:
    - Para Ubuntu: **walinuxagent inicio del servicio**
    - Para otras distribuciones: **service waagent start**


## <a name="uninstall-the-mobility-service"></a>Desinstalación de Mobility Service

1. Desinstalar manualmente el servicio de movilidad de la máquina virtual de Azure, mediante uno de los métodos siguientes. 
    - En Windows, en Panel de control > **Agregar o quitar programas**, desinstale **Microsoft Azure Site Recovery Mobility Service/Servidor de destino maestro**. En un símbolo del sistema con privilegios elevados, ejecute:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - En Linux, inicie sesión como un usuario raíz. En un terminal, vaya a **/user/local/ASR** y ejecute el siguiente comando:
        ```
        ./uninstall.sh -Y
        ```
2. Reinicie la máquina virtual antes de configurar la replicación.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de revisión](site-recovery-extension-troubleshoot.md) para la extensión de Site Recovery en el agente de máquina virtual de Azure.
[Replicación rápida](azure-to-azure-quickstart.md) de una máquina virtual de Azure a una región secundaria.
