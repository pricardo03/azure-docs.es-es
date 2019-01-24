---
title: 'Ejemplo de script de Azure PowerShell: creación de un entorno de prueba de Azure Firewall'
description: 'Ejemplo de script de Azure PowerShell: creación de un entorno de prueba de Azure Firewall.'
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 3b55767a4375d41b1dc9c4357ca25e562a3cfabe
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438259"
---
# <a name="create-an-azure-firewall-test-environment"></a>Creación de un entorno de prueba de Azure Firewall

Este script de ejemplo crea un firewall y un entorno de red de prueba. La red tiene una red virtual, con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Las subredes ServersSubnet y JumpboxSubnet tienen un servidor de Windows Server de 2 núcleos en ellas.

El firewall está en AzureFirewallSubnet y se configura con una colección de reglas de aplicación con una única regla que permite el acceso a www.microsoft.com.

Se crea una ruta definida por el usuario que señala el tráfico de red desde ServersSubnet a través del firewall, en donde se aplican las reglas de firewall.

Puede ejecutar el script desde Azure [Cloud Shell](https://shell.azure.com/powershell) o desde una instalación de PowerShell local. 

Si ejecuta PowerShell de forma local, este script requiere la versión más reciente del módulo AzureRM PowerShell (6.9.0 o posterior). Ejecute `Get-Module -ListAvailable AzureRM` para ver cuál es la versión instalada. 

Puede usar `PowerShellGet` si necesita actualizar. Esta versión está integrada en Windows 10 y Windows Server 2016.

> [!NOTE]
>Otras versiones de Windows le pedirán que instale `PowerShellGet` antes de utilizarla. Puede ejecutar `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` para determinar si está instalada en el sistema. Si la salida está en blanco, deberá instalar la versión más reciente de [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Para más información, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.4.0)

Cualquier instalación de Azure PowerShell existente hecha con el instalador de plataforma web entrará en conflicto con la instalación de PowerShellGet y se debe eliminar.

Recuerde que si PowerShell se ejecuta localmente, también deberá ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la VM y todos los recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una red virtual y grupos de seguridad de red. Cada comando de la tabla siguiente crea un vínculo a documentación específica del comando:

| Get-Help | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Crea un objeto de configuración de subred. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Crea una subred de front-end y una red virtual de Azure. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Crea reglas de seguridad que se asignarán a un grupo de seguridad de red. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Crea reglas NSG que permiten o bloquean puertos específicos para subredes concretas. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Asocia NSG a subredes. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Crea una dirección IP pública para acceder a la VM desde Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Crea interfaces de red virtual y las asocia a subredes de front-end y back-end de la red virtual. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Crea una configuración de máquina virtual. Esta configuración incluye diversa información, como el nombre de la máquina virtual, sistema el operativo y las credenciales administrativas. La configuración se utiliza durante la creación de las máquinas virtuales. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cree una máquina virtual. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |
|[New-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewall?view=azurermps-6.9.0)| Crea una nueva instancia de Azure Firewall.|
|[Get-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermfirewall?view=azurermps-6.9.0)|Obtiene un objeto de Azure Firewall.|
|[New-AzureRmFirewallApplicationRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewallapplicationrule?view=azurermps-6.9.0)|Crea una nueva regla de aplicación de Azure Firewall.|
|[Set-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermfirewall?view=azurermps-6.9.0)|Confirma los cambios en el objeto de Azure Firewall.|


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

