---
title: Actualización de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a actualizar clústeres de Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: 2cadd4b33cb52307599ce1e83eee8370ef9850fe
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692785"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Actualización de un clúster de Azure Kubernetes Service (AKS)

Como parte del ciclo de vida de un clúster de AKS, a menudo es preciso actualizar Kubernetes a su versión más reciente. Es importante que aplique las versiones de seguridad de Kubernetes más recientes o que realice la actualización necesaria para disfrutar de las últimas características. Este artículo muestra cómo actualizar los componentes principales o un único grupo de nodos de forma predeterminada en un clúster de AKS.

Para AKS clústeres que usan varios grupos de nodos o nodos de Windows Server (ambas actualmente en versión preliminar de AKS), consulte [actualizar un grupo de nodos de AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Antes de empezar

Este artículo es preciso que ejecute la CLI de Azure versión 2.0.65 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="check-for-available-aks-cluster-upgrades"></a>Compruebe las actualizaciones disponibles del clúster de AKS

Para comprobar qué versiones de Kubernetes están disponibles para su clúster, use el comando [az aks get-upgrades][az-aks-get-upgrades]. En el ejemplo siguiente se comprueba si hay actualizaciones disponibles para el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Al actualizar un clúster de AKS, no pueden omitirse las versiones secundarias de Kubernetes. Por ejemplo, se actualiza entre *1.11.x* -> *1.12.x* o *1.12.x* -> *1.13.x* se permiten, sin embargo *1.11.x* -> *1.13.x* no es.
>
> Para actualizar, desde *1.11.x* -> *1.13.x*, la primera actualización de *1.11.x* -> *1.12.x*, a continuación, actualice desde *1.12.x* -> *1.13.x*.

La salida del ejemplo siguiente se muestra que el clúster puede actualizarse a la versión *1.12.7* o *1.12.8*:

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>Actualización de un clúster de AKS

Con una lista de las versiones disponibles para el clúster de AKS, use el comando [az aks upgrade][az-aks-upgrade] para realizar la actualización. Durante el proceso de actualización, AKS agrega un nuevo nodo al clúster que ejecuta la versión especificada de Kubernetes, a continuación, cuidadosamente [acordonamiento y avanza] [ kubernetes-drain] uno de los nodos antiguos para minimizar las interrupciones a en ejecución aplicaciones. Cuando el nodo nuevo se confirma como si se ejecutaran los pods de aplicación, se elimina el nodo antiguo. Este proceso se repite hasta que se hayan actualizado todos los nodos del clúster.

En el ejemplo siguiente se actualiza un clúster a la versión *1.12.8*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

Dicha actualización tarda varios minutos. El tiempo exacto dependerá del número de nodos que tenga.

Para confirmar que la actualización se ha realizado correctamente, use el comando [az aks show][az-aks-show]:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

La salida del ejemplo siguiente se muestra que el clúster se ejecuta ahora *1.12.8*:

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
