---
title: 'Tutorial: Indexación de datos a partir de bases de datos de Azure SQL en un código de ejemplo de C#: Azure Search'
description: Un código de ejemplo en C# le mostrará cómo conectarse a Azure SQL Database, extraer datos que permitan búsquedas y que se cargarán en un índice de Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: bb082fb83f8c2521b0deabced6f851e62b785e8f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485389"
---
# <a name="c-tutorial-crawl-an-azure-sql-database-using-azure-search-indexers"></a>Tutorial de C#: Cómo rastrear una base de datos de Azure SQL mediante los indexadores de Azure Search

Aprenda a configurar un indexador para extraer datos que permiten búsquedas de una base de datos de Azure SQL de ejemplo. [Los indexadores](search-indexer-overview.md) son un componente de Azure Search que rastrean orígenes de datos externos y rellenan un [índice de búsqueda](search-what-is-an-index.md) con contenido. De todos los indexadores, el más usado es el indexador de Azure SQL Database. 

Tener un profundo conocimiento de la configuración del indexador resulta de utilidad porque simplifica la cantidad de código que se tiene que escribir y mantener. En lugar de preparar e insertar un conjunto de datos JSON conforme a un esquema, puede asociar un indexador a un origen de datos, hacer que el indexador extraiga los datos y los inserte en un índice y, luego, de manera opcional, puede ejecutar el indexador según una programación recurrente para seleccionar los cambios en el origen subyacente.

En este tutorial, se usarán las [bibliotecas cliente .NET de Azure Search](https://aka.ms/search-sdk) y una aplicación de consola de .NET Core para realizar las siguientes tareas:

> [!div class="checklist"]
> * Agregar información del servicio de búsqueda a la configuración de la aplicación
> * Preparar un conjunto de datos externo en la base de datos de Azure SQL 
> * Revisar las definiciones de índice e indexador en el código de ejemplo
> * Ejecutar el código de indexador para importar los datos
> * Buscar en el índice
> * Ver la configuración del indexador en el portal

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se usan los siguientes servicios, herramientas y datos. 

[Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) almacena el origen de datos externo usado por un indexador. La solución de ejemplo proporciona un archivo de datos SQL para crear la tabla. En este tutorial se indican los pasos para crear el servicio y la base de datos.

Se puede usar cualquier edición de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) para ejecutar la solución de ejemplo. Se han probado código de ejemplo e instrucciones en la edición Community Edition gratuita.

En [Azure-Samples/search-dotnet-getting-started](https://github.com/Azure-Samples/search-dotnet-getting-started) puede encontrar la solución de ejemplo, en el repositorio de GitHub de ejemplos de Azure. Descargue y extraiga la solución. De forma predeterminada, las soluciones son de solo lectura. Haga clic con el botón derecho en la solución y borre el atributo de solo lectura para que se puedan modificar los archivos.

> [!Note]
> Si va a usar el servicio gratuito de Azure Search, está limitado a tres índices, tres indexadores y tres orígenes de datos. En este tutorial se crea uno de cada uno. Asegúrese de que haya espacio en el servicio para aceptar los nuevos recursos.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas de REST requieren la dirección URL del servicio y una clave de acceso en cada solicitud. Con ambos se crea un servicio de búsqueda, por lo que si ha agregado Azure Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-get-started-postman/get-url-key.png "Get an HTTP endpoint and access key")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio. Tener una clave válida genera la confianza, solicitud a solicitud, entre la aplicación que envía la solicitud y el servicio que se encarga de ella.

## <a name="set-up-connections"></a>Configuración de las conexiones
La información de conexión para los servicios requeridos se especifica en el archivo **appsettings.json** de la solución. 

1. En Visual Studio, abra el archivo **DotNetHowToIndexers.sln**.

1. En el Explorador de soluciones, abra **appsettings.json** para que pueda rellenar cada valor de configuración.  

