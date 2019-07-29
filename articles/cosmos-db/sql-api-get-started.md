---
title: Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB
description: Aprenda a crear recursos de SQL API de Azure Cosmos DB mediante una aplicación de consola en C#.
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: kirankk
ms.openlocfilehash: 37b6a9947d3cabe1f566f842e321229efe9d03b6
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598521"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Compilación de una aplicación de consola .NET para administrar datos de la cuenta de API de SQL de Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Le damos la bienvenida al tutorial de introducción a la API de SQL de Azure Cosmos DB. Después de seguir este tutorial, tendrá una aplicación de consola que crea recursos de Azure Cosmos DB y los consulta. En este tutorial se usa la [versión 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) del SDK de .NET de Azure Cosmos DB, cuyo destino pueden ser [.NET Framework](https://dotnet.microsoft.com/download) o [.NET Core](https://dotnet.microsoft.com/download).

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
1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#**  / **Aplicación de consola (.NET Framework)** , asigne un nombre al proyecto y, luego, haga clic en **Aceptar**.
    ![Captura de pantalla de la ventana Nuevo proyecto](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)

    > [!NOTE]
    > En caso de que el destino sea .NET Core, en el cuadro de diálogo **Nuevo proyecto**, seleccione **Visual C#**  / **Aplicación de consola (.NET Core)** , asigne un nombre al proyecto y haga clic en **Aceptar**

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la nueva aplicación de la consola, que se encuentra en la solución de Visual Studio y, a continuación, haga clic en **Administrar paquetes NuGet...**

    ![Captura de pantalla del menú contextual del proyecto](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. En la pestaña **NuGet**, haga clic en **Examinar** y escriba **Microsoft.Azure.Cosmos** en el cuadro de búsqueda.
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
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
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
        private Database database;

        // The container we will create.
        private Container container;

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

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Presione **F5** para ejecutar la aplicación. La salida de la ventana de consola muestra el mensaje `End of demo, press any key to exit.` que confirma que se realizó la conexión a Azure Cosmos DB. A continuación, puede cerrar la ventana de consola.

Felicidades. Se ha conectado correctamente a una cuenta de Azure Cosmos DB. 

## <a name="step-4-create-a-database"></a>Paso 4: Creación de una base de datos
Se puede crear una base de datos mediante la función [**CreateDatabaseIfNotExistsAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) o [**CreateDatabaseAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) de la clase ``CosmosClient``. Una base de datos es el contenedor lógico de elementos con particiones en contenedores.

1. Copie y pegue el método **CreateDatabase** debajo del método **GetStartedDemoAsync**. **CreateDatabase** creará una base de datos con el identificador ``FamilyDatabase``, en caso de que no exista ya, con el identificador especificado en el campo ``databaseId``. 

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

1. Copie y pegue el código siguiente, en el que se creó una instancia de CosmosClient para llamar al método **CreateDatabase** que acaba de agregar.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    En este momento, el código debe parecerse a este, con el punto de conexión y la clave principal rellenados.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using System.Collections.Generic;
    using System.Net;
    using Microsoft.Azure.Cosmos;

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
            private Database database;

            // The container we will create.
            private Container container;

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

            /// <summary>
            /// Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            /// </summary>
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabaseAsync();
            }

            /// <summary>
            /// Create the database if it does not exist
            /// </summary>
            private async Task CreateDatabaseAsync()
            {
                // Create a new database
                this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
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

Se puede crear un contenedor mediante las funciones [**CreateContainerIfNotExistsAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerIfNotExistsAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) o [**CreateContainerAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) en la clase **CosmosContainers**. Un contenedor se compone de elementos (que en el caso de SQL API son documentos JSON) y una lógica de aplicación del servidor en JavaScript asociada, como por ejemplo procedimientos almacenados, funciones definidas por el usuario y desencadenadores.

1. Copie y pegue el método **CreateContainerAsync** debajo del método **CreateDatabaseAsync**. **CreateContainerAsync** creará un contenedor con el identificador ``FamilyContainer``, en caso de que no exista, con el identificador especificado en el campo ``containerId``con las particiones de la propiedad``LastName``.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Copie y pegue el código siguiente donde creó una instancia de CosmosClient para llamar al método **CreateContainer** que acaba de agregar.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainerAsync();
    }
    ```

   Presione **F5** para ejecutar la aplicación.

Felicidades. Ha creado correctamente un contenedor de Azure Cosmos DB.  

## <a id="CreateDoc"></a>Paso 6: Agregar elementos al contenedor
Para crear un elemento se puede usar la función [**CreateItemAsync**](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Container_CreateItemAsync__1___0_System_Nullable_Microsoft_Azure_Cosmos_PartitionKey__Microsoft_Azure_Cosmos_ItemRequestOptions_System_Threading_CancellationToken_) de la clase **CosmosContainer**. Cuando se usa SQL API, los elementos se proyectan como documentos, que son contenido JSON (arbitrario) definido por el usuario. Ya puede insertar un elemento en el contenedor de Azure Cosmos DB.

En primer lugar, vamos a crear una clase denominada **Family** que representará los objetos almacenados en Azure Cosmos DB en este ejemplo. También se crearán las subclases **Parent**, **Child**, **Pet**, **Address** que se usan dentro de **Family**. Tenga en cuenta que el elemento deben tener una propiedad **Id** serializada como **id** en JSON.

1. Seleccione **Ctrl + Mayús + A** para abrir el cuadro de diálogo **Agregar nuevo elemento**. Agregue una nueva clase de **Family.cs** al proyecto.

    ![Captura de pantalla de la adición de una nueva clase de Family.cs al proyecto](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Copie y pegue la clase **Family**, **Parent**, **Child**, **Pet** y **Address** en **Family.cs**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]

1. Vuelva a **Program.cs** y agregue el método **AddItemsToContainerAsync** en el método **CreateContainerAsync**.
Antes de crear un elemento, el código se asegura de que no haya uno con el mismo identificador. Se insertan dos elementos, uno para la familia Andersen y otro para la familia Wakefield.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]

1. Agregue una llamada a ``AddItemsToContainerAsync`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainerAsync();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha creado correctamente dos elementos de Azure Cosmos DB.  

