---
title: 'Tutorial: Creación y prueba de una puerta de enlace de NAT: Azure PowerShell'
titlesuffix: Azure Virtual Network NAT
description: En este tutorial se muestra cómo crear una puerta de enlace de NAT mediante Azure PowerShell y cómo probar el servicio NAT.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to test a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: e0fb78be363795bbb09fb0f14bd018fceea34a48
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897894"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-powershell-and-test-the-nat-service"></a>Tutorial: Creación de una puerta de enlace de NAT mediante Azure PowerShell y prueba del servicio NAT

En este tutorial, creará una puerta de enlace de NAT para proporcionar conectividad saliente a las máquinas virtuales de Azure. Para probar la puerta de enlace de NAT, implemente una máquina virtual de origen y destino. Luego, realizará conexiones salientes a una dirección IP pública. Estas conexiones irán desde la máquina virtual de origen a la de destino. Para simplificar las cosas, en este tutorial se implementan el origen y el destino en dos redes virtuales diferentes del mismo grupo de recursos.

>[!NOTE] 
>NAT de Azure Virtual Network está disponible como versión preliminar pública en este momento y en un conjunto limitado de [regiones](./nat-overview.md#region-availability). Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Puede realizar este tutorial con Azure Cloud Shell o ejecutar los comandos respectivos de forma local.  Si no ha usado nunca Azure Cloud Shell, debe [iniciar sesión ahora](https://shell.azure.com).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con [az group create](https://docs.microsoft.com/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

En el ejemplo siguiente se crea un grupo de recursos llamado **myResourceGroupNAT** en la ubicación **eastus2**:


```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzResourceGroup -Name $rsg -Location $loc
```

## <a name="create-the-nat-gateway"></a>Creación de la puerta de enlace de NAT

### <a name="create-a-public-ip-address"></a>Crear una dirección IP pública

Para acceder a Internet, necesita una o varias direcciones IP públicas para la puerta de enlace de NAT. Use [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) para crear el recurso de dirección IP pública llamado **myPublicIPsource** en **myResourceGroupNAT**. El resultado de este comando se almacenará en una variable llamada **$publicIPsource** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pips = 'myPublicIPsource'
$alm = 'Static'
$sku = 'Standard'

$publicIPsource = 
New-AzPublicIpAddress -Name $pips -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -Sku $sku
```

### <a name="create-a-public-ip-prefix"></a>Creación de un prefijo de dirección IP pública

 Use [New-AzPublicIpPrefix](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipprefix?view=latest) para crear un recurso de prefijo de dirección IP pública llamado **myPublicIPprefixsource** en **myResourceGroupNAT**.  El resultado de este comando se almacenará en una variable llamada **$publicIPPrefixsource** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$prips = 'mypublicIPprefixsource'

$publicIPPrefixsource = 
New-AzPublicIpPrefix -Name $prips -ResourceGroupName $rsg -Location $loc -PrefixLength 31
```

### <a name="create-a-nat-gateway-resource"></a>Creación de un recurso de puerta de enlace de NAT

En esta sección se detalla cómo crear y configurar los componentes siguientes del servicio NAT mediante el recurso de puerta de enlace de NAT:
  - Un grupo de direcciones IP públicas y un prefijo de dirección IP pública que se usarán en los flujos salientes traducidos por el recurso de puerta de enlace de NAT.
  - Cambie el tiempo de espera de inactividad de 4 (el valor predeterminado) a 10 minutos.

Cree una puerta de enlace de NAT de Azure global con [New-AzNatGateway](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway). El resultado de este comando creará un recurso de puerta de enlace llamado **myNATgateway** que usa la dirección IP pública **myPublicIPsource** y el prefijo de dirección IP pública **myPublicIPprefixsource**. El tiempo de espera de inactividad se establece en 10 minutos.  El resultado de este comando se almacenará en una variable llamada **$natGateway** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$nnm = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $nnm -ResourceGroupName $rsg -PublicIpAddress $publicIPsource -PublicIpPrefix $publicIPPrefixsource -Location $loc -Sku $sku -IdleTimeoutInMinutes 10      
  ```

En este momento, la puerta de enlace de NAT es funcional y todo lo que queda es configurar qué subredes de una red virtual deben usarla.

## <a name="prepare-the-source-for-outbound-traffic"></a>Preparación del origen para el tráfico saliente

Se le va a guiar por la configuración de un entorno de prueba completo. Configurará una prueba mediante herramientas de código abierto para comprobar la puerta de enlace de NAT. Se comenzará con el origen, que usará la puerta de enlace de NAT que creamos anteriormente.

### <a name="configure-virtual-network-for-source"></a>Configuración de una red virtual para el origen

Cree la red virtual y asocie la subred a la puerta de enlace.

Cree una red virtual llamada **myVnetsource** con una subred llamada **mySubnetsource** con [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) en **myResourceGroupNAT** usando [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest). El espacio de direcciones IP de la red virtual es **192.168.0.0/16**. La subred de la red virtual es **192.168.0.0/24**.  El resultado de los comandos se almacenará en las variables llamadas **$subnetsource** y **$Vnetsource** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$sbn = 'mySubnetsource'
$spfx = '192.168.0.0/24'
$vnm = 'myVnetsource'
$vpfx = '192.168.0.0/16'
$loc = 'eastus2'

$subnetsource = 
New-AzVirtualNetworkSubnetConfig -Name $sbn -AddressPrefix $spfx -NatGateway $natGateway

$vnetsource = 
New-AzVirtualNetwork -Name $vnm -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnet
```

Todo el tráfico saliente a destinos de Internet usa ahora el servicio NAT.  No es necesario configurar una UDR.

Antes de poder probar la puerta de enlace de NAT, es necesario crear una máquina virtual de origen.  Se va a asignar un recurso de dirección IP pública como una dirección IP pública de nivel de instancia para acceder a esta máquina virtual desde el exterior. Esta dirección solo se usa para el acceso de prueba.  Se va a demostrar cómo el servicio NAT tiene prioridad sobre otras opciones de salida.

Como ejercicio, también puede crear esta máquina virtual sin una dirección IP pública y crear otra máquina virtual para usarla como JumpBox sin una dirección IP pública.

### <a name="create-public-ip-for-source-vm"></a>Creación de una dirección IP pública para la máquina virtual de origen

Se va a crear una dirección IP pública que se usará para acceder a la máquina virtual.  Use [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) para crear el recurso de dirección IP pública llamado **myPublicIPVM** en **myResourceGroupNAT**.  El resultado de este comando se almacenará en una variable llamada **$publicIpsourceVM** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$pipvm = 'myPublicIpsourceVM'
$alm = 'Static'

$publicIpsourceVM = 
New-AzPublicIpAddress -Name $pipvm -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>Creación de un grupo de seguridad de red y exposición del punto de conexión de SSH para la máquina virtual

Como las direcciones IP públicas estándar son "seguras de forma predeterminada", se debe crear un grupo de seguridad de red para permitir el acceso de entrada mediante SSH. El servicio NAT depende de la dirección del flujo. Este grupo de seguridad de red no se usará para la salida una vez que la puerta de enlace de NAT esté configurada en la misma subred. Use [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) para crear un recurso de grupo de seguridad de red llamado **myNSGsource**. Use [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) para crear una regla de grupo de seguridad de red para el acceso mediante SSH llamada **ssh** en **myResourceGroupNAT**. El resultado de este comando se almacenará en una variable llamada **$nsgsource** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$rnm = 'ssh'
$rdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$nsnm = 'myNSGsource'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $rdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$nsgsource = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule 
```

### <a name="create-nic-for-source-vm"></a>Creación de una NIC para la máquina virtual de origen

Cree una interfaz de red con [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) llamada **myNicsource**. Este comando asociará la dirección IP pública y el grupo de seguridad de red. El resultado de este comando se almacenará en una variable llamada **$nicsource** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nin = 'myNicsource'

$nicsource = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nin -NetworkSecurityGroupID $nsgsource.Id -PublicIPAddressID $publicIPVMsource.Id -SubnetID $vnetsource.Subnets[0].Id -Location $loc
```

### <a name="create-a-source-vm"></a>Creación de una máquina virtual de origen

#### <a name="create-ssh-key-pair"></a>Creación del par de claves SSH

Necesita un par de claves SSH para completar esta guía de inicio rápido. Si ya tiene un par de claves SSH, puede omitir este paso.

Use ssh-keygen para crear un par de claves SSH.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```
Para más información sobre cómo crear pares de claves SSH, incluido el uso de PuTTy, consulte [Uso de claves SSH con Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

Si crea el par de claves SSH mediante Cloud Shell, se almacenará en una imagen de contenedor. Esta [cuenta de almacenamiento se crea automáticamente](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage). No elimine la cuenta de almacenamiento, ni el recurso compartido de archivos que contiene, hasta que haya recuperado las claves.

#### <a name="create-vm-configuration"></a>Creación de una configuración de máquina virtual

Para crear una máquina virtual en PowerShell, cree una configuración que tenga valores como, por ejemplo, la imagen que se va a usar, el tamaño y las opciones de autenticación. Después, la configuración se utiliza para crear la máquina virtual.

Defina las credenciales de SSH, la información del sistema operativo y el tamaño de máquina virtual. En este ejemplo, la clave SSH se almacena en ~/.ssh/id_rsa.pub.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmn = 'myVMsource'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigsource = 
New-AzVMConfig -VMName $vmn -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigsource -Linux -ComputerName $vmn -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigsource -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigsource -Id $nicsource.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigsource -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
Combine las definiciones de configuración para crear una máquina virtual llamada **myVMsource** con [New-AzVM](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) en **myResourceGroupNAT**.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigsource
```

Aunque el comando devolverá resultados inmediatamente, la máquina virtual puede tardar unos minutos en implementarse.

## <a name="prepare-destination-for-outbound-traffic"></a>Preparación del destino para el tráfico saliente

Ahora se va a crear un destino para el tráfico saliente traducido por el servicio NAT para que pueda probarlo.

### <a name="configure-virtual-network-for-destination"></a>Configuración de una red virtual para el destino

Es necesario crear una red virtual donde estará la máquina virtual de destino.  Estos comandos son los mismos que para la máquina virtual de origen. Se han agregado pequeños cambios para exponer el punto de conexión de destino.

Cree una red virtual llamada **myVnetdestination** con una subred llamada **mySubnetdestination** con [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) en **myResourceGroupNAT** usando [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest). El espacio de direcciones IP de la red virtual es **192.168.0.0/16**. La subred de la red virtual es **192.168.0.0/24**.  El resultado de los comandos se almacenará en las variables llamadas **$subnetdestination** y **$Vnetdestination** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sbdn = 'mySubnetdestination'
$spfx = '192.168.0.0/24'
$vdn = 'myVnetdestination'
$vpfx = '192.168.0.0/16'

$subnetdestination = 
New-AzVirtualNetworkSubnetConfig -Name $sbdn -AddressPrefix $spfx

$vnetdestination = 
New-AzVirtualNetwork -Name $vdn -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnetdestination
```

### <a name="create-public-ip-for-destination-vm"></a>Creación de una dirección IP pública para la máquina virtual de destino

Se va a crear una dirección IP pública que se usará para acceder a la máquina virtual de destino.  Use [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) para crear el recurso de dirección IP pública llamado **myPublicIPdestinationVM** en **myResourceGroupNAT**.  El resultado de este comando se almacenará en una variable llamada **$publicIpdestinationVM** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$all = 'Static'
$pipd = 'myPublicIPdestinationVM'

$publicIpdestinationVM = 
New-AzPublicIpAddress -Name $pipd -ResourceGroupName $rsg -AllocationMethod $all -Location $loc -Sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-and-http-endpoint-for-vm"></a>Creación de un grupo de seguridad de red y exposición del punto de conexión SSH y HTTP para la máquina virtual

Las direcciones IP públicas estándar son "seguras de forma predeterminada", así que se debe crear un grupo de seguridad de red para permitir el acceso de entrada mediante SSH. Use [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) para crear un recurso de grupo de seguridad de red llamado **myNSGdestination**. Use [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) para crear una regla de grupo de seguridad de red para el acceso mediante SSH llamada **ssh**.  Use [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) para crear una regla de grupo de seguridad de red para el acceso mediante HTTP llamada **http**. Ambas reglas se crearán en **myResourceGroupNAT**. El resultado de este comando se almacenará en una variable llamada **$nsgdestination** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$snm = 'ssh'
$sdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$hnm = 'http'
$hdsc = 'HTTP access'
$nsnm = 'myNSGdestination'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $snm -Description $sdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$httprule = 
New-AzNetworkSecurityRuleConfig -Name $hnm -Description $hdsc -Access $acc -Protocol $prt -Direction $dir -Priority 101 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$nsgdestination = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule,$httprule
```

### <a name="create-nic-for-destination-vm"></a>Creación de una NIC para la máquina virtual de destino

Cree una interfaz de red con [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) llamada **myNicdestination**. Este comando se asociará con la dirección IP pública y el grupo de seguridad de red. El resultado de este comando se almacenará en una variable llamada **$nicdestination** para su uso posterior.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nnm = 'myNicdestination'

$nicdestination = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nnm -NetworkSecurityGroupID $nsgdestination.Id -PublicIPAddressID $publicIPdestinationVM.Id -SubnetID $vnetdestination.Subnets[0].Id -Location $loc
```

### <a name="create-a-destination-vm"></a>Creación de una máquina virtual de destino

#### <a name="create-vm-configuration"></a>Creación de una configuración de máquina virtual

Para crear una máquina virtual en PowerShell, cree una configuración que tenga valores como, por ejemplo, la imagen que se va a usar, el tamaño y las opciones de autenticación. Después, la configuración se utiliza para crear la máquina virtual.

Defina las credenciales de SSH, la información del sistema operativo y el tamaño de máquina virtual. En este ejemplo, la clave SSH se almacena en ~/.ssh/id_rsa.pub.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration

$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$vmd = 'myVMdestination'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigdestination = New-AzVMConfig -VMName $vmd -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigdestination -Linux -ComputerName $vmd -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigdestination -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigdestination -Id $nicdestination.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigdestination -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
Combine las definiciones de configuración para crear una máquina virtual llamada **myVMdestination** con [New-AzVM](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) en **myResourceGroupNAT**.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigdestination
```

Aunque el comando devolverá resultados inmediatamente, la máquina virtual puede tardar unos minutos en implementarse.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Preparación de un servidor web y prueba de la carga en la máquina virtual de destino

En primer lugar, es necesario detectar la dirección IP de la máquina virtual de destino.  Para obtener la dirección IP pública de la máquina virtual, use [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest). 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPdestinationVM'
  
Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Copie la dirección IP pública y, luego, péguela en un bloc de notas para que pueda usarla en pasos posteriores. Indique que esta es la máquina virtual de destino.

### <a name="sign-in-to-destination-vm"></a>Inicio de sesión en la máquina virtual de destino

Las credenciales de SSH deben almacenarse en la instancia de Cloud Shell de la operación anterior.  Abra [Azure Cloud Shell](https://shell.azure.com) en el explorador. Use la dirección IP recuperada en el paso anterior para conectarse a la máquina virtual mediante SSH. 

```bash
ssh azureuser@<ip-address-destination>
```

Una vez que haya iniciado sesión, copie y pegue los siguientes comandos.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Estos comandos actualizarán la máquina virtual, instalarán Nginx y crearán un archivo de 100 Kbytes. Este archivo se recuperará de la máquina virtual de origen mediante el servicio NAT.

Cierre la sesión SSH con la máquina virtual de destino.

## <a name="prepare-test-on-source-vm"></a>Preparación de la prueba en la máquina virtual de origen

En primer lugar, es necesario detectar la dirección IP de la máquina virtual de origen.  Para obtener la dirección IP pública de la máquina virtual, use [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest). 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPsourceVM'

Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Copie la dirección IP pública y, luego, péguela en un bloc de notas para que pueda usarla en pasos posteriores. Indique que esta es la máquina virtual de origen.

### <a name="log-into-source-vm"></a>Inicio de sesión en la máquina virtual de origen

De nuevo, las credenciales de SSH se almacenan en Cloud Shell. Abra una nueva pestaña de [Azure Cloud Shell](https://shell.azure.com) en el explorador.  Use la dirección IP recuperada en el paso anterior para conectarse a la máquina virtual mediante SSH. 

```bash
ssh azureuser@<ip-address-source>
```

Copie y pegue los siguientes comandos para preparar la prueba del servicio NAT.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Estos comandos actualizarán la máquina virtual, instalarán Go, instalarán [Hey](https://github.com/rakyll/hey) desde GitHub y actualizarán el entorno de Shell.

Ahora está listo para probar el servicio NAT.

## <a name="validate-nat-service"></a>Validación del servicio NAT

Mientras ha iniciado sesión en la máquina virtual de origen, puede usar **cURL** y **Hey** para generar solicitudes a la dirección IP de destino.

Use cURL para recuperar el archivo de 100 Kbytes.  Reemplace **\<ip-address-destination>** en el ejemplo siguiente por la dirección IP de destino que ha copiado anteriormente.  El parámetro **--output** indica que se descartará el archivo recuperado.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

También puede generar una serie de solicitudes mediante **Hey**. De nuevo, reemplace **\<ip-address-destination>** por la dirección IP de destino que ha copiado anteriormente.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Este comando generará 100 solicitudes, 10 de ellas a la vez, con un tiempo de espera de 30 segundos. La conexión TCP no se volverá a usar.  Cada solicitud recuperará 100 Kbytes.  Al final de la ejecución, **Hey** notificará algunas estadísticas sobre el rendimiento del servicio NAT.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar el comando [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) para quitar el grupo de recursos y todos los recursos que contiene.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado una puerta de enlace de NAT y una máquina virtual de origen y destino y, luego, ha probado la puerta de enlace de NAT.

Revise las métricas de Azure Monitor para ver el funcionamiento del servicio NAT. Diagnostique problemas, como el agotamiento de recursos de los puertos SNAT disponibles.  El agotamiento de los recursos de los puertos SNAT se soluciona fácilmente agregando recursos de dirección IP pública o recursos de prefijo de dirección IP pública adicionales (o ambos).

- Obtenga más información sobre [Virtual Network NAT](./nat-overview.md).
- Obtenga más información sobre [recursos de puerta de enlace de NAT](./nat-gateway-resource.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con la CLI de Azure](./quickstart-create-nat-gateway-cli.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con PowerShell](./quickstart-create-nat-gateway-powershell.md).
- Inicio rápido para la implementación de [recursos de puerta de enlace de NAT con Azure Portal](./quickstart-create-nat-gateway-portal.md).
- [Proporcione comentarios sobre la versión preliminar pública](https://aka.ms/natfeedback).

> [!div class="nextstepaction"]

