---
title: Creación de un volumen estático para varios pods en Azure Kubernetes Service (AKS)
description: Aprenda a crear manualmente un volumen con Azure Files para usarlo con varios pods simultáneos en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: 65e94a271fc8fc72ac74d51af3cf7b717f8410b0
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65072076"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>Creación manual y uso de un volumen con un recurso compartido de Azure Files en Azure Kubernetes Service (AKS)

Las aplicaciones que usan contenedores a menudo necesitan acceder a un volumen de datos externo y conservar datos en él. Si varios pods necesitan acceso simultáneo al mismo volumen de almacenamiento, puede usar Azure Files para conectarse mediante el [protocolo Bloque de mensajes del servidor (SMB)][smb-overview]. Este artículo muestra cómo crear manualmente un recurso compartido de Azure Files y asociarlo a un pod en AKS.

Para obtener más información sobre los volúmenes de Kubernetes, consulte [opciones de almacenamiento para las aplicaciones en AKS][concepts-storage].

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, vea la guía de inicio rápido AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También necesita la CLI de Azure versión 2.0.59 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Creación de un recurso compartido de archivos de Azure

Para poder utilizar Azure Files como volumen de Kubernetes, es preciso crear una cuenta de Azure Storage y el recurso compartido de archivos. Los siguientes comandos crean un grupo de recursos denominado *myAKSShare*, una cuenta de almacenamiento y un recurso compartido de archivos denominado *aksshare*:

```azurecli-interactive
# Change these four parameters as needed for your own environment
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create a resource group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create a storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $AZURE_STORAGE_CONNECTION_STRING

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Tome nota del nombre de la cuenta de almacenamiento y la clave que se muestran al final de la salida del script. Estos valores son necesarios cuando se crea el volumen de Kubernetes en uno de los pasos siguientes.

## <a name="create-a-kubernetes-secret"></a>Creación de un secreto de Kubernetes

Kubernetes necesita credenciales para acceder al recurso compartido de archivos creado en el paso anterior. Estas credenciales se almacenan en un [secreto de Kubernetes][kubernetes-secret], al que se hace referencia al crear un pod de Kubernetes.

Use el comando `kubectl create secret` para crear el secreto. En el ejemplo siguiente se crea un recurso compartido denominado *azure-secret* y rellena los valores de *azurestorageaccountname* y *azurestorageaccountkey* en el paso anterior. Para usar una cuenta de almacenamiento de Azure existente, proporcione su nombre y su clave.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Montaje del recurso compartido de archivos como un volumen

Para montar el recurso compartido de Azure Files en el pod, configure el volumen en las especificaciones del contenedor. Cree un nuevo archivo denominado `azure-files-pod.yaml` con el contenido siguiente. Si ha cambiado el nombre del recurso compartido de Azure Files o el nombre del secreto, actualice los valores *shareName* y *secretName*. Además, actualice el valor de `mountPath`, que es la ruta de acceso en la que se monta el recurso compartido de Azure Files en el pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Use el comando `kubectl` para crear el pod.

```console
kubectl apply -f azure-files-pod.yaml
```

Ahora tiene un pod en ejecución con un recurso compartido de Azure Files montado en */mnt/azure*. Puede usar `kubectl describe pod mypod` para comprobar que el recuso compartido se montó correctamente. La siguiente salida de ejemplo condensada muestra el volumen montado en el contenedor:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Sat, 02 Mar 2019 00:05:47 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
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

Si utiliza un clúster de versión 1.8.5 o superior y crea estáticamente el objeto de volumen persistente, deben especificarse las opciones de montaje en el objeto *PersistentVolume*.

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

Para las prácticas recomendadas asociadas, consulte [procedimientos recomendados para el almacenamiento y copias de seguridad en AKS][operator-best-practices-storage].

Para más información acerca de la interactuación de los clústeres de AKS con Azure Files, consulte el [complemento de Kubernetes para Azure Files][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
