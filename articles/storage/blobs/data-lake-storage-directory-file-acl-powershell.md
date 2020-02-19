---
title: PowerShell de Azure Data Lake Storage Gen2 para archivos y ACL (versión preliminar)
description: Use cmdlets de PowerShell para administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan habilitado un espacio de nombres jerárquico (HNS).
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: a5ad4cbd11d6468413a43576e2156ee807067aa8
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153356"
---
# <a name="use-powershell-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Uso de PowerShell para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2 (versión preliminar)

En este artículo se explica cómo usar PowerShell para crear y administrar directorios, archivos y permisos en cuentas de almacenamiento que tengan habilitado un espacio de nombres jerárquico (HNS). 

> [!IMPORTANT]
> El módulo de PowerShell que se incluye en este artículo se encuentra actualmente en versión preliminar pública.

[Asignación de Gen1 a Gen2](#gen1-gen2-map) | [Envíenos sus comentarios](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>Prerrequisitos

> [!div class="checklist"]
> * Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](data-lake-storage-quickstart-create-account.md) instrucciones para crear uno.
> * Debe tener instalada la versión de .NET Framework 4.7.2 o superior. Vea [Descargar .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
> * La versión de PowerShell `5.1` o posterior.

## <a name="install-powershell-modules"></a>Instalación de módulos PowerShell

1. Use el siguiente comando para comprobar que la versión de PowerShell que ha instalado es `5.1` o posterior. 

    ```powershell
    echo $PSVersionTable.PSVersion.ToString() 
    ```
    
    Para actualizar la versión de PowerShell, vea [Actualización de Windows PowerShell existente](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).
    
2. Instale el módulo de **PowershellGet** más reciente. Luego, cierre la consola de PowerShell y vuelva a abrirla.

    ```powershell
    install-Module PowerShellGet –Repository PSGallery –Force 
    ```

3.  Instale el módulo de versión preliminar **Az.Storage**.

    ```powershell
    install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.9.1-preview –AllowPrerelease –AllowClobber –Force 
    ```

    Para más información sobre cómo instalar módulos de PowerShell, vea [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

## <a name="connect-to-the-account"></a>Conexión con la cuenta

Abra una ventana de comandos de Windows PowerShell y, a continuación, inicie sesión en su suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

```powershell
Connect-AzAccount
```

Si su identidad está asociada a más de una suscripción, establezca la suscripción activa en la suscripción de la cuenta de almacenamiento en la que quiere crear y administrar directorios. En este ejemplo, reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

A continuación, elija cómo desea que sus comandos obtengan autorización para la cuenta de almacenamiento. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Opción 1: Obtención de autorización mediante Azure Active Directory (AD)

Con este enfoque, el sistema garantiza que su cuenta de usuario tiene los permisos de ACL y las asignaciones de control de acceso basado en rol (RBAC). 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Opción 2: Obtención de autorización mediante la clave de cuenta de almacenamiento

Con este enfoque, el sistema no comprueba los permisos ACL o de RBAC.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

## <a name="create-a-file-system"></a>Creación de un sistema de archivos

Un sistema de archivos actúa como contenedor de los archivos. Puede crear uno con el cmdlet `New-AzDatalakeGen2FileSystem`. 

En este ejemplo se crea un sistema de archivos llamado `my-file-system`.

```powershell
$filesystemName = "my-file-system"
New-AzDatalakeGen2FileSystem -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Creación de un directorio

Cree una referencia de directorio con el cmdlet `New-AzDataLakeGen2Item`. 

En este ejemplo se agrega un directorio denominado `my-directory` a un sistema de archivos.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

En este ejemplo se agrega el mismo directorio, pero también se establecen los permisos, la máscara de permisos, los valores de propiedad y los valores de metadatos. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>Visualización de las propiedades de directorio

En este ejemplo se obtiene un directorio con el cmdlet `Get-AzDataLakeGen2Item` y, después, se imprimen los valores de propiedad en la consola.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Directory.PathProperties.Group
$dir.Directory.PathProperties.Owner
$dir.Directory.Metadata
$dir.Directory.Properties
```

## <a name="rename-or-move-a-directory"></a>Cambio de nombre o traslado de un directorio

Cambie el nombre de un directorio o muévalo con el cmdlet `Move-AzDataLakeGen2Item`.

En este ejemplo se cambia el nombre de un subdirectorio de `my-directory` a `my-new-directory`.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

En este ejemplo se mueve un directorio denominado `my-directory` a un subdirectorio de `my-directory-2` denominado `my-subdirectory`. En este ejemplo también se aplica una máscara de permisos al subdirectorio.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2 -Umask --------x -PathRenameMode Posix
```

## <a name="delete-a-directory"></a>Eliminación de un directorio

Elimine un directorio con el cmdlet `Remove-AzDataLakeGen2Item`.

En este ejemplo se elimina un directorio denominado `my-directory`. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

Puede usar el parámetro `-Force` para quitar el archivo sin preguntar.

## <a name="download-from-a-directory"></a>Descarga de un directorio

Descargue un archivo de un directorio con el cmdlet `Get-AzDataLakeGen2ItemContent`.

En este ejemplo se descarga un archivo denominado `upload.txt` de un directorio denominado `my-directory`. 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Lista del contenido del directorio

Muestre el contenido de un directorio con el cmdlet `Get-AzDataLakeGen2ChildItem`.

En este ejemplo se muestra el contenido de un directorio denominado `my-directory`.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname
```

En este ejemplo no se devuelven valores de las propiedades `ACL`, `Permissions`, `Group` y `Owner`. Para obtener esos valores, use el parámetro `-FetchPermission`. 

En el siguiente ejemplo se muestra el contenido del mismo directorio, pero también usa el parámetro `-FetchPermission` para devolver valores de las propiedades `ACL`, `Permissions`, `Group` y `Owner`. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchPermission
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

Para mostrar el contenido de un sistema de archivos, omita el parámetro `-Path` del comando.

## <a name="upload-a-file-to-a-directory"></a>Carga de un archivo en un directorio

Cargue un archivo en un directorio con el cmdlet `New-AzDataLakeGen2Item`.

En este ejemplo se carga un archivo denominado `upload.txt` en un directorio denominado `my-directory`. 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

En este ejemplo se carga el mismo archivo, pero después se establecen los permisos, la máscara de permisos, los valores de propiedad y los valores de metadatos del archivo de destino. En este ejemplo también se imprimen estos valores en la consola.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.File.Metadata
$file1.File.Properties
```

## <a name="show-file-properties"></a>Visualización de las propiedades del archivo

En este ejemplo se obtiene un archivo con el cmdlet `Get-AzDataLakeGen2Item` y, después, se imprimen los valores de propiedad en la consola.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.File.PathProperties.Group
$file.File.PathProperties.Owner
$file.File.Metadata
$file.File.Properties
```

## <a name="delete-a-file"></a>Eliminación de un archivo

Elimine un archivo con el cmdlet `Remove-AzDataLakeGen2Item`.

En este ejemplo se elimina un archivo denominado `upload.txt`. 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

Puede usar el parámetro `-Force` para quitar el archivo sin preguntar.

## <a name="manage-access-permissions"></a>Administración de permisos de acceso

Puede obtener, establecer y actualizar los permisos de acceso de los directorios y archivos.

> [!NOTE]
> Si usa Azure Active Directory (Azure AD) para autorizar comandos, asegúrese de que la entidad de seguridad tiene asignado el [rol Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Para más información sobre cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, vea [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="get-directory-and-file-permissions"></a>Obtención de permisos de archivo y directorio

Obtenga la ACL de un directorio o de un archivo con el cmdlet `Get-AzDataLakeGen2Item`.

En este ejemplo se obtiene la ACL de un **directorio** y luego se imprime la ACL en la consola.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```

En este ejemplo se obtiene la ACL de un **archivo** y luego se imprime la ACL en la consola.

```powershell
$filePath = "my-directory/upload.txt"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

En la siguiente imagen se muestra la salida después de obtener la ACL de un directorio.

![Obtención de una salida de ACL](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

En este ejemplo, el usuario propietario tiene permisos de lectura, escritura y ejecución. El grupo propietario tiene permisos de solo lectura y ejecución. Para más información sobre las listas de control de acceso, vea [Control de acceso en Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Establecimiento de permisos de archivo y directorio

Use el cmdlet `New-AzDataLakeGen2ItemAclObject` para crear una ACL para el usuario propietario, el grupo propietario u otros usuarios. Luego, use el cmdlet `Update-AzDataLakeGen2Item` para confirmar la ACL.

En este ejemplo se establece la ACL en un **directorio** del usuario propietario, el grupo propietario o de otros usuarios, y luego se imprime la ACL en la consola.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```
En este ejemplo se establece la ACL en un **archivo** del usuario propietario, el grupo propietario o de otros usuarios, y luego se imprime la ACL en la consola.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath -Acl $acl
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

En la siguiente imagen se muestra la salida después de establecer la ACL de un archivo.

![Obtención de una salida de ACL](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

En este ejemplo, el usuario propietario y el grupo propietario tienen permisos de solo lectura y escritura. Los demás usuarios tienen permisos de escritura y ejecución. Para más información sobre las listas de control de acceso, vea [Control de acceso en Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Actualización de permisos de archivo y directorio

Use el cmdlet `Get-AzDataLakeGen2Item` para obtener la ACL de un directorio o de un archivo. Luego, use el cmdlet `New-AzDataLakeGen2ItemAclObject` para crear una nueva entrada de ACL. Use el cmdlet `Update-AzDataLakeGen2Item` para aplicar la nueva ACL.

En este ejemplo se concede a un usuario permisos de escritura y ejecución en un directorio.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Get the directory ACL
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

# To avoid duplicate ACL, remove the ACL entries that will be added later.
foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "group" -and $a.DefaultScope -eq $true-and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}

# Add ACL Entries
$aclnew = New-AzDataLakeGen2ItemAclObject -AccessControlType group -EntityId $id -Permission "-wx" -DefaultScope -InputObject $aclnew

# Update ACL on server
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $aclnew  

```

En este ejemplo se concede a un usuario permisos de escritura y ejecución en un archivo.

```powershell
$filesystemName = "my-file-system"
$fileName = "my-directory/upload.txt"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Get the file ACL
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName).ACL

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

# To avoid duplicate ACL, remove the ACL entries that will be added later.
foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "group" -and $a.DefaultScope -eq $true-and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}

# Add ACL Entries
$aclnew = New-AzDataLakeGen2ItemAclObject -AccessControlType group -EntityId $id -Permission "-wx" -DefaultScope -InputObject $aclnew

# Update ACL on server
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName -Acl $aclnew 

```

### <a name="set-permissions-on-all-items-in-a-file-system"></a>Establecimiento de permisos en todos los elementos de un sistema de archivos

`Get-AzDataLakeGen2Item` y el parámetro `-Recurse` se pueden usar junto con el cmdlet `Update-AzDataLakeGen2Item` de forma recursiva para establecer la ACL de todos los directorios y archivos de un sistema de archivos. 

```powershell
$filesystemName = "my-file-system"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Recurse -FetchPermission | Update-AzDataLakeGen2Item -Acl $acl
```
<a id="gen1-gen2-map" />

## <a name="gen1-to-gen2-mapping"></a>Asignación de Gen1 a Gen2

En la siguiente tabla se refleja la asignación de cmdlets usados con Data Lake Storage Gen1 a cmdlets de Data Lake Storage Gen2.

|Cmdlet de Data Lake Storage Gen1| Cmdlet de Data Lake Storage Gen2| Notas |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|De forma predeterminada, el cmdlet Get-AzDataLakeGen2ChildItem solo muestra los elementos secundarios de primer nivel. El parámetro -Recurse muestra elementos secundarios de forma recursiva. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemOwner<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|Los elementos de salida del cmdlet Get-AzDataLakeGen2Item tiene estas propiedades: Acl, Owner, Group y Permission.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|El cmdlet Get-AzDataLakeGen2FileContent descarga el contenido del archivo en el archivo local.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|New-AzDataLakeStoreItem|New-AzDataLakeGen2Item|Este cmdlet carga el contenido del archivo nuevo desde un archivo local.|
|Remove-AzDataLakeStoreItem|Remove-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemOwner<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Update-AzDataLakeGen2Item|El cmdlet Update-AzDataLakeGen2Item actualiza solo un único elemento, y no de forma recursiva. Si desea actualizar de forma recursiva, muestre los elementos mediante el cmdlet Get-AzDataLakeStoreChildItem y, a continuación, canalice al cmdlet Update-AzDataLakeGen2Item.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|El cmdlet Get-AzDataLakeGen2Item notificará un error si el elemento no existe.|



## <a name="see-also"></a>Consulte también

* [Problemas conocidos](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Usar Azure PowerShell con Azure Storage](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Cmdlets de PowerShell de almacenamiento](/powershell/module/az.storage)

