---
title: 'Tutorial: Indexación de datos en C# a partir de bases de datos de Azure SQL'
titleSuffix: Azure Cognitive Search
description: En este tutorial, se conectará a una base de datos de Azure SQL, extraerá datos que permiten búsquedas y los cargará en un índice de Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/26/2020
ms.openlocfilehash: 978587b68e719b79db31ff25adaf2b38d2235095
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650121"
---
# <a name="tutorial-index-azure-sql-data-in-c-using-azure-cognitive-search-indexers"></a>Tutorial: Indexación de datos de Azure SQL en C# mediante los indexadores de Azure Cognitive Search

Utilice C# para configurar un [indexador](search-indexer-overview.md) que extraiga datos que permitan búsquedas de Azure SQL Database y los envíe a un índice de búsqueda. En este tutorial se usan las [bibliotecas cliente .NET de Azure Cognitive Search](https://aka.ms/search-sdk) y una aplicación de consola de .NET Core para realizar las siguientes tareas:

> [!div class="checklist"]
> * Creación de un origen de datos que se conecte a Azure SQL Database
> * Configuración de un indexador
> * Ejecución de un indexador para cargar datos en un índice
> * Consulta de un índice como paso de comprobación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

+ [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Creación](search-create-service-portal.md) o [búsqueda de un servicio de búsqueda existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) 

> [!Note]
> Puede usar un servicio gratuito para este tutorial. Un servicio de búsqueda gratuito tiene una limitación de tres índices, tres indexadores y tres orígenes de datos. En este tutorial se crea uno de cada uno. Antes de empezar, asegúrese de que haya espacio en el servicio para aceptar los nuevos recursos.

## <a name="download-source-code"></a>Descarga del código fuente

El código fuente de este tutorial se encuentra en la carpeta [DotNetHowToIndexer](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) del repositorio [Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) de GitHub.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas API requieren la dirección URL del servicio y una clave de acceso. Con ambos se crea un servicio de búsqueda, por lo que, si ha agregado Azure Cognitive Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

   ![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-get-started-postman/get-url-key.png "Obtención de una clave de acceso y un punto de conexión HTTP")

## <a name="set-up-connections"></a>Configuración de las conexiones

1. Inicie Visual Studio y abra **DotNetHowToIndexers.sln**.

