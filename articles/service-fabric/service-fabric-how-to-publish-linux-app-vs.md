---
title: Información sobre la creación y publicación de aplicaciones .Net Core en un clúster remoto de Azure Service Fabric en Linux | Microsoft Docs
description: Creación y publicación de aplicaciones .Net Core destinadas a un clúster de Linux remoto desde Visual Studio
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/20/2019
ms.author: pepogors
ms.openlocfilehash: 46d76edbe8cede12e8c7811f43c28a65c1ebaed0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068127"
---
# <a name="use-visual-studio-to-create-and-publish-net-core-applications-targeting-a-remote-linux-service-fabric-cluster"></a>Uso de Visual Studio para crear y publicar aplicaciones .Net Core destinadas a un clúster remoto de Service Fabric en Linux
Con las herramientas de Visual Studio puede desarrollar y publicar aplicaciones .Net Core de Service Fabric destinadas a un clúster de Service Fabric en Linux. La versión del SDK debe ser 3.4 o superior para implementar una aplicación .Net Core destinada a clústeres de Service Fabric en Linux desde Visual Studio.

> [!Note]
> Visual Studio no admite la depuración de aplicaciones de Service Fabric destinadas a Linux.
>

## <a name="create-a-service-fabric-application-targeting-net-core"></a>Creación de una aplicación de Service Fabric destinada a .Net Core
1. Inicie Visual Studio como **administrador**.
2. Cree un proyecto con **Archivo->Nuevo->Proyecto**.
3. En el cuadro de diálogo **Nuevo proyecto**, elija **Nube -> Aplicación de Service Fabric**.
![create-application]
4. Asigne un nombre a la aplicación y haga clic en **Aceptar**.
5. En la página **Nuevo servicio de Service Fabric**, seleccione el tipo de servicio que le gustaría crear en la **sección .Net Core**.
![create-service]

## <a name="deploy-to-a-remote-linux-cluster"></a>Implementación en un clúster de Linux remoto
1. En el Explorador de soluciones, haga clic con el botón derecho en la aplicación y seleccione **Compilar**.
![build-application]
2. Una vez que se ha completado el proceso de compilación para la aplicación, haga clic con el botón derecho en el servicio y seleccione editar el **archivo csproj**.
![edit-csproj]
3. Cambie la propiedad UpdateServiceFabricManifestEnabled de True a **False** si el servicio es un **tipo de proyecto de actor**. Si la aplicación no tiene un servicio de actor, vaya al paso 4.
```xml
    <UpdateServiceFabricManifestEnabled>False</UpdateServiceFabricManifestEnabled>
```
> [!Note]
> Al establecer UpdateServiceFabricManifestEnabled en false, se deshabilitarán las actualizaciones para el archivo ServiceManifest.xml durante una compilación. Los cambios como agregar, quitar o cambiar el nombre del servicio no se reflejarán en el archivo ServiceManifest.xml. Si se realiza cualquier cambio, tendrá que actualizar ServiceManifest de forma manual, o bien establecer temporalmente UpdateServiceFabricManifestEnabled en true y compilar el servicio que va a actualizar el archivo ServiceManifest.xml y, después, revertirlo a false.
>

4. Actualice el valor RuntimeIndetifier de win7-x64 a la plataforma de destino en el proyecto de servicio.
```xml
    <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```
5. En ServiceManifest, actualice el programa de punto de entrada para quitar .exe. 
```xml
    <EntryPoint> 
    <ExeHost> 
        <Program>Actor1</Program> 
    </ExeHost> 
    </EntryPoint>
```
6. En el Explorador de soluciones, haga clic con el botón derecho en la aplicación y seleccione **Publicar**. Aparece el cuadro de diálogo **Publicar**.
7. En **Punto de conexión**, seleccione el punto de conexión para el clúster remoto de Service Fabric de Linux que le gustaría usar como destino.
![publish-application]

<!--Image references-->
[create-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-application-remote-linux.png
[create-service]:./media/service-fabric-how-to-vs-remote-linux-cluster/create-service-remote-linux.png
[build-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/build-application-remote-linux.png
[edit-csproj]:./media/service-fabric-how-to-vs-remote-linux-cluster/edit-csproj-remote-linux.png
[publish-application]:./media/service-fabric-how-to-vs-remote-linux-cluster/publish-remote-linux.png

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre [Introducción a Service Fabric con .Net Core](https://azure.microsoft.com/resources/samples/service-fabric-dotnet-core-getting-started/)
