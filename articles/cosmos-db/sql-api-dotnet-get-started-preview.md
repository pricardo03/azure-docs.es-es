---
title: Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB (versión preliminar del SDK versión 3)
description: Tutorial en el que se crea una base de datos en línea y la aplicación de consola de C# con la API de SQL.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: 27686adb7ae20e50ce822f83002135844f9f93bb
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037028"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account-sdk-version-3-preview"></a>Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB (versión preliminar del SDK versión 3)

> [!div class="op_single_selector"]
> * [.NET (versión preliminar)](sql-api-dotnet-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (versión preliminar)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Le damos la bienvenida al tutorial de introducción a la API de SQL de Azure Cosmos DB. Después de seguir este tutorial, tendrá una aplicación de consola que crea recursos de Azure Cosmos DB y los consulta. En este tutorial se usa la [versión 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) de .NET SDK de Azure Cosmos DB, que tiene como destino [.NET Standard 2.0.](https://docs.microsoft.com/dotnet/standard/net-standard)

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> * Creación de una cuenta de Azure Cosmos DB y conexión a ella
> * Configuración del proyecto en Visual Studio
> * Creación de una base de datos y un contenedor
> * Adición de elementos al contenedor
> * Consulta en el contenedor
> * Operaciones CRUD en el elemento
> * Eliminación de la base de datos

¿No tiene tiempo? ¡No se preocupe! La solución completa está disponible en [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started). Para obtener instrucciones rápidas, diríjase a la [sección sobre cómo obtener la solución completa del tutorial](#GetSolution).

Comencemos.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos DB
Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Configuración de la solución de Visual Studio](#SetupVS). Si usa el Emulador de Azure Cosmos DB, siga los pasos que se indican en [Emulador de Azure Cosmos DB](local-emulator.md) para configurarlo y vaya directamente a [Configuración del proyecto de Visual Studio](#SetupVS).

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>Paso 2: Configuración del proyecto de Visual Studio
1. Abra **Visual Studio 2017** en el equipo.
1. En el menú **Archivo**, seleccione **Nuevo** y elija **Proyecto**.
1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#** / **Aplicación de consola (.NET Framework)**, asigne un nombre al proyecto y, luego, haga clic en **Aceptar**.
    ![Captura de pantalla de la ventana Nuevo proyecto](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)
1. En el **Explorador de soluciones**, haga clic con el botón derecho en la nueva aplicación de la consola, que se encuentra en la solución de Visual Studio y, a continuación, haga clic en **Administrar paquetes NuGet...**
    
    ![Captura de pantalla del menú contextual del proyecto](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. En la pestaña **NuGet**, haga clic en **Examinar** y escriba **Microsoft.Azure.Cosmos** en el cuadro de búsqueda. No olvide marcar *Include prelease* (Incluir versión preliminar) para buscar la versión preliminar.
1. En los resultados, busque **Microsoft.Azure.Cosmos** y haga clic en **Instalar**.
   El identificador del paquete de la biblioteca de cliente de la API de SQL de Azure Cosmos DB es [Microsoft Azure Cosmos DB Client Library](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/).
   ![Captura de pantalla del menú NuGet para encontrar el SDK de cliente de Azure Cosmos DB](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Si recibe un mensaje sobre cómo revisar los cambios en la solución, haga clic en **Aceptar**. Si recibe un mensaje acerca de la aceptación de licencia, haga clic en **Acepto**.

Estupendo. Ahora que hemos terminado la configuración, comencemos a escribir algo de código. Puede buscar un proyecto con código completo de este tutorial en [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started).

## <a id="Connect"></a>Paso 3: Conexión a una cuenta de Azure Cosmos DB
1. En primer lugar, reemplace las referencias que aparecen al principio de su aplicación de C#, en el archivo **Program.cs**, por estas otras:
    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;
    ```
1. Ahora, agregue estas constantes y variables a la clase pública ``Program``.
    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE

        // The Azure Cosmos DB endpoint for running this sample.
        private static readonly string EndpointUri = "<your endpoint here>";
        // The primary key for the Azure Cosmos account.
        private static readonly string PrimaryKey = "<your primary key>";

        // The Cosmos client instance
        private CosmosClient cosmosClient;

        // The database we will create
        private CosmosDatabase database;

        // The container we will create.
        private CosmosContainer container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```
    Observe que, si está familiarizado con la versión anterior del SDK de .NET, puede estar habituado a ver los términos "colección" y "documento". Dado que Azure Cosmos DB admite varios modelos de API, todas las versiones del SDK de .NET posteriores a la 3.0 usan los términos genéricos "contenedor" y "elemento". Un contenedor puede ser una colección, un grafo o una tabla. Un elemento puede ser un documento, un vértice o borde, o una fila, y es el contenido que hay en un contenedor. [Más información acerca de las bases de datos, contenedores y elementos.](databases-containers-items.md)

1. Recupere la dirección URL y la clave principal del punto de conexión de [Azure Portal](https://portal.azure.com).

    En Azure Portal, vaya a la cuenta de Azure Cosmos DB y haga clic en **Claves**.

    Copie el URI del portal y péguelo en `<your endpoint URL>` en el archivo ```Program.cs```. Copie la CLAVE PRINCIPAL del portal y péguela en `<your primary key>`.

   ![Captura de pantalla para obtener las claves de Azure Cosmos DB de Azure Portal](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. A continuación, se creará una instancia de ```CosmosClient``` y se configurará algo de scaffolding para nuestro programa.

    Debajo del método **Main**, agregue una nueva tarea asincrónica denominada **GetStartedDemoAsync**, que creará una instancia del nuevo ```CosmosClient```. Se usará **GetStartedDemoAsync** como punto de entrada que llama a los métodos que operan en recursos de Azure Cosmos DB.

    ```csharp
    public static async Task Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    /*
        Entry point to call methods that operate on Azure Cosmos DB resources in this sample
    */   
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
    }
    ```

1. Agregue el código siguiente para ejecutar la tarea asincrónica **GetStartedDemoAsync** desde su método **Main**. El método **Main** detectará las excepciones y las escribirá en la consola.
    ```csharp
    public static async Task Main(string[] args)
    {
        // ADD THIS PART TO YOUR CODE
        try
        {
            Console.WriteLine("Beginning operations...\n");
            Program p = new Program();
            await p.GetStartedDemoAsync();
        }
        catch (CosmosException de)
        {
            Exception baseException = de.GetBaseException();
            Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: {0}\n", e);
        }
        finally
        {
            Console.WriteLine("End of demo, press any key to exit.");
            Console.ReadKey();
        }
    }
    ```

1. Presione **F5** para ejecutar la aplicación. La salida de la ventana de consola muestra el mensaje `End of demo, press any key to exit.` que confirma que se realizó la conexión a Azure Cosmos DB. A continuación, puede cerrar la ventana de consola. 

Felicidades. Se ha conectado correctamente a una cuenta de Azure Cosmos DB. 

## <a name="step-4-create-a-database"></a>Paso 4: Creación de una base de datos
Se puede crear una base de datos mediante la función [**CreateDatabaseIfNotExistsAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabases) o [**CreateDatabaseAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabases) de la clase ``CosmosDatabases``. Una base de datos es el contenedor lógico de elementos con particiones en contenedores.
    
1. Copie y pegue el método **CreateDatabase** debajo de su método **GetStartedDemoAsync**. **CreateDatabase** creará una nueva base de datos con el identificador ``FamilyDatabase`` si no existe ya, con el identificador especificado en el campo ``databaseId``. 

    ```csharp
    /*
        Create the database if it does not exist
    */    
    private async Task CreateDatabase()
    {
        // Create a new database
        this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
        Console.WriteLine("Created Database: {0}\n", this.database.Id);
    }
    ```

1. Copie y pegue el código siguiente donde creó una instancia de CosmosClient para llamar al método **CreateDatabase** que acaba de agregar.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabase(); 
    }
    ```

    En este momento, el código debe parecerse a este, con el punto de conexión y la clave principal rellenados.
    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;

    namespace CosmosGettingStarted
    {
        class Program
        {
            // The Azure Cosmos DB endpoint for running this sample.
            private static readonly string EndpointUri = "<your endpoint here>";
            // The primary key for the Azure Cosmos account.
            private static readonly string PrimaryKey = "<your primary key>";

            // The Cosmos client instance
            private CosmosClient cosmosClient;
        
            // The database we will create
            private CosmosDatabase database;

            // The container we will create.
            private CosmosContainer container;

            // The name of the database and container we will create
            private string databaseId = "FamilyDatabase";
            private string containerId = "FamilyContainer";

            public static async Task Main(string[] args)
            {
                try
                {
                    Console.WriteLine("Beginning operations...");
                    Program p = new Program();
                    await p.GetStartedDemoAsync();
                }
                catch (CosmosException de)
                {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: {0}\n", e);
                }
                finally
                {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
                }
            }

            /*
                Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            */
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabase();
            }

            /*
                Create the database if it does not exist
            */    
            private async Task CreateDatabase()
            {
                // Create a new database
                this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha creado correctamente una base de datos de Azure Cosmos DB.  

## <a id="CreateColl"></a>Paso 5: Crear un contenedor
> [!WARNING]
> Al llamar al método **CreateContainerIfNotExistsAsync** se creará un contenedor nuevo, lo que afectará a los precios. Para obtener más información, visite nuestra [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Para crear un contenedor se puede usar o bien la función [**CreateContainerIfNotExistsAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmoscontainers) o [**CreateContainerAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmoscontainers) en la clase **CosmosContainers**. Un contenedor se compone de elementos (que, en el caso de la API de SQL, son documentos JSON) y la lógica de aplicaciones del lado servidor de JavaScript asociada, como procedimientos almacenados, funciones definidas por el usuario y desencadenadores.

1. Copie y pegue el método **CreateContainer** debajo del método **CreateDatabase**. **CreateContainer** crea un contenedor con el identificador ``FamilyContainer`` si no existe, con el identificador especificado en el campo ``containerId``. 

    ```csharp
    /*
        Create the container if it does not exist. 
        Specifiy "/LastName" as the partition key since we're storing family information, to ensure good distribution of requests and storage.
    */
    private async Task CreateContainer()
    {
        // Create a new container
        this.container = await this.database.Containers.CreateContainerIfNotExistsAsync(containerId, "/LastName");
        Console.WriteLine("Created Container: {0}\n", this.container.Id);
    }
    ```

1. Copie y pegue el código siguiente donde creó una instancia de CosmosClient para llamar al método **CreateContainer** que acaba de agregar.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainer();
    }

Select **F5** to run your application.

Congratulations! You have successfully created an Azure Cosmos DB container.  

## <a id="CreateDoc"></a>Step 6: Add items to the container
An item can be created by using the [**CreateItemAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmositems) function of the **CosmosItems** class. When using the SQL API, items are projected as documents, which are user-defined (arbitrary) JSON content. You can now insert an item into your Azure Cosmos DB container.

First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample. We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**. Note that documents must have an **Id** property serialized as **id** in JSON. 
1. Select **Ctrl+Shift+A** to open the **Add New Item** dialog. Add a new class **Family.cs** to your project. 

    ![Screen shot of adding a new Family.cs class into the project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** class into **Family.cs**. 
    ```csharp
    using Newtonsoft.Json;

    namespace CosmosGettingStarted
    {
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
    }
    ```
1. Vuelva a **Program.cs** y agregue el método **AddItemsToContainer** en el método **CreateContainer**. Antes de crear un elemento, el código se asegura de que no haya ya uno con el mismo identificador. Se insertan dos elementos, uno para la familia Andersen y otro para la familia Wakefield.

    ```csharp
    /*
        Add Family items to the container
    */
    private async Task AddItemsToContainer()
    {
        // Create a family object for the Andersen family
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

        // Read the item to see if it exists. Note ReadItemAsync will not throw an exception if an item does not exist. Instead, we check the StatusCode property off the response object. 
        CosmosItemResponse<Family> andersenFamilyResponse = await this.container.Items.ReadItemAsync<Family>(andersenFamily.LastName, andersenFamily.Id);

        if (andersenFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Andersen family. Note we provide the value of the partition key for this item, which is "Andersen"
            andersenFamilyResponse = await this.container.Items.CreateItemAsync<Family>(andersenFamily.LastName, andersenFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", andersenFamilyResponse.Resource.Id, andersenFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", andersenFamilyResponse.Resource.Id);
        }

        // Create a family object for the Wakefield family
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

        // Read the item to see if it exists
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily.Id);

        if (wakefieldFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Wakefield family. Note we provide the value of the partition key for this item, which is "Wakefield"
            wakefieldFamilyResponse = await this.container.Items.CreateItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", wakefieldFamilyResponse.Resource.Id, wakefieldFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", wakefieldFamilyResponse.Resource.Id);
        }
    }
    ```
1. Agregue una llamada a ``AddItemsToContainer`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainer();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha creado correctamente dos elementos de Azure Cosmos DB.  


## <a id="Query"></a>Paso 7: Consulta de los recursos de Azure Cosmos DB
Azure Cosmos DB admite [consultas](sql-api-sql-query.md) enriquecidas en los documentos JSON que se almacenan en las colecciones. El ejemplo de código siguiente muestra cómo ejecutar una consulta en los elementos que se insertaron en el paso anterior.

1. Copie y pegue el método **RunQuery** debajo del método **AddItemsToContainer**.

    ```csharp
    /*
        Run a query (using Azure Cosmos DB SQL syntax) against the container
    */
    private async Task RunQuery()
    {
        var sqlQueryText = "SELECT * FROM c WHERE c.LastName = 'Andersen'";
        var partitionKeyValue = "Andersen";

        Console.WriteLine("Running query: {0}\n", sqlQueryText);

        CosmosSqlQueryDefinition queryDefinition = new CosmosSqlQueryDefinition(sqlQueryText);
        CosmosResultSetIterator<Family> queryResultSetIterator = this.container.Items.CreateItemQuery<Family>(queryDefinition, partitionKeyValue);

        List<Family> families = new List<Family>();

        while (queryResultSetIterator.HasMoreResults)
        {
            CosmosQueryResponse<Family> currentResultSet = await queryResultSetIterator.FetchNextSetAsync();
            foreach (Family family in currentResultSet)
            {
                families.Add(family);
                Console.WriteLine("\tRead {0}\n", family);
            }
        }
    }
    ```
1. Agregue una llamada a ``RunQuery`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();

        //ADD THIS PART TO YOUR CODE
        await this.RunQuery();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha realizado correctamente una consulta en un contenedor de Azure Cosmos DB.

## <a id="ReplaceItem"></a>Paso 8: Sustitución de un elemento JSON
Ahora, se va a actualizar un elemento en Azure Cosmos DB.

1. Copie y pegue el método **ReplaceFamilyItem** debajo del método **RunQuery**. Tenga en cuenta que estamos cambiando la propiedad ``IsRegistered`` de la familia y el ``Grade`` de uno de los hijos.
    ```csharp
    /*
    Update an item in the container
    */
    private async Task ReplaceFamilyItem()
    {
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>("Wakefield", "Wakefield.7");
        var itemBody = wakefieldFamilyResponse.Resource;
        
        // update registration status from false to true
        itemBody.IsRegistered = true;
        // update grade of child
        itemBody.Children[0].Grade = 6;

        // replace the item with the updated content
        wakefieldFamilyResponse = await this.container.Items.ReplaceItemAsync<Family>(itemBody.LastName, itemBody.Id, itemBody);
        Console.WriteLine("Updated Family [{0},{1}]\n. Body is now: {2}\n", itemBody.LastName, itemBody.Id, wakefieldFamilyResponse.Resource);
    }
    ```
1. Agregue una llamada a ``ReplaceFamilyItem`` en el método ``GetStartedDemo``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItem();
    }

Select **F5** to run your application.

Congratulations! You have successfully replaced an Azure Cosmos DB item.

## <a id="DeleteDocument"></a>Step 9: Delete item
Now, we will delete an item in Azure Cosmos DB.

1. Copy and paste the **DeleteFamilyItem** method below your **ReplaceFamilyItem** method.
    ```csharp
    /*
    Delete an item in the container
    */
    private async Task DeleteFamilyItem()
    {
        var partitionKeyValue = "Wakefield";
        var familyId = "Wakefield.7";

        // Delete an item. Note we must provide the partition key value and id of the item to delete
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.DeleteItemAsync<Family>(partitionKeyValue, familyId);
        Console.WriteLine("Deleted Family [{0},{1}]\n", partitionKeyValue, familyId);
    }
    ```
1. Agregue una llamada a ``DeleteFamilyItem`` en el método ``GetStartedDemo``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItem();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha eliminado correctamente un elemento de Azure Cosmos DB.

## <a id="DeleteDatabase"></a>Paso 10: Eliminación de la base de datos
Ahora se eliminará la base de datos. La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (contenedores, elementos y cualquier procedimiento almacenado, funciones definidas por el usuario y desencadenadores). También se eliminará la instancia de **CosmosClient**.

1. Copie y pegue el método **DeleteDatabaseAndCleanup** debajo del método **DeleteFamilyItem**.
    ```csharp
    /*
    Delete the database and dispose of the Cosmos Client instance
    */
    private async Task DeleteDatabaseAndCleanup()
    {
        CosmosDatabaseResponse databaseResourceResponse = await this.database.DeleteAsync();
        // Also valid: await this.cosmosClient.Databases["FamilyDatabase"].DeleteAsync();

        Console.WriteLine("Deleted Database: {0}\n", this.databaseId);

        //Dispose of CosmosClient
        this.cosmosClient.Dispose();
    }
    ```
1. Agregue una llamada a ``DeleteDatabaseAndCleanup`` en el método ``GetStartedDemo``.
    
    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();
        await this.DeleteFamilyItem();

        //ADD THIS PART TO YOUR CODE        
        await this.DeleteDatabaseAndCleanup();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha eliminado correctamente una base de datos de Azure Cosmos DB.

## <a id="Run"></a>Paso 11: Ejecutar la aplicación de consola de C#
Presione F5 en Visual Studio para compilar y ejecutar la aplicación en modo de depuración.

Verá la salida de la aplicación entera en una ventana de consola. La salida mostrará los resultados de las consultas que hemos agregado y debe coincidir con el texto de ejemplo siguiente.

```
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

Felicidades. Ha completado este tutorial y tiene una aplicación de consola de C# en funcionamiento.

## <a id="GetSolution"></a> Obtención de la solución completa del tutorial
Si no dispuso de tiempo para completar los pasos de este tutorial, o simplemente desea descargar los ejemplos de código, puede obtenerlos de [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started). 

Para compilar la solución GetStarted, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una [cuenta de Azure Cosmos DB][cosmos-db-create-account].
* La solución [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) está disponible en GitHub.

Para restaurar las referencias a .NET SDK de Azure Cosmos DB en Visual Studio, haga clic con el botón derecho en la solución **GetStarted** en el Explorador de soluciones y, después, haga clic en **Restaurar paquetes NuGet**. A continuación, en el archivo App.config, actualice los valores EndPointUri y PrimaryKey como se describe en el artículo [Conexión a una cuenta de Azure Cosmos DB](#Connect).

Y, eso es todo, compílelo y habrá terminado.


## <a name="next-steps"></a>Pasos siguientes
* ¿Desea un tutorial de ASP.NET MVC más complejo? Consulte [Tutorial de ASP.NET MVC: Desarrollo de aplicaciones web con Azure Cosmos DB](sql-api-dotnet-application-preview.md).
* ¿Desea realizar pruebas de escala y de rendimiento con Azure Cosmos DB? Consulte [Pruebas de escala y rendimiento con Azure Cosmos DB](performance-testing.md).
* Obtenga más información sobre cómo [supervisar las solicitudes, el uso y el almacenamiento de Azure Cosmos DB](monitor-accounts.md).
* Ejecute las consultas en nuestro conjunto de datos de ejemplo en el [área de consultas](https://www.documentdb.com/sql/demo).
* Para más información sobre Azure Cosmos DB, consulte [Bienvenido a Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-dotnet-preview.md#create-account
