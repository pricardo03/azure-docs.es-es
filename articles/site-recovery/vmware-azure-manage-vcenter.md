---
title: Administración de servidores VMware vCenter para la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo agregar y administrar VMware vCenter para la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ramamill
ms.openlocfilehash: 9694c682f171ab715812b05fed2064c9bbcd36b3
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518089"
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

## <a name="modify-the-vcenter-ip-address-and-port"></a>Modificar la dirección IP de vCenter y el puerto

1. Inicie sesión en el Portal de Azure.
2. Vaya a **almacén de Recovery Services** > **infraestructura de Site Recovery** > **servidores de configuración**.
3. Haga clic en el servidor de configuración vCenter está asignado a.
4. En el **servidores vCenter** sección, haga clic en el servidor de vCenter que desea modificar.
5. En la página de resumen de vCenter, actualice la dirección IP y puerto de vCenter en los campos correspondientes y, a continuación, guarde los cambios.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

6. Para que los cambios sean efectivos, espere entre 15 minutos o [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="migrate-all-protected-virtual-machines-to-a-new-vcenter"></a>Migrar todas las máquinas virtuales a un nuevo vCenter

Para migrar todas las máquinas virtuales para el nuevo vCenter, no agregue otra cuenta de vCenter. Esto puede conducir a las entradas duplicadas. Simplemente actualice la dirección IP del nuevo vCenter:

1. Inicie sesión en el Portal de Azure.
2. Vaya a **almacén de Recovery Services** > **infraestructura de Site Recovery** > **servidores de configuración**.
3. Haga clic en el servidor de configuración antiguo vCenter está asignado a.
4. En el **servidores vCenter** sección, haga clic en el que va a migrar desde vCenter.
5. En la página de resumen de vCenter, actualice la dirección IP del nuevo vCenter en el campo **vCenter server/vSphere host o dirección IP**. Guarde los cambios.

En cuanto se actualiza la dirección IP, los componentes de Site Recovery comenzará a recibir información de detección de máquinas virtuales desde el nuevo vCenter. Esto no afectará a las actividades de replicación en curso.

## <a name="migrate-few-protected-virtual-machines-to-a-new-vcenter"></a>Migrar algunas máquinas virtuales protegidas a un nuevo vCenter

> [!NOTE]
> En esta sección es aplicable solo si va a migrar algunas de sus máquinas virtuales protegidas a un nuevo vCenter. Si desea proteger un nuevo conjunto de máquinas virtuales de un nuevo vCenter, [agregar nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault) y comenzar con  **[habilitar la protección](vmware-azure-tutorial.md#enable-replication)**.

Para mover algunas máquinas virtuales a un nuevo vCenter:

1. [Agregar los nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault).
2. [Deshabilite la replicación de las máquinas virtuales](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) va a migrar.
3. Completar la migración de máquinas virtuales seleccionadas para el nuevo vCenter.
4. Ahora, proteger las máquinas virtuales migradas por [seleccionando el nuevo vCenter al habilitar la protección](vmware-azure-tutorial.md#enable-replication).

> [!TIP]
> Si el número de máquinas virtuales que se está migrando es **mayor** que el número de máquinas virtuales se conservan en el antiguo servidor de vCenter, actualice la dirección IP del nuevo vCenter mediante las instrucciones proporcionadas aquí. Para las máquinas virtuales de algunas que se conservan en el antiguo servidor de vCenter, [Deshabilitar replicación](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure); [agregar nuevos detalles de vCenter al servidor de configuración](#add-vmware-server-to-the-vault)e iniciar  **[habilitar la protección](vmware-azure-tutorial.md#enable-replication)**.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

1. ¿Si las máquinas virtuales protegidas se mueven desde un host ESXi a otro, afectará la replicación?

    No, esto no afectará a la replicación en curso. Sin embargo, [Asegúrate de implementar el servidor de destino maestro con suficientes privilegios](vmware-azure-reprotect.md#deploy-a-separate-master-target-server)

2. ¿Cuáles son los números de puerto usado para comunicación entre otros Site Recovery y vCenter componentes?

    El puerto predeterminado es 443. Servidor de configuración accederán a información de host vCenter/vSphere mediante este puerto. Si desea actualizar esta información, haga clic en [aquí](#modify-the-vcenter-ip-address-and-port).
