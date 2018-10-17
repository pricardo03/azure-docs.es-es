---
title: Creación de un espacio de desarrollo de Kubernetes en la nube | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 09/26/2018
ms.topic: quickstart
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: a5bcd814cce6612c0074f2a85bb8757b50f183ca
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585601"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-net-core-and-vs-code"></a>Inicio rápido: Creación de un espacio de desarrollo de Kubernetes con Azure Dev Spaces (.NET Core y VS Code)

En esta guía, aprenderá a:

- Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
- Desarrollar código de forma iterativa en contenedores con VS Code y la línea de comandos.
- Depurar el código en el espacio de desarrollo de VS Code

> [!Note]
> **Si se queda bloqueado** en cualquier momento, consulte la sección [Solución de problemas](troubleshooting.md) o publique un comentario en esta página. También puede probar con este [tutorial](get-started-netcore.md), que es más detallado.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
- [Visual Studio Code](https://code.visualstudio.com/download)
- [CLI de Azure ](/cli/azure/install-azure-cli?view=azure-cli-latest) versión 2.0.43 o superior.
- Un clúster de Kubernetes que ejecute Kubernetes 1.9.6 o posterior, en las regiones Este de EE. UU., Centro de EE. UU., Oeste de EE. UU. 2, Europa Occidental o Este de Canadá, con **Enrutamiento de aplicación HTTP habilitado**.

    ```cmd
    az group create --name MyResourceGroup --region <region>
    az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.2 --enable-addons http_application_routing
    ```

## <a name="set-up-azure-dev-spaces"></a>Configuración de Azure Dev Spaces

La CLI de Azure y la extensión Azure Dev Spaces se pueden instalar y ejecutar en equipos Windows, Mac o Linux. Para Linux, se admiten las siguientes distribuciones: Ubuntu (18.04, 16.04 y 14.04), Debian 8 y 9, RHEL 7, Fedora 26 +, CentOS 7, openSUSE 42.2 y SLES 12.

Siga estos pasos para configurar Azure Dev Spaces:

1. Configure Dev Spaces en su clúster de AKS: `az aks use-dev-spaces -g MyResourceGroup -n MyAKS`
1. Descargue el [extensión de Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) para VS Code. Haga clic en Instalar una vez que se encuentre en la página de Marketplace de la extensión, y de nuevo en VS Code.

## <a name="build-and-run-code-in-kubernetes"></a>Compilación y ejecución de código en Kubernetes

1. Descargue el código de ejemplo de GitHub:[https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) 
1. Cambie el directorio a la carpeta webfrontend: `cd dev-spaces/samples/dotnetcore/getting-started/webfrontend`
1. Genere los recursos de Docker y del gráfico de Helm: `azds prep --public`
1. Compile y ejecute el código en AKS. En la ventana del terminal, ejecute este comando desde la **carpeta webfrontend**: `azds up`
1. Examine la salida de la consola para obtener información acerca de la dirección URL que creó el comando `up`. Tendrá el formato siguiente: 

   `(pending registration) Service 'webfrontend' port 'http' will be available at <url>\r\nService 'webfrontend' port 80 (TCP) is available at http://localhost:<port>` 

   Abra esta dirección URL en una ventana del explorador y verá la aplicación web de carga. 
   
   > [!Note]
   > En la primera ejecución, puede tardar varios minutos hasta que el DNS público esté preparado. Si no se resuelve la dirección URL pública, puede usar la dirección URL alternativa http://localhost:<portnumber> que se muestra en la salida de la consola. Si utiliza la dirección URL del host local, puede parecer que el contenedor se ejecuta localmente, pero en realidad se ejecuta en AKS. Para mayor comodidad y para facilitar la interacción con el servicio desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure. Puede volver y probar la dirección URL pública más adelante cuando el registro DNS esté listo.

### <a name="update-a-content-file"></a>Actualización de un archivo de contenido

1. Busque un archivo, como `./Views/Home/Index.cshtml`, y realice una modificación en el código HTML. Por ejemplo, cambie la línea 70 que dice `<h2>Application uses</h2>` a algo como: `<h2>Hello k8s in Azure!</h2>`
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

En esta sección, usará VS Code para depurar directamente el contenedor que se ejecuta en Azure. También aprenderá a obtener un bucle de edición-ejecución-prueba más rápido.

![](./media/common/edit-refresh-see.png)

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Inicialización de activos de depuración con la extensión de VS Code
En primer lugar, debe configurar el proyecto de código para que VS Code se comunique con nuestro espacio de desarrollo en Azure. La extensión de VS Code para Azure Dev Spaces proporciona un comando auxiliar para establecer la configuración de depuración. 

Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Esto agrega la configuración de depuración para los espacios de Azure Dev Spaces en la carpeta `.vscode`. Este comando no se debe confundir con el comando `azds prep`, que configura el proyecto para la implementación.

![](./media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>Selección de la configuración de depuración de AZDS
1. Para abrir la vista de depuración, haga clic en el icono de depuración en la **barra de actividad** en el lateral de VS Code.
1. Seleccione **.NET Core Launch (AZDS)** [Inicio de .NET Core (AZDS)] como la configuración de depuración activa.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Si no ve ningún comando de Azure Dev Spaces en la paleta de comandos, asegúrese de haber instalado la extensión de Visual Studio Code para Azure Dev Spaces. Asegúrese de que el área de trabajo que abre en VS Code es la carpeta que contiene azds.yaml.


### <a name="debug-the-container-in-kubernetes"></a>Depuración del contenedor en Kubernetes
Presione **F5**  para depurar el código en Kubernetes.

De forma similar al comando `up`, el código se sincroniza con el espacio de desarrollo y se crea un contenedor que se implementa en Kubernetes. En esta ocasión, por supuesto, el depurador se asocia al contenedor remoto.

> [!Tip]
> La barra de estado de VS Code mostrará una dirección URL en la que se puede hacer clic.

Establezca un punto de interrupción en un archivo de código del lado servidor, por ejemplo en la función `Index()` del archivo de origen `Controllers/HomeController.cs`. Al actualizar la página del navegador, el punto de interrupción se activa.

Tiene acceso completo a la información de depuración, igual que si el código se ejecutara localmente, como la pila de llamadas, las variables locales o la información de excepción, por ejemplo.

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

**Ahora tiene un método para iterar rápidamente el código y depurarlo directamente en Kubernetes.**

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la forma en que Azure Dev Spaces le ayuda a desarrollar aplicaciones más complejas en varios contenedores y cómo puede simplificar el desarrollo en colaboración mediante el uso de distintas versiones o bifurcaciones del código en distintos espacios. 

> [!div class="nextstepaction"]
> [Trabajo con varios contenedores y desarrollo en equipo](team-development-netcore.md)
