---
title: Administración de conexiones en Azure Functions
description: Aprenda a evitar problemas de rendimiento en Azure Functions mediante el uso de los clientes de conexión estáticos.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: glenga
ms.openlocfilehash: 4246259445cf096b5353ab87a9ed83f87332dc78
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56299333"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Administración de conexiones en Azure Functions

Las funciones de una aplicación de función comparten recursos y entre dichos recursos compartidos se encuentran las conexiones (conexiones HTTP, conexiones de base de datos y conexiones a los servicios de Azure, como Storage). Cuando se ejecutan simultáneamente muchas funciones es posible que se agoten las conexiones disponibles. Este artículo explica cómo codificar las funciones para evitar el uso de más conexiones que las que se necesitan realmente.

## <a name="connections-limit"></a>Límite de conexiones

El número de conexiones disponibles está limitado en parte porque una aplicación de función se ejecuta en el [espacio aislado de Azure App Service](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox). Una de las restricciones que impone este espacio aislado en el código es un [límite en el número de conexiones, actualmente 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Cuando se alcanza este límite, el runtime de las funciones crea un registro con el siguiente mensaje: `Host thresholds exceeded: Connections`.

La probabilidad de exceder el límite aumenta cuando el [controlador de escala agrega instancias de aplicación de función](functions-scale.md#how-the-consumption-plan-works) para controlar más solicitudes. Cada instancia de aplicación de función puede ejecutar muchas funciones a la vez, todas de las cuales usan conexiones que se consideran en el límite de 300.

## <a name="use-static-clients"></a>Uso de clientes estáticos

Para evitar mantener más conexiones de las necesarias, reutilice las instancias de cliente en lugar de crear nuevas con cada invocación de función. Los clientes de .NET, como [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) y los clientes de Azure Storage, pueden administrar las conexiones si usa un cliente único y estático.

Estas son algunas directrices que se deben seguir al utilizar a un cliente específico del servicio en una aplicación de Azure Functions:

- **NO** cree un nuevo cliente con cada invocación de función.
- **CREE** un único cliente estático que pueda ser utilizado por cada invocación de función.
- **CONSIDERE** la posibilidad de crear un cliente único y estático en una clase auxiliar compartida si funciones diferentes utilizan el mismo servicio.

## <a name="client-code-examples"></a>Ejemplos de código de cliente

En esta sección se muestran los procedimientos recomendados para crear y utilizar clientes a partir del código de función.

### <a name="httpclient-example-c"></a>Ejemplo de HttpClient (C#)

Este es un ejemplo de código de función de C# que crea un cliente [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) estático:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

Una pregunta común sobre el cliente [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) de .NET es "¿Debería desechar mi cliente?" En general, se desechan los objetos que implementan `IDisposable` cuando se ha terminado de utilizarlos. Pero no se desecha un cliente estático porque su uso no ha terminado cuando finaliza la función. Desea que el cliente estático viva en toda la duración de la aplicación.

### <a name="http-agent-examples-nodejs"></a>Ejemplos de agente HTTP (Node.js)

Debido a que proporciona mejores opciones de administración de conexiones, debería utilizar la clase nativa [`http.agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) en lugar de los métodos no nativos, como el módulo `node-fetch`. Los parámetros de conexión se configuran mediante las opciones de la clase `http.agent`. Consulte [new Agent(\[options\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options) para ver las opciones detalladas disponibles con el agente HTTP.

El parámetro `http.globalAgent` global que se utiliza en `http.request()` tiene todos estos valores establecidos a sus respectivos valores predeterminados. La forma recomendada de configurar los límites de conexión en Functions es establecer un número máximo globalmente. El siguiente ejemplo establece el número máximo de sockets para la aplicación de función:

```js
http.globalAgent.maxSockets = 200;
```

 En el ejemplo siguiente se crea una nueva solicitud HTTP con un agente HTTP personalizado solo para esa solicitud.

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>Ejemplo de código de DocumentClient (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) se conecta a una instancia de Azure Cosmos DB. La documentación de Azure Cosmos DB recomienda el [uso de un cliente de Azure Cosmos DB singleton para el ciclo de vida de la aplicación](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). En el ejemplo siguiente se muestra un patrón para realizarlo en una función:

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

## <a name="sqlclient-connections"></a>Conexiones de SqlClient

El código de la función puede usar el proveedor de datos .NET Framework para SQL Server ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) para establecer conexiones con una base de datos relacional de SQL. También es el proveedor subyacente para los marcos de datos que se basan en ADO.NET, como Entity Framework. A diferencia de las conexiones de [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) y [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
), ADO.NET implementa la agrupación de conexiones de manera predeterminada. Sin embargo, como todavía puede quedarse sin conexiones, debería optimizar las conexiones con la base de datos. Para más información, consulte el artículo sobre la [agrupación de conexiones de SQL Server (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Algunos marcos de datos, como [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx), habitualmente obtienen cadenas de conexión desde la sección **ConnectionStrings** de un archivo de configuración. En este caso, debe agregar explícitamente las cadenas de conexión de base de datos SQL a la colección **Cadenas de conexión** de la configuración de la aplicación de función y en el [archivo local.settings.json](functions-run-local.md#local-settings-file) del proyecto local. Si está creando un objeto [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) en el código de la función, debe almacenar el valor de la cadena de conexión en **Configuración de la aplicación** con las otras conexiones.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de por qué se recomiendan clientes estáticos, consulte [Antipatrón Improper Instantiation](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Para más sugerencias de rendimiento de Azure Functions, consulte [Optimización del rendimiento y la confiabilidad de Azure Functions](functions-best-practices.md).
