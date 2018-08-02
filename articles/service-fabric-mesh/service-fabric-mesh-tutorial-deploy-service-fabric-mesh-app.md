---
title: 'Tutorial: Implementación de una aplicación de Service Fabric Mesh en Service Fabric Mesh | Microsoft Docs'
description: Obtenga información sobre cómo publicar una aplicación de Azure Service Fabric Mesh que consta de un sitio web de ASP.NET Core que se comunica con un servicio web de back-end.
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
ms.date: 07/26/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 350749161260768071afbb47b854cb2e9184bd9d
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284734"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-web-application"></a>Tutorial: Implementación de una aplicación web de Service Fabric Mesh

Este tutorial es la tercera parte de una serie y muestra cómo publicar una aplicación web de Azure Service Fabric Mesh directamente desde Visual Studio.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Publicar la aplicación en Azure
> * Comprobar el estado de implementación de la aplicación
> * Consultar todas las aplicaciones implementadas actualmente en la suscripción
> * Consultar los registros de aplicaciones
> * Limpiar los recursos utilizados por la aplicación

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación web de Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Depurar localmente la aplicación](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Publicación de la aplicación en Azure

Aprenderá a crear una aplicación de Azure Service Fabric Mesh que tiene un servicio de front-end web de ASP.NET y un servicio de back-end de ASP.NET Core Web API. A continuación, depurará la aplicación en el clúster de desarrollo local y publicará la aplicación en Azure. Cuando haya terminado, tendrá una aplicación sencilla de tareas pendientes que muestra cómo realizar una llamada entre servicios en una aplicación web de Service Fabric Mesh.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Asegúrese de que se haya [configurado el entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md), que incluye la instalación del entorno de ejecución de Service Fabric, el SDK, Docker y Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Descarga de la aplicación de ejemplo de tareas pendientes

Si no compiló la aplicación de ejemplo de tareas pendientes en la [segunda parte de esta serie de tutoriales](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md), puede descargarla. En una ventana Comandos, ejecute el comando siguiente para clonar el repositorio de la aplicación de ejemplo en la máquina local.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

La aplicación se encuentra en el directorio `src\todolistapp`.

## <a name="publish-to-azure"></a>Publicación en Azure

Para publicar el proyecto de Service Fabric Mesh a Azure, haga clic con el botón derecho en **ServiceFabricMeshApp** en Visual Studio y seleccione **Publicar...**

A continuación, verá un cuadro de diálogo **Publicación de la aplicación de Service Fabric**.

![Cuadro de diálogo de publicación de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Seleccione la cuenta y la suscripción de Azure. Elija una **ubicación**. En este artículo se utiliza **Este de EE. UU**.

En **Grupo de recursos**, seleccione **\<Crear un nuevo grupo de recursos...>**. Aparece un cuadro de diálogo donde se va a crear un nuevo grupo de recursos. En este artículo se utiliza la ubicación **Este de EE. UU.** y llama al grupo **sfmeshTutorial1RG** (si la organización tiene varias personas con la misma suscripción, elija un nombre de grupo único).  Presione **Crear** para crear el grupo de recursos y volver al cuadro de diálogo Publicar.

![Cuadro de diálogo del nuevo grupo de recursos de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

De nuevo en el cuadro de diálogo **Publicación de la aplicación de Service Fabric**, en **Azure Container Registry**, seleccione **\<Create New Container Registry...>** (Crear una instancia de Azure Container Registry...>). En el cuadro de diálogo **Crear Registro de contenedor**, utilice un nombre único para el **nombre de la instancia de Container Registry**. Especifique una **ubicación** (en este tutorial se utiliza **East US**). Seleccione el **grupo de recursos** que creó en el paso anterior en la lista desplegable, por ejemplo, **sfmeshTutorial1RG**. Establezca el **SKU** en **Básico** y, después, presione **Crear** para volver al cuadro de diálogo Publicar.

![Cuadro de diálogo del nuevo grupo de recursos de Service Fabric Mesh en Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Si obtiene un error que indica que un proveedor de recursos no se ha registrado en la suscripción, puede registrarlo. Consulte en primer lugar si el proveedor de recursos está disponible para su suscripción:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Si el proveedor del registro de contenedor (`Microsoft.ContainerRegistry`) está disponible, regístrelo desde Powershell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

En el cuadro de diálogo Publicar, presione el botón **Publicar** para implementar la aplicación de Service Fabric en Azure.

Al publicar en Azure por primera vez, la imagen del docker se inserta en Azure Container Registry (ACR), que tarda tiempo según el tamaño de la imagen. Las publicaciones posteriores del mismo proyecto serán más rápidas. Para supervisar el progreso de la implementación, seleccione el panel **Herramientas de Service Fabric** en la ventana **Salida** de Visual Studio. Una vez finalizada la implementación, la salida de **Herramientas de Service Fabric** mostrará la dirección IP y el puerto de la aplicación en forma de dirección URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Abra un explorador web y vaya a la dirección URL para ver el sitio web que se ejecuta en Azure.

## <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh 
Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para el resto de los pasos. Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).


## <a name="check-application-deployment-status"></a>Comprobar el estado de implementación de la aplicación

En este momento, se ha implementado la aplicación. También puede comprobar el estado mediante el uso del comando `app show`. 

El nombre de la aplicación para la aplicación del tutorial es `ServiceMeshApp`. Recopile los detalles sobre la aplicación con el siguiente comando:

```azurecli-interactive
az mesh app show --resource-group $rg --name ServiceMeshApp
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Consulta de todas las aplicaciones implementadas actualmente en la suscripción

Puede usar el comando "app list" para obtener una lista de aplicaciones que ha implementado en la suscripción.

```cli
az mesh app list --output table
```

## <a name="see-the-application-logs"></a>Consulta de los registros de aplicaciones

Examine los registros de la aplicación implementada:

```azurecli-interactive
az mesh code-package-log get --resource-group $rg --application-name ServiceMeshApp --service-name todoservice --replica-name 0 --code-package-name ServiceMeshApp
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine todos los recursos que haya creado. Como ha creado un nuevo grupo de recursos para hospedar los recursos tanto de ACR como del servicio Service Fabric Mesh, puede eliminar este grupo de recursos con seguridad, lo que eliminará todos los recursos asociados.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Además, puede eliminar el grupo de recursos [desde el portal](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources). 

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:
> [!div class="checklist"]
> * Publicar la aplicación en Azure
> * Comprobar el estado de implementación de la aplicación
> * Consultar toda la aplicación que ha implementado en la suscripción
> * Consultar los registros de aplicaciones
> * Limpiar los recursos utilizados por la aplicación

Ahora que ha completado la publicación de una aplicación de Service Fabric Mesh en Azure, pruebe lo siguiente:

* Explore el [ejemplo de aplicación de votación](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) para ver otro ejemplo de comunicación entre servicios.
* Lea sobre los [recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md).
* Lea sobre [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest