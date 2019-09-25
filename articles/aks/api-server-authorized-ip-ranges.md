---
title: Intervalos de direcciones IP autorizadas por el servidor de API en Azure Kubernetes Service (AKS)
description: Aprenda a proteger el clúster mediante un intervalo de direcciones IP para acceder al servidor de API en Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 59e64b7c84e589da57ea28d6655c9305f4fdc101
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71058343"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Versión preliminar: protección del acceso al servidor de API con intervalos de direcciones IP autorizadas en Azure Kubernetes Service (AKS)

En Kubernetes, el servidor de API recibe solicitudes para realizar acciones en el clúster, como crear recursos o reducir el número de nodos. El servidor de API es la manera centralizada de interactuar con un clúster y administrarlo. Para mejorar la seguridad del clúster y minimizar los ataques, el servidor de API solo debe ser accesible desde un conjunto limitado de intervalos de direcciones IP.

En este artículo se muestra como usar los intervalos de direcciones IP autorizadas por el servidor de API para limitar las solicitudes de control del panel. Esta funcionalidad actualmente está en su versión preliminar.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por hecho que trabaja con clústeres que usan [kubenet][kubenet].  Con clústeres basados en [Azure Container Networking Interface (CNI)][cni-networking], no tendrá la tabla de rutas necesaria para asegurar el acceso.  Tendrá que crear la tabla de rutas manualmente.  Consulte [Administración de tablas de ruta](https://docs.microsoft.com/azure/virtual-network/manage-route-table) para más información.

Los intervalos de direcciones IP autorizadas por el servidor de API solo funcionan para los clústeres nuevos de AKS que cree. En este artículo se muestra cómo utilizar la CLI de Azure para crear un clúster de AKS.

Es preciso que esté instalada y configurada la versión 2.0.61 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI][install-azure-cli] (Instalación de la CLI de Azure).

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Para configurar intervalos de direcciones IP autorizadas por el servidor de API, necesita la versión 0.4.1 o una posterior de la extensión de la CLI para la *versión preliminar de AKS*. Instale la extensión de la CLI de Azure *aks-preview* con el comando [az extension add][az-extension-add] y, a continuación, busque las actualizaciones disponibles con el comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flag-for-your-subscription"></a>Registro de la marca de características para la suscripción

Para usar intervalos de direcciones IP autorizadas por el servidor de API, primero debe habilitar una marca de características en la suscripción. Para registrar la marca de características *APIServerSecurityPreview*, use el comando [az feature register][az-feature-register] tal como se muestra en el siguiente ejemplo:

> [!CAUTION]
> Actualmente, al registrar una característica en una suscripción, no se puede anular el proceso. Después de habilitar algunas características en vista previa, se pueden usar los valores predeterminados en todos los clústeres de AKS y, luego, se pueden crear en la suscripción. No habilite características en vista previa en las suscripciones de producción. Use una suscripción independiente para probar las características en vista previa y recopilar comentarios.

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado de registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

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
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-outbound-gateway-for-firewall-rules"></a>Creación de la puerta de enlace de salida para las reglas de firewall

Para asegurarse de que los nodos de un clúster se comunican confiablemente con el servidor de API al habilitar los intervalos de direcciones IP autorizadas en la siguiente sección, cree un firewall de Azure para usarlo como puerta de enlace de salida. La dirección IP del firewall de Azure se agregará a la lista de direcciones IP autorizadas por el servidor de API en la siguiente sección.

> [!WARNING]
> El uso de Azure Firewall puede suponer costos considerables en el período de facturación mensual. El requisito de usar Azure Firewall solo debería ser necesario en este período inicial de versión preliminar. Para más información y para planear los costos, consulte [Precios de Azure Firewall][azure-firewall-costs].
>
> Por otro lado, si el clúster usa el [equilibrador de carga de SKU estándar][standard-sku-lb], no es necesario configurar Azure Firewall como la puerta de enlace de salida. Use el comando [az network public-ip list][az-network-public-ip-list] y especifique el grupo de recursos de su clúster de AKS, que normalmente comienza con *MC_* . Así, se muestra la dirección IP pública del clúster, la cual puede incluir en la lista de permitidos. Por ejemplo:
>
> ```azurecli-interactive
> RG=$(az aks show --resource-group myResourceGroup --name myAKSClusterSLB --query nodeResourceGroup -o tsv)
> SLB_PublicIP=$(az network public-ip list --resource-group $RG --query [].ipAddress -o tsv)
> az aks update --api-server-authorized-ip-ranges $SLB_PublicIP --resource-group myResourceGroup --name myAKSClusterSLB
> ```

En primer lugar, obtenga el nombre del grupo de recursos *MC_* del clúster de AKS y la red virtual. Después, cree una subred con el comando [az network vnet subnet create][az-network-vnet-subnet-create]. En el siguiente ejemplo se crea una subred denominada *AzureFirewallSubnet* con el rango de CIDR *10.200.0.0/16*:

```azurecli-interactive
# Get the name of the MC_ cluster resource group
MC_RESOURCE_GROUP=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query nodeResourceGroup -o tsv)

# Get the name of the virtual network used by the cluster
VNET_NAME=$(az network vnet list \
    --resource-group $MC_RESOURCE_GROUP \
    --query [0].name -o tsv)

# Create a subnet in the virtual network for Azure Firewall
az network vnet subnet create \
    --resource-group $MC_RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name AzureFirewallSubnet \
    --address-prefixes 10.200.0.0/16
```

Para crear una instancia de Azure Firewall, instale la extensión de la CLI *azure-firewall* con el comando [az extension add][az-extension-add]. A continuación, cree un firewall con el comando [az network firewall create][az-network-firewall-create]. En el siguiente ejemplo se crea un firewall de Azure denominado *myAzureFirewall*:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

El firewall de Azure se asigna una dirección IP pública mediante la cual fluye tráfico de salida. Cree una dirección pública mediante el comando [az network public-ip create][az-network-public-ip-create] y, después, cree una configuración de IP en el firewall con el comando [az network firewall ip-config create][az-network-firewall-ip-config-create] que aplique la dirección IP pública:

```azurecli-interactive
# Create a public IP address for the firewall
FIREWALL_PUBLIC_IP=$(az network public-ip create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallPublicIP \
    --sku Standard \
    --query publicIp.ipAddress -o tsv)

# Associated the firewall with virtual network
az network firewall ip-config create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallIPConfig \
    --vnet-name $VNET_NAME \
    --firewall-name myAzureFirewall \
    --public-ip-address myAzureFirewallPublicIP
```

Ahora cree la regla de red de firewall de Azure para *permitir* todo el tráfico *TCP* mediante el comando [az network firewall network-rule create][az-network-firewall-network-rule-create]. En el ejemplo siguiente se crea una regla de red denominada *AllowTCPOutbound* para el tráfico con cualquier dirección de origen o de destino:

```azurecli-interactive
az network firewall network-rule create \
    --resource-group $MC_RESOURCE_GROUP \
    --firewall-name myAzureFirewall \
    --name AllowTCPOutbound \
    --collection-name myAzureFirewallCollection \
    --priority 200 \
    --action Allow \
    --protocols TCP \
    --source-addresses '*' \
    --destination-addresses '*' \
    --destination-ports '*'
```

Para asociar el firewall de Azure con la ruta de red, obtenga la información de la tabla de rutas existentes, la dirección IP interna del firewall de Azure y la dirección IP del servidor de API. Estas direcciones IP se especifican en la siguiente sección para controlar cómo se debe enrutar el tráfico para la comunicación con el clúster.

```azurecli-interactive
# Get the AKS cluster route table
ROUTE_TABLE=$(az network route-table list \
    --resource-group $MC_RESOURCE_GROUP \
    --query "[?contains(name,'agentpool')].name" -o tsv)

# Get internal IP address of the firewall
FIREWALL_INTERNAL_IP=$(az network firewall show \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewall \
    --query ipConfigurations[0].privateIpAddress -o tsv)

# Get the IP address of API server endpoint
K8S_ENDPOINT_IP=$(kubectl get endpoints -o=jsonpath='{.items[?(@.metadata.name == "kubernetes")].subsets[].addresses[].ip}')
```

Por último, cree una ruta en la tabla de rutas de red de AKS existente mediante el comando [az network route-table route create][az-network-route-table-route-create] que permita al tráfico utilizar el dispositivo de firewall de Azure para la comunicación con el servidor de API.

```azurecli-interactive
az network route-table route create \
    --resource-group $MC_RESOURCE_GROUP \
    --route-table-name $ROUTE_TABLE \
    --name AzureFirewallAPIServer \
    --address-prefix $K8S_ENDPOINT_IP/32 \
    --next-hop-ip-address $FIREWALL_INTERNAL_IP \
    --next-hop-type VirtualAppliance

echo "Public IP address for the Azure Firewall instance that should be added to the list of API server authorized addresses is:" $FIREWALL_PUBLIC_IP
```

Anote la dirección IP pública del dispositivo de Azure Firewall. Esta dirección se agrega a la lista de intervalos de direcciones IP autorizadas por el servidor de API en la siguiente sección.

## <a name="enable-authorized-ip-ranges"></a>Habilitación de los intervalos de direcciones IP autorizadas

Para habilitar los intervalos de direcciones IP autorizadas por el servidor de API, se proporciona una lista con todos ellos. Al especificar un intervalo de CIDR, comience por la primera dirección IP del intervalo. Por ejemplo, *137.117.106.90/29* es un intervalo válido, pero asegúrese de especificar la primera dirección IP del intervalo, por ejemplo, *137.117.106.88/29*.

Use el comando [az aks update][az-aks-update] y, en el parámetro *--intervalos-de-direcciones-ip-autorizadas-por-el-servidor-de-api*, especifique los intervalos de direcciones IP autorizadas por el servidor de API que se permitirán. Estos intervalos de direcciones IP suelen ser intervalos de direcciones que usan las redes locales. Agregue la dirección IP pública de su propio firewall Azure obtenido en el paso anterior, por ejemplo, *20.42.25.196/32*.

En el siguiente ejemplo se habilitan los intervalos de direcciones IP autorizadas por el servidor de API en el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Los intervalos de direcciones IP que autorizar son *20.42.25.196/32* (la dirección IP pública del firewall de Azure), después, *172.0.0.0/16* y *168.10.0.0/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.0/16,168.10.0.0/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Actualización o deshabilitación de los intervalos de direcciones IP autorizadas

Para actualizar o deshabilitar los intervalos de direcciones IP autorizadas, vuelva a usar el comando [az aks update][az-aks-update]. Especifique el intervalo de CIDR que desee permitir o uno vacío para deshabilitar intervalos de direcciones IP autorizadas por el servidor de API, tal y como se muestra en el siguiente ejemplo:

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
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[create-aks-sp]: kubernetes-service-principal.md#manually-create-a-service-principal
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[standard-sku-lb]: load-balancer-standard.md
