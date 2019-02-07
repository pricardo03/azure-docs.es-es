---
title: Guía de inicio rápido para administrar recursos compartidos de archivos de Azure mediante Azure PowerShell
description: Use esta guía de inicio rápido para administrar recursos compartidos de archivos de Azure mediante Azure PowerShell.
services: storage
author: wmgries
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 53f2b59032e7e9725b067ef472b2b6dcf94bcb72
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662481"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Inicio rápido: Creación y administración de un recurso compartido de archivos de Azure con Azure PowerShell 
En esta guía se describen los conceptos básicos sobre cómo trabajar con [recursos compartidos de archivos de Azure](storage-files-introduction.md) con PowerShell. Los recursos compartidos de archivos de Azure son iguales a otros recursos compartidos de archivos, pero se almacenan en la nube y están respaldados por la plataforma Azure. Los recursos compartidos de archivos de Azure admiten el protocolo SMB estándar del sector y permiten el uso compartido entre varias máquinas, aplicaciones e instancias. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si desea instalar y usar PowerShell de forma local, para esta guía se requiere la versión 0.7 del módulo de Azure PowerShell o una versión posterior. Para averiguar qué versión del módulo de Azure PowerShell está ejecutando, ejecute `Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-Az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Login-AzAccount` para iniciar sesión en la cuenta de Azure.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Si ya no tiene un grupo de recursos de Azure, puede crear uno nuevo con el cmdlet [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región Este de EE. UU.:

```azurepowershell-interactive
New-AzResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento
Una cuenta de almacenamiento es un grupo compartido de almacenamiento que puede utilizar para implementar recursos compartidos de archivos de Azure u otros recursos de almacenamiento como blobs o colas. Una cuenta de almacenamiento puede contener un número ilimitado de recursos compartidos y un recurso compartido puede almacenar un número ilimitado de archivos, hasta los límites de capacidad de la cuenta de almacenamiento.

En este ejemplo se crea una cuenta de almacenamiento con el cmdlet [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount). La cuenta de almacenamiento se llama *mystorageaccount<random number>* y se almacena una referencia a esa cuenta de almacenamiento en la variable **$storageAcct**. Los nombres de la cuenta de almacenamiento deben ser únicos, por lo que debe usar `Get-Random` para anexar un número al nombre para que sea único. 

```azurepowershell-interactive 
$storageAcct = New-AzStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Creación de un recurso compartido de archivos de Azure
Ahora puede crear su primer recurso compartido de archivos de Azure. Puede crear un recurso compartido de archivos mediante el cmdlet [New-AzStorageShare](/powershell/module/az.storage/New-AzStorageShare). Este ejemplo crea un recurso compartido denominado `myshare`.

```azurepowershell-interactive
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

Los nombres de recursos compartidos deben estar formados por letras minúsculas, números y guiones sencillos, pero no pueden empezar con un guion. Para obtener detalles completos sobre cómo asignar un nombre a recursos compartidos y archivos, consulte [Asignación de nombres y referencia a recursos compartidos, directorios, archivos y metadatos](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

## <a name="use-your-azure-file-share"></a>Uso de un recurso compartido de archivos de Azure
Azure Files proporciona dos métodos para trabajar con archivos y carpetas dentro de un recurso compartido de archivos de Azure: el [protocolo de bloque de mensajes del servidor (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) estándar del sector y el [REST de archivo](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Para montar un recurso compartido de archivos con SMB, consulte el siguiente documento según su sistema operativo:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Uso de un recurso compartido de archivos de Azure con el protocolo File REST 
Se puede trabajar directamente con el protocolo REST de archivo (es decir, escribir a mano las llamadas a HTTP de REST), pero la manera más habitual de usar este protocolo es mediante el módulo de Azure PowerShell, la [CLI de Azure](storage-how-to-use-files-cli.md) o un SDK de Azure Storage; todos ellos proporcionan un buen contenedor para el protocolo REST de archivo en el lenguaje de programación o script de su elección.  

En la mayoría de los casos, usará el recurso compartido de archivos de Azure a través del protocolo SMB, dado que permite usar las aplicaciones y herramientas existentes que se desean usar; sin embargo, existen varias razones por las que es beneficioso usar la API REST de archivo en lugar de SMB, como por ejemplo:

- Va a examinar el recurso compartido de archivos desde PowerShell Cloud Shell (que no puede montar recursos compartidos de archivos a través de SMB).
- Debe ejecutar un script o una aplicación desde un cliente que no puede montar recursos compartidos de SMB, como los clientes locales que no tienen desbloqueado el puerto 445.
- Se quieren aprovechar las ventajas de los recursos sin servidor, como [Azure Functions](../../azure-functions/functions-overview.md). 

En los ejemplos siguientes se muestra cómo usar el módulo de Azure PowerShell para manipular el recurso compartido de archivos de Azure con el protocolo REST de archivo. 

#### <a name="create-directory"></a>Creación del directorio
Para crear un nuevo directorio denominado *myDirectory* en la raíz del recurso compartido de archivos de Azure, use el cmdlet [New-AzStorageDirectory](/powershell/module/az.storage/New-AzStorageDirectory).

```azurepowershell-interactive
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Cargar un archivo
Para demostrar cómo cargar un archivo mediante el cmdlet [Set-AzStorageFileContent](/powershell/module/az.storage/Set-AzStorageFileContent), primero es necesario crear un archivo dentro de la unidad temporal de PowerShell Cloud Shell donde cargar. 

Este ejemplo pone la fecha y hora actuales en un nuevo archivo en el disco temporal y, a continuación, lo carga en el recurso compartido de archivos.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

Si está ejecutando PowerShell localmente, debe sustituir `C:\Users\ContainerAdministrator\CloudDrive\` con una ruta de acceso que exista en la máquina.

Después de cargar el archivo, puede usar el cmdlet [Get-AzStorageFile](/powershell/module/Az.Storage/Get-AzStorageFile) para asegurarse de que el archivo se cargó en el recurso compartido de archivos de Azure. 

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Descarga de un archivo
Puede usar el cmdlet [Get-AzStorageFileContent](/powershell/module/az.storage/Get-AzStorageFilecontent) para descargar una copia del archivo que acaba de cargar en la unidad temporal de Cloud Shell.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item `
     -Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Después de descargar el archivo, puede usar el cmdlet `Get-ChildItem` para ver que el archivo se ha descargado a la unidad temporal de PowerShell Cloud Shell.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

#### <a name="copy-files"></a>Copiar archivos
Una tarea habitual consiste en copiar los archivos de un recurso compartido de archivos a otro o entre contenedores de almacenamiento de Azure Blob. Para demostrar esta funcionalidad, puede crear un nuevo recurso compartido y copiar el archivo recién cargado en este nuevo recurso compartido con el cmdlet [Start-AzStorageFileCopy](/powershell/module/az.storage/Start-AzStorageFileCopy). 

```azurepowershell-interactive
New-AzStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Ahora, si enumera los archivos en el nuevo recurso compartido, debería ver el archivo copiado.

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Aunque el cmdlet `Start-AzStorageFileCopy` es conveniente para los movimientos de archivos ad hoc entre recursos compartidos de archivos de Azure y contenedores de almacenamiento de Azure Blob, se recomienda usar AzCopy para movimientos mayores (en cuanto al número o el tamaño de los archivos que se van a mover). Obtenga más información sobre [AzCopy para Windows](../common/storage-use-azcopy.md) y [AzCopy para Linux](../common/storage-use-azcopy-linux.md). AzCopy se debe instalar localmente; ya que no está disponible en Cloud Shell. 

## <a name="create-and-manage-share-snapshots"></a>Creación y administración de instantáneas de recurso compartido
Una tarea útil adicional que puede hacer con un recurso compartido de archivos de Azure consiste en crear instantáneas de recurso compartido. Una instantánea conserva un punto en el tiempo para un recurso compartido de archivos de Azure. Las instantáneas de recurso compartido son similares a las tecnologías de sistemas operativos que es posible que conozca, como son:
- [Servicio de instantáneas de volumen (VSS)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal) para sistemas de archivos Windows como NTFS y ReFS
- Instantáneas del [Administrador de volúmenes lógicos (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) para sistemas Linux
- Instantáneas de [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) para macOS. 
 Puede crear una instantánea de un recurso compartido mediante el método `Snapshot` en el objeto de PowerShell para un recurso compartido de archivos, que se recupera con el cmdlet [Get-AzStorageShare](/powershell/module/az.storage/Get-AzStorageShare). 

```azurepowershell-interactive
$share = Get-AzStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Examen de las instantáneas de recurso compartido
Puede examinar el contenido de la instantánea del recurso compartido pasando la referencia de la instantánea (`$snapshot`) al parámetro `-Share` del cmdlet `Get-AzStorageFile`.

```azurepowershell-interactive
Get-AzStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Enumerar instantáneas del recurso compartido
Puede ver la lista de instantáneas que ha realizado para el recurso compartido con el siguiente comando.

```azurepowershell-interactive
Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Restauración desde una instantánea de recurso compartido
Puede restaurar un archivo mediante el comando `Start-AzStorageFileCopy` que hemos usado antes. Para los fines de esta guía de inicio rápido, primero eliminaremos nuestro archivo `SampleUpload.txt` que cargamos previamente para poder restaurarlo a partir de la instantánea.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"
 # Restore SampleUpload.txt from the share snapshot
Start-AzStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Eliminación de una instantánea de recurso compartido
Puede eliminar una instantánea de recurso compartido mediante el cmdlet [Remove-AzStorageShare](/powershell/module/az.storage/Remove-AzStorageShare), con la variable que contiene la referencia `$snapshot` al parámetro `-Share`.

```azurepowershell-interactive
Remove-AzStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado, puede usar el cmdlet [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos relacionados. 

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

Como alternativa, puede quitar recursos de uno en uno:

- Quitar los recursos compartidos de archivos de Azure que hemos creado para esta guía de inicio rápido.

    ```azurepowershell-interactive
    Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
        Remove-AzStorageShare -Context $storageAcct.Context -Name $_.Name
    }
    ```

- Quitar la propia cuenta de almacenamiento (así se quitarán implícitamente los recursos compartidos de archivos de Azure que hemos creado, así como cualquier otro recurso de almacenamiento que pueda haber creado, como un contenedor de Azure Blob Storage).

    ```azurepowershell-interactive
    Remove-AzStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [¿Qué es Azure Files?](storage-files-introduction.md)