Las dos primeras entradas las puede rellenar en este mismo momento mediante las claves de dirección URL y de administrador del servicio Azure Search. Dado un punto de conexión de `https://mydemo.search.windows.net`, el nombre del servicio que debe proporcionar es `mydemo`.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "AzureSqlConnectionString": "Put your Azure SQL database connection string here",
}
```

La última entrada requiere una base de datos existente. La creará en el paso siguiente.

## <a name="prepare-sample-data"></a>Preparación de datos de ejemplo

En este paso, creará un origen de datos externo que se pueda rastrear con un indexador. Puede usar Azure Portal y el archivo *hotels.sql* del ejemplo para crear el conjunto de datos en Azure SQL Database. Azure Search usa conjuntos de filas planas, como los que se generan a partir de una vista o una consulta. El archivo SQL de la solución de ejemplo crea y rellena una sola tabla.

En el ejercicio siguiente se da por supuesto que no hay ningún servidor ni base de datos, y se le indica que cree ambos en el paso 2. Opcionalmente, si tiene un recurso existente, puede agregar en él la tabla de hoteles, a partir del paso 4.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/). 

2. Busque o cree una instancia de **Azure SQL Database** para crear una base de datos, un servidor y un grupo de recursos. Puede usar los valores predeterminados y el plan de tarifa más bajo. Una ventaja de la creación de un servidor es que puede especificar un nombre de usuario y una contraseña de administrador, que son necesarios para crear y cargar las tablas en un paso posterior.

   ![Página Nueva base de datos](./media/search-indexer-tutorial/indexer-new-sqldb.png)

3. Haga clic en **Crear** para implementar el nuevo servidor y la nueva base de datos. Espere a que se implementen el servidor y la base de datos.

4. Abra la página SQL Database de la nueva base de datos, si aún no está abierta. En el nombre del recurso debe poner *SQL Database* y no *SQL Server*.

   ![Página SQL Database](./media/search-indexer-tutorial/hotels-db.png)

4. En el panel de navegación, haga clic en **Editor de consultas (versión preliminar)** .

5. Haga clic en **Iniciar sesión** y escriba el nombre de usuario y la contraseña del administrador del servidor.

6. Haga clic en **Abrir consulta** y navegue a la ubicación de *hotels.sql*. 

7. Seleccione el archivo y haga clic en **Abrir**. El script debe tener un aspecto similar a la siguiente captura de pantalla:

   ![Script de SQL](./media/search-indexer-tutorial/sql-script.png)

8. Haga clic en **Ejecutar** para ejecutar la consulta. En el panel de resultados, verá un mensaje de consulta correcta, correspondiente a 3 filas.

9. Para devolver un conjunto de filas de esta tabla, puede ejecutar la siguiente consulta como paso de comprobación:

    ```sql
    SELECT HotelId, HotelName, Tags FROM Hotels
    ```
    La consulta prototipo, `SELECT * FROM Hotels`, no funciona en el Editor de consultas. Los datos de ejemplo incluyen coordenadas geográficas en el campo Ubicación, que no se gestionan en el editor por el momento. Para obtener una lista de otras columnas para consultar, puede ejecutar esta instrucción:`SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.Hotels')`

10. Ahora que tiene un conjunto de datos externo, copie la cadena de conexión de ADO.NET correspondiente a la base de datos. En la página SQL Database de la base de datos, vaya a **Configuración** > **Cadenas de conexión** y copie la cadena de conexión de ADO.NET.
 
    Una cadena de conexión de ADO.NET se parece al ejemplo siguiente, modificada para usar un nombre de base de datos, un nombre de usuario y una contraseña válidos.

    ```sql
    Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
    ```
11. Pegue la cadena de conexión en "AzureSqlConnectionString" como tercera entrada en el archivo **appsettings.json** en Visual Studio.

    ```json
    {
      "SearchServiceName": "<placeholder-Azure-Search-service-name>",
      "SearchServiceAdminApiKey": "<placeholder-admin-key-for-Azure-Search>",
      "AzureSqlConnectionString": "Server=tcp:hotels-db.database.windows.net,1433;Initial Catalog=hotels-db;Persist Security  Info=False;User ID={your_username};Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    }
    ```

## <a name="understand-the-code"></a>Comprendiendo el código

Una vez que los valores de configuración y los datos estén en su lugar, el ejemplo de programa de **DotNetHowToIndexers.sln** estará listo para la compilación y la ejecución. Antes de hacerlo, dedique un minuto a estudiar las definiciones de índice e indexador de este ejemplo. El código pertinente aparece en dos archivos:

  + **hotel.cs**, que contiene el esquema que define el índice.
  + **Program.cs**, que contiene las funciones para crear y administrar las estructuras del servicio.

### <a name="in-hotelcs"></a>En hotel.cs

