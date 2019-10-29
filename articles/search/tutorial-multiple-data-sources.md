---
title: 'Tutorial de C#: Indexación de varios orígenes de datos'
titleSuffix: Azure Cognitive Search
description: Aprenda a importar datos desde varios orígenes en un único índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3b94e3e352f4d6b5cd7da41feb9660be2ffed2bd
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72786484"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-cognitive-search-index"></a>Tutorial de C#: Combinación de datos de varios orígenes en un índice de Azure Cognitive Search

Azure Cognitive Search puede importar, analizar e indexar datos desde varios orígenes en un único índice combinado de búsqueda. Esto es compatible con situaciones donde se agregan datos estructurados con datos menos estructurados o datos de texto sin formato, o incluso de otros orígenes, como documentos de texto, HTML o JSON.

En este tutorial se describe cómo indexar datos de hoteles desde un origen de datos de Azure Cosmos DB y combinarlos con los detalles de las habitaciones de hotel dibujados de los documentos de Azure Blob Storage. El resultado será un índice de búsqueda combinado de hoteles que contiene tipos de datos complejos.

En este tutorial se usa C#, el SDK de .NET para Azure Cognitive Search y Azure Portal para realizar las siguientes tareas:

> [!div class="checklist"]
> * Cargar datos de ejemplo y crear orígenes de datos
> * Identificar la clave del documento
> * Definir y crear el índice
> * Indexar los datos de los hoteles desde Azure Cosmos DB
> * Combinar los datos de las habitaciones de hotel desde Blob Storage

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se usan los siguientes servicios, herramientas y datos. 

