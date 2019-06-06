---
title: Configuración de un entorno de desarrollo de Windows para compilar aplicaciones de Service Fabric Mesh | Microsoft Docs
description: Configure el entorno de desarrollo de Windows para que pueda crear una aplicación de Service Fabric Mesh e implementarla en Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 5ab817c65ab562f37b456cc3589624c1876084f0
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428201"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Configuración del entorno de desarrollo de Windows para compilar aplicaciones de Service Fabric Mesh

Para compilar y ejecutar aplicaciones de Azure Service Fabric Mesh en el equipo de desarrollo Windows, necesita:

* Docker
* Visual Studio 2017 o posterior
* Service Fabric Mesh en tiempo de ejecución.
* SDK y herramientas de Service Fabric Mesh.

Una de las siguientes versiones de Windows:

* Windows 10 (Enterprise, Professional o Education), versiones 1709 (Fall Creators Update) o 1803 (Actualización de abril de 2018 de Windows 10)
* Windows Server versión 1709
* Windows Server versión 1803

Usará las instrucciones siguientes le ayudarán a instalar todo se basa en la versión de Windows que se está ejecutando.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 o posterior es necesario para implementar aplicaciones de Service Fabric de la malla. [Instale la versión 15.6.0][download-visual-studio], u otra posterior, y habilite las cargas de trabajo siguientes:

* ASP.NET y desarrollo web
* Desarrollo de Azure

## <a name="install-docker"></a>Instalación de Docker

Si ya tiene instalado Docker, asegúrese de que tiene la última versión. Docker puede pedirle que cuando una nueva versión es horizontal, pero comprobar manualmente para asegurarse de que la versión más reciente.

#### <a name="install-docker-on-windows-10"></a>Instalación de Docker en Windows 10

Descargue e instale la versión más reciente de [Docker Community Edition para Windows][download-docker] para admitir las aplicaciones de Service Fabric en contenedores que usa Service Fabric Mesh.

Durante la instalación, seleccione **Use Windows containers instead of Linux containers** (Usar contenedores de Windows en lugar de contenedores de Linux) cuando se le pida.

Si Hyper-V no está habilitada en el equipo, ofrecerá instalador de Docker para habilitarla. Haga clic en **Aceptar** para hacerlo si se le solicita.

#### <a name="install-docker-on-windows-server-2016"></a>Instalación de Docker en Windows Server 2016

Si el rol de Hyper-V no está habilitado, abra PowerShell como administrador y ejecute el siguiente comando para habilitar Hyper-V y, después, reinicie el equipo. Para obtener más información, consulte [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Reinicie el equipo.

Abra PowerShell como administrador y ejecute el siguiente comando para instalar Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK y herramientas

Instale el entorno de ejecución, el SDK y las herramientas de Service Fabric Mesh en el siguiente orden.

1. Instale el [SDK de Service Fabric Mesh][download-sdkmesh] mediante el instalador de plataforma web. Con esto también se instalará el SDK y el runtime de Microsoft Azure Service Fabric.
2. Instale la [extensión de Visual Studio Service Fabric Mesh (versión preliminar)][download-tools] desde Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Compilación de un clúster

> [!IMPORTANT]
> Se **debe** ejecutar Docker antes de poder compilar un clúster.
> Compruebe que Docker se está ejecutando. Para ello, abra una ventana de terminal y ejecute `docker ps` para ver si se produce un error. Si la respuesta no indica un error, Docker se está ejecutando y ya está listo para compilar un clúster.

> [!Note]
> Si va a desarrollar en un equipo con Windows Fall Creators Update (versión 1709), solo puede usar las imágenes de Docker de la versión 1709 de Windows.
> Si va a desarrollar en un equipo con la Actualización de abril de 2018 de Windows 10 (versión 1803), puede usar las imágenes de Docker de las versiones 1709 o 1803 de Windows.

Si usa Visual Studio, puede omitir esta sección porque Visual Studio creará un clúster local para usted si no tiene uno.

Para obtener el mejor rendimiento de la depuración al crear y ejecutar una única aplicación de Service Fabric en un momento, cree un clúster de desarrollo local de nodo único. Si ejecuta varias aplicaciones a la vez, cree un clúster de cinco nodos de desarrollo local. El clúster se debe ejecutar cada vez que implementa o depura un proyecto de Service Fabric Mesh.

Después de instalar el entorno de ejecución, los SDK, las herramientas de Visual Studio y Docker, y de que Docker se ejecute, cree un clúster de desarrollo.

1. Cierre la ventana de PowerShell.
2. Abra una nueva ventana de PowerShell con privilegios elevados como administrador. Este paso es necesario para cargar los módulos de Service Fabric que instaló recientemente.
3. Ejecute el siguiente comando de PowerShell para crear un clúster de desarrollo:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Para iniciar la herramienta Administrador de clústeres locales, ejecute el siguiente comando de PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Cuando la herramienta Administrador de clústeres de servicio se esté ejecutando (aparece en la bandeja del sistema), haga clic con el botón derecho en ella y luego en **Iniciar clúster local** (Iniciar clúster local).

![Figura 1: Iniciar el clúster local](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Ya está listo para crear aplicaciones de Service Fabric Mesh.

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial [Creación de una aplicación de Azure Service Fabric](service-fabric-mesh-tutorial-create-dotnetcore.md).

Busque respuestas a [preguntas comunes y problemas conocidos](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/