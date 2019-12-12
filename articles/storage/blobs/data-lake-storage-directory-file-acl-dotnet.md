---
title: Uso de Azure .NET con archivos y listas de control de acceso (ACL) en Azure Data Lake Storage Gen2 (versión preliminar)
description: Use la biblioteca cliente de Azure Storage con el fin de administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS).
author: normesta
ms.service: storage
ms.date: 11/24/2019
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: a756518688b5b1f8b854165de69d3444b772eabc
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74931218"
---
# <a name="use-net-for-files--acls-in-azure-data-lake-storage-gen2-preview"></a>Uso de .NET con archivos y listas de control de acceso (ACL) en Azure Data Lake Storage Gen2 (versión preliminar)

En este artículo se muestra cómo usar .NET para crear y administrar directorios, archivos y permisos en cuentas de almacenamiento que tengan habilitado el espacio de nombres jerárquico (HNS). 

> [!IMPORTANT]
> El paquete NuGet [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6) que se incluye en este artículo se encuentra actualmente en versión preliminar pública.

[Paquete (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6) | [Ejemplos](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake) | [Referencia de API](https://azuresdkdocs.blob.core.windows.net/$web/dotnet/Azure.Storage.Files.DataLake/12.0.0-preview.6/api/index.html) | [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md) | [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-net/issues)

## <a name="prerequisites"></a>Requisitos previos

> [!div class="checklist"]
> * Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](data-lake-storage-quickstart-create-account.md) instrucciones para crear uno.

## <a name="set-up-your-project"></a>Configurar su proyecto

Para empezar, instale el paquete NuGet [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/).

Para más información sobre cómo instalar paquetes NuGet, consulte [Instalación y administración de paquetes en Visual Studio con el Administrador de paquetes NuGet](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio).

A continuación, agregue estas instrucciones USING al inicio del archivo de código.

```csharp
using Azure.Storage.Files.DataLake;
using Azure.Storage.Files.DataLake.Models;
using System.IO;
using Azure;
```

## <a name="connect-to-the-account"></a>Conexión con la cuenta

Para usar los fragmentos de código de este artículo, tiene que crear una instancia de **DataLakeServiceClient** que represente la cuenta de almacenamiento. La forma más fácil de obtener una es usar una clave de cuenta. 

En este ejemplo se crea una instancia de **DataLakeServiceClient** mediante una clave de cuenta.

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

Un sistema de archivos actúa como contenedor de los archivos. Puede crear uno llamando al método **FileSystemClient.CreateFileSystemAsync**.

En este ejemplo se crea un sistema de archivos llamado `my-file-system`. 

```cs
public async Task<DataLakeFileSystemClient> CreateFileSystem
    (DataLakeServiceClient serviceClient)
{
        return await serviceClient.CreateFileSystemAsync("my-file-system");
}
```

## <a name="create-a-directory"></a>Creación de directorios

Cree una referencia de directorio llamando al método **FileSystemClient.CreateDirectoryAsync**.

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

Cambie el nombre de un directorio o muévalo llamando al método **DirectoryClient.RenameAsync**. Pase la ruta de acceso del directorio que busca a un parámetro. 

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

Elimine un directorio llamando al método **DirectoryClient.Delete**.

En este ejemplo se elimina un directorio denominado `my-directory`.  

```cs
public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    directoryClient.Delete();
}
```

## <a name="manage-a-directory-acl"></a>Administración de una lista de control de acceso de un directorio

Obtenga la lista de control de acceso (ACL) de un directorio llamando al método **directoryClient.GetAccessControlAsync** y establezca dicha lista con una llamada al método **DirectoryClient.SetAccessControl**.

En este ejemplo se obtiene y después se establece la ACL de un directorio denominado `my-directory`. La cadena `user::rwx,group::r-x,other::rw-` concede al usuario propietario permisos de lectura, escritura y ejecución, permisos de solo lectura y ejecución al grupo propietario, y permisos de lectura y escritura al resto.

```cs
public async Task ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient)
{
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.GetDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        await directoryClient.GetAccessControlAsync();

    Console.WriteLine(directoryAccessControl.Acl);

    directoryClient.SetAccessControl("user::rwx,group::r-x,other::rw-");

}

```

## <a name="upload-a-file-to-a-directory"></a>Carga de un archivo en un directorio

En primer lugar, cree una referencia de archivo en el directorio de destino creando una instancia de la clase **DataLakeFileClient**. Cargue un archivo llamando al método **DataLakeFileClient.AppendAsync**. Asegúrese de completar la carga llamando al método **DataLakeFileClient.FlushAsync**.

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

## <a name="manage-a-file-acl"></a>Administración de la lista de control de acceso de un archivo

Obtenga la lista de control de acceso (ACL) de un archivo llamando al método **DataLakeFileClient.GetAccessControlAsync** y establezca dicha lista con una llamada al método **FileClient.SetAccessControl**.

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

    Console.WriteLine(FileAccessControl.Acl);

    fileClient.SetAccessControl("user::rwx,group::r-x,other::rw-");
}
```

## <a name="download-from-a-directory"></a>Descarga de un directorio 

En primer lugar, cree una instancia de **DataLakeFileClient** que represente al archivo que desea descargar. Use el método **FileClient.ReadAsync** y analice el valor devuelto para obtener un objeto [Stream](https://docs.microsoft.com/dotnet/api/system.io.stream). Use cualquier archivo .NET que procese API para guardar bytes de la transmisión en un archivo. 

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

Enumere el contenido de un directorio con una llamada al método **FileSystemClient.ListPathsAsync** y, después, con la enumeración de los resultados.

En este ejemplo, se imprimen los nombres de cada uno de los archivos que se ubican en un directorio denominado `my-directory`.

```cs
public async Task ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient)
{
    IAsyncEnumerator<PathItem> enumerator = 
        fileSystemClient.ListPathsAsync("my-directory").GetAsyncEnumerator();

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

* [Documentación de referencia de API](https://azuresdkdocs.blob.core.windows.net/$web/dotnet/Azure.Storage.Files.DataLake/12.0.0-preview.6/api/index.html)
* [Paquete (NuGet)](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/12.0.0-preview.6)
* [Muestras](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake)
* [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Files.DataLake/GEN1_GEN2_MAPPING.md)
* [Problemas conocidos](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Envíenos sus comentarios](https://github.com/Azure/azure-sdk-for-net/issues)

