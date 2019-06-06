---
title: RDP en nodos de Windows Server del clúster de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo crear una conexión RDP con el clúster de Azure Kubernetes Service (AKS) los nodos de Windows Server para tareas de mantenimiento y solución de problemas.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 06/04/2019
ms.author: twhitney
ms.openlocfilehash: 11f6869d4d5a2ee0ef2e986ee8268c7a001ea015
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688635"
---
# <a name="connect-with-rdp-to-azure-kubernetes-service-aks-cluster-windows-server-nodes-for-maintenance-or-troubleshooting"></a>Conectar con RDP a Azure Kubernetes Service (AKS) nodos del clúster Windows Server para el mantenimiento o la solución de problemas

Durante el ciclo de vida del clúster de Azure Kubernetes Service (AKS), necesita tener acceso a un nodo de AKS Windows Server. Este acceso podría ser para mantenimiento, recopilación de registros u otras operaciones de solución de problemas. Puede tener acceso a los nodos de AKS Windows Server con RDP. Como alternativa, si desea usar SSH para acceder a los nodos de AKS Windows Server y que tiene acceso el mismo par de claves que se usó durante la creación del clúster, puede seguir los pasos descritos en [SSH en nodos de clúster de Azure Kubernetes Service (AKS)] [ssh-steps]. Por motivos de seguridad, los nodos de AKS no están expuestos a Internet.

Soporte del nodo de Windows Server está actualmente en versión preliminar de AKS.

Este artículo muestra cómo crear una conexión RDP con un nodo AKS mediante sus direcciones IP privadas.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que tiene un clúster AKS existente con un nodo de Windows Server. Si necesita un clúster de AKS, consulte el artículo sobre [crear un clúster de AKS con un contenedor de Windows mediante la CLI de Azure][aks-windows-cli]. Necesitará el nombre de usuario de administrador de Windows y la contraseña para el nodo del servidor de Windows que desea solucionar los problemas. También necesita un cliente RDP como [Microsoft Remote Desktop][rdp-mac].

También necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="deploy-a-virtual-machine-to-the-same-subnet-as-your-cluster"></a>Implementar una máquina virtual en la misma subred que el clúster

Los nodos de Windows Server del clúster de AKS no tienen direcciones IP accesibles externamente. Para realizar una conexión RDP, puede implementar una máquina virtual con una dirección IP accesible públicamente a la misma subred que los nodos de Windows Server.

En el ejemplo siguiente se crea una máquina virtual denominada *myVM* en el *myResourceGroup* grupo de recursos.

En primer lugar, obtenga la subred usada por el grupo de nodos de Windows Server. Para obtener el identificador de subred, se necesita el nombre de la subred. Para obtener el nombre de la subred, se necesita el nombre de la red virtual. Obtiene el nombre de red virtual consultando el clúster para su lista de redes. Para consultar el clúster, necesita su nombre. Puede obtener todos estos ejecutando lo siguiente en Azure Cloud Shell:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
VNET_NAME=$(az network vnet list -g $CLUSTER_RG --query [0].name -o tsv)
SUBNET_NAME=$(az network vnet subnet list -g $CLUSTER_RG --vnet-name $VNET_NAME --query [0].name -o tsv)
SUBNET_ID=$(az network vnet subnet show -g $CLUSTER_RG --vnet-name $VNET_NAME --name $SUBNET_NAME --query id -o tsv)
```

Ahora que tiene el SUBNET_ID, ejecute el siguiente comando en la misma ventana Azure Cloud Shell para crear la máquina virtual:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2019datacenter \
    --admin-username azureuser \
    --admin-password myP@ssw0rd12 \
    --subnet $SUBNET_ID \
    --query publicIpAddress -o tsv
```

La salida del ejemplo siguiente se muestra la máquina virtual se ha creado correctamente y muestra la dirección IP pública de la máquina virtual.

```console
13.62.204.18
```

Registrar la dirección IP pública de la máquina virtual. Usará esta dirección en un paso posterior.

## <a name="get-the-node-address"></a>Obtener la dirección de nodo

Para administrar un clúster de Kubernetes, usará [kubectl][kubectl], el cliente de línea de comandos de Kubernetes. Si usa Azure Cloud Shell, `kubectl` ya está instalado. Para instalar `kubectl` localmente, use el comando [az aks install-cli][az-aks-install-cli]:
    
```azurecli-interactive
az aks install-cli
```

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][az-aks-get-credentials]. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Lista de la dirección IP interna de los nodos de Windows Server mediante la [kubectl get] [ kubectl-get] comando:

```console
kubectl get nodes -o wide
```

La salida del ejemplo siguiente muestra las direcciones IP internas de todos los nodos del clúster, incluidos los nodos de Windows Server.

```console
$ kubectl get nodes -o wide
NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Registrar la dirección IP interna del nodo de Windows Server que desea solucionar. Usará esta dirección en un paso posterior.

## <a name="connect-to-the-virtual-machine-and-node"></a>Conectarse a la máquina virtual y el nodo

Conectarse a la dirección IP pública de la máquina virtual que creó anteriormente con un cliente RDP como [Microsoft Remote Desktop][rdp-mac].

![Imagen de la conexión a la máquina virtual con un cliente RDP](media/rdp/vm-rdp.png)

Una vez conectado a la máquina virtual, conectar con el *dirección IP interna* del nodo de Windows Server que desea solucionar problemas con un cliente RDP desde dentro de la máquina virtual.

![Imagen de la conexión con el nodo de Windows Server mediante un cliente RDP](media/rdp/node-rdp.png)

Ahora está conectado a su nodo de Windows Server.

![Imagen de la ventana de cmd en el nodo del servidor de Windows](media/rdp/node-session.png)

Ahora puede ejecutar los comandos de la solución de problemas el *cmd* ventana. Dado que los nodos de Windows Server usan Windows Server Core, no hay una GUI completa u otras herramientas de interfaz gráfica de usuario cuando se conecta a un nodo de Windows Server a través de RDP.

## <a name="remove-rdp-access"></a>Quitar el acceso RDP

Cuando haya terminado, cierre la conexión RDP en el nodo de Windows Server, luego, salga de la sesión RDP a la máquina virtual. Después de salir ambas sesiones RDP, elimine la máquina virtual con el [az vm delete] [ az-vm-delete] comando:

```azurecli-interactive
az vm delete --resource-group myResourceGroup --name myVM
```

## <a name="next-steps"></a>Pasos siguientes

Si necesita datos adicionales de solución de problemas, puede [ver los registros del nodo maestro de Kubernetes] [ view-master-logs] o [Azure Monitor][azure-monitor-containers].

<!-- EXTERNAL LINKS -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[rdp-mac]: https://aka.ms/rdmac

<!-- INTERNAL LINKS -->
[aks-windows-cli]: windows-container-cli.md
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-vm-delete]: /cli/azure/vm#az-vm-delete
[azure-monitor-containers]: ../azure-monitor/insights/container-insights-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ssh-steps]: ssh.md
[view-master-logs]: view-master-logs.md
