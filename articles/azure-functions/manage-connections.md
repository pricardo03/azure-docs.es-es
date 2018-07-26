---
title: Administración de conexiones en Azure Functions
description: Aprenda a evitar problemas de rendimiento en Azure Functions mediante el uso de los clientes de conexión estáticos.
services: functions
documentationcenter: ''
author: tdykstra
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: tdykstra
ms.openlocfilehash: 9e5c56dc3679e9ffbd67d906ca7d971439319ee5
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125383"
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

## <a name="httpclient-code-example"></a>Ejemplo de código de HttpClient

Este es un ejemplo de código de función que crea un cliente [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) estático:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("http://example.com");
    // Rest of function
}
```

Una pregunta común sobre el cliente [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) de .NET es "¿Debería desechar mi cliente?" En general, se desechan los objetos que implementan `IDisposable` cuando se ha terminado de utilizarlos. Pero no se desecha un cliente estático porque su uso no ha terminado cuando finaliza la función. Desea que el cliente estático viva en toda la duración de la aplicación.

## <a name="documentclient-code-example"></a>Ejemplo de código de DocumentClient

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