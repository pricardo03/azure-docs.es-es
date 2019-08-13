---
title: Conexión SSH con los nodos de un clúster de Azure Kubernetes Service (AKS)
description: Aprenda a crear una conexión SSH con los nodos de clúster de Azure Kubernetes Service (AKS) para la solución de problemas y tareas de mantenimiento.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: mlearned
ms.openlocfilehash: 6ddd1b160110e7a751f54f89b387a62d94e9308e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "67614475"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Conexión con SSH a los nodos de clúster de Azure Kubernetes Service (AKS) para mantenimiento o solución de problemas

Durante el ciclo de vida del clúster de Azure Kubernetes Service (AKS), es posible que necesite acceder a un nodo de AKS. Este acceso podría ser para mantenimiento, recopilación de registros u otras operaciones de solución de problemas. Puede acceder a los nodos de AKS mediante SSH, incluidos los nodos de Windows Server (actualmente en versión preliminar en AKS). También puede [conectarse a los nodos de Windows Server mediante conexiones de protocolo de escritorio remoto (RDP)][aks-windows-rdp]. Por motivos de seguridad, los nodos de AKS no están expuestos a Internet.

En este artículo se muestra cómo crear una conexión SSH con un nodo de AKS mediante sus direcciones IP privadas.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También es preciso que esté instalada y configurada la versión 2.0.64 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="add-your-public-ssh-key"></a>Adición de la clave SSH pública

De forma predeterminada, las claves SSH se obtienen, o generan, y luego se agregan a los nodos cuando crea un clúster de AKS. Si necesita especificar claves SSH diferentes a las utilizadas cuando creó el clúster de AKS, agregue su clave SSH pública a los nodos de AKS de Linux. Si es necesario, puede crear una clave SSH con [macOS, Linux][ssh-nix] o [Windows][ssh-windows]. Si utiliza PuTTY Gen para crear el par de claves, guarde dicho par en un formato OpenSSH en lugar del formato de clave privada PuTTy predeterminado (archivo .ppk).

