---
title: Conexión SSH con los nodos de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a crear una conexión SSH con los nodos de clúster de Azure Kubernetes Service (AKS) para la solución de problemas y tareas de mantenimiento.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/21/2018
ms.author: iainfou
ms.openlocfilehash: f0097c2ba42378c66bedd614032b63c23a2483dd
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447121"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Conexión con SSH a los nodos de clúster de Azure Kubernetes Service (AKS) para mantenimiento o solución de problemas

Durante el ciclo de vida del clúster de Azure Kubernetes Service (AKS), es posible que necesite acceder a un nodo de AKS. Este acceso podría ser para mantenimiento, recopilación de registros u otras operaciones de solución de problemas. Los nodos de AKS son máquinas virtuales Linux, por lo que puede acceder a ellos mediante SSH. Por motivos de seguridad, los nodos de AKS no están expuestos a Internet.

En este artículo se muestra cómo crear una conexión SSH con un nodo de AKS mediante sus direcciones IP privadas.

## <a name="add-your-public-ssh-key"></a>Adición de la clave SSH pública

Por defecto, las claves SSH se generan cuando se crea un clúster de AKS. Si no especificó sus propias claves SSH cuando creó el clúster de AKS, agregue las claves SSH públicas a los nodos de AKS. 

Para agregar la clave SSH a un nodo de AKS, complete los pasos siguientes:

1. Obtenga el nombre del grupo de recursos para los recursos del clúster de AKS mediante [az aks show][az-aks-show]. Proporcione su propio grupo de recursos básicos y el nombre del grupo de AKS:

    ```azurecli
    az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
    ```

1. Enumere las máquinas virtuales en el grupo de recursos de clúster de AKS con el comando [az vm list][az-vm-list]. Estas máquinas virtuales son los nodos de AKS:

    ```azurecli
    az vm list --resource-group MC_myResourceGroup_myAKSCluster_eastus -o table
    ```

    En la siguiente salida de ejemplo se muestran los nodos de AKS:

    ```
    Name                      ResourceGroup                                  Location
    ------------------------  ---------------------------------------------  ----------
    aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
    ```

1. Para agregar las claves SSH al nodo, utilice el comando [az vm user update][az-vm-user-update]. Proporcione el nombre del grupo de recursos y luego uno de los nodos de AKS obtenidos en el paso anterior. De forma predeterminada, el nombre de usuario para los nodos de AKS es *azureuser*. Proporcione la ubicación de su propia ubicación de clave pública SSH, como *~/.ssh/id_rsa.pub*, o pegue el contenido de su clave pública SSH:

    ```azurecli
    az vm user update \
      --resource-group MC_myResourceGroup_myAKSCluster_eastus \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="get-the-aks-node-address"></a>Obtención de la dirección del nodo de AKS

Los nodos de AKS no están expuestos públicamente a Internet. Para conectarse mediante SSH a los nodos de AKS, use la dirección IP privada.

Vea la dirección IP privada de un nodo de clúster de AKS con el comando [az vm list-ip-addresses][az-vm-list-ip-addresses]. Proporcione su propio nombre de grupo de recursos de clúster de AKS obtenido en un paso anterior [az-aks-show][az-aks-show]:

```azurecli
az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table
```

En la siguiente salida de ejemplo se muestran las direcciones IP privadas de los nodos de AKS:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

## <a name="create-the-ssh-connection"></a>Creación de la conexión SSH

Para crear una conexión SSH a un nodo AKS, ejecute un pod asistente en el clúster de AKS. Este pod asistente proporciona acceso SSH al clúster y después acceso adicional al nodo SSH. Para crear y utilizar este pod asistente, siga estos pasos:

1. Ejecute una imagen de contenedor `debian` y asocie a ella una sesión de terminal. Este contenedor se puede usar para crear una sesión de SSH con cualquier nodo del clúster de AKS:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

1. La imagen base de Debian no incluye componentes SSH. Una vez que la sesión de terminal esté conectada al contenedor, instale un cliente SSH mediante `apt-get` de la siguiente manera:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. En una nueva ventana de terminal, no conectada al contenedor, enumere los pods en el clúster de AKS mediante el comando [kubectl get pods][kubectl-get]. El pod creado en el paso anterior comienza con el nombre *aks-ssh*, como se muestra en el siguiente ejemplo:

    ```
    $ kubectl get pods
    
    NAME                       READY     STATUS    RESTARTS   AGE
    aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
    ```

1. En el primer paso de este artículo, ha agregado la clave SSH pública al nodo de AKS. Ahora, copie la clave SSH privada en el pod. Esta clave privada se utiliza para crear el SSH en los nodos de AKS.

    Proporcione su propio nombre de pod *aks-ssh* obtenido en el paso anterior. Si es necesario, cambie *~/.ssh/id_rsa* a la ubicación de la clave SSH privada:

    ```console
    kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
    ```

1. De vuelta en la sesión de terminal en el contenedor, actualice los permisos de la clave SSH privada copiada `id_rsa` para que sea de solo lectura para el usuario:

    ```console
    chmod 0600 id_rsa
    ```

1. Ahora, cree una conexión SSH al nodo de AKS. De nuevo, el nombre de usuario para los nodos de AKS es *azureuser*. Acepte el aviso para continuar con la conexión, ya que la clave SSH es la primera en la que se confía. A continuación, se proporcionan con el aviso de bash del nodo de AKS:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4
    
    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.
    
    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud
    
    [...]
    
    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>Eliminación del acceso SSH

Cuando haya finalizado, `exit` la sesión de SSH y, a continuación, `exit` la sesión del contenedor interactiva. Cuando esta sesión de contenedor se cierra, se elimina el pod usado para el acceso SSH desde el clúster de AKS.

## <a name="next-steps"></a>Pasos siguientes

Si necesita datos adicionales para la solución de problemas, puede [ver los registros de kubelet][view-kubelet-logs] o [ver los registros del nodo maestro de Kubernetes][view-master-logs].

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
