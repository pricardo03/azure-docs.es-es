---
title: Implementar una aplicación de pila doble de IPv6 en Azure virtual network - CLI
titlesuffix: Azure Virtual Network
description: En este artículo se muestra cómo implementa una aplicación de pila doble de IPv6 en Azure virtual network mediante la CLI de Azure.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: 12fbf2ae5387ac0a9350cc203f4a6f2587c8dafe
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62131011"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---cli-preview"></a>Implementar una aplicación de pila doble de IPv6 en Azure virtual network - CLI (versión preliminar)

Este artículo muestra cómo implementar una aplicación de doble pila (IPv4 + IPv6) en Azure que incluya una red virtual de la pila dual con una subred de pila doble, con configuraciones de front-end dual (IPv4 + IPv6), las máquinas virtuales con NIC que tienen una configuración de IP dual, un equilibrador de carga reglas del grupo de seguridad de red dual y dos direcciones IP públicas.

> [!Important]
> Pila dual de IPv6 para Azure Virtual Network está actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI de Azure localmente en su lugar, este inicio rápido requiere que use la CLI de Azure versión 2.0.49 o posterior. Ejecute `az --version` para buscar la versión instalada. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli) para obtener información sobre la instalación o actualización.

## <a name="prerequisites"></a>Requisitos previos
Para usar IPv6 para la característica de red virtual de Azure, debe configurar su suscripción mediante Azure PowerShell como sigue:

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Se tarda hasta 30 minutos en completar el registro de características. Puede comprobar su estado de registro ejecutando el siguiente comando de CLI de Azure:

```azurelci
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
```
Una vez completado el registro, ejecute el siguiente comando:

```azurelci
az provider register --namespace Microsoft.Network
```
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para poder crear la red virtual de doble pila, debe crear un grupo de recursos con [crear grupo az](/cli/azure/group). En el ejemplo siguiente se crea un grupo de recursos denominado *myRGDualStack* en el *eastus* ubicación:

```azurecli
az group create \
--name DsResourceGroup01 \
--location eastus
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses-for-load-balancer"></a>Creación de IPv4 e IPv6 direcciones IP públicas para el equilibrador de carga
Para obtener acceso a los puntos de conexión IPv4 e IPv6 en Internet, necesita direcciones IP públicas IPv4 e IPv6 para el equilibrador de carga. Cree una dirección IP pública con [az network public-ip create](/cli/azure/network/public-ip). En el ejemplo siguiente se crea IPv4 e IPv6 dirección IP pública denominada *dsPublicIP_v4* y *dsPublicIP_v6* en el *myRGDualStack* grupo de recursos:

```azurecli
# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location eastus \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv6

```

## <a name="create-public-ip-addresses-for-vms"></a>Creación de direcciones IP públicas para máquinas virtuales

Para obtener acceso remoto a las máquinas virtuales en internet, necesita las direcciones IP públicas IPv4 para las máquinas virtuales. Cree una dirección IP pública con [az network public-ip create](/cli/azure/network/public-ip).

```azurecli
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku BASIC  \
--allocation-method dynamic  \
--version IPv4
```

## <a name="create-basic-load-balancer"></a>Creación de un equilibrador de carga básico

En esta sección, configurará dos front-end IP (IPv4 e IPv6) y el grupo de direcciones de back-end del equilibrador de carga y, a continuación, crear un equilibrador de carga básico.

### <a name="create-load-balancer"></a>Creación de un equilibrador de carga

Cree el equilibrador de carga básica con [crear az network lb](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) denominado **dsLB** que incluya un grupo de front-end llamado **dsLbFrontEnd_v4**, un grupo de back-end llamado  **dsLbBackEndPool_v4** que está asociado con la dirección IP pública de IPv4 **dsPublicIP_v4** que creó en el paso anterior. 

```azurecli
az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Basic \
--location eastus \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4
```

### <a name="create-ipv6-frontend"></a>Cree front-end de IPv6

Crear una dirección IP de front-end de IPV6 con [crear az network lb frontend-ip](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create). En el ejemplo siguiente se crea una configuración de IP de front-end denominada *dsLbFrontEnd_v6* y adjunta el *dsPublicIP_v6* dirección:

```azurepowershell-interactive
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

