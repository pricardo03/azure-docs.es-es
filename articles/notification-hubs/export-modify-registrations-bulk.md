---
title: Exportar e importar registros de Azure Notification Hubs en masa | Microsoft Docs
description: Obtenga información sobre cómo usar el soporte técnico de forma masiva de Notification Hubs para realizar un gran número de operaciones en un centro de notificaciones, o para exportar todos los registros.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/18/2019
ms.author: jowargo
ms.openlocfilehash: a86c3bd85f9d611787a41754f49ee2475ba33a9a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175795"
---
# <a name="export-and-import-azure-notification-hubs-registrations-in-bulk"></a>Exportar e importar registros de Azure Notification Hubs en masa
Hay escenarios en los que se requiere para crear o modificar gran cantidad de registros en un centro de notificaciones. Algunos de estos escenarios son actualizaciones de etiqueta a la continuación de los cálculos por lotes, o migrar una implementación de inserción existente para usar Notification Hubs.

En este artículo se explica cómo realizar un gran número de operaciones en un centro de notificaciones o para exportar todos los registros, de forma masiva.

## <a name="high-level-flow"></a>Flujo de alto nivel
Soporte de batch está diseñado para admitir los trabajos de ejecución prolongada que implican millones de registros. Para lograr esta escala, soporte de batch usa Azure Storage para almacenar la salida y los detalles del trabajo. Para las operaciones de actualización masiva, el usuario es necesario para crear un archivo en un contenedor de blobs, cuyo contenido es la lista de las operaciones de actualización de registro. Cuando se inicia el trabajo, el usuario proporciona una dirección URL para el blob de entrada, junto con una dirección URL a un directorio de salida (también en un contenedor de blobs). Una vez iniciado el trabajo, el usuario puede comprobar el estado consultando una ubicación de la dirección URL proporcionada en el inicio del trabajo. Un trabajo específico solo puede realizar operaciones de un tipo específico (crea, actualiza o elimina). Las operaciones de exportación se realizan de forma análoga.

## <a name="import"></a>Importar

### <a name="set-up"></a>Instalación
En esta sección se supone que tiene las siguientes entidades:

- Un centro de notificaciones aprovisionado.
- Un contenedor de blobs de Azure Storage.
- Referencias a los paquetes de NuGet de Azure Service Bus y almacenamiento de Azure.

### <a name="create-input-file-and-store-it-in-a-blob"></a>Crear archivo de entrada y almacenarlo en un blob
Un archivo de entrada contiene una lista de registros que se serializan en XML, uno por fila. Mediante el SDK de Azure, en el ejemplo de código siguiente se muestra cómo serializar los registros y cargarlos en el contenedor de blobs.

```csharp
private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
{
    StringBuilder builder = new StringBuilder();
    foreach (var registrationDescription in descriptions)
    {
        builder.AppendLine(RegistrationDescription.Serialize());
    }

    var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
    using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
    {
        inputBlob.UploadFromStream(stream);
    }
}
```

> [!IMPORTANT]
> El código anterior serializa los registros en la memoria y, a continuación, carga toda la secuencia en un blob. Si ha cargado un archivo de más de unos cuantos megabytes, consulte las instrucciones de blobs de Azure para realizar estos pasos: Por ejemplo, [blobs en bloques](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).

### <a name="create-url-tokens"></a>Crear tokens de URL
Una vez cargado el archivo de entrada, genere las direcciones URL para proporcionar a su centro de notificaciones para el archivo de entrada y el directorio de salida. Puede usar dos contenedores de blob distinto para la entrada y salida.

```csharp
static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + sasContainerToken);
}

static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Read
    };
    string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + "/" + filePath + sasToken);
}
```

### <a name="submit-the-job"></a>Enviar el archivo
Con las dos URL de entrada y salida, ahora puede iniciar el trabajo por lotes.

```csharp
NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
var createTask = client.SubmitNotificationHubJobAsync(
new NotificationHubJob {
        JobType = NotificationHubJobType.ImportCreateRegistrations,
        OutputContainerUri = outputContainerSasUri,
        ImportFileUri = inputFileSasUri
    }
);
createTask.Wait();

var job = createTask.Result;
long i = 10;
while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
{
    var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
    getJobTask.Wait();
    job = getJobTask.Result;
    Thread.Sleep(1000);
    i--;
}
```

Además de las direcciones URL de entrada y salidas, este ejemplo se crea un `NotificationHubJob` objeto que contiene un `JobType` objeto, que puede ser uno de los siguientes tipos:

