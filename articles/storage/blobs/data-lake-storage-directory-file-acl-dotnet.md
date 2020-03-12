---
title: SDK de .NET para Azure Data Lake Storage Gen2 para archivos y ACL (versión preliminar)
description: Use la biblioteca cliente de Azure Storage con el fin de administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS).
author: normesta
ms.service: storage
ms.date: 01/09/2020
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 0c279118df3a9205e82f8444b261922c688578da
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "78969081"
---
# <a name="use-net-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Uso de .NET para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2 (versión preliminar)

En este artículo se muestra cómo usar .NET para crear y administrar directorios, archivos y permisos en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS). 

> [!IMPORTANT]
> El paquete NuGet [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake) que se incluye en este artículo se encuentra actualmente en versión preliminar pública.

[Paquete (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake) | [Ejemplos](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake) | [Referencia de API](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake) | [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md) | [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-net/issues)

## <a name="prerequisites"></a>Prerrequisitos

> [!div class="checklist"]
> * Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](data-lake-storage-quickstart-create-account.md) instrucciones para crear uno.

## <a name="set-up-your-project"></a>Configurar su proyecto

Para empezar, instale el paquete NuGet [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/).

Para más información sobre cómo instalar paquetes NuGet, consulte [Instalación y administración de paquetes en Visual Studio con el Administrador de paquetes NuGet](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio).

A continuación, agregue estas instrucciones USING al inicio del archivo de código.

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Storage.Files.DataLake.Models;
using Azure.Storage;
using System.IO;
using Azure;
```

## <a name="connect-to-the-account"></a>Conexión con la cuenta

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) que represente la cuenta de almacenamiento. La forma más fácil de obtener una es usar una clave de cuenta. 

En este ejemplo se crea una instancia de [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) mediante una clave de cuenta.

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient,
    string accountName, string accountKey)
{
    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient
        (new Uri(dfsUri), sharedKeyCredential);
}
```

## <a name="create-a-file-system"></a>Creación de un sistema de archivos

Un sistema de archivos actúa como contenedor de los archivos. Para crear uno puede llamar al método [DataLakeServiceClient.CreateFileSystem](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient.createfilesystemasync).

En este ejemplo se crea un sistema de archivos llamado `my-file-system`. 

```cs
public async Task<DataLakeFileSystemClient> CreateFileSystem
    (DataLakeServiceClient serviceClient)
{
        return await serviceClient.CreateFileSystemAsync("my-file-system");
}
```

## <a name="create-a-directory"></a>Creación de un directorio

Para crear una referencia de directorio, llame al método [DataLakeFileSystemClient.CreateDirectoryAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.createdirectoryasync).

En este ejemplo se agrega un directorio denominado `my-directory` a un sistema de archivos y luego se agrega un subdirectorio denominado `my-subdirectory`. 

```cs
public async Task<DataLakeDirectoryClient> CreateDirectory
    (DataLakeServiceClient serviceClient, string fileSystemName)
{
    DataLakeFileSystemClient fileSystemClient =
        serviceClient.GetFileSystemClient(fileSystemName);

    DataLakeDirectoryClient directoryClient =
        await fileSystemClient.CreateDirectoryAsync("my-directory");

    return await directoryClient.CreateSubDirectoryAsync("my-subdirectory");
}
```

## <a name="rename-or-move-a-directory"></a>Cambio de nombre o traslado de un directorio

Cambie el nombre de un directorio o muévalo con el método [DataLakeDirectoryClient.RenameAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.renameasync). Pase la ruta de acceso del directorio que busca a un parámetro. 

En este ejemplo se cambia el nombre de un subdirectorio a `my-subdirectory-renamed`.

```cs
public async Task<DataLakeDirectoryClient> 
    RenameDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory/my-subdirectory");

    return await directoryClient.RenameAsync("my-directory/my-subdirectory-renamed");
}
```

En este ejemplo se mueve un directorio denominado `my-subdirectory-renamed` a un subdirectorio de un directorio denominado `my-directory-2`. 

```cs
public async Task<DataLakeDirectoryClient> MoveDirectory
    (DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
            fileSystemClient.GetDirectoryClient("my-directory/my-subdirectory-renamed");

    return await directoryClient.RenameAsync("my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Eliminación de un directorio

Elimine un directorio o muévalo mediante una llamada al método [DataLakeDirectoryClient.Delete](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.delete).

En este ejemplo se elimina un directorio denominado `my-directory`.  

```cs
public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    directoryClient.Delete();
}
```

## <a name="manage-a-directory-acl"></a>Administración de una lista de control de acceso de directorio

Obtenga la lista de control de acceso (ACL) de un directorio al llamar al método [DataLakeDirectoryClient.GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.getaccesscontrolasync) y establezca dicha lista con una llamada al método [DataLakeDirectoryClient.SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakedirectoryclient.setaccesscontrollist).

> [!NOTE]
> Si la aplicación autoriza el acceso mediante Azure Active Directory (Azure AD), asegúrese de que la entidad de seguridad que la aplicación usa para autorizar el acceso tiene asignado el [rol de propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Para obtener más información acerca de cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control). 

En este ejemplo se obtiene y después se establece la ACL de un directorio denominado `my-directory`. La cadena `user::rwx,group::r-x,other::rw-` concede al usuario propietario permisos de lectura, escritura y ejecución, permisos de solo lectura y ejecución al grupo propietario, y permisos de lectura y escritura al resto.

```cs
public async Task ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        await directoryClient.GetAccessControlAsync();

    Console.WriteLine(directoryAccessControl.AccessControlList);

    IList<PathAccessControlItem> accessControlList
        = PathAccessControlExtensions.ParseAccessControlList
        ("user::rwx,group::r-x,other::rw-");

    directoryClient.SetAccessControlList(accessControlList);

}

