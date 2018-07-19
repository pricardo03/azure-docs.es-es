---
title: Implementar Azure File Sync (versión preliminar) | Microsoft Docs
description: Aprenda a implementar Azure File Sync de principio a fin.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: wgries
ms.openlocfilehash: 808bc3908790c8d6dedf1d9f00a4c70b42c7c490
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867076"
---
# <a name="deploy-azure-file-sync-preview"></a>Implementar Azure File Sync (versión preliminar)
Use Azure File Sync (versión preliminar) para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

Se recomienda encarecidamente leer [Planeamiento de una implementación de Azure Files](storage-files-planning.md) y [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md) antes de seguir los pasos que se describen en este artículo.

## <a name="prerequisites"></a>requisitos previos
* Una cuenta de Azure Storage y un recurso compartido de archivos de Azure en la misma región en la que quiere implementar Azure File Sync. Para más información, consulte:
    - [Disponibilidad en regiones](storage-sync-files-planning.md#region-availability) de Azure File Sync.
    - [Crear una cuenta de almacenamiento](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para obtener una descripción paso a paso sobre cómo crear una cuenta de almacenamiento.
    - [Creación de un recurso compartido de archivos](storage-how-to-create-file-share.md) para obtener una descripción paso a paso sobre cómo crear un recurso compartido de archivos.
* Al menos una instancia de Windows Server o un clúster de Windows Server compatible para la sincronización con Azure File Sync. Para obtener más información acerca de las versiones compatibles de Windows Server, consulte [Interoperabilidad con Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).
* Asegúrese de que PowerShell 5.1 está instalado en el servidor Windows Server. Si utiliza Windows Server 2012 R2, asegúrese de que ejecuta al menos PowerShell 5.1.\*. Esta comprobación se puede omitir sin riesgo alguno en Windows Server 2016, ya que PowerShell 5.1 es la versión predeterminada estándar. En Windows Server 2012 R2, para comprobar que se ejecuta PowerShell 5.1.\* es preciso ver el valor de la propiedad **PSVersion** del objeto **$PSVersionTable**:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Si el valor de PSVersion es menor que 5.1.\*, como sucede con la mayoría de las instalaciones nuevas de Windows Server 2012 R2, puede realizar la actualización fácilmente si descarga e instala [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). El paquete adecuado que hay que descargar e instalar para Windows Server 2012 R2 es **Win8.1AndW2K12R2-KB\*\*\*\*\*\*\*-x64.msu**.

    > [!Note]  
    > Azure File Sync aún no admite PowerShell 6+ en Windows Server 2012 R2 o Windows Server 2016.
* El módulo de PowerShell AzureRM en los servidores con los que desea utilizar Azure File Sync. Para más información sobre cómo instalar los módulos de PowerShell AzureRM, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Siempre se recomienda usar la versión más reciente de los módulos de Azure PowerShell. 

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Preparación de Windows Server para su uso con Azure File Sync
Para cada servidor que se va a usar con Azure File Sync, incluyendo cada nodo de servidor en un clúster de conmutación por error, deshabilite la **Configuración de seguridad mejorada de Internet Explorer**. Esto solo es necesario para el registro inicial del servidor. Se puede volver a habilitar una vez registrado el servidor.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
1. Abra el Administrador del servidor.
2. Haga clic en **Servidor Local**:  
    !["Servidor local" en el lado izquierdo de la interfaz de usuario Administrador del servidor](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. En el panel secundario **Propiedades**, seleccione el vínculo de **Configuración de seguridad mejorada de IE**.  
    ![Panel "Configuración de seguridad mejorada de IE" en la interfaz de usuario Administrador del servidor](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. En el cuadro de diálogo **Configuración de seguridad mejorada de Internet Explorer**, seleccione **Desactivado** para **Administradores** y **Usuarios**:  
    ![Ventana emergente de la configuración de seguridad mejorada de Internet Explorer con "Desactivado" seleccionado](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Para deshabilitar la configuración de seguridad mejorada de Internet Explorer, ejecute lo siguiente desde una sesión de PowerShell con privilegios elevados:

```PowerShell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="install-the-azure-file-sync-agent"></a>Instalación del agente de Azure File Sync
El agente de Azure File Sync es un paquete descargable que permite la sincronización de Windows Server con un recurso compartido de archivos de Azure. 

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Puede descargar el agente desde el [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?linkid=858257). Una finalice la descarga, haga doble clic en el paquete MSI para iniciar la instalación del agente de Azure File Sync.

> [!Important]  
> Si piensa usar Azure File Sync con un clúster de conmutación por error, el agente de Azure File Sync se debe instalar en todos los nodos del clúster.

Se recomienda hacer lo siguiente:
- Dejar la ruta de instalación predeterminada (C:\Archivos de programa\Azure\StorageSyncAgent), para simplificar las operaciones de solución de problemas y mantenimiento del servidor.
- Habilitar Microsoft Update para mantener actualizado Azure File Sync. Todas las actualizaciones del agente de Azure File Sync, incluidas las actualizaciones y revisiones de características, se realizan desde Microsoft Update. Se recomienda instalar la actualización más reciente de Azure File Sync. Para obtener más información, consulte [Directiva de actualización del agente de Azure File Sync](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Al término de la instalación del agente de Azure File Sync, la interfaz de usuario Registro del servidor se abre automáticamente. Debe tener un servicio de sincronización de almacenamiento antes del registro; vea la sección siguiente sobre cómo crear un servicio de sincronización de almacenamiento.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Ejecute el siguiente código de PowerShell para descargar la versión adecuada del agente de Azure File Sync para su sistema operativo e instálelo en el sistema.

```PowerShell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875004 `
        -OutFile "StorageSyncAgent.exe" 
}
elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://go.microsoft.com/fwlink/?linkid=875002 `
        -OutFile "StorageSyncAgent.exe" 
}
else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2 and Windows Server 2016")
}

# Extract the MSI from the install package
$tempFolder = New-Item -Path "afstemp" -ItemType Directory
Start-Process -FilePath ".\StorageSyncAgent.exe" -ArgumentList "/C /T:$tempFolder" -Wait

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "$($tempFolder.FullName)\StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.exe", ".\afstemp" -Recurse -Force
```

---

## <a name="deploy-the-storage-sync-service"></a>Implementación del servicio de sincronización de almacenamiento 
La implementación de Azure File Sync comienza por situar un recurso del **servicio de sincronización de almacenamiento** en un grupo de recursos de la suscripción seleccionada. Se recomienda aprovisionar únicamente los necesarios. Se creará una relación de confianza entre los servidores y este recurso y un servidor solo se puede registrar en un servicio de sincronización de almacenamiento. Como resultado, se recomienda implementar tantos servicios de sincronización de almacenamiento como necesite para separar los grupos de servidores. Tenga en cuenta que los servidores de servicios de sincronización de almacenamiento diferentes no se pueden sincronizar entre sí.

> [!Note]
> El servicio de sincronización de almacenamiento hereda los permisos de acceso de la suscripción y el grupo de recursos en los que se ha implementado. Se recomienda que compruebe cuidadosamente quién tiene acceso al mismo. Las entidades con acceso de escritura pueden empezar a sincronizar nuevos conjuntos de archivos de servidores registrados en este servicio de sincronización de almacenamiento y hacer que los datos fluyan al almacenamiento de Azure al que tengan acceso.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para implementar un servicio de sincronización de almacenamiento, vaya a [Azure Portal](https://portal.azure.com/), haga clic en *Nuevo* y busque Azure File Sync. En los resultados de la búsqueda, seleccione **Azure File Sync (versión preliminar)** y, a continuación, seleccione **Crear** para abrir la pestaña **Implementar la sincronización del almacenamiento**.

En el panel que se abre, escriba la siguiente información:

- **Nombre**: un nombre único (por suscripción) para el servicio de sincronización de almacenamiento.
- **Suscripción**: la suscripción en la que quiere crear el servicio de sincronización de almacenamiento. Según la estrategia de configuración de la organización, puede tener acceso a una o más suscripciones. Una suscripción de Azure es el contenedor más básico para la facturación de cada servicio en la nube (como Azure Files).
- **Grupo de recursos**: un grupo de recursos es un grupo lógico de recursos de Azure, como una cuenta de almacenamiento o un servicio de sincronización de almacenamiento. Puede crear un nuevo grupo de recursos o seleccionar uno existente para Azure File Sync. (Se recomienda usar los grupos de recursos como contenedores para aislar los recursos de manera lógica para su organización, como la agrupación de recursos de RR. HH. o recursos de un proyecto específico).
- **Ubicación**: la región en la que quiere implementar Azure File Sync. Solo las regiones admitidas están disponibles en esta lista.

Cuando haya terminado, seleccione **Crear** para implementar el servicio de sincronización de almacenamiento.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Antes de interactuar con los cmdlets de administración de Azure File Sync, debe importar un archivo DLL y crear un contexto de administración de Azure File Sync. Esto es necesario porque los cmdlets de administración de Azure File Sync todavía no forman parte del módulo de PowerShell AzureRM.

> [!Note]  
> El paquete StorageSync.Management.PowerShell.Cmdlets.dll, que contiene los cmdlets de administración de Azure File Sync, contiene (intencionadamente) un cmdlet con un verbo no aprobado (`Login`). El nombre `Login-AzureRmStorageSync` se ha elegido para que coincida con el alias del cmdlet `Login-AzureRmAccount` en el módulo de PowerShell AzureRM. Este mensaje de error (y el cmdlet) se eliminarán cuando el agente de Azure File Sync se agregue al módulo de PowerShell AzureRM.

```PowerShell
$acctInfo = Login-AzureRmAccount

# The location of the Azure File Sync Agent. If you have installed the Azure File Sync 
# agent to a non-standard location, please update this path.
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzureRmResourceGroup -Name $resourceGroup -Location $region
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region
```

Una vez haya creado el contexto de Azure File Sync con el cmdlet `Login-AzureRmStorageSync`, puede crear el servicio de sincronización de almacenamiento. No olvide reemplazar `<my-storage-sync-service>` por el nombre deseado del servicio de sincronización de almacenamiento.

```PowerShell
$storageSyncName = "<my-storage-sync-service>"
New-AzureRmStorageSyncService -StorageSyncServiceName $storageSyncName
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Registro de un servidor de Windows Server con el servicio de sincronización de almacenamiento
El registro del servidor Windows Server con un servicio de sincronización de almacenamiento establece una relación de confianza entre el servidor (o clúster) y el servicio de sincronización de almacenamiento. Un servidor solo se puede registrar en un servicio de sincronización de almacenamiento y puede sincronizarse con otros servidores y recursos compartidos de archivos de Azure asociados con el mismo servicio de sincronización de almacenamiento.

> [!Note]
> El registro del servidor utiliza sus credenciales de Azure para crear una relación de confianza entre el servicio de sincronización de almacenamiento y el servidor Windows Server; sin embargo, posteriormente, el servidor crea y usa su propia identidad, que es válida mientras el servidor permanece registrado y el token de la firma de acceso compartido (SAS de almacenamiento) actual sea válido. No se puede emitir un nuevo token de SAS para el servidor una vez que se anula el registro del servidor, lo que elimina la capacidad del servidor de acceder a los recursos compartidos de archivos de Azure, deteniendo cualquier sincronización.

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Al término de la instalación del agente de Azure File Sync, la interfaz de usuario Registro del servidor debería abrirse automáticamente. Si no es así, puede abrirla manualmente desde la ubicación del archivo: C:\Archivos de programa\Azure\StorageSyncAgent\ServerRegistration.exe. Cuando la interfaz de usuario Registro del servidor se abra, seleccione **Iniciar sesión** para comenzar.

Después de iniciar sesión se le pedirá la información siguiente:

![Captura de pantalla de la interfaz de usuario Registro del servidor](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Suscripción de Azure**: la suscripción que contiene el servicio de sincronización de almacenamiento (vea [Implementación del servicio de sincronización de almacenamiento](#deploy-the-storage-sync-service)). 
- **Grupo de recursos**: el grupo de recursos que contiene el servicio de sincronización de almacenamiento.
- **Servicio de sincronización de almacenamiento**: el nombre del servicio de sincronización de almacenamiento con el que quiere registrar.

Una vez que haya seleccionado la información adecuada, haga clic en **Registrar** para completar el registro del servidor. Como parte del proceso de registro, se le solicita un inicio de sesión adicional.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$registeredServer = Register-AzureRmStorageSyncServer -StorageSyncServiceName $storageSyncName
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Creación de un grupo de sincronización y un punto de conexión en la nube
Un grupo de sincronización define la topología de sincronización de un conjunto de archivos. Los puntos de conexión dentro de un grupo de sincronización se mantienen sincronizados entre sí. Un grupo de sincronización debe contener al menos un punto de conexión en la nube, que representa un recurso compartido de archivos de Azure y uno o más puntos de conexión de servidor. Un punto de conexión de servidor representa una ruta de acceso en un servidor registrado. Un servidor puede tener puntos de conexión de servidor en varios grupos de sincronización. Puede crear tantos grupos de sincronización como sea necesario para describir correctamente la topología de sincronización deseada.

Un punto de conexión en la nube es un puntero a un recurso compartido de archivos de Azure. Todos los puntos de conexión de servidor se sincronizarán con un punto de conexión en la nube, lo que hace que el punto de conexión en la nube actúe como concentrador. La cuenta de almacenamiento para el recurso compartido de archivos de Azure debe encontrarse en la misma región que el servicio de sincronización de almacenamiento. Se sincronizará la totalidad del recurso compartido de archivos de Azure, con una excepción: una carpeta especial, comparable a la carpeta oculta "System Volume Information" de un volumen NTFS, se aprovisionará. Este directorio se llama ".SystemShareInformation". Contiene metadatos de sincronización importantes que no se sincronizarán con otros puntos de conexión. No la utilice ni elimine.

> [!Important]  
> Puede realizar cambios en cualquier punto de conexión en la nube o punto de conexión de servidor en el grupo de sincronización y sincronizar los archivos con los demás puntos de conexión del grupo de sincronización. Si realiza algún cambio directamente en el punto de conexión en la nube (recurso compartido de archivos de Azure), tenga en cuenta que un trabajo de detección de cambios de Azure File Sync deberá detectar primero esos cambios. Se inicia un trabajo de detección de cambios para un punto de conexión en la nube solo una vez cada 24 horas. Para obtener más información, consulte [Preguntas más frecuentes de Azure Files](storage-files-faq.md#afs-change-detection).

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para crear un grupo de sincronización, en [Azure Portal](https://portal.azure.com/), vaya al servicio de sincronización de almacenamiento y haga clic en **+ Grupo de sincronización**:

![Creación de un grupo de sincronización en Azure Portal](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

En el panel que se abre, escriba la información siguiente para crear un grupo de sincronización con un punto de conexión en la nube:

- **Nombre del grupo de sincronización**: el nombre del grupo de sincronización que se va a crear. Este nombre debe ser único dentro del servicio de sincronización de almacenamiento, pero puede ser cualquier nombre que considere lógico.
- **Suscripción**: la suscripción en la que ha implementado el servicio de sincronización de almacenamiento en [Implementación del servicio de sincronización de almacenamiento](#deploy-the-storage-sync-service).
- **Cuenta de almacenamiento**: si elige **Seleccionar cuenta de almacenamiento**, aparece otro panel donde puede seleccionar la cuenta de almacenamiento que tiene el recurso compartido de archivos de Azure con el que quiere realizar la sincronización.
- **Recurso compartido de archivos de Azure**: el nombre del recurso compartido de archivos de Azure con el que desea realizar la sincronización.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Para crear el grupo de sincronización, ejecute el comando de PowerShell siguiente. No olvide reemplazar `<my-sync-group>` por el nombre deseado del grupo de sincronización.

```PowerShell
$syncGroupName = "<my-sync-group>"
New-AzureRmStorageSyncGroup -SyncGroupName $syncGroupName -StorageSyncService $storageSyncName
```

Una vez que se ha creado correctamente el grupo de sincronización, puede crear el punto de conexión en la nube. No olvide reemplazar `<my-storage-account>` y `<my-file-share>` por los valores esperados.

```PowerShell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzureRmStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzureStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzureRmStorageSyncCloudEndpoint `
    -StorageSyncServiceName $storageSyncName `
    -SyncGroupName $syncGroupName ` 
    -StorageAccountResourceId $storageAccount.Id
    -StorageAccountShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Creación de un punto de conexión de servidor
Un punto de conexión de servidor representa una ubicación específica en un servidor registrado, como una carpeta en un volumen de servidor. Un punto de conexión de servidor debe ser una ruta de acceso en un servidor registrado (en lugar de un recurso compartido montado) y para usar niveles de nube, la ruta de acceso debe estar en un volumen que no sea del sistema. No se admite el almacenamiento conectado a la red (NAS).

# <a name="portaltabportal"></a>[Portal](#tab/portal)
Para agregar un punto de conexión de servidor, vaya al grupo de sincronización recién creado y seleccione **Agregar punto de conexión del servidor**.

![Adición de un nuevo punto de conexión de servidor al panel Grupo de sincronización](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

En el panel **Agregar punto de conexión de servidor**, escriba la siguiente información para crear un punto de conexión de servidor:

- **Servidor registrado**: el nombre del servidor o el clúster en el que se va a crear el punto de conexión de servidor.
- **Ruta de acceso**: la ruta de acceso de Windows Server que se va a sincronizar como parte del grupo de sincronización.
- **Niveles de la nube**: un conmutador para habilitar o deshabilitar los niveles de la nube. Con los niveles de la nube, los archivos que se usen o a los que se acceda con poca frecuencia se pueden colocar en capas en Azure Files.
- **Espacio disponible del volumen**: la cantidad de espacio libre que se reserva en el volumen en el que reside el punto de conexión de servidor. Por ejemplo, si el espacio disponible del volumen se establece en el 50 % en un volumen con un único punto de conexión de servidor, casi la mitad de la cantidad de datos se coloca en capas en Azure Files. Con independencia de si la característica de niveles en la nube está habilitada, el recurso compartido de archivos de Azure siempre tiene una copia completa de los datos en el grupo de sincronización.

Para agregar el punto de conexión de servidor, seleccione **Crear**. Los archivos se mantienen ahora sincronizados entre el recurso compartido de archivos de Azure y Windows Server. 

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Ejecute los siguientes comandos de PowerShell para crear el punto de conexión de servidor y asegúrese de reemplazar `<your-server-endpoint-path>` y `<your-volume-free-space>` por los valores deseados.

```PowerShell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering $true `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
}
else {
    # Create server endpoint
    New-AzureRmStorageSyncServerEndpoint `
        -StorageSyncServiceName $storageSyncName `
        -SyncGroupName $syncGroupName `
        -ServerId $registeredServer.Id `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Incorporación con Azure File Sync
Los pasos recomendados para la incorporación en Azure File Sync por primera vez con un tiempo de inactividad nulo conservando al mismo tiempo la fidelidad total en los archivos y la lista de control de acceso (ACL) son los siguientes:
 
1. Implementar un servicio de sincronización de almacenamiento.
2. Crear un grupo de sincronización.
3. Instalar el agente de Azure File Sync en el servidor con el conjunto de datos completo.
4. Registrar ese servidor y crear un punto de conexión de servidor en el recurso compartido. 
5. Permitir que la sincronización realice la carga completa al recurso compartido de archivos de Azure (punto de conexión en la nube).  
6. Una vez finalizada la carga inicial, instalar el agente de Azure File Sync en cada uno de los demás servidores.
7. Crear nuevos recursos compartidos de archivos en cada uno de los demás servidores.
8. Crear puntos de conexión de servidor en nuevos recursos compartidos de archivos con la directiva de niveles de nube, si lo desea. (Este paso requiere que haya almacenamiento adicional disponible para la instalación inicial).
9. Permitir que el agente de Azure File Sync realice una restauración rápida del espacio de nombres completo sin la transferencia de datos real. Después de la sincronización del espacio de nombres completo, el motor de sincronización rellenará el espacio en disco local en función de la directiva de niveles de nube para el punto de conexión del servidor. 
10. Asegúrese de que la sincronización finaliza y pruebe la topología según sea necesario. 
11. Redirigir a los usuarios y las aplicaciones a este recurso compartido nuevo.
12. Si lo desea, se pueden eliminar todos los recursos compartidos duplicados en los servidores.
 
Si no tiene almacenamiento adicional para la incorporación inicial y le gustaría conectarse a los recursos compartidos existentes, puede inicializar previamente los datos de los recursos compartidos de archivos de Azure. Se recomienda este enfoque si y solo si puede aceptar un tiempo de inactividad y garantiza de forma absoluta que no cambia ningún dato en los recursos compartidos del servidor durante el proceso de incorporación inicial. 
 
1. Asegúrese que los datos no pueden cambiar en ninguno de los servidores durante el proceso de incorporación.
2. Inicialice previamente los recursos compartidos de archivos de Azure con los datos del servidor mediante cualquier herramienta de transferencia de datos a través de SMB, p. ej. Robocopy, copia directa de SMB. Puesto que AzCopy no carga datos a través de SMB, no se puede usar para la inicialización previa.
3. Cree la topología de Azure File Sync con los puntos de conexión de servidor que desee que apunten a los recursos compartidos existentes.
4. Permita que la sincronización finalice el proceso de conciliación en todos los puntos de conexión. 
5. Una vez completada la conciliación, puede abrir recursos compartidos para los cambios.
 
Tenga en cuenta que, actualmente, el enfoque de inicialización previa tiene algunas limitaciones: 
- No se conserva la fidelidad total en los archivos. Por ejemplo, los archivos pierden las ACL y las marcas de tiempo.
- Los datos cambian en el servidor antes de que la topología de sincronización esté totalmente activa y la ejecución puede producir conflictos en los puntos de conexión del servidor.  
- Después de crearse el punto de conexión en la nube, Azure File Sync ejecuta un proceso para detectar los archivos en la nube antes de llevar a cabo la sincronización inicial. El tiempo necesario para completar este proceso depende de distintos factores, como la velocidad de la red, el ancho de banda disponible y el número de archivos y carpetas. Para una estimación aproximada del lanzamiento de la versión preliminar, el proceso de detección se ejecuta a una velocidad aproximada de diez archivos/s. Por lo tanto, incluso si la inicialización previa se ejecuta rápido, el tiempo total para obtener un sistema totalmente operativo puede ser considerablemente mayor cuando los datos se inicializan previamente en la nube.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Migración de una implementación de la replicación DFS (DFS-R) a Azure File Sync
Para migrar una implementación de DFS-R a Azure File Sync:

1. Cree un grupo de sincronización para representar la topología DFS-R que se va a reemplazar.
2. Comience por el servidor que tiene el conjunto completo de datos en la topología DFS-R que se va a migrar. Instale Azure File Sync en ese servidor.
3. Registre ese servidor y cree un punto de conexión de servidor para el primer servidor que desea migrar. No habilite niveles en la nube.
4. Deje que todos los datos se sincronicen con el recurso compartido de archivos de Azure (punto de conexión en la nube).
5. Instale y registre el agente de Azure File Sync en cada uno de los demás servidores de DFS-R.
6. Deshabilite DFS-R. 
7. Cree un punto de conexión de servidor en cada uno de los servidores de DFS-R. No habilite niveles en la nube.
8. Asegúrese de que la sincronización finaliza y pruebe la topología según sea necesario.
9. Retire DFS-R.
10. Ahora se pueden habilitar los niveles en la nube en cualquier punto de conexión de servidor según sea necesario.

Para más información, consulte [Interoperabilidad de Azure File Sync con el sistema de archivos distribuido (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Pasos siguientes
- [Adición o eliminación de un punto de conexión de servidor de Azure File Sync ](storage-sync-files-server-endpoint.md)
- [Registro y cancelación del registro de un servidor con Azure File Sync](storage-sync-files-server-registration.md)
