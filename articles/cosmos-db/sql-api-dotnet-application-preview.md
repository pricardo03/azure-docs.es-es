---
title: Tutorial para desarrollar una aplicación web ASP.NET MVC con Azure Cosmos DB mediante el SDK en versión preliminar de .NET
description: En este tutorial se describe cómo crear una aplicación web ASP.NET MVC mediante Azure Cosmos DB. Podrá almacenar datos JSON y acceder a ellos desde una aplicación de tareas pendientes hospedada en Azure.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/03/2018
ms.author: dech
ms.openlocfilehash: bf1da7e8a1041b15076ebda6eeac9b0a75c567c0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57857171"
---
# <a name="tutorial-develop-an-aspnet-mvc-web-application-with-azure-cosmos-db-by-using-net-preview-sdk"></a>Tutorial: Desarrollo de una aplicación web ASP.NET MVC con Azure Cosmos DB mediante el SDK en versión preliminar de .NET 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [.NET (versión preliminar)](sql-api-dotnet-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


En este tutorial aprenderá a usar Azure Cosmos DB para almacenar datos, y acceder a estos, desde una aplicación de ASP.NET MVC hospedada en Azure. En este tutorial se usará .NET SDK V3 que se encuentra actualmente en versión preliminar. En la siguiente imagen se muestra la página web que se compilará mediante el ejemplo de este artículo:
 
![Captura de pantalla de la aplicación web MVC de lista de tareas pendientes creada por este tutorial: tutorial ASP NET MVC paso a paso](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-image01.png)

Si no tiene tiempo para completar el tutorial, puede descargar el proyecto de ejemplo completo de [GitHub][GitHub]. 

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> * Creación de una cuenta de Azure Cosmos
> * Creación de una aplicación ASP.NET MVC
> * Conexión de la aplicación a Azure Cosmos DB 
> * Realizar operaciones CRUD en los datos

