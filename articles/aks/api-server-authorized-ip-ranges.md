---
title: Intervalos de direcciones IP autorizadas por el servidor de API en Azure Kubernetes Service (AKS)
description: Aprenda a proteger el clúster mediante un intervalo de direcciones IP para acceder al servidor de API en Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 8418499cc3e094162ac7483aaa6c71e74db95ae1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472958"
---
# <a name="secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Protección del acceso al servidor de API con intervalos de direcciones IP autorizadas en Azure Kubernetes Service (AKS)

En Kubernetes, el servidor de API recibe solicitudes para realizar acciones en el clúster, como crear recursos o reducir el número de nodos. El servidor de API es la manera centralizada de interactuar con un clúster y administrarlo. Para mejorar la seguridad del clúster y minimizar los ataques, el servidor de API solo debe ser accesible desde un conjunto limitado de intervalos de direcciones IP.

En este artículo se muestra cómo usar los intervalos IP autorizados por el servidor de API para limitar qué direcciones IP y CIDR pueden acceder al plano de control.

> [!IMPORTANT]
> En los nuevos clústeres, los intervalos IP autorizados del servidor de API solo se admiten en la SKU *Estándar* del equilibrador de carga. Los clústeres existentes con la SKU *Básico* del equilibrador de carga y los intervalos IP autorizados del servidor de API seguirán funcionando tal cual. Esos clústeres también pueden actualizarse y seguirán funcionando.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por hecho que trabaja con clústeres que usan [kubenet][kubenet].  Con clústeres basados en [Azure Container Networking Interface (CNI)][cni-networking], no tendrá la tabla de rutas necesaria para asegurar el acceso.  Tendrá que crear la tabla de rutas manualmente.  Consulte [Administración de tablas de ruta](https://docs.microsoft.com/azure/virtual-network/manage-route-table) para más información.

Los intervalos de direcciones IP autorizadas por el servidor de API solo funcionan para los clústeres nuevos de AKS que cree. En este artículo se muestra cómo utilizar la CLI de Azure para crear un clúster de AKS.

Es preciso que esté instalada y configurada la versión 2.0.76 de la CLI de Azure, o cualquier otra posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI][install-azure-cli] (Instalación de la CLI de Azure).

## <a name="limitations"></a>Limitaciones

Al configurar intervalos de direcciones IP autorizadas por el servidor de API se aplican las siguientes limitaciones:

* Actualmente no puede usar Azure Dev Spaces como comunicación con el servidor de API, también está bloqueado.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Información general sobre los intervalos de direcciones IP autorizadas por el servidor de API

El servidor de API de Kubernetes es el modo en el que se exponen las API de Kubernetes subyacentes. Este componente proporciona la interacción de las herramientas de administración, como `kubectl` o el panel de Kubernetes. AKS proporciona un patrón de clúster de inquilino único con un servidor de API dedicado. De forma predeterminada, el servidor de API se asigna a una dirección IP pública y debe controlar el acceso mediante controles de acceso basado en rol (RBAC).

Para proteger el acceso al panel de control de AKS y al servidor de API (que, de lo contrario, serían accesibles al público), puede habilitar y usar intervalos de direcciones IP autorizadas. De esta manera, solo se permite la comunicación con el servidor de API a los intervalos de direcciones IP definidas. Las solicitudes al servidor de API procedentes de direcciones IP que no formen parte de este intervalo se bloquean. Debe continuar usando RBAC para autorizar a los usuarios y las acciones que soliciten.

Para más información sobre el servidor de API y otros componentes del clúster, consulte los [conceptos clave de Kubernetes para AKS][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

Los intervalos de direcciones IP autorizadas por el servidor de API solo funcionan para los clústeres nuevos de AKS. No se pueden habilitar los intervalos de direcciones IP autorizadas como parte de la creación del clúster. Si intenta hacerlo, los nodos del clúster no podrán acceder al servidor de API durante la implementación, ya que la dirección IP de salida no está definida en ese momento.

Primero cree un clúster con el comando [az aks create][az-aks-create]. En el ejemplo siguiente se crea un solo clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="update-cluster-with-authorized-ip-ranges"></a>Actualización del clúster con intervalos IP autorizados

De forma predeterminada, el clúster usa la [SKU Estándar del equilibrador de carga][standard-sku-lb], que puede usar para configurar la puerta de enlace de salida. Use el comando [az network public-ip list][az-network-public-ip-list] y especifique el grupo de recursos de su clúster de AKS, que normalmente comienza con *MC_* . Así, se muestra la dirección IP pública del clúster, la cual puede permitir. Use el comando [az aks update][az-aks-update] y especifique el parámetro *--api-server-authorized-ip-ranges* para permitir la dirección IP del clúster. Por ejemplo:

```azurecli-interactive
RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
SLB_PublicIP=$(az network public-ip list --resource-group $RG --query [].ipAddress -o tsv)
az aks update --api-server-authorized-ip-ranges $SLB_PublicIP --resource-group myResourceGroup --name myAKSCluster
```

Para habilitar intervalos IP autorizados del servidor de API, use el comando [az aks update][az-aks-update] y especifique el parámetro *--api-server-authorized-ip-ranges* para proporcionar una lista de intervalos IP autorizados. Estos intervalos IP suelen ser intervalos de direcciones que usan las redes locales o IP públicas. Al especificar un intervalo de CIDR, comience por la primera dirección IP del intervalo. Por ejemplo, *137.117.106.90/29* es un intervalo válido, pero asegúrese de especificar la primera dirección IP del intervalo, por ejemplo, *137.117.106.88/29*.

En el siguiente ejemplo se habilitan los intervalos de direcciones IP autorizadas por el servidor de API en el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Los intervalos IP que se autorizan son *172.0.0.0/16* (intervalo de direcciones de pod/nodos) y *168.10.0.0/18* (ServiceCidr):

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 172.0.0.0/16,168.10.0.0/18
```

> [!NOTE]
> Debe agregar estos intervalos a una lista de permitidos:
> - La dirección IP pública del firewall
> - El CIDR del servicio
> - El intervalo de direcciones de las subredes, con los nodos y pods
> - Cualquier intervalo que represente redes desde las que se va a administrar el clúster

## <a name="disable-authorized-ip-ranges"></a>Deshabilitación de los intervalos IP autorizados

Para deshabilitar los intervalos IP autorizados, use [az aks update][az-aks-update] y especifique un intervalo vacío para deshabilitar los intervalos IP autorizados del servidor de API. Por ejemplo:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha habilitado intervalos de direcciones IP autorizadas por el servidor de API. Este enfoque forma parte del modo de ejecución segura de un clúster de AKS.

Para más información, consulte [Conceptos de seguridad de las aplicaciones y los clústeres en Azure Kubernetes Service (AKS)][concepts-security] y [Procedimientos recomendados para la seguridad de clústeres y las actualizaciones en AKS][operator-best-practices-cluster-security].

<!-- LINKS - external -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet

<!-- LINKS - internal -->
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[standard-sku-lb]: load-balancer-standard.md
