---
title: Creación de un punto de conexión de RESTful para proveedores personalizados
description: En este tutorial se explica cómo crear un punto de conexión de RESTful para proveedores personalizados. Detallará cómo tratar las solicitudes y respuestas para los métodos HTTP de RESTful admitidos.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 176e3b02cbda7577e306d86363cfe5b41335fb6e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799184"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Creación de un punto de conexión de RESTful para proveedores personalizados

Los proveedores personalizados le permiten personalizar los flujos de trabajo en Azure. Un proveedor personalizado es un contrato entre Azure y un `endpoint`. Este tutorial le guiará por el proceso de creación de un proveedor personalizado de RESTful `endpoint`. Si no está familiarizado con los proveedores personalizados de Azure, consulte [la introducción a los proveedores de recursos personalizados](./custom-providers-overview.md).

Este tutorial se divide en los pasos siguientes:

- Trabajo con las acciones y recursos personalizados
- Creación de particiones de recursos personalizados en el almacenamiento
- Compatibilidad con los métodos de RESTful del proveedor personalizado
- Integración de las operaciones de RESTful

Este tutorial se basará en los siguientes tutoriales:

- [Configuración de Azure Functions para los proveedores personalizados de Azure](./tutorial-custom-providers-function-setup.md)

> [!NOTE]
> Este tutorial se basa en el tutorial anterior. Algunos de los pasos del tutorial solo funcionarán si se ha configurado una función de Azure para que funcione con proveedores personalizados.

## <a name="working-with-custom-actions-and-custom-resources"></a>Trabajo con las acciones y recursos personalizados

En este tutorial, actualizaremos la función para que funcione como un punto de conexión de RESTful para nuestro proveedor personalizado. En Azure, los recursos y las acciones se modelan después de la especificación de RESTful básica: PUT crea un nuevo recurso; GET (instancia) recupera un recurso existente, DELETE elimina un recurso existente, POST desencadena una acción y GET (colección) enumera todos los recursos existentes. Para este tutorial, se utilizarán las tablas de Azure como almacenamiento, pero cualquier base de datos o servicio de almacenamiento puede servir.

## <a name="how-to-partition-custom-resources-in-storage"></a>Creación de particiones de recursos personalizados en el almacenamiento

Como vamos a crear un servicio RESTful, es necesario almacenar los recursos creados en el almacenamiento. Para Azure Table Storage, es necesario generar las claves de partición y de fila de los datos. En el caso de los proveedores personalizados, los datos deben particionarse en el proveedor personalizado. Cuando se envía una solicitud entrante al proveedor personalizado, este agregará el encabezado `x-ms-customproviders-requestpath` a la solicitud saliente a `endpoint`.

Encabezado `x-ms-customproviders-requestpath` de ejemplo para un recurso personalizado:

```
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{myResourceType}/{myResourceName}
```

Basándose en el encabezado `x-ms-customproviders-requestpath` de ejemplo anterior, podemos crear los parámetros partitionKey y rowKey para nuestro almacenamiento de la siguiente manera:

Parámetro | Plantilla | DESCRIPCIÓN
---|---
partitionKey | '{subscriptionId}:{resourceGroupName}:{resourceProviderName}' | El parámetro partitionKey indica cómo se particionan los datos. Para la mayoría de los casos, los datos se deben particionar por la instancia del proveedor personalizado.
rowKey | '{myResourceType}:{myResourceName}' | El parámetro rowKey indica el identificador individual de los datos. La mayoría de las veces es el nombre del recurso.

Además, también necesitamos crear una nueva clase para modelar nuestro recurso personalizado. En este tutorial, agregaremos la clase `CustomResource` a nuestra función, que es una clase genérica que acepta cualquier entrada de datos:

```csharp
// Custom Resource Table Entity
public class CustomResource : TableEntity
{
    public string Data { get; set; }
}
```

Esto crea una clase básica basada en `TableEntity`, que se usa para almacenar los datos. La clase `CustomResource` hereda dos propiedades de `TableEntity`: partitionKey y rowKey.

## <a name="support-custom-provider-restful-methods"></a>Compatibilidad con los métodos de RESTful del proveedor personalizado

