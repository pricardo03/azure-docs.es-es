---
title: 'Guía de inicio rápido: Creación de una máquina virtual Linux con Azure PowerShell | Microsoft Docs'
description: En esta guía de inicio rápido, aprenderá a usar Azure PowerShell para crear una máquina virtual Linux.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/17/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: df6f99bfe9f1ae7b79f0f382fdee4fe4f1578bad
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407544"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-azure-with-powershell"></a>Guía de inicio rápido: Creación de una máquina virtual Linux en Azure con PowerShell

El módulo de Azure PowerShell se usa para crear y administrar recursos de Azure desde la línea de comandos de PowerShell o en scripts. En esta guía de inicio rápido se muestra cómo usar el módulo de Azure PowerShell para implementar una máquina virtual Linux en Azure. En esta guía de inicio rápido se usa la imagen de Marketplace de Ubuntu 16.04 LTS de Canonical. Para ver la máquina virtual en acción, conéctese a ella mediante SSH e instale el servidor web NGINX.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

Si desea instalar y usar PowerShell de forma local para esta guía necesitará la versión 5.7.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

## <a name="create-ssh-key-pair"></a>Creación del par de claves SSH

Necesita un par de claves SSH para completar esta guía de inicio rápido. Si ya tiene un par de claves SSH, puede omitir este paso.

Abra un shell de Bash y utilice [ssh-keygen](https://www.ssh.com/ssh/keygen/) para crear un par de claves SSH. Si no tiene un shell de Bash en la máquina local, puede usar [Azure Cloud Shell](https://shell.azure.com/bash).  

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```

Para más información sobre cómo crear pares de claves SSH, incluido el uso de PuTTy, consulte [Uso de claves SSH con Windows](ssh-from-windows.md).

Si crea el par de claves SSH mediante Cloud Shell, este se almacenará en una imagen de contenedor de una [cuenta de almacenamiento que Cloud Shell crea automáticamente](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage). No elimine la cuenta de almacenamiento ni el recurso compartido de archivos que contiene hasta que haya recuperado las claves o perderá el acceso a la máquina virtual. 

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Un grupo de recursos es un contenedor lógico en el que se implementan y administran los recursos de Azure.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-virtual-network-resources"></a>Creación de recursos de virtual 

Cree una red virtual, una subred, una dirección IP pública. Estos recursos se utilizan para proporcionar conectividad de red con la máquina virtual y conectarla a Internet:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -Name "myVNET" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

Cree una regla de tráfico y de grupo de seguridad de red de Azure. El grupo de seguridad de red protege la máquina virtual con reglas de entrada y de salida. En el ejemplo siguiente, se crea una regla de entrada para el puerto TCP 22 que permite conexiones SSH. Para permitir el tráfico web de entrada, también se crea una regla de entrada para el puerto TCP 80.

```azurepowershell-interactive
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleSSH"  `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 22 `
  -Access "Allow"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWWW"  `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access "Allow"

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -Name "myNetworkSecurityGroup" `
  -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Cree una tarjeta de interfaz de red (NIC) virtual con [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface): La NIC virtual conecta la máquina virtual a una subred, un grupo de seguridad de red y una dirección IP pública.

```azurepowershell-interactive
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name "myNic" `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Para crear una máquina virtual en PowerShell, cree una configuración que tenga valores como, por ejemplo, la imagen que se va a usar, el tamaño y las opciones de autenticación. Después, la configuración se utiliza para crear la máquina virtual.

Defina las credenciales de SSH, la información del sistema operativo y el tamaño de máquina virtual. En este ejemplo, la clave SSH se almacena en `~/.ssh/id_rsa.pub`. 

```azurepowershell-interactive
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig `
  -VMName "myVM" `
  -VMSize "Standard_D1" | `
Set-AzureRmVMOperatingSystem `
  -Linux `
  -ComputerName "myVM" `
  -Credential $cred `
  -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest" | `
Add-AzureRmVMNetworkInterface `
  -Id $nic.Id

# Configure the SSH key
$sshPublicKey = cat ~/.ssh/id_rsa.pub
Add-AzureRmVMSshPublicKey `
  -VM $vmconfig `
  -KeyData $sshPublicKey `
  -Path "/home/azureuser/.ssh/authorized_keys"
```

Ahora, combine las definiciones de configuración anteriores para crearlas con [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
New-AzureRmVM `
  -ResourceGroupName "myResourceGroup" `
  -Location eastus -VM $vmConfig
```

La implementación de la máquina virtual tardará unos minutos. Cuando finalice la implementación, pase a la siguiente sección.

## <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

Cree una conexión SSH con la máquina virtual mediante la dirección IP pública. Para ver la dirección IP pública de la máquina virtual, use el cmdlet [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress):

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

Con el mismo shell de Bash que usó para crear el par de claves SSH (como [Azure Cloud Shell](https://shell.azure.com/bash) o el shell de Bash local) pegue el comando de conexión de SSH en el shell para crear una sesión de SSH.

```bash
ssh azureuser@10.111.12.123
```

Cuando se le solicite, el nombre de usuario de inicio de sesión es *azureuser*. Si se usa una frase de contraseña con las claves SSH, debe especificarla cuando se le solicite.


## <a name="install-nginx"></a>Instalación de NGINX

Para ver la máquina virtual en acción, instale al servidor de web de NGINX. En la sesión de SSH, actualice los orígenes de paquete e instale el paquete NGINX más reciente.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

Cuando haya terminado, escriba `exit` para salir de la sesión de SSH.


## <a name="view-the-web-server-in-action"></a>Visualización del servidor web en acción

Use un explorador web de su elección para ver la página de bienvenida predeterminada de NGINX. Escriba la dirección IP pública de la máquina virtual como dirección web. La dirección IP pública se encuentra en la página de introducción de la máquina virtual o como parte de la cadena de conexión de SSH que usó anteriormente.

![Sitio NGINX predeterminado](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede usar el cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, implementó una máquina virtual sencilla, creó un grupo de seguridad de red y una regla e instaló un servidor web básico. Para más información acerca de las máquinas virtuales de Azure, continúe con el tutorial de máquinas virtuales Linux.

> [!div class="nextstepaction"]
> [Tutoriales de máquinas virtuales Linux de Azure](./tutorial-manage-vm.md)
