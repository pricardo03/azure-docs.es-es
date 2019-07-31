---
title: Use a Windows troubleshooting VM in the Azure portal (Uso de una máquina virtual Windows de solución de problemas en Azure Portal) | Microsoft Docs
description: Aprenda a solucionar problemas de la máquina virtual Windows en Azure mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure Portal.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/13/2018
ms.author: genli
ms.openlocfilehash: 8ab6fc75475cd99e3d803450476880175f12d2b6
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881136"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure Portal
Si la máquina virtual Windows de Azure se encuentra un error de disco o de arranque, deberá realizar los pasos para solucionar problemas en el propio disco duro virtual. Un ejemplo habitual sería una actualización de aplicación con error que impide que la máquina virtual se pueda arrancar correctamente. En este artículo se detalla cómo utilizar Azure Portal para conectar el disco duro virtual a otra máquina virtual Windows para solucionar los errores y, posteriormente, volver a crear la máquina virtual original. 

## <a name="recovery-process-overview"></a>Introducción al proceso de recuperación
El proceso de solución de problemas es el siguiente:

1. Elimine la máquina virtual que tiene problemas, conservando los discos duros virtuales.
2. Conecte y monte el disco duro virtual en otra máquina virtual Windows con el fin de solucionar problemas.
3. Conéctese a la máquina virtual de solución de problemas. Edite los archivos o ejecute cualquier herramienta necesaria para solucionar los problemas del disco duro virtual original.
4. Desmonte y desconecte el disco duro virtual de la máquina virtual de solución de problemas.
5. Cree una máquina virtual mediante el disco duro virtual original.

Para la máquina virtual que usa el disco administrado, ahora podemos usar Azure PowerShell para cambiar el disco del sistema operativo de una máquina virtual. No es necesario eliminar ni volver a crear la máquina virtual. Para más información, consulte [Solución de problemas de una máquina virtual Windows mediante la conexión del disco del sistema operativo a una máquina virtual de recuperación mediante Azure PowerShell](troubleshoot-recovery-disks-windows.md).

> [!NOTE]
> Este artículo no se aplica a la máquina virtual con el disco no administrado.

## <a name="determine-boot-issues"></a>Determinación de los problemas de arranque
Para determinar por qué la máquina virtual no es capaz de arrancar correctamente, examine la captura de pantalla de la máquina virtual de los diagnósticos de arranque. Un ejemplo habitual sería una actualización de aplicación con error o un disco duro virtual subyacente que se va a eliminar o mover.

Seleccione la máquina virtual en el portal y, a continuación, desplácese hacia abajo hasta la sección **Soporte técnico y solución de problemas**. Haga clic en **Diagnósticos de arranque** para ver la captura de pantalla. Tenga en cuenta todos los mensajes de error o códigos de error específicos para ayudar a determinar por qué la máquina virtual encuentra un problema.

