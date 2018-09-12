---
title: Configuración de una canalización de CI/CD con la tarea de compilación del emulador de Azure Cosmos DB
description: Tutorial acerca de cómo configurar el flujo de trabajo de compilación y publicación en Visual Studio Team Services (VSTS) mediante la tarea de compilación del emulador de Cosmos DB
services: cosmos-db
keywords: Emulador de Azure Cosmos DB
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 8/28/2018
ms.author: dech
ms.openlocfilehash: 37bb43435c34f14145b3642aa12c5cb0f16d780c
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783881"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-visual-studio-team-services"></a>Configuración de una canalización de CI/CD con la tarea de compilación del emulador de Azure Cosmos DB en Visual Studio Team Services

El emulador de Azure Cosmos DB proporciona un entorno local que emula el servicio Azure Cosmos DB con fines de desarrollo. Dicho emulador permite desarrollar y probar aplicaciones localmente sin necesidad de crear una suscripción a Azure ni incurrir en gastos. 

La tarea de compilación del emulador de Azure Cosmos DB para Visual Studio Team Services (VSTS) permite hacer lo mismo en un entorno de integración continua. Con la tarea de compilación, se pueden realizar pruebas en el emulador como parte de los flujos de trabajo de compilación y publicación. La tarea pone en marcha un contenedor de Docker con el emulador ya en ejecución y proporciona un punto de conexión que puede utilizar el resto de la definición de compilación. Puede crear e inicie tantas instancias del emulador como sea necesario, y cada una de ellas se ejecuta en un contenedor independiente. 

En este artículo se muestra cómo configurar una canalización de CI en VSTS para una aplicación de ASP.NET que usa la tarea de compilación del emulador de Cosmos DB para realizar las pruebas. 

## <a name="install-the-emulator-build-task"></a>Instalación de la tarea de compilación del emulador

Para usar la tarea de compilación, en primer lugar es preciso instalarla en la organización de VSTS. Busque la extensión **emulador de Azure Cosmos DB** en el [Marketplace](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) y haga clic en **Consígalo gratis.**

![Busque e instale la tarea de compilación del emulador de Azure Cosmos DB en el Marketplace de VSTS](./media/tutorial-setup-ci-cd/addExtension_1.png)

A continuación, elija la organización en la que desea instalar la extensión. 

> [!NOTE]
> Para instalar una extensión en una organización de VSTS, es preciso ser propietario de una cuenta o administrador de una colección de proyectos. Si no tiene permisos, pero es miembro de una cuenta, puede solicitar extensiones en su lugar. [Más información.](https://docs.microsoft.com/vsts/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions) 

![Elija la organización de VSTS en la que instalar una extensión](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Creación de una definición de compilación

Ahora que la extensión está instalada, es preciso agregarla a una [definición de compilación.](https://docs.microsoft.com/vsts/pipelines/get-started-designer?view=vsts&tabs=new-nav) Puede modificar una definición de compilación existente o crear una nueva. Si ya dispone de una definición de compilación, puede pasar directamente a [Adición de una tarea de compilación del emulador a una definición de compilación](#addEmulatorBuildTaskToBuildDefinition).

Para crear una definición de compilación, vaya a la pestaña **Compilación y lanzamiento** de VSTS. Seleccione **+Nuevo.**

![Crear una definición de compilación](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png) Para habilitar las compilaciones, seleccione el proyecto de equipo, el repositorio y la rama deseados. 

![Seleccionar el proyecto de equipo, el repositorio y la rama de la definición de compilación ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

Por último, seleccione la plantilla que desee para la definición de compilación. En este tutorial se seleccionará la plantilla **ASP.NET**. 

![Seleccionar la plantilla de definición de compilación deseada ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_3.png)

Ya hay una definición de compilación que se puede configurar para usar la tarea de compilación del emulador de Azure Cosmos DB que es similar a la siguiente. 

![Plantilla de definición de compilación ASP.NET](./media/tutorial-setup-ci-cd/CreateNewBuildDef_4.png)

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Adición de la tarea a una definición de compilación

Para agregar la tarea de compilación del emulador, busque **cosmos** en el cuadro de búsqueda y seleccione **Agregar**. La tarea de compilación iniciará un contenedor, en el que ya estará en ejecución una instancia del emulador de Cosmos DB, por lo que la tarea debe colocarse antes de cualquier otra tarea que el emulador espere que se esté ejecutando.

![Adición de la tarea de compilación del emulador a una definición de compilación](./media/tutorial-setup-ci-cd/addExtension_3.png) En este tutorial, la tarea se agregará al principio de la fase 1 para tener la certeza de que el emulador está disponible antes de ejecutar las pruebas.
La definición de compilación completada ahora es como esta. 

![Plantilla de definición de compilación ASP.NET](./media/tutorial-setup-ci-cd/CreateNewBuildDef_5.png)

## <a name="configure-tests-to-use-the-emulator"></a>Configuración de pruebas para usar el emulador
Ahora, se van a configurar las pruebas para usar el emulador. La tarea de compilación del emulador exporta una variable de entorno, 'CosmosDbEmulator.Endpoint', a la que las tareas posteriores de la canalización de compilación pueden realizar solicitudes. 

En este tutorial, se va a usar la tarea [Visual Studio Test](https://github.com/Microsoft/vsts-tasks/blob/master/Tasks/VsTestV2/README.md) para ejecutar pruebas unitarias configuradas mediante un archivo **.runsettings**. Para más información acerca de la configuración de pruebas unitarias, visite la [documentación](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017).

A continuación encontrará un ejemplo de un archivo **.runsettings** que define los parámetros que se van a pasan a las pruebas unitarias de una aplicación. Tenga en cuenta que la variable `authKey` que se usa es la [clave conocida](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) del emulador. `authKey` es la clave que espera la tarea de compilación del emulador y debe estar definida en el archivo **.runsettings**.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```
A los parámetros `TestRunParameters` se les hace referencia mediante una propiedad `TestContext` en el proyecto de prueba de la aplicación. Este es un ejemplo de una prueba que se ejecuta en Cosmos DB. 

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Vaya a las opciones de ejecución de la tarea Visual Studio Test. En la opción **Archivo de configuración**, especifique que las pruebas se configuran mediante el archivo **.runsettings**. En la opción **Reemplazar parámetros de serie de pruebas**, agregue ` -endpoint $(CosmosDbEmulator.Endpoint)`. Al hacerlo, configurará la tarea para hacer referencia al punto de conexión de la tarea de compilación del emulador, en lugar de al definido en el archivo **.runsettings**.  

![Reemplazar la variable del punto de conexión por el punto de conexión de la tarea de compilación del emulador](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Ejecución de la compilación
Ahora, guarde la compilación y póngala en la cola. 

![Guardar y ejecutar la compilación](./media/tutorial-setup-ci-cd/runBuild_1.png)

Una vez que se ha iniciado la compilación, observe que la tarea del emulador de Cosmos DB ha comenzado a extraer la imagen de Docker con el emulador instalado. 

![Guardar y ejecutar la compilación](./media/tutorial-setup-ci-cd/runBuild_4.png)

Cuando se complete la compilación, observe que pasa las pruebas; todas ellas se ejecutan en el emulador de Cosmos DB desde la tarea de compilación.

![Guardar y ejecutar la compilación](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca del uso del emulador para el desarrollo y las pruebas locales, consulte [Uso del Emulador de Azure Cosmos DB para desarrollo y pruebas de forma local](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Para exportar los certificados SSL del emulador, consulte [Exportación de los certificados del emulador de Azure Cosmos DB para su uso con Java, Python y Node.js](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates)