> [!NOTE]
> Actualmente, las claves SSH solo se pueden agregar a los nodos de Linux mediante la CLI de Azure. Si usa nodos de Windows Server, use las claves SSH proporcionadas cuando creó el clúster de AKS y vaya al paso que trata sobre [cómo obtener la dirección del nodo de AKS](#get-the-aks-node-address). O bien, [conéctese a los nodos de Windows Server mediante conexiones de protocolo de escritorio remoto (RDP)][aks-windows-rdp].

Los pasos para obtener la dirección IP privada de los nodos AKS son diferentes según el tipo de clúster de AKS que ejecute:

* Para la mayoría de los clústeres de AKS, siga los pasos para [obtener la dirección IP para los clústeres de AKS normales](#add-ssh-keys-to-regular-aks-clusters).
* Si usa cualquier característica en vista previa (GB) en AKS que use conjuntos de escalado de máquinas virtuales, como varios grupos de nodos o compatibilidad con contenedores de Windows Server, [siga los pasos para los clústeres de AKS basados en conjuntos de escalado de máquinas virtuales](#add-ssh-keys-to-virtual-machine-scale-set-based-aks-clusters).

### <a name="add-ssh-keys-to-regular-aks-clusters"></a>Incorporación de claves SSH a los clústeres de AKS normales

Para agregar la clave SSH a un nodo de AKS de Linux, complete los pasos siguientes:

1. Obtenga el nombre del grupo de recursos para los recursos del clúster de AKS mediante [az aks show][az-aks-show]. El nombre del clúster se asigna a la variable denominada *CLUSTER_RESOURCE_GROUP*. Reemplace *myResourceGroup* por el nombre del grupo de recursos donde se encuentra en clúster de AKS:

    ```azurecli-interactive
    CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
    ```

1. Enumere las máquinas virtuales del grupo de recursos de clúster de AKS con el comando [az vm list][az-vm-list]. Estas máquinas virtuales son los nodos de AKS:

    ```azurecli-interactive
    az vm list --resource-group $CLUSTER_RESOURCE_GROUP -o table
    ```

    En la siguiente salida de ejemplo se muestran los nodos de AKS:

    ```
    Name                      ResourceGroup                                  Location
    ------------------------  ---------------------------------------------  ----------
    aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
    ```

1. Para agregar las claves SSH al nodo, utilice el comando [az vm user update][az-vm-user-update]. Proporcione el nombre del grupo de recursos y luego uno de los nodos de AKS obtenidos en el paso anterior. De forma predeterminada, el nombre de usuario para los nodos de AKS es *azureuser*. Proporcione la ubicación de su propia ubicación de clave pública SSH, como *~/.ssh/id_rsa.pub*, o pegue el contenido de su clave pública SSH:

    ```azurecli-interactive
    az vm user update \
      --resource-group $CLUSTER_RESOURCE_GROUP \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

### <a name="add-ssh-keys-to-virtual-machine-scale-set-based-aks-clusters"></a>Incorporación de las claves SSH a los clústeres de AKS basados en conjuntos de escalado de máquinas virtuales

Para agregar la clave SSH a un nodo de AKS de Linux que forma parte de un conjunto de escalado de máquinas virtuales, complete los pasos siguientes:

1. Obtenga el nombre del grupo de recursos para los recursos del clúster de AKS mediante [az aks show][az-aks-show]. El nombre del clúster se asigna a la variable denominada *CLUSTER_RESOURCE_GROUP*. Reemplace *myResourceGroup* por el nombre del grupo de recursos donde se encuentra en clúster de AKS:

    ```azurecli-interactive
    CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
    ```

1. A continuación, obtenga el conjunto de escalado de máquinas virtuales para el clúster de AKS mediante el comando [az vmss list][az-vmss-list]. El nombre del conjunto de escalado de máquinas virtuales se asigna a la variable denominada *SCALE_SET_NAME*:

    ```azurecli-interactive
    SCALE_SET_NAME=$(az vmss list --resource-group $CLUSTER_RESOURCE_GROUP --query [0].name -o tsv)
    ```

1. Para agregar las claves SSH a los nodos de un conjunto de escalado de máquinas virtuales, use el comando [az vmss extension set][az-vmss-extension-set]. El grupo de recursos de clúster y el nombre del conjunto de escalado de máquinas virtuales se proporcionan desde los comandos anteriores. De forma predeterminada, el nombre de usuario para los nodos de AKS es *azureuser*. Si es necesario, actualice la ubicación de su propia ubicación de clave pública de SSH, como *~/.ssh/id_rsa.pub*:

    ```azurecli-interactive
    az vmss extension set  \
        --resource-group $CLUSTER_RESOURCE_GROUP \
        --vmss-name $SCALE_SET_NAME \
        --name VMAccessForLinux \
        --publisher Microsoft.OSTCExtensions \
        --version 1.4 \
        --protected-settings "{\"username\":\"azureuser\", \"ssh_key\":\"$(cat ~/.ssh/id_rsa.pub)\"}"
    ```

1. Aplique la clave SSH a los nodos mediante el comando [az vmss update-instances][az-vmss-update-instances]:

    ```azurecli-interactive
    az vmss update-instances --instance-ids '*' \
        --resource-group $CLUSTER_RESOURCE_GROUP \
        --name $SCALE_SET_NAME
    ```

## <a name="get-the-aks-node-address"></a>Obtención de la dirección del nodo de AKS

Los nodos de AKS no están expuestos públicamente a Internet. Para conectarse mediante SSH a los nodos de AKS, use la dirección IP privada. En el paso siguiente, creará un pod de asistente en el clúster de AKS que le permitirá conectarse mediante SSH a esta dirección IP privada del nodo. Los pasos para obtener la dirección IP privada de los nodos AKS son diferentes según el tipo de clúster de AKS que ejecute:

* Para la mayoría de los clústeres de AKS, siga los pasos para [obtener la dirección IP para los clústeres de AKS normales](#ssh-to-regular-aks-clusters).
* Si usa cualquier característica en vista previa (GB) en AKS que use conjuntos de escalado de máquinas virtuales, como varios grupos de nodos o compatibilidad con contenedores de Windows Server, [siga los pasos para los clústeres de AKS basados en conjuntos de escalado de máquinas virtuales](#ssh-to-virtual-machine-scale-set-based-aks-clusters).

### <a name="ssh-to-regular-aks-clusters"></a>SSH a los clústeres de AKS normales

Vea la dirección IP privada de un nodo de clúster de AKS con el comando [az vm list-ip-addresses][az-vm-list-ip-addresses]. Proporcione su propio nombre de grupo de recursos de clúster de AKS obtenido en un paso anterior [az-aks-show][az-aks-show]:

```azurecli-interactive
az vm list-ip-addresses --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

En la siguiente salida de ejemplo se muestran las direcciones IP privadas de los nodos de AKS:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

### <a name="ssh-to-virtual-machine-scale-set-based-aks-clusters"></a>SSH a los clústeres de AKS basados en conjuntos de escalado de máquinas virtuales

Enumere la dirección IP interna de los nodos utilizando el [comando kubectl get][kubectl-get]:

```console
kubectl get nodes -o wide
```

La salida del ejemplo siguiente muestra las direcciones IP internas de todos los nodos del clúster, incluido un nodo de Windows Server.

```console
$ kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Registre la dirección IP interna del nodo que desea solucionar. Usará esta dirección en un paso posterior.

## <a name="create-the-ssh-connection"></a>Creación de la conexión SSH

Para crear una conexión SSH a un nodo AKS, ejecute un pod asistente en el clúster de AKS. Este pod asistente proporciona acceso SSH al clúster y después acceso adicional al nodo SSH. Para crear y utilizar este pod asistente, siga estos pasos:

1. Ejecute una imagen de contenedor `debian` y asocie a ella una sesión de terminal. Este contenedor se puede usar para crear una sesión de SSH con cualquier nodo del clúster de AKS:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

    > [!TIP]
    > Si usa nodos de Windows Server (actualmente en versión preliminar en AKS), agregue un selector de nodo al comando para programar el contenedor de Debian en un nodo de Linux como se indica a continuación:
    >
    > `kubectl run -it --rm aks-ssh --image=debian --overrides='{"apiVersion":"apps/v1","spec":{"template":{"spec":{"nodeSelector":{"beta.kubernetes.io/os":"linux"}}}}}'`

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
        https://www.ubuntu.com/business/services/cloud
    
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
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-windows-rdp]: rdp.md
[ssh-nix]: ../virtual-machines/linux/mac-create-ssh-keys.md
[ssh-windows]: ../virtual-machines/linux/ssh-from-windows.md
[az-vmss-list]: /cli/azure/vmss#az-vmss-list
[az-vmss-extension-set]: /cli/azure/vmss/extension#az-vmss-extension-set
[az-vmss-update-instances]: /cli/azure/vmss#az-vmss-update-instances
