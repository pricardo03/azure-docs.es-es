---
title: Configuración de un entorno de desarrollo de Windows para aplicaciones de Service Fabric Mesh | Microsoft Docs
description: Configure el entorno de desarrollo de Windows para que pueda crear una aplicación de Service Fabric Mesh e implementarla en Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 96549696013a2dd94741090a0a017b57a3b1e19e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125168"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-applications"></a>Configuración del entorno de desarrollo de Windows para compilar aplicaciones de Service Fabric

Para compilar y ejecutar aplicaciones de Azure Service Fabric en la máquina de desarrollo Windows, debe instalar el entorno en tiempo de ejecución de Service Fabric, el SDK y las herramientas.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="supported-operating-system-versions"></a>Versiones de sistemas operativos compatibles

Se admiten las siguientes versiones de sistemas operativos para desarrollo:

* Windows 10 (Enterprise, Professional o Education)
* Windows Server 2016

## <a name="enable-hyper-v"></a>Habilitar Hyper-V

Debe habilitar Hyper-V para que pueda crear aplicaciones de Service Fabric. 

### <a name="windows-10"></a>Windows 10

Abra PowerShell como administrador y ejecute el siguiente comando:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

Reinicie el equipo. Para más información sobre cómo habilitar Hyper-V, consulte [Instalación de Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### <a name="windows-server-2016"></a>Windows Server 2016

Abra PowerShell como administrador y ejecute el siguiente comando:

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Reinicie el equipo. Para más información sobre cómo habilitar Hyper-V, consulte [Instalación del rol de Hyper-V en Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server).

## <a name="visual-studio"></a>Visual Studio

Se necesita Visual Studio 2017 para implementar aplicaciones de Service Fabric. [Instale la versión 15.6.0][download-visual-studio], u otra posterior, y habilite las cargas de trabajo siguientes:

- ASP.NET y desarrollo web
- Desarrollo de Azure

## <a name="docker"></a>Docker

Instale Docker para admitir las aplicaciones de Service Fabric en contenedores que usa Service Fabric Mesh.

### <a name="windows-10"></a>Windows 10

Descargue e instale la versión más reciente de [Docker Community Edition para Windows][download-docker]. 

Durante la instalación, seleccione **Use Windows containers instead of Linux containers** (Usar contenedores de Windows en lugar de contenedores de Linux) cuando se le pida. A continuación, cierre la sesión e iníciela de nuevo. Después de iniciar sesión de nuevo, si no habilitó anteriormente Hyper-V, puede que se le pida hacerlo ahora. Debe habilitar Hyper-V y, a continuación, reiniciar el equipo.

Una vez reiniciado el equipo, Docker le solicitará que habilite la característica **Containers**. Hágalo y, a continuación, reinicie el equipo.

### <a name="windows-server-2016"></a>Windows Server 2016

Utilice los siguientes comandos de PowerShell para instalar Docker. Para más información, consulte [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Reinicie el equipo.

## <a name="sdk-and-tools"></a>SDK y herramientas

Instale el runtime, el SDK y las herramientas de Service Fabric Mesh en un orden dependiente.

1. Instale el [SDK de Service Fabric Mesh][download-sdkmesh] mediante el instalador de plataforma web. Con esto también se instalará el SDK y el runtime de Microsoft Azure Service Fabric.
2. Instale la [extensión de Herramientas de Microsoft Azure Service Fabric para Visual Studio (versión preliminar)][download-tools] desde Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Compilación de un clúster

Para obtener el mejor rendimiento de depuración al crear y ejecutar aplicaciones de Service Fabric, se recomienda crear un clúster de desarrollo local de nodo único. Este clúster se debe ejecutar cada vez que implementa o depura un proyecto de Service Fabric Mesh.

Se **debe** ejecutar Docker antes de poder compilar un clúster. Compruebe que Docker se está ejecutando. Para ello, abra una ventana de terminal y ejecute `docker ps` para ver si se produce un error. Si la respuesta no indica un error, Docker se está ejecutando y ya está listo para compilar un clúster.

Después de instalar el runtime, los SDK y las herramientas de Visual Studio, cree un clúster de desarrollo.

1. Cierre la ventana de PowerShell.
2. Abra una nueva ventana de PowerShell con privilegios elevados como administrador. Este paso es necesario para cargar los módulos de Service Fabric que instaló.
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