- `ImportCreateRegistrations`
- `ImportUpdateRegistrations`
- `ImportDeleteRegistrations`

Una vez que se complete la llamada, el centro de notificaciones continúa el trabajo y puede comprobar su estado con la llamada a [GetNotificationHubJobAsync](/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.getnotificationhubjobasync?view=azure-dotnet).

Al finalizar el trabajo, puede inspeccionar los resultados echando un vistazo a los siguientes archivos del directorio de salida:

- `/<hub>/<jobid>/Failed.txt`
- `/<hub>/<jobid>/Output.txt`

Estos archivos contienen la lista de operaciones correctas e incorrectas del lote. El formato de archivo es `.cvs`, en la que cada fila tiene el número de línea del archivo de entrada original y la salida de la operación (normalmente, la descripción del registro creado o actualizado).

### <a name="full-sample-code"></a>Código de ejemplo completo
El siguiente código de ejemplo importa los registros en un centro de notificaciones.

```csharp
using Microsoft.ServiceBus.Notifications;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Xml;

namespace ConsoleApplication1
{
    class Program
    {
        private static string CONNECTION_STRING = "---";
        private static string HUB_NAME = "---";
        private static string INPUT_FILE_NAME = "CreateFile.txt";
        private static string STORAGE_ACCOUNT = "---";
        private static string STORAGE_PASSWORD = "---";
        private static StorageUri STORAGE_ENDPOINT = new StorageUri(new Uri("---"));

        static void Main(string[] args)
        {
            var descriptions = new[]
            {
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMkUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMjUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMhUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMdUxREQFBlVTTkMwMQ"),
            };

            //write to blob store to create an input file
            var blobClient = new CloudBlobClient(STORAGE_ENDPOINT, new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(STORAGE_ACCOUNT, STORAGE_PASSWORD));
            var container = blobClient.GetContainerReference("testjobs");
            container.CreateIfNotExists();

            SerializeToBlob(container, descriptions);

            // TODO then create Sas
            var outputContainerSasUri = GetOutputDirectoryUrl(container);
            var inputFileSasUri = GetInputFileUrl(container, INPUT_FILE_NAME);


            //Lets import this file
            NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
            var createTask = client.SubmitNotificationHubJobAsync(
                new NotificationHubJob {
                    JobType = NotificationHubJobType.ImportCreateRegistrations,
                    OutputContainerUri = outputContainerSasUri,
                    ImportFileUri = inputFileSasUri
                }
            );
            createTask.Wait();

            var job = createTask.Result;
            long i = 10;
            while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
            {
                var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
                getJobTask.Wait();
                job = getJobTask.Result;
                Thread.Sleep(1000);
                i--;
            }
        }

        private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
        {
            StringBuilder builder = new StringBuilder();
            foreach (var registrationDescription in descriptions)
            {
                builder.AppendLine(RegistrationDescription.Serialize());
            }

            var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
            using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
            {
                inputBlob.UploadFromStream(stream);
            }
        }

        static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + sasContainerToken);
        }

        static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + "/" + filePath + sasToken);
        }

        static string GetJobPath(string namespaceName, string notificationHubPath, string jobId)
        {
            return string.Format(CultureInfo.InvariantCulture, @"{0}//{1}/{2}/", namespaceName, notificationHubPath, jobId);
        }
    }
}
```

## <a name="export"></a>Exportación
La exportación de registros es similar a la importación, con las siguientes diferencias:

- Solo necesita la dirección URL de salida.
- Crear un NotificationHubJob de tipo ExportRegistrations.

### <a name="sample-code-snippet"></a>Fragmento de código de ejemplo
Este es un fragmento de código de ejemplo para la exportación de registros en Java:

```java
// submit an export job
NotificationHubJob job = new NotificationHubJob();
job.setJobType(NotificationHubJobType.ExportRegistrations);
job.setOutputContainerUri("container uri with SAS signature");
job = hub.submitNotificationHubJob(job);

// wait until the job is done
while(true){
    Thread.sleep(1000);
    job = hub.getNotificationHubJob(job.getJobId());
    if(job.getJobStatus() == NotificationHubJobStatus.Completed)
        break;
}

```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre los registros, consulte los artículos siguientes:

- [Administración de registros](notification-hubs-push-notification-registration-management.md)
- [Etiquetas para los registros](notification-hubs-tags-segment-push-message.md)
- [Registros de plantilla](notification-hubs-templates-cross-platform-push-messages.md)
