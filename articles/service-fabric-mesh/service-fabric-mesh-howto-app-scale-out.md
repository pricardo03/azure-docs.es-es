---
title: Escalar servicios en una aplicación de Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información sobre cómo escalar servicios de forma independiente en una aplicación que se ejecute en Service Fabric Mesh mediante la CLI de Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: a4260fd808643971036ad87c01bd2fdec299ccc6
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089750"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Escalar servicios en una aplicación que se ejecute en Service Fabric Mesh

En este artículo se muestra cómo escalar microservicios de forma independiente en una aplicación. En este ejemplo, la aplicación Visual Objects consta de dos microservicios: `web` y `worker`. 

El servicio `web` es una aplicación de ASP.NET Core con una página web en la que se muestran triángulos en el explorador. El explorador muestra un triángulo para cada instancia del servicio `worker`. 

El servicio `worker` mueve el triángulo a intervalos predefinidos en el espacio, y envía la ubicación del triángulo al servicio `web`. Utiliza un DNS para resolver la dirección del servicio `web`.

## <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh 
Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta tarea. Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure y configure la suscripción.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos
Cree un grupo de recursos en el que implementar la aplicación. Puede usar un grupo de recursos existente y omitir este paso. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Implemente la aplicación con un servicio de trabajo.
Cree su aplicación en el grupo de recursos mediante el comando `deployment create`.

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
El comando anterior implementa una aplicación Linux con la [plantilla mesh_rp.base.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Si quiere implementar una aplicación Windows, use la [plantilla mesh_rp.base.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

Pasados unos minutos, el comando debe devolver lo siguiente:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abrir la aplicación
Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala en un navegador. Muestra una página web con un triángulo que se mueve por el espacio.

El comando de implementación devuelve la dirección IP pública del punto de conexión de servicio. Si lo desea, también puede consultar el recurso de red para buscar la dirección IP pública del punto de conexión de servicio. 
 
El nombre de los recursos de red de esta aplicación es `visualObjectsNetwork`; puede obtener información sobre él mediante el siguiente comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Escalar el servicio `worker`

Escale el servicio `worker` a tres instancias mediante el comando siguiente. 

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
El comando anterior implementa una aplicación Linux con la [plantilla mesh_rp.scaleout.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Si quiere implementar una aplicación Windows, use la [plantilla mesh_rp.scaleout.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

Una vez que la aplicación esté implementada correctamente, el explorador debería mostrar una página web con tres triángulos que se mueven por el espacio.

## <a name="delete-the-resources"></a>Eliminación de los recursos

Para conservar los recursos limitados que se asignaron al programa de vista previa, le recomendamos que elimine los recursos con frecuencia. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Pasos siguientes
- Échele un vistazo a la aplicación de ejemplo Visual Objects en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).