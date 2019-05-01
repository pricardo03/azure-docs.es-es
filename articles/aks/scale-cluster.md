---
title: Escalado de un clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo escalar el número de nodos en un clúster de Azure Kubernetes Service (AKS).
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 01/10/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 558a3b6dc15293ab9a0895aa4f9f709ba2d0a51f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032172"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Escalación del número de nodos en un clúster de Azure Kubernetes Service (AKS)

Si las necesidades del recurso de las aplicaciones cambian, puede escalar manualmente un clúster de AKS para ejecutar un número diferente de nodos. Durante la reducción vertical, los nodos se [acordonarán y vaciarán][kubernetes-drain] minuciosamente para minimizar las interrupciones en las aplicaciones en ejecución. Durante el escalado vertical, el comando `az` espera hasta que el clúster de Kubernetes marca los nodos como `Ready`.

## <a name="scale-the-cluster-nodes"></a>Escalado de los nodos de clúster

En primer lugar, obtenga el *nombre* del grupo de nodos con el comando [az aks show][az-aks-show]. En el ejemplo siguiente se obtiene el nombre del grupo de nodos del clúster denominado *myAKSCluster* en el grupo de recursos *myResourceGroup*:

```azurecli
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

Use el comando `az aks scale` para escalar los nodos de clúster. En el siguiente ejemplo, se escala un clúster denominado *myAKSCluster* en un único nodo. Proporcione su propio valor para *--nodepool-name* del comando anterior, como *nodepool1*:

```azurecli
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
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": true,
  "fqdn": "myaksclust-myresourcegroup-19da35-0d60b16a.hcp.chinaeast2.azmk8s.io",
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.9.11",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "[...]"
        }
      ]
    }
  },
  "location": "chinaeast2",
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_chinaeast2",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "servicePrincipalProfile": {
    "clientId": "[...]",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la implementación y administración de AKS con los tutoriales de AKS.

> [!div class="nextstepaction"]
> [Tutorial de AKS][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-show