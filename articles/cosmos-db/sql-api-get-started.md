---
title: Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB
description: Tutorial en el que se crea una base de datos en línea y la aplicación de consola de C# con la API de SQL.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: sngun
ms.openlocfilehash: a8d144b2cb8ee18c69dc4c4768b09422d44bade2
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617328"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (versión preliminar)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (versión preliminar)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Le damos la bienvenida al tutorial de introducción a la API de SQL de Azure Cosmos DB. Una vez completado este tutorial, tendrá una aplicación de consola que crea recursos de Azure Cosmos DB y los consulta.

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
>
> - Creación de una cuenta de Azure Cosmos DB y cómo conectarse a ella
> - Configuración de una solución de Visual Studio
> - Creación de una base de datos
> - Creación de una colección
> - Creación de documentos JSON
> - Consulta de la colección
> - Actualización de un documento JSON
> - Eliminar un documento
> - Eliminación de la base de datos

## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2017 con el flujo de trabajo de desarrollo de Azure instalado:
- Puede descargar y usar [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/), que es **gratuita**. Asegúrese de que habilita **Desarrollo de Azure** durante la instalación de Visual Studio. 

Una suscripción a Azure o una cuenta de evaluación gratuita de Cosmos DB:
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
  
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  
  
- Si usa el Emulador de Azure Cosmos DB, siga los pasos que se indican en [Emulador de Azure Cosmos DB](local-emulator.md) para configurarlo. A continuación, inicie el tutorial que se indica en [Configuración de la solución de Visual Studio](#SetupVS).
  
## <a name="get-the-completed-solution"></a>Obtención de la solución completa

Si no tiene tiempo para completar este tutorial, o simplemente quiere descargar los ejemplos de código, puede obtener la solución completa en [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

Para ejecutar la solución completa descargada: 

1. Asegúrese de tener instalados los [requisitos previos](#prerequisites). 
1. Abra el archivo descargado de la solución *GetStarted.sln* en Visual Studio.
1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **GetStarted** y seleccione **Administrar paquetes NuGet**.
1. En la pestaña **NuGet**, seleccione **Restaurar** para restaurar las referencias en el SDK de .NET de Azure Cosmos DB.
1. En el archivo *App.config*, actualice los valores `EndpointUrl` y `PrimaryKey` tal como se describe en la sección [Conexión a una cuenta de Azure Cosmos DB](#Connect).
1. Seleccione **Depurar** > **Iniciar sin depurar** o presione **Ctrl**+**F5** para compilar y ejecutar la aplicación.

## <a name="create-an-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Siga estas instrucciones para crear una cuenta de Azure Cosmos DB en Azure Portal. Si ya tiene una cuenta de Azure Cosmos DB, puede ir directamente a [Configuración de la solución de Visual Studio](#SetupVS). 

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Configuración de la solución de Visual Studio

1. Abra Visual Studio 2017, seleccione **Archivo** > **Nuevo** > **Proyecto**.
   
1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#** > **Aplicación de consola (.NET Framework)**, asigne un nombre al proyecto *AzureCosmosDBApp* y, luego, haga clic en **Aceptar**.
   
   ![Captura de pantalla de la ventana de nuevo proyecto](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
   
1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **AzureCosmosDBApp** y seleccione **Administrar paquetes NuGet**.
   
   ![Menú contextual del proyecto](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
   
1. En la pestaña **NuGet**, seleccione **Examinar** y escriba *azure documentdb* en el cuadro de búsqueda.
   
1. Busque y seleccione **Microsoft.Azure.DocumentDB** y, después, seleccione **Instalar** si aún no está instalado.
   
   El identificador del paquete de la biblioteca de cliente de la API de SQL de Azure Cosmos DB es [Microsoft Azure Cosmos DB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).
   
   ![Captura de pantalla del menú NuGet para encontrar el SDK de cliente de Azure Cosmos DB](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)
   
   Si recibe un mensaje sobre cómo revisar los cambios en la solución, seleccione **Aceptar**. Si recibe un mensaje acerca de la aceptación de la licencia, seleccione **Acepto**.

## <a id="Connect"></a>Conexión a una cuenta de Azure Cosmos DB

Ahora, comience a escribir código. Encontrará el archivo *Project.cs* completo para este tutorial en [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

1. En **Explorador de soluciones**, seleccione *Program.cs* y, en el editor de código, agregue las referencias siguientes al principio del archivo:
   
   ```csharp
   using System.Net;
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Newtonsoft.Json;
   ```
   
1. A continuación, agregue las siguientes dos constantes y la variable `client` a `public class Program`.
   
   ```csharp
   
   public class Program
   {
      private const string EndpointUrl = "<your endpoint URL>";
      private const string PrimaryKey = "<your primary key>";
      private DocumentClient client;
   ```
   
1. La dirección URL del punto de conexión y la clave principal permiten que la aplicación pueda conectarse a su cuenta de Azure Cosmos DB, así como que dicha cuenta confíe en la conexión. Copie las claves de [Azure Portal](https://portal.azure.com) y péguelas en el código. 

   
   1. En el panel izquierdo de la cuenta de Azure Cosmos DB, seleccione **Claves**.
      
      ![Visualización y copia de las clave de acceso en Azure Portal](./media/sql-api-get-started/nosql-tutorial-keys.png)
      
   1. En **Claves de lectura y escritura**, copie el valor de **URI** mediante el botón Copiar de la derecha y péguelo en `<your endpoint URL>` de *Program.cs*. Por ejemplo:  
      
      `private const string EndpointUrl = "https://mysqlapicosmosdb.documents.azure.com:443/";`
      
   1. Copie el valor de **PRIMARY KEY** y péguelo en `<your primary key>` de *Program.cs*. Por ejemplo:  
      
      `private const string PrimaryKey = "19ZDNJAiYL26tmnRvoez6hmtIfBGwjun50PWRjNYMC2ig8Ob9hYk7Fq1RYSv8FcIYnh1TdBISvCh7s6yyb0000==";`
   
1. Después del método `Main`, agregue una nueva tarea asincrónica denominada `GetStartedDemo`, que crea una nueva instancia de `DocumentClient` denominada `client`.
   
   ```csharp
      private async Task GetStartedDemo()
      {
        client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
      }
   ```
   
1. Agregue el siguiente código al método `Main` para ejecutar la tarea `GetStartedDemo`. El método `Main` detecta las excepciones y las escribe en la consola.
   
   ```csharp
      static void Main(string[] args)
      {
        try
        {
           Program p = new Program();
           p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
           Exception baseException = de.GetBaseException();
           Console.WriteLine($"{de.StatusCode} error occurred: {de.Message}, Message: {baseException.Message}");
        }
        catch (Exception e)
        {
           Exception baseException = e.GetBaseException();
           Console.WriteLine($"Error: {e.Message}, Message: {baseException.Message}");
        }
        finally
        {
           Console.WriteLine("End of demo, press any key to exit.");
           Console.ReadKey();
        }
      }
   ```
   
1. Presione **F5** para ejecutar la aplicación. 
   
1. Cuando aparezca el mensaje **End of demo, press any key to exit** (Final de la demostración, presione cualquier tecla para salir) en la ventana de la consola, significará que la conexión se estableció correctamente. Presione cualquier tecla para cerrar la ventana de la consola. 

Se ha conectado correctamente a su cuenta de Azure Cosmos DB. Ahora, trabaje con algunos recursos de Azure Cosmos DB.  

## <a name="create-a-database"></a>Creación de una base de datos

Una [base de datos](databases-containers-items.md#azure-cosmos-databases) de Azure Cosmos DB es un contenedor lógico de almacenamiento de documentos JSON particionado en recopilaciones. Crear una base de datos mediante el método [CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync) de la clase `DocumentClient`. 

1. Antes de agregar el código para crear una base de datos, agregue un método de asistente para escribirlo en la consola. Copie y pegue el siguiente método `WriteToConsoleAndPromptToContinue` en el código después del método `GetStartedDemo`.
   
   ```csharp
   private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
   {
      Console.WriteLine(format, args);
      Console.WriteLine("Press any key to continue...");
      Console.ReadKey();
   }
   ```
   
1. Copie y pegue la siguiente línea en el método `GetStartedDemo`, después de la línea `client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);`. Este código crea una base de datos denominada `FamilyDB`.
   
   ```csharp
      await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
   ```
   
1. Presione **F5** para ejecutar la aplicación.

Ha creado correctamente una base de datos de Azure Cosmos DB. Puede ver la base de datos en [Azure Portal](https://portal.azure.com) si selecciona **Explorador de datos** en la navegación izquierda de la cuenta de Azure Cosmos DB. 

## <a id="CreateColl"></a>Creación de una colección

Una colección es un contenedor de documentos JSON asociado a la lógica de aplicación de JavaScript. Para crear una colección, se puede usar el método [CreateDocumentCollectionIfNotExistsAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync#overloads) de la clase `DocumentClient`. 

> [!IMPORTANT]
> **CreateDocumentCollectionIfNotExistsAsync** crea una nueva colección con un rendimiento reservado, lo que tiene implicaciones en los precios. Para más información, visite la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 

1. Copie y pegue el código siguiente en el método `GetStartedDemo`, después de la línea `await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });`. Este código crea una colección de documentos denominada `FamilyCollection`.
   
   ```csharp
      await client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });
   ```
   
1. Presione **F5** para ejecutar la aplicación.

Ha creado correctamente una colección de documentos de Azure Cosmos DB. Puede ver la colección en su base de datos **FamilyDB** del **Explorador de datos** de Azure Portal.  

## <a id="CreateDoc"></a>Creación de documentos JSON

Los documentos son contenido JSON definido por el usuario y arbitrario. Los documentos deben tener una propiedad ID serializada como `id` en JSON. Cree documentos con el método [CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync#overloads) de la clase `DocumentClient`. 

> [!TIP]
> Si ya dispone de datos que quiere almacenar en la base de datos, puede usar la [herramienta de migración de datos](import-data.md) de Azure Cosmos DB para importarlos.
>

El código siguiente crea e inserta dos documentos en la colección de bases de datos. En primer lugar, cree una clase `Family` y las subclases `Parent`, `Child`, `Pet` y `Address` para usarlas en `Family`. A continuación, cree un método `CreateFamilyDocumentIfNotExists` y, después, cree e inserte dos documentos. 

1. Copie y pegue las clases `Family`, `Parent`, `Child`, `Pet` y `Address` en el código después del método `WriteToConsoleAndPromptToContinue`.
   
   ```csharp
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }
   ```
   
1. Copie y pegue el método `CreateFamilyDocumentIfNotExists` después de la clase `Address` que acaba de agregar.
   
   ```csharp
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            WriteToConsoleAndPromptToContinue($"Found {family.Id}");
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                WriteToConsoleAndPromptToContinue($"Created Family {family.Id}");
            }
            else
            {
                throw;
            }
        }
    }
   ```
   
1. Copie y pegue el código siguiente al final del método `GetStartedDemo`, después de la línea `await client.CreateDocumentCollectionIfNotExistsAsync`. Este código crea e inserta dos documentos: uno para la familia Andersen y otro para la familia Wakefield.
   
   ```csharp
    Family andersenFamily = new Family
    {
        Id = "AndersenFamily",
        LastName = "Andersen",
        Parents = new Parent[]
        {
            new Parent { FirstName = "Thomas" },
            new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
            new Child
            {
                FirstName = "Henriette Thaulow",
                Gender = "female",
                Grade = 5,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Fluffy" }
                }
            }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
        Id = "WakefieldFamily",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
            new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
            new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
            new Child
            {
                FamilyName = "Merriam",
                FirstName = "Jesse",
                Gender = "female",
                Grade = 8,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Goofy" },
                    new Pet { GivenName = "Shadow" }
                }
            },
            new Child
            {
                FamilyName = "Miller",
                FirstName = "Lisa",
                Gender = "female",
                Grade = 1
            }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);
   ```
   
1. Presione **F5** para ejecutar la aplicación.

Ha creado correctamente dos documentos de Azure Cosmos DB. Puede ver los documentos en su base de datos **FamilyDB** y en la colección **FamilyCollection** del **Explorador de datos** de Azure Portal.   

![Diagrama que muestra la relación jerárquica entre la cuenta, la base de datos en línea, la colección y los documentos](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Consulta de los recursos de Azure Cosmos DB

Azure Cosmos DB admite [consultas](how-to-sql-query.md) enriquecidas en los documentos JSON que se almacenan en las colecciones. En el siguiente código de ejemplo se usa sintaxis LINQ y SQL de Azure Cosmos DB para ejecutar una consulta en los documentos de ejemplo.

1. Copie y pegue el siguiente método `ExecuteSimpleQuery` en el código después del método `CreateFamilyDocumentIfNotExists`.
   
   ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options.
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Find the Andersen family by its LastName.
        IQueryable<Family> familyQuery = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
            .Where(f => f.LastName == "Andersen");

        // Execute the query synchronously. 
        // You could also execute it asynchronously using the IDocumentQuery<T> interface.
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine($"\tRead {family}");
        }

        // Now execute the same query using direct SQL.
        IQueryable<Family> familyQueryInSql = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
            "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
            queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
            Console.WriteLine($"\tRead {family}");
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
   ```
   
1. Copie y pegue el código siguiente al final del método `GetStartedDemo`, después de la línea `await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);`.
   
   ```csharp
      ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Presione **F5** para ejecutar la aplicación.

La consulta anterior devuelve el elemento completo para la familia Andersen. Ha consultado correctamente una colección de Azure Cosmos DB.

En el siguiente diagrama se muestra cómo llama la sintaxis de la consulta SQL de Azure Cosmos DB a la colección. La misma lógica se aplica a la consulta LINQ.

![Diagrama que ilustra el ámbito y el significado de la consulta usada por el tutorial de NoSQL para crear una aplicación de consola de C#.](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

La palabra clave [FROM](how-to-sql-query.md#FromClause) es opcional en la consulta SQL, ya que las consultas de Azure Cosmos DB ya tienen como ámbito una única colección. Puede intercambiar `FROM Families f` por `FROM root r`, o cualquier otro nombre de variable que elija. Azure Cosmos DB deducirá que `Families`, `root` o el nombre de variable elegido hace referencia a la colección actual.

## <a id="ReplaceDocument"></a>Actualización de un documento JSON

La API de SQL de Azure Cosmos DB permite actualizar y reemplazar documentos JSON.  

1. Copie y pegue el siguiente método `ReplaceFamilyDocument` en el código después del método `ExecuteSimpleQuery`.
   
   ```csharp
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
       await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
       WriteToConsoleAndPromptToContinue($"Replaced Family {familyName}");
    }
   ```
   
1. Copie y pegue el código siguiente al final del método `GetStartedDemo`, después de la línea `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");`. El código actualiza los datos en uno de los documentos y, después, ejecuta la consulta de nuevo para mostrar el documento modificado.
   
   ```csharp
   // Update the Grade of the Andersen Family child
   andersenFamily.Children[0].Grade = 6;
   await ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily", andersenFamily);
   ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Presione **F5** para ejecutar la aplicación.

El resultado de consulta muestra que el método `Grade` para el elemento secundario de la familia Andersen se actualizó de `5` a `6`. Ha actualizado y sustituido correctamente un documento de Azure Cosmos DB. 

## <a id="DeleteDocument"></a>Eliminación de un documento JSON

La API de SQL de Azure Cosmos DB admite la eliminación de documentos JSON.  

1. Copie y pegue el siguiente método `DeleteFamilyDocument` después del método `ReplaceFamilyDocument`.
   
   ```csharp
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine($"Deleted Family {documentName}");
    }
   ```
   
1. Copie y pegue el código siguiente al final del método `GetStartedDemo`, después de la segunda línea `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");`.
   
   ```csharp
   await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");
   ```
   
1. Presione **F5** para ejecutar la aplicación.

Ha eliminado correctamente un documento de Azure Cosmos DB. 

## <a id="DeleteDatabase"></a>Eliminación de la base de datos

Elimine la base de datos que ha creado para quitarla y quitar todos sus recursos secundarios, incluidos la colección y los documentos. 

1. Copie y pegue el código siguiente al final del método `GetStartedDemo`, después de la línea `await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");`. 
   
   ```csharp
   // Clean up - delete the database
   await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));
   ```
   
1. Presione **F5** para ejecutar la aplicación.

Ha eliminado correctamente la base de datos de Azure Cosmos DB. Puede ver en el **Explorador de datos** de su cuenta de Azure Cosmos DB que la base de datos FamilyDB se eliminó. 

## <a id="Run"></a>Ejecución de toda la aplicación de consola C#

Presione **F5** en Visual Studio para compilar y ejecutar toda la aplicación de consola C# en modo de depuración. Debería ver el resultado siguiente en la ventana de la consola:

```bash
Created Family AndersenFamily
Press any key to continue ...
 Created Family WakefieldFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Replaced Family AndersenFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Deleted Family AndersenFamily
End of demo, press any key to exit.
```

Felicidades. Ha completado el tutorial y ya dispone de una aplicación de consola C# en funcionamiento que crea, consulta, actualiza y elimina recursos de Azure Cosmos DB.  

## <a name="next-steps"></a>Pasos siguientes
* Para más información sobre Azure Cosmos DB, consulte [Bienvenido a Azure Cosmos DB](introduction.md).
* Para ver un tutorial de ASP.NET MVC más complejo, vea [Tutorial de ASP.NET MVC: Desarrollo de aplicaciones web con Azure Cosmos DB](sql-api-dotnet-application.md).
* Para realizar pruebas de escala y rendimiento con Azure Cosmos DB, consulte [Pruebas de escala y rendimiento con Azure Cosmos DB](performance-testing.md).
* Para obtener más información sobre cómo supervisar las solicitudes, el uso y el almacenamiento de Azure Cosmos DB, vea [Supervisión de cuentas](monitor-accounts.md).
* Ejecute las consultas en un conjunto de datos de ejemplo en el [área de consultas](https://www.documentdb.com/sql/demo).

