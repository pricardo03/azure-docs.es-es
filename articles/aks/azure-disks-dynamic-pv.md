---
title: Creación de volúmenes persistentes con Azure Kubernetes Service
description: Aprenda a usar discos de Azure para crear volúmenes persistentes de pods en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/20/2018
ms.author: iainfou
ms.openlocfilehash: 129a39294d52a312c91fc6a4fd009d76e88b4ff7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185229"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Creación de volúmenes persistentes con discos de Azure para Azure Kubernetes Service (AKS)

Un volumen persistente representa un fragmento de almacenamiento aprovisionado para su uso con pods de Kubernetes. Un volumen persistente puede usarse en uno o varios pods y puede aprovisionarse de forma dinámica o estática. Para más información sobre volúmenes persistentes de Kubernetes, vea el artículo sobre [volúmenes persistentes de Kubernetes][kubernetes-volumes]. En este artículo se muestra cómo usar volúmenes persistentes con discos de Azure en un clúster de Azure Kubernetes Service (AKS).

> [!NOTE]
> Solo se puede montar un disco de Azure con el tipo de *modo de acceso* *ReadWriteOnce*, que lo pone a disposición de un único nodo de AKS. Si necesita compartir un volumen persistente en varios nodos, considere la posibilidad de usar [Azure Files][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Clases de almacenamiento integradas

Una clase de almacenamiento se usa para definir cómo se crea dinámicamente una unidad de almacenamiento con un volumen persistente. Para más información sobre las clases de almacenamiento de Kubernetes, vea las [clases de almacenamiento de Kubernetes][kubernetes-storage-classes].

Cada clúster de AKS incluye dos clases de almacenamiento creadas previamente y configuradas ambas para funcionar con discos de Azure:

* La clase de almacenamiento *predeterminada* aprovisiona un disco de Azure estándar.
    * Standard Storage está respaldado por unidades de disco duro y ofrece un almacenamiento rentable, al tiempo que mantiene un rendimiento superior. Los discos estándar son ideales para cargas de trabajo de desarrollo y prueba rentables.
* La clase de almacenamiento *Premium administrada* aprovisiona un disco de Azure premium.
    * Los discos Premium están respaldados por un disco de latencia reducida y alto rendimiento basado en SSD. Es perfecto para máquinas virtuales que ejecutan cargas de trabajo de producción. Si los nodos de AKS del clúster usan almacenamiento Premium, seleccione la clase *administrada Premium*.

Use el comando [kubectl get sc][kubectl-get] para ver las clases de almacenamiento que se crearon previamente. El ejemplo siguiente muestra las clases de almacenamiento creadas previamente disponibles dentro de un clúster de AKS:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Las notificaciones de volumen persistente se especifican en GiB, pero los discos administrados de Azure se facturan por SKU para un tamaño específico. Estas SKU varían de 32 GiB para S4 o discos P4 a 4 TiB para S50 o discos P50. El rendimiento de transferencia de datos de red y el rendimiento IOPS de un disco administrado Premium depende de la SKU y del tamaño de instancia de los nodos en el clúster de AKS. Para más información, consulte [Pricing and Performance of Managed Disks][managed-disk-pricing-performance] (Precios y rendimiento de los discos administrados).

## <a name="create-a-persistent-volume-claim"></a>Creación de una notificación de volumen persistente

Una notificación de volumen persistente (PVC) se usa para aprovisionar automáticamente el almacenamiento en función de una clase de almacenamiento. En ese caso, una PVC puede usar una de las clases de almacenamiento que crearon previamente para crear un disco administrado de Azure estándar o premium.

Cree un archivo denominado `azure-premium.yaml` y cópielo en el siguiente código manifiesto. La notificación solicita un disco llamado `azure-managed-disk` esto es *5GB* de tamaño con acceso *ReadWriteOnce*. La clase *Premium administrada*se especifica como la clase de almacenamiento.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Para crear un disco que usa almacenamiento estándar, use `storageClassName: default` en lugar de la clase *Premium administrada*.

Cree la notificación del volumen persistente con el comando [kubectl create][kubectl-create] y especifique su archivo *azure-premium.yaml*:

```
$ kubectl create -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Uso del volumen persistente

Una vez que se haya creado la notificación de volumen persistente y tras el aprovisionamiento correcto del disco, se puede crear un pod con acceso al disco. El siguiente manifiesto crea un pod NGINX básico que utiliza la notificación de volumen persistente *azure-managed-disk* para montar el disco de Azure en la ruta de acceso `/mnt/azure`.

Cree un archivo denominado `azure-pvc-disk.yaml` y cópielo en el siguiente código manifiesto.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Cree el pod con el comando [kubectl create][kubectl-create], tal como se muestra en el ejemplo siguiente:

```
$ kubectl create -f azure-pvc-disk.yaml

pod/mypod created
```

Ahora tiene un pod en ejecución con el disco de Azure montado en el directorio `/mnt/azure`. Esta configuración puede verse al examinar el pod mediante `kubectl describe pod mypod`, tal y como se muestra en el siguiente ejemplo reducido:

```
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false

Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Realización de una copia de seguridad de un volumen persistente

Para realizar una copia de seguridad de los datos de su volumen persistente, tome una instantánea del disco administrado para el volumen. A continuación, puede usar esta instantánea para crear un disco restaurado y asociarlo a los pods como un medio para restaurar los datos.

En primer lugar, obtenga el nombre del volumen con el comando `kubectl get pvc`, como por ejemplo para la clase PVC denominada *azure-managed-disk*:

```
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

Este nombre de volumen constituye el nombre de disco de Azure subyacente. Realice una consulta para el identificador del disco con [az disk list][az-disk-list] y proporcione el nombre del volumen PVC, tal como se muestra en el ejemplo siguiente:

```
$ az disk list --query '[].id|[?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Use el identificador del disco para crear un disco de instantánea con [az snapshot create][az-snapshot-create]. El ejemplo siguiente crea una instantánea denominada *pvcSnapshot* en el mismo grupo de recursos que el clúster de AKS (*MC_myResourceGroup_myAKSCluster_eastus*). Puede tener algún problema con los permisos si crea instantáneas y restaura discos en grupos de recursos a los que el clúster de AKS no tenga acceso.

```azurecli
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Según la cantidad de datos en el disco, la instantánea puede tardar unos minutos en crearse.

## <a name="restore-and-use-a-snapshot"></a>Restauración y uso de una instantánea

Para restaurar el disco y usarlo con un pod de Kubernetes, utilice la instantánea como un origen al crear un disco con [az disk create][az-disk-create]. Esta operación conserva el recurso original si luego necesita tener acceso a la instantánea de datos original. En el ejemplo siguiente se crea un disco llamado *pvcRestored* desde la instantánea denominada *pvcSnapshot*:

```azurecli
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Para usar el disco restaurado con un pod, especifique el identificador del disco en el manifiesto. Obtenga el identificador del disco con el comando [az disk show][az-disk-show]. En el ejemplo siguiente se obtiene el identificador del disco para *pvcRestored* creado en el paso anterior:

```azurecli
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Cree un manifiesto de pod llamado `azure-restored.yaml` y especifique el URI de disco obtenido en el paso anterior. En el ejemplo siguiente se crea un servidor web NGINX básico, con el disco restaurado montado como un volumen en */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
    - name: myfrontendrestored
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Cree el pod con el comando [kubectl create][kubectl-create], tal como se muestra en el ejemplo siguiente:

```
$ kubectl create -f azure-restored.yaml

pod/mypodrestored created
```

Puede usar `kubectl describe pod mypodrestored` para ver los detalles del pod, como el siguiente ejemplo reducido que muestra la información de volumen:

```
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los volúmenes persistentes de Kubernetes con discos de Azure.

> [!div class="nextstepaction"]
> [Complemento de Kubernetes para discos de Azure][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
