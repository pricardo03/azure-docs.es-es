---
title: Creación de un espacio de desarrollo de Kubernetes en la nube mediante .NET Core y VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: 0055276e8ce6ba6e22b8c2e664b3d2ae58b12345
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159731"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core"></a>Introducción a Azure Dev Spaces con .NET Core

[!INCLUDE [](includes/learning-objectives.md)]

[!INCLUDE [](includes/see-troubleshooting.md)]

Ya está preparado para crear un espacio de desarrollo basado en Kubernetes en Azure.

[!INCLUDE [](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure
Azure Dev Spaces requiere una configuración mínima de la máquina local. La mayor parte de la configuración del espacio de desarrollo se almacena en la nube y se puede compartir con otros usuarios. El equipo local puede ejecutar Windows, Mac o Linux. Para Linux, se admiten las siguientes distribuciones: Ubuntu (18.04, 16.04 y 14.04), Debian 8 y 9, RHEL 7, Fedora 26 +, CentOS 7, openSUSE 42.2 y SLES 12.

Para comenzar, descargue y ejecute la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!IMPORTANT]
> Si ya tiene instalada la CLI de Azure, asegúrese de que usa la versión 2.0.43, o cualquier versión superior.

[!INCLUDE [](includes/sign-into-azure.md)]

[!INCLUDE [](includes/use-dev-spaces.md)]

[!INCLUDE [](includes/install-vscode-extension.md)]

Mientras espera a que se cree el clúster, puede empezar a desarrollar código.

## <a name="create-a-web-app-running-in-a-container"></a>Crear una aplicación web que se ejecuta en un contenedor

En esta sección, va a crear una aplicación web de ASP.NET Core y a ejecutarla en un contenedor de Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Cree una aplicación web ASP.NET Core
Si tiene [.NET Core](https://www.microsoft.com/net) instalado, puede crear rápidamente una aplicación web de ASP.NET Core en una carpeta llamada `webfrontend`.
    
```cmd
dotnet new mvc --name webfrontend
```

O bien **descargue el código de ejemplo de GitHub** en https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio GitHub a su entorno local. El código de esta guía se encuentra en `samples/dotnetcore/getting-started/webfrontend`.

[!INCLUDE [](includes/azds-prep.md)]

[!INCLUDE [](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Actualización de un archivo de contenido
Con Azure Dev Spaces no se trata solo de conseguir que el código se ejecute en Kubernetes, sino de permitirle ver rápida e iterativamente que los cambios de código surtan efecto en un entorno de Kubernetes en la nube.

1. Busque el archivo `./Views/Home/Index.cshtml` y realice una edición en el código HTML. Por ejemplo, cambie la línea 70 que dice `<h2>Application uses</h2>` a algo como: `<h2>Hello k8s in Azure!</h2>`
1. Guarde el archivo. Momentos después, en la ventana de terminal verá un mensaje que indica que se ha actualizado un archivo en el contenedor en ejecución.
1. Regrese al explorador y actualice la página. Debería ver la página web que muestra el código HTML actualizado.

¿Qué ha ocurrido? Las ediciones de archivos de contenido, como HTML y CSS, no requieren compilarlos de nuevo en una aplicación web de .NET Core, por lo que un comando `azds up` activo sincroniza automáticamente cualquier archivo de contenido modificado en el contenedor que se está ejecutando en Azure, para que pueda ver sus ediciones de contenido de inmediato.

### <a name="update-a-code-file"></a>Actualización de un archivo de código
Actualizar archivos de código requiere un poco más de trabajo, porque una aplicación .NET Core necesita volver a crear y producir archivos binarios de aplicaciones actualizados.

1. En la ventana de terminal, presione `Ctrl+C` (para detener `azds up`).
1. Abra el archivo de código denominado `Controllers/HomeController.cs` y edite el mensaje que se mostrará en la página Acerca de: `ViewData["Message"] = "Your application description page.";`
1. Guarde el archivo.
1. Ejecute `azds up` en la ventana de terminal. 

Este comando vuelve a crear la imagen del contenedor y vuelve a implementar el gráfico de Helm. Para que los cambios de código surtan efecto en la aplicación en ejecución, vaya al menú Acerca de la aplicación web.


Pero hay un método *más rápido* para desarrollar código, que se verá en la siguiente sección. 

## <a name="debug-a-container-in-kubernetes"></a>Depuración de un contenedor en Kubernetes

[!INCLUDE [](includes/debug-intro.md)]

[!INCLUDE [](includes/init-debug-assets-vscode.md)]


### <a name="select-the-azds-debug-configuration"></a>Selección de la configuración de depuración de AZDS
1. Para abrir la vista de depuración, haga clic en el icono de depuración en la **barra de actividad** en el lateral de VS Code.
1. Seleccione **.NET Core Launch (AZDS)** [Inicio de .NET Core (AZDS)] como la configuración de depuración activa.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Si no ve ningún comando de Azure Dev Spaces en la paleta de comandos, asegúrese de haber instalado la extensión de Visual Studio Code para Azure Dev Spaces. Asegúrese de que el área de trabajo que abre en VS Code es la carpeta que contiene azds.yaml.


### <a name="debug-the-container-in-kubernetes"></a>Depuración del contenedor en Kubernetes
Presione **F5**  para depurar el código en Kubernetes.

De forma similar al comando `up`, el código se sincroniza con el espacio de desarrollo y se crea un contenedor que se implementa en Kubernetes. En esta ocasión, por supuesto, el depurador se asocia al contenedor remoto.

[!INCLUDE [](includes/tip-vscode-status-bar-url.md)]

Establezca un punto de interrupción en un archivo de código del lado servidor, por ejemplo en la función `Index()` del archivo de origen `Controllers/HomeController.cs`. Al actualizar la página del navegador, el punto de interrupción se activa.

Tiene acceso completo a la información de depuración, tal como lo haría si el código se ejecutara localmente, como la pila de llamadas, las variables locales o la información de excepción, por ejemplo.

### <a name="edit-code-and-refresh"></a>Edición del código y actualización
Con el depurador activo, realice una edición del código. Por ejemplo, modifique el mensaje de la página Acerca de en `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Guarde el archivo y, en el **panel de acciones de depuración**, haga clic en el botón **Actualizar**. 

![](media/get-started-netcore/debug-action-refresh.png)

En lugar de volver a crear e implementar una nueva imagen de contenedor cada vez que se realizan modificaciones en el código, lo que a menudo lleva un tiempo considerable, Azure Dev Spaces volverá a compilar el código de manera incremental dentro del contenedor existente para proporcionar un bucle de modificación/depuración más rápido.

Actualice la aplicación web en el explorador y vaya a la página Acerca de. Debería ver aparecer el mensaje personalizado en la interfaz de usuario.

**Ahora tiene un método para iterar rápidamente el código y depurarlo directamente en Kubernetes.** A continuación, verá cómo puede crear y llamar a un segundo contenedor.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el desarrollo en equipo](team-development-netcore.md)
