---
title: 'Guía de inicio rápido: dirigir el tráfico web con Azure Application Gateway: Azure PowerShell| Microsoft Docs'
description: Obtenga información sobre cómo utilizar Azure PowerShell para crear una instancia de Azure Application Gateway que dirija el tráfico web a las máquinas virtuales de un grupo de back-end.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 06/11/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: c0e80b1354302f227cb448391c7a92100049cc3a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053341"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Inicio rápido: Dirección del tráfico web con Azure Application Gateway: Azure PowerShell

En este inicio rápido se muestra cómo usar Azure PowerShell para crear rápidamente una puerta de enlace de aplicaciones.  Después de crear la puerta de enlace de aplicaciones, pruébela para asegurarse de que funciona correctamente. Con Azure Application Gateway, puede dirigir el tráfico web de la aplicación a recursos específicos mediante la asignación de agentes de escucha a los puertos, la creación de reglas y la adición de recursos a un grupo de back-end. Para simplificar, en este artículo se usa una configuración sencilla con una dirección IP de front-end pública, un agente de escucha básico para hospedar un único sitio en esta puerta de enlace de aplicaciones, dos máquinas virtuales que se usan con el grupo de back-end y una regla de enrutamiento de solicitudes básica.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-powershell-module"></a>Módulo de Azure PowerShell

Si decide instalar y usar Azure PowerShell localmente, para este tutorial se requiere la versión 1.0.0 del módulo de Azure PowerShell o cualquier versión posterior.

1. Para encontrar la versión, ejecute `Get-Module -ListAvailable Az`. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). 
2. Ejecute `Login-AzAccount` para crear una conexión con Azure.

### <a name="resource-group"></a>Grupos de recursos

En Azure, puede asignar recursos relacionados a un grupo de recursos. Puede usar un grupo de recursos existente o crear uno nuevo. En este ejemplo, creará un grupo de recursos mediante el cmdlet [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup) como se indica a continuación: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

### <a name="required-network-resources"></a>Recursos de red necesarios

Para que Azure se comunique entre los recursos que se crean, se necesita una red virtual.  La subred de la puerta de enlace de aplicaciones solo puede contener puertas de enlace de aplicaciones. No se permite ningún otro recurso.  Puede crear una subred para Application Gateway o usar una existente. En este ejemplo se crean dos subredes: una para la puerta de enlace de aplicaciones y la otra para los servidores back-end. Puede configurar la dirección IP de front-end de Application Gateway para que sea pública o privada, según el caso de uso. En este ejemplo, elegimos una IP de front-end pública.

1. Cree la configuración de subred mediante una llamada a [New-AzVirtualNetworkSubnetConfig](/powershell/module/Az.network/new-Azvirtualnetworksubnetconfig).
2. Cree la red virtual con las configuraciones de la subred mediante una llamada a [New-AzVirtualNetwork](/powershell/module/Az.network/new-Azvirtualnetwork). 
3. Cree la dirección IP pública mediante una llamada a [New-AzPublicIpAddress](/powershell/module/Az.network/new-Azpublicipaddress). 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
### <a name="backend-servers"></a>Servidores back-end

Los servidores back-end pueden constar de NIC, conjuntos de escalado de máquinas virtuales, direcciones IP públicas e internas, nombres de dominio completos (FQDN) y servidores back-end multiinquilino como Azure App Service. En este ejemplo, se crean dos máquinas virtuales que usará Azure como servidores back-end para la puerta de enlace de aplicaciones. También se instala IIS en las máquinas virtuales para comprobar que Azure ha instalado correctamente la puerta de enlace de aplicaciones.

#### <a name="create-two-virtual-machines"></a>Creación de dos máquinas virtuales

1. Cree una interfaz de red con [New-AzNetworkInterface](/powershell/module/Az.network/new-Aznetworkinterface). 
2. Cree una configuración de máquina virtual con [New-AzVMConfig](/powershell/module/Az.compute/new-Azvmconfig).
3. Cree la máquina virtual con [New-AzVM](/powershell/module/Az.compute/new-Azvm).