- [Cree un servicio Azure Cognitive Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

- [Cree una cuenta de Azure Cosmos DB](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de hoteles de ejemplo.

- [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos JSON en blobs.

- [Instale Visual Studio](https://visualstudio.microsoft.com/) para usarlo como IDE.

### <a name="install-the-project-from-github"></a>Instalación del proyecto desde GitHub

1. Busque el repositorio de ejemplo en GitHub: [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Seleccione **Clone or download** (Clonar o descargar) y cree su copia local privada del proyecto.
1. Abra Visual Studio e instale el paquete NuGet para Microsoft Azure Cognitive Search, si aún no está instalado. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y **Manage NuGet Packages for Solution...** (Administrar paquetes NuGet para la solución...). En la pestaña **Examinar**, busque e instale **Microsoft.Azure.Search** (versión 9.0.1 o posterior). Tendrá que hacer clic en algunos cuadros de diálogo adicionales para completar la instalación.

    ![Uso de NuGet para agregar bibliotecas de Azure](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Mediante Visual Studio, vaya al repositorio local y abra el archivo de solución **AzureSearchMultipleDataSources.sln**.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Para interactuar con el servicio Azure Cognitive Search, necesita la dirección URL del servicio y una clave de acceso. Con ambos se crea un servicio de búsqueda, por lo que, si ha agregado Azure Cognitive Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-get-started-postman/get-url-key.png "Obtención de una clave de acceso y un punto de conexión HTTP")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio. Una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que la envía y el servicio que se encarga de ella.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Preparación de datos de ejemplo de Azure Cosmos DB

Este ejemplo utiliza dos conjuntos pequeños de datos que describen las siete hoteles ficticios. Uno de ellos describe los hoteles en sí y se cargará en una base de datos de Azure Cosmos DB. El otro contiene los detalles de las habitaciones y se proporciona como siete archivos JSON independientes que se cargarán en Azure Blob Storage.

1. [Inicie sesión en Azure Portal](https://portal.azure.com) y vaya a la página Introducción de la cuenta de Azure Cosmos DB.

1. En la barra de menús, haga clic en Agregar contenedor. Especifique "Crear nueva base de datos" y use el nombre **hotel-rooms-db**. Escriba **hotels** como nombre de la colección y **/HotelId** como clave de partición. Haga clic en **Aceptar** para crear la base de datos y el contenedor.

   ![Adición de un contenedor de Azure Cosmos DB](media/tutorial-multiple-data-sources/cosmos-add-container.png "Incorporación de un contenedor de Azure Cosmos DB")

1. Vaya al Explorador de datos de Cosmos DB y seleccione **items** (elementos) bajo el contenedor **hotels** de la base de datos **hotel-salas-db**. En la barra de comandos, haga clic en **Upload Item** (Cargar elemento).

   ![Carga en la colección de Azure Cosmos DB](media/tutorial-multiple-data-sources/cosmos-upload.png "Carga en una colección de Cosmos DB")

1. En el panel de carga haga clic en el botón de carpeta y vaya al archivo **cosmosdb/HotelsDataSubset_CosmosDb.json** de la carpeta del proyecto. Haga clic en **Aceptar** para iniciar la carga.

   ![Selección del archivo que se va a cargar](media/tutorial-multiple-data-sources/cosmos-upload2.png "Seleccionar el archivo que se va a cargar")

1. Utilice el botón Actualizar para actualizar la vista de los elementos de la colección de hoteles. Verá siete nuevos documentos de base de datos enumerados.

## <a name="prepare-sample-blob-data"></a>Preparación de los datos de blob

1. [Inicie sesión en Azure Portal](https://portal.azure.com), vaya a su cuenta de Azure Storage, haga clic en **Blobs** y, después, en **+Contenedor**.

1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) denominado **hotel-rooms** para almacenar los archivos JSON de las habitaciones de hotel de ejemplo. Puede establecer el nivel de acceso público a cualquiera de sus valores válidos.

   ![Creación de un contenedor de blobs](media/tutorial-multiple-data-sources/blob-add-container.png "Creación de un contenedor de blobs")

1. Una vez creado el contenedor, ábralo y seleccione **Cargar** en la barra de comandos.

   ![Carga en la barra de comandos](media/search-semi-structured-data/upload-command-bar.png "Carga en la barra de comandos")

1. Vaya a la carpeta que contiene los archivos de ejemplo. Selecciónelos todos ellos y haga clic en **Cargar**.

   ![Carga de archivos](media/tutorial-multiple-data-sources/blob-upload.png "Carga de archivos")

Una vez finalizada la carga, los archivos deberían aparecer en la lista del contenedor de datos.

## <a name="set-up-connections"></a>Configuración de las conexiones

La información de conexión para los servicios de búsqueda y los orígenes de datos se especifican en el archivo **appsettings.json** de la solución. 

1. En Visual Studio, abra el archivo **AzureSearchMultipleDataSources.sln**.

1. En el Explorador de soluciones, edite el archivo **appsettings.json**.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "BlobStorageAccountName": "Put your Azure Storage account name here",
  "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
  "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
  "CosmosDBDatabaseName": "hotel-rooms-db"
}
```

Las dos primeras entradas usan las claves de dirección URL y de administrador del servicio Azure Cognitive Search. Por ejemplo, dado un punto de conexión de `https://mydemo.search.windows.net`, el nombre del servicio que debe proporcionar es `mydemo`.

Las siguientes entradas especifican nombres de cuenta e información de la cadena de conexión de los orígenes de datos de Azure Blob Storage y Azure Cosmos DB.

### <a name="identify-the-document-key"></a>Identificar la clave del documento

En Azure Cognitive Search, el campo de clave identifica de manera exclusiva cada documento del índice. Cada índice de búsqueda debe tener exactamente un campo clave de tipo `Edm.String`. Ese campo de clave debe estar presente para cada documento de un origen de datos que se agregue al índice. (de hecho, es el único campo obligatorio).

Al indexar datos de varios orígenes, cada valor de clave de origen de datos debe asignarse al mismo campo de clave en el índice combinado. A menudo requiere planeación inicial para identificar una clave de documentación significativa para el índice; asegúrese de que existe en todos los orígenes de datos.

Los indexadores de Azure Cognitive Search pueden usar asignaciones de campo para cambiar el nombre e incluso el formato de campos de datos durante la indexación para poder dirigir los datos de origen al campo de índice correcto.

Por ejemplo, en nuestros datos de Azure Cosmos DB de ejemplo, el identificador de los hoteles se denomina **HotelId**. Pero en los archivos de blob JSON de las habitaciones, se denomina **Id**. El programa se encarga de ello mediante la asignación del campo **Id** desde los blobs hasta el campo de clave **HotelId** del índice.

> [!NOTE]
> En la mayoría de los casos, las claves de documento generadas automáticamente, como las que crean algunos indexadores de forma predeterminada, no son buenas claves de documento para los índices combinados. En general, es preferible usar un valor de clave único y significativo que ya exista en los orígenes de datos o que se pueda agregar fácilmente a ellos.

## <a name="understand-the-code"></a>Comprendiendo el código

Una vez que los valores de configuración y los datos estén en su lugar, el ejemplo de programa de **AzureSearchMultipleDataSources.sln** estará listo para la compilación y la ejecución.

Esta sencilla aplicación de consola de C#/.NET realiza las siguientes tareas:
* Crea un nuevo índice de Azure Cognitive Search basado en la estructura de datos de la clase Hotel de C# (que también hace referencia a las clases Address y Room).
* Crea un origen de datos de Azure Cosmos DB y un indexador que asigna los datos de Azure Cosmos DB a campos de índice.
* Ejecuta el indexador de Azure Cosmos DB para cargar datos de Hotel.
* Crea un origen de datos de Azure Blob Storage y un indexador que asigna datos de blob JSON a campos de índice.
* Ejecuta el indexador de Azure Blob Storage para cargar datos de Rooms.

 Antes de ejecutar el programa, dedíquele un minuto a estudiar el código y las definiciones de índice e indexador de este ejemplo. El código pertinente aparece en dos archivos:

  + **Hotel.cs** contiene el esquema que define el índice.
  + **Program.cs** contiene las funciones que crean el índice de Azure Cognitive Search, los orígenes de datos y los indexadores, y cargan los resultados combinados en el índice.

### <a name="define-the-index"></a>Definición del índice

Este programa de ejemplo usa el SDK de .NET para definir y crear un índice de Azure Cognitive Search. Aprovecha la clase [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) para generar una estructura de índice a partir de una clase de modelo de datos de C#.

El modelo de datos se define mediante la clase Hotel, que también contiene referencias a las clases Address y Room. FieldBuilder explora en profundidad varias definiciones de clase para generar una estructura de datos compleja para el índice. Se usan etiquetas de metadatos para definir los atributos de cada campo, como si se puede buscar u ordenar.

Los siguientes fragmentos de código del archivo **Hotel.cs** muestran cómo se pueden especificar un campo único y una referencia a otra clase de modelo de datos.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

En el archivo **Program.cs**, el índice se define con un nombre y una colección de campos generados por el método `FieldBuilder.BuildForType<Hotel>()` y se crea como sigue:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Creación de un origen de datos de Azure Cosmos DB y de un indexador

A continuación, el programa principal incluirá lógica para crear el origen de datos de Azure Cosmos DB para los datos de los hoteles.

En primer lugar, concatena el nombre de la base de datos de Azure Cosmos DB con la cadena de conexión. A continuación, define el objeto de origen de datos, incluida la configuración específica de los orígenes de Azure Cosmos DB, como la propiedad [useChangeDetection].

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Una vez creado el origen de datos, el programa configura un indexador de Azure Cosmos DB denominado **hotel-rooms-cosmos-indexer**.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
El programa eliminará los indexadores existentes con el mismo nombre antes de crear el nuevo, por si desea ejecutar este ejemplo más veces.

Este ejemplo define una programación para el indexador para que se ejecute una vez al día. Puede quitar la propiedad de programación de esta llamada si no desea que el indexador se vuelva a ejecutar automáticamente en el futuro.

### <a name="index-azure-cosmos-db-data"></a>Indexación de datos de Azure Cosmos DB

Una vez creados el origen de datos y el indexador, el código que ejecuta este último es breve:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Este ejemplo incluye un bloque sencillo try-catch para notificar los errores que puedan producirse durante la ejecución.

Tras la ejecución del indexador de Azure Cosmos DB, el índice de búsqueda contendrá un conjunto integral de documentos de ejemplo de hoteles. Sin embargo, el campo de habitaciones de cada hotel será una matriz vacía, ya que el origen de datos de Azure Cosmos DB no contenía detalles de las habitaciones. A continuación, el programa realizará una extracción de Blob Storage para cargar y combinar los datos de las habitaciones.

### <a name="create-blob-storage-data-source-and-indexer"></a>Creación del indexador y del origen de datos de Blob Storage

Para obtener los detalles de las habitaciones, el programa primero configura el origen de datos de Blob Storage para hacer referencia a un conjunto de archivos de blob JSON individuales.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Una vez creado el origen de datos, el programa configura un indexador de blobs denominado **hotel-rooms-blob-indexer**.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

Los blobs JSON contienen un campo de clave denominado **Id** en lugar de **HotelId**. El código usa la clase `FieldMapping` para indicar al indexador que dirija el valor del campo **Id** a la clave de documento **HotelId** del índice.

Los indexadores de Blob Storage pueden usar parámetros que identifiquen el modo de análisis que usar. El modo de análisis es diferente para los blobs que representan un único documento o varios en el mismo blob. En este ejemplo, cada blob representa un documento de índice único, por lo que el código usa el parámetro `IndexingParameters.ParseJson()`.

Para más información sobre los parámetros de análisis del indexador, consulte el artículo sobre la [Indexación de blobs JSON](search-howto-index-json-blobs.md). Para más información sobre la especificación de estos parámetros con el SDK de .NET, consulte la clase [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions).

El programa eliminará los indexadores existentes con el mismo nombre antes de crear el nuevo, por si desea ejecutar este ejemplo más veces.

Este ejemplo define una programación para el indexador para que se ejecute una vez al día. Puede quitar la propiedad de programación de esta llamada si no desea que el indexador se vuelva a ejecutar automáticamente en el futuro.

### <a name="index-blob-data"></a>Indexación de datos de blob

Una vez creados el origen de datos de Blob Storage y el indexador, el código que ejecuta el indexador es simple:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Dado que el índice ya se ha rellenado con datos de los hoteles desde la base de datos de Azure Cosmos DB, el indexador de blobs actualiza los documentos existentes en el índice y agrega los detalles de las habitaciones.

> [!NOTE]
> Si tiene los mismos campos (distintos del clave) en ambos orígenes de datos y los datos de esos campos no coinciden, el índice contendrá los valores del indexador que se ejecutara más recientemente. En nuestro ejemplo, ambos orígenes de datos contienen un campo **HotelName**. Si, por alguna razón, los datos de este campo difieren para documentos con el mismo valor de calve, los datos de **HotelName** del origen de datos que se indexara más recientemente serán los que se almacenen en el índice como valor.

## <a name="search-your-json-files"></a>Buscar los archivos JSON

Puede explorar el índice de búsqueda relleno tras la ejecución del programa con el [**Explorador de búsqueda**](search-explorer.md) del portal.

En Azure Portal, abra la página **Introducción** del servicio de búsqueda y busque el índice **hotel-rooms-sample** en la lista **Índices**.

  ![Lista de índices de Azure Cognitive Search](media/tutorial-multiple-data-sources/index-list.png "Lista de índices de Azure Cognitive Search")

Haga clic en el índice hotel-rooms-sample en la lista. Verá una interfaz del Explorador de búsqueda para el índice. Escriba una consulta con un término, como "Luxury". Debería ver al menos un documento en los resultados y este documento debería mostrar una lista de objetos de las habitaciones de la matriz.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contenga el servicio Azure Cognitive Search. Para eliminar de forma definitiva todo lo que contenga el grupo de recursos, elimine el grupo. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio Azure Cognitive Search.

## <a name="next-steps"></a>Pasos siguientes

Hay varios enfoques y varias opciones para la indexación de blobs JSON. Si los datos de origen incluyen contenido JSON, puede revisar estas opciones para ver lo que funciona mejor con ese escenario.

> [!div class="nextstepaction"]
> [Indexación de blobs JSON con el indexador de blobs de Azure Cognitive Search](search-howto-index-json-blobs.md)

Es posible que desee aumentar los datos del índice estructurado desde un origen con datos con enriquecimiento cognitivo a partir de blobs desestructurados o contenido de texto. En el siguiente tutorial se muestra cómo usar Cognitive Services junto con Azure Cognitive Search mediante el SDK de .NET.

> [!div class="nextstepaction"]
> [Llamada a Cognitive Services APIs en una canalización de indexación de Azure Cognitive Search](cognitive-search-tutorial-blob-dotnet.md)
