---
title: 'Tutorial: Actualización de una aplicación de Azure Service Fabric Mesh | Microsoft Docs'
description: Actualización de una aplicación de Service Fabric con Visual Studio
services: service-fabric-mesh
documentationcenter: .net
author: dkkapur
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/29/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 23809abd06d626eb87e5d5d15d265f1769b97b66
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56806747"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Tutorial: Actualización de una aplicación de Service Fabric con Visual Studio

Este tutorial es la cuarta parte de una serie y muestra cómo actualizar una aplicación de Azure Service Fabric Mesh directamente desde Visual Studio. La actualización incluirá tanto una actualización de código como una actualización de configuración. Verá que los pasos para actualizar y publicar desde dentro de Visual Studio son los mismos.

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

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Actualización de un servicio de Service Fabric Mesh mediante Visual Studio

En este artículo se muestra cómo actualizar un microservicio dentro de una aplicación. En este ejemplo, modificaremos el `WebFrontEnd` servicio para mostrar una categoría de tareas y aumentar la cantidad de CPU que se le asigna. Después, se actualizará el servicio implementado.

## <a name="modify-the-config"></a>Modificación de la configuración

Cuando se crea una aplicación de Service Fabric de la malla, Visual studio agrega un **parameters.yaml** archivo para cada entorno de implementación (en la nube y local). En estos archivos, puede definir parámetros y sus valores que, a continuación, se pueden hacer referencia desde los archivos de malla *.yaml como service.yaml o network.yaml.  Visual Studio proporciona algunas de las variables, por ejemplo, cuánta CPU puede usar el servicio.

Actualizaremos el `WebFrontEnd_cpu` parámetro para actualizar los recursos de cpu para `1.5` antes que el **WebFrontEnd** servicio se usará con más asiduidad.

1. En el **todolistapp** del proyecto, en **entornos** > **en la nube**, abra el **parameters.yaml** archivo. Modificar el `WebFrontEnd_cpu`, valor `1.5`. El nombre del parámetro está precedido por el nombre del servicio `WebFrontEnd_` como práctica recomendada para distinguirlo de los parámetros del mismo nombre que se aplican a distintos servicios.

    ```xml
    WebFrontEnd_cpu: 1.5
    ```

2. Abra el **WebFrontEnd** del proyecto **service.yaml** archivo bajo **WebFrontEnd** > **los recursos del servicio**.

    Tenga en cuenta que el de `resources:` sección `cpu:` está establecido en `"[parameters('WebFrontEnd_cpu')]"`. Si el proyecto se compila para la nube, el valor de `'WebFrontEnd_cpu` se tomará de la **entornos** > **en la nube** > **parameters.yaml** de archivos y será `1.5`. Si el proyecto se compila para ejecutarse localmente, se tendrá el valor de la **entornos** > **Local** > **parameters.yaml** archivo, y será "0,5".

> [!Tip]
> De forma predeterminada, el archivo de parámetros del mismo nivel del archivo profile.yaml se usará para proporcionar los valores para ese archivo profile.yaml.
> Por ejemplo, entornos > nube > parameters.yaml proporciona los valores de parámetro para entornos > nube > profile.yaml.
>
> Puede invalidar esto agregando lo siguiente al archivo profile.yaml:`parametersFilePath=”relative or full path to the parameters file”` Por ejemplo,  o `parametersFilePath=”..\CommonParameters.yaml”`.

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

Si está realizando una actualización de código o una actualización de configuración (en este caso que estamos haciendo ambos), actualice la aplicación de malla de Service Fabric en Azure con el botón secundario en **todolistapp** en Visual Studio y, a continuación, seleccione **publicar...**

A continuación, verá un cuadro de diálogo **Publicación de la aplicación de Service Fabric**.

Utilice la **perfil objetivo** lista desplegable para seleccionar el archivo profile.yaml que se usará para esta implementación. Estamos actualizando la aplicación en la nube, así que seleccionamos la **cloud.yaml** en la lista desplegable, que usará el `WebFrontEnd_cpu` valor de 1.0 definida en ese archivo.

![Cuadro de diálogo de publicación de Service Fabric mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Seleccione la cuenta y la suscripción de Azure. Establecer el **ubicación** a la ubicación que usó cuando se publicó originalmente la aplicación de tareas pendientes en Azure. Este artículo utiliza **Este de EE. UU**.

Establecer **grupo de recursos** al grupo de recursos que usó cuando se publicó originalmente la aplicación de tareas pendientes en Azure.

Establecer **Azure Container Registry** para el nombre del registro de contenedor de azure que creó cuando se publicó originalmente la aplicación de tareas pendientes en Azure.

En el cuadro de diálogo Publicar, presione el botón **Publicar** para actualizar la aplicación de tareas pendientes en Azure.

Supervisar el progreso de la actualización seleccionando el **herramientas de Service Fabric** panel en Visual Studio **salida** ventana. 

Después de la imagen se compila y se insertan en Azure Container Registry, un **estado** vínculo aparecerá en la salida que puede hacer clic para supervisar la implementación en Azure portal.

Una vez finalizada la actualización, la salida de **Herramientas de Service Fabric** mostrará la dirección IP y el puerto de la aplicación en forma de dirección URL.

```json
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Abra un explorador web y vaya a la dirección URL para ver el sitio web que se ejecuta en Azure. Ahora debería ver una página web que contiene una columna de categoría.

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:
> [!div class="checklist"]
> * Cómo actualizar una aplicación de Service Fabric Mesh mediante Visual Studio

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Limpiar los recursos de Service Fabric Mesh](service-fabric-mesh-tutorial-cleanup-resources.md)