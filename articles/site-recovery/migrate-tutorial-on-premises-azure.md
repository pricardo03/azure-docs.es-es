---
title: Migración de máquinas locales a Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo migrar máquinas locales a Azure mediante Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/12/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: bd41244192efa1333bc90bec8c00f38aaaa7f612
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714996"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migración de máquinas locales a Azure

Además de utilizar el servicio [Azure Site Recovery](site-recovery-overview.md) para administrar y coordinar la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure para garantizar la continuidad empresarial y la recuperación ante desastres (BCDR), también puede usar Site Recovery para administrar la migración de máquinas locales a Azure.


En este tutorial se muestra cómo migrar máquinas virtuales locales y servidores físicos a Azure. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Selección de un objetivo de replicación
> * Configure el entorno de origen y destino
> * Configurar una directiva de replicación
> * Habilitar replicación
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada
> * Ejecutar una conmutación por error única en Azure

Este es el tercer tutorial de una serie. En este tutorial se da por hecho que ya ha realizado las tareas de los tutoriales anteriores:

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. Prepare servidores [VMware](vmware-azure-tutorial-prepare-on-premises.md) o [Hyper-V] (hyper-v-prepare-on-premises-tutorial.md) locales.

Antes de empezar, le recomendamos que revise las arquitecturas del escenario de recuperación ante desastres de [VMware](vmware-azure-architecture.md) o [Hyper-V](hyper-v-azure-architecture.md).


## <a name="prerequisites"></a>Requisitos previos

No se admiten dispositivos que se hayan exportado con controladores paravirtualizados.


## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Haga clic en **Crear un recurso** > **Supervisión y administración** > **Backup y Site Recovery**.
3. En el apartado **Nombre**, especifique el nombre descriptivo **ContosoVMVault**. Si tiene más de una suscripción, seleccione la apropiada.
4. Cree un grupo de recursos denominado **ContosoRG**.
5. Especifique una región de Azure. Para comprobar las regiones admitidas, consulte la disponibilidad geográfica en [Detalles de precios de Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para acceder rápidamente al almacén desde el panel, haga clic en **Anclar al panel** y, después, en **Crear**.

   ![Almacén nuevo](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

El nuevo almacén se agrega al **Panel**, en **Todos los recursos**, y en la página principal **Almacenes de Recovery Services**.



## <a name="select-a-replication-goal"></a>Selección de un objetivo de replicación

Seleccione aquello que desea replicar y la ubicación donde se va a realizar la replicación.
1. Haga clic en **Almacenes de Recovery Services** > almacén.
2. En el menú de recursos, haga clic en **Site Recovery** > **Preparar la infraestructura** > **Objetivo de protección**.
3. En **Objetivo de protección**, seleccione el contenido que quiera migrar.
    - **VMware**: seleccione **To Azure (En Azure)** > **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor).
    - En **Máquina física**: seleccione **To Azure (En Azure)** > **No virtualizado/Otro**.
    - **Hyper-V**: seleccione **To Azure (En Azure)** > **Yes, with Hyper-V (Sí, con Hyper-V)**. Si VMM administra las máquinas virtuales de Hyper-V, seleccione **Sí**.


## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

- [Configure](vmware-azure-tutorial.md#set-up-the-source-environment) el entorno de origen para las máquinas virtuales de VMware.
- [Configure](physical-azure-disaster-recovery.md#set-up-the-source-environment) el entorno de origen para los servidores físicos.
- [Configure](hyper-v-azure-tutorial.md#set-up-the-source-environment) el entorno de origen para las máquinas virtuales de Hyper-V.

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

Seleccione y compruebe los recursos de destino.

1. Haga clic en **Preparar infraestructura** > **Destino** y seleccione la suscripción de Azure que quiere usar.
2. Especifique el modelo de implementación de Resource Manager.
3. Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles.

## <a name="set-up-a-replication-policy"></a>Configurar una directiva de replicación

- [Configure una directiva de replicación](vmware-azure-tutorial.md#create-a-replication-policy) para las máquinas virtuales de VMware.
- [Configure una directiva de replicación](physical-azure-disaster-recovery.md#create-a-replication-policy) para los servidores físicos.
- [Configure una directiva de replicación](hyper-v-azure-tutorial.md#set-up-a-replication-policy) para las máquinas virtuales de Hyper-VM.


## <a name="enable-replication"></a>Habilitar replicación

- [Habilite la replicación](vmware-azure-tutorial.md#enable-replication) para las máquinas virtuales de VMware.
- [Habilite la replicación](physical-azure-disaster-recovery.md#enable-replication) de los servidores físicos.
- Habilite la replicación de las máquinas virtuales de Hyper-V [con](hyper-v-vmm-azure-tutorial.md#enable-replication) o [sin VMM](hyper-v-azure-tutorial.md#enable-replication).


## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba

Ejecute una [conmutación por error de prueba](tutorial-dr-drill-azure.md) en Azure para asegurarse de que todo funciona de la forma esperada.


## <a name="migrate-to-azure"></a>Migración a Azure

Ejecute una conmutación por error para las máquinas que desea migrar.

1. En **Configuración** > **Elementos replicados**, haga clic en la máquina > **Conmutación por error**.
2. En **Conmutación por error**, seleccione un **punto de recuperación** en el que realizar la conmutación por error. Seleccione el punto de recuperación más reciente.
3. La configuración de la clave de cifrado no es importante para este escenario.
4. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. Site Recovery intentará apagar las máquinas virtuales antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
5. Compruebe que la máquina virtual de Azure aparece en Azure según lo previsto.
6. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Completar migración**. Con esta acción se completa el proceso de migración, y se detienen la replicación de la máquina virtual y la facturación de Site Recovery para la VM.

    ![Completar migración](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **No cancele una conmutación por error en curso**: antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual. Si se cancela una conmutación por error en curso, la conmutación por error se detiene, pero no se replica la máquina virtual de nuevo.

En algunos escenarios, la conmutación por error requiere un procesamiento adicional que tarda aproximadamente de ocho a diez minutos en completarse. Puede observar tiempos de conmutación por error de prueba más largos en los servidores físicos, las máquinas de VMware Linux, las máquinas virtuales de VMware que no tienen el servicio DHCP habilitado y las máquinas virtuales de VMware que no tienen los siguientes controladores de arranque: storvsc, vmbus, storflt, intelide y atapi.

## <a name="after-migration"></a>Después de la migración

Una vez migradas las máquinas a Azure, hay una serie de pasos que debe completar.

Algunos pasos se pueden automatizar como parte del proceso de migración con la funcionalidad de scripts de automatización integrada de los [planes de recuperación]( https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation).   


### <a name="post-migration-steps-in-azure"></a>Pasos posteriores a la migración a Azure

- Realice los ajustes de la aplicación posteriores a la migración, como actualizar las cadenas de conexión de la base de datos y las configuraciones del servidor web. 
- Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
- El [agente de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) administra la interacción de una máquina virtual con el controlador de tejido de Azure. Se requiere para algunos servicios de Azure, como Azure Backup, Site Recovery y Azure Security.
    - Si va a migrar servidores físicos y máquinas de VMware, el instalador de Mobility Service instala el agente de máquina virtual de Azure disponible en máquinas Windows. En máquinas virtuales Linux, se recomienda que instale al agente después de la conmutación por error. a
    - Si va a migrar máquinas virtuales de Azure en una región secundaria, se debe aprovisionar el agente de máquina virtual de Azure en la máquina virtual antes de la migración.
    - Si va a migrar máquinas virtuales de Hyper-V a Azure, instale al agente de máquina virtual de Azure en la máquina virtual de Azure después de la migración.
- Quite manualmente cualquier proveedor/agente de Site Recovery de la máquina virtual. Si migra servidores físicos o máquinas virtuales de VMware, [desinstale el servicio Mobility][vmware-azure-install-mobility-service.md#uninstall-mobility-service-on-a-windows-server-computer] de la máquina virtual de Azure.
- Para aumentar la resistencia:
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](azure-to-azure-quickstart.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso de tráfico de entrada con la [administración Just-In-Time]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) de Azure Security Center.
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implemente [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ) y visite [Azure Security Center](https://azure.microsoft.com/services/security-center/ ).
- Para supervisión y administración:
    - Considere la posibilidad de implementar [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) para supervisar el gasto y el uso de recursos.

### <a name="post-migration-steps-on-premises"></a>Pasos posteriores a la migración local

- Mueva el tráfico de la aplicación hasta la aplicación que se ejecuta en la instancia de máquina virtual de Azure migrada.
- Quite las máquinas virtuales locales del inventario de máquinas virtuales local.
- Quite las máquinas virtuales locales de las copias de seguridad locales.
- Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial se migran máquinas virtuales locales a máquinas virtuales de Azure. Ahora, puede [configurar la recuperación ante desastres](azure-to-azure-replicate-after-migration.md) para las máquinas virtuales de Azure en una región secundaria de Azure.

  