> [!NOTE]
> Si no está copiando el código directamente del tutorial, el contenido de la respuesta debe ser un JSON válido y establece el encabezado `Content-Type` como `application/json`.

Ahora que tenemos la configuración de la partición de datos, se va a aplicar la técnica de scaffolding en el CRUD básico y en los métodos de desencadenamiento para los recursos y acciones personalizados. Como los proveedores personalizados actúan como un proxy, la solicitud y la respuesta deben modelarse y tratarse por RESTful `endpoint`. Siga estos fragmentos de código para controlar las operaciones de RESTful básicas:

### <a name="trigger-custom-action"></a>Desencadenamiento de la acción personalizada

En el caso de los proveedores personalizados, se desencadena una acción personalizada mediante las solicitudes `POST`. Una acción personalizada puede aceptar opcionalmente un cuerpo de la solicitud que contenga un conjunto de parámetros de entrada. La acción debe entonces devolver una respuesta que indique el resultado de la acción, así como si se realizado correctamente o ha producido un error. En este tutorial, agregaremos el método `TriggerCustomAction` a nuestra función:

```csharp
/// <summary>
/// Triggers a custom action with some side effect.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <returns>The http response result of the custom action.</returns>
public static async Task<HttpResponseMessage> TriggerCustomAction(HttpRequestMessage requestMessage)
{
    var myCustomActionRequest = await requestMessage.Content.ReadAsStringAsync();

    var actionResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    actionResponse.Content = myCustomActionRequest != string.Empty ? 
        new StringContent(JObject.Parse(myCustomActionRequest).ToString(), System.Text.Encoding.UTF8, "application/json") :
        null;
    return actionResponse;
}
```

El método `TriggerCustomAction` acepta una solicitud entrante y simplemente devuelve la respuesta con un código de estado correcto. 

### <a name="create-custom-resource"></a>Creación de un recurso personalizado

En el caso de los proveedores personalizados, se crea un recurso personalizado mediante las solicitudes `PUT`. El proveedor personalizado aceptará un cuerpo de la solicitud JSON, que contiene un conjunto de propiedades para el recurso personalizado. En Azure, los recursos siguen un modelo de RESTful. La misma dirección URL de solicitud que se utilizó para crear un recurso también debería poder recuperar y eliminar el recurso. En este tutorial, agregaremos el método `CreateCustomResource` para crear nuevos recursos:

```csharp
/// <summary>
/// Creates a custom resource and saves it to table storage.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="azureResourceId">The parsed Azure resource Id.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the created custom resource.</returns>
public static async Task<HttpResponseMessage> CreateCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, ResourceId azureResourceId, string partitionKey, string rowKey)
{
    // Adds the Azure top-level properties.
    var myCustomResource = JObject.Parse(await requestMessage.Content.ReadAsStringAsync());
    myCustomResource["name"] = azureResourceId.Name;
    myCustomResource["type"] = azureResourceId.FullResourceType;
    myCustomResource["id"] = azureResourceId.Id;

    // Save the resource into storage.
    var insertOperation = TableOperation.InsertOrReplace(
        new CustomResource
        {
            PartitionKey = partitionKey,
            RowKey = rowKey,
            Data = myCustomResource.ToString(),
        });
    await tableStorage.ExecuteAsync(insertOperation);

    var createResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    createResponse.Content = new StringContent(myCustomResource.ToString(), System.Text.Encoding.UTF8, "application/json");
    return createResponse;
}
```

El método `CreateCustomResource` actualiza la solicitud entrante para incluir los campos específicos de Azure: `id`, `name` y `type`. Estos campos son propiedades de nivel superior que utilizan los servicios en Azure. Permiten que el proveedor personalizado se integre con otros servicios como Azure Policy, plantillas de Azure Resource Manager y Registros de actividad de Azure.

Propiedad | Muestra | DESCRIPCIÓN
---|---|---
Nombre | '{myCustomResourceName}' | El nombre del recurso personalizado.
Tipo | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | El espacio de nombres del tipo de recurso.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>{resourceTypeName}/{myCustomResourceName}' | El identificador del recurso.

Además de agregar las propiedades, también guardamos el documento en Azure Table Storage. 

### <a name="retrieve-custom-resource"></a>Recuperación de recursos personalizados

En el caso de los proveedores personalizados, se recupera un recurso personalizado mediante las solicitudes `GET`. El proveedor personalizado *no* aceptará un cuerpo de la solicitud JSON. En el caso de las solicitudes `GET`, el **punto de conexión** debe utilizar el encabezado `x-ms-customproviders-requestpath` para devolver el recurso ya creado. En este tutorial, agregaremos el método `RetrieveCustomResource` para recuperar los recursos existentes:

```csharp
/// <summary>
/// Retrieves a custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the existing custom resource.</returns>
public static async Task<HttpResponseMessage> RetrieveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    var retrieveResponse = requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NotFound);

    retrieveResponse.Content = existingCustomResource != null ?
            new StringContent(existingCustomResource.Data, System.Text.Encoding.UTF8, "application/json"):
            null;
    return retrieveResponse;
}
```

En Azure, los recursos deben seguir un modelo de RESTful. La dirección URL de la solicitud que creó el recurso también debe devolver el recurso si se realiza una solicitud `GET`.

### <a name="remove-custom-resource"></a>Supresión de un recurso personalizado

En el caso de los proveedores personalizados, se quita un recurso personalizado mediante las solicitudes `DELETE`. El proveedor personalizado *no* aceptará un cuerpo de la solicitud JSON. En el caso de las solicitudes `DELETE`, el **punto de conexión** debe utilizar el encabezado `x-ms-customproviders-requestpath` para eliminar el recurso ya creado. En este tutorial, agregaremos el método `RemoveCustomResource` para quitar los recursos existentes:

```csharp
/// <summary>
/// Removes an existing custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the result of the delete.</returns>
public static async Task<HttpResponseMessage> RemoveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    if (existingCustomResource != null) {
        var deleteOperation = TableOperation.Delete(existingCustomResource);
        await tableStorage.ExecuteAsync(deleteOperation);
    }

    return requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NoContent);
}
```

En Azure, los recursos deben seguir un modelo de RESTful. La dirección URL de la solicitud que creó el recurso también debe eliminar el recurso si se realiza una solicitud `DELETE`.

### <a name="list-all-custom-resources"></a>Enumeración de recursos personalizados

Para los proveedores personalizados, se puede enumerar una lista de los recursos personalizados existentes mediante las solicitudes `GET` de la colección. El proveedor personalizado *no* aceptará un cuerpo de la solicitud JSON. En el caso de las solicitudes `GET` de la colección, el `endpoint` debe utilizar el encabezado `x-ms-customproviders-requestpath` para enumerar los recursos ya creados. En este tutorial, agregaremos el método `EnumerateAllCustomResources` para enumerar los recursos existentes.

```csharp
/// <summary>
/// Enumerates all the stored custom resources for a given type.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="resourceType">The resource type of the enumeration.</param>
/// <returns>The http response containing a list of resources stored under 'value'.</returns>
public static async Task<HttpResponseMessage> EnumerateAllCustomResources(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string resourceType)
{
    // Generate upper bound of the query.
    var rowKeyUpperBound = new StringBuilder(resourceType);
    rowKeyUpperBound[rowKeyUpperBound.Length - 1]++;

    // Create the enumeration query.
    var enumerationQuery = new TableQuery<CustomResource>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, partitionKey),
            TableOperators.And,
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, resourceType),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, rowKeyUpperBound.ToString()))));
    
    var customResources = (await tableStorage.ExecuteQuerySegmentedAsync(enumerationQuery, null))
        .ToList().Select(customResource => JToken.Parse(customResource.Data));

    var enumerationResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    enumerationResponse.Content = new StringContent(new JObject(new JProperty("value", customResources)).ToString(), System.Text.Encoding.UTF8, "application/json");
    return enumerationResponse;
}
```

> [!NOTE]
> La clave de la fila mayor y menor que la sintaxis de la tabla de Azure para realizar una consulta "startswith" para las cadenas. 

Para enumerar todos los recursos existentes, generamos una consulta en la tabla de Azure que garantiza que los recursos existen bajo nuestra partición de proveedor personalizado. La consulta comprueba entonces que la clave de fila comienza con el mismo `{myResourceType}`.

