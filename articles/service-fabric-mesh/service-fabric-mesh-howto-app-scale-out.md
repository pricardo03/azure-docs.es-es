---
title: Escalar servicios en una aplicación de Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información sobre cómo escalar servicios de forma independiente en una aplicación que se ejecute en Service Fabric Mesh mediante la CLI de Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/08/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: e68bcd135c33c7fd83908b8fed0fd098a698fd36
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038592"
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

Cree un grupo de recursos en el que implementar la aplicación.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Implemente la aplicación con un servicio de trabajo.

Cree su aplicación en el grupo de recursos mediante el comando `deployment create`.

En el siguiente ejemplo se implementa una aplicación Linux con la [plantilla mesh_rp.base.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Para implementar una aplicación Windows, use la [[mesh_rp.base.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

Pasados unos minutos, el comando debe devolver lo siguiente:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abrir la aplicación

El comando de implementación devolverá la dirección IP pública del punto de conexión de servicio. Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala desde un navegador. Mostrará una página web con un triángulo en movimiento.

El nombre de recurso de red para esta aplicación es `visualObjectsNetwork`. Puede ver información sobre la aplicación, como su descripción, ubicación, grupo de recursos, etc. mediante el comando siguiente:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Escalar el servicio `worker`

Escale el servicio `worker` a tres instancias mediante el comando siguiente. En el siguiente ejemplo se implementa una aplicación Linux con la [plantilla mesh_rp.scaleout.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Para implementar una aplicación Windows, use la [plantilla mesh_rp.scaleout.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Tenga en cuenta que las imágenes de contenedor más grandes pueden tardar más en implementarse.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```

Una vez que la aplicación esté implementada correctamente, el navegador debería mostrar una página web con tres triángulos en movimiento.

## <a name="delete-the-resources"></a>Eliminación de los recursos

Elimine periódicamente los recursos que ya no se use en Azure. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron (lo que elimina todo el contenido asociado con el grupo de recursos) con el siguiente comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Échele un vistazo a la aplicación de ejemplo Visual Objects en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).