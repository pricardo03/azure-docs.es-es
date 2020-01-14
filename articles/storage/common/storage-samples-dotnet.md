---
title: Ejemplos de Azure Storage con .NET | Microsoft Docs
description: Consulte, descargue y ejecute código de ejemplo y aplicaciones para Azure Storage. Descubra ejemplos introductorios de blobs, colas, tablas y archivos, usando las bibliotecas de cliente de almacenamiento .NET.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/06/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: 043a21aec5753dd16f558ad29d2300ada7a35d72
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431777"
---
# <a name="azure-storage-samples-using-net"></a>Ejemplos de Azure Storage con .NET

En la tabla siguiente se proporciona información general sobre el repositorio de ejemplos y los escenarios que abarca cada ejemplo. Haga clic en los vínculos para ver el código de ejemplo correspondiente en GitHub.

> [!NOTE]
> En estos ejemplos se usa la biblioteca .NET v11 de Azure Storage. Para ver el código de v12, consulte [samples](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples) en el repositorio de GitHub.

## <a name="blob-samples"></a>Ejemplos de blob

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Append Blob | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs#L1144) |
| Blob en bloques | [Azure Blob Storage Photo Gallery Web Application (Aplicación web de la galería de fotos de Almacenamiento de blobs de Azure)](https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs) |
| Cifrado de cliente | [Ejemplos de cifrado de blobs](https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs) |
| Copia de blobs | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Create Container | [Azure Blob Storage Photo Gallery Web Application (Aplicación web de la galería de fotos de Almacenamiento de blobs de Azure)](https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs) |
| Delete Blob | [Azure Blob Storage Photo Gallery Web Application (Aplicación web de la galería de fotos de Almacenamiento de blobs de Azure)](https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs) |
| Delete Container | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Metadatos/propiedades/estadísticas de blobs | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| ACL/metadatos/propiedades de contenedor | [Azure Blob Storage Photo Gallery Web Application (Aplicación web de la galería de fotos de Almacenamiento de blobs de Azure)](https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs) |
| Get Page Ranges | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Contenedor/blob de concesión | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Contenedor/blob de lista | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs) |
| Blob en páginas | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs) |
| SAS | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Propiedades de servicio | [Introducción a blobs](https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs) |
| Instantánea de blob | [Backup Azure Virtual Machine Disks with Incremental Snapshots](https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs) (Copias de seguridad de discos de máquinas virtuales de Azure con instantáneas incrementales) |

## <a name="file-samples"></a>Ejemplos de archivo

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Crear recursos compartidos/directorios/archivos | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs) |
| Eliminar recursos compartidos/directorios/archivos | [Getting Started with Azure File Service in .NET (Introducción al servicio Archivo de Azure en .NET)](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs) |
| Metadatos/propiedades de directorio | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs) |
| Descargar archivos | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs) |
| Propiedades/metadatos/métricas de archivo | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs) |
| Propiedades de servicio de archivo | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs) |
| Directorios y archivos de lista | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs) |
| Recursos compartidos de lista | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs) |
| Propiedades/metadatos/estadísticas de recurso compartido | [Ejemplo de File Storage .NET de Azure Storage](https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs) |

## <a name="queue-samples"></a>Ejemplos de cola

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Agregar mensaje | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs) |
| Cifrado de cliente | [Cifrado de cliente de Cola de Azure Storage en .NET](https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs) |
| Crear colas | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs) |
| Eliminar mensaje/cola | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs) |
| Inspección de mensajes | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs) |
| ACL/metadatos/estadísticas de cola | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs) |
| Propiedades de Queue Service | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs) |
| Actualizar mensaje | [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs) |

## <a name="table-samples"></a>Ejemplos de tabla

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Crear tabla | [Administración de simultaneidad con Almacenamiento de Azure: aplicación de ejemplo](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262) |
| Eliminar entidad/tabla | [Introducción a Azure Table Storage mediante .NET](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs) |
| Insertar/combinar/reemplazar entidad | [Administración de simultaneidad con Almacenamiento de Azure: aplicación de ejemplo](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262) |
| Query Entities | [Introducción a Azure Table Storage mediante .NET](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs) |
| Consultar tablas | [Introducción a Azure Table Storage mediante .NET](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs) |
| ACL/propiedades de tabla | [Introducción a Azure Table Storage mediante .NET](https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs) |
| Update Entity | [Administración de simultaneidad con Almacenamiento de Azure: aplicación de ejemplo](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262) |

## <a name="azure-code-samples-library"></a>Biblioteca de ejemplos de código de Azure

Para ver la biblioteca de completa de ejemplos, vaya a la biblioteca [Ejemplos de código de Azure](https://azure.microsoft.com/resources/samples/?service=storage), que incluye ejemplos para Azure Storage que puede descargar y ejecutar localmente. En la biblioteca de código de ejemplo se proporciona código de ejemplo en formato zip. Como alternativa, puede explorar y clonar el repositorio de GitHub para cada ejemplo.

[!INCLUDE [storage-dotnet-samples-include](../../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a>Guías de introducción

Consulte las guías siguientes si busca instrucciones sobre cómo instalar las bibliotecas de cliente de Azure Storage y cómo empezar a usarlas.

* [Getting Started with Azure Blob Service in .NET (Introducción al servicio BLOB de Azure en .NET)](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Getting Started with Azure Queue Service in .NET (Introducción al servicio Cola de Azure en .NET)](../storage-dotnet-how-to-use-queues.md)
* [Getting Started with Azure Table Service in .NET (Introducción al servicio Tabla de Azure en .NET)](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Getting Started with Azure File Service in .NET (Introducción al servicio Archivo de Azure en .NET)](../storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a>Pasos siguientes

Para información sobre ejemplos para otros lenguajes:

* Java: [Azure Storage samples using Java](storage-samples-java.md) (Ejemplos de Azure Storage con Java)
* JavaScript/Node.js: [Ejemplos de Azure Storage con JavaScript](storage-samples-javascript.md)
* Python: [Ejemplos de Azure Storage con Python](storage-samples-python.md)
* El resto de lenguajes: [Ejemplos de Azure Storage](../storage-samples.md)
