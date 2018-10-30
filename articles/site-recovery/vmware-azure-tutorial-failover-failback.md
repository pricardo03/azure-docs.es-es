---
title: Conmutación por error y conmutación por recuperación de servidores físicos y máquinas virtuales de VMware replicados en Azure con Site Recovery | Microsoft Docs
description: Obtenga información sobre cómo conmutar por error servidores físicos y máquinas virtuales de VMware en Azure y conmutar por recuperación en el sitio local con Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/11/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 7e586e7e3ec8c16dcd215dbc11251d1b9fe928e1
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457097"
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Conmutación por error y conmutación por recuperación de servidores físicos y máquinas virtuales de VMware replicados en Azure

En este tutorial se describe cómo conmutar por error una máquina virtual de VMware en Azure. Después de que ha conmutado por error, conmutará por recuperación a su sitio local cuando esté disponible. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Comprobar las propiedades de máquinas virtuales de VMware para comprobar que se cumplen los requisitos de Azure.
> * Ejecutar una conmutación por error en Azure.
> * Crear un servidor de proceso y un servidor de destino maestro para la conmutación por recuperación.
> * Volver a proteger máquinas virtuales de Azure en el sitio local.
> * Realizar la conmutación por error de Azure en el entorno local.
> * Volver a proteger las máquinas virtuales locales para iniciar de nuevo la replicación en Azure

>[!NOTE]
>Los tutoriales están diseñados para mostrarle la ruta de implementación más sencilla para un escenario. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para más información acerca de los pasos de la conmutación por error de prueba, lea la [guía de procedimientos](site-recovery-failover.md).

Este es el quinto tutorial de una serie. En este tutorial se da por hecho que ya ha realizado las tareas de los tutoriales anteriores.

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. [Preparación de instancias locales de VMware](vmware-azure-tutorial-prepare-on-premises.md)
3. [Configuración de la recuperación ante desastres](vmware-azure-tutorial.md)
4. [Exploración de la recuperación ante desastres](tutorial-dr-drill-azure.md)
5. Además de realizar los pasos anteriores, es útil [revisar la arquitectura](vmware-azure-architecture.md) de este escenario de recuperación ante desastres.

## <a name="failover-and-failback"></a>Conmutación por error y conmutación por recuperación

La conmutación por error y la conmutación por recuperación constan de cuatro fases:

1. **Conmutación por error en Azure**: se conmutan por error las máquinas del sitio local a Azure.
2. **Reprotección de máquinas virtuales de Azure**: vuelva a proteger las máquinas virtuales de Azure de modo que comiencen a replicarse de nuevo en las máquinas virtuales locales de VMware. La máquina virtual local está apagada durante la reprotección. Esto ayuda a garantizar la coherencia de datos durante la replicación.
3. **Conmutación por error en el entorno local**: ejecute una conmutación por error para realizar una conmutación por recuperación desde Azure.
4. **Reprotección en máquinas virtuales locales**: después de que los datos se hayan conmutado por recuperación, vuelva a proteger las máquinas virtuales locales conmutadas por recuperación para que comiencen a replicarse en Azure.

## <a name="verify-vm-properties"></a>Comprobar las propiedades de la máquina virtual

