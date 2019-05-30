---
title: Servidor de API autorizado los intervalos de IP en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo para proteger el clúster mediante una dirección IP intervalos de direcciones para el acceso al servidor de API en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 5c27d47a918939d012abee3c2317eba39587d734
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243580"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Preview - autoriza el acceso seguro a la API de servidor mediante intervalos de direcciones IP en Azure Kubernetes Service (AKS)

En Kubernetes, el servidor de la API recibe las solicitudes para realizar acciones en el clúster, como crear recursos o reducir el número de nodos. El servidor de API es la forma central interactuar y administrar un clúster. Para mejorar la seguridad del clúster y minimizar los ataques, el servidor de API solo debe ser accesible desde un conjunto limitado de intervalos de direcciones IP.

Este artículo muestra cómo usar API servidor autorizado los intervalos de direcciones IP para limitar las solicitudes de plano de control. Esta funcionalidad actualmente está en su versión preliminar.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio, participación. Se proporcionan para recopilar comentarios y los errores de nuestra comunidad. En la vista previa, estas características no están diseñadas para su uso en producción. Características en versión preliminar pública se incluyen en el soporte técnico de "mejor esfuerzo". Asistencia de los equipos de soporte técnico de AKS está disponible durante el horario comercial del Pacífico (PST) solo timezone. Para obtener más información, consulte los siguientes artículos de soporte técnico:
>
> * [Directivas de soporte técnico AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de empezar

Servidor de la API autorizado intervalos de direcciones IP solo funcionan para los clústeres AKS nuevo que cree. Este artículo muestra cómo crear un clúster AKS mediante la CLI de Azure.

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Los comandos CLI para configurar los intervalos IP de servidor autorizado API están disponibles en el *-versión preliminar de aks* extensión de la CLI. Instalar el *-versión preliminar de aks* extensión de CLI de Azure mediante el [Agregar extensión az] [ az-extension-add] de comandos, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Si ha instalado anteriormente el *-versión preliminar de aks* instalar haya disponible de extensión, actualizaciones utilizando el `az extension update --name aks-preview` comando.

### <a name="register-feature-flag-for-your-subscription"></a>Registrar la marca de características para la suscripción

Para usar la API de servidor autorizado los intervalos de IP, habilite primero una marca de características en su suscripción. Para registrar el *APIServerSecurityPreview* marca de características, use la [register de la característica de az] [ az-feature-register] comando tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Limitaciones

Al configurar intervalos IP del servidor autorizado de API, se aplican las siguientes limitaciones:

* Actualmente no puede usar espacios de desarrollo de Azure como la comunicación con el servidor de la API también está bloqueada.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Información general del servidor de API de los intervalos IP autorizados

El servidor de API de Kubernetes es cómo se exponen las APIs Kubernetes subyacente. Este componente proporciona la interacción de las herramientas de administración, como `kubectl` o el panel de Kubernetes. AKS proporciona a un patrón de inquilino único clúster, con un servidor dedicado de API. De forma predeterminada, el servidor de API se asigna una dirección IP pública y se debe controlar el acceso mediante controles de acceso basado en roles (RBAC).

Para proteger el acceso al plano de control de AKS en caso contrario, es públicamente accesible / servidor de la API, puede habilitar y utilizar los intervalos IP autorizados. Estos intervalos IP autorizados solo permiten intervalos de direcciones IP definidos para comunicarse con el servidor de API. Se bloquea una solicitud realizada al servidor de API desde una dirección IP que no forma parte de estos intervalos IP autorizados. Aún debe usar RBAC para, a continuación, autorizar a los usuarios y las acciones que se solicitan.

Para usar la funcionalidad de intervalo IP autorizada, una dirección IP pública se expone en el grupo de nodos mediante la implementación de un servicio básico de NGINX. El servidor de API se comunica con el grupo de nodos a través de esta dirección IP autorizada. A continuación, definir intervalos de direcciones IP adicionales que pueden tener acceso al servidor de API.

Para obtener más información sobre el servidor de API y otros componentes del clúster, consulte [conceptos básicos de Kubernetes para AKS][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

Los intervalos de IP de servidor autorizado de API solo funcionan con los nuevos clústeres AKS. No se puede habilitar los intervalos IP autorizados como parte del clúster de la operación de creación. Si intenta habilitar los intervalos IP autorizados como parte del clúster creación proceso, los nodos del clúster no se puede tener acceso al servidor de API durante la implementación, como la dirección IP de salida no se ha definido en ese momento.

En primer lugar, cree un clúster mediante el [crear az aks] [ az-aks-create] comando. En el ejemplo siguiente se crea un clúster de nodo único denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*.

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

## <a name="create-outbound-gateway-for-firewall-rules"></a>Crear puerta de enlace de salida para las reglas de firewall

Para asegurarse de que los nodos en un clúster de forma confiable pueden comunicarse con el servidor de API cuando se habilita los intervalos IP autorizados en la sección siguiente, crear un firewall de Azure para su uso como la puerta de enlace de salida. La dirección IP del servidor de Azure, a continuación, se agrega a la lista de direcciones IP de servidor API autorizados en la sección siguiente.

> [!WARNING]
> El uso de Firewall de Azure puede incurrir en costos importantes a través de un ciclo de facturación mensual. El requisito de Firewall de Azure solo debería ser necesario en este período de versión preliminar inicial. Para obtener más información y planeación de costo, consulte [Firewall de Azure precios][azure-firewall-costs].

En primer lugar, obtenga el *MC_* nombre del grupo de recursos para el clúster de AKS y la red virtual. A continuación, cree una subred mediante el [crear subred de red virtual de red az] [ az-network-vnet-subnet-create] comando. En el ejemplo siguiente se crea una subred denominada *AzureFirewallSubnet* con el intervalo CIDR de *10.200.0.0/16*:

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

Para crear un Firewall de Azure, instale el *firewall de azure* extensión CLI mediante el [Agregar extensión az] [ az-extension-add] comando. A continuación, cree un firewall mediante el [crear servidor de seguridad de red az] [ az-network-firewall-create] comando. En el ejemplo siguiente se crea un firewall de Azure denominado *myAzureFirewall*:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

Un firewall de Azure se asigna una dirección IP pública que fluye el tráfico de salida. Cree una dirección pública mediante la [crear az network public-ip] [ az-network-public-ip-create] comando y, después, cree una configuración de IP en el firewall mediante la [firewall de red ip-config de az crear] [ az-network-firewall-ip-config-create] que aplica a la dirección IP pública:

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

Ahora cree la regla de red de firewall de Azure para *permitir* todas *TCP* tráfico mediante el [crear red de firewall de red de az-rule] [ az-network-firewall-network-rule-create] comando. En el ejemplo siguiente se crea una regla de red denominada *AllowTCPOutbound* para el tráfico con cualquier dirección de origen o destino:

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

Para asociar el firewall de Azure con la ruta de red, obtenga la información de la tabla de rutas existentes, la dirección IP interna del servidor de Azure y, a continuación, la dirección IP del servidor de la API. Estas direcciones IP se especifican en la sección siguiente para controlar cómo se debería enrutar el tráfico para la comunicación del clúster.

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

Por último, cree una ruta en el AKS red ruta tabla existente mediante el [crear ruta de az network route-table] [ az-network-route-table-route-create] comandos que permita el tráfico utilizar el dispositivo de firewall de Azure para el servidor de la API comunicación.

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

Anote la dirección IP pública de su dispositivo de Firewall de Azure. Esta dirección se agrega a la lista de intervalos IP de servidor autorizado de API en la sección siguiente.

## <a name="enable-authorized-ip-ranges"></a>Habilitar los intervalos IP autorizados

Para habilitar los intervalos IP del servidor autorizado de API, deberá proporcionar una lista de intervalos de direcciones IP autorizadas. Al especificar un intervalo CIDR, comience con la primera dirección IP del intervalo. Por ejemplo, *137.117.106.90/29* es un intervalo válido, pero asegúrese de especificar la primera dirección IP del intervalo, como *137.117.106.88/29*.

Use [actualizar az aks] [ az-aks-update] comando y especifique el *--api-server-autorizado-intervalos de direcciones ip* para permitir. Estos intervalos de direcciones IP suelen ser intervalos usados en las redes locales. Agregue la dirección IP pública del firewall Azure obtenido en el paso anterior, como *20.42.25.196/32*.

En el ejemplo siguiente se habilitan los intervalos IP de servidor autorizado de API en el clúster denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Los intervalos de direcciones IP para autorizar son *20.42.25.196/32* (el firewall de Azure dirección IP pública), a continuación, *172.0.0.10/16* y *168.10.0.10/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.10/16,168.10.0.10/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Actualizar o deshabilitar los intervalos IP autorizados

Para actualizar o deshabilitar los intervalos IP autorizados, debe volver a usar [actualizar az aks] [ az-aks-update] comando. Especifique el intervalo CIDR actualizado que desea permitir o especificar un intervalo vacío para deshabilitar el servidor de la API autorizado los intervalos IP, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha habilitado API intervalos de IP de servidor autorizado. Este enfoque es una parte de la forma de ejecutar un clúster de AKS.

Para obtener más información, consulte [conceptos de seguridad de aplicaciones y clústeres de AKS] [ concepts-security] y [procedimientos recomendados de seguridad del clúster y las actualizaciones en AKS] [ operator-best-practices-cluster-security].

<!-- LINKS - external -->
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/

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
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
