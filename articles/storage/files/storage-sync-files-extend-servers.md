---
title: 'Tutorial: Ampliación de servidores de archivos de Windows con Azure File Sync | Microsoft Docs'
description: Aprenda a ampliar servidores de archivos de Windows con Azure File Sync, desde el principio del proceso al final.
services: storage
author: wmgries
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 3ebf450f4e84fed572307a18f20f36013e32c7a5
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630706"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Tutorial: Extensión de servidores de archivos de Windows con Azure File Sync
En este tutorial, mostraremos los pasos básicos para ampliar la capacidad de almacenamiento de un servidor con Windows Server mediante Azure File Sync. Aunque para el tutorial se va a usar una máquina virtual de Azure con Windows Server, lo habitual es realizar este proceso para servidores locales. Si está listo para implementar Azure File Sync en su propio entorno, vaya al artículo [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md).

> [!div class="checklist"]
> * Implementación del servicio de sincronización de almacenamiento
> * Preparación de Windows Server para su uso con Azure File Sync
> * Instalación del agente de Azure File Sync
> * Registro de un servidor de Windows Server con el servicio de sincronización de almacenamiento
> * Creación de un grupo de sincronización y un punto de conexión en la nube
> * Creación de un punto de conexión de servidor

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="prepare-your-environment"></a>Preparación del entorno
Antes de implementar Azure File Sync, en este tutorial hay que configurar algunos elementos. Además de crear una cuenta de Azure Storage y un recurso compartido de archivos, creará una máquina virtual de Windows Server 2016 Datacenter y preparará dicho servidor para Azure File Sync.

### <a name="create-a-folder-and-txt-file"></a>Creación de una carpeta y un archivo .txt

En el equipo local, cree una carpeta nueva denominada *FilesToSync* y agregue un archivo de texto denominado *mytestdoc.txt*. En este tutorial cargará dicho archivo en el recurso compartido de archivos.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos
A continuación creará un recurso compartido de archivos.

1. Cuando se complete la implementación de la cuenta de Azure Storage, haga clic en **Ir al recurso**.
1. Haga clic en **Archivos** en el panel de la cuenta de almacenamiento.

    ![Haga clic en Archivos](./media/storage-sync-files-extend-servers/click-files.png)