```

### <a name="configure-ipv6-back-end-address-pool"></a>Configurar el grupo de direcciones de back-end de IPv6

Crear una dirección de back-end de IPv6 de los grupos con [crear az network lb-grupo de direcciones](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create). El ejemplo siguiente crea el grupo de direcciones de back-end denominado *dsLbBackEndPool_v6* para incluir las máquinas virtuales con configuraciones de NIC de IPv6:

```azurecli
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01
```

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. Defina la configuración de la IP de front-end para el tráfico entrante y el grupo de IP de back-end para el tráfico entrante, junto con los puertos de origen y destino requeridos. 

Cree una regla de equilibrador de carga con [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create). En el ejemplo siguiente se crea reglas de equilibrador de carga denominadas *dsLBrule_v4* y *dsLBrule_v6* y equilibra el tráfico en *TCP* puerto *80* a las configuraciones de IP front-end de IPv4 e IPv6:

```azurecli
az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v4


az network lb rule create \ 
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--backend-pool-name dsLbBackEndPool_v6

```

## <a name="create-network-resources"></a>Crear recursos de red
Antes de implementar algunas máquinas virtuales, debe crear los recursos de red auxiliares - conjunto de disponibilidad, el grupo de seguridad de red, la red virtual y NIC virtuales. 
### <a name="create-an-availability-set"></a>Crear un conjunto de disponibilidad
Para mejorar la disponibilidad de la aplicación, coloque las máquinas virtuales en un conjunto de disponibilidad.

Cree el conjunto de disponibilidad con [az vm availability-set create](https://docs.microsoft.com/cli/azure/vm/availability-set?view=azure-cli-latest). En el ejemplo siguiente se crea un conjunto de disponibilidad denominado *dsAVset*:

```azurecli
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location eastus \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  
```

### <a name="create-network-security-group"></a>Creación de un grupo de seguridad de red

Cree un grupo de seguridad de red para las reglas que rige la comunicación entrante y saliente de la red virtual.

#### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

Cree un grupo de seguridad de red con [crear az network nsg](https://docs.microsoft.com/cli/azure/network/nsg?view=azure-cli-latest#az-network-nsg-create)


```azurecli
az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location eastus

```

#### <a name="create-a-network-security-group-rule-for-inbound-and-outbound-connections"></a>Cree una regla de grupo de seguridad de red para conexiones entrantes y salientes

Cree una regla de grupo de seguridad de red para permitir las conexiones RDP a través de la conexión del puerto 3389, internet a través del puerto 80 y para las conexiones salientes con [crear regla de nsg de red az](https://docs.microsoft.com/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-create).

```azurecli
# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"
```


### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree la red virtual con el comando [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create). En el ejemplo siguiente se crea una red virtual denominada *dsVNET* con subredes *dsSubNET_v4* y *dsSubNET_v6*:

```azurecli
# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location eastus  \
--address-prefixes "10.0.0.0/16" "ace:cab:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "ace:cab:deca:deed::/64" \
--network-security-group dsNSG1
```

### <a name="create-nics"></a>Creación de tarjetas NIC

Crear NIC virtuales para cada máquina virtual con [crear nic de red az](https://docs.microsoft.com/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create). El ejemplo siguiente crea una NIC virtual para cada máquina virtual. Cada NIC tiene dos configuraciones de IP (1 configuración de IPv4, 1 configuración de IPv6). Crear la configuración de IPV6 con [crear az network nic ip-config](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create).
 
```azurecli
# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB
```

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree las máquinas virtuales con [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create). En el ejemplo siguiente, se crean dos máquinas virtuales y los componentes de red virtual necesarios, si aún no existen. 

Crear máquina virtual *dsVM0* como sigue:

```azurecli
 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest  
```
Crear máquina virtual *dsVM1* como sigue:

```azurecli
az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest 
```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Ver la red virtual de pila doble de IPv6 en Azure portal
Puede ver la red virtual de pila doble de IPv6 en Azure portal como sigue:
1. En la barra de búsqueda, escriba *dsVnet*.
2. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela. Esto inicia el **Introducción** página de la red virtual de pila doble llamada *dsVnet*. La red virtual de la pila dual muestra las dos NIC con las configuraciones de IPv4 e IPv6 ubicadas en la subred de pila doble denominada *dsSubnet*.

  ![Red virtual de pila doble de IPv6 en Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> IPv6 para Azure virtual network está disponible en el portal de Azure en solo lectura para esta versión preliminar.


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group#az-group-delete) para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurecli
 az group delete --name DsRG1
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un equilibrador de carga básica con una configuración de IP de frontend dual (IPv4 e IPv6). También creó un dos máquinas virtuales que se incluyen las NIC con configuraciones de IP duales (IPV4 + IPv6) que se han agregado al grupo de back-end del equilibrador de carga. Para más información sobre la compatibilidad con IPv6 en redes virtuales de Azure, consulte [¿qué es IPv6 para Azure Virtual Network?](ipv6-overview.md)