Compruebe las propiedades de la máquina virtual y asegúrese de que la máquina virtual cumpla con los [requisitos de Azure](vmware-physical-azure-support-matrix.md#replicated-machines).

1. En **Elementos protegidos**, haga clic en **Elementos replicados** > VM.

2. En el panel **Elemento replicado**, puede ver un resumen de la información de la máquina virtual, el estado de mantenimiento y los puntos de recuperación disponibles más recientes. Haga clic en **Propiedades** para ver más detalles.

3. En **Proceso y red**, puede modificar el nombre de Azure, el grupo de recursos, el tamaño de destino, el [conjunto de disponibilidad](../virtual-machines/windows/tutorial-availability-sets.md) y la [configuración de discos administrados](#managed-disk-considerations)

4. Puede ver y modificar la configuración de red, incluida la red o subred en la que se va a ubicar la máquina virtual de Azure después de la conmutación por error y la dirección IP que se le va a asignar.

5. En **Discos** puede ver información sobre los discos de datos y el sistema operativo de la máquina virtual.

## <a name="run-a-failover-to-azure"></a>Ejecutar una conmutación por error en Azure.

1. En **Configuración** > **Elementos replicados**, haga clic en VM > **Conmutación por error**.

2. En **Conmutación por error**, seleccione un **Punto de recuperación** en el que realizar la conmutación por error. Puede seleccionar una de las siguientes opciones:
   - **Último**: esta opción procesa primero todos los datos enviados a Site Recovery. Ofrece el objetivo de punto de recuperación (RPO) mínimo, ya que la máquina virtual de Azure creada después de la conmutación por error tiene todos los datos replicados en Site Recovery al desencadenarse la conmutación por error.
   - **Procesado más recientemente**: con esta opción se realiza una conmutación por error de la máquina virtual al último punto de recuperación procesado por Site Recovery. Esta opción proporciona un objetivo de tiempo de recuperación (RTO) bajo, ya que no se invierte tiempo en el procesamiento de datos sin procesar.
   - **Más reciente coherente con la aplicación**: esta opción conmuta por error la máquina virtual al punto de recuperación más reciente coherente con la aplicación que haya procesado Site Recovery.
   - **Personalizado**: especifique un punto de recuperación.

3. Seleccione **Shut down machine before beginning failover** (Apagar la máquina antes de comenzar la conmutación por error) para intentar apagar las máquinas virtuales de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.

En algunos escenarios, la conmutación por error requiere un procesamiento adicional que tarda aproximadamente de ocho a diez minutos en completarse. Puede que observe **tiempos de conmutación por error de prueba mayores** para las máquinas virtuales VMware con el servicio de movilidad de la versión anterior a 9.8, servidores físicos, máquinas virtuales VMware Linux, máquinas virtuales Hyper-V protegidas como servidores físicos, máquinas virtuales VMware que no tienen habilitado el servicio DHCP y máquinas virtuales VMware que no tienen los siguientes controladores de arranque: storvsc, vmbus, storflt, intelide o atapi.

> [!WARNING]
> **No cancele una conmutación por error en curso**: antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual.
> Si se cancela una conmutación por error en curso, la conmutación por error se detiene, pero no se replica la máquina virtual de nuevo.

## <a name="connect-to-failed-over-virtual-machine-in-azure"></a>Conexión con una máquina virtual con conmutación por error en Azure

1. Si desea conectarse a máquinas virtuales de Azure mediante RDP/SSH después de la conmutación por error, siga los requisitos resumidos en [esta](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) tabla.
2. Después de la conmutación por error, vaya a la máquina virtual y [conéctese](../virtual-machines/windows/connect-logon.md) a ella para realizar la validación.
3. Después de la validación, haga clic en **Confirmar** para finalizar el punto de recuperación de la máquina virtual después de la conmutación por error. Tras la confirmación, todos los demás puntos de recuperación disponibles se eliminarán. De esta forma se completa la actividad de conmutación por error.

>[!TIP]
> **Cambiar el punto de recuperación** le ayuda a elegir un punto de recuperación diferente después de la conmutación por error, si no está satisfecho con la máquina virtual de conmutación por error. Después de la **confirmación**, esta opción ya no estará disponible.

Siga los pasos descritos [aquí](site-recovery-failover-to-azure-troubleshoot.md) para solucionar problemas de conectividad tras la conmutación por error.

## <a name="preparing-for-reprotection-of-azure-vm"></a>Preparación para volver a proteger la máquina virtual Azure

- Puede usar el servidor de procesos local (servidor de procesos integrado) que se instala automáticamente en el servidor de configuración como parte de la instalación **si tiene una conexión de Azure ExpressRoute**.

> [!IMPORTANT]
> Si tiene una conexión VPN entre el entorno local y Azure, debe configurar una máquina virtual de Azure como un servidor de procesos para la reprotección y la conmutación por recuperación. Para configurar un servidor de procesos en Azure, siga las instrucciones de [este artículo](vmware-azure-set-up-process-server-azure.md).

Para más información acerca de los requisitos previos para la reprotección y la conmutación por recuperación, consulte esta [sección] ](vmware-azure-reprotect.md##before-you-begin). 

### <a name="configure-the-master-target-server"></a>Configuración del servidor de destino maestro

El servidor de destino maestro recibe y controla los datos de replicación durante la conmutación por recuperación desde Azure. De forma predeterminada, está disponible en el servidor de configuración local. En este tutorial, vamos a usar el servidor de destino maestro predeterminado.

>[!NOTE]
>Proteger una máquina virtual basada en Linux requiere la creación de un servidor de destino maestro independiente. [Haga clic aquí](vmware-azure-install-linux-master-target.md) para más información.

Si la máquina virtual está en un **host ESXi administrado por un servidor vCenter**, el servidor de destino maestro debe tener acceso al almacén de datos de la máquina virtual (VMDK) para escribir los datos replicados en los discos de la máquina virtual. Asegúrese de que el almacén de datos de la máquina virtual esté montado en el host del destino maestro con acceso de lectura y escritura.

Si la máquina virtual está en un **ESXi que no está administrado por un servidor vCenter**, el servicio Site Recovery crea una nueva máquina virtual al volver a realizar la protección. Esta máquina virtual se crea en el host ESX en el que se crea el destino maestro.
El disco duro de la máquina virtual debe estar en un almacén de datos que sea accesible para el host en el que se esté ejecutando el servidor de destino maestro.

Si la máquina virtual **no usa vCenter**, debe completar la detección del host en el que se ejecute el servidor de destino maestro para poder volver a proteger la máquina. Lo mismo se aplica si realiza la conmutación por recuperación en servidores físicos. Otra opción (si existe la máquina virtual local) es eliminarla antes de realizar una conmutación por recuperación. En este caso, la conmutación por recuperación crea una nueva máquina virtual en el mismo host que el host ESX de destino maestro. Cuando se conmuta por recuperación a una ubicación alternativa, los datos se recuperan en el mismo almacén de datos y el mismo host ESX que los usados por el servidor de destino principal local.

No se puede usar Storage vMotion en el servidor de destino maestro. Si lo hace, la conmutación por recuperación no funcionará, ya que los discos no están disponibles para él. Excluya los servidores de destino maestros de la lista de vMotion.

>[!Warning]
>Si usa otro servidor de destino maestro para reproteger un grupo de replicación, el servidor no puede proporcionar un momento dado común.

## <a name="reprotect-azure-vms"></a>Reprotección de las máquinas virtuales de Azure

Volver a proteger la máquina virtual Azure lleva a la replicación de los datos en máquina virtual local. Esto es un paso obligatorio antes de realizar la conmutación por error desde Azure a la máquina virtual local. Siga las instrucciones dadas siguientes para volver a realizar la protección.

1. En **Configuración** >  **Elementos replicados**, haga clic con el botón derecho en la máquina virtual que se ha conmutado por error > **Volver a proteger**.
2. En **Reproteger**, compruebe que la opción **De Azure a local** esté seleccionada.
3. Especifique el servidor de destino maestro local y el servidor de procesos.
4. En **Almacén de datos**, seleccione el almacén de datos de destino maestro en el que quiera recuperar los discos en el entorno local. Si se ha eliminado la máquina virtual, se crean discos nuevos en este almacén de datos. Esta configuración se omite si los discos ya existen, pero debe especificar un valor.
5. Seleccione la unidad de retención de destino maestra. La directiva de conmutación por recuperación se selecciona automáticamente.
6. Haga clic en **Aceptar** para comenzar con la reprotección. Comienza un trabajo para replicar la máquina virtual desde Azure en el sitio local. Puede realizar el seguimiento del progreso en la pestaña **Trabajos**.
7. Una vez que el estado de la máquina virtual en **Elementos replicados** cambia a **Protegido**, la máquina está preparada para la conmutación por error en el entorno local.

> [!NOTE]
> La máquina virtual de Azure se puede recuperar en una máquina virtual local existente o en una ubicación alternativa. Lea [este artículo](concepts-types-of-failback.md) para más información.

## <a name="run-a-failover-from-azure-to-on-premises"></a>Ejecución de una conmutación por error desde Azure en el entorno local

Para volver a replicar en el entorno local, se usa directiva de conmutación por recuperación. Esta directiva se genera automáticamente cuando crea una directiva de replicación para la replicación en Azure:

- La directiva se asocia automáticamente al servidor de configuración.
- La directiva no se puede modificar.
- Los valores de la directiva son:
    - Umbral RPO = 15 minutos
    - Retención de punto de recuperación = 24 horas
    - Frecuencia de las instantáneas coherentes con la aplicación = 60 minutos

Ejecute la conmutación por error de la manera siguiente:

1. En la página **Elementos replicados**, haga clic con el botón derecho en la máquina y seleccione **Conmutación por error**.
2. En **Confirmar conmutación por error**, compruebe que la dirección de la conmutación por error sea desde Azure.
    ![dirección de conmutación por error](media/vmware-azure-tutorial-failover-failback/failover-direction.PNG)
3. Seleccione el punto de recuperación que desee usar para la conmutación por error. Un punto de recuperación coherente con la aplicación se produce antes del punto más reciente en el tiempo y provocará pérdida de datos.

    >[!WARNING]
    >Cuando se ejecuta la conmutación por error, Site Recovery apaga las máquinas virtuales de Azure y arranca la máquina virtual local. Habrá cierto tiempo de inactividad, por lo que debe elegir la hora adecuada.

4. Se puede realizar un seguimiento del progreso del trabajo en el **Almacén de Recovery Services** > **Supervisión e informes** > **Trabajos de Site Recovery**.
5. Tras finalizar la conmutación por error, haga clic con el botón derecho en la máquina virtual y haga clic en **Confirmar**. Esto desencadenará un trabajo en el que se quitarán las máquinas virtuales de Azure.
6. Compruebe que las máquinas virtuales de Azure se hayan apagado según lo previsto.

## <a name="reprotect-on-premises-machines-to-azure"></a>Reprotección de máquinas locales en Azure

Los datos ahora deben volver a estar en el sitio local, pero no se está replicando en Azure. Puede volver a iniciar la replicación en Azure de la siguiente forma:

1. En el almacén > **Elementos protegidos** >**Replicated Items**, seleccione la máquina virtual objeto de la recuperación virtual y haga clic en **Volver a proteger**.
2. Seleccione el servidor de procesos que se usa para enviar los datos replicados a Azure y haga clic en **Aceptar**.

Una vez completada la reprotección, la máquina virtual se vuelve a replicar en Azure y podrá ejecutar una conmutación por error según sea necesario.
