---
title: Administración de servidores de VMware vCenter en Azure Site Recovery
description: En este artículo se describe cómo agregar y administrar VMware vCenter para la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ramamill
ms.openlocfilehash: 8f339103f67f37d10999ef43fa57a6eb27b60f37
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74083974"
---
# <a name="manage-vmware-vcenter-server"></a>Administración de servidores VMware vCenter

En este artículo se describen las diversas operaciones de Site Recovery que se pueden realizar en un servidor VMware vCenter. Compruebe los [requisitos previos](vmware-physical-azure-support-matrix.md#replicated-machines) antes de empezar.


## <a name="set-up-an-account-for-automatic-discovery"></a>Configuración de una cuenta de detección automática

Site Recovery necesita acceso a VMware para que el servidor de procesos detecte automáticamente máquinas virtuales, así como para la conmutación por error y por recuperación de máquinas virtuales. Cree una cuenta de acceso de la manera siguiente:

1. Inicie sesión en la máquina del servidor de configuración.
2. Abra el archivo cspsconfigtool.exe mediante el acceso directo del escritorio.
3. En **Administrar cuenta**, haga clic en la pestaña **Agregar cuenta**.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
1. Proporcione los detalles de la cuenta y haga clic en **Aceptar** para agregarla.  La cuenta debe tener los privilegios que se resumen en la tabla siguiente. 

En unos 15 minutos la información de la cuenta se sincronizará con el servicio Site Recovery.

### <a name="account-permissions"></a>Permisos de la cuenta

|**Task** | **Cuenta** | **Permisos** | **Detalles**|
|--- | --- | --- | ---|
|**Detección y migración automáticas (sin conmutación por recuperación)** | Necesita al menos un usuario de solo lectura. | Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).|
|**Replicación y conmutación por error** | Necesita al menos un usuario de solo lectura.| Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).<br/><br/> Es útil para la migración, pero no para la replicación completa, la conmutación por error o la conmutación por recuperación.|
|**Replicación, conmutación por error y conmutación por recuperación** | Se recomienda crear un rol (AzureSiteRecoveryRole) con los permisos necesarios y, después, asignarlo a un grupo o usuario de VMware. | Objeto de centro de datos –> Propagar a objeto secundario, rol=AzureSiteRecoveryRole<br/><br/> Almacén de datos -> Asignar espacio, examinar almacén de datos, operaciones de archivo de bajo nivel, quitar archivo, actualizar archivos de máquina virtual<br/><br/> Red -> Asignación de red<br/><br/> Recursos-> Asignar máquina virtual al grupo de recursos, migrar apagado de máquina virtual, migrar máquinas virtuales encendidas<br/><br/> Tareas -> Crear tarea, actualizar tarea<br/><br/> Máquina virtual -> Configuración<br/><br/> Máquina virtual -> Interactuar -> Responder a pregunta, conexión de dispositivos, configurar soporte de CD, Configurar soporte de disquete, apagar, encender, instalación de herramientas de VMware<br/><br/> Máquina virtual -> Inventario -> Crear, registrar, anular registro<br/><br/> Máquina virtual -> Aprovisionamiento -> Permitir descarga de máquina virtual, permitir carga de archivos de máquina virtual<br/><br/> Máquina virtual -> Instantáneas -> Quitar instantáneas | El usuario se asigna en el nivel de centro de datos y tiene acceso a todos los objetos de este.<br/><br/> Para restringir el acceso, asigne el rol **Sin acceso** con **Propagate to child object** (Propagar a objeto secundario) a los objetos secundarios (hosts de vSphere, almacenes de datos, máquinas virtuales y redes).|


## <a name="add-vmware-server-to-the-vault"></a>Adición de un servidor VMware al almacén

1. En Azure Portal, abra el almacén > **Infraestructura de Site Recovery** > **Servidores de configuración** y abra el servidor de configuración.
2. En la página **Detalles**, haga clic en **+vCenter**.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials"></a>Modificación de credenciales

Modificación de las credenciales usadas para conectarse al servidor vCenter o host de ESXi de la siguiente manera:

