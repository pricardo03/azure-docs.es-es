---
title: Creación de volúmenes persistentes con Azure Kubernetes Service
description: Aprenda a usar discos de Azure para crear volúmenes persistentes de pods en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/10/2018
ms.author: iainfou
ms.openlocfilehash: 14617b57f59c068aa015c9bfea9b4d18520b4152
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38473689"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Creación de volúmenes persistentes con discos de Azure para Azure Kubernetes Service (AKS)

Un volumen persistente representa un fragmento de almacenamiento aprovisionado para su uso con pods de Kubernetes. Un volumen persistente puede usarse en uno o varios pods y puede aprovisionarse de forma dinámica o estática. Para más información sobre volúmenes persistentes de Kubernetes, vea el artículo sobre [volúmenes persistentes de Kubernetes][kubernetes-volumes]. En este artículo se muestra cómo usar volúmenes persistentes con discos de Azure en un clúster de Azure Kubernetes Service (AKS).

> [!NOTE]
> Solo se puede montar un disco de Azure con el tipo de *modo de acceso* *ReadWriteOnce*, que lo pone a disposición de un único nodo de AKS. Si necesita compartir un volumen persistente en varios nodos, considere la posibilidad de usar [Azure Files][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Clases de almacenamiento integradas

Una clase de almacenamiento se usa para definir cómo se crea dinámicamente una unidad de almacenamiento con un volumen persistente. Para más información sobre las clases de almacenamiento de Kubernetes, vea las [clases de almacenamiento de Kubernetes][kubernetes-storage-classes].

Cada clúster de AKS incluye dos clases de almacenamiento creadas previamente y configuradas para funcionar con discos de Azure. La clase de almacenamiento *predeterminada* aprovisiona un disco de Azure estándar. La clase de almacenamiento *Premium administrada* aprovisiona un disco de Azure premium. Si los nodos de AKS del clúster usan almacenamiento Premium, seleccione la clase *administrada Premium*.

Use el comando [kubectl get sc][kubectl-get] para ver las clases de almacenamiento que se crearon previamente. El ejemplo siguiente muestra las clases de almacenamiento creadas previamente disponibles dentro de un clúster de AKS:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Las notificaciones de volumen persistente se especifican en GiB, pero los discos administrados de Azure se facturan por SKU para un tamaño específico. Estas SKU varían de 32 GiB para S4 o discos P4 a 4 TiB para S50 o discos P50. Además, el rendimiento y el rendimiento de IOPS de un disco administrado Premium depende tanto de la SKU como del tamaño de instancia de los nodos en el clúster de AKS. Para más información, consulte [Pricing and Performance of Managed Disks][managed-disk-pricing-performance] (Precios y rendimiento de los discos administrados).

## <a name="create-a-persistent-volume-claim"></a>Creación de una notificación de volumen persistente

Una notificación de volumen persistente (PVC) se usa para aprovisionar automáticamente el almacenamiento en función de una clase de almacenamiento. En ese caso, una PVC puede usar una de las clases de almacenamiento que crearon previamente para crear un disco administrado de Azure estándar o premium.

Cree un archivo denominado `azure-premium.yaml` y cópielo en el siguiente código manifiesto.

Tenga en cuenta que la clase de almacenamiento  *administrada Premium* se especifica en la anotación y que la notificación solicita un disco con *5 GB* de tamaño y acceso *ReadWriteOnce*.

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

Cree la notificación del volumen persistente con el comando [kubectl apply][kubectl-apply] y especifique su archivo *azure-premium.yaml*:

```console
kubectl apply -f azure-premium.yaml
```

## <a name="use-the-persistent-volume"></a>Uso del volumen persistente

Una vez que se haya creado la notificación de volumen persistente y tras el aprovisionamiento correcto del almacenamiento, se puede crear un pod con acceso al disco. El siguiente manifiesto crea un pod que utiliza la notificación de volumen persistente *azure-managed-disk* para montar el disco de Azure en la ruta de acceso `/mnt/azure`.

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

Cree el pod con el comando [kubectl apply][kubectl-apply].

```console
kubectl apply -f azure-pvc-disk.yaml
```

Ahora tiene un pod en ejecución con el disco de Azure montado en el directorio `/mnt/azure`. Esta configuración puede verse al examinar el pod mediante `kubectl describe pod mypod`.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los volúmenes persistentes de Kubernetes con discos de Azure.

> [!div class="nextstepaction"]
> [Complemento de Kubernetes para discos de Azure][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