![Visualización de los registros de consola de diagnóstico de arranque](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

También puede hacer clic en **Descargar captura de pantalla** para descargar una captura de la captura de pantalla de la máquina virtual.

## <a name="view-existing-virtual-hard-disk-details"></a>Visualización de los detalles del disco duro virtual existente
Antes de poder conectar el disco duro virtual a otra máquina virtual, debe identificar el nombre del disco duro virtual (VHD).

Seleccione la máquina virtual que tiene el problema y seleccione **Discos**. Registre el nombre del disco del sistema operativo como en el ejemplo siguiente:

![Selección de blobs de almacenamiento](./media/troubleshoot-recovery-disks-portal-windows/view-disk.png)

## <a name="delete-existing-vm"></a>Eliminación de la VM existente
Los discos duros virtuales y las máquinas virtuales son dos recursos diferentes de Azure. Un disco duro virtual es el recurso donde se almacenan el propio sistema operativo, las aplicaciones y las configuraciones. La propia máquina virtual consiste solo en metadatos que definen el tamaño o la ubicación y hace referencia a recursos como un disco duro virtual o una tarjeta de interfaz de red virtual (NIC). Cada disco duro virtual tiene una concesión que se asigna cuando se conecta a una máquina virtual. Aunque los discos de datos se pueden conectar y desconectar incluso mientras se está ejecutando la máquina virtual, no se puede desasociar el disco del sistema operativo, a menos que se elimine el recurso de máquina virtual. La concesión continúa para asociar el disco del sistema operativo a una máquina virtual incluso cuando esa máquina virtual está en un estado detenido o desasignado.

El primer paso para recuperar la máquina virtual es eliminar el propio recurso de máquina virtual. Al eliminar la máquina virtual, los discos duros virtuales se dejan en su cuenta de almacenamiento. Después de eliminar la máquina virtual, conecte el disco duro virtual a otra máquina virtual para localizar y solucionar los errores.

Seleccione la máquina virtual en el portal y, a continuación, haga clic en **Eliminar**:

![Captura de pantalla de diagnósticos de arranque de máquina virtual que muestran un error de arranque](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

Espere hasta que la máquina virtual haya terminado la eliminación antes de conectar el disco duro virtual a otra máquina virtual. La concesión en el disco duro virtual que lo asocia a la máquina virtual debe liberarse antes de poder conectar el disco duro virtual a otra máquina virtual.

## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Conexión del disco duro virtual existente a otra máquina virtual
Para los pasos siguientes, se usa otra máquina virtual con el fin de solucionar problemas. Conecte el disco duro virtual existente a esta máquina virtual de solución de problemas para examinar y modificar el contenido del disco. Por ejemplo, este proceso le permite corregir todos los errores de configuración o revisar archivos de registro adicionales de la aplicación o sistema. Elija o cree otra máquina virtual que se usará con fines de solución de problemas.

1. Seleccione el grupo de recursos desde el portal y, a continuación, seleccione la máquina virtual de solución de problemas. Seleccione **Discos**, elija **Editar** y haga clic en **Agregar disco de datos**:

    ![Conexión del disco existente en el portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. En la lista **Discos de datos**, seleccione el disco del sistema operativo de la máquina virtual que identificó. Si no ve el disco del sistema operativo, asegúrese de que la máquina virtual de solución de problemas y el disco del sistema operativo se encuentran en la misma región (ubicación).
3. Seleccione **Guardar** para aplicar los cambios.

## <a name="mount-the-attached-data-disk"></a>Montaje del disco de datos conectado

1. Abra una conexión a Escritorio remoto a la máquina virtual. Seleccione la máquina virtual en el portal y haga clic en **Conectar**. Descargue y abra el archivo de conexión RDP. Escriba sus credenciales para iniciar sesión en la máquina virtual como se indica a continuación:

    ![Inicie sesión en la máquina virtual utilizando el Escritorio remoto](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. Abra **Administrador del servidor** y, a continuación, seleccione **Servicios de archivos y almacenamiento**. 

    ![Seleccione Servicios de archivos y almacenamiento en Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. El disco de datos se detecta y conecta automáticamente. Para ver una lista de los discos conectados, seleccione **Discos**. Puede seleccionar el disco de datos para ver información de volumen, incluida la letra de unidad. En el ejemplo siguiente se muestra el disco de datos conectado a una máquina virtual y con **F:** :

    ![Disco conectado e información de volumen en Administrador del servidor](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Solución de problemas en el disco duro virtual original
Con el disco duro virtual existente montado, ahora puede realizar todos los pasos de mantenimiento y solución de problemas según sea necesario. Una vez que se han resuelto los problemas, continúe con los pasos siguientes.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Desmontaje y desconexión del disco duro virtual original
Una vez resueltos los errores, desconecte el disco duro virtual existente de la máquina virtual de solución de problemas. No se podrá usar el disco duro virtual en ninguna otra máquina virtual hasta que se libere la concesión que conecta el disco duro virtual a la máquina virtual de solución de problemas.

1. En la sesión RDP de la máquina virtual, abra **Administrador del servidor** y, a continuación, seleccione **Servicios de archivos y almacenamiento**:

    ![Seleccione Servicios de archivos y almacenamiento en Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Seleccione **Discos** y, a continuación, seleccione el disco de datos. Haga clic con el botón derecho en el disco de datos y seleccione **Desconectar**:

    ![Establezca el disco de datos como sin conexión en Administrador del servidor.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Ahora, desconecte el disco duro virtual de la máquina virtual. Seleccione la máquina virtual en Azure Portal y haga clic en **Discos**. 
4. Seleccione **Editar**, elija el disco del sistema operativo que ha conectado y haga clic en **Desasociar**:

    ![Desconecte el disco duro virtual existente](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Espere hasta que la máquina virtual haya desconectado correctamente el disco de datos antes de continuar.

## <a name="create-vm-from-original-hard-disk"></a>Creación de máquina virtual a partir del disco duro original

### <a name="method-1-use-azure-resource-manager-template"></a>Método 1: Usar una plantilla de Azure Resource Manager
Para crear una máquina virtual a partir del disco duro virtual original, utilice [esta plantilla de Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet). La plantilla implementa una máquina virtual en una red virtual existente o nueva mediante la dirección URL del disco duro virtual del comando anterior. Haga clic en el botón **Implementar en Azure** como se muestra a continuación:

![Implementación de máquina virtual desde una plantilla de GitHub](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

La plantilla se carga en Azure Portal para su implementación. Escriba los nombres de la nueva máquina virtual y los recursos de Azure existentes y pegue la dirección URL en el disco duro virtual existente. Para comenzar la implementación, haga clic en **Comprar**:

![Implementación de una máquina virtual a partir de una plantilla](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)

### <a name="method-2-create-a-vm-from-the-disk"></a>Método 2: Crear una máquina virtual desde el disco

1. En Azure Portal, seleccione el grupo de recursos en el portal y busque el disco del sistema operativo. También puede buscar el disco mediante el nombre del mismo:

    ![Búsqueda del disco desde Azure Portal](./media/troubleshoot-recovery-disks-portal-windows/search-disk.png)
1. Seleccione **Información general** y, luego, **Crear máquina virtual**.
    ![Creación de una máquina virtual a partir de un disco desde Azure Portal](./media/troubleshoot-recovery-disks-portal-windows/create-vm-from-disk.png)
1. Siga el asistente para crear la máquina virtual.

## <a name="re-enable-boot-diagnostics"></a>Rehabilitación de los diagnósticos de arranque
Cuando se crea la máquina virtual desde el disco duro virtual existente, puede que no se habilite automáticamente el diagnóstico de arranque. Para comprobar el estado del diagnóstico de arranque y activarlo si es necesario, seleccione la máquina virtual en el portal. En **Supervisión**, haga clic en **Configuración de diagnóstico**. Asegúrese de que el estado es **Activado**, y que la marca de verificación situada junto a **Diagnósticos de arranque** está seleccionada. Si realiza cambios, haga clic en **Guardar**:

![Actualización de la configuración de los diagnósticos de arranque](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Pasos siguientes
Si tiene problemas para conectarse a la máquina virtual, consulte [Solución de problemas de conexiones RDP a una máquina virtual de Azure](troubleshoot-rdp-connection.md). Para problemas con el acceso a aplicaciones que se ejecutan en su máquina virtual, consulte [Solucionar problemas de conectividad de aplicaciones en una máquina virtual Windows](troubleshoot-app-connection.md).

Para más información sobre el uso de Resource Manager, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