1. Haga clic en **+Recurso compartido de archivos**.

    ![Haga clic en el botón Agregar recurso compartido de archivos](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Asigne al nuevo recurso compartido de archivos el nombre *afsfileshare*, escriba "1" en **Cuota** y, después, haga clic en **Crear**. El valor máximo de la cuota es 5 TiB, pero para este tutorial solo se necesita 1 GB.

    ![Especificar el nombre y la cuota del nuevo recurso compartido de archivos](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Seleccione el nuevo recurso compartido de archivos y en su ubicación, haga clic en **Cargar**.

    ![Cargar un archivo](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Vaya a la carpeta *FilesToSync* en la que creó el archivo .txt, seleccione *mytestdoc.txt* y haga clic en **Cargar**.

    ![Examinar recurso compartido de archivos](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Ya ha creado una cuenta de Azure Storage y un recurso compartido de archivos con un solo archivo en Azure. Ahora creará la máquina virtual de Azure con Windows Server 2016 Datacenter para representar el servidor local en este tutorial.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Implementación de una máquina virtual y conexión de un disco de datos

1. A continuación, expanda el menú del lado izquierdo del portal y elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
1. En el cuadro de búsqueda que está encima de la lista de recursos de **Azure Marketplace**, busque **Windows Server 2016 Datacenter**, selecciónelo y, después, elija **Crear**.
1. En la pestaña **Conceptos básicos**, en **Detalles del proyecto**, seleccione el grupo de recursos que creó para este tutorial.

   ![Especificación de la información básica de la máquina virtual en la hoja del Portal](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. En **Detalles de instancia**, especifique un nombre de máquina virtual, como *myVM*.
1. Deje la configuración predeterminada en **Región**, **Opciones de disponibilidad**, **Imagen** y **Tamaño**.
1. En **Cuenta de administrador**, especifique el **nombre de usuario** y la **contraseña** de la máquina virtual.
1. En **Reglas de puerto de entrada**, elija **Permitir los puertos seleccionados** y luego seleccione **RDP (3389)** y **HTTP** en la lista desplegable.

   Antes de crear la máquina virtual, es preciso crear un disco de datos.

1. Haga clic en **Siguiente: Discos**

   ![Adición de discos de datos](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. En la pestaña **Discos**, en **Opciones de disco**, deje los valores predeterminados.
1. En **DISCOS DE DATOS**, haga clic en **Crear y adjuntar un nuevo disco**.

1. Para este tutorial, deje los valores predeterminados, salvo que debe cambiar el valor de **Tamaño (GiB)** a **1 GB**.

   ![Detalles del disco de datos](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Haga clic en **OK**.
1. Haga clic en **Revisar + crear**.
1. Haga clic en **Create**(Crear).

   Puede hacer clic en el icono **Notificaciones** para ver el **progreso de la implementación**. La creación de una máquina virtual nueva tardará varios minutos en completarse.

1. Una vez que se complete la implementación de la máquina virtual, haga clic en **Ir al recurso**.

   ![Ir al recurso](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Ya ha creado una nueva máquina virtual y ha conectado un disco de datos. Ahora tiene que conectarse a la máquina virtual.

### <a name="connect-to-your-vm"></a>Conexión a la máquina virtual

1. En Azure Portal, haga clic en **Conectar** en la página de propiedades de la máquina virtual.

   ![Conexión a una máquina virtual de Azure desde el portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. En la página **Conectarse a una máquina virtual**, mantenga las opciones predeterminadas para conectarse por **dirección IP** a través del puerto 3389 y haga clic **en Descargar archivo RDP**.

   ![Descarga del archivo RDP](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Abra el archivo RDP que descargó y haga clic en **Conectar** cuando se le solicite.
1. En la ventana **Seguridad de Windows**, seleccione **Más opciones** y, después, **Usar otra cuenta**. Escriba el nombre de usuario con el siguiente formato *localhost\nombre de usuario*, escriba la contraseña que creó para la máquina virtual y haga clic en **Aceptar**.

   ![Más opciones](./media/storage-sync-files-extend-servers/local-host2.png)

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Haga clic en **Sí** o en **Continuar** para crear la conexión.

### <a name="prepare-the-windows-server"></a>Preparación del servidor de Windows Server
En el caso del servidor de **Windows Server 2016 Datacenter**, deshabilite **Configuración de seguridad mejorada de Internet Explorer**. Este paso solo es necesario para el registro inicial del servidor. Se puede volver a habilitar una vez registrado el servidor.

En la máquina virtual de **Windows Server 2016 Datacenter**, el **Administrador del servidor** se abrirá automáticamente.  Si el **Administrador del servidor** no se abre de forma predeterminada, búsquelo en el Explorador.

1. En el **Administrador del servidor**, haga clic en **Servidor local**.

   !["Servidor local" a la izquierda de la interfaz de usuario Administrador del servidor](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. En el panel secundario **Propiedades**, seleccione el vínculo de **Configuración de seguridad mejorada de IE**.  

    ![El panel "Configuración de seguridad mejorada de IE" en la interfaz de usuario del Administrador del servidor](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. En el cuadro de diálogo **Configuración de seguridad mejorada de Internet Explorer**, seleccione **Desactivado** en **Administradores** y **Usuarios**.

    ![Ventana emergente Configuración de seguridad mejorada de Internet Explorer con "Desactivado" seleccionado](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Ya puede agregar el disco de datos a la máquina virtual.

### <a name="add-the-data-disk"></a>Incorporación del disco de datos

1. En la máquina virtual de **Windows Server 2016 Datacenter**, haga clic en **Files and storage services** > **Volumes** > **Disks** (Archivos y servicios de almacenamiento > Volúmenes > Discos).

    ![Disco de datos](media/storage-sync-files-extend-servers/your-disk.png)

1. Haga clic con el botón derecho en el disco de 1 GB llamado **Msft Virtual Disk** y haga clic en **New volume** (Volumen nuevo).
1. Complete el asistente dejando los valores predeterminados, anote la letra de la unidad asignada y haga clic en **Crear**.
1. Haga clic en **Cerrar**.

   Ya está el disco en línea y ha creado un volumen. Para confirmar que el disco de datos se ha agregado correctamente, abra el explorador en la máquina virtual y confirme que la nueva unidad está presente.

1. En el explorador de la máquina virtual, expanda **Este PC** y haga doble clic en la nueva unidad. En este ejemplo es la unidad F:.
1. Haga clic con el botón derecho y seleccione **Nueva** > **carpeta**. Asigne a la carpeta el nombre *FilesToSync*.
1. Haga doble clic en la carpeta **FilesToSync**.
1. Haga clic con el botón derecho y seleccione **Nuevo** > **Documento de texto**. Asigne el archivo de texto el nombre *MyTestFile*.

    ![Agregar un archivo de texto nuevo](media/storage-sync-files-extend-servers/new-file.png)

1. Cierre el **Explorador** y el **Administrador del servidor**.

### <a name="download-the-azure-powershell-module"></a>Descarga del módulo Azure PowerShell
A continuación, en la máquina virtual con **Windows Server 2016 Datacenter**, instale el **módulo Azure PowerShell** en el servidor.

1. En la máquina virtual, abra una ventana de PowerShell con privilegios elevados
1. Ejecute el siguiente comando:

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

   > [!NOTE]
   > Si tiene una versión de NuGet anterior a la 2.8.5.201, se le pedirá que descargue e instale la versión más reciente de NuGet.

   De forma predeterminada, la Galería de PowerShell no está configurada como un repositorio de confianza para PowerShellGet. La primera vez que use PSGallery verá el siguiente mensaje:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Responda `Yes` o `Yes to All` para continuar con la instalación.

El módulo `Az` es un módulo acumulativo para los cmdlets de Azure PowerShell. Al instalarlo, se descargan todos los módulos disponibles de Azure Resource Manager y hace que sus cmdlets estén disponibles para su uso.

Ya ha terminado de configurar el entorno para el tutorial y está listo para empezar a implementar el **servicio de sincronización de almacenamiento**.

## <a name="deploy-the-service"></a>Implementación del servicio 
La implementación de Azure File Sync comienza por situar un recurso del **servicio de sincronización de almacenamiento** en un grupo de recursos de la suscripción seleccionada. El servicio de sincronización de almacenamiento hereda los permisos de acceso de la suscripción y el grupo de recursos en los que lo ha implementado.

1. En Azure Portal, haga clic en **Crear un recurso** y busque **Azure File Sync**.
1. En los resultados de la búsqueda, haga clic en **Azure File Sync**.
1. Seleccione **Crear** para abrir la pestaña **Implementar la sincronización del almacenamiento**.

   ![Implementar la sincronización del almacenamiento](media/storage-sync-files-extend-servers/afs-info.png)

   En el panel que se abre, escriba la siguiente información:

   | Valor | DESCRIPCIÓN |
   | ----- | ----- |
   | **Nombre** | Un nombre único (por suscripción) para el servicio de sincronización de almacenamiento.<br><br>En este tutorial, se usa *afssyncservice02*. |
   | **Suscripción** | La suscripción que utiliza para este tutorial. |
   | **Grupos de recursos** | El grupo de recursos que utiliza para este tutorial.<br><br>En este tutorial, se usa *afsresgroup101918*. |
   | **Ubicación** | Este de EE. UU |

1. Cuando haya terminado, seleccione **Crear** para implementar el **servicio de sincronización de almacenamiento**.
1. Haga clic en la pestaña **Notificaciones** > **Ir al recurso**.

## <a name="install-the-agent"></a>Instalación del agente
El agente de Azure File Sync es un paquete descargable que permite la sincronización de Windows Server con un recurso compartido de archivos de Azure.

1. Vuelva a la máquina virtual con **Windows Server 2016 Datacenter** y abra **Internet Explorer**.
1. Vaya al [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?linkid=858257). Desplácese hacia abajo hasta la sección **agente de Azure File Sync** y haga clic en **Descargar**.

   ![Descarga del agente de File Sync](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Active la casilla de **StorageSyncAgent_V3_WS2016. EXE** y haga clic en **Siguiente**.

   ![Seleccionar agente](media/storage-sync-files-extend-servers/select-agent.png)

1. **Permitir una vez** > **Ejecutar** > **Abrir** el archivo.
1. Si no lo ha hecho, cierre la ventana de PowerShell.
1. Acepte los valores predeterminados en el **Asistente para instalación de agente de sincronización de almacenamiento**.
1. Haga clic en **Instalar**.
1. Haga clic en **Finalizar**

Ha implementado el servicio de sincronización de Azure e instalado el agente en la máquina virtual con **Windows Server 2016 Datacenter**. Ahora tiene que registrar la máquina virtual en el **servicio de sincronización de almacenamiento**.

## <a name="register-windows-server"></a>Registro de Windows Server
El registro del servidor Windows Server con un servicio de sincronización de almacenamiento establece una relación de confianza entre el servidor (o clúster) y el servicio de sincronización de almacenamiento. Un servidor solo se puede registrar en un servicio de sincronización de almacenamiento y puede sincronizarse con otros servidores y recursos compartidos de archivos de Azure asociados con el mismo servicio de sincronización de almacenamiento.

Tras la instalación del **agente de Azure File Sync**, la interfaz de usuario de Registro del servidor se debe abrir automáticamente. Si no es así, puede abrirla manualmente desde su ubicación de archivo: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Cuando la interfaz de usuario de Registro del servidor se abra en la máquina virtual, haga clic en **Aceptar**.
1. Haga clic en **Iniciar sesión** para comenzar.
1. Inicie sesión con las credenciales de su cuenta de Azure y haga clic en **Iniciar sesión**.
1. Proporcione la siguiente información:

   ![Captura de pantalla de la interfaz de usuario Registro del servidor](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Valor | DESCRIPCIÓN |
   | **Suscripción de Azure** | La suscripción que contiene el servicio de sincronización de almacenamiento de este tutorial. |
   | **Grupo de recursos** | El grupo de recursos que contiene el servicio de sincronización de almacenamiento de este tutorial. En este tutorial, se ha usado *afsresgroup101918*. |
   | **Servicio de sincronización de almacenamiento** | El nombre del servicio de sincronización de almacenamiento que se ha usado en este tutorial. En este tutorial, se ha usado *afssyncservice02*. |

1. Haga clic en **Registrar** para completar el registro del servidor.
1. Como parte del proceso de registro, se le solicita un inicio de sesión adicional. Inicie sesión y haga clic en **Siguiente**.
1. Haga clic en **OK**.

## <a name="create-a-sync-group"></a>Creación de un grupo de sincronización
Un grupo de sincronización define la topología de sincronización de un conjunto de archivos. Un grupo de sincronización debe contener al menos un punto de conexión en la nube, que representa un recurso compartido de archivos de Azure y uno o varios puntos de conexión de servidor. Un punto de conexión de servidor representa una ruta de acceso en un servidor registrado.

1. Para crear un grupo de sincronización, en [Azure Portal](https://portal.azure.com/), seleccione **+ Grupo de sincronización** en el servicio de sincronización de almacenamiento que creó para este tutorial. En este tutorial se ha usado *afssyncservice02* como ejemplo.

   ![Creación de un grupo de sincronización en Azure Portal](media/storage-sync-files-extend-servers/add-sync-group.png)

1. En el panel que se abre, escriba la información siguiente para crear un grupo de sincronización con un punto de conexión en la nube:

   | Valor | DESCRIPCIÓN |
   | ----- | ----- |
   | **Nombre del grupo de sincronización** | Este nombre debe ser único dentro del servicio de sincronización de almacenamiento, pero puede ser cualquier nombre que considere lógico. En este tutorial, se usa *afssyncgroup*.|
   | **Suscripción** | La suscripción en la que se ha implementado el servicio de sincronización de almacenamiento de este tutorial. |
   | **Cuenta de almacenamiento** |Haga clic en **Seleccionar una cuenta de almacenamiento**. En el panel que aparece, seleccione la cuenta de almacenamiento que tiene el recurso compartido de archivos de Azure que creó para este tutorial. Hemos usado *afsstoracct101918*. |
   | **Recurso compartido de archivos de Azure** | El nombre del recurso compartido de archivos de Azure que creó para este tutorial. Hemos usado *afsfileshare*. |

1. Haga clic en **Create**(Crear).

Si selecciona el grupo de sincronización, puede ver que ahora tiene uno **punto de conexión en la nube**.

## <a name="add-a-server-endpoint"></a>Adición de un punto de conexión de servidor
Un punto de conexión de servidor representa una ubicación específica en un servidor registrado, como una carpeta en un volumen de servidor.

1. Para agregar un punto de conexión de servidor, seleccione al grupo de sincronización recién creado y, después, seleccione **Agregar punto de conexión del servidor**.

   ![Adición de un nuevo punto de conexión de servidor al panel Grupo de sincronización](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. En el panel **Agregar punto de conexión de servidor**, escriba la siguiente información para crear un punto de conexión de servidor:

   | | |
   | ----- | ----- |
   | Valor | DESCRIPCIÓN |
   | **Servidor registrado** | El nombre del servidor que creó para este tutorial. Hemos usado *afsvm101918* en este tutorial |
   | **Path** | La ruta de acceso del servidor de Windows Server a la unidad que ha creado para el tutorial. En nuestro ejemplo, es *f:\filestosync*. |
   | **Nube por niveles** | Déjelo deshabilitado para este tutorial. |
   | **Espacio disponible del volumen** | Déjelo en blanco para este tutorial. |

1. Haga clic en **Create**(Crear).

Los archivos ya están sincronizados entre el recurso compartido de archivos de Azure y Windows Server.

![Azure Storage se ha sincronizado correctamente](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido los pasos básicos necesarios para ampliar la capacidad de almacenamiento de un servidor con Windows Server mediante Azure File Sync. Siga este vínculo para obtener una visión más completa del planeamiento de una implementación de Azure File Sync.

> [!div class="nextstepaction"]
> [Planeamiento de una implementación de Azure Files Sync](./storage-sync-files-planning.md)