```

## <a name="upload-a-file-to-a-directory"></a>Carga de un archivo en un directorio

En primer lugar, cree una referencia de archivo en el directorio de destino creando una instancia de la clase [DataLakeFileClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient). Cargue un archivo llamando al método [DataLakeFileClient.AppendAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.appendasync). Asegúrese de completar la carga llamando al método [DataLakeFileClient.FlushAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.flushasync).

En este ejemplo se carga un archivo de texto en un directorio denominado `my-directory`.    

```cs
public async Task UploadFile(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = await directoryClient.CreateFileAsync("uploaded-file.txt");

    FileStream fileStream = 
        File.OpenRead("C:\\file-to-upload.txt");

    long fileSize = fileStream.Length;

    await fileClient.AppendAsync(fileStream, offset: 0);

    await fileClient.FlushAsync(position: fileSize);

}
```

## <a name="manage-a-file-acl"></a>Administración de una lista de control de acceso de archivo

Obtenga la lista de control de acceso (ACL) de un archivo al llamar al método [DataLakeFileClient.GetAccessControlAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.getaccesscontrolasync) y establezca dicha lista con una llamada al método [DataLakeFileClient.SetAccessControlList](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.setaccesscontrollist).

> [!NOTE]
> Si la aplicación autoriza el acceso mediante Azure Active Directory (Azure AD), asegúrese de que la entidad de seguridad que la aplicación usa para autorizar el acceso tiene asignado el [rol de propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Para obtener más información acerca de cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control). 

En este ejemplo se obtiene y después se establece la ACL de un archivo denominado `my-file.txt`. La cadena `user::rwx,group::r-x,other::rw-` concede al usuario propietario permisos de lectura, escritura y ejecución, permisos de solo lectura y ejecución al grupo propietario, y permisos de lectura y escritura al resto.

```cs
public async Task ManageFileACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.GetFileClient("hello.txt");

    PathAccessControl FileAccessControl =
        await fileClient.GetAccessControlAsync();

    Console.WriteLine(FileAccessControl.AccessControlList);

    IList<PathAccessControlItem> accessControlList
        = PathAccessControlExtensions.ParseAccessControlList
        ("user::rwx,group::r-x,other::rw-");

    fileClient.SetAccessControlList(accessControlList);
}
```

## <a name="download-from-a-directory"></a>Descarga de un directorio 

En primer lugar, cree una instancia de [DataLakeFileClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient) que represente al archivo que quiere descargar. Use el método [DataLakeFileClient.ReadAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefileclient.readasync) y analice el valor devuelto para obtener un objeto [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream). Use cualquier archivo .NET que procese API para guardar bytes de la transmisión en un archivo. 

En este ejemplo se usa [BinaryReader](https://docs.microsoft.com/dotnet/api/system.io.binaryreader) y [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream) para guardar bytes en un archivo. 

```cs
public async Task DownloadFile(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.GetFileClient("my-image.png");

    Response<FileDownloadInfo> downloadResponse = await fileClient.ReadAsync();

    BinaryReader reader = new BinaryReader(downloadResponse.Value.Content);

    FileStream fileStream = 
        File.OpenWrite("C:\\my-image-downloaded.png");

    int bufferSize = 4096;

    byte[] buffer = new byte[bufferSize];

    int count;

    while ((count = reader.Read(buffer, 0, buffer.Length)) != 0)
    {
        fileStream.Write(buffer, 0, count);
    }

    await fileStream.FlushAsync();

    fileStream.Close();
}
```

## <a name="list-directory-contents"></a>Lista del contenido del directorio

Enumere el contenido de un directorio con una llamada al método [FileSystemClient.GetPathsAsync](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakefilesystemclient.getpathsasync) y, después, con la enumeración de los resultados.

En este ejemplo, se imprimen los nombres de cada uno de los archivos que se ubican en un directorio denominado `my-directory`.

```cs
public async Task ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient)
{
    IAsyncEnumerator<PathItem> enumerator = 
        fileSystemClient.GetPathsAsync("my-directory").GetAsyncEnumerator();

    await enumerator.MoveNextAsync();

    PathItem item = enumerator.Current;

    while (item != null)
    {
        Console.WriteLine(item.Name);

        if (!await enumerator.MoveNextAsync())
        {
            break;
        }
                
        item = enumerator.Current;
    }

}
```

## <a name="see-also"></a>Consulte también

* [Documentación de referencia de API](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake)
* [Paquete (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake)
* [Muestras](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake)
* [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md)
* [Problemas conocidos](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-net/issues)

