---
title: Creación de un espacio de desarrollo de Kubernetes en la nube | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 06/06/2018
ms.topic: quickstart
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: 16ec493708f85e9b3819943e131b9f9c3649f27e
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824645"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-net-core-and-visual-studio"></a>Inicio rápido: Creación de un espacio de desarrollo de Kubernetes con Azure Dev Spaces (.NET Core y Visual Studio)

En esta guía, aprenderá a:

- Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
- Desarrollar código de forma iterativa en contenedores con Visual Studio
- Depurar el código que se ejecuta en un clúster.

> [!Note]
> **Si se queda bloqueado** en cualquier momento, consulte la sección [Solución de problemas](troubleshooting.md) o publique un comentario en esta página. También puede probar con este [tutorial](get-started-netcore-visualstudio.md), que es más detallado.

## <a name="prerequisites"></a>Requisitos previos

- Un clúster de Kubernetes que ejecute Kubernetes 1.9.6, en las regiones EastUS, WestEurope o CanadaEast, con Enrutamiento de solicitudes HTTP habilitado.

  ![No olvide habilitar Enrutamiento de solicitudes HTTP.](media/common/Kubernetes-Create-Cluster-3.PNG)

- Visual Studio 2017 con la carga de trabajo Desarrollo de Web instalada. Si no está instalada, descárguela [aquí](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

## <a name="set-up-azure-dev-spaces"></a>Configuración de Azure Dev Spaces

Instale la [extensión de Visual Studio para Azure Dev Spaces](https://aka.ms/get-azds-visualstudio).

## <a name="connect-to-a-cluster"></a>Conectarse a un clúster

A continuación, creará y configurará un proyecto para Azure Dev Spaces.

### <a name="create-an-aspnet-web-app"></a>Creación de una aplicación web de ASP.NET

En Visual Studio 2017, cree un nuevo proyecto. Actualmente, el proyecto debe ser una **aplicación web de ASP.NET Core**. Asigne el nombre **webfrontend** al proyecto.

Seleccione la plantilla **Aplicación web (controlador de vista de modelos)** y asegúrese de que apunta a **.NET Core** y **ASP.NET Core 2.0**.

### <a name="create-a-dev-space-in-azure"></a>Creación de un espacio de desarrollo en Azure

Con el proyecto que acaba de crear abierto, seleccione **Azure Dev Spaces**  en el menú desplegable de configuración de inicio, como se muestra a continuación.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

En el cuadro de diálogo que se muestra a continuación, asegúrese de que ha iniciado sesión con la cuenta adecuada y, a continuación, seleccione un clúster existente.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Por ahora, deje en el menú desplegable **Espacio** la opción `default`. Active la casilla **Publicly Accessible** (Acceso público) para que la aplicación web esté accesible mediante un punto de conexión público.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Haga clic en **Aceptar** para seleccionar o crear el clúster.

Si elige un clúster que no ha sido configurado para trabajar con Azure Dev Spaces, verá un mensaje preguntando si desea configurarlo.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Elija **Aceptar**. 

### <a name="look-at-the-files-added-to-project"></a>Examen de los archivos agregados al proyecto
Mientras espera a que se cree el espacio de desarrollo, mire los archivos que se han agregado al proyecto cuando eligió usar Azure Dev Spaces.

- Se ha agregado una carpeta llamada `charts` y, dentro de ella, un [gráfico de Helm](https://docs.helm.sh) para la aplicación a la que se ha aplicado la técnica scaffolding. Estos archivos se usan para implementar la aplicación en el espacio de desarrollo.
- `Dockerfile` contiene la información necesaria para empaquetar la aplicación en el formato estándar de Docker.
- `azds.yaml` contiene la información de configuración que necesita el espacio de desarrollo, como por ejemplo si se puede acceder a la aplicación a través de un punto de conexión público.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Depuración de un contenedor en Kubernetes
Cuando el espacio de desarrollo se haya creado correctamente, puede depurar la aplicación. Establezca un punto de interrupción en el código, por ejemplo, en la línea 20 del archivo `HomeController.cs`, donde se establece la variable `Message`. Pulse **F5** para iniciar la depuración. 

Visual Studio se comunicará con el espacio de desarrollo para compilar e implementar la aplicación y, después, abrirá un explorador con la aplicación web en ejecución. Puede parecer que el contenedor se ejecuta localmente, pero en realidad lo hace en el espacio de desarrollo de Azure. La razón de la dirección de localhost es porque Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.

Haga clic en el vínculo **Acerca de** en la parte superior de la página para desencadenar el punto de interrupción. Tiene acceso completo a la información de depuración, tal como lo haría si el código se ejecutara localmente, como la pila de llamadas, las variables locales o la información de excepción, por ejemplo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Trabajo con varios contenedores y desarrollo en equipo](get-started-netcore-visualstudio.md#call-another-container)