> [!TIP]
> En este tutorial se supone que tiene experiencia previa con ASP.NET MVC y Azure Websites. Si no está familiarizado con ASP.NET o las [herramientas que son requisito previo](#prerequisites), se recomienda que descargue el proyecto de ejemplo completo de [GitHub][GitHub], que agregue los paquetes de NuGet necesarios y que lo ejecute. Una vez compilado el proyecto, puede revisar este artículo para obtener información sobre el código en el contexto del proyecto.

## <a name="prerequisites"></a>Requisitos previos 

Antes de seguir las instrucciones del presente artículo, asegúrese de tener los siguientes recursos:

* **Una cuenta de Azure activa:** Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

* Microsoft Azure SDK para .NET de Visual Studio 2017, disponible a través del instalador de Visual Studio.

Se han realizado todas las capturas de pantalla de este artículo mediante Microsoft Visual Studio Community 2017. Si el sistema está configurado con una versión diferente, es probable que las pantallas y opciones no coincidan completamente, pero si cumple los requisitos previos mencionados, esta solución debería funcionar.

## <a name="create-an-azure-cosmos-account"></a>Paso 1: Creación de una cuenta de Azure Cosmos

Para comenzar, crearemos una cuenta de Azure Cosmos. Si ya tiene una cuenta de API de SQL para Azure Cosmos DB, o si usa el emulador de Azure Cosmos DB para los fines de este tutorial, puede ir directamente a la sección [Creación de una aplicación ASP.NET MVC nueva](#create-a-new-mvc-application).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

En la siguiente sección, creará una aplicación de ASP.NET MVC. 

## <a name="create-a-new-mvc-application"></a>Paso 2: Creación de una aplicación ASP.NET MVC nueva

1. En Visual Studio, en el menú **Archivo**, elija **Nuevo** y, luego, **Proyecto**. Aparecerá el cuadro de diálogo **Nuevo proyecto** .

2. En la ventana **Nuevo proyecto**, expanda las plantillas **instaladas**, **Visual C#**  y **Web** y, luego, seleccione **Aplicación web ASP.NET**. 

   ![Creación de un proyecto de aplicación web de ASP.NET](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-new-project-dialog.png)

3. En el cuadro **Nombre** , escriba el nombre del proyecto. Este tutorial utiliza el nombre "todo". Si decide usar un nombre distinto, siempre que en el tutorial se hable del espacio de nombres "todo", adapte los ejemplos de código proporcionados para usar el nombre que haya dado a la aplicación. 

4. Seleccione **Examinar** para ir a la carpeta donde desea crear el proyecto y, luego, elija **.Net Framework 4.6.1** o superior. Seleccione **Aceptar**. 

5. Aparece el cuadro de diálogo **Nueva aplicación web ASP.NET**. En el panel Plantillas, seleccione **MVC**.

6. Seleccione **Aceptar** y deje que Visual Studio realice la tarea de scaffolding en torno a la plantilla vacía de ASP.NET MVC. 

7. Después de que Visual Studio haya terminado de crear la aplicación MVC reutilizable, tiene una aplicación de ASP.NET vacía que puede ejecutar de manera local.

## <a name="add-nuget-packages"></a>Paso 3: Incorporación del paquete NuGet de Azure Cosmos DB al proyecto

Ahora que tenemos la mayoría del código de plataforma de ASP.NET MVC que necesitamos para esta solución, vamos a agregar los paquetes NuGet necesarios para conectarse a Azure Cosmos DB.

1. El SDK de .NET para Azure Cosmos DB se empaquete y distribuye como un paquete de NuGet. Para obtener el paquete NuGet en Visual Studio, use el Administrador de paquetes NuGet de Visual Studio; para ello, haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y, luego, seleccione **Administrar paquetes NuGet**.
   
   ![Captura de pantalla de las opciones del botón derecho para el proyecto de aplicación web en el Explorador de soluciones, con Administrar paquetes NuGet resaltado.](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-manage-nuget.png)
   
2. Aparecerá el cuadro de diálogo **Administrar paquetes de NuGet** . En el cuadro **Examinar** de NuGet, escriba **Microsoft.Azure.Cosmos**. Desde los resultados, instale la versión preliminar 3.0.0.1 de **Microsoft.Azure.Cosmos**. Se descarga e instala el paquete de Azure Cosmos DB y sus dependencias, como Newtonsoft.Json. Haga clic en **OK** (Aceptar) en el ventana **Preview** (Versión preliminar) y **I Accept** (Acepto) en la ventana **License Acceptance** (Aceptación de licencia) para completar la instalación.
   
   También puede usar la Consola del Administrador de paquetes para instalar el paquete NuGet. Para ello, en el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. En el símbolo del sistema, escriba el siguiente comando:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos -Version 3.0.0.1-preview
   ```        

3. Después de instalar el paquete, la solución de Visual Studio debe contener las dos nuevas referencias de biblioteca a Microsoft.Azure.Cosmos.Client y Newtonsoft.Json.
  
## <a name="set-up-the-mvc-application"></a>Paso 4: Configuración de la aplicación ASP.NET MVC

Ahora vamos a agregar los modelos, las vistas y los controladores a esta aplicación MVC:

* [Adición de un modelo](#add-a-model).
* [Adición de un controlador](#add-a-controller).
* [Adición de vistas](#add-views).

### <a name="add-a-model"></a> Incorporación de un modelo

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Modelos**, seleccione **Agregar** y **Clase**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** .

1. Asigne a la nueva clase el nombre **TodoItem.cs** y seleccione **Agregar**. 

1. A continuación, reemplace el código de la clase "Todoitem" por el código siguiente:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Models/TodoItem.cs)]
   
   Los datos almacenados en Azure Cosmos DB se transmiten mediante la conexión y se almacenan como JSON. Para controlar la forma en la que JSON.NET serializa y deserializa los objetos, puede usar el atributo **JsonProperty** como se mostró en la clase **TodoItem** que se creó. No solo puede controlar el formato del nombre de propiedad cuando va en JSON, sino que puede cambiar el nombre de las propiedades .NET como hizo con la propiedad **Description**. 

### <a name="add-a-controller"></a>Adición de un controlador

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Controladores**, seleccione **Agregar** y luego **Controlador**. Aparecerá el cuadro de diálogo **Agregar scaffold** .

1. Seleccione **Controlador MVC 5 - Vacío** y, a continuación, haga clic en **Agregar**.

   ![Captura de pantalla del cuadro de diálogo Agregar scaffold con la opción Controlador de MVC 5: en blanco resaltada](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Asigne al nuevo controlador el nombre **ItemController y sustituya el código de ese archivo por el siguiente:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Controllers/ItemController.cs)]

   El atributo **ValidateAntiForgeryToken** se usa aquí para ayudar a proteger esta aplicación frente a los ataques de falsificación de solicitud entre sitios. Es más que solo agregar este atributo, sus vistas funcionarán también con este token antifalsificación. Para más información sobre el tema y ver ejemplos de cómo implementar correctamente este atributo, consulte [Prevención contra la falsificación de solicitud entre sitios][Preventing Cross-Site Request Forgery]. El código fuente proporcionado en [GitHub][GitHub] tiene la implementación completa.
   
   También se usa el atributo **Bind** en el parámetro de método para ayudar a proteger contra ataques de publicación en exceso. Para más información, consulte [Operaciones básicas CRUD en ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].
    
### <a name="add-views"></a>Adición de vistas

A continuación, se van a crear las tres vistas siguientes: 

* [Agregar una vista de elementos de lista](#AddItemIndexView)
* [Agregar una nueva vista de elementos](#AddNewIndexView).
* [Agregar una vista de edición de elementos](#AddEditIndexView).

#### <a name="AddItemIndexView"></a>Agregar una vista de elementos de lista

1. En el **Explorador de soluciones**, expanda la carpeta **Vistas**, haga clic con el botón derecho en la carpeta vacía **Elemento** que ha creado Visual Studio cuando agregó **ItemController** anteriormente, haga clic en **Agregar** y, a continuación, haga clic en **Vista**.
   
   ![Captura de pantalla del Explorador de soluciones que muestra la carpeta Elemento creada por Visual Studio con los comandos Agregar vista resaltados](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view.png)

2. En el cuadro de diálogo **Agregar vista**, actualice los siguientes valores:
   
   * En el cuadro **Nombre de vista**, escriba ***Índice***.
   * En el cuadro **Plantilla**, seleccione ***Lista***.
   * En el cuadro **Clase de modelo**, seleccione ***Elemento (todo.Models)***.
   * En el cuadro de página de diseño, escriba ***~/Views/Shared/_Layout.cshtml***.
     
   ![Captura de pantalla que muestra el cuadro de diálogo Agregar vista](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-add-view-dialog.png)

3. Cuando haya agregado estos valores, haga clic en **Agregar** y permita que Visual Studio cree una vista de plantilla. Una vez hecho, se abre el archivo cshtml que se crea. Puede cerrar ese archivo en Visual Studio, ya que se volverá a él más tarde.

#### <a name="AddNewIndexView"></a>Agregar una nueva vista de elementos

Lo mismo que ha creado una vista para elementos de lista, cree una vista para crear elementos mediante los pasos siguientes:

1. En el **Explorador de soluciones**, haga clic de nuevo en la carpeta **Elemento** con el botón derecho, seleccione **Agregar** y, a continuación, **Vista**.

1. En el cuadro de diálogo **Agregar vista**, actualice los siguientes valores:
   
   * En el cuadro **Nombre de vista**, escriba ***Crear***.
   * En el cuadro **Plantilla**, seleccione ***Crear***.
   * En el cuadro **Clase de modelo**, seleccione ***Elemento (todo.Models)***.
   * En el cuadro de página de diseño, escriba ***~/Views/Shared/_Layout.cshtml***.
   * Seleccione **Agregar**.
   
#### <a name="AddEditIndexView"></a>Agregar una vista de edición de elementos

Y por último, agregue una vista para editar un elemento con los pasos siguientes:

1. En el **Explorador de soluciones**, haga clic de nuevo en la carpeta **Elemento** con el botón derecho, seleccione **Agregar** y, a continuación, **Vista**.

1. En el cuadro de diálogo **Agregar vista** , realice lo siguiente:
   
   * En el cuadro **Nombre de vista**, escriba ***Editar***.
   * En el cuadro **Plantilla**, seleccione ***Editar***.
   * En el cuadro **Clase de modelo**, seleccione ***Elemento (todo.Models)***.
   * En el cuadro de página de diseño, escriba ***~/Views/Shared/_Layout.cshtml***.
   * Seleccione **Agregar**.

Una vez hecho esto, cierre todos los documentos cshtml en Visual Studio, ya que volverá a estas vistas más tarde.

## <a name="connect-to-cosmosdb"></a>Paso 5: Conexión a Azure Cosmos DB 

Ahora que ya se han tratado los elementos estándar de MVC, se va a agregar el código para conectarse a Azure Cosmos DB y realizar operaciones CRUD. 

### <a name="perform-crud-operations"></a> Realizar operaciones CRUD en los datos

Lo primero que debe hacerse es agregar una clase que contenga la lógica para conectarse a Azure Cosmos DB y usarlo. En este tutorial, se encapsulará esta lógica en una clase llamada TodoItemService.cs. Este código lee los valores del punto de conexión de Azure Cosmos DB del archivo de configuración y realiza operaciones CRUD, como mostrar elementos incompletos y crear, editar y eliminar los elementos. 

1. En el **Explorador de soluciones**, cree una carpeta bajo el proyecto denominada **Servicios**.

1. Haga clic con el botón derecho en la carpeta **Servicios**, seleccione **Agregar** y, después, seleccione **Clase**. Asigne a la nueva clase el nombre **TodoItemService** y seleccione **Agregar**.

1. Agregue el código siguiente a la clase **TodoItemService** y reemplace el código de ese archivo por el código siguiente:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-web-app/src/Services/TodoItemService.cs)]
 
1. El código anterior lee los valores de la cadena de conexión del archivo de configuración. Para actualizar los valores de la cadena de conexión de la cuenta de Azure Cosmos, abra el archivo **Web.config** en el proyecto y agregue las líneas siguientes bajo la sección `<AppSettings>`:

   ```csharp
    <add key="endpoint" value="<enter the URI from the Keys blade of the Azure Portal>" />
    <add key="primaryKey" value="<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>" />
    <add key="database" value="Tasks" />
    <add key="container" value="Items" />
   ```
 
1. Actualice los valores endpoint y primarykey con los valores recuperados de la hoja **Claves** de Azure Portal. Use el **URI** de la hoja Claves como valor de la configuración de punto de conexión y utilice **CLAVE PRINCIPAL** o **CLAVE SECUNDARIA** de la hoja Claves como valor de la configuración de claves. Esta acción se encarga de la conexión de Azure Cosmos DB a la aplicación; ahora se agregará la lógica de la aplicación.

1. Abra **Global.asax.cs** y agregue la siguiente línea al método **Application_Start** 
   
   ```csharp
   TodoItemService.Initialize().GetAwaiter().GetResult();
   ```

   En este punto, la solución debe poder compilar el proyecto sin errores. Si ejecuta la aplicación ahora, se abrirá **HomeController** y la vista **Índice** de ese controlador. Este es el comportamiento predeterminado para el proyecto de plantillas MVC que se seleccionó al comienzo. Cambiemos el enrutamiento en esta aplicación MVC para modificar este comportamiento.

1. Abra ***App\_Start\RouteConfig.cs***, busque la línea que empieza con "defaults:" y actualícela con el siguiente código:

   ```csharp
   defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }
   ```

   Este código indica ahora a ASP.NET MVC que si no ha especificado un valor en la dirección URL para controlar el comportamiento de enrutamiento, en lugar de **Inicio**, se use **Elemento** como controlador e **Índice** como vista.

Ahora, si ejecuta la aplicación, llama a su **ItemController** que llama a los métodos GetItems desde la clase TodoItemService que defina en la sección siguiente. 

Si crea y ejecuta este proyecto ahora, deberá ver algo parecido a esto.    

![Captura de pantalla de la aplicación web de lista de tareas pendientes creada con este tutorial de base de datos](./media/sql-api-dotnet-application-preview/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Paso 6: Ejecución de la aplicación de forma local

Para probar la aplicación en su máquina local, use los siguientes pasos:

1. Presione F5 en Visual Studio para compilar la aplicación en modo de depuración. De esa forma, se debe compilar la aplicación e iniciar un explorador con la página de cuadrícula vacía que vimos anteriormente:
   
   ![Captura de pantalla de la aplicación web de lista de tareas pendientes creada con este tutorial de base de datos](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. Haga clic en el vínculo **Crear nuevo** y agregue valores a los campos **Nombre** y **Descripción**. Deje la casilla **Completado** sin seleccionar; de lo contrario, el elemento nuevo se agrega en estado completado y no aparece en la lista inicial.
   
3. Haga clic en **Crear**; se le redirigirá a la vista **Índice** y aparecerá el elemento en la lista. Puede agregar unos cuantos elementos más a la lista de tareas pendientes.

    ![Captura de pantalla de la vista de índice](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-an-item.png)
  
4. Haga clic en **Editar** junto a un **elemento** de la lista y llegará a la vista de **edición**, donde puede actualizar cualquier propiedad de su objeto, incluida la marca **Completado**. Si selecciona la marca **Completado** y hace clic en **Guardar**, el **elemento** se quita de la lista de tareas incompletas.
   
   ![Captura de pantalla de la vista de índice con el cuadro Completado activado](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-completed-item.png)

5. Una vez que haya probado la aplicación, presione Ctrl+F5 para detener la depuración de la aplicación. Está listo para la implementación.

## <a name="deploy-the-application-to-azure"></a>Paso 7: Implementación de la aplicación 
Ahora que toda la aplicación funciona correctamente con Azure Cosmos DB, vamos a implementar esta aplicación web en Azure App Service.  

1. Para publicar esta aplicación, haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Publicar**.
   
2. En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service**, luego seleccione **Crear nuevo** para crear un perfil de App Service o elija **Seleccionar existente** para usar un perfil existente.

3. Si tiene un perfil de Azure App Service existente, seleccione una **suscripción** de la lista desplegable. Use el filtro **Ver** para ordenar por grupo de recursos o tipo de recurso. A continuación, busque la instancia de Azure App Service necesaria y seleccione **Aceptar**. 
   
   ![Cuadro de diálogo App Service en Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-app-service.png)

4. Para crear un nuevo perfil de Azure App Service, haga clic en **Crear nuevo** en el cuadro de diálogo **Publicar**. En el cuadro de diálogo **Crear App Service**, escriba el nombre de la aplicación web y la suscripción, el grupo de recursos y el plan de App Service adecuados y, luego, haga clic en **Crear**.

   ![Cuadro de diálogo Crear App Service en Visual Studio](./media/sql-api-dotnet-application-preview/asp-net-mvc-tutorial-create-app-service.png)

En unos segundos, Visual Studio publica la aplicación web e inicia un explorador donde puede ver su trabajo ejecutándose en Azure.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a compilar una aplicación web ASP.NET MVC que puede tener acceso a los datos almacenados en Azure Cosmos DB. Avance al siguiente artículo:

> [!div class="nextstepaction"]
> [Compilación de una aplicación Java para acceder a los datos almacenados en una cuenta de API de SQL de Azure Cosmos DB]( sql-api-java-application.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-todo-app
