---
title: Implementar una aplicación en Azure Service Fabric Mesh mediante una plantilla | Microsoft Docs
description: Obtenga información sobre cómo implementar una aplicación de .NET Core en Service Fabric Mesh a partir de una plantilla mediante la CLI de Azure.
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
ms.date: 07/12/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 356e8019f9a4a64cb1c1c113d0f44990aa4e0f95
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038560"
---
# <a name="deploy-a-service-fabric-mesh-application-to-service-fabric-mesh-using-a-template"></a>Implementar una aplicación de Service Fabric Mesh en Service Fabric Mesh mediante una plantilla
En este artículo se muestra cómo implementar una aplicación de .NET Core en Service Fabric Mesh mediante una plantilla. Cuando termine, tendrá una aplicación de votación con un front-end web de ASP.NET Core que guarda los resultados de la votación en un servicio back-end en el clúster. El front-end usa un DNS para resolver la dirección del servicio de back-end.

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

## <a name="deploy-the-application"></a>Implementación de la aplicación
Cree su aplicación en el grupo de recursos mediante el comando `deployment create`.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

El comando anterior implementa una aplicación Windows con la [plantilla mesh_rp.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json). Si quiere implementar una aplicación Linux, use la [plantilla mesh_rp.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.linux.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

Pasados unos minutos, el comando debe devolver lo siguiente:

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>` 

## <a name="open-the-application"></a>Abrir la aplicación
Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala en un navegador. Aparece la siguiente página web. 

![Aplicación de votación](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

Ahora puede agregar opciones de votación a la aplicación y votar con ella, o eliminar las opciones de voto.

El comando de implementación devuelve la dirección IP pública del punto de conexión de servicio. Si lo desea, también puede consultar el recurso de red para buscar la dirección IP pública del punto de conexión de servicio. 

El nombre de los recursos de red de esta aplicación es `VotingAppNetwork`; puede obtener información sobre él mediante el siguiente comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
```

## <a name="check-the-application-details"></a>Comprobar los detalles de la aplicación
Puede comprobar el estado de la aplicación mediante el comando `app show`. El nombre de aplicación para la aplicación implementada es "VotingApp", por lo que puede obtener sus detalles. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

## <a name="list-the-deployed-applications"></a>Enumerar las aplicaciones implementadas
Puede usar el comando "app list" para obtener una lista de aplicaciones que ha implementado en la suscripción. 

```azurecli-interactive
az mesh app list -o table
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no necesite la aplicación y sus recursos relacionados, elimine el grupo de recursos que los contiene. 

```azurecli-interactive
az group delete --resource-group myResourceGroup  
``` 

## <a name="next-steps"></a>Pasos siguientes
- Compruebe la aplicación de ejemplo para realizar votaciones en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).


