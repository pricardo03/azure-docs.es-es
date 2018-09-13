---
title: Uso de Azure Files con AKS
description: Usar de Azure Disks con AKS
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: iainfou
ms.openlocfilehash: dfc9171f54effe3da7a0f13695ab233d561357d4
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285692"
---
# <a name="persistent-volumes-with-azure-files"></a>Volúmenes persistentes con archivos de Azure

Un volumen persistente es un fragmento de almacenamiento que se ha creado para usarlo en un clúster de Kubernetes. Un volumen persistente puede usarse en uno o varios pods y puede crearse de forma dinámica o estática. En este documento se detalla la **creación dinámica** de un recurso compartido de archivos de Azure como volumen persistente.

Para obtener más información sobre volúmenes persistentes de Kubernetes, incluida la creación estática, consulte el artículo sobre [volúmenes persistentes de Kubernetes][kubernetes-volumes].

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Al realizar la creación dinámica de un recurso compartido de archivos de Azure como volumen de Kubernetes, se puede usar cualquier cuenta de almacenamiento, siempre que esté en el grupo de recursos del **nodo** de AKS. Este grupo es el que tiene el prefijo *MC_* que se creó mediante el aprovisionamiento de los recursos para el clúster de AKS. Obtenga el nombre del grupo de recursos con el comando [az aks show][az-aks-show].

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Puede utilizar el comando [az storage account create][az-storage-account-create] para crear una cuenta de almacenamiento.

Actualice `--resource-group` con el nombre del grupo de recursos recopilado en el último paso y `--name` con un nombre de su elección. Proporcione su propio nombre de cuenta de almacenamiento único:

```azurecli
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --sku Standard_LRS
```

> [!NOTE]
> Azure Files actualmente solo funcionan con almacenamiento estándar. Si usa almacenamiento Premium, el volumen no se aprovisiona.

## <a name="create-a-storage-class"></a>Creación de una clase de almacenamiento

Una clase de almacenamiento se utiliza para definir cómo se crea un recurso compartido de archivos de Azure. En la clase puede especificarse una cuenta de almacenamiento. Si no se especifica una cuenta de almacenamiento, deben especificarse *skuName* y *location*, y se evalúan en busca de alguna coincidencia todas las cuentas de almacenamiento en el grupo de recursos asociado. Para más información sobre las clases de almacenamiento de Kubernetes para Azure Files, vea las [clases de almacenamiento de Kubernetes][kubernetes-storage-classes].

Cree un archivo denominado `azure-file-sc.yaml` y cópielo en el ejemplo siguiente de manifiesto. Actualice el valor *storageAccount* con el nombre de la cuenta de almacenamiento que creó en el paso anterior. Para obtener más información sobre *mountOptions*, consulte la sección [Opciones de montaje][mount-options].

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
  storageAccount: mystorageaccount
```

Cree la clase de almacenamiento con el comando [kubectl apply][kubectl-apply]:

```console
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-a-cluster-role-and-binding"></a>Creación de un rol y un enlace de clúster

Los clústeres AKS usan el control de acceso basado en roles (RBAC) de Kubernetes para limitar las acciones que se pueden realizar. *Roles* define los permisos para conceder, y *enlaces* los aplica a los usuarios deseados. Estas asignaciones se pueden aplicar a un espacio de nombres especificado o a todo el clúster. Para más información, consulte [Uso de la autorización de RBAC][kubernetes-rbac].

Para permitir que la plataforma de Azure cree los recursos de almacenamiento necesarios, cree los elementos *ClusterRole* y *ClusterRoleBinding*. Cree un archivo denominado `azure-pvc-roles.yaml` y cópielo en el siguiente código YAML:

```yaml
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: system:azure-cloud-provider
rules:
- apiGroups: ['']
  resources: ['secrets']
  verbs:     ['get','create']
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: system:azure-cloud-provider
roleRef:
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
  name: system:azure-cloud-provider
subjects:
- kind: ServiceAccount
  name: persistent-volume-binder
  namespace: kube-system
```

Asigne los permisos con el comando [kubectl apply][kubectl-apply]:

```console
kubectl apply -f azure-pvc-roles.yaml
```

## <a name="create-a-persistent-volume-claim"></a>Creación de una notificación de volumen persistente

Una notificación de volumen persistente (PVC) usa el objeto de clase de almacenamiento para aprovisionar de forma dinámica un recurso compartido de archivos de Azure. El siguiente código de YAML puede utilizarse para crear una notificación de volumen persistente con un tamaño de *5 GB* y con acceso *ReadWriteMany*. Para obtener más información sobre los modos de acceso, consulte la documentación sobre [volúmenes persistentes de Kubernetes][access-modes].

Ahora cree un archivo denominado `azure-file-pvc.yaml` y cópielo en el siguiente código YAML. Asegúrese de que *storageClassName* coincide con la clase de almacenamiento creada en el último paso:

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

Cree la notificación del volumen persistente con el comando [kubectl apply][kubectl-apply]:

```console
kubectl apply -f azure-file-pvc.yaml
```

Una vez completado, se creará el recurso compartido de archivos. También se crea un secreto de Kubernetes que incluye las credenciales y la información de conexión. Puede usar el comando [kubectl get][kubectl-get] para ver el estado de la PVC:

```
$ kubectl get pvc azurefile

NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound     pvc-8436e62e-a0d9-11e5-8521-5a8664dc0477   5Gi        RWX            azurefile      5m
```

## <a name="use-the-persistent-volume"></a>Uso del volumen persistente

El siguiente código de YAML crea un pod que utiliza la notificación de volumen persistente *azurefile* para montar el recurso compartido de archivos de Azure en la ruta de acceso */mnt/azure*.

Cree un archivo denominado `azure-pvc-files.yaml` y cópielo en el siguiente código YAML. Asegúrese de que *claimName* coincide con la clase PVC creada en el último paso.

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

```console
kubectl apply -f azure-pvc-files.yaml
```

Ahora tiene un pod en ejecución con el disco de Azure montado en el directorio */mnt/azure*. Esta configuración puede verse al examinar el pod mediante `kubectl describe pod mypod`. La siguiente salida de ejemplo condensada muestra el volumen montado en el contenedor:

```
Containers:
  myfrontend:
    Container ID:   docker://053bc9c0df72232d755aa040bfba8b533fa696b123876108dec400e364d2523e
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:d85914d547a6c92faa39ce7058bd7529baacab7e0cd4255442b04577c4d1f424
    State:          Running
      Started:      Wed, 15 Aug 2018 22:22:27 +0000
    Ready:          True
    Mounts:
      /mnt/azure from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-8rv4z (ro)
[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azurefile2
    ReadOnly:   false
[...]
```

## <a name="mount-options"></a>Opciones de montaje

Los valores predeterminados *fileMode* y *dirMode* varían entre las distintas versiones de Kubernetes, tal y como se describe en la tabla siguiente.

| version | value |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| v1.8.6 o posterior | 0755 |
| v1.9.0 | 0700 |
| v1.9.1 o posterior | 0755 |

Si utiliza un clúster de versión 1.8.5 o superior y crea dinámicamente el volumen persistente con una clase de almacenamiento, se pueden especificar las opciones de montaje en el objeto de la clase de almacenamiento. En el ejemplo siguiente se establece *0777*:

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

Si utiliza un clúster de versión 1.8.5 o superior y crea estáticamente el objeto de volumen persistente, deben especificarse las opciones de montaje en el objeto *PersistentVolume*. Para obtener más información sobre la creación estática de un volumen persistente, consulte el artículo sobre [volúmenes persistentes estáticos][pv-static].

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

Si se usa un clúster con las versiones 1.8.0 a 1.8.4, se puede especificar un contexto de seguridad con el valor *runAsUser* definido en *0*. Para más información sobre el contexto de seguridad de pod, vea [Configure a Security Context][kubernetes-security-context] (Configuración de un contexto de seguridad).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los volúmenes persistentes de Kubernetes con Azure Files.

> [!div class="nextstepaction"]
> [Complemento de Kubernetes para Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/aks#az-aks-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