1. En Explorador de soluciones, abra **appsettings.json** y reemplace los valores de los marcadores de posición por la información de conexión al servicio de búsqueda. Si la dirección URL completa es "https://my-demo-service.search.windows.net", el nombre del servicio que se va a proporcionar es "my-demo-service".

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
    }
    ```

La última entrada requiere una base de datos existente. La creará en el paso siguiente.

## <a name="prepare-sample-data"></a>Preparación de datos de ejemplo

En este paso creará un origen de datos externo en Azure SQL Database que se pueda rastrear con un indexador. Puede usar Azure Portal y el archivo *hotels.sql* del ejemplo para crear el conjunto de datos en Azure SQL Database. Azure Cognitive Search usa conjuntos de filas planas, como los que se generan a partir de una vista o una consulta. El archivo SQL de la solución de ejemplo crea y rellena una sola tabla.

Si tiene un recurso de Azure SQL Database existente, puede agregarle la tabla de hoteles, a partir del paso 4.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/).

1. Busque o cree una instancia de **SQL Database**. Puede usar los valores predeterminados y el plan de tarifa más bajo. Una ventaja de la creación de un servidor es que puede especificar un nombre de usuario y una contraseña de administrador, que son necesarios para crear y cargar las tablas en un paso posterior.

   ![Página de base de datos nueva](./media/search-indexer-tutorial/indexer-new-sqldb.png "Página Nueva base de datos")

1. Haga clic en **Revisar y crear** para implementar el nuevo servidor y la nueva base de datos. Espere a que se implementen el servidor y la base de datos.

1. En el panel de navegación, haga clic en **Editor de consultas (versión preliminar)** y escriba el nombre de usuario y la contraseña del administrador del servidor. 

   Si se le deniega el acceso, copie la dirección IP del cliente del mensaje de error y haga clic en el vínculo **Establecer el firewall del servidor** para agregar una regla que permita el acceso desde el equipo cliente, mediante la dirección IP del cliente para el intervalo. Puede que la regla tarde varios minutos en aplicarse.

1. En el editor de consultas, haga clic en **Abrir consulta** y vaya a la ubicación del archivo *hotels.sql* en el equipo local. 

1. Seleccione el archivo y haga clic en **Abrir**. El script debe tener un aspecto similar a la siguiente captura de pantalla:

   ![Script de SQL](./media/search-indexer-tutorial/sql-script.png "Script de SQL")

1. Haga clic en **Ejecutar** para ejecutar la consulta. En el panel de resultados, verá un mensaje de consulta correcta, correspondiente a 3 filas.

1. Para devolver un conjunto de filas de esta tabla, puede ejecutar la siguiente consulta como paso de comprobación:

    ```sql
    SELECT * FROM Hotels
    ```

1. Copie la cadena de conexión ADO.NET correspondiente a la base de datos. En **Configuración** > **Cadenas de conexión**, copie la cadena de conexión ADO.NET, como en el ejemplo siguiente.

    ```sql
    Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```

1. Pegue la cadena de conexión en "AzureSqlConnectionString" como tercera entrada en el archivo **appsettings.json** en Visual Studio.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:{your_dbname}.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

1. Escriba la contraseña en la cadena de conexión del archivo **appsettings.json**. La base de datos y los nombres de usuario se copiarán en la cadena de conexión, pero la contraseña debe escribirse manualmente.

## <a name="build-the-solution"></a>Compile la solución

Presione F5 para compilar la solución. El programa se ejecuta en modo de depuración. Una ventana de consola informa del estado de cada operación.

   ![Salida de consola](./media/search-indexer-tutorial/console-output.png "Salida de consola")

El código se ejecuta localmente en Visual Studio y se conecta al servicio de búsqueda en Azure, que, a su vez, se conecta a Azure SQL Database y recupera el conjunto de datos. Con estas muchas operaciones, hay varios puntos de error posibles. Si recibe un error, compruebe primero las condiciones siguientes:

+ La información de conexión del servicio de búsqueda que proporciona está limitada al nombre del servicio en este tutorial. Si escribió la dirección URL completa, las operaciones se detienen durante la creación del índice, con un error de conexión.

+ La información de conexión de la base de datos **appsettings.json**. Debe ser la cadena de conexión de ADO.NET obtenida del portal, modificada para incluir un nombre de usuario y una contraseña que sean válidos para la base de datos. La cuenta de usuario debe tener permiso para recuperar datos. Se debe permitir el acceso a la dirección IP del cliente local.

+ Los límites de recursos. Recuerde que el nivel Gratis tiene un límite de tres índices, tres indexadores y tres orígenes de datos. Un servicio en el límite máximo no puede crear nuevos objetos.

## <a name="check-results"></a>Comprobar los resultados

Use Azure Portal para comprobar la creación de objetos y use **Explorador de búsqueda** para consultar el índice.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Información general** del servicio de búsqueda, abra cada lista correspondiente para comprobar que el objeto se ha creado. **Índices**, **Indexadores** y **Orígenes de datos** contendrán "hotels", "azure-sql-indexer" y "azure-sql", respectivamente.

   ![Iconos de indexador y origen de datos](./media/search-indexer-tutorial/tiles-portal.png)

1. Seleccione el índice hotels. En la página de hoteles, **Explorador de búsqueda** es la primera pestaña. 

1. Haga clic en **Buscar** para emitir una consulta vacía. 

   Las tres entradas del índice se devuelven como documentos JSON. El Explorador de búsqueda devuelve documentos en JSON para que pueda ver la estructura completa.

   ![Consulta de un índice](./media/search-indexer-tutorial/portal-search.png "Consultar un índice")
   
1. A continuación, escriba una cadena de búsqueda: `search=river&$count=true`. 

   Esta consulta invoca la búsqueda de texto completo en el término `river`, y el resultado incluye un recuento de los documentos coincidentes. Devolver el número de documentos coincidencias es útil en escenarios de pruebas cuando se tiene un índice grande con miles o millones de documentos. En este caso, solo un documento coincide con la consulta.

1. Por último, escriba una cadena de búsqueda que limite la salida JSON a los campos de interés: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   La respuesta de la consulta se reduce a los campos seleccionados, lo que da lugar a una salida más concisa.

## <a name="explore-the-code"></a>Exploración del código

Ahora que sabe lo que crea el código de ejemplo, volvamos a la solución para revisarlo. El código pertinente aparece en dos archivos:

  + **hotel.cs**, que contiene un esquema que define el índice
  + **Program.cs**, que contiene las funciones para crear y administrar las estructuras del servicio

### <a name="in-hotelcs"></a>En hotel.cs

El esquema de índice define la colección de campos, incluidos los atributos que especifican las operaciones permitidas; por ejemplo, si un campo de texto completo permite búsquedas, filtros o clasificación, como se muestra en la siguiente definición de campo para HotelName. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Un esquema también puede incluir otros elementos, como los perfiles de puntuación para aumentar una puntuación de búsqueda, analizadores personalizados y otras construcciones. Sin embargo, para nuestros fines, el esquema se define de forma dispersa, estando compuesto solo de los campos encontrados en los conjuntos de datos de ejemplo.

### <a name="in-programcs"></a>En Program.cs

El programa principal incluye lógica para la creación de un cliente, un índice, un origen de datos y un indexador. El código busca y elimina los recursos existentes del mismo nombre, bajo el supuesto de que este programa se podría ejecutar varias veces.

El objeto de origen de datos está configurado con valores específicos de los recursos de Azure SQL Database, [indexación parcial o incremental](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) incluida, para aprovechar las [características de detección de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) integradas de Azure SQL. La base de datos de demostración de hoteles de Azure SQL tiene una columna de "eliminación temporal" denominada **IsDeleted**. Cuando esta columna se establece en true en la base de datos, el indexador quita el documento correspondiente del índice de Azure Cognitive Search.

  ```csharp
  Console.WriteLine("Creating data source...");

  DataSource dataSource = DataSource.AzureSql(
      name: "azure-sql",
      sqlConnectionString: configuration["AzureSQLConnectionString"],
      tableOrViewName: "hotels",
      deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
          softDeleteColumnName: "IsDeleted",
          softDeleteMarkerValue: "true"));
  dataSource.DataChangeDetectionPolicy = new SqlIntegratedChangeTrackingPolicy();

  searchService.DataSources.CreateOrUpdateAsync(dataSource).Wait();
  ```

Los objetos de indexador son independientes de las plataformas, donde la configuración la programación y la invocación son siempre los mismos, no importa el origen. Este indexador de ejemplo incluye un programa, una opción de restablecimiento que elimina su historial y llama un método para crear el indexador y ejecutarlo inmediatamente.

  ```csharp
  Console.WriteLine("Creating Azure SQL indexer...");
  Indexer indexer = new Indexer(
      name: "azure-sql-indexer",
      dataSourceName: dataSource.Name,
      targetIndexName: index.Name,
      schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
  // Indexers contain metadata about how much they have already indexed
  // If we already ran the sample, the indexer will remember that it already
  // indexed the sample data and not run again
  // To avoid this, reset the indexer if it exists
  exists = await searchService.Indexers.ExistsAsync(indexer.Name);
  if (exists)
  {
      await searchService.Indexers.ResetAsync(indexer.Name);
  }

  await searchService.Indexers.CreateOrUpdateAsync(indexer);

  // We created the indexer with a schedule, but we also
  // want to run it immediately
  Console.WriteLine("Running Azure SQL indexer...");

  try
  {
      await searchService.Indexers.RunAsync(indexer.Name);
  }
  catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
  {
      Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
  }
  ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando trabaje con su propia suscripción, al final de un proyecto, es recomendable eliminar los recursos que ya no necesite. Los recursos que se dejan en ejecución pueden costarle mucho dinero. Puede eliminar los recursos de forma individual o eliminar el grupo de recursos para eliminar todo el conjunto de recursos.

Puede buscar y administrar los recursos en el portal, mediante el vínculo Todos los recursos o Grupos de recursos en el panel de navegación izquierdo.

Si está usando un servicio gratuito, recuerde que está limitado a tres índices, indexadores y orígenes de datos. Puede eliminar elementos individuales en el portal para mantenerse por debajo del límite.

## <a name="next-steps"></a>Pasos siguientes

En Azure Cognitive Search, los indexadores están disponibles para varios orígenes de datos de Azure. Como paso siguiente, explore el indexador de Azure Blob Storage.

> [!div class="nextstepaction"]
> [Indexación de documentos en Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)