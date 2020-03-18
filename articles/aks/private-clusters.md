---
title: Creación de un clúster privado de Azure Kubernetes Service
description: Aprenda a crear un clúster privado de Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 2/21/2020
ms.openlocfilehash: 0a05bd15fff97d4f0020f6ce82ee90a2fe995edf
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2020
ms.locfileid: "78944195"
---
# <a name="create-a-private-azure-kubernetes-service-cluster-preview"></a>Creación de un clúster privado de Azure Kubernetes Service (versión preliminar)

En un clúster privado, el servidor de la API o el plano de control tienen direcciones IP internas que se definen en el documento [RFC1918 sobre la asignación de direcciones para conexiones privadas](https://tools.ietf.org/html/rfc1918). Mediante el uso de un clúster privado, puede asegurarse de que el tráfico entre el servidor de la API y los grupos de nodos permanece solo en la red privada.

El plano de control o el servidor de la API están en una suscripción de Azure administrada mediante Azure Kubernetes Service (AKS). El grupo de clústeres o nodos de un cliente está en la suscripción del cliente. El servidor y el grupo de clústeres o nodos pueden comunicarse entre sí a través del [servicio de Azure Private Link][private-link-service] en la red virtual del servidor de la API y de un punto de conexión privado expuesto en la subred del clúster de AKS del cliente.

> [!IMPORTANT]
> Las características en vista previa de AKS están disponibles como opción de participación y autoservicio. Las versiones preliminares se proporcionan *tal cual* y *como están disponibles*, y están excluidas del Acuerdo de Nivel de Servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente *en la medida de lo posible*. Por tanto, estas características no están diseñadas para su uso en producción. Para más información, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS](support-policies.md)
> * [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="prerequisites"></a>Prerrequisitos

* La versión 2.0.77 de la CLI de Azure u otra versión posterior y la versión 0.4.18 de la extensión de la versión preliminar de la CLI de AKS.

## <a name="currently-supported-regions"></a>Regiones admitidas actualmente

* Este de Australia
* Sudeste de Australia
* Sur de Brasil
* Centro de Canadá
* Este de Canadá
* Centro de EE. UU.
* Este de Asia
* Este de EE. UU.
* Este de EE. UU. 2
* EUAP de Este de EE. UU. 2
* Centro de Francia
* Norte de Alemania
* Japón Oriental
* Japón Occidental
* Centro de Corea del Sur
* Corea del Sur
* Centro-Norte de EE. UU
* Norte de Europa
* Norte de Europa
* Centro-sur de EE. UU.
* Sur de Reino Unido 2
* Oeste de Europa
* Oeste de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU. 2

## <a name="currently-supported-availability-zones"></a>Zonas de disponibilidad admitidas actualmente

* Centro de EE. UU.
* Este de EE. UU.
* Este de EE. UU. 2
* Centro de Francia
* Japón Oriental
* Norte de Europa
* Sudeste de Asia
* Sur de Reino Unido 2
* Oeste de Europa
* Oeste de EE. UU. 2

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>Instalación de la extensión de la versión preliminar de AKS de la CLI de Azure más reciente

Para usar clústeres privados, necesita la versión 0.4.18 o una versión posterior de la extensión de la versión preliminar de AKS de la CLI de Azure. Instale la extensión de la versión preliminar de AKS de la CLI de Azure con el comando [az extension add][az-extension-add] y, después, busque las actualizaciones disponibles con este comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Actualmente, al registrar una característica en una suscripción, no se puede anular el registro de esa característica. Después de habilitar algunas características en vista previa, puede usar los valores predeterminados en todos los clústeres de AKS que se crearon en la suscripción. No habilite características en vista previa en las suscripciones de producción. Use una suscripción independiente para probar las características en vista previa y recopilar comentarios.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado de registro aparezca como *Registrado*. Puede comprobar el estado mediante el siguiente comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Cuando el estado sea registrado, actualice el registro del proveedor de recursos *Microsoft.ContainerService* mediante el siguiente comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Creación de un clúster privado de AKS

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos o seleccione uno existente en el clúster de AKS.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Redes básicas predeterminadas 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Donde *--enable-private-cluster* es una marca obligatoria para un clúster privado. 

### <a name="advanced-networking"></a>Redes avanzadas  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Donde *--enable-private-cluster* es una marca obligatoria para un clúster privado. 

> [!NOTE]
> Si la dirección CIDR del puente de Docker (172.17.0.1/16) entra en conflicto con el CIDR de la subred, cambie la dirección del puente de Docker.

## <a name="options-for-connecting-to-the-private-cluster"></a>Opciones para conectarse al clúster privado

El punto de conexión del servidor de la API no tiene ninguna dirección IP pública. Para administrar el servidor de API, necesitará una máquina virtual que tenga acceso a la red virtual de Azure (VNet) del clúster de AKS. Hay varias opciones para establecer la conectividad de red con el clúster privado.

* Crear una máquina virtual en la misma red virtual de Azure (VNet) que el clúster de AKS.
* Usar una máquina virtual de una red diferente y configurar el [emparejamiento de red virtual][virtual-network-peering].  Consulte la sección siguiente para más información sobre esta opción.
* Usar una conexión de [ExpressRoute o VPN][express-route-or-VPN].

La opción más sencilla es crear una máquina virtual en la misma red virtual que el clúster de AKS.  ExpressRoute y las VPN incrementan los costos y requieren redes más complejas.  Para utilizar el emparejamiento de red virtual, debe planear los intervalos CIDR de la red para asegurarse de que no haya intervalos superpuestos.

## <a name="virtual-network-peering"></a>Emparejamiento de redes virtuales de Azure

Tal y como se ha dicho, el emparejamiento de red virtual es un mecanismo para acceder a un clúster privado. Si desea usar el emparejamiento de red virtual, tiene que configurar un vínculo entre la red virtual y la zona DNS privada.
    
1. Vaya al grupo de recursos MC_* en Azure Portal.  
2. Seleccione la zona DNS privada.   
3. En el panel izquierdo, seleccione el vínculo **red virtual**.  
4. Cree un nuevo vínculo para agregar la red virtual de la máquina virtual a la zona DNS privada. El vínculo de la zona DNS puede tardar unos minutos en estar disponible.  
5. Vuelva al grupo de recursos MC_* en Azure Portal.  
6. En el panel derecho, seleccione la red virtual. El nombre de la red virtual tiene el formato *aks-vnet-\** .  
7. En el panel izquierdo, seleccione **Emparejamientos**.  
8. Seleccione **Agregar**, agregue la red virtual de la máquina virtual y, después, cree el emparejamiento.  
9. Vaya a la red virtual en la que tiene la máquina virtual, seleccione **Emparejamientos**, seleccione la red virtual de AKS y, después, cree el emparejamiento. Si los intervalos de direcciones de la red virtual de AKS y de la red virtual de la máquina virtual entran en conflicto, se produce un error de emparejamiento. Para más información, vea el artículo [Emparejamiento de redes virtuales][virtual-network-peering].

## <a name="dependencies"></a>Dependencias  
* El servicio Azure Private Link solo se admite en Standard Azure Load Balancer. No se admite en Basic Azure Load Balancer.  
* Para usar un servidor DNS personalizado, implemente un servidor de AD con DNS para el reenvío a la IP 168.63.129.16.

## <a name="limitations"></a>Limitaciones 
* Los intervalos autorizados de direcciones IP no se pueden aplicar al punto de conexión del servidor de API privada. Solo se aplican al servidor de API pública.
* Availability Zones se admite actualmente para determinadas regiones. Consulte el principio de este documento. 
* Las [limitaciones del servicio Azure Private Link][private-link-service] se aplican a los clústeres privados, a puntos de conexión privados de Azure y a puntos de conexión del servicio de red virtual, que actualmente no se admiten en la misma red virtual.
* No se pueden usar los nodos virtuales de un clúster privado para poner en marcha Azure Container Instances (ACI) en una red virtual de Azure privada.
* No se admite de serie la integración de Azure DevOps con clústeres privados.
* En el caso de los clientes que necesitan habilitar Azure Container Registry para trabajar con instancias privadas de AKS, la red virtual de Container Registry debe estar emparejada con la red virtual del clúster del agente.
* Actualmente no hay compatibilidad con Azure Dev Spaces.
* No se admite la conversión de clústeres de AKS existentes en clústeres privados.
* La eliminación o modificación del punto de conexión privado en la subred del cliente hará que el clúster deje de funcionar. 
* Actualmente no existe compatibilidad con los datos en directo de Azure Monitor para contenedores.
* Actualmente no se admite la posibilidad de *traer el propio DNS*.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: /private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md

