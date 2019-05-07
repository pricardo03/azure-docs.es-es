---
title: Migrar sin particiones de contenedores de Azure Cosmos DB a contenedores con particiones
description: Obtenga información sobre cómo migrar todos los existentes sin particiones contenedores en contenedores con particiones.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: e8aaee8cb7df794652b2c560c5250921c1a4e060
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160641"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Migrar sin particiones de contenedores a contenedores con particiones

Azure Cosmos DB admite la creación de contenedores sin una clave de partición. Actualmente puede crear contenedores sin particiones utilizando la CLI de Azure y Azure Cosmos DB SDK (. NET, Java, NodeJs) que tienen una versión menor o igual que 2.x. No se puede crear contenedores sin particiones mediante el portal de Azure. Sin embargo, dichos contenedores sin particiones no son elásticos y han corregido la capacidad de almacenamiento del límite de 10 GB y el rendimiento de 10 K RU/s.

Los contenedores sin particiones son heredados y debe migrar los contenedores sin particiones existentes a contenedores con particiones para escalar el almacenamiento y rendimiento. Azure Cosmos DB proporciona un mecanismo definido por el sistema para migrar los contenedores sin particiones a contenedores con particiones. Este documento se explica cómo todos los existentes sin particiones contenedores se migrado automáticamente en contenedores con particiones. Puede aprovechar la característica de migración automática solo si usa la versión V3 de SDK en todos los idiomas.

> [!NOTE] 
> Actualmente, no puede migrar las cuentas de Azure Cosmos DB MongoDB y API de Gremlin mediante el uso de los pasos descritos en este documento. 

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Migración de contenedor mediante la clave de partición definido por el sistema

Para admitir la migración, Azure Cosmos DB define una clave de partición definido por el sistema denominada `/_partitionkey` en todos los contenedores que no tienen una clave de partición. No se puede cambiar la definición de clave de partición después de haber migran los contenedores. Por ejemplo, la definición de un contenedor que se migra a un contenedor con particiones será como sigue: 

```json
{
    "Id": "CollId" 
  "partitionKey": {
    "paths": [
      "/_partitionKey"
    ],
    "kind": "Hash"
  },
}
```
 
Después de migra el contenedor, puede crear documentos rellenando el `_partitionKey` propiedad junto con las demás propiedades del documento. El `_partitionKey` propiedad representa la clave de partición de los documentos. 

Elección de la clave de partición correcta es importante utilizar de forma óptima el rendimiento aprovisionado. Para obtener más información, consulte [cómo elegir una clave de partición](partitioning-overview.md) artículo. 

> [!NOTE]
> Puede aprovechar clave de partición definida del sistema solo si usa la versión más reciente o V3 de SDK en todos los idiomas.

El ejemplo siguiente muestra un código de ejemplo para crear un documento con la clave de partición definida del sistema y leer ese documento:

**Representación JSON del documento**

```csharp
DeviceInformationItem = new DeviceInformationItem
{
   "id": "elevator/PugetSound/Building44/Floor1/1",
   "deviceId": "3cf4c52d-cc67-4bb8-b02f-f6185007a808",
   "_partitionKey": "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

public class DeviceInformationItem
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }

    [JsonProperty(PropertyName = "deviceId")]
    public string DeviceId { get; set; }

    [JsonProperty(PropertyName = "_partitionKey")]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234", 
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
} 

CosmosItemResponse<DeviceInformationItem > response = 
  await migratedContainer.Items.CreateItemAsync(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
CosmosItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  ); 

```

Para obtener un ejemplo completo, vea el [ejemplos de .net](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) repositorio de GitHub. 
                      
## <a name="migrate-the-documents"></a>Migrar los documentos

Mientras que la definición del contenedor se ha mejorado con una propiedad de clave de partición, los documentos dentro del contenedor no automática migrado. Lo que significa que la propiedad de clave de partición de sistema `/_partitionKey` ruta de acceso no se agrega automáticamente a los documentos existentes. Deberá volver a particionar los documentos existentes mediante la lectura de los documentos que se crearon sin una clave de partición y vuelva a escribirlas con `_partitionKey` propiedad en los documentos. 

## <a name="access-documents-that-dont-have-a-partition-key"></a>Tener acceso a documentos que no tienen una clave de partición

Las aplicaciones pueden tener acceso a los documentos existentes que no tienen una clave de partición mediante el uso de la propiedad del sistema especial denominada "CosmosContainerSettings.NonePartitionKeyValue", este es el valor de los documentos no migrados. Puede utilizar esta propiedad en todas las operaciones CRUD y de consulta. El ejemplo siguiente muestra un ejemplo para leer un documento único desde el NonePartitionKey. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: CosmosContainerSettings.NonePartitionKeyValue, 
  id: device.Id
); 

```

Para obtener el ejemplo completo sobre cómo volver a particionar los documentos, consulte el [ejemplos de .net](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples) repositorio de GitHub. 

## <a name="compatibility-with-sdks"></a>Compatibilidad con SDK

Versión anterior del SDK de Azure Cosmos DB como V2.x.x y V1.x.x no admiten la propiedad de clave de partición definido por el sistema. Por lo tanto, al leer la definición del contenedor de un SDK antiguo, no contiene ninguna definición de clave de partición y estos contenedores se comportarán exactamente igual que antes. Las aplicaciones compiladas con la versión anterior del SDK de seguirán trabajando con sin particiones tal cual sin realizar ningún cambio. 

Si un contenedor migrado es consumido por la versión más reciente o V3 de SDK y empezar a rellenar la clave de partición definido por el sistema dentro de los nuevos documentos, no puede tener acceso (lectura, actualización, eliminación, consulta) esos documentos desde los SDK anteriores ya.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)
* [Aprovisionar rendimiento en contenedores y bases de datos](set-throughput.md)
* [Uso de la cuenta de Azure Cosmos](account-overview.md)