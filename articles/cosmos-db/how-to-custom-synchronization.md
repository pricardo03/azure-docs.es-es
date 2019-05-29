---
title: Implementación de la sincronización personalizada para optimizar la disponibilidad y el rendimiento en Azure Cosmos DB
description: Aprenda a implementar la sincronización personalizada para optimizar la disponibilidad y el rendimiento en Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: f10e260432a93a0413d65d6f5814d00a50e9465a
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560271"
---
# <a name="implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Implementación de la sincronización personalizada para optimizar la disponibilidad y el rendimiento

Azure Cosmos DB ofrece [cinco niveles de coherencia bien definidos](consistency-levels.md) entre los que elegir para buscar el equilibrio entre coherencia, rendimiento y disponibilidad. Una fuerte coherencia garantiza que los datos se replican sincrónicamente y se mantiene su durabilidad en todas las regiones donde está disponible la cuenta de Azure Cosmos. Esta configuración proporciona el nivel más alto de durabilidad a costa de rendimiento y disponibilidad. Si quiere que la aplicación controle o relaje la durabilidad de los datos en función de sus necesidades sin poner en peligro la disponibilidad, puede emplear la *sincronización personalizada* en la capa de la aplicación para lograr el nivel deseado de durabilidad.

La siguiente imagen representa visualmente el modelo de sincronización personalizada:

![Sincronización personalizada](./media/how-to-custom-synchronization/custom-synchronization.png)

En este escenario, un contenedor de Azure Cosmos se replica globalmente entre varias regiones en muchos continentes. El uso de una fuerte coherencia en todas las regiones de este escenario afecta al rendimiento. Para garantizar un mayor nivel de durabilidad de los datos sin poner en peligro la latencia de escritura, la aplicación puede usar dos clientes que compartan el mismo [token de sesión](how-to-manage-consistency.md#utilize-session-tokens).

El primer cliente puede escribir datos en la región local (por ejemplo, oeste de EE. UU.). El segundo cliente (por ejemplo, en el este de EE. UU.) es un cliente de lectura que se usa para garantizar la sincronización. Como el token de sesión fluye desde la respuesta de escritura hasta la siguiente lectura, la lectura garantiza la sincronización de las escrituras con el este de EE. UU. Azure Cosmos DB garantiza que las escrituras se ven en al menos una región. Se tiene la garantía de que sobrevivirán a una interrupción regional si la región de escritura original deja de funcionar. En este escenario, cada escritura se sincroniza con el este de EE. UU., lo que reduce la latencia de emplear una coherencia fuerte en todas las regiones. En un escenario de arquitectura multimaestro, donde las escrituras se producen en cada región, puede ampliar este modelo para realizar la sincronización con varias regiones en paralelo.

## <a name="configure-the-clients"></a>Configuración de los clientes

En el ejemplo siguiente se muestra una capa de acceso a datos que crea una instancia de dos clientes para la sincronización personalizada:

```csharp
class MyDataAccessLayer
{
    private DocumentClient writeClient;
    private DocumentClient readClient;

    public async Task InitializeAsync(Uri accountEndpoint, string key)
    {
        ConnectionPolicy writeConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp,
            UseMultipleWriteLocations = true
        };
        writeConnectionPolicy.SetCurrentLocation(LocationNames.WestUS);

        ConnectionPolicy readConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        };
        readConnectionPolicy.SetCurrentLocation(LocationNames.EastUS);

        writeClient = new DocumentClient(accountEndpoint, key, writeConnectionPolicy);
        readClient = new DocumentClient(accountEndpoint, key, readConnectionPolicy, ConsistencyLevel.Session);

        await Task.WhenAll(new Task[]
        {
            writeClient.OpenAsync(),
            readClient.OpenAsync()
        });
    }
}
```

## <a name="implement-custom-synchronization"></a>Implementación de la sincronización personalizada

Una vez que los clientes se inicializan, la aplicación puede realizar escrituras en la región local (oeste de EE. UU.) y puede forzar la sincronización de las escrituras con el este de EE. UU, como se indica a continuación.

```csharp
class MyDataAccessLayer
{
    public async Task CreateItem(string containerLink, Document document)
    {
        ResourceResponse<Document> response = await writeClient.CreateDocumentAsync(
            containerLink, document);

        await readClient.ReadDocumentAsync(response.Resource.SelfLink,
            new RequestOptions { SessionToken = response.SessionToken });
    }
}
```

Puede extender el modelo para realizar la sincronización con varias regiones en paralelo.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la distribución global y la coherencia en Azure Cosmos DB, lea estos artículos:

* [Selección del nivel de coherencia adecuado en Azure Cosmos DB](consistency-levels-choosing.md)
* [Inconvenientes de la coherencia, disponibilidad y rendimiento en Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Administración de la coherencia en Azure Cosmos DB](how-to-manage-consistency.md)
* [Creación de particiones y distribución de datos en Azure Cosmos DB](partition-data.md)
