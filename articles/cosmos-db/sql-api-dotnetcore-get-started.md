---
title: 'Tutorial: Creación de una aplicación .NET Core para administrar los datos almacenados de la cuenta de API de SQL para Azure Cosmos DB'
description: En este tutorial se crea una base de datos en línea y la aplicación de consola de C# mediante el SDK de .NET Core de la API de SQL para Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: sngun
Customer intent: As a developer, I want to build a .NET Core application to access and manage Azure Cosmos DB resources so that customers can utilize the global distribution, elastic scaling, multi-master, and other capabilities that Azure Cosmos DB offers.
ms.openlocfilehash: 81c80dbd7ffa8e5c4e7b2ebb1c9d17eb6007ae9f
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802358"
---
# <a name="tutorial-build-a-net-core-app-to-manage-data-stored-in-a-sql-api-account"></a>Tutorial: Creación de una aplicación .NET Core para administrar los datos almacenados de una cuenta de API de SQL

> [!div class="op_single_selector"]
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (versión preliminar)](sql-api-dotnet-core-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [.NET (versión preliminar)](sql-api-dotnet-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Como desarrollador, puede que tenga aplicaciones que usan datos de documentos NoSQL. Puede usar la cuenta de la API de SQL en Azure Cosmos DB para almacenar datos de documentos y acceder a dichos datos. En este tutorial se muestra cómo crear una aplicación .NET Core para crear y consultar los datos almacenados de la cuenta de la API de SQL de Azure Cosmos DB. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Creación de una cuenta de Azure Cosmos DB y conexión a ella
> * Configuración de la solución de Visual Studio
> * Creación de una base de datos en línea
> * Creación de una colección
> * Creación de documentos JSON
> * Realización de operaciones CRUD sobre los elementos, el contenedor y la base de datos

¿No tiene tiempo a crear la aplicación? La solución completa está disponible en [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). 

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Descargue y use [edición gratuita de Visual Studio 2017 Community](https://www.visualstudio.com/downloads/). Si está desarrollando una aplicación de Plataforma universal de Windows (UWP), debe usar **Visual Studio 2017 con la versión 15.4** o superior. Asegúrese de que habilita **Desarrollo de Azure** durante la instalación de Visual Studio.

    * Para MacOS o Linux, puede desarrollar aplicaciones de .NET Core desde la línea de comandos mediante la instalación del [SDK de .NET Core](https://www.microsoft.com/net/core#macos) para la plataforma que elija. 

    * Para Windows, puede desarrollar aplicaciones de .NET Core desde la línea de comandos mediante la instalación del [SDK de .NET Core](https://www.microsoft.com/net/core#windows). 

## <a name="create-an-azure-cosmos-account"></a>Creación de una cuenta de Azure Cosmos

Siga estos pasos para crear una cuenta de Azure Cosmos:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Configuración de la solución de Visual Studio

1. Abra **Visual Studio 2017** en el equipo.

2. En el menú **Archivo**, seleccione **Nuevo** y elija **Proyecto**.

3. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Plantillas** > **Visual C#** > **.NET Core** > **Aplicación de consola (.NET Core)**, asigne un nombre a su proyecto **DocumentDBGettingStarted** y luego seleccione **Aceptar**.

   ![Captura de pantalla de la ventana de nuevo proyecto](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. En el **Explorador de soluciones**, haga clic con el botón derecho en **DocumentDBGettingStarted**.

5. En el mismo menú, seleccione **Administrar paquetes NuGet**.

   ![Captura de pantalla del menú contextual del proyecto](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. En la pestaña **NuGet**, seleccione **Examinar** en la parte superior de la ventana y escriba **azure documentdb** en el cuadro de búsqueda. Asegúrese de que la casilla **Incluir versión preliminar** está seleccionada.

7. En los resultados, busque **Microsoft.Azure.DocumentDB.Core** y haga clic en **Instalar**.

   El identificador del paquete para la biblioteca es [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Si su destino es una versión de .NET Framework (como net461) incompatible con este paquete de NuGet para .NET Core, use [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB), que es compatible con todas las versiones de .NET Framework a partir de .NET Framework 4.5.

8. Cuando se le solicite, acepte el contrato de licencia y las instalaciones de los paquetes NuGet.

Ahora que ha finalizado el programa de instalación, puede empezar a escribir código. Puede buscar un proyecto con código completo de este tutorial en [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).

## <a id="Connect"></a>Conexión a una cuenta de Azure Cosmos

Conéctese a una cuenta de Azure Cosmos importando las dependencias necesarias. Para importar las dependencias, agregue el código siguiente al principio del archivo Program.cs:

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

Después, agregue estas dos constantes y la variable *client* debajo de la clase pública *Program*.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Seguidamente, vaya a [Azure Portal](https://portal.azure.com) para recuperar el identificador URI y la clave principal. El URI y la clave principal de Azure Cosmos DB son necesarios para que la aplicación sepa a dónde debe conectarse y para que Azure Cosmos DB confíe en la conexión de la aplicación.

En Azure Portal, vaya a la cuenta de Azure Cosmos y, después, seleccione **Claves**.

Copie el URI desde el Portal y péguelo en `<your endpoint URI>` en el archivo program.cs. Después, copie la CLAVE PRINCIPAL del Portal y péguela en `<your key>`. Asegúrese de quitar el < and > pero deje las comillas dobles alrededor de la clave y el punto de conexión.

![Obtención de claves en Azure Portal][keys]

Iniciaremos la aplicación de introducción, para lo que crearemos una nueva instancia de **DocumentClient**.

Debajo del método **Main**, agregue la nueva tarea asincrónica denominada **GetStartedDemo**, que creará una instancia del nuevo **DocumentClient**.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Agregue el siguiente código para ejecutar la tarea asincrónica desde el método **Main** . El método **Main** detecta las excepciones y las escribe en la consola.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

Seleccione el botón **DocumentDBGettingStarted** para crear y ejecutar la aplicación.

## <a id="CreateDatabase"></a>Creación de una base de datos

Antes de agregar el código para crear una base de datos, agregue un método de asistente para escribirlo en la consola. Copie y pegue el método **WriteToConsoleAndPromptToContinue** debajo del método **GetStartedDemo**.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Cree su base de datos de Azure Cosmos DB mediante el método `CreateDatabaseAsync` de la clase **DocumentClient**. Una base de datos es un contenedor lógico de almacenamiento de documentos JSON particionado en recopilaciones. Copie y pegue el código siguiente en el método **GetStartedDemo** debajo de la creación del cliente. Esto crea una base de datos denominada *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

## <a id="CreateColl"></a>Creación de una colección

Cree una colección usando el método `CreateDocumentCollectionAsync` de la clase **DocumentClient**. Una colección es un contenedor de documentos JSON asociado a la lógica de aplicación de JavaScript.

> [!WARNING]
> **CreateDocumentCollectionAsync** crea una nueva colección con un rendimiento reservado, lo que tiene implicaciones en los precios. Para más detalles, consulte la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).

Copie y pegue el código siguiente en el método **GetStartedDemo** debajo de la creación de la base de datos. Este código crea una colección de documentos denominada *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

## <a id="CreateDoc"></a>Creación de documentos JSON

Cree un documento usando el método `CreateDocumentAsync` de la clase **DocumentClient**. Los documentos son contenido JSON definido por el usuario (arbitrario). Ahora puede insertar uno o varios documentos. 

En primer lugar, cree una clase denominada **Family** que represente los objetos almacenados en Azure Cosmos DB. También creará las subclases **Parent**, **Child**, **Pet** y **Address** que se usan dentro de **Family**. Los documentos deben tener una propiedad **Id** serializada como **id** en JSON. Para crear estas clases, agregue las siguientes subclases internas después del método **GetStartedDemo**. Copie y pegue las clases **Family**, **Parent**, **Child**, **Pet** y **Address** debajo del método **WriteToConsoleAndPromptToContinue**.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
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

Copie y pegue el método **CreateFamilyDocumentIfNotExists** debajo del método **CreateDocumentCollectionIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

E inserte dos documentos, uno para la familia Andersen y otro para la familia Wakefield. Copie y pegue el código siguiente `// ADD THIS PART TO YOUR CODE` en el método **GetStartedDemo** debajo de la creación de la colección de documentos.

```csharp
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

![Relación jerárquica entre la cuenta, la base de datos en línea, la colección](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Consulta de los recursos de Azure Cosmos DB

Azure Cosmos DB admite consultas enriquecidas en los documentos JSON que se almacenan en las colecciones. En el siguiente ejemplo, se muestran diversas consultas (usando la sintaxis SQL de Azure Cosmos DB y LINQ) que puede ejecutar en los documentos que hemos insertado en el paso anterior.

Copie y pegue el método **ExecuteSimpleQuery** debajo del método **CreateFamilyDocumentIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Copie y pegue el código siguiente en el método **GetStartedDemo** debajo de la creación del segundo documento.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

En el siguiente diagrama se muestra cómo se llama a la sintaxis de la consulta SQL de Azure Cosmos DB con la colección creada. La misma lógica se aplica a la consulta LINQ.

![Diagrama que ilustra el ámbito y el significado de la consulta usada por el tutorial de NoSQL para crear una aplicación de consola de C#.](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

La palabra clave `FROM` es opcional en la consulta porque las consultas de Azure Cosmos DB ya tienen como ámbito una única colección. Por lo tanto, «FROM Families f" se puede intercambiar por  "FROM root r", o cualquier otra variable de nombre que elija. Azure Cosmos DB deduce que la familia, la raíz o el nombre de variable que eligió hace referencia a la colección actual de forma predeterminada.

## <a id="ReplaceDocument"></a>Reemplazo de un documento JSON

Azure Cosmos DB admite la sustitución de documentos JSON.  

Copie y pegue el método **ReplaceFamilyDocument** debajo del método **ExecuteSimpleQuery**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
}
```

Copie y pegue el código siguiente en el método **GetStartedDemo** debajo de la ejecución de la consulta. Después de reemplazar el documento, este volverá a ejecutar la misma consulta para ver el documento modificado.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

## <a id="DeleteDocument"></a>Eliminación de un documento JSON

Azure Cosmos DB admite la eliminación de documentos JSON.  

Copie y pegue el método **DeleteFamilyDocument debajo** del método **ReplaceFamilyDocument**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
    Console.WriteLine("Deleted Family {0}", documentName);
}
```

Copie y pegue el código siguiente en el método **GetStartedDemo** debajo de la ejecución de la segunda consulta.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

## <a id="DeleteDatabase"></a>Eliminación de la base de datos

La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (colecciones, documentos, etc.). Copie y pegue el código siguiente en el método **GetStartedDemo** debajo del documento para eliminar toda la base de datos y todos los recursos secundarios.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Seleccione el botón **DocumentDBGettingStarted** para ejecutar la aplicación.

## <a id="Run"></a>Ejecución de la aplicación de consola de C#

Seleccione el botón **DocumentDBGettingStarted** en Visual Studio para crear la aplicación en modo de depuración. Ahora debería ver la salida de la aplicación de introducción en la ventana de la consola. La salida mostrará los resultados de las consultas que hemos agregado y debe coincidir con el texto de ejemplo siguiente.

```bash
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

Ahora ha completado este tutorial y tiene una aplicación de consola de C# en funcionamiento.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede eliminar el grupo de recursos, la cuenta de Azure Cosmos DB y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione **Eliminar** y luego confirme el nombre del grupo de recursos que va a eliminar.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a crear una aplicación .NET Core para administrar los datos almacenados de la cuenta de la API de SQL de Azure Cosmos DB. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Compilación de una aplicación de consola de Java para la API de SQL](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