## <a id="Query"></a>Paso 7: Consulta de los recursos de Azure Cosmos DB
Azure Cosmos DB admite [consultas](sql-api-sql-query.md) enriquecidas en los documentos JSON que se almacenan en las colecciones. El ejemplo de código siguiente muestra cómo ejecutar una consulta en los elementos que se insertaron en el paso anterior.

1. Copie y pegue el método **QueryItemsAsync** debajo del método **AddItemsToContainerAsync**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. Agregue una llamada a ``QueryItemsAsync`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.QueryItemsAsync();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha realizado correctamente una consulta en un contenedor de Azure Cosmos DB.

## <a id="ReplaceItem"></a>Paso 8: Sustitución de un elemento JSON
Ahora, se va a actualizar un elemento en Azure Cosmos DB.

1. Copie y pegue el método **ReplaceFamilyItemAsync** debajo del método **QueryItemsAsync**. Tenga en cuenta que estamos cambiando la propiedad ``IsRegistered`` de la familia y el ``Grade`` de uno de los hijos.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. Agregue una llamada a ``ReplaceFamilyItemAsync`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItemAsync();
    }
    ```

   Presione **F5** para ejecutar la aplicación.

Felicidades. Ha sustituido correctamente un elemento de Azure Cosmos DB.

## <a id="DeleteDocument"></a>Paso 9: Eliminar elemento
Ahora, se va a eliminar un elemento de Azure Cosmos DB.

1. Copie y pegue el método **DeleteFamilyItemAsync** debajo del método **ReplaceFamilyItemAsync**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. Agregue una llamada a ``DeleteFamilyItemAsync`` en el método ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();
        await this.ReplaceFamilyItemAsync();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItemAsync();
    }
    ```

Presione **F5** para ejecutar la aplicación.

Felicidades. Ha eliminado correctamente un elemento de Azure Cosmos DB.

## <a id="DeleteDatabase"></a>Paso 10: Eliminación de la base de datos
Ahora se eliminará la base de datos. La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (contenedores, elementos y cualquier procedimiento almacenado, funciones definidas por el usuario y desencadenadores). También se eliminará la instancia de **CosmosClient**.

1. Copie y pegue el método **DeleteDatabaseAndCleanupAsync** debajo del método **DeleteFamilyItemAsync**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. Agregue una llamada a ``DeleteDatabaseAndCleanupAsync`` en el método ``GetStartedDemoAsync``.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

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

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
