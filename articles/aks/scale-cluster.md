---
title: Escalado de un clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo escalar el número de nodos en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfoulds
ms.openlocfilehash: de3f8613c93715aecf7e9e066a8ad1d82e4379e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475129"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Escalación del número de nodos en un clúster de Azure Kubernetes Service (AKS)

Si las necesidades del recurso de las aplicaciones cambian, puede escalar manualmente un clúster de AKS para ejecutar un número diferente de nodos. Durante la reducción vertical, los nodos se [acordonarán y vaciarán][kubernetes-drain] minuciosamente para minimizar las interrupciones en las aplicaciones en ejecución. Durante el escalado vertical, AKS espera hasta que el clúster de Kubernetes marca los nodos como `Ready` antes de que se programen pods en ellos.

## <a name="scale-the-cluster-nodes"></a>Escalado de los nodos de clúster

En primer lugar, obtenga el *nombre* del grupo de nodos con el comando [az aks show][az-aks-show]. En el ejemplo siguiente se obtiene el nombre del grupo de nodos del clúster denominado *myAKSCluster* en el grupo de recursos *myResourceGroup*:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

La salida de ejemplo siguiente muestra que el *nombre* es *nodepool1*:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Use el comando[az aks scale][az-aks-scale] para escalar los nodos de clúster. En el siguiente ejemplo, se escala un clúster denominado *myAKSCluster* en un único nodo. Proporcione su propio valor para *--nodepool-name* del comando anterior, como *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

La siguiente salida de ejemplo muestra que el clúster ha escalado correctamente a un nodo, como se muestra en la sección *agentPoolProfiles*:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, escaló manualmente un clúster de AKS para aumentar o disminuir el número de nodos. También puede usar [Cluster Autoscaler][cluster-autoscaler] (actualmente en la versión preliminar de AKS) para escalar automáticamente el clúster.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[cluster-autoscaler]: cluster-autoscaler.md
