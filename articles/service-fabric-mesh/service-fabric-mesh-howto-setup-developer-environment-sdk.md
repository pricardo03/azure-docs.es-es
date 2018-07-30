---
title: Configuración de un entorno de desarrollo de Windows para aplicaciones de Service Fabric Mesh | Microsoft Docs
description: Configure el entorno de desarrollo de Windows para que pueda crear una aplicación de Service Fabric Mesh e implementarla en Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/20/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 589bef1894a3bee1e6974a0ea2516200fae2891f
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185550"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Configuración del entorno de desarrollo de Windows para compilar aplicaciones de Service Fabric

Para compilar y ejecutar aplicaciones de Azure Service Fabric en la máquina de desarrollo Windows, debe instalar el entorno en tiempo de ejecución de Service Fabric, el SDK y las herramientas.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Versiones de sistemas operativos compatibles

Se admiten las siguientes versiones de sistemas operativos para desarrollo:

* Windows 10 (Enterprise, Professional o Education)
* Windows Server 2016

## <a name="visual-studio"></a>Visual Studio

Se necesita Visual Studio 2017 para implementar aplicaciones de Service Fabric. [Instale la versión 15.6.0][download-visual-studio], u otra posterior, y habilite las cargas de trabajo siguientes:

- ASP.NET y desarrollo web
- Desarrollo de Azure
 
## <a name="windows-10---install-docker"></a>Windows 10: Instalación de Docker

Descargue e instale la versión más reciente de [Docker Community Edition para Windows][download-docker] para admitir las aplicaciones de Service Fabric en contenedores que usa Service Fabric Mesh.

Durante la instalación, seleccione **Use Windows containers instead of Linux containers** (Usar contenedores de Windows en lugar de contenedores de Linux) cuando se le pida. Si Hyper-V no está habilitado en la máquina, la instalación de Docker se ofrecerá para habilitarlo. Haga clic en **Aceptar** para hacerlo si se le solicita.

## <a name="windows-server-2016---install-hyper-v-and-docker"></a>Windows Server 2016: Instalación de Hyper-V y Docker

**Instalación de Hyper-V**

En primer lugar, abra PowerShell como administrador y ejecute el siguiente comando para instalar Hyper-V y, después, reinicie el equipo. Para más información, consulte [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

**Instalación de Docker**

Abra PowerShell como administrador y ejecute el siguiente comando para instalar Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Reinicie el equipo.

## <a name="sdk-and-tools"></a>SDK y herramientas

Instale el entorno de ejecución, el SDK y las herramientas de Service Fabric Mesh en el siguiente orden.

1. Instale el [SDK de Service Fabric Mesh][download-sdkmesh] mediante el instalador de plataforma web. Con esto también se instalará el SDK y el runtime de Microsoft Azure Service Fabric.
2. Instale la [extensión de Herramientas de Microsoft Azure Service Fabric para Visual Studio (versión preliminar)][download-tools] desde Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Compilación de un clúster

Si va a usar Visual Studio, puede omitir esta sección ya que Visual Studio creará un clúster local en su lugar si no tiene uno.

Para obtener el mejor rendimiento de depuración al crear y ejecutar aplicaciones de Service Fabric, se recomienda crear un clúster de desarrollo local de nodo único. Este clúster se debe ejecutar cada vez que implementa o depura un proyecto de Service Fabric Mesh.

Se **debe** ejecutar Docker antes de poder compilar un clúster. Compruebe que Docker se está ejecutando. Para ello, abra una ventana de terminal y ejecute `docker ps` para ver si se produce un error. Si la respuesta no indica un error, Docker se está ejecutando y ya está listo para compilar un clúster.

Después de instalar el runtime, los SDK y las herramientas de Visual Studio, cree un clúster de desarrollo.

1. Cierre la ventana de PowerShell.
2. Abra una nueva ventana de PowerShell con privilegios elevados como administrador. Este paso es necesario para cargar los módulos de Service Fabric que instaló recientemente.
3. Ejecute el siguiente comando de PowerShell para crear un clúster de desarrollo:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster -UseMachineName
    ```

4. Para iniciar la herramienta Administrador de clústeres locales, ejecute el siguiente comando de PowerShell:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```

Ya está listo para crear aplicaciones de Service Fabric Mesh.

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial [Creación de una aplicación de Azure Service Fabric](service-fabric-mesh-tutorial-create-dotnetcore.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.ServiceFabricMesh
[download-visual-studio]: https://www.visualstudio.com/downloads/