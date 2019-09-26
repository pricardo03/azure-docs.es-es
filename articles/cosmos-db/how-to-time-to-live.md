---
title: Aprenda a configurar y administrar el período de vida en Azure Cosmos DB
description: Aprenda a configurar y administrar el período de vida en Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: mjbrown
ms.openlocfilehash: ddda7b96147892efb38cb0405120db3613e98cf8
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104869"
---
# <a name="configure-time-to-live-in-azure-cosmos-db"></a>Configuración del período de vida en Azure Cosmos DB

En Azure Cosmos DB, puede elegir configurar el período de vida (TTL) en el nivel de contenedor, o puede invalidarlo en un nivel de elemento después de configurarlo para el contenedor. Puede configurar el período de vida de un contenedor mediante Azure Portal o los SDK específicos del lenguaje. Las invalidaciones del período de vida en el nivel de elemento se pueden configurar mediante los SDK.

## <a name="enable-time-to-live-on-a-container-using-azure-portal"></a>Habilitar el período de vida en un contenedor mediante Azure Portal

Siga estos pasos para habilitar el período de vida de un contenedor para que no expire. Habilite esta opción para permitir que el período de vida se pueda invalidar en el nivel de elemento. También puede establecer el período de vida especificando un valor distinto a cero segundos.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Cree una cuenta de Azure Cosmos DB o seleccione una ya existente.

3. Abra el panel **Explorador de datos**.

4. Seleccione un contenedor existente, expándalo y modifique los valores siguientes:

   * Abra la ventana **Escala y configuración**.
   * En **Configuración** busque, **Período de vida**.
   * Seleccione **Activado (valor no predeterminado)** o seleccione **Activado** y establezca un valor para el período de vida
   * Haga clic en **Guardar** para guardar los cambios.

   ![Configuración del período de vida en Azure Portal](./media/how-to-time-to-live/how-to-time-to-live-portal.png)


- Si el valor de DefaultTimeToLive es null, el de Período de vida será Desactivado
- Si el valor de DefaultTimeToLive es -1, el de Período de vida será Activado (valor no predeterminado)
- Si DefaultTimeToLive tiene cualquier otro valor Int (excepto el 0) el de Período de vida será Activado

## <a name="enable-time-to-live-on-a-container-using-sdk"></a>Habilitar el período de vida en un contenedor mediante SDK

### <a id="dotnet-enable-noexpiry"></a>SDK de .NET V2 (Microsoft.Azure.DocumentDB)

```csharp
// Create a new container with TTL enabled and without any expiration value
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = -1; //(never expire by default)

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition);
```

### <a id="dotnet-enable-noexpiry"></a>SDK de .NET V3 (Microsoft.Azure.Cosmos)

```csharp
// Create a new container with TTL enabled and without any expiration value
await client.GetDatabase("database").CreateContainerAsync(new ContainerProperties
{
    Id = "container",
    PartitionKeyPath = "/myPartitionKey",
    DefaultTimeToLive = -1 //(never expire by default)
});
```

## <a name="set-time-to-live-on-a-container-using-sdk"></a>Establecimiento del período de vida en un contenedor mediante SDK

Para establecer el período de vida en un contenedor, deberá proporcionar un número positivo distinto de cero que indique el período de tiempo en segundos. En función del valor del período de vida configurado, se eliminarán todos los elementos del contenedor posteriores a la última marca de tiempo modificada del elemento `_ts`.

### <a id="dotnet-enable-withexpiry"></a>SDK de .NET V2 (Microsoft.Azure.DocumentDB)

```csharp
// Create a new container with TTL enabled and a 90 day expiration
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition;
```

### <a id="dotnet-enable-withexpiry"></a>SDK de .NET V3 (Microsoft.Azure.Cosmos)

```csharp
// Create a new container with TTL enabled and a 90 day expiration
await client.GetDatabase("database").CreateContainerAsync(new ContainerProperties
{
    Id = "container",
    PartitionKeyPath = "/myPartitionKey",
    DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
});
```

### <a id="nodejs-enable-withexpiry"></a>SDK para NodeJS

```javascript
const containerDefinition = {
          id: "sample container1",
        };

async function createcontainerWithTTL(db: Database, containerDefinition: ContainerDefinition, collId: any, defaultTtl: number) {
      containerDefinition.id = collId;
      containerDefinition.defaultTtl = defaultTtl;
      await db.containers.create(containerDefinition);
}
```

## <a name="set-time-to-live-on-an-item"></a>Establecimiento del período de vida en un elemento

Además de establecer un período de vida predeterminado en un contenedor, también puede establecerlo para un elemento. Si establece el período de vida en el nivel de elemento, se reemplazará el período de vida predeterminado del elemento en ese contenedor.

* Para establecer el período de vida en un elemento, debe proporcionar un número positivo distinto de cero que indica el período, en segundos, para hacer que expire el elemento después de la marca de tiempo de última modificación del elemento (`_ts`).

* Si el elemento no tiene un campo para el período de vida, se aplicará de forma predeterminada al elemento el valor del período de vida establecido en el contenedor.

* Si el TTL se deshabilita en el nivel de contenedor, se omitirá el campo TTL en el elemento hasta que se vuelva a habilitar el TTL en el contenedor.

### <a id="portal-set-ttl-item"></a>Azure Portal

