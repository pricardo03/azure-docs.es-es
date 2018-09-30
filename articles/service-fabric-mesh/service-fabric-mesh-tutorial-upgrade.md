---
title: 'Tutorial: Actualización de una aplicación de Azure Service Fabric Mesh | Microsoft Docs'
description: Actualización de una aplicación de Service Fabric con Visual Studio
services: service-fabric-mesh
documentationcenter: .net
author: tylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: a734eb7b4efecf14a4d47a660c00d561a5fa6677
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971814"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Tutorial: Actualización de una aplicación de Service Fabric con Visual Studio

Este tutorial es la cuarta parte de una serie y muestra cómo actualizar una aplicación de Azure Service Fabric Mesh directamente desde Visual Studio. La actualización incluirá tanto una actualización de código como una actualización de configuración. Verá que los pasos para actualizar y publicar desde Visual Studio son los mismos.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Actualización de un servicio de Service Fabric Mesh mediante Visual Studio

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación de Service Fabric Mesh en Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Depurar una aplicación de Service Fabric Mesh que se ejecuta en el clúster de desarrollo local](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Implementar una aplicación de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Actualizar una aplicación de Service Fabric Mesh
> * [Limpiar los recursos de Service Fabric Mesh](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no ha implementado la aplicación de tareas pendientes, siga las instrucciones de [Publicación de una aplicación web de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

# <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Actualización de un servicio de Service Fabric Mesh mediante Visual Studio

En este artículo se muestra cómo actualizar microservicios de forma independiente en una aplicación.  En este ejemplo, se modificará el servicio `WebFrontEnd` para mostrar una categoría de tarea. Después, se actualizará el servicio implementado.

## <a name="modify-the-config"></a>Modificación de la configuración

Las actualizaciones pueden deberse a cambios de código, cambios de configuración o ambos.  Para introducir un cambio de configuración, abra el archivo `WebFrontEnd` del proyecto `service.yaml` (que se encuentra bajo el nodo **Recursos del servicio**).

En la sección `resources:`, cambie `cpu:` de 0,5 a 1,0, en previsión de que el front end de la web se utilice mucho. Debería ser parecido a este:

```xml
              ...
              resources:
                requests:
                  cpu: 1.0
                  memoryInGB: 1
              ...
```

## <a name="modify-the-model"></a>Modificación del modelo

Para introducir un cambio de código, agregue una propiedad `Category` a la clase `ToDoItem` en el archivo `ToDoItem.cs`.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

A continuación, actualice el método `Load()`, en el mismo archivo, para establecer la categoría en una cadena predeterminada:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>Modificación del servicio

El proyecto `WebFrontEnd` es una aplicación de ASP.NET Core con una página web en la que se muestran elementos de la lista de tareas pendientes. En el proyecto `WebFrontEnd`, abra `Index.cshtml` y agregue las dos líneas siguientes, indicadas a continuación, para mostrar la categoría de la tarea:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Cree y ejecute la aplicación para comprobar que ve una nueva columna de categoría en la página web que enumera las tareas.

## <a name="upgrade-the-app-from-visual-studio"></a>Actualización de la aplicación desde Visual Studio

Con independencia de si está haciendo una actualización de código o una actualización de configuración (en este caso estamos haciendo ambas cosas), para actualizar la aplicación Service Fabric Mesh en Azure haga clic con el botón derecho del ratón en **todolistapp** en Visual Studio y seleccione **Publicar...**

A continuación, verá un cuadro de diálogo **Publicación de la aplicación de Service Fabric**.

![Cuadro de diálogo de publicación de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Seleccione la cuenta y la suscripción de Azure. Asegúrese de que **Ubicación** se establece en la ubicación que ha usado cuando se publicó originalmente la aplicación de tareas pendientes en Azure. Este artículo utiliza **Este de EE. UU**.

Asegúrese de que **Grupo de recursos** se establece en el grupo de recursos que ha usado cuando se publicó originalmente la aplicación de tareas pendientes en Azure.

Asegúrese de que **Azure Container Registry** se establece en el nombre de Azure Container Registry que ha usado cuando se publicó originalmente la aplicación de tareas pendientes en Azure.

En el cuadro de diálogo Publicar, presione el botón **Publicar** para actualizar la aplicación de tareas pendientes en Azure.

Para supervisar el progreso de la implementación, seleccione el panel **Herramientas de Service Fabric** en la ventana **Salida** de Visual Studio. Una vez finalizada la actualización, la salida de **Herramientas de Service Fabric** mostrará la dirección IP y el puerto de la aplicación en forma de dirección URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Abra un explorador web y vaya a la dirección URL para ver el sitio web que se ejecuta en Azure. Ahora debería ver una página web que contiene una columna de categoría.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:
> [!div class="checklist"]
> * Actualización de una aplicación de Service Fabric Mesh mediante Visual Studio

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Limpiar los recursos de Service Fabric Mesh](service-fabric-mesh-tutorial-cleanup-resources.md)