---
title: 'Tutorial de Kubernetes en Azure: actualización del clúster'
description: 'Tutorial de Kubernetes en Azure: actualización del clúster'
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d66197b69a0804a49fabb72e9b97c77e000bdf88
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131650"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Actualización de Kubernetes en Azure Kubernetes Service (AKS)

Un clúster de Azure Kubernetes Service (AKS) se puede actualizar mediante la CLI de Azure. Para minimizar las interrupciones en las aplicaciones en ejecución, los nodos de Kubernetes se [acordonan y vacían][kubernetes-drain] minuciosamente durante el proceso de actualización.

En este tutorial, la séptima parte de siete, se actualiza un clúster de Kubernetes. Las tareas que debe completar incluyen las siguientes:

> [!div class="checklist"]
> * Identificación de las versiones de Kubernetes actuales y disponibles
> * Actualización de los nodos de Kubernetes
> * Validación de una actualización correcta

## <a name="before-you-begin"></a>Antes de empezar

En tutoriales anteriores se ha empaquetado una aplicación en una imagen de contenedor, esta imagen se ha cargado en Azure Container Registry y se ha creado un clúster de Kubernetes. La aplicación se ejecutó después en el clúster de Kubernetes.

Si no ha realizado estos pasos y quiere continuar, vuelva al [tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

## <a name="get-cluster-versions"></a>Obtención de versiones del clúster

Antes de actualizar un clúster, use el comando [az aks get-upgrades][] para comprobar qué versiones de Kubernetes están disponibles para la actualización.

```azurecli
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

En este ejemplo, la versión del nodo actual es *1.9.6* y las versiones disponibles se muestran en la columna *Actualizaciones*.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------
default  myResourceGroup  1.9.6            1.9.6              1.10.3
```

## <a name="upgrade-cluster"></a>Actualización del clúster

Use el comando [az aks upgrade][] para actualizar los nodos de clúster. En el ejemplo siguientes se actualiza el clúster a la versión *1.10.3*.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.10.3
```

El siguiente ejemplo reducido de salida muestra que *kubernetesVersion* ahora informa de *1.10.3*:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.3",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-upgrade"></a>Validación de la actualización

Confirme que la actualización se realizó correctamente con el comando [az aks show][].

```azurecli
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Resultados:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.3               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, actualizó Kubernetes en un clúster de AKS. Se completaron las siguientes tareas:

> [!div class="checklist"]
> * Identificación de las versiones de Kubernetes actuales y disponibles
> * Actualización de los nodos de Kubernetes
> * Validación de una actualización correcta

Siga este vínculo para conocer más acerca de AKS.

> [!div class="nextstepaction"]
> [Introducción a AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade