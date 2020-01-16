---
title: Clúster privado de Azure Kubernetes Service
description: Aprenda a crear un clúster privado de Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 12/10/2019
ms.author: mlearned
ms.openlocfilehash: 8af0f998df2a92e51078a2e23806cca07ff08ca3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475018"
---
# <a name="public-preview---private-azure-kubernetes-service-cluster"></a>Versión preliminar pública: clúster privado de Azure Kubernetes Service

En un clúster privado, el servidor del plano de control o de API tendrá direcciones IP internas definidas en el documento [RFC1918](https://tools.ietf.org/html/rfc1918).  Mediante el uso de un clúster privado, puede asegurarse de que el tráfico entre el servidor de API y los grupos de nodos permanece solo en la red privada.

El servidor del plano de control o de API, que se encuentra en una suscripción de Azure administrada por AKS, y el grupo de clústeres o nodos del cliente, que se encuentra en una suscripción de cliente, se pueden comunicar entre sí a través del [servicio Private Link][private-link-service] de la red virtual del servidor de API y de un punto de conexión privado expuesto en la subred del clúster de AKS del cliente.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS](support-policies.md)
> * [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

* Necesita la versión 2.0.77 de la CLI de Azure u otra versión posterior y la versión 0.4.18 de la extensión aks-preview.

## <a name="current-supported-regions"></a>Regiones admitidas actualmente
* Oeste de EE. UU.
* Oeste de EE. UU. 2
* Este de EE. UU. 2
* Centro de Canadá
* Europa del Norte
* Europa occidental
* Este de Australia

## <a name="install-latest-aks-cli-preview-extension"></a>Instalación de la extensión de la versión preliminar de la CLI de AKS más reciente

Para usar clústeres privados, necesita la versión 0.4.18 o posterior de la extensión *aks-preview* de la CLI. Instale la extensión de la CLI de Azure *aks-preview* con el comando [az extension add][az-extension-add] y, a continuación, busque las actualizaciones disponibles con el comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Actualmente, al registrar una característica en una suscripción, no se puede anular el registro de esa característica. Después de habilitar algunas características en vista previa, se pueden usar los valores predeterminados en todos los clústeres de AKS y, luego, se pueden crear en la suscripción. No habilite características en vista previa en las suscripciones de producción. Use una suscripción independiente para probar las características en vista previa y recopilar comentarios.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado aparezca como *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Cuando el estado sea Registrado, actualice el registro del proveedor de recursos *Microsoft.ContainerService* mediante el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Creación de un clúster privado de AKS

#### <a name="default-basic-networking"></a>Redes básicas predeterminadas 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Donde --enable-private-cluster es una marca obligatoria para un clúster privado. 

#### <a name="advanced-networking"></a>Redes avanzadas  

```azurecli-interactive
az aks create \ 
    --resource-group <private-cluster-resource-group>\ 
    --name <private-cluster-name> \ 
    --load-balancer-sku standard
    --enable-private-cluster 
    --network-plugin azure \ 
    --vnet-subnet-id <subnet-id> \ 
    --docker-bridge-address 172.17.0.1/16 \ 
    --dns-service-ip 10.2.0.10 \ 
    --service-cidr 10.2.0.0/24 \ 
```
Donde --enable-private-cluster es una marca obligatoria para un clúster privado. 

## <a name="steps-to-connect-to-the-private-cluster"></a>Pasos para conectarse al clúster privado
El punto de conexión del servidor de API no tiene ninguna dirección IP pública. Por lo tanto, los usuarios deberán crear una máquina virtual de Azure en una red virtual y conectarse al servidor de API. Los pasos son:

* Obtenga credenciales para conectarse al clúster.

   ```azurecli-interactive
   az aks get-credentials --name MyManagedCluster --resource-group MyResourceGroup
   ```
* Cree una máquina virtual en la misma red virtual que el clúster de AKS o bien cree una máquina virtual en otra red virtual y empareje esta red virtual con la red virtual del clúster de AKS.
* Si crea una máquina virtual en una red virtual diferente, deberá configurar un vínculo entre esta red virtual y la zona DNS privada.
    * Vaya al grupo de recursos MC_* en el portal. 
    * Haga clic en la zona DNS privada. 
    * En el panel izquierdo, seleccione el vínculo Red virtual.
    * Cree un nuevo vínculo para agregar la red virtual de la máquina virtual a la zona DNS privada *(el vínculo de la zona DNS tarda unos minutos en estar disponible)* .
* Conéctese mediante SSH a la máquina virtual.
* Instale la herramienta Kubectl y ejecute comandos Kubectl.

## <a name="dependencies"></a>Dependencias  
* Solo equilibrador de carga estándar; no se admite el equilibrador de carga básico.  

## <a name="limitations"></a>Limitaciones 
* Se aplican las mismas [limitaciones del servicio Azure Private Link][private-link-service] a los clústeres privados, puntos de conexión privados de Azure y puntos de conexión del servicio de red virtual, que actualmente no se admiten en la misma red virtual.
* No se admiten nodos virtuales en un clúster privado para poner en marcha instancias privadas de ACI en una red virtual de Azure privada.
* No se admite de serie la integración de Azure DevOps con clústeres privados.
* Si los clientes necesitan habilitar ACR para trabajar con clústeres de AKS privados, la red virtual de ACR deberá emparejarse con la red virtual del clúster del agente.
* Actualmente no hay compatibilidad con Azure Dev Spaces.
* No se admite la conversión de clústeres de AKS existentes en clústeres privados.  
* La eliminación o modificación del punto de conexión privado en la subred del cliente hará que el clúster deje de funcionar. 
* Actualmente no existe compatibilidad con los datos en directo de Azure Monitor para contenedores.
* Actualmente no se admite la posibilidad de traer el propio DNS.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: https://docs.microsoft.com/azure/private-link/private-link-service-overview
