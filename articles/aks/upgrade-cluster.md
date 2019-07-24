---
title: Actualización de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a actualizar clústeres de Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: dd88b5a044fe495da374178be8774f45bdd30f61
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614065"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Actualización de un clúster de Azure Kubernetes Service (AKS)

Como parte del ciclo de vida de un clúster de AKS, a menudo es preciso actualizar Kubernetes a su versión más reciente. Es importante que aplique las versiones de seguridad de Kubernetes más recientes o que realice la actualización necesaria para disfrutar de las últimas características. En este artículo se muestra cómo actualizar los componentes principales o un único grupo de nodos predeterminado en un clúster de AKS.

Para los clústeres de AKS que usan varios grupos de nodos o nodos de Windows Server (ambos actualmente en versión preliminar de AKS), consulte [Actualización de un grupo de nodos en AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Antes de empezar

Para este artículo es preciso usar la versión 2.0.65 de la CLI de Azure, o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="check-for-available-aks-cluster-upgrades"></a>Compruebe las actualizaciones disponibles del clúster de AKS

Para comprobar qué versiones de Kubernetes están disponibles para su clúster, use el comando [az aks get-upgrades][az-aks-get-upgrades]. En el ejemplo siguiente se comprueba si hay actualizaciones disponibles para el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Al actualizar un clúster de AKS, no pueden omitirse las versiones secundarias de Kubernetes. Por ejemplo, se permiten las actualizaciones entre *1.11.x* -> *1.12.x* o *1.12.x* -> *1.13.x*; sin embargo, no entre *1.11.x* -> *1.13.x*.
>
> Para actualizar de *1.11.x* -> *1.13.x*,la primera actualización sería de *1.11.x* -> *1.12.x* y, después, de *1.12.x* -> *1.13.x*.

La siguiente salida del ejemplo muestra que el clúster puede actualizarse a las versiones *1.12.7* o *1.12.8*:

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>Actualización de un clúster de AKS

Con una lista de las versiones disponibles para el clúster de AKS, use el comando [az aks upgrade][az-aks-upgrade] command to upgrade. During the upgrade process, AKS adds a new node to the cluster that runs the specified Kubernetes version, then carefully [cordon and drains][kubernetes-drain] uno de los nodos anteriores para minimizar las interrupciones en las aplicaciones en ejecución. Cuando se confirma que el nuevo nodo ejecuta pods de aplicación, el anterior se elimina. Este proceso se repite hasta que se hayan actualizado todos los nodos del clúster.

En el ejemplo siguientes se actualiza un clúster a la versión *1.12.8*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

Dicha actualización tarda varios minutos. El tiempo exacto dependerá del número de nodos que tenga.

Para confirmar que la actualización se ha realizado correctamente, use el comando [az aks show][az-aks-show]:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

La salida del ejemplo siguiente muestra que el clúster ahora ejecuta la versión *1.12.8*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.12.8               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se ha mostrado cómo actualizar un clúster de AKS existente. Para más información acerca de la implementación y administración de clústeres de AKS, consulte el conjunto de tutoriales.

> [!div class="nextstepaction"]
> [Tutoriales de AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
