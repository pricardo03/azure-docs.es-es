---
title: Crear y usar archivos de recursos - Azure Batch | Microsoft Docs
description: Obtenga información sobre cómo crear archivos de recursos de Azure Batch desde diversos orígenes de entrada.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 03/14/2019
ms.author: lahugh
ms.openlocfilehash: 113faffb0ebac50a67c96ce21e0ee2c1564bb4fc
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405654"
---
# <a name="creating-and-using-resource-files"></a>Crear y usar archivos de recursos

A menudo, una tarea de Azure Batch requiere algún tipo de datos que se va a procesar. Archivos de recursos son los medios para proporcionar los datos a la máquina virtual de proceso por lotes (VM) a través de una tarea. Todos los tipos de tareas admiten archivos de recursos: inicio de tareas, tareas, tareas de preparación de trabajos, tareas de liberación de trabajos, etcetera. En este artículo se trata algunos métodos comunes sobre cómo crear archivos de recursos y colocarlos en una máquina virtual.  

Archivos de recursos son un mecanismo para colocar los datos en una máquina virtual en el lote, pero el tipo de datos y cómo se usa es flexible. Sin embargo, hay algunos casos de uso comunes:

1. Archivos comunes de aprovisionar en cada máquina virtual con archivos de recursos en una tarea de inicio
1. Aprovisionamiento de datos de entrada para ser procesados por tareas

Los archivos comunes que podrían ser, por ejemplo, los archivos en una tarea de inicio que se usa para instalar aplicaciones que ejecutan las tareas. Datos de entrada podrían ser cualquier información que va a procesar por lotes o datos de vídeo o imagen raw.

## <a name="types-of-resource-files"></a>Tipos de archivos de recursos

Hay varias opciones diferentes disponibles para generar archivos de recursos. El proceso de creación de archivos de recursos varía dependiendo de dónde se almacenan los datos originales.

Opciones para crear un archivo de recursos:

- [Dirección URL del contenedor de almacenamiento](#storage-container-url): Genera un archivo de recursos de cualquier contenedor de almacenamiento en Azure
- [Nombre del contenedor de almacenamiento](#storage-container-name): Genera un archivo de recursos del nombre de un contenedor en una cuenta de almacenamiento de Azure vinculada a Batch
- [Punto de conexión Web](#web-endpoint): Genera un archivo de recursos desde cualquier dirección URL HTTP válida

### <a name="storage-container-url"></a>Dirección URL del contenedor de almacenamiento

Mediante una dirección URL de contenedor de almacenamiento, significa que puede tener acceso a archivos en cualquier contenedor de almacenamiento en Azure con los permisos correctos.

En este C# ejemplo, los archivos ya se han cargado en un contenedor de Azure storage como almacenamiento de blobs. Para obtener acceso a los datos necesarios para crear un archivo de recursos, primero es necesario obtener acceso al contenedor de almacenamiento.

Crear una firma de acceso compartido (SAS) URI con los permisos correctos para tener acceso al contenedor de almacenamiento. Establezca la hora de expiración y los permisos para la SAS. En este caso, no se especifica ninguna hora de inicio, por lo que la SAS entra en vigor inmediatamente y expira dos horas después de generarlo.

```csharp
SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
{
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
    Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List
};
```

> [!NOTE]
> Para el acceso de contenedor, debe tener `Read` y `List` permisos, mientras que con el acceso de blob, solo necesitará `Read` permiso.

Una vez configurados los permisos, crear el token de SAS y dar formato a la dirección URL de SAS para el acceso al contenedor de almacenamiento. Con la dirección URL de SAS con formato para el contenedor de almacenamiento, generar un archivo de recursos con [ `FromStorageContainerUrl` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.resourcefile.fromstoragecontainerurl?view=azure-dotnet).

```csharp
CloudBlobContainer container = blobClient.GetContainerReference(containerName);

string sasToken = container.GetSharedAccessSignature(sasConstraints);
string containerSasUrl = String.Format("{0}{1}", container.Uri, sasToken);

ResourceFile inputFile = ResourceFile.FromStorageContainerUrl(containerSasUrl);
```

Es una alternativa para generar una dirección URL de SAS habilitar el acceso de lectura anónimo y público a un contenedor y sus blobs en Azure Blob storage. Al hacerlo, puede conceder acceso de solo lectura a estos recursos sin compartir la clave de cuenta y sin necesidad de una SAS. Acceso de lectura público se utiliza normalmente para escenarios donde desea que ciertos blobs estén siempre disponibles para el acceso de lectura anónimo. Si este escenario se adapta a su solución, consulte el [acceso anónimo a blobs](../storage/blobs/storage-manage-access-to-resources.md) artículo para obtener más información sobre la administración del acceso a los datos blob.

### <a name="storage-container-name"></a>Nombre del contenedor de almacenamiento

En lugar de configurar y crear una dirección URL de SAS, puede usar el nombre de su contenedor de almacenamiento de Azure para tener acceso a los datos blob. El contenedor de almacenamiento utilizada debe para en la cuenta de almacenamiento de Azure que esté vinculada a su cuenta de Batch, conocida como la cuenta autostorage. El nombre del contenedor de almacenamiento de una cuenta de autostorage permite omitir la configuración y la creación de una dirección URL de SAS para acceder a un contenedor de almacenamiento.

En este ejemplo, se supone que los datos que se usará para la creación del archivo de recursos ya están en una cuenta de Azure Storage vinculada a su cuenta de Batch. Si no tienes una cuenta de autostorage, consulte los pasos de [crear una cuenta de Batch](batch-account-create-portal.md) para obtener más información sobre cómo crear y vincular una cuenta.

Mediante el uso de una cuenta de almacenamiento vinculado, no es necesario crear y configurar una dirección URL de SAS para un contenedor de almacenamiento. En su lugar, proporcione el nombre del contenedor de almacenamiento en su cuenta de almacenamiento vinculada.

```csharp
ResourceFile inputFile = ResourceFile.FromAutoStorageContainer(containerName);
```

### <a name="web-endpoint"></a>Punto de conexión web

Datos que no se cargan en el almacenamiento de Azure todavía pueden utilizarse para crear archivos de recursos. Puede especificar cualquier dirección URL HTTP válida que contiene los datos de entrada. Se proporciona la dirección URL a la API de lote y, a continuación, los datos se utilizan para crear un archivo de recursos.

En la siguiente C# ejemplo, los datos de entrada se hospeda en el punto de conexión de GitHub ficticia. La API recupera el archivo desde el punto de conexión web válida y genera un archivo de recursos que será consumido por la tarea. No se requieren credenciales para este escenario.

```csharp
ResourceFile inputFile = ResourceFile.FromUrl("https://github.com/foo/file.txt", filePath);
```

## <a name="tips-and-suggestions"></a>Recomendaciones y sugerencias

Cada tarea de Batch de Azure usa archivos de forma diferente, motivo por el lote tiene las opciones disponibles para administrar archivos de tareas. Los escenarios siguientes no están diseñados para ser exhaustiva, pero en su lugar cubrir algunas situaciones comunes y ofreciendo recomendaciones.

### <a name="many-resource-files"></a>Muchos de los archivos de recursos

Un trabajo por lotes puede contener varias tareas que utilizan el mismos archivos comunes. Si los archivos de tareas comunes se comparten entre muchas tareas, con un paquete de aplicación para que contenga los archivos en lugar de usar archivos de recursos puede ser una mejor opción. Paquetes de aplicación proporcionan la optimización de la velocidad de descarga. Además, se almacena en caché los datos en paquetes de aplicación entre las tareas, por lo que si los archivos de la tarea no cambian a menudo, los paquetes de aplicaciones pueden ser una buena elección para la solución. Con paquetes de aplicación, no es necesario administrar varios archivos de recursos manualmente o generar direcciones URL de SAS para acceder a los archivos en el almacenamiento de Azure. Batch funciona en segundo plano con el almacenamiento de Azure para almacenar e implementar paquetes de aplicación para los nodos de proceso.

Si cada tarea tiene muchos archivos únicos para esa tarea, los archivos de recursos más probable están la mejor opción. Las tareas que utilizan a menudo archivos únicos deben actualizarse o reemplazarse, que no es tan fácil de hacer con el contenido de los paquetes de aplicación. Archivos de recursos proporcionan más flexibilidad para actualizar, agregar o modificar archivos individuales.

### <a name="number-of-resource-files-per-task"></a>Número de archivos de recursos por tarea

Si hay varios archivos de cientos de recursos especificados en una tarea, Batch puede rechazar la tarea para que ésta sea demasiado grande. Es mejor mantener las tareas pequeñas, ya que minimiza el número de archivos de recursos en la propia tarea.

Si no hay ninguna manera de minimizar el número de archivos de la tarea necesita, puede optimizar la tarea mediante la creación de un único archivo de recursos que hace referencia a un contenedor de almacenamiento de archivos de recursos. Para ello, coloque los archivos de recursos en un contenedor de almacenamiento de Azure y usar los diferentes modos de "Contenedor" de los archivos de recursos. Use las opciones de prefijo de blob para especificar las colecciones de los archivos se descargaran para sus tareas.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [paquetes de aplicación](batch-application-packages.md) como una alternativa a los archivos de recursos.
- Para obtener más información sobre el uso de contenedores de archivos de recursos, consulte [cargas de trabajo de contenedor](batch-docker-container-workloads.md).
- Para obtener información sobre cómo recopilar y guardar los datos de salida de las tareas, consulte [guardar las salidas de trabajos y tareas](batch-task-output.md).
- Obtenga información acerca de las [API y herramientas de Batch](batch-apis-tools.md) disponibles para la creación de soluciones de Batch.
