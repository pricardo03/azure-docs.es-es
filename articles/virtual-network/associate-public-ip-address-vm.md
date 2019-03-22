---
title: Asociar una dirección IP pública a una máquina virtual
titlesuffix: Azure Virtual Network
description: Obtenga información sobre cómo asociar una dirección IP pública a una máquina virtual.
services: virtual-network
documentationcenter: ''
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: jdial
ms.openlocfilehash: ce573ff8fe61f2e1d4c88963e0f21fc9402776e9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58083222"
---
# <a name="associate-a-public-ip-address-to-a-virtual-machine"></a>Asociar una dirección IP pública a una máquina virtual

En este artículo, obtendrá información sobre cómo asociar una dirección IP pública a una máquina virtual existente (VM). Si desea conectarse a una máquina virtual desde internet, la máquina virtual debe tener una dirección IP pública asociada a él. Si desea crear una nueva máquina virtual con una dirección IP pública, puede hacerlo mediante el [portal Azure](virtual-network-deploy-static-pip-arm-portal.md), el [interfaz de línea de comandos (CLI) Azure](virtual-network-deploy-static-pip-arm-cli.md), o [PowerShell](virtual-network-deploy-static-pip-arm-ps.md). Las direcciones IP públicas tienen un precio simbólico. Para obtener información detallada, consulte [Precios](https://azure.microsoft.com/pricing/details/ip-addresses/). Hay un límite al número de direcciones IP públicas que puede usar por suscripción. Para obtener más información, consulte [límites](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits-1).

Puede usar el [portal Azure](#azure-portal), Azure [interfaz de línea de comandos](#azure-cli) (CLI), o [PowerShell](#powershell) para asociar una dirección IP pública a una máquina virtual.

## <a name="azure-portal"></a>Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a o busque la máquina virtual que desea agregar la dirección IP pública y, a continuación, selecciónelo.
3. En **configuración**, seleccione **redes**y, a continuación, seleccione la interfaz de red que desea agregar la dirección IP pública, como se muestra en la siguiente imagen:

   ![Seleccionar interfaz de red](./media/associate-public-ip-address-vm/select-nic.png)

   > [!NOTE]
   > Las direcciones IP públicas asociadas a las interfaces de red conectadas a una máquina virtual. En la imagen anterior, la máquina virtual solo tiene una interfaz de red. Si la máquina virtual tiene varias interfaces de red, deberían aparecer y seleccionaría la interfaz de red que desea asociar la dirección IP pública.

4. Seleccione **configuraciones de IP** y, a continuación, seleccione una configuración de IP, tal como se muestra en la siguiente imagen:

   ![Seleccione la configuración de IP](./media/associate-public-ip-address-vm/select-ip-configuration.png)

   > [!NOTE]
   > Las direcciones IP públicas asociadas a las configuraciones de IP para una interfaz de red. En la imagen anterior, la interfaz de red tiene una configuración IP. Si la interfaz de red tiene varias configuraciones de IP, deberían aparecer en la lista y seleccionaría la configuración de IP que desea asociar la dirección IP pública.

5. Seleccione **habilitado**, a continuación, seleccione **dirección IP (*configurar los valores obligatorios*)**. Elegir una dirección IP pública existente, que cierra automáticamente el **Elegir dirección IP pública** cuadro. Si no tiene ninguna dirección IP pública disponible enumerada, deberá crear uno. Para obtener información sobre cómo hacerlo, consulte [crear una dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address). Seleccione **guardar**, tal y como se muestra en la imagen que sigue y, a continuación, cierre el cuadro de la configuración de IP.

   ![Habilitar dirección IP pública](./media/associate-public-ip-address-vm/enable-public-ip-address.png)

   > [!NOTE]
   > Las direcciones IP públicas que aparecen son aquellos que existen en la misma región que la máquina virtual. Si tiene varias direcciones IP públicas que creó en la región, todos aparecerán aquí. Si cualquiera está atenuada, es porque la dirección ya está asociada a un recurso diferente.

6. Ver la dirección IP pública asignada a la configuración de IP, como se muestra en la imagen siguiente. Puede tardar unos segundos en aparecer una dirección IP.

   ![Asignada dirección IP pública de vista](./media/associate-public-ip-address-vm/view-assigned-public-ip-address.png)

   > [!NOTE]
   > Se asigna la dirección de un grupo de direcciones se usan en cada región de Azure. Para ver una lista de grupos de direcciones que se usan en cada región, consulte [intervalos de IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). La dirección asignada puede ser cualquier dirección en los grupos utilizados para la región. Si necesita la dirección que se asignará a partir de un grupo específico en la región, use un [prefijo de dirección IP pública](public-ip-address-prefix.md).

7. [Permitir el tráfico de red a la máquina virtual](#allow-network-traffic-to-the-vm) con reglas de seguridad en un grupo de seguridad de red.

## <a name="azure-cli"></a>Azure CLI

Instalar el [CLI de Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), o usar Azure Cloud Shell. Azure Cloud Shell es un shell de Bash gratuito que se puede ejecutar directamente en Azure Portal. Tiene la CLI de Azure preinstalada y configurada para utilizarla con la cuenta. Seleccione el **Pruébelo** botón en la CLI de los comandos siguientes. Seleccionar **Pruébelo** invoca un Cloud Shell que pueda iniciar sesión con su cuenta de Azure.

1. Si usa la CLI localmente en Bash, inicie sesión en Azure con `az login`.
2. Una dirección IP pública está asociada a una configuración IP de una interfaz de red conectada a una máquina virtual. Use la [actualización de az network nic-ip-config](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) comando para asociar una dirección IP pública a una configuración IP. En el ejemplo siguiente se asocia una dirección IP pública existente denominada *myVMPublicIP* a la configuración de IP denominada *ipconfigmyVM* de una interfaz de red denominado *myVMVMNic* que existe un grupo de recursos denominado *myResourceGroup*.
  
   ```azurecli-interactive
   az network nic ip-config update \
     --name ipconfigmyVM \
     --nic-name myVMVMNic \
     --resource-group myResourceGroup \
     --public-ip-address myVMPublicIP
   ```

   - Si no tiene una dirección IP pública existente, use el [crear az network public-ip](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) comando para crear uno. Por ejemplo, el siguiente comando crea una dirección IP pública denominada *myVMPublicIP* en un grupo de recursos denominado *myResourceGroup*.
  
     ```azurecli-interactive
     az network public-ip create --name myVMPublicIP --resource-group myResourceGroup
     ```

     > [!NOTE]
     > El comando anterior crea una dirección IP pública con valores predeterminados para varias configuraciones que desea personalizar. Para obtener más información acerca de todas las configuración de direcciones IP pública, consulte [crear una dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address). Se asigna la dirección de un grupo de direcciones IP públicas utilizadas para cada región de Azure. Para ver una lista de grupos de direcciones que se usan en cada región, consulte [intervalos de IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

   - Si no conoce el nombre de una interfaz de red conectada a la máquina virtual, use el [lista de az vm nic](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-list) comandos para verlos. Por ejemplo, el comando siguiente enumera los nombres de las interfaces de red conectados a una máquina virtual denominada *myVM* en un grupo de recursos denominado *myResourceGroup*:

     ```azurecli-interactive
     az vm nic list --vm-name myVM --resource-group myResourceGroup
     ```

     La salida incluye una o varias líneas que son similares al ejemplo siguiente:
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

     En el ejemplo anterior, *myVMVMNic* es el nombre de la interfaz de red.

   - Si no conoce el nombre de una configuración IP para una interfaz de red, use la [lista de az network nic ip-config](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-list) comando para recuperarlos. Por ejemplo, el comando siguiente enumera los nombres de las configuraciones de IP de una interfaz de red denominada *myVMVMNic* en un grupo de recursos denominado *myResourceGroup*:

     ```azurecli-interactive
     az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
     ```

3. Ver la dirección IP pública asignada a la configuración de IP con el [az vm list-ip-addresses](/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) comando. El ejemplo siguiente muestra las direcciones IP asignadas a una máquina virtual existente denominada *myVM* en un grupo de recursos denominado *myResourceGroup*.

   ```azurecli-interactive
   az vm list-ip-addresses --name myVM --resource-group myResourceGroup --out table
   ```

   > [!NOTE]
   > Se asigna la dirección de un grupo de direcciones se usan en cada región de Azure. Para ver una lista de grupos de direcciones que se usan en cada región, consulte [intervalos de IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). La dirección asignada puede ser cualquier dirección en los grupos utilizados para la región. Si necesita la dirección que se asignará a partir de un grupo específico en la región, use un [prefijo de dirección IP pública](public-ip-address-prefix.md).

4. [Permitir el tráfico de red a la máquina virtual](#allow-network-traffic-to-the-vm) con reglas de seguridad en un grupo de seguridad de red.

## <a name="powershell"></a>PowerShell

Instalar [PowerShell](/powershell/azure/install-az-ps), o usar Azure Cloud Shell. Azure Cloud Shell es un shell gratuito que se ejecuta directamente en Azure Portal. Tiene preinstalado y configurado PowerShell para usarlo con su cuenta. Seleccione el **Pruébelo** botón en PowerShell, los comandos siguientes. Seleccionar **Pruébelo** invoca un Cloud Shell que pueda iniciar sesión con su cuenta de Azure.

1. Si usa PowerShell localmente, inicie sesión en Azure con `Connect-AzAccount`.
2. Una dirección IP pública está asociada a una configuración IP de una interfaz de red conectada a una máquina virtual. Use la [Get AzVirtualNetwork](/powershell/module/Az.Network/Get-AzVirtualNetwork) y [Get AzVirtualNetworkSubnetConfig](/powershell/module/Az.Network/Get-AzVirtualNetworkSubnetConfig) comandos para obtener la red virtual y subred que se encuentra en la interfaz de red. A continuación, use el [Get AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) comando para obtener una interfaz de red y la [Get AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) comando para obtener una dirección IP pública existente. A continuación, utilice el [conjunto AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/Set-AzNetworkInterfaceIpConfig) comando para asociar la dirección IP pública a la configuración IP y el [conjunto AzNetworkInterface](/powershell/module/Az.Network/Set-AzNetworkInterface) comando para escribir la nueva configuración de IP para el interfaz de red.

   En el ejemplo siguiente se asocia una dirección IP pública existente denominada *myVMPublicIP* a la configuración de IP denominada *ipconfigmyVM* de una interfaz de red denominado *myVMVMNic* que existe en una subred denominada *myVMSubnet* en una red virtual denominada *myVMVNet*. Todos los recursos están en un grupo de recursos denominado *myResourceGroup*.
  
   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name myVMVNet -ResourceGroupName myResourceGroup
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name myVMSubnet -VirtualNetwork $vnet
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $pip = Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup
   $nic | Set-AzNetworkInterfaceIpConfig -Name ipconfigmyVM -PublicIPAddress $pip -Subnet $subnet
   $nic | Set-AzNetworkInterface
   ```

   - Si no tiene una dirección IP pública existente, use el [New AzPublicIpAddress](/powershell/module/Az.Network/New-AzPublicIpAddress) comando para crear uno. Por ejemplo, el siguiente comando crea un *dinámica* dirección IP pública denominada *myVMPublicIP* en un grupo de recursos denominado *myResourceGroup* en el  *eastus* región.
  
     ```azurepowershell-interactive
     New-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup -AllocationMethod Dynamic -Location eastus
     ```

     > [!NOTE]
     > El comando anterior crea una dirección IP pública con valores predeterminados para varias configuraciones que desea personalizar. Para obtener más información acerca de todas las configuración de direcciones IP pública, consulte [crear una dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address). Se asigna la dirección de un grupo de direcciones IP públicas utilizadas para cada región de Azure. Para ver una lista de grupos de direcciones que se usan en cada región, consulte [intervalos de IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

   - Si no conoce el nombre de una interfaz de red conectada a la máquina virtual, use el [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) comandos para verlos. Por ejemplo, el comando siguiente enumera los nombres de las interfaces de red conectados a una máquina virtual denominada *myVM* en un grupo de recursos denominado *myResourceGroup*:

     ```azurepowershell-interactive
     $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
     $vm.NetworkProfile
     ```

     La salida incluye una o varias líneas que son similares en el ejemplo siguiente. En la salida del ejemplo *myVMVMNic* es el nombre de la interfaz de red.
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

   - Si no conoce el nombre de la red virtual o subred que se encuentra en la interfaz de red, use el `Get-AzNetworkInterface` comando para ver la información. Por ejemplo, el comando siguiente obtiene la información de red y subred virtual de una interfaz de red denominada *myVMVMNic* en un grupo de recursos denominado *myResourceGroup*:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $ipConfigs = $nic.IpConfigurations
     $ipConfigs.Subnet | Select Id
     ```

     La salida incluye una o varias líneas que son similares en el ejemplo siguiente. En la salida del ejemplo *myVMVNET* es el nombre de la red virtual y *myVMSubnet* es el nombre de la subred.
  
     ```
     "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVMVNET/subnets/myVMSubnet",
     ```

   - Si no conoce el nombre de una configuración IP para una interfaz de red, use la [Get AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) comando para recuperarlos. Por ejemplo, el comando siguiente enumera los nombres de las configuraciones de IP de una interfaz de red denominada *myVMVMNic* en un grupo de recursos denominado *myResourceGroup*:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $nic.IPConfigurations
     ```

     La salida incluye una o varias líneas que son similares en el ejemplo siguiente. En la salida del ejemplo *ipconfigmyVM* es el nombre de una configuración de IP.
  
     ```
     Id     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM
     ```

3. Ver la dirección IP pública asignada a la configuración de IP con el [Get AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) comando. El ejemplo siguiente muestra la dirección asignada a una dirección IP pública denominada *myVMPublicIP* en un grupo de recursos denominado *myResourceGroup*.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup | Select IpAddress
   ```

   Si no conoce el nombre de la dirección IP pública asignada a una configuración IP, ejecute los siguientes comandos para obtenerlo:

   ```azurepowershell-interactive
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $nic.IPConfigurations
   $address = $nic.IPConfigurations.PublicIpAddress
   $address | Select Id
   ```

   La salida incluye una o varias líneas que son similares en el ejemplo siguiente. En la salida del ejemplo *myVMPublicIP* es el nombre de la dirección IP pública asignada a la configuración de IP.

   ```
   "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP"
   ```

   > [!NOTE]
   > Se asigna la dirección de un grupo de direcciones se usan en cada región de Azure. Para ver una lista de grupos de direcciones que se usan en cada región, consulte [intervalos de IP de centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). La dirección asignada puede ser cualquier dirección en los grupos utilizados para la región. Si necesita la dirección que se asignará a partir de un grupo específico en la región, use un [prefijo de dirección IP pública](public-ip-address-prefix.md).

4. [Permitir el tráfico de red a la máquina virtual](#allow-network-traffic-to-the-vm) con reglas de seguridad en un grupo de seguridad de red.

## <a name="allow-network-traffic-to-the-vm"></a>Permitir el tráfico de red a la máquina virtual

Antes de poder conectarse a la dirección IP pública de internet, asegúrese de que tiene los puertos necesarios abiertos en cualquier grupo de seguridad de red que es posible que haya asociado a la interfaz de red, la subred de la interfaz de red o ambos. Aunque la seguridad grupos filtrar el tráfico a la dirección IP privada de la interfaz de red, una vez que el tráfico de internet entrante llega a la dirección IP pública, Azure traduce la dirección pública a la dirección IP privada de direcciones, por lo que si un grupo de seguridad de red impide que el flujo de tráfico, la comunicación con la dirección IP pública se produce un error. Puede ver las reglas de seguridad vigentes para una interfaz de red y su subred utilizando la [Portal](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-portal), [CLI](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-cli), o [PowerShell](diagnose-network-traffic-filter-problem.md#diagnose-using-powershell).

## <a name="next-steps"></a>Pasos siguientes

Permitir el tráfico entrante de internet a la máquina virtual con un grupo de seguridad de red. Para obtener información sobre cómo crear un grupo de seguridad de red, consulte [trabajar con grupos de seguridad de red](manage-network-security-group.md#work-with-network-security-groups). Para más información sobre los grupos de seguridad de red, consulte [grupos de seguridad](security-overview.md).