---
title: Conmutación por error y conmutación por recuperación de máquinas virtuales de Hyper-V replicadas en Azure con Site Recovery | Microsoft Docs
description: Obtenga información sobre cómo conmutar por error máquinas virtuales de Hyper-V en Azure y conmutar por recuperación en el sitio local con Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 6a34187a87c6ecda461357a1c2fc8747ddf4b056
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294299"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Conmutación por error y conmutación por recuperación de máquinas virtuales de Hyper-V replicadas en Azure

En este tutorial se describe cómo conmutar por error una máquina virtual de Hyper-V en Azure. Después de que haya conmutado por error, conmute por recuperación a su sitio local cuando esté disponible. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Verificar las propiedades de máquinas virtuales de Hyper-V para comprobar que se cumplen los requisitos de Azure.
> * Ejecutar una conmutación por error en Azure.
> * Conmutar por recuperación de Azure a un entorno local
> * Realizar replicación inversa en las máquinas virtuales locales para volver a iniciar la replicación en Azure

Este es el quinto tutorial de su serie. En él se da por hecho que ya ha realizado las tareas de los tutoriales anteriores.    

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. [Preparación de un entorno de Hyper-V local](tutorial-prepare-on-premises-hyper-v.md)
3. Configuración de la recuperación ante desastres de [máquinas virtuales de Hyper-V](tutorial-hyper-v-to-azure.md) o de [máquinas virtuales de Hyper-V administradas en nubes VMM de System Center](tutorial-hyper-v-vmm-to-azure.md)
4. [Exploración de la recuperación ante desastres](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Preparación de la conmutación por error y la conmutación por recuperación

Asegúrese de que no hay instantáneas en la máquina virtual y de que la máquina virtual local se ha desactivado durante la conmutación por recuperación. Esto ayuda a garantizar la coherencia de datos durante la replicación. No encienda la máquina virtual local durante la conmutación por recuperación. 

La conmutación por error y la conmutación por recuperación constan de tres etapas:

1. **Conmutación por error a Azure**: conmutar por error máquinas virtuales de Hyper-V desde el sitio local a Azure.
2. **Conmutación por recuperación a un sitio local**: conmutar por error máquinas virtuales de Azure al sitio local, cuando esté disponible. Empieza a replicar las máquinas virtuales a las máquinas virtuales de Hyper-V locales. Después de la sincronización de datos inicial, conmute por error las máquinas virtuales de Azure al sitio local.  
3. **Replicación inversa en máquinas virtuales locales**: después de que los datos se hayan conmutado por recuperación, realice replicación inversa en las máquinas virtuales locales para que comiencen a replicarse en Azure.

## <a name="verify-vm-properties"></a>Comprobar las propiedades de la máquina virtual

Antes de la conmutación por error Compruebe las propiedades de la máquina virtual y asegúrese de que cumple los [requisitos de Azure](hyper-v-azure-support-matrix.md#replicated-vms).

1. En **Elementos protegidos**, haga clic en **Elementos replicados** > <nombre de la máquina virtual>.

2. En el panel **Elemento replicado**, puede revisar la información sobre la máquina virtual, el estado de mantenimiento y los puntos de recuperación disponibles más recientes. Haga clic en **Propiedades** para ver más detalles.
     - En **Proceso y red**, puede modificar tanto la configuración de la máquina virtual como la de la red, incluidas la red o subred en las que se va a ubicar la máquina virtual de Azure después de la conmutación por error y la dirección IP que se le va a asignar. No se admite la conmutación por recuperación de Managed Disks desde Azure a Hyper-V.
      - En **Discos** puede ver información sobre los discos de datos y el sistema operativo de la máquina virtual.

## <a name="failover-to-azure"></a>Conmutación por error a Azure

1. En **Configuración** > **Elementos replicados**, haga clic en VM > **Conmutación por error**.
2. En **Conmutación por error**, seleccione el punto de recuperación **más reciente**. 
3. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. Site Recovery intentará apagar las máquinas virtuales de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
4. Una vez verificada la conmutación por error, haga clic en **Confirmar**. Así se eliminan los puntos de recuperación disponibles.

> [!WARNING]
> **No cancele una conmutación por error en curso**: si cancela una conmutación por error en curso, esta se detiene, pero la máquina virtual no se volverá a replicar.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Conmutación por recuperación de una máquina virtual de Azure a un entorno local y replicación inversa de la máquina virtual local

Básicamente, la operación de conmutación por recuperación es una conmutación por error desde Azure hasta el sitio local y en la replicación inversa empieza de nuevo la replicación de las máquinas virtuales desde el sitio local hasta Azure.

1. En **Configuración** > **Elementos replicados**, haga clic en VM > **Conmutación por error planeada**.
2. En **Confirmar conmutación por error planeada**, compruebe la dirección de la conmutación por error (de Azure) y seleccione las ubicaciones de origen y de destino.
3. Seleccione **Sincronizar datos antes de la conmutación por error (sincronizar solo cambios diferenciales)**. Esta opción minimiza el tiempo de inactividad de la máquina virtual, ya que se sincroniza sin necesidad de apagarla.
4. Inicie la conmutación por error. Puede seguir el progreso de la conmutación por error en la pestaña **Trabajos** .
5. Una vez que la sincronización de datos inicial haya finalizado y esté listo para apagar las máquinas virtuales de Azure, haga clic en **Trabajos** > nombre del trabajo de conmutación por error planeado > **Completar conmutación por error**. La máquina virtual de Azure se apagará, se transferirán los cambios más recientes en el entorno local y se iniciará la máquina virtual local.
6. Inicie sesión en la máquina virtual local para comprobar que está disponible según lo previsto.
7. La máquina virtual local se encuentra ahora en un estado de **confirmación pendiente**. Haga clic en **Confirmar**. Así se eliminan las máquinas virtuales de Azure y sus discos, y se prepara la máquina virtual local para la replicación inversa.
Para iniciar la replicación de la máquina virtual local en Azure, habilite la **replicación inversa**. Esto desencadena la replicación de cambios diferenciales que se han producido desde que se apagó la máquina virtual de Azure.  
