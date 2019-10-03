---
title: Configuración del equilibrio de carga y las reglas de salida con Azure PowerShell
titlesuffix: Azure Load Balancer
description: En este artículo se muestra cómo configurar el equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure PowerShell.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 6e85d31e40e35b9d796fc66394a6eb446c7302ad
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263059"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-powershell"></a>Configuración del equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure PowerShell

En este artículo se muestra cómo configurar las reglas de salida en Standard Load Balancer con Azure PowerShell.  

Al terminar, el recurso del equilibrador de carga contiene dos servidores front-end y las reglas asociadas a estos: una de entrada y otra de salida.  Cada front-end tiene una referencia a una dirección IP pública, y este escenario usa una dirección IP pública diferente para la entrada y la salida.   La regla de equilibrio de carga proporciona solo equilibrio de carga de entrada y la regla de salida controla el proceso NAT de salida proporcionada para la máquina virtual.  En este artículo se usan dos grupos de back-end independientes, uno para la entrada y otro para salida, a fin de ilustrar las funcionalidades y permitir la flexibilidad para este escenario.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-azure-account"></a>Conectarse a la cuenta de Azure
Inicie sesión en la suscripción a Azure con el comando [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) y siga las instrucciones de la pantalla:
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-resource-group"></a>Creación de un grupo de recursos

Cree un grupo de recursos con [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

En el ejemplo siguiente se crea un grupo de recursos denominado *myresourcegroupoutbound* en la ubicación *eastus2*:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-virtual-network"></a>Creación de una red virtual
Cree una red virtual denominada *myvnetoutbound* con una subred llamada *mysubnetoutbound* en *myresourcegroupoutbound* con [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0) y [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0):

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-inbound-public-ip-address"></a>Creación de una dirección IP pública de entrada 

Para obtener acceso a la aplicación web en Internet, necesita una dirección IP pública para el equilibrador de carga. Un equilibrador de carga estándar solo admite direcciones IP públicas estándar. Use [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) para crear una dirección IP pública estándar denominada *mypublicipinbound* en *myresourcegroupoutbound*.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-outbound-public-ip-address"></a>Creación de una dirección IP pública de salida 

Cree una dirección IP estándar para la configuración de salida del front-end del equilibrador de carga mediante [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0).

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-azure-load-balancer"></a>Creación del equilibrador de carga de Azure

En esta sección se detalla cómo se pueden crear y configurar los componentes siguientes del equilibrador de carga:
  - Una dirección IP de front-end que recibe el tráfico de red entrante en el equilibrador de carga.
  - Un grupo de back-end al que la dirección IP de front-end envía el tráfico de red de carga equilibrada.
  - Un grupo de back-end para la conectividad de salida. 
  - Un sondeo de estado que determina el estado de las instancias de máquina virtual de back-end.
  - Una regla de entrada de equilibrador de carga que define cómo se distribuye el tráfico a las máquinas virtuales.
  - Una regla de salida de equilibrador de carga que define cómo se distribuye el tráfico de las máquinas virtuales.

### <a name="create-inbound-frontend-ip"></a>Creación de una dirección IP de front-end de entrada
Cree la configuración de IP de front-end de salida del equilibrador de carga con [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) que incluya una configuración de IP de front-end de entrada denominada *myfrontendinbound*, es decir que esté asociada a la dirección IP pública *mypublicipinbound*:

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-outbound-frontend-ip"></a>Creación de una dirección IP de front-end de salida
Cree la configuración de IP de front-end de salida del equilibrador de carga con [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) que incluya una configuración de IP de front-end de salida denominada *myfrontendoutbound*, es decir que esté asociada a la dirección IP pública *mypublicipoutbound*:

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-inbound-backend-pool"></a>Creación de un grupo de back-end de entrada
Cree el grupo de back-end de entrada para el equilibrador de carga denominado *bepoolinbound* con [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0):

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-outbound-backend-pool"></a>Creación de un grupo de back-end de salida
Cree un grupo adicional de direcciones de back-end denominado *bepooloutbound* para definir la conectividad de salida para un grupo de máquinas virtuales con [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0). La creación de un grupo de salida independiente proporciona una flexibilidad máxima, pero también puede omitir este paso y usar solo el grupo *bepoolinbound* de entrada.  :

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-health-probe"></a>Creación de un sondeo de estado

Los sondeos de estado comprueban todas las instancias de máquina virtual para asegurarse de que pueden enviar tráfico de red. La instancia de máquina virtual con comprobaciones de sondeo incorrectas se elimina del equilibrador de carga hasta que vuelve a estar en línea y una comprobación de sondeo determina que es correcta. Cree un sondeo de estado con [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0) para supervisar el estado de las máquinas virtuales. 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-load-balancing-rule"></a>Creación de una regla de equilibrio de carga

Una regla de equilibrador de carga define la configuración de la dirección IP de front-end para el tráfico entrante y el grupo de back-end para recibir el tráfico, junto con los puertos de origen y destino requeridos. Cree una regla de equilibrador de carga *myinboundlbrule* con [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0) para escuchar el puerto 80 en el grupo de servidores front-end *myfrontendinbound* y enviar tráfico de red con equilibrio de carga al conjunto de direcciones de back-end *bepoolinbound*, que también usan el puerto 80. 

>[!NOTE]
>Esta regla de equilibrio de carga deshabilita la (S)NAT de salida automática como resultado de esta regla con el parámetro **-DisableOutboundSNAT**. La NAT de salida solo la proporciona la regla de salida.

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

### <a name="create-outbound-rule"></a>Creación de una regla de salida

Una regla de salida define la IP pública de front-end, representada por el front-end *myfrontendoutbound*, que se usará para todo el tráfico NAT de salida, así como el grupo de back-end al que se aplica esta regla.  Cree una regla de salida *myoutboundrule* para la traducción de red de salida de todas las máquinas virtuales (configuraciones de IP de la NIC) en el grupo de back-end *bepool*.  El comando siguiente también cambia el tiempo de espera de inactividad de salida de 4 a 15 minutos y asigna puertos SNAT 10000 en lugar de 1024.  Revise [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0) para más información.

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Si no quiere usar un grupo de salida independiente, puede cambiar el argumento del grupo de direcciones en el comando anterior y especificar *$bepoolin*.  Se recomienda usar grupos independientes para garantizar la flexibilidad y la legibilidad de la configuración resultante.

### <a name="create-load-balancer"></a>Creación de un equilibrador de carga

Use [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0) para crear con la dirección IP de entrada un equilibrador de carga denominado *lb* que incluya una configuración de IP de front-end de entrada y un grupo de back-end *bepoolinbound* que esté asociado a la dirección IP pública *mypublicipinbound* que creó en el paso anterior.

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

En este momento, puede agregar las máquinas virtuales al grupo de back-end *bepoolinbound* __y__ *bepooloutbound* mediante la actualización de la configuración de IP de los recursos de la NIC correspondientes mediante [Add-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede usar el comando [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0) para quitar el grupo de recursos, el equilibrador de carga y todos los recursos relacionados.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Pasos siguientes
En este artículo, creó su equilibrador de carga estándar, configuró ambas reglas de tráfico del equilibrador de carga de entrada, configuró las máquinas virtuales y las sometió a pruebas de sondeo en el grupo de back-end. Para más información acerca de Azure Load Balancer, diríjase a los tutoriales correspondientes.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
