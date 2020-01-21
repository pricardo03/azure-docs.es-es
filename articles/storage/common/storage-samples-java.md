---
title: Ejemplos de Azure Storage con Java | Microsoft Docs
description: Consulte, descargue y ejecute código de ejemplo y aplicaciones para Azure Storage. Descubra ejemplos introductorios de blobs, colas, tablas y archivos, usando las bibliotecas de cliente de almacenamiento Java.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 09/06/2019
ms.service: storage
ms.subservice: common
ms.topic: sample
ms.openlocfilehash: dfdde1ae981dcd2d539dec3667e44e90cef4d1c8
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75748033"
---
# <a name="azure-storage-samples-using-java"></a>Ejemplos de Azure Storage con Java

En la tabla siguiente se proporciona información general sobre el repositorio de ejemplos y los escenarios que abarca cada ejemplo. Haga clic en los vínculos para ver el código de ejemplo correspondiente en GitHub.

> [!NOTE]
> En estos ejemplos se usa la biblioteca Java v11 de Azure Storage. Para ver el código de v12, consulte [samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-blob/src/samples/java/com/azure/storage/blob) en el repositorio de GitHub.

## <a name="blob-samples-v11"></a>Ejemplos de blob (V11)

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Append Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Blob en bloques | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Cifrado de cliente | [Introducción a Cifrado de cliente de Azure en Java](https://github.com/Azure-Samples/storage-java-client-side-encryption) |
| Copia de blobs | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Create Container | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Delete Blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Delete Container | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Metadatos/propiedades/estadísticas de blobs | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) (Introducción a Blob service de Azure en Java) |
| ACL/metadatos/propiedades de contenedor | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) (Introducción a Blob service de Azure en Java) |
| Get Page Ranges | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399) (Introducción a Blob service de Azure en Java) |
| Contenedor/blob de concesión | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Contenedor/blob de lista | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| Blob en páginas | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |
| SAS | [Ejemplo de pruebas de SAS](https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513) |
| Propiedades de servicio | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java) (Introducción a Blob service de Azure en Java) |
| Instantánea de blob | [Getting Started with Azure Blob Service in Java](https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java) (Introducción a Blob service de Azure en Java) |

## <a name="file-samples-v11"></a>Ejemplos de archivo (v11)

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Crear recursos compartidos/directorios/archivos | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) (Introducción a Azure File service en Java) |
| Eliminar recursos compartidos/directorios/archivos | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) (Introducción a Azure File service en Java) |
| Metadatos/propiedades de directorio | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) (Introducción a Azure File service en Java) |
| Descargar archivos | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) (Introducción a Azure File service en Java) |
| Propiedades/metadatos/métricas de archivo | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) (Introducción a Azure File service en Java) |
| Propiedades de servicio de archivo | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) (Introducción a Azure File service en Java) |
| Directorios y archivos de lista | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) (Introducción a Azure File service en Java) |
| Recursos compartidos de lista | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java) (Introducción a Azure File service en Java) |
| Propiedades/metadatos/estadísticas de recurso compartido | [Getting Started with Azure File Service in Java](https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java) (Introducción a Azure File service en Java) |

## <a name="queue-samples-v11"></a>Ejemplos de cola (v11)

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Agregar mensaje | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63) |
| Cifrado de cliente | [Introducción a Cifrado de cliente de Azure en Java](https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java) |
| Crear colas | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| Eliminar mensaje/cola | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| Inspección de mensajes | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java) |
| ACL/metadatos/estadísticas de cola | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| Propiedades de Queue Service | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java) |
| Actualizar mensaje | [Introducción al servicio Cola de Azure en .NET](https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java)
|
## <a name="table-samples-v11"></a>Ejemplos de tabla (v11)

| **Escenario** | **Código de ejemplo** |
|--------------|-----------------|
| Crear tabla | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
| Eliminar entidad/tabla | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
| Insertar/combinar/reemplazar entidad | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
| Query Entities | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
| Consultar tablas | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
| ACL/propiedades de tabla | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java) (Introducción a Azure Table service en Java) |
| Update Entity | [Getting Started with Azure Table service in Java](https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java) (Introducción a Azure Table service en Java) |
## <a name="azure-code-samples-library"></a>Biblioteca de ejemplos de código de Azure

Para ver la biblioteca de completa de ejemplos, vaya a la biblioteca [Ejemplos de código de Azure](https://azure.microsoft.com/resources/samples/?service=storage), que incluye ejemplos para Azure Storage que puede descargar y ejecutar localmente. En la biblioteca de código de ejemplo se proporciona código de ejemplo en formato zip. Como alternativa, puede explorar y clonar el repositorio de GitHub para cada ejemplo.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Guías de introducción

Consulte las guías siguientes si busca instrucciones sobre cómo instalar las bibliotecas de cliente de Azure Storage y cómo empezar a usarlas.

* [Getting Started with Azure Blob Service in Java](../blobs/storage-quickstart-blobs-java.md) (Introducción a Blob service de Azure en Java)
* [Introducción al servicio Cola de Azure en .NET](../queues/storage-java-how-to-use-queue-storage.md)
* [Getting Started with Azure Table service in Java](../../cosmos-db/table-storage-how-to-use-java.md) (Introducción a Azure Table service en Java)
* [Getting Started with Azure File Service in Java](../files/storage-java-how-to-use-file-storage.md) (Introducción a Azure File service en Java)

## <a name="next-steps"></a>Pasos siguientes

Para información sobre ejemplos para otros lenguajes:

* .NET: [Azure Storage samples using .NET](storage-samples-dotnet.md) (Ejemplos de Azure Storage con .NET)
* JavaScript/Node.js: [Ejemplos de Azure Storage con JavaScript](storage-samples-javascript.md)
* Python: [Ejemplos de Azure Storage con Python](storage-samples-python.md)
* El resto de lenguajes: [Ejemplos de Azure Storage](storage-samples.md)
