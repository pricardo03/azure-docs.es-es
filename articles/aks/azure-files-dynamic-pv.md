---
title: Uso de Azure Files con AKS
description: Usar de Azure Disks con AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 84500791887194884e1ec7d15ddfbc169ba22517
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098352"
---
# <a name="persistent-volumes-with-azure-files"></a>Volúmenes persistentes con archivos de Azure

Un volumen persistente es un fragmento de almacenamiento que se ha creado para usarlo en un clúster de Kubernetes. Un volumen persistente puede usarse en uno o varios pods y puede crearse de forma dinámica o estática. En este documento se detalla la **creación dinámica** de un recurso compartido de archivos de Azure como volumen persistente.

Para obtener más información sobre volúmenes persistentes de Kubernetes, incluida la creación estática, consulte el artículo sobre [volúmenes persistentes de Kubernetes][kubernetes-volumes].

## <a name="create-storage-account"></a>Crear cuenta de almacenamiento

Al realizar la creación dinámica de un recurso compartido de archivos de Azure como volumen de Kubernetes, se puede usar cualquier cuenta de almacenamiento, siempre que esté en el grupo de recursos del **nodo** de AKS. Este es el que tiene el prefijo `MC_` que se creó mediante el aprovisionamiento de los recursos para el clúster de AKS. Obtenga el nombre del grupo de recursos con el comando [az resource show][az-resource-show].

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Puede utilizar el comando [az storage account create][az-storage-account-create] para crear una cuenta de almacenamiento.

Actualice `--resource-group` con el nombre del grupo de recursos recopilado en el último paso y `--name` con un nombre de su elección.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Azure Files actualmente solo funcionan con almacenamiento estándar. Si usa almacenamiento premium, el volumen no se podrá aprovisionar.

## <a name="create-storage-class"></a>Creación de la clase de almacenamiento

Una clase de almacenamiento se utiliza para definir cómo se crea un recurso compartido de archivos de Azure. En la clase puede especificarse una cuenta de almacenamiento específica. Si no se especifica una cuenta de almacenamiento, debe especificarse `skuName` y `location`, y se evalúan en busca de alguna coincidencia todas las cuentas de almacenamiento en el grupo de recursos asociado.

Para más información sobre las clases de almacenamiento de Kubernetes para los archivos Azure, vea las [clases de almacenamiento de Kubernetes][kubernetes-storage-classes].

Cree un archivo denominado `azure-file-sc.yaml` y cópielo en el siguiente código manifiesto. Actualice `storageAccount` con el nombre de la cuenta de almacenamiento de destino. Vea la sección [Opciones de montaje] para obtener más información sobre `mountOptions`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Cree la clase de almacenamiento con el comando [kubectl apply][kubectl-apply].

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Creación de una notificación de volumen persistente

Una notificación de volumen persistente (PVC) usa el objeto de clase de almacenamiento para aprovisionar de forma dinámica un recurso compartido de archivos de Azure.

El siguiente código de YAML puede utilizarse para crear una notificación de volumen persistente con un tamaño de `5GB` y con acceso `ReadWriteMany`. Para obtener más información sobre los modos de acceso, consulte la documentación sobre [volúmenes persistentes de Kubernetes][access-modes].

Cree un archivo denominado `azure-file-pvc.yaml` y cópielo en el siguiente código YAML. Asegúrese de que `storageClassName` coincide con la clase de almacenamiento creada en el último paso.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Cree la notificación del volumen persistente con el comando [kubectl apply][kubectl-apply].

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Una vez completado, se creará el recurso compartido de archivos. También se crea un secreto de Kubernetes que incluye las credenciales y la información de conexión.

## <a name="using-the-persistent-volume"></a>Uso del volumen persistente

El siguiente código de YAML crea un pod que utiliza la notificación de volumen persistente `azurefile` para montar el recurso compartido de archivos de Azure en la ruta de acceso `/mnt/azure`.

Cree un archivo denominado `azure-pvc-files.yaml` y cópielo en el siguiente código YAML. Asegúrese de que `claimName` coincide con la clase PVC creada en el último paso.

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
        claimName: azurefile
```

Cree el pod con el comando [kubectl apply][kubectl-apply].

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Ahora tiene un pod en ejecución con el disco de Azure montado en el directorio `/mnt/azure`. Esta configuración puede verse al examinar el pod mediante `kubectl describe pod mypod`.

## <a name="mount-options"></a>Opciones de montaje

Los valores predeterminados fileMode y dirMode varían entre las distintas versiones de Kubernetes, tal y como se describe en la tabla siguiente.

| version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| v1.8.6 o posterior | 0755 |
| v1.9.0 | 0700 |
| v1.9.1 o posterior | 0755 |

Si utiliza un clúster de versión 1.8.5 o superior y crea dinámicamente el volumen persistente con una clase de almacenamiento, se pueden especificar las opciones de montaje en el objeto de la clase de almacenamiento. En el ejemplo siguiente se establece `0777`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Si utiliza un clúster de versión 1.8.5 o superior y crea estáticamente el objeto de volumen persistente, deben especificarse las opciones de montaje en el objeto `PersistentVolume`. Para obtener más información sobre la creación estática de un volumen persistente, consulte el artículo sobre [volúmenes persistentes estáticos][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Si se usa un clúster con las versiones 1.8.0 a 1.8.4, se puede especificar un contexto de seguridad con el valor `runAsUser` definido en `0`. Para más información sobre el contexto de seguridad de pod, vea [Configure a Security Context][kubernetes-security-context] (Configuración de un contexto de seguridad).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los volúmenes persistentes de Kubernetes con Azure Files.

> [!div class="nextstepaction"]
> [Complemento de Kubernetes para Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[mount-options]: #mount-options
