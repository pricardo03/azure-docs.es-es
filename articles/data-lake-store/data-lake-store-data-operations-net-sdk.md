---
title: '.NET SDK: operaciones del sistema de archivos en Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Use el SDK de .NET de Azure Data Lake Storage Gen1 para realizar operaciones del sistema de archivos en Data Lake Storage Gen1, como, por ejemplo, crear carpetas.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 57f4485e70bf91713539b3398fc93d6810c3c28e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163247"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>Operaciones del sistema de archivos en Azure Data Lake Storage Gen1 mediante el uso del SDK de .NET
> [!div class="op_single_selector"]
> * [SDK de .NET](data-lake-store-data-operations-net-sdk.md)
> * [SDK de Java](data-lake-store-get-started-java-sdk.md)
> * [API DE REST](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Con este artículo aprenderá a realizar operaciones del sistema de archivos en Data Lake Storage Gen1 con el SDK de .NET. Las operaciones de sistema de archivos incluyen, por ejemplo, la creación de carpetas en una cuenta de Data Lake Storage Gen1 o la carga y descarga de archivos.

Para instrucciones sobre cómo realizar operaciones de administración de cuentas en Data Lake Storage Gen1 con el SDK de .NET, consulte [Operaciones de administración de cuentas en Data Lake Storage Gen1 con el SDK de .NET](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Requisitos previos
* **Visual Studio 2013, 2015 o 2017**. En las instrucciones siguientes se usa Visual Studio 2017.

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).

* **Cuenta de Azure Data Lake Storage Gen1**. Para obtener instrucciones acerca de cómo crear una cuenta, consulte [Introducción a Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Creación de una aplicación .NET
El ejemplo de código disponible [en GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) le guía a través del proceso de creación de archivos en el almacén, concatenación de archivos, descarga de un archivo y eliminación de algunos archivos en el almacén. Esta sección del artículo le guía a través de las principales partes del código.

1. Abra Visual Studio y cree una aplicación de consola.
2. En el menú **Archivo**, haga clic en **Nuevo** y en **Proyecto**.
3. En **Nuevo proyecto**, escriba o seleccione los siguientes valores:

   | Propiedad | Valor |
   | --- | --- |
   | Categoría |Plantillas/Visual C#/Windows |
   | Plantilla |Aplicación de consola |
   | NOMBRE |CreateADLApplication |

4. Haga clic en **Aceptar** para crear el proyecto.

5. Agregue los paquetes NuGet al proyecto.

   1. Haga clic con el botón derecho en el Explorador de soluciones y haga clic en **Administrar paquetes de NuGet**.
   2. En la pestaña **Administrador de paquetes NuGet**, asegúrese de que la opción **Origen del paquete** esté establecida en **nuget.org** y que esté activada la casilla **Incluir versión preliminar**.
   3. Busque e instale los siguientes paquetes NuGet:

      * `Microsoft.Azure.DataLake.Store`: en este tutorial se usa v1.0.0.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: en este tutorial se usa v2.3.1.
    
    Cierre el **Administrador de paquetes NuGet**.

6. Abra **Program.cs**, elimine el código existente e incluya las siguientes instrucciones para agregar referencias a espacios de nombres.

        using System;
        using System.IO;using System.Threading;
        using System.Linq;
        using System.Text;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.DataLake.Store;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Declare las variables como se indica a continuación y proporcione los valores de los marcadores de posición. Además, asegúrese de que la ruta de acceso local y el nombre de archivo que proporciona existen en el equipo.

        namespace SdkSample
        {
            class Program
            {
                private static string _adlsg1AccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net";        
            }
        }

En las restantes secciones de este artículo, se puede ver cómo se utilizan los métodos .NET disponibles para realizar operaciones como la autenticación, la carga de archivos, etc.

## <a name="authentication"></a>Autenticación

* Para la autenticación del usuario final para la aplicación, consulte [End-user authentication with Data Lake Storage Gen1 using .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md) (Autenticación del usuario final con Data Lake Storage Gen1 mediante el SDK de .NET).
* Para la autenticación entre servicios para la aplicación, consulte [Service-to-service authentication with Data Lake Storage Gen1 using .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md) (Autenticación entre servicios con Data Lake Storage Gen1 mediante el SDK de .NET).


## <a name="create-client-object"></a>Creación del objeto de cliente
El fragmento de código siguiente crea los objetos de cliente del sistema de archivos de Data Lake Storage Gen1, que se usan para emitir solicitudes al servicio.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsg1AccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Creación de un archivo y un directorio
Agregue el siguiente fragmento de código a su aplicación. Este fragmento de código agrega un archivo, así como los directorios principales que no existen.

    // Create a file - automatically creates any parent directories that don't exist
    // The AdlsOutputStream preserves record boundaries - it does not break records while writing to the store
    using (var stream = client.CreateFile(fileName, IfExists.Overwrite))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is test data to write.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);

        textByteArray = Encoding.UTF8.GetBytes("This is the second line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="append-to-a-file"></a>Anexión a un archivo
El fragmento de código siguiente anexa datos a un archivo existente en la cuenta de Data Lake Storage Gen1.

    // Append to existing file
    using (var stream = client.GetAppendStream(fileName))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="read-a-file"></a>Lectura de un archivo
El fragmento de código siguiente lee el contenido de un archivo en Data Lake Storage Gen1.

    //Read file contents
    using (var readStream = new StreamReader(client.GetReadStream(fileName)))
    {
        string line;
        while ((line = readStream.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

## <a name="get-file-properties"></a>Obtención de las propiedades del archivo
El fragmento de código siguiente devuelve las propiedades asociadas a un archivo o un directorio.

    // Get file properties
    var directoryEntry = client.GetDirectoryEntry(fileName);
    PrintDirectoryEntry(directoryEntry);

La definición del método `PrintDirectoryEntry` está disponible como parte del ejemplo [en GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted). 

## <a name="rename-a-file"></a>Cambio del nombre de un archivo
El fragmento de código siguiente cambia el nombre de un archivo existente en una cuenta de Data Lake Storage Gen1.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Enumeración de un directorio
El fragmento de código siguiente enumera los directorios en una cuenta de Data Lake Storage Gen1

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

La definición del método `PrintDirectoryEntry` está disponible como parte del ejemplo [en GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted).

## <a name="delete-directories-recursively"></a>Eliminación de directorios de forma recursiva
El fragmento de código siguiente elimina recursivamente un directorio y todos sus subdirectorios.

    // Delete a directory and all its subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Ejemplos
Aquí se muestran un par de ejemplos sobre cómo usar el SDK del sistema de archivos de Data Lake Storage Gen1.
* [Ejemplo básico en GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Ejemplo avanzado en GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Otras referencias
* [Operaciones de administración de cuentas en Data Lake Storage Gen1 con el SDK de .NET](data-lake-store-get-started-net-sdk.md)
* [Data Lake Storage Gen1 .NET SDK Reference](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet) (Referencia del SDK de .NET de Data Lake Storage Gen1)

## <a name="next-steps"></a>Pasos siguientes
* [Protección de datos en Data Lake Storage Gen1](data-lake-store-secure-data.md)
