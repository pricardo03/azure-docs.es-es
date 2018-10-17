---
title: 'Tutorial de Kubernetes en Azure: Actualización de un clúster'
description: En este tutorial de Azure Kubernetes Service (AKS) aprenderá a actualizar un clúster de AKS existente a la versión de Kubernetes más reciente disponible.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c0710be11b95b66d16661b5aff9cbf739ccda92
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901953"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Actualización de Kubernetes en Azure Kubernetes Service (AKS)

Como parte del ciclo de vida de la aplicación y del clúster, es posible que desee actualizar su versión de Kubernetes a la más reciente y usar las nuevas características. Un clúster de Azure Kubernetes Service (AKS) se puede actualizar mediante la CLI de Azure. Para minimizar las interrupciones en las aplicaciones en ejecución, los nodos de Kubernetes se [acordonan y vacían][kubernetes-drain] minuciosamente durante el proceso de actualización.

En este tutorial, la séptima parte de siete, se actualiza un clúster de Kubernetes. Aprenderá a:

> [!div class="checklist"]
> * Identificación de las versiones de Kubernetes actuales y disponibles
> * Actualización de los nodos de Kubernetes
> * Validación de una actualización correcta

## <a name="before-you-begin"></a>Antes de empezar

En tutoriales anteriores se ha empaquetado una aplicación en una imagen de contenedor, esta imagen se ha cargado en Azure Container Registry y se ha creado un clúster de Kubernetes. La aplicación se ejecutó después en el clúster de Kubernetes. Si no ha realizado estos pasos y quiere continuar, vuelva al [tutorial 1: Creación de imágenes de contenedor][aks-tutorial-prepare-app].

Para realizar este tutorial es necesario disponer de la versión 2.0.44, o superior, de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Obtención de las versiones disponibles del clúster

Antes de actualizar un clúster, use el comando [az aks get-upgrades][] para comprobar qué versiones de Kubernetes están disponibles para la actualización:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

En el siguiente ejemplo, la versión actual es la *1.9.6* y las versiones disponibles se muestran en la columna *Upgrades* (Actualizaciones).

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------
default  myResourceGroup  1.9.9            1.9.9              1.10.3, 1.10.5, 1.10.6
```

## <a name="upgrade-a-cluster"></a>Actualización de un clúster

Use el comando [az aks upgrade][] para actualizar el clúster de AKS. En el ejemplo siguientes se actualiza el clúster a la versión *1.10.6* de Kubernetes.

> [!NOTE]
> No se pueden actualizar varias versiones secundarias al mismo tiempo. Por ejemplo, puede actualizar la versión *1.9.6* a la *1.10.3*, pero no la *1.9.6* directamente a la *1.11.x*. Para actualizar de la versión *1.9.6* a la *1.11.x*, primero actualice de la *1.9.6* a la *1.10.3* y, después, realizar otra actualización de la versión  *1.10.3* a la *1.11.x*.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.6
```

La siguiente salida de ejemplo condensada muestra *kubernetesVersion*, que ahora es *1.10.6*:

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
  "kubernetesVersion": "1.10.6",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Validación de una actualización

Confirme que la actualización se realizó correctamente con el comando [az aks show][], tal como se muestra a continuación:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

La salida del ejemplo siguiente muestra que el clúster de AKS ejecuta *KubernetesVersion 1.10.6*:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.6               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Eliminación del clúster

Como esta es la última parte de la serie de tutoriales, puede que desee eliminar el clúster de AKS. Mientras los nodos de Kubernetes se ejecutan en Azure Virtual Machines (VM), seguirán acumulando cargos, aunque no se use el clúster. Use el comando [az group delete][az-group-delete] para quitar el grupo de recursos, el servicio de contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete].

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, actualizó Kubernetes en un clúster de AKS. Ha aprendido a:

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
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