## <a name="integrate-restful-operations"></a>Integración de las operaciones de RESTful

Cuando todos los métodos RESTful se agregan a la función, podemos actualizar el método `Run` principal para llamar a las funciones y tratar las diferentes solicitudes REST:

```csharp
/// <summary>
/// Entry point for the Azure Function webhook and acts as the service behind a custom provider.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="log">The logger.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <returns>The http response for the custom Azure API.</returns>
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log, CloudTable tableStorage)
{
    // Get the unique Azure request path from request headers.
    var requestPath = req.Headers.GetValues("x-ms-customproviders-requestpath").FirstOrDefault();

    if (requestPath == null)
    {
        var missingHeaderResponse = req.CreateResponse(HttpStatusCode.BadRequest);
        missingHeaderResponse.Content = new StringContent(
            new JObject(new JProperty("error", "missing 'x-ms-customproviders-requestpath' header")).ToString(),
            System.Text.Encoding.UTF8, 
            "application/json");
    }

    log.LogInformation($"The Custom Provider Function received a request '{req.Method}' for resource '{requestPath}'.");

    // Determines if it is a collection level call or action.
    var isResourceRequest = requestPath.Split('/').Length % 2 == 1;
    var azureResourceId = isResourceRequest ? 
        ResourceId.FromString(requestPath) :
        ResourceId.FromString($"{requestPath}/");

    // Create the Partition Key and Row Key
    var partitionKey = $"{azureResourceId.SubscriptionId}:{azureResourceId.ResourceGroupName}:{azureResourceId.Parent.Name}";
    var rowKey = $"{azureResourceId.FullResourceType.Replace('/', ':')}:{azureResourceId.Name}";

    switch (req.Method)
    {
        // Action request for an custom action.
        case HttpMethod m when m == HttpMethod.Post && !isResourceRequest:
            return await TriggerCustomAction(
                requestMessage: req);

        // Enumerate request for all custom resources.
        case HttpMethod m when m == HttpMethod.Get && !isResourceRequest:
            return await EnumerateAllCustomResources(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                resourceType: rowKey);

        // Retrieve request for a custom resource.
        case HttpMethod m when m == HttpMethod.Get && isResourceRequest:
            return await RetrieveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Create request for a custom resource.
        case HttpMethod m when m == HttpMethod.Put && isResourceRequest:
            return await CreateCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                azureResourceId: azureResourceId,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Remove request for a custom resource.
        case HttpMethod m when m == HttpMethod.Delete && isResourceRequest:
            return await RemoveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Invalid request received.
        default:
            return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}
``` 

El método `Run` actualizado incluirá ahora el enlace de entrada `tableStorage` que se agregó en Azure Table Storage. La primera parte del método ahora leerá el encabezado `x-ms-customproviders-requestpath` y usará la biblioteca `Microsoft.Azure.Management.ResourceManager.Fluent` para analizar el valor como un identificador de recurso. El encabezado `x-ms-customproviders-requestpath` lo envía el proveedor personalizado y designa la ruta de acceso de la solicitud entrante. Con el identificador del recurso analizado, ahora podemos generar los parámetros partitionKey y rowKey para que los datos busquen o almacenen recursos personalizados.

Además de agregar los métodos y las clases, tenemos que actualizar los métodos using de la función. Agregue lo siguiente en la parte superior del archivo:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"
#r "../bin/Microsoft.Azure.Management.ResourceManager.Fluent.dll"

using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Configuration;
using System.Text;
using System.Threading;
using System.Globalization;
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Si se perdió durante cualquier punto de este tutorial, puede encontrar el código de ejemplo completo en la [referencia de punto de conexión RESTful de C# del proveedor personalizado](./reference-custom-providers-csharp-endpoint.md). Cuando se completa la función, guarde la dirección URL de la función que se puede utilizar para desencadenar la función, tal y como se utilizará en los tutoriales posteriores.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos creado un punto de conexión de RESTful para trabajar con el proveedor personalizado de Azure `endpoint`. Vaya al artículo siguiente para aprender a crear un proveedor personalizado.

- [Tutorial: Creación de un proveedor personalizado](./tutorial-custom-providers-create.md)
