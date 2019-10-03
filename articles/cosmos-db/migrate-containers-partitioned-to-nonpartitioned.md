---
title: Migración de contenedores de Azure Cosmos sin particiones a contenedores con particiones
description: Obtenga información sobre cómo migrar todos los contenedores sin particiones existentes en contenedores con particiones.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: mjbrown
ms.openlocfilehash: 77d70aaa9c1ae5a111a47e08f259c0ce95fd7c92
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300117"
---
# <a name="migrate-non-partitioned-containers-to-partitioned-containers"></a>Migrar contenedores sin particiones a contenedores con particiones

Azure Cosmos DB admite la creación de contenedores sin una clave de partición. Actualmente puede crear contenedores sin particiones utilizando la CLI de Azure y los SDK de Azure Cosmos DB (.NET, Java, NodeJs) que tienen una versión inferior o igual a 2.x. No se pueden crear contenedores sin particiones con Azure Portal. Sin embargo, dichos contenedores sin particiones no son elásticos y tienen una capacidad de almacenamiento fija de 10 GB y un límite de rendimiento de 10K RU/s.

Los contenedores sin particiones son heredados y debe migrar los contenedores sin particiones existentes a los contenedores con particiones para escalar el almacenamiento y el rendimiento. Azure Cosmos DB proporciona un mecanismo definido por el sistema para migrar los contenedores sin particiones a contenedores con particiones. En este documento se explica cómo todos los contenedores sin particiones existentes se migran automáticamente a contenedores con particiones. Puede aprovechar la característica de migración automática solo si usa la versión V3 de los SDK en todos los idiomas.

> [!NOTE]
> Actualmente, no puede migrar las cuentas de Azure Cosmos DB MongoDB y API de Gremlin mediante el uso de los pasos descritos en este documento.

## <a name="migrate-container-using-the-system-defined-partition-key"></a>Migración de contenedores con la clave de partición definida por el sistema

Para admitir la migración, Azure Cosmos DB define una clave de partición definida por el sistema denominada `/_partitionkey` en todos los contenedores que no tienen una clave de partición. No se puede cambiar la definición de la clave de partición después de haber migrado los contenedores. Por ejemplo, la definición de un contenedor que se migra a un contenedor con particiones será como sigue:

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

Después de migrar el contenedor, puede crear documentos rellenando la propiedad `_partitionKey` junto con las demás propiedades del documento. La propiedad `_partitionKey` representa la clave de partición de los documentos.

Elegir la clave de partición correcta es importante para usar de forma óptima el rendimiento aprovisionado. Para obtener más información, vea el artículo sobre [cómo elegir una clave de partición correcta](partitioning-overview.md).

> [!NOTE]
> Puede aprovechar la clave de partición definida por el sistema solo si usa la versión V3 o la más reciente de los SDK en todos los idiomas.

En el elemento siguiente se muestra un código de ejemplo para crear un documento con la clave de partición definida del sistema y leerlo:

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

    [JsonProperty(PropertyName = "_partitionKey", NullValueHandling = NullValueHandling.Ignore)]
    public string PartitionKey {get {return this.DeviceId; set; }
}

CosmosContainer migratedContainer = database.Containers["testContainer"];

DeviceInformationItem deviceItem = new DeviceInformationItem() {
  Id = "1234",
  DeviceId = "3cf4c52d-cc67-4bb8-b02f-f6185007a808"
}

ItemResponse<DeviceInformationItem > response = 
  await migratedContainer.CreateItemAsync<DeviceInformationItem>(
    deviceItem.PartitionKey, 
    deviceItem
  );

// Read back the document providing the same partition key
ItemResponse<DeviceInformationItem> readResponse = 
  await migratedContainer.ReadItemAsync<DeviceInformationItem>( 
    partitionKey:deviceItem.PartitionKey, 
    id: device.Id
  );

```

Para obtener un ejemplo completo, vea el repositorio de GitHub de [ejemplos de .Net](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples).
                      
## <a name="migrate-the-documents"></a>Migración de los documentos

Mientras que la definición del contenedor se mejora con una propiedad de clave de partición, los documentos del contenedor no se migran automáticamente. Lo que significa que la ruta `/_partitionKey` de la propiedad de la clave de partición del sistema no se agrega automáticamente a los documentos existentes. Deberá volver a particionar los documentos existentes mediante la lectura de los documentos que se crearon sin una clave de partición y volver a escribirlos con la propiedad `_partitionKey` en los documentos.

## <a name="access-documents-that-dont-have-a-partition-key"></a>Acceso a los documentos que no tienen una clave de partición

Las aplicaciones pueden tener acceso a los documentos existentes que no tienen una clave de partición mediante el uso de la propiedad del sistema especial denominada "CosmosContainerSettings.NonePartitionKeyValue", este es el valor de los documentos no migrados. Puede usar esta propiedad en todas las operaciones CRUD y de consulta. En el ejemplo siguiente se muestra un ejemplo para leer un documento único desde NonePartitionKey. 

```csharp
CosmosItemResponse<DeviceInformationItem> readResponse = 
await migratedContainer.Items.ReadItemAsync<DeviceInformationItem>( 
  partitionKey: PartitionKey.None, 
  id: device.Id
); 

```

Para obtener el ejemplo completo sobre cómo volver a particionar los documentos, vea el repositorio de GitHub de [ejemplos de .Net](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples). 

## <a name="compatibility-with-sdks"></a>Compatibilidad con SDK

Las versiones anteriores de los SDK de Azure Cosmos DB, como V2.x.x y V1.x.x, no admiten la propiedad de la clave de partición definida por el sistema. Por lo tanto, al leer la definición del contenedor de un SDK antiguo, no contiene ninguna definición de clave de partición y estos contenedores se comportarán exactamente igual que antes. Las aplicaciones compiladas con la versión anterior del SDK seguirán trabajando sin particiones tal cual sin realizar ningún cambio. 

Si un contenedor migrado es consumido por la versión más reciente/V3 de SDK y comienza a rellenar la clave de partición definida por el sistema dentro de los nuevos documentos, no podrá acceder (lectura, actualización, eliminación, consulta) a esos documentos desde los SDK anteriores.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de particiones en Azure Cosmos DB](partitioning-overview.md)
* [Unidades de solicitud en Azure Cosmos DB](request-units.md)
* [Aprovisionar rendimiento en contenedores y bases de datos](set-throughput.md)
* [Uso de la cuenta de Azure Cosmos](account-overview.md)