El esquema de índice define la colección de campos, incluidos los atributos que especifican las operaciones permitidas; por ejemplo, si un campo de texto completo permite búsquedas, filtros o clasificación, como se muestra en la siguiente definición de campo para HotelName. 

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
```

Un esquema también puede incluir otros elementos, como los perfiles de puntuación para aumentar una puntuación de búsqueda, analizadores personalizados y otras construcciones. Sin embargo, para nuestros fines, el esquema se define de forma dispersa, estando compuesto solo de los campos encontrados en los conjuntos de datos de ejemplo.

En este tutorial, el indexador extrae los datos de un origen de datos. En la práctica, puede asociar varios indexadores al mismo índice y crear un índice de búsqueda consolidado a partir de varios orígenes de datos. Puede usar el mismo par de indexador-índice y variar solo los orígenes de datos, o un índice con varias combinaciones de indexador y origen de datos, según dónde necesite la flexibilidad.

### <a name="in-programcs"></a>En Program.cs

El programa principal incluye lógica para la creación de un cliente, un índice, un origen de datos y un indexador. El código busca y elimina los recursos existentes del mismo nombre, bajo el supuesto de que este programa se podría ejecutar varias veces.

El objeto de origen de datos está configurado con valores específicos de los recursos de Azure SQL Database, [indexación incremental](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) incluida, para aprovechar las [características de detección de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) integradas de Azure SQL. La base de datos de demostración de hoteles de Azure SQL tiene una columna de "eliminación temporal" denominada **IsDeleted**. Cuando esta columna se establece en true en la base de datos, el indexador quita el documento correspondiente del índice de Azure Search.

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



## <a name="run-the-indexer"></a>Ejecución del indexador

En este paso, compile y ejecute el programa. 

1. En el Explorador de soluciones, haga clic con el botón derecho en **DotNetHowToIndexers** y seleccione **Compilar**.
2. De nuevo, haga clic con el botón derecho en **DotNetHowToIndexers**, seguido de **Depurar** > **Iniciar nueva instancia**.

El programa se ejecuta en modo de depuración. Una ventana de consola informa del estado de cada operación.

  ![Script de SQL](./media/search-indexer-tutorial/console-output.png)

El código se ejecuta localmente en Visual Studio y se conecta al servicio de búsqueda en Azure, que, a su vez, usa la cadena de conexión para conectarse a Azure SQL Database y recuperar el conjunto de datos. Con estas muchas operaciones, existen varios posibles puntos de error, pero si recibe un error, compruebe primero las siguientes condiciones:

+ La información de conexión del servicio de búsqueda que proporciona está limitada al nombre del servicio en este tutorial. Si escribió la dirección URL completa, las operaciones se detienen durante la creación del índice, con un error de conexión.

+ La información de conexión de la base de datos **appsettings.json**. Debe ser la cadena de conexión de ADO.NET obtenida del portal, modificada para incluir un nombre de usuario y una contraseña que sean válidos para la base de datos. La cuenta de usuario debe tener permiso para recuperar datos.

+ Los límites de recursos. Recuerde que el servicio compartido (gratuito) tiene límites de tres índices, tres indexadores y tres orígenes de datos. Un servicio en el límite máximo no puede crear nuevos objetos.

## <a name="search-the-index"></a>Búsqueda en el índice 

En Azure Portal, en la página Información general del servicio de búsqueda, haga clic en **Explorador de búsqueda** en la parte superior para enviar algunas consultas sobre el nuevo índice.

1. Haga clic en **Cambiar índice** en la parte superior para seleccionar el índice *hotels*.

2. Haga clic en el botón **Buscar** para emitir una búsqueda vacía. 

   Las tres entradas del índice se devuelven como documentos JSON. El Explorador de búsqueda devuelve documentos en JSON para que pueda ver la estructura completa.

3. A continuación, escriba una cadena de búsqueda: `search=river&$count=true`. 

   Esta consulta invoca la búsqueda de texto completo en el término `river`, y el resultado incluye un recuento de los documentos coincidentes. Devolver el número de documentos coincidencias es útil en escenarios de pruebas cuando se tiene un índice grande con miles o millones de documentos. En este caso, solo un documento coincide con la consulta.

4. Por último, escriba una cadena de búsqueda que limite la salida JSON a los campos de interés: `search=river&$count=true&$select=hotelId, baseRate, description`. 

   La respuesta de la consulta se reduce a los campos seleccionados, lo que da lugar a una salida más concisa.

## <a name="view-indexer-configuration"></a>Visualización de la configuración del indexador

Todos los indexadores, incluido el que acaba de crear mediante programación, se muestran en el portal. Puede abrir una definición de indexador y ver su origen de datos, o configurar una programación de actualización para seleccionar las filas nuevas y modificadas.

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y, en la página **Información general** del servicio de búsqueda, haga clic en los vínculos de **Índices**, **Indexadores** y  **Orígenes de datos**.
3. Seleccione algún objeto individual para ver sus valores de configuración o modificarlos.

   ![Iconos de indexador y origen de datos](./media/search-indexer-tutorial/tiles-portal.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contiene el servicio Azure Search. Para eliminar de forma definitiva todo lo que contenga el grupo de recursos, elimine el grupo. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio Azure Search.

## <a name="next-steps"></a>Pasos siguientes

Puede asociar algoritmos de enriquecimiento por IA a una canalización del indexador. Como paso siguiente, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Indexación de documentos en Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)