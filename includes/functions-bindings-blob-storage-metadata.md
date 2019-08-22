---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 3788fdb954917f28f64a0dfe035bed4ded0932d5
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641876"
---
El desencadenador de blobs proporciona varias propiedades de metadatos. Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estos valores tienen la misma semántica que el tipo [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet).

|Propiedad  |Escriba  |DESCRIPCIÓN  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Ruta de acceso del blob de desencadenamiento.|
|`Uri`|`System.Uri`|Es el identificador URI del blob correspondiente a la ubicación principal.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Son las propiedades del sistema del blob. |
|`Metadata` |`IDictionary<string,string>`|Son los metadatos definidos por el usuario para el blob.|

Por ejemplo, los ejemplos de JavaScript y el script de C# siguientes registran la ruta de acceso al blob de desencadenamiento, incluido el contenedor:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```