Use estos pasos para habilitar el período de vida en un elemento:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Cree una cuenta de Azure Cosmos DB o seleccione una ya existente.

3. Abra el panel **Explorador de datos**.

4. Seleccione un contenedor existente, expándalo y modifique los valores siguientes:

   * Abra la ventana **Escala y configuración**.
   * En **Configuración** busque, **Período de vida**.
   * Seleccione **Activado (valor no predeterminado)** o seleccione **Activado** y establezca un valor para el período de vida. 
   * Haga clic en **Guardar** para guardar los cambios.

5. A continuación, vaya al elemento para el que quiere establecer el período de vida, agregue la propiedad `ttl` y seleccione **Actualizar**. 

   ```json
   {
    "id": "1",
    "_rid": "Jic9ANWdO-EFAAAAAAAAAA==",
    "_self": "dbs/Jic9AA==/colls/Jic9ANWdO-E=/docs/Jic9ANWdO-EFAAAAAAAAAA==/",
    "_etag": "\"0d00b23f-0000-0000-0000-5c7712e80000\"",
    "_attachments": "attachments/",
    "ttl": 10,
    "_ts": 1551307496
   }
   ```

### <a id="dotnet-set-ttl-item"></a>SDK de .NET (cualquiera)

```csharp
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName="cid")]
    public string CustomerId { get; set; }
    // used to set expiration policy
    [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
    public int? ttl { get; set; }

    //...
}
// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder
{
    Id = "SO05",
    CustomerId = "CO18009186470",
    ttl = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days
};
```

### <a id="nodejs-set-ttl-item"></a>SDK para NodeJS

```javascript
const itemDefinition = {
          id: "doc",
          name: "sample Item",
          key: "value", 
          ttl: 2
        };
```


## <a name="reset-time-to-live"></a>Restablecimiento del período de vida

Puede restablecer el período de vida en un elemento mediante la realización de una operación de escritura o actualización en él. La operación de escritura o actualización establecerá `_ts` en la hora actual, y el período de vida para el elemento que va a expirar comenzará de nuevo. Si desea cambiar el período de vida de un elemento, puede actualizar el campo al igual que actualizaría cualquier otro campo.

### <a id="dotnet-extend-ttl-item"></a>SDK de .NET V2 (Microsoft.Azure.DocumentDB)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = 60 * 30 * 30; // update time to live
response = await client.ReplaceDocumentAsync(readDocument);
```

### <a id="dotnet-extend-ttl-item"></a>SDK de .NET V3 (Microsoft.Azure.Cosmos)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
ItemResponse<SalesOrder> itemResponse = await client.GetContainer("database", "container").ReadItemAsync<SalesOrder>("SO05", new PartitionKey("CO18009186470"));

itemResponse.Resource.ttl = 60 * 30 * 30; // update time to live
await client.GetContainer("database", "container").ReplaceItemAsync(itemResponse.Resource, "SO05");
```

## <a name="turn-off-time-to-live"></a>Desactivación del período de vida

Si se ha establecido el período de vida en un elemento y ya no desea que ese elemento expire, puede obtener el elemento, quitar el campo TTL y reemplazar el elemento en el servidor. Si quita el campo TTL del elemento, se aplicará a este el valor predeterminado del período de vida asignado al contenedor. Establezca el valor del período de vida en -1 para evitar que un elemento expire y no herede el valor del período de vida del contenedor.

### <a id="dotnet-turn-off-ttl-item"></a>SDK de .NET V2 (Microsoft.Azure.DocumentDB)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = null; // inherit the default TTL of the container

response = await client.ReplaceDocumentAsync(readDocument);
```

### <a id="dotnet-turn-off-ttl-item"></a>SDK de .NET V3 (Microsoft.Azure.Cosmos)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
ItemResponse<SalesOrder> itemResponse = await client.GetContainer("database", "container").ReadItemAsync<SalesOrder>("SO05", new PartitionKey("CO18009186470"));

itemResponse.Resource.ttl = null; // inherit the default TTL of the container
await client.GetContainer("database", "container").ReplaceItemAsync(itemResponse.Resource, "SO05");
```

## <a name="disable-time-to-live"></a>Deshabilitar el período de vida

Para deshabilitar el período de vida en un contenedor e impedir que el proceso en segundo plano compruebe los elementos expirados, se debe eliminar la propiedad `DefaultTimeToLive` del contenedor. Eliminar esta propiedad es diferente de establecerla en -1. Si la establece en -1, los nuevos elementos que se agreguen al contenedor no expirarán nunca. No obstante, puede invalidar este valor en elementos específicos del contenedor. Si elimina la propiedad TTL del contenedor los elementos no expirarán nunca, incluso si se ha reemplazado explícitamente el valor predeterminado anterior del TTL.

### <a id="dotnet-disable-ttl"></a>SDK de .NET V2 (Microsoft.Azure.DocumentDB)

```csharp
// Get the container, update DefaultTimeToLive to null
DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
// Disable TTL
collection.DefaultTimeToLive = null;
await client.ReplaceDocumentCollectionAsync(collection);
```

### <a id="dotnet-disable-ttl"></a>SDK de .NET V3 (Microsoft.Azure.Cosmos)

```csharp
// Get the container, update DefaultTimeToLive to null
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Disable TTL
containerResponse.Resource.DefaultTimeToLive = null;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el período de vida en el siguiente artículo:

* [Período de vida](time-to-live.md)
