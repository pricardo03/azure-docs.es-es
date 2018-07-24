---
title: 'Tutorial: Creación, depuración e implementación de una aplicación web multiservicio en Azure Service Fabric Mesh | Microsoft Docs'
description: En este tutorial se crea una aplicación Azure Service Fabric Mesh multiservicio que consta de un sitio web de ASP.NET Core que se comunica con un servicio web de back-end, lo depura localmente y lo publica en Azure.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 692a384c3695001677b7a3d6161b77f3f015ad7c
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125876"
---
# <a name="tutorial-create-debug-and-deploy-a-multi-service-web-application-to-service-fabric-mesh"></a>Tutorial: Creación, depuración e implementación de una aplicación web multiservicio en Azure Service Fabric Mesh

Este tutorial es la primera parte de una serie. Aprenderá a crear una aplicación de Azure Service Fabric Mesh que tiene un servicio de front-end web de ASP.NET y un servicio de back-end de ASP.NET Core Web API. A continuación, depurará la aplicación en el clúster de desarrollo local y publicará la aplicación en Azure. Cuando haya terminado, tendrá una aplicación sencilla de tareas pendientes que muestra cómo realizar una llamada entre servicios en una aplicación que se ejecuta en Azure Service Fabric Mesh.

Si no desea crear manualmente la aplicación de tareas pendientes, puede [descargar el código fuente](https://github.com/azure-samples/service-fabric-mesh) de la aplicación terminada y pasar directamente a [Depuración local de la aplicación](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md).

En la primera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Crear una aplicación de Service Fabric Mesh que consta de un front-end web ASP.NET.
> * Crear un modelo que representa los elementos de tareas pendientes.
> * Crear un servicio back-end y recuperar los datos de él.
> * Agregar un controlador y un objeto DataContext como parte del patrón de controlador de vista de modelos para el servicio back-end.
> * Crear una página web para mostrar los elementos de tareas pendientes.
> * Crear variables de entorno que identifican el servicio back-end.

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * Crear una aplicación Service Fabric Mesh
> * [Depurar localmente la aplicación](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Publicar la aplicación en Azure](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Asegúrese de que se haya [configurado el entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md), que incluye la instalación del entorno de ejecución de Service Fabric, el SDK, Docker y Visual Studio 2017.

## <a name="create-a-service-fabric-mesh-project"></a>Creación de un proyecto de Service Fabric Mesh

Ejecute Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto...**

En el cuadro de diálogo **Nuevo proyecto**, en el cuadro **Buscar** de la parte superior, escriba `mesh`. Seleccione la plantilla **Aplicación de Service Fabric Mesh**. [Si no ve la plantilla, asegúrese de que ha instalado el SDK de Mesh y la versión preliminar de las herramientas de VS como se describe en [Set up your Windows development environment to build Service Fabric applications ](service-fabric-mesh-howto-setup-developer-environment-sdk.md) (Configuración de un entorno de desarrollo Windows para compilar aplicaciones de Service Fabric)].  

En el cuadro **Nombre**, escriba `todolistapp` y en el cuadro **Ubicación**, establezca como ruta de acceso a la carpeta la ubicación en que desea que se almacenen los archivos del proyecto.

Asegúrese de que la opción **Create directory for solution** (Crear directorio para la solución) esté activada y haga clic en **Aceptar** para crear el proyecto de Service Fabric Mesh.

![Cuadro de diálogo de nuevo proyecto de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-project.png)

Luego verá el cuadro de diálogo **Nuevo servicio de Service Fabric**.

### <a name="create-the-web-front-end-service"></a>Creación del servicio front-end web

En el cuadro de diálogo **Nuevo servicio de Service Fabric**, seleccione el tipo de proyecto **ASP.NET Core** y asegúrese de que en **Container OS** (SO de contenedor) se ha seleccionado **Windows**.

En **Service Name** (Nombre de servicio), seleccione **WebFrontEnd**. Presione **Aceptar** para crear el servicio ASP.NET Core.

![Cuadro de diálogo de nuevo proyecto de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-service-fabric-service.png)

A continuación verá el cuadro de diálogo **Nueva aplicación web ASP.NET Core**. En el cuadro de diálogo **Nueva aplicación web ASP.NET Core**, seleccione **Aplicación web** y haga clic en **Aceptar**.

![Nueva aplicación ASP.NET Core de Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-aspnetcore-app.png)

Ya tiene una aplicación de Service Fabric Mesh. A continuación, cree el modelo para la información de tareas pendientes.

## <a name="create-the-to-do-items-model"></a>Creación del modelo de elementos de tareas pendientes

Por simplicidad, las tareas pendientes se almacenan en una lista en memoria. Cree una biblioteca de clases para los elementos de tareas pendientes y una lista que los contenga. En Visual Studio, que actualmente tiene **todolistapp** cargado, seleccione **Archivo** > **Agregar** > **Nuevo proyecto**.

En el cuadro de diálogo **Nuevo proyecto**, en el cuadro **Buscar** de la parte superior, escriba `C# .net core class`. Seleccione la plantilla **Biblioteca de clases (.NET Core)**.

En el cuadro **Nombre**, escriba `Model`. Haga clic en **Aceptar** para crear la biblioteca de clases.

En el Explorador de soluciones, en **Modelo**, haga clic con el botón derecho en **Class1.cs** y elija **Cambiar nombre**. Cambiar el nombre de la clase **ToDoItem.cs**. Si aparece un aviso en el que se le pregunta si desea cambiar el nombre de todas las referencias, haga clic en **Sí**.

Reemplace el contenido del elemento `class ToDoItem` vacío por:

```csharp
public class ToDoItem
{
    public string Description { get; set; }
    public int Index { get; set; }
    public bool Completed { get; set; }

    public ToDoItem(string description)
    {
        Description = description;
        Index = 0;
    }

    public static ToDoItem Load(string description, int index, bool completed)
    {
        ToDoItem newItem = new ToDoItem(description)
        {
            Index = index,
            Completed = completed
        };

        return newItem;
    }
}
```

Esta clase representa elementos de tareas pendientes individuales.

En Visual Studio, haga clic con el botón derecho en la biblioteca de clases **Model** y seleccione **Agregar** > **Clase...**  para crear una lista que contenga los elementos de tareas pendientes. Aparecerá el cuadro de diálogo **Agregar un nuevo elemento**. En **Nombre**, seleccione `ToDoList.cs` y haga clic en **Agregar**.

En **ToDoList.cs**, reemplace el elemento `class ToDoList` vacío por:

```csharp
public class ToDoList
{
    private List<ToDoItem> _items;

    public string Name { get; set; }
    public IEnumerable<ToDoItem> Items { get => _items; }

    public ToDoList(string name)
    {
        Name = name;
        _items = new List<ToDoItem>();
    }

    public ToDoItem Add(string description)
    {
        var item = new ToDoItem(description);
        _items.Add(item);
        item.Index = _items.IndexOf(item);
        return item;
    }
    public void Add(ToDoItem item)
    {
        _items.Add(item);
        item.Index = _items.Count - 1;
    }

    public ToDoItem RemoveAt(int index)
    {
        if (index >= 0 && index < _items.Count)
        {
            var result = _items[index];
            _items.RemoveAt(index);

            // Reorder items
            for (int i = index; i < _items.Count; i++)
            {
                _items[i].Index = i;
            }

            return result;
        }
        else
        {
            throw new IndexOutOfRangeException();
        }
    }
}
```

A continuación, cree el servicio de Service Fabric que realizará el seguimiento de los elementos de tareas pendientes.

## <a name="create-the-back-end-service"></a>Creación del servicio back-end

En la ventana del **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en **todolistapp** y haga clic en **Agregar** > **Nuevo servicio de Service Fabric...**

Aparece el cuadro de diálogo **Nuevo servicio de Service Fabric**. Seleccione el tipo de proyecto **ASP.NET Core** y asegúrese de que en **OS Container** (SO de contenedor) se ha seleccionado **Windows**.

En **Service Name** (Nombre de servicio), seleccione **ToDoService**. Haga clic en **Aceptar** para crear el servicio ASP.NET Core. A continuación, aparece el cuadro de diálogo **Nueva aplicación web ASP.NET Core**. En dicho cuadro de diálogo, seleccione **API** y, después, **Aceptar**, y un proyecto del servicio se agrega a la solución.

![Nueva aplicación ASP.NET Core de Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-webapi.png)

Dado que el servicio back-end no proporciona ninguna interfaz de usuario, desactive la opción de iniciar el explorador cuando se inicia el servicio. En el **Explorador de soluciones**, haga clic con el botón derecho en **ToDoService** y seleccione **Propiedades**. En la ventana de propiedades que aparece, seleccione la pestaña **Depurar** de la izquierda y desactive la opción **Iniciar explorador**. Presione **CTRL+S** para guardar el cambio.

Dado que este servicio mantiene la información de tareas pendientes, agregue una referencia a la biblioteca de clases Model. En el Explorador de soluciones, haga clic con el botón derecho en **ToDoService** y seleccione **Agregar** > **Referencia...** Aparecerá el cuadro de diálogo **Administrador de referencias**.

En **Administrador de referencias**, seleccione la casilla de **Model** y haga clic en **Aceptar**.

### <a name="add-a-data-context"></a>Adición de un contexto de datos

A continuación, cree un contexto de datos que coordine cómo se sirven los datos del modelo de datos.

Para agregar la clase del contexto de datos, en el Explorador de soluciones, haga clic con el botón derecho en **ToDoService** y, después, en **Agregar** > **Clase**.
En el cuadro de diálogo **Agregar un nuevo elemento** que aparece, asegúrese de que está seleccionada **Clase**, en **Nombre** seleccione `DataContext` y haga clic en **Agregar**.

En **DataContext.cs**, reemplace el contenido del elemento `class DataContext` vacío por:

```csharp
public static class DataContext
{
    public static Model.ToDoList ToDoList { get; } = new Model.ToDoList("Azure learning List");

    static DataContext()
    {
        ToDoList = new Model.ToDoList("Main List");

        // Seed to-do list

        ToDoList.Add(Model.ToDoItem.Load("Learn about microservices", 0, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric", 1, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric Mesh", 2, false));
    }
}
```

Este contexto de datos mínimos rellena algunos elementos de tareas pendientes de ejemplo y proporciona acceso a ellos.

### <a name="add-a-controller"></a>Adición de un controlador

Al crear el proyecto **ToDoService**, la plantilla proporcionó un controlador predeterminado que controla las solicitudes HTTP y crea la respuesta HTTP. En el **Explorador de soluciones**, en **ToDoService**, abra la carpeta **Controllers** para ver el archivo **ValuesController.cs**. 

Haga clic con el botón derecho en **ValuesController.cs** y, después, en **Cambiar nombre**. Cambie el nombre del archivo `ToDoController.cs`. Si aparece un aviso acerca del cambio de nombre de todas las referencias, haga clic en **Sí**.

Abra el archivo **ToDoController.cs** y reemplace el contenido de `class ToDoController` por:

```csharp
[Route("api/[controller]")]
public class ToDoController : Controller
{
    // GET api/todo
    [HttpGet]
    public IEnumerable<Model.ToDoItem> Get()
    {
        return DataContext.ToDoList.Items;
    }

    // GET api/todo/5
    [HttpGet("{index}")]
    public Model.ToDoItem Get(int index)
    {
        return DataContext.ToDoList.Items.ElementAt(index);
    }

    //// POST api/values
    //[HttpPost]
    //public void Post([FromBody]string value)
    //{
    //}

    //// PUT api/values/5
    //[HttpPut("{id}")]
    //public void Put(int id, [FromBody]string value)
    //{
    //}

    // DELETE api/values/5
    [HttpDelete("{index}")]
    public void Delete(int index)
    {
    }
}
```

En este tutorial no se implementa la adición, eliminación, etc.: Nos centramos en la comunicación con otro servicio.

## <a name="create-the-web-page-that-displays-to-do-items"></a>Creación de la página web que muestra los elementos de tareas pendientes

Con el servicio back-end implementado, escriba el código del sitio web que mostrará los elementos de tareas pendientes que proporciona. Los siguientes pasos se realizan en el proyecto **WebFrontEnd**.

La página web que muestra los elementos de tareas pendientes necesita acceder a la clase **ToDoItem** y a la lista.
En el **Explorador de soluciones**, agregue una referencia al proyecto Model, para lo que debe hacer clic con el botón derecho en **WebFrontEnd** y seleccionar **Agregar** > **Referencia...** Aparecerá el cuadro de diálogo **Administrador de referencias**.

En **Administrador de referencias**, haga clic en la casilla de **Model** y haga clic en **Aceptar**.

En el **Explorador de soluciones**, abra la página Index.cshtml, para lo que debe ir a **WebFrontEnd** > **Pages** > **Index.cshtml**. Abra **index.cshtml**.

Reemplace el contenido de todo el archivo por el siguiente código HTML que define una tabla sencilla que muestra los elementos de tareas pendientes:

```HTML
@page
@model IndexModel
@{
    ViewData["Title"] = "Home page";
}

<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Abra el código de la página de índice en el **Explorador de soluciones**, para lo que debe abrir **Index.cshtml** y, después, **Index.cshtml.cs**. En la parte superior de **Index.cshtml.cs**, agregue `using System.Net.Http;`

Reemplace el contenido de `public class IndexModel` por:

```csharp
public class IndexModel : PageModel
{
    public Model.ToDoItem[] Items = new Model.ToDoItem[] { };

    public void OnGet()
    {
        HttpClient client = new HttpClient();

        using (HttpResponseMessage response = client.GetAsync(backendUrl).GetAwaiter().GetResult())
        {
            if (response.StatusCode == System.Net.HttpStatusCode.OK)
            {
                Items = Newtonsoft.Json.JsonConvert.DeserializeObject<Model.ToDoItem[]>(response.Content.ReadAsStringAsync().Result);
            }
        }
    }

    private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ServiceName")}";
    private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
}
```

### <a name="create-environment-variables"></a>Creación de variables de entorno

La dirección URL del servicio back-end es necesaria para comunicarse con dicho servicio. Para este tutorial, el siguiente fragmento de código (que se ha define anteriormente como parte de IndexModel) lee las variables de entorno para crear la dirección URL:

```csharp
private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ServiceName")}";
private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
```

La dirección URL está formada por el nombre del servicio y el puerto. Toda esta información se encuentra en el archivo service.yaml, que se encuentra en el proyecto **ToDoService**.

Vaya al **Explorador de soluciones**, al proyecto **ToDoService** y abra **Service Resources** > **service.yaml**.

![Figura 1: el archivo service.yaml de ToDoService](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-port.png)

* El nombre del servicio, `ToDoService`, se encuentra en `services:`, después de `name:`. Consulte (1) en la ilustración anterior.
* El puerto, `20008`, se encuentra en `endpoints:`, después de `port:`. Consulte (2) en la ilustración anterior. Es posible que el número de puerto de su proyecto no coincida.

Después se definirán las variables de entorno que representan el nombre del servicio y el número de puerto en el proyecto WebFrontEnd, con el fin de que pueda llamar al servicio back-end.

En el **Explorador de soluciones**, vaya a **WebFrontEnd** > **Service Resources** > **service.yaml** para definir las variables que especifican la dirección del servicio back-end.

En el archivo service.yaml, agregue las siguientes variables en `environmentVariables`. El espaciado es importante, así que alinee las variables que agregue con las restantes de `environmentVariables:`

> [!IMPORTANT]
> Para aplicar sangría a las variables del archivo service.yaml se deben usar espacios, no tabulaciones, ya que si se usan estas, no se compilará. Visual Studio puede insertar tabulaciones cuando se crean las variables de entorno. Reemplace todas las tabulaciones por espacios. Aunque, verá errores en la salida de depuración de **build**, la aplicación se iniciará. Sin embargo, esto no funcionará hasta que convierta las tabulaciones en espacios. Para asegurarse de que no hay tabulaciones en el archivo service.yaml, puede activar la visualización de espacios en blanco en el editor de Visual Studio con **Edición**  > **Opciones avanzadas**  > **Ver espacios en blanco**.

El archivo **service.yaml** del proyecto **WebFrontEnd** debe ser parecido al siguiente, aunque su valor de `ApiHostPort` probablemente será diferente:

![Service.yaml en el proyecto WebFrontEnd](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-envvars.png)

Ya está listo para compilar e implementar la imagen de la aplicación de Service Fabric Mesh, junto con el servicio web back-end, en el clúster local.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Crear una aplicación de Service Fabric Mesh que consta de un front-end web ASP.NET.
> * Crear un modelo que representa los elementos de tareas pendientes.
> * Crear un servicio back-end y recuperar los datos de él.
> * Agregar un controlador y un objeto DataContext como parte del patrón de controlador de vista de modelos para el servicio back-end.
> * Crear una página web para mostrar los elementos de tareas pendientes.
> * Crear variables de entorno que identifican el servicio back-end.

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Depuración de una aplicación de Service Fabric Mesh que se ejecuta localmente](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
