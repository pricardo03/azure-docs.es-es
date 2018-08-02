---
title: Establecimiento y recuperación de propiedades y metadatos de objetos en Azure Storage | Microsoft Docs
description: Almacenamiento de metadatos personalizados en los objetos de Azure Storage y establecimiento y recuperación de propiedades del sistema.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/16/2018
ms.author: tamram
ms.openlocfilehash: 2641e1653e14a7c101d95b02b8a5af71ceb9fdc6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398181"
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Establecimiento y recuperación de propiedades y metadatos

Objetos en las propiedades de compatibilidad de sistema de Azure Storage y metadatos definidos por el usuario, además de los datos que contienen. En este artículo se describen las propiedades del sistema de administración y los metadatos definidos por el usuario con la [biblioteca de cliente de Azure Storage para .NET](https://www.nuget.org/packages/WindowsAzure.Storage/).

* **Propiedades del sistema**: en cada recurso de almacenamiento existen propiedades del sistema. Algunas se pueden leer o establecer, mientras que otras son de solo lectura. En segundo plano, algunas propiedades del sistema corresponden a ciertos encabezados HTTP estándar. Las bibliotecas cliente de Azure Storage mantienen estas propiedades por usted.

* **Metadatos definidos por el usuario**: los metadatos definidos por el usuario se componen de uno o más pares nombre-valor que especifique para un recurso de Azure Storage. Puede usar metadatos para almacenar valores adicionales con un recurso. Los valores de metadatos se proporcionan para uso personal y no afectan a cómo se comporta el recurso.

El proceso de recuperación de los valores de propiedad y metadatos de un recurso de almacenamiento consta de dos pasos. Para poder leer estos valores, tiene que capturarlos explícitamente llamando al método **FetchAttributes** o **FetchAttributesAsync**. La excepción es si llama al método **Exists** o **ExistsAsync** en un recurso. Cuando se llama a uno de estos métodos, Azure Storage llama al método **FetchAttributes** adecuado en segundo plano como parte de la llamada al método **Exists**.

> [!IMPORTANT]
> Si descubre que no se han rellenado los valores de propiedad ni metadatos para un recurso de almacenamiento, compruebe que su código llama al método **FetchAttributes** o **FetchAttributesAsync**.
>
> Los pares nombre-valor de los metadatos solo pueden contener caracteres ASCII. Los pares de nombre/valor de metadatos son encabezados HTTP válidos y, por tanto, deben cumplir todas las restricciones que controlan los encabezados HTTP. Se recomienda que utilice la codificación de direcciones URL o codificación Base64 para nombres y valores que contengan caracteres no ASCII.
>

## <a name="setting-and-retrieving-properties"></a>Establecimiento y recuperación de propiedades
Para recuperar valores de propiedad, llame al método **FetchAttributesAsync** en el blob o contenedor para rellenar las propiedades y, a continuación, lea los valores.

Para establecer propiedades en un objeto, especifique el valor de propiedad y, a continuación, llame al método **SetProperties** .

En el ejemplo de código siguiente se crea un contenedor y se escriben algunos de sus valores de propiedad en una ventana de la consola.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
await container.FetchAttributesAsync();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Establecer y recuperar metadatos
Puede especificar metadatos como uno o más pares nombre-valor en un recurso de blob o contenedor. Para establecer los metadatos, agregue pares nombre-valor a la colección **Metadata** del recurso y, después, llame al método **SetMetadata** o **SetMetadataAsync** para guardar los valores en el servicio.

> [!NOTE]
> El nombre de los metadatos debe cumplir las convenciones de nomenclatura para los identificadores de C#.
>
>

El ejemplo de código siguiente establece los metadatos en un contenedor. Un valor se establece mediante el método **Agregar** de la colección. El otro valor se establece mediante la sintaxis implícita de clave/valor. Ambos son válidos.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    // Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    // Set the container's metadata.
    await container.SetMetadataAsync();
}
```

Para recuperar metadatos, llame al método **FetchAttributes** o **FetchAttributesAsync** en el blob o contenedor para rellenar la colección **Metadata** y después lea los valores tal como se muestra en el ejemplo siguiente.

```csharp
public static async Task ListContainerMetadataAsync(CloudBlobContainer container)
{
    // Fetch container attributes in order to populate the container's properties and metadata.
    await container.FetchAttributesAsync();

    // Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
* [Documentación de referencia de la biblioteca cliente de Azure Storage para .NET](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [Paquete de NuGet de la biblioteca cliente de Azure Storage para .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
