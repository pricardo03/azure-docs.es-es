---
title: 'Inicio rápido: Creación de un índice en una aplicación de consola de C#: Azure Search'
description: Obtenga información sobre cómo crear un índice de búsqueda de texto completo en C# con el SDK de Azure Search para .NET.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/08/2019
ms.openlocfilehash: 83842893e0ffc6bb954832cd65b6312b59bbcaa3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59269051"
---
# <a name="quickstart-1---create-an-azure-search-index-in-c"></a>Inicio rápido: 1 - Creación de un índice de Azure Search en C#
> [!div class="op_single_selector"]
> * [C#](search-create-index-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
> * [postman](search-fiddler.md)
>*

Este artículo le guía por el proceso de creación de un [índice de Azure Search](search-what-is-an-index.md) mediante C# y el [SDK de .NET](https://aka.ms/search-sdk). Esta es la primera lección de un ejercicio que consta de tres partes para crear, cargar y consultar un índice. Para la creación de índices es preciso realizar estas tareas:

> [!div class="checklist"]
> * Crear un objeto [`SearchServiceClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) para conectarse a un servicio de búsqueda.
> * Crear un objeto [`Index`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) y usarlo como parámetro en `Indexes.Create`.
> * Llame al método `Indexes.Create` en `SearchServiceClient` para enviar `Index` a un servicio.

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se usan los siguientes servicios, herramientas y datos. 

[Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este inicio rápido.

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), cualquier edición. Se han probado código de ejemplo e instrucciones en la edición Community Edition gratuita.

[DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) proporciona la solución de ejemplo, una aplicación de consola de .NET Core escrita en C#, que se encuentra en el repositorio de GitHub de ejemplos de Azure. Descargue y extraiga la solución. De forma predeterminada, las soluciones son de solo lectura. Haga clic con el botón derecho en la solución y borre el atributo de solo lectura para que se puedan modificar los archivos. Los datos se incluyen en la solución.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas al servicio requieren un punto de conexión de URL y una clave de acceso en cada solicitud. Con ambos se crea un servicio de búsqueda, por lo que si ha agregado Azure Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

2. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio. Tener una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que envía la solicitud y el servicio que se encarga de ella.

## <a name="1---configure-and-build"></a>1: Configuración y compilación

1. Abra el archivo **DotNetHowTo.sln** en Visual Studio.

1. En el archivo appsettings.json, reemplace el contenido predeterminado por el ejemplo siguiente y, después, especifique el nombre del servicio y la clave de API de administrador del servicio. 


   ```json
   {
       "SearchServiceName": "Put your search service name here (not the full URL)",
       "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
    }
   ```

  En el caso del nombre del servicio, basta con el propio nombre. Por ejemplo, si la dirección URL es https://mydemo.search.windows.net, agregue `mydemo` al archivo JSON.

1. Presione F5 para compilar la solución y ejecute la aplicación de consola. En los restantes pasos de este ejercicio y de los siguientes se explora el funcionamiento de este código. 

Como alternativa, puede consultar [Uso de Azure Search desde una aplicación .NET](search-howto-dotnet-sdk.md) para obtener una cobertura más detallada de los comportamientos del SDK. 

<a name="CreateSearchServiceClient"></a>

## <a name="2---create-a-client"></a>2 - Cree un cliente

Para empezar a usar el SDK de .NET de Azure Search, cree una instancia de la clase `SearchServiceClient` . Esta clase tiene varios constructores. El que desea tiene el nombre del servicio de búsqueda y un objeto `SearchCredentials` como parámetros. `SearchCredentials` encapsula su clave de API.

El siguiente código se puede encontrar en el archivo Program.cs. Crea un nuevo `SearchServiceClient` con valores para el nombre de servicio de búsqueda y la clave de API que se almacenan en el archivo de configuración de la aplicación (appsettings.json).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

`SearchServiceClient` tiene una propiedad `Indexes`. Esta propiedad proporciona todos los métodos que necesita para crear, enumerar, actualizar o eliminar los índices de Azure Search.

> [!NOTE]
> La clase `SearchServiceClient` administra las conexiones con el servicio de búsqueda. Para evitar la apertura de un número excesivo de conexiones, debe intentar, si es posible, compartir una única instancia de `SearchServiceClient` en la aplicación. Sus métodos son seguros para subprocesos lo que permite habilitar este tipo de uso compartido.
> 
> 

<a name="DefineIndex"></a>

## <a name="3---construct-index"></a>3 - Cree el índice
El índice se crea mediante una llamada al método `Indexes.Create`. Este método toma como parámetro un objeto `Index` que define el índice de Azure Search. Cree un objeto `Index` e inicialícelo como se indica a continuación:

1. Establezca la propiedad `Name` del objeto `Index` en el nombre del índice.

2. Establezca la propiedad `Fields` del objeto `Index` en una matriz de objetos `Field`. La manera más fácil de crear los objetos `Field` es mediante la llamada al método `FieldBuilder.BuildForType`, pasando una clase de modelo para el parámetro de tipo. Una clase de modelo tiene propiedades que se asignan a los campos del índice. Esto le permite enlazar documentos desde el índice de búsqueda con las instancias de la clase de modelo.

> [!NOTE]
> Si no tiene pensado usar una clase de modelo, todavía puede definir el índice mediante la creación de objetos `Field` directamente. Puede proporcionar el nombre del campo en el constructor, junto con el tipo de datos (o el analizador de campos de cadena). También puede establecer otras propiedades, como `IsSearchable` o `IsFilterable`, por nombrar algunas.
>
>

Es importante que tenga en cuenta sus necesidades del negocio y la experiencia del usuario al diseñar el índice. A cada campo se le deben asignar los [atributos](https://docs.microsoft.com/rest/api/searchservice/Create-Index) que controlan qué características de búsqueda (filtrado, navegación por facetas, ordenación etc.) se aplican a cada campo. Para cualquier propiedad que no establezca de forma explícita, el valor predeterminado de la clase `Field` es deshabilitar la característica de búsqueda correspondiente, excepto si la habilita específicamente.

En este ejemplo, el nombre del índice es "hotels" y los campos se definen mediante una clase de modelo. Cada propiedad de la clase de modelo tiene atributos que determinan los comportamientos relacionados con la búsqueda del campo de índice correspondiente. La clase del modelo se define de la manera siguiente:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Hemos elegido cuidadosamente los atributos de cada propiedad en función de cómo creemos que se usarán en una aplicación. Por ejemplo, es probable que las personas que buscan hoteles estén interesadas en coincidencias de palabras clave en el campo `description`, por eso hemos habilitado la búsqueda de texto completo para ese campo agregando el atributo `IsSearchable` a la propiedad `Description`.

Tenga en cuenta que exactamente un campo del índice de tipo `string` se debe designar como el campo *clave* agregando el atributo `Key` (consulte `HotelId` en el ejemplo anterior).

La definición del índice anterior usa un analizador de lenguaje para el campo `description_fr` porque está diseñado para almacenar texto en francés. Para más información, consulte [Incorporación de analizadores de idiomas a un índice de Azure Search](index-add-language-analyzers.md).

> [!NOTE]
> De forma predeterminada, el nombre de cada propiedad de la clase de modelo tiene su correspondencia en el nombre de campo del índice. Si quiere asignar todos los nombres de propiedad a nombres de campo con mayúsculas y minúsculas Camel, marque la clase con el atributo `SerializePropertyNamesAsCamelCase`. Si quiere asignarlos a un nombre diferente, puede usar el atributo `JsonProperty` como la propiedad `DescriptionFr` anterior. El atributo `JsonProperty` tiene prioridad sobre el atributo `SerializePropertyNamesAsCamelCase`.
> 
> 

Ahora que hemos definido una clase de modelo, podemos crear fácilmente una definición de índice:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = FieldBuilder.BuildForType<Hotel>()
};
```

## <a name="4---call-indexescreate"></a>4 - Llame a Indexes.Create
Ahora que tiene un objeto `Index` inicializado, para crear el índice solo debe realizar una llamada a `Indexes.Create` en el objeto `SearchServiceClient`:

```csharp
serviceClient.Indexes.Create(definition);
```

Para una solicitud correcta, el método realizará la devolución normalmente. Si hay un problema con la solicitud, como un parámetro no válido, el método producirá una `CloudException`.

Cuando haya terminado con un índice y desee eliminarlo, llame al método `Indexes.Delete` en su `SearchServiceClient`. Por ejemplo: 

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> El código de ejemplo de este artículo usa los métodos sincrónicos del SDK de Azure Search para .NET por motivos de simplicidad. En sus propias aplicaciones, recomendamos que use métodos asincrónicos para mantener su escalabilidad y capacidad de respuesta. Por ejemplo, en los ejemplos anteriores podría utilizar `CreateAsync` y `DeleteAsync` en lugar de `Create` y `Delete`.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
En este inicio rápido, ha creado un índice de Azure Search vacío basado en un esquema que define los comportamientos y tipos de datos de un campo. El índice es un índice básico que consta de un nombre y una colección de campos con atributos. Un índice más realista incluiría otros elementos, tales como [perfiles de puntuación](index-add-scoring-profiles.md), [proveedores de sugerencias](index-add-suggesters.md) para admitir la escritura anticipada, [sinónimos](search-synonyms.md) y, posiblemente, [ analizadores personalizados](index-add-custom-analyzers.md). Es recomendable repasar estas funcionalidades una vez que comprenda el flujo de trabajo básico.

En el siguiente tutorial de inicio rápido de esta serie se trata cómo cargar el índice con contenido en el que se puedan realizar búsquedas.

> [!div class="nextstepaction"]
> [Carga de datos en un índice de Azure Search mediante C#](search-import-data-dotnet.md)