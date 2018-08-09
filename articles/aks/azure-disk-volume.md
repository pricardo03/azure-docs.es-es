---
title: Usar de Azure Disks con AKS
description: Usar de Azure Disks con AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: aa9b92df84a48ef4cb706e9e89e0f6c0a25cd42a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420499"
---
# <a name="volumes-with-azure-disks"></a>Volúmenes con discos de Azure

Las aplicaciones que usan contenedores a menudo necesitan acceder a un volumen de datos externo y conservar datos en él. Los discos de Azure se pueden usar como este almacén de datos externos. En este artículo se describe el uso de un disco de Azure como un volumen de Kubernetes en un clúster de Azure Kubernetes Service (AKS).

Para más información sobre volúmenes de Kubernetes, consulte el artículo sobre [volúmenes de Kubernetes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Creación de un disco de Azure

Antes de montar un disco administrado de Azure como un volumen de Kubernetes, el disco debe existir en el grupo de recursos del **nodo** de AKS. Obtenga el nombre del grupo de recursos con el comando [az resource show][az-resource-show].

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Use el comando [az disk create][az-disk-create] para crear el disco de Azure.

Actualice `--resource-group` con el nombre del grupo de recursos recopilado en el último paso y `--name` con un nombre de su elección.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Cuando haya creado el disco, verá un resultado similar al siguiente. Este valor es el identificador del disco, que se usa al montar el disco.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```
> [!NOTE]
> Los discos administrados de Azure se facturan por SKU de un tamaño específico. Estas SKU varían de 32 GiB para S4 o discos P4 a 4 TiB para S50 o discos P50. Además, el rendimiento y el rendimiento IOPS de un disco administrado Premium depende de la SKU y del tamaño de instancia de los nodos en el clúster de AKS. Consulte [Pricing and Performance of Managed Disks][managed-disk-pricing-performance] (Precios y rendimiento de los discos administrados).

> [!NOTE]
> Si tiene que crear el disco en un grupo de recursos independiente, también deberá agregar la entidad de servicio de Azure Kubernetes Service (AKS) del clúster en el grupo de recursos que contiene el disco con el rol `Contributor`. 
>

## <a name="mount-disk-as-volume"></a>Montaje del disco como un volumen

Monte el disco de Azure en su pod mediante la configuración del volumen en las especificaciones del contenedor.

Cree un nuevo archivo denominado `azure-disk-pod.yaml` con el contenido siguiente. Actualice `diskName` con el nombre del disco recién creado y `diskURI` con el identificador del disco. Además, anote el valor de `mountPath`, que es la ruta de acceso en la que se monta el disco de Azure en el pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-disk-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
      - name: azure
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Use kubectl para crear el pod.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

Ahora tiene un pod en ejecución con un disco de Azure montado en `/mnt/azure`.

## <a name="next-steps"></a>Pasos siguientes

Aprenda más sobre los volúmenes de Kubernetes con discos de Azure.

> [!div class="nextstepaction"]
> [Complemento de Kubernetes para discos de Azure][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
