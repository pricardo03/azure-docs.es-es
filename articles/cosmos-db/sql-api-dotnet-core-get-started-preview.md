---
title: Compilación de una aplicación de consola .NET Core para administrar datos en la cuenta de SQL API de Azure Cosmos DB (versión preliminar de la versión 3 de SDK)
description: Tutorial en el que crea una base de datos en línea y la aplicación de consola de C# mediante el SDK de .NET Core de la API de SQL de Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: 92dcd62fa0079b22ba6a2721959a200c2556a4c1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852724"
---
# <a name="build-a-net-core-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account-sdk-version-3-preview"></a>Compilación de una aplicación de consola .NET Core para administrar datos en la cuenta de SQL API de Azure Cosmos DB (versión preliminar de la versión 3 de SDK)

> [!div class="op_single_selector"]
> * [.NET Core (versión preliminar)](sql-api-dotnet-core-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET (versión preliminar)](sql-api-dotnet-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Le damos la bienvenida al tutorial de introducción de SQL API de Azure Cosmos DB con .NET Core. Después de seguir este tutorial, tendrá una aplicación de consola .NET Core que crea recursos de Azure Cosmos DB y los consulta. Este tutorial usa la [versión 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) del SDK de .NET de Azure Cosmos DB, con destinos [.NET Standard 2.0.](https://docs.microsoft.com/dotnet/standard/net-standard)

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> * Creación de una cuenta de Azure Cosmos DB y conexión a ella
> * Configuración del proyecto en Visual Studio
> * Creación de una base de datos y un contenedor
> * Adición de elementos al contenedor
> * Consulta en el contenedor
> * Operaciones CRUD en el elemento
> * Eliminación de la base de datos


¿No tiene tiempo a crear la aplicación? ¡No se preocupe! La solución completa está disponible en [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started). Para obtener instrucciones rápidas, diríjase a la sección [Obtener la solución completa del tutorial de NoSQL](#GetSolution) .

¿Desea compilar una aplicación de Xamarin iOS, Android o Forms mediante la API de SQL y el SDK de .NET Core? Consulte [Creación de aplicaciones móviles con Xamarin y Azure Cosmos DB](mobile-apps-with-xamarin.md).

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Si aún no tiene Visual Studio de 2017 instalado, puede descargar y usar la versión [gratuita de Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Si está desarrollando una aplicación de Plataforma universal de Windows (UWP), debe usar **Visual Studio 2017 con la versión 15.4** o superior. Asegúrese de que habilita la carga de trabajo **Desarrollo de Azure** durante la instalación de Visual Studio.

    * Si está trabajando en MacOS o Linux, puede desarrollar aplicaciones de .NET Core desde la línea de comandos mediante la instalación del [SDK de .NET Core](https://www.microsoft.com/net/core#macos) para la plataforma que elija. 

    * Si está trabajando en Windows, puede desarrollar aplicaciones de .NET Core desde la línea de comandos mediante la instalación del [SDK de .NET Core](https://www.microsoft.com/net/core#windows). 

    * Puede usar su propio editor o descargar [Visual Studio Code](https://code.visualstudio.com/), que es gratuito y funciona en Windows, Linux y MacOS. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos DB

Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Configuración de la solución de Visual Studio](#SetupVS). Si usa el emulador de Azure Cosmos DB, siga los pasos que se indican en el [emulador de Azure Cosmos DB](local-emulator.md) para configurar el emulador y vaya directamente a [Configuración de la solución de Visual Studio](#SetupVS).

[!INCLUDE [cosmos-db-create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos DB
Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Configuración de la solución de Visual Studio](#SetupVS). Si usa el Emulador de Azure Cosmos DB, siga los pasos que se indican en [Emulador de Azure Cosmos DB](local-emulator.md) para configurarlo y vaya directamente a [Configuración del proyecto de Visual Studio](#SetupVS).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Paso 2: Configuración del proyecto de Visual Studio
1. Abra **Visual Studio 2017** en el equipo.
1. En el menú **Archivo**, seleccione **Nuevo** y elija **Proyecto**.
1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#** / **Aplicación de consola (.NET Core)**, asigne un nombre al proyecto y luego haga clic en **Aceptar**.
    ![Captura de pantalla de la ventana Nuevo proyecto](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-new-project.png)
1. En el **Explorador de soluciones**, haga clic con el botón derecho en la nueva aplicación de la consola, que se encuentra en la solución de Visual Studio y, a continuación, haga clic en **Administrar paquetes NuGet...**
    
    ![Captura de pantalla del menú contextual del proyecto](./media/sql-api-dotnetcore-get-started/dotnetcore-tutorial-visual-studio-manage-nuget.png)
1. En la pestaña **NuGet**, haga clic en **Examinar** y escriba **Microsoft.Azure.Cosmos** en el cuadro de búsqueda.
1. En los resultados, busque **Microsoft.Azure.Cosmos** y haga clic en **Instalar**.
   El identificador del paquete de la biblioteca de cliente de la API de SQL de Azure Cosmos DB es [Microsoft Azure Cosmos DB Client Library](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/).
   ![Captura de pantalla del menú NuGet para encontrar el SDK de cliente de Azure Cosmos DB](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Si recibe un mensaje sobre cómo revisar los cambios en la solución, haga clic en **Aceptar**. Si recibe un mensaje acerca de la aceptación de licencia, haga clic en **Acepto**.

Estupendo. Ahora que hemos terminado la configuración, comencemos a escribir algo de código. Puede buscar un proyecto con código completo de este tutorial en [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started/).

## <a id="Connect"></a>Paso 3: Conexión a una cuenta de Azure Cosmos DB
1. En primer lugar, reemplace las referencias que aparecen al principio de su aplicación de C#, en el archivo **Program.cs**, por estas otras:
    ```csharp
    using System;
    using System.Threading.Tasks;
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

1. A continuación, se creará una nueva instancia de ```CosmosClient``` y se configurará algo de scaffolding para el programa.

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
Se puede crear una base de datos mediante la función [**CreateDatabaseIfNotExistsAsync**](https://aka.ms/CosmosDotnetAPIDocs) o [**CreateDatabaseAsync**](https://aka.ms/CosmosDotnetAPIDocs) de la clase ``Databases``. Una base de datos es el contenedor lógico de elementos con particiones en contenedores.
    
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

    En este momento, el código debe parecerse a este, con el punto de conexión y la clave principal rellenados. Tenga en cuenta que el espacio de nombres variarán según el nombre del proyecto.
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;

    namespace CosmosGettingStartedDotnetCoreTutorial
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

## <a id="CreateColl"></a>Paso 5: Creación de un contenedor
> [!WARNING]
> Al llamar al método **CreateContainerIfNotExistsAsync** se creará un contenedor nuevo, lo que afectará a los precios. Para obtener más información, visite nuestra [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Para crear un contenedor se puede usar o bien la función [**CreateContainerIfNotExistsAsync**](https://aka.ms/CosmosDotnetAPIDocs) o [**CreateContainerAsync**](https://aka.ms/CosmosDotnetAPIDocs) en la clase **Containers**. Un contenedor se compone de elementos (que en el caso de SQL API son documentos JSON) y una lógica de aplicación de lado servidor JavaScript asociado, como por ejemplo procedimientos almacenados, funciones definidas por el usuario y desencadenadores.

1. Copie y pegue el método **CreateContainer** debajo de su método **CreateDatabase**. **CreateContainer** creará un nuevo contenedor con el identificador ``FamilyContainer`` si no existe, con el identificador especificado en el campo ``containerId``. 

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

Select  **F5** to run your application.

Congratulations! You have successfully created an Azure Cosmos DB container.  

## <a id="CreateDoc"></a>Step 6: Add items to the container
An item can be created by using the [**CreateItemAsync**](https://aka.ms/CosmosDotnetAPIDocs) function of the **Items** class. When using the SQL API, items are projected as documents, which are user-defined (arbitrary) JSON content. You can now insert an item into your Azure Cosmos DB container.

First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample. We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**. Note that documents must have an **Id** property serialized as **id** in JSON. 
1. Select  **Ctrl+Shift+A** to open the **Add New Item** dialog. Add a new class **Family.cs** to your project. 

    ![Screen shot of adding a new Family.cs class into the project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** class into **Family.cs**. Note your namespace will differ based on the name of your project.
    ```csharp
    using Newtonsoft.Json;

    namespace CosmosGettingStartedDotnetCoreTutorial
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

1. Copie y pegue el método **RunQuery** a continuación del método **AddItemsToContainer**.

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

1. Copie y pegue el método**ReplaceFamilyItem** a continuación del método **RunQuery**. Tenga en cuenta que estamos cambiando la propiedad ``IsRegistered`` de la familia y el ``Grade`` de uno de los hijos.
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

Select  **F5** to run your application.

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

## <a id="DeleteDatabase"></a>Paso 10: Eliminar la base de datos
Ahora se eliminará la base de datos. La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (contenedores, elementos y cualquier procedimiento almacenado, funciones definidas por el usuario y desencadenadores). También se eliminará la instancia de **CosmosClient**.

1. Copie y pegue el método **DeleteDatabaseAndCleanup** a continuación del método **DeleteFamilyItem**.
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
Presione **F5** en Visual Studio para compilar la aplicación en modo de depuración.

Ahora debería ver la salida de la aplicación de introducción completa en la ventana de la consola. La salida mostrará los resultados de las consultas que hemos agregado y debe coincidir con el texto de ejemplo siguiente.

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

Para compilar la solución GetStarted que contiene todos los ejemplos de este artículo, necesitará lo siguiente:

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una [cuenta de Azure Cosmos DB][cosmos-db-create-account].
* La solución [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-core-getting-started) está disponible en GitHub.

Para restaurar las referencias a SQL API para el SDK de .NET Core de Azure Cosmos DB en Visual Studio, haga clic con el botón derecho en la solución **GetStarted** en el Explorador de soluciones y, después, seleccione **Restaurar paquetes de NuGet**. A continuación, en el archivo **Program.cs**, actualice los valores ``EndpointUri`` y ``PrimaryKey`` como se describe en [Conexión a una cuenta de Azure Cosmos DB](#Connect).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo crear una aplicación .NET Core para administrar los datos de SQL API de Azure Cosmos DB. Ahora puede importar datos adicionales en la cuenta de Cosmos DB. 

> [!div class="nextstepaction"]
> [Importación de datos a Azure Cosmos DB](import-data.md)

[cosmos-db-create-account]: create-sql-api-dotnet-preview.md#create-account

