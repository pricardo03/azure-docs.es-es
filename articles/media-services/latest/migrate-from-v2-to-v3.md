---
title: Migración de Azure Media Services v2 a v3 | Microsoft Docs
description: En este artículo se describen los cambios realizados en Azure Media Services v3 y se muestran las diferencias entre las dos versiones.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, transmisión, difusión, en vivo, sin conexión
ms.service: media-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: a17bad5beb651aaa085c626296c200a00c30647e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376369"
---
# <a name="migrate-from-media-services-v2-to-v3"></a>Migración de Media Services v2 a v3

En este artículo se describen los cambios realizados en Azure Media Services (AMS) v3 y se muestran las diferencias entre las dos versiones.

## <a name="why-should-you-move-to-v3"></a>¿Por qué debe migrar a v3?

### <a name="api-is-more-approachable"></a>La API es más cercana

*  v3 se basa en una superficie de API unificada que expone tanto la funcionalidad de administración como la de operaciones construidas sobre Azure Resource Manager. Las plantillas de Azure Resource Manager se pueden usar para crear e implementar transformaciones, puntos de conexión de streaming, LiveEvents, etc.
* Documento de la especificación Open API (también conocida como Swagger).
* Diferentes SDK disponibles para .NET, .Net Core, Node.js, Python, Java y Ruby.
* Integración con la CLI de Azure.

### <a name="new-features"></a>Nuevas características

* La codificación ahora admite ingesta HTTPS (entrada basada en direcciones URL).
* Las transformaciones son nuevas en v3. Una transformación se utiliza para compartir configuraciones, crear plantillas de Azure Resource Manager y aislar la configuración de codificación para un inquilino o cliente específico. 
* Un recurso puede tener varios objetos StreamingLocator con diferentes configuraciones de empaquetado dinámico y cifrado dinámico.
* La protección de contenido es compatible con características de varias claves. 
* LiveEvent.Preview admite empaquetado dinámico y cifrado dinámico. Esto habilita la protección de contenido en la vista previa, así como el empaquetado DASH y HLS.
* LiveOuput es más fácil de usar que la anterior entidad Program. 
* Se ha agregado compatibilidad con RBAC en las entidades.

## <a name="changes-from-v2"></a>Cambios desde la versión v2

* En Media Services v3, el cifrado de almacenamiento (cifrado con AES-256) solo es compatible con versiones anteriores si los recursos se crearon con Media Services v2. Esto significa que la versión v3 funciona con los recursos cifrados de almacenamiento ya existentes pero no permitirá la creación de otros nuevos.

    En el caso de recursos creados con v3, Media Services admite el cifrado de almacenamiento del lado servidor de [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
    
* Los SDK de Media Services se han desacoplado del SDK de Storage, lo que proporciona mayor control sobre el SDK de Storage utilizado y se evitan problemas de control de versiones. 
* En v3, todas las velocidades de bits de codificación se expresan en bits por segundo. Esto es diferente a los valores preestablecidos de Media Encoder Standard v2 de REST. Por ejemplo, la velocidad de bits en v2 se especificaría como 128, mientras que en v3 sería 128000. 
* En v3 no existen AssetFiles, AccessPolicies ni IngestManifests.
* Los ContentKeys ya no son una entidad, propiedad del objeto StreamingLocator.
* La compatibilidad con Event Grid reemplaza los NotificationEndpoints.
* Se cambió el nombre de algunas entidades

  * JobOutput reemplaza a Task, ahora parte del objeto Job.
  * LiveEvent reemplaza a Channel.
  * LiveOutput reemplaza a Program.
  * StreamingLocator reemplaza a Locator.

## <a name="code-changes"></a>Cambios de código

### <a name="create-an-asset-and-upload-a-file"></a>Crear un recurso y cargar un archivo 

#### <a name="v2"></a>v2

```csharp
IAsset asset = context.Assets.Create(assetName, storageAccountName, options);

IAssetFile assetFile = asset.AssetFiles.Create(assetFileName);

assetFile.Upload(filePath);
```

#### <a name="v3"></a>v3

```csharp
Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, permissions: AssetContainerPermission.ReadWrite, expiryTime: DateTime.Now.AddHours(1));

var sasUri = new Uri(response.AssetContainerSasUrls.First());
CloudBlobContainer container = new CloudBlobContainer(sasUri);

var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
blob.UploadFromFile(fileToUpload);
```

### <a name="submit-a-job"></a>Enviar un trabajo

#### <a name="v2"></a>v2

```csharp
IMediaProcessor processor = context.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

IJob job = jobs.Create($"Job for {inputAsset.Name}");

ITask task = job.Tasks.AddNew($"Task for {inputAsset.Name}", processor, taskConfiguration);

task.InputAssets.Add(inputAsset);

task.OutputAssets.AddNew(outputAssetName, outputAssetStorageAccountName, outputAssetOptions);

job.Submit();
```

#### <a name="v3"></a>v3

```csharp
client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, new Asset());

JobOutput[] jobOutputs = { new JobOutputAsset(outputAssetName)};

JobInput jobInput = JobInputAsset(assetName: assetName);

Job job = client.Jobs.Create(resourceGroupName,
accountName, transformName, jobName,
new Job {Input = jobInput, Outputs = jobOutputs});
```

### <a name="publish-an-asset-with-aes-encryption"></a>Publicar un recurso con cifrado AES 

#### <a name="v2"></a>v2

1. Crear ContentKeyAuthorizationPolicyOption
2. Crear ContentKeyAuthorizationPolicy
3. Crear AssetDeliveryPolicy
4. Crear Asset y cargar contenido o enviar trabajo y usar el recurso de salida
5. Asociar AssetDeliveryPolicy con Asset
6. Crear ContentKey
7. Adjuntar ContentKey a Asset
8. Crear AccessPolicy
9. Crear Locator

#### <a name="v3"></a>v3

1. Crear ContentKeyPolicy
2. Crear Asset
3. Cargar contenido o usar Asset como JobOutput
4. Creación de StreamingLocator

## <a name="next-steps"></a>Pasos siguientes

Para ver lo fácil que es empezar a codificar y hacer streaming de archivos de vídeo, consulte [Stream files](stream-files-dotnet-quickstart.md) (Streaming de vídeos). 