Al ejecutar el ejemplo de código siguiente para crear las máquinas virtuales, Azure le pide las credenciales. Escriba *azureuser* como nombre de usuario y *Azure123456!* como contraseña:
    
```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostic `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="create-an-application-gateway"></a>Creación de una puerta de enlace de aplicaciones

### <a name="create-the-ip-configurations-and-frontend-port"></a>Creación de las configuraciones IP y el puerto de front-end

1. Use [New-AzApplicationGatewayIPConfiguration](/powershell/module/Az.network/new-Azapplicationgatewayipconfiguration) para crear la configuración que asocia la subred que ha creado con la puerta de enlace de aplicaciones. 
2. Use [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/Az.network/new-Azapplicationgatewayfrontendipconfig) para crear la configuración que asigna la dirección IP pública que ha creado previamente con la puerta de enlace de aplicaciones. 
3. Use [New-AzApplicationGatewayFrontendPort](/powershell/module/Az.network/new-Azapplicationgatewayfrontendport) para asignar el puerto 80 para el acceso a la puerta de enlace de aplicaciones.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool"></a>Creación del grupo de servidores back-end

1. Use [New-AzApplicationGatewayBackendAddressPool](/powershell/module/Az.network/new-Azapplicationgatewaybackendaddresspool) para crear el grupo de servidores back-end para la puerta de enlace de aplicaciones. 
2. Configure los valores del grupo de servidores back-end mediante [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/Az.network/new-Azapplicationgatewaybackendhttpsetting).

```azurepowershell-interactive
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzApplicationGatewayBackendHttpSetting `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Creación del agente de escucha e incorporación de una regla

Azure necesita un agente de escucha para que la puerta de enlace de aplicaciones enrute el tráfico de forma adecuada al grupo de servidores back-end. También necesita una regla para que el agente de escucha sepa qué grupo de servidores back-end se usa para el tráfico entrante. 

1. Cree un agente de escucha mediante [New-AzApplicationGatewayHttpListener](/powershell/module/Az.network/new-Azapplicationgatewayhttplistener) con la configuración de front-end y el puerto de front-end creados anteriormente. 
2. Use [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/Az.network/new-Azapplicationgatewayrequestroutingrule) para crear una regla denominada *rule1*. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

Ahora que ha creado los recursos auxiliares necesarios, cree la puerta de enlace de aplicaciones:

1. Use [New-AzApplicationGatewaySku](/powershell/module/Az.network/new-Azapplicationgatewaysku) para especificar los parámetros para la puerta de enlace de aplicaciones.
2. Use [New-AzApplicationGateway](/powershell/module/Az.network/new-Azapplicationgateway) para crear la puerta de enlace de aplicaciones.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

No es necesario instalar IIS para crear la puerta de enlace de aplicaciones, pero se instaló en este inicio rápido para comprobar si la puerta de enlace de aplicaciones se creó correctamente. Use IIS para probar la puerta de enlace de aplicaciones:

1. Ejecute [Get-AzPublicIPAddress](/powershell/module/Az.network/get-Azpublicipaddress) para obtener la dirección IP pública de la puerta de enlace de aplicaciones. 
2. Copie la dirección IP pública y péguela en la barra de direcciones del explorador. Al actualizar el explorador, verá aparecer el nombre de la máquina virtual. Una respuesta válida corrobora que la puerta de enlace de aplicaciones se ha creado correctamente y puede conectarse correctamente con el back-end.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Prueba de la puerta de enlace de aplicaciones](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con la puerta de enlace de aplicaciones, elimine el grupo de recursos. Mediante la eliminación del grupo de recursos también elimina la puerta de enlace de aplicaciones y todos sus recursos relacionados. 

Para eliminar el grupo de recursos, llame al cmdlet [Remove-AzResourceGroup](/powershell/module/Az.resources/remove-Azresourcegroup) de la siguiente forma:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con una puerta de enlace de aplicaciones mediante Azure PowerShell](./tutorial-manage-web-traffic-powershell.md)