1. Inicie sesión en el servidor de configuración y ejecute cspsconfigtool.exe desde el escritorio.
2. En **Administrar cuenta**, haga clic en la pestaña **Agregar cuenta**.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)
3. Proporcione los detalles de la nueva cuenta y haga clic en **Aceptar** para agregarla. La cuenta debe tener los privilegios enumerados [anteriormente](#account-permissions).
4. En Azure Portal, abra el almacén > **Infraestructura de Site Recovery** > **Servidores de configuración** y abra el servidor de configuración.
5. En la página **Detalles**, haga clic en **Actualizar servidor**.
6. Cuando finalice el trabajo de actualización del servidor, seleccione la instancia de vCenter Server para abrir la página **Resumen** de vCenter.
7. Seleccione la cuenta recién agregada en el campo **vCenter server/vSphere host account** (Cuenta de servidor vCenter/host de vSphere) y haga clic en **Guardar**.

   ![modify-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Eliminación de un servidor de vCenter

1. En Azure Portal, abra el almacén > **Infraestructura de Site Recovery** > **Servidores de configuración** y abra el servidor de configuración.
2. En la página **Detalles**, seleccione el servidor de vCenter.
3. Haga clic en el botón **Eliminar**.

   ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-vcenter-ip-address-and-port"></a>Modificación del puerto y la dirección IP de vCenter

1. Inicie sesión en el Portal de Azure.
2. Vaya a **Almacén de Recovery Services** > **Infraestructura de Site Recovery** > **Servidores de configuración**.
3. Haga clic en el servidor de configuración al que está asignado vCenter.
4. En la sección **Servidores vCenter**, haga clic en la instancia de vCenter que desea modificar.
5. En la página de resumen de vCenter, actualice la dirección IP y el puerto de la instancia de vCenter en los campos correspondientes y, a continuación, guarde sus cambios.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

6. Para que los cambios surtan efecto, espere 15 minutos o [actualice el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="migrate-all-protected-virtual-machines-to-a-new-vcenter"></a>Migración de todas las máquinas virtuales protegidas a una nueva instancia de vCenter

Para migrar todas las máquinas virtuales a la nueva instancia de vCenter, no agregue otra cuenta de vCenter. Esto puede dar lugar a entradas duplicadas. Solo tiene que actualizar las direcciones IP de la nueva instancia de vCenter:

1. Inicie sesión en el Portal de Azure.
2. Vaya a **Almacén de Recovery Services** > **Infraestructura de Site Recovery** > **Servidores de configuración**.
3. Haga clic en el servidor de configuración al que está asignada la instancia de vCenter antigua.
4. En la sección **Servidores vCenter**, haga clic en la instancia de vCenter desde la que tiene previsto migrar.
5. En la página de resumen de vCenter, actualice la dirección IP de la nueva instancia de vCenter en el campo **vCenter server/vSphere hostname or IP address** (Nombre de host o dirección IP de vCenter Server/vSphere). Guarde los cambios.

Tan pronto como se actualice la dirección IP, los componentes de Site Recovery empezarán a recibir información de detección de máquinas virtuales de la nueva instancia de vCenter. Esto no afectará a las actividades de replicación en curso.

## <a name="migrate-few-protected-virtual-machines-to-a-new-vcenter"></a>Migración de algunas máquinas virtuales protegidas a una nueva instancia de vCenter

> [!NOTE]
> Esta sección solo es aplicable al migrar algunas de sus máquinas virtuales protegidas a una nueva instancia de vCenter. Si desea proteger un nuevo conjunto de máquinas virtuales de una nueva instancia de vCenter, [agregue nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault) y empiece con **[Habilitar protección](vmware-azure-tutorial.md#enable-replication)** .

Para trasladar algunas máquinas virtuales a una nueva instancia de vCenter:

1. [Agregue los nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault).
2. [Deshabilite la replicación de las máquinas virtuales](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) que tiene previsto migrar.
3. Complete la migración de las máquinas virtuales seleccionadas a la nueva instancia de vCenter.
4. Ahora, proteja las máquinas virtuales migradas [seleccionando la nueva instancia de vCenter al habilitar la protección](vmware-azure-tutorial.md#enable-replication).

> [!TIP]
> Si el número de máquinas virtuales que se migran es **mayor** que el de máquinas virtuales retenidas en la instancia de vCenter antigua, actualice la dirección IP de la nueva instancia de vCenter mediante las instrucciones aquí especificadas. En el caso de las máquinas virtuales que se retienen en la instancia de vCenter antigua, [deshabilite la replicación](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure); [agregue nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault) e inicie **[Habilitar protección](vmware-azure-tutorial.md#enable-replication)** .

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

1. Si las máquinas virtuales protegidas se trasladan de un host ESXi a otro, ¿afectará a la replicación?

    No, esto no afectará a la replicación en curso. Sin embargo, [asegúrese de implementar el servidor de destino maestro con suficientes privilegios](vmware-azure-reprotect.md#deploy-a-separate-master-target-server).

2. ¿Cuáles son los números de puerto usados para la comunicación entre vCenter y otros componentes de Site Recovery?

    El puerto predeterminado es 443. El servidor de configuración tendrá acceso a la información de host de vCenter/vSphere a través de este puerto. Si desea actualizar esta información, haga clic [aquí](#modify-the-vcenter-ip-address-and-port).
