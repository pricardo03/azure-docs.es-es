---
title: 'Ejemplo de script de Azure PowerShell: creación de un entorno de prueba de Azure Firewall'
description: 'Ejemplo de script de Azure PowerShell: creación de un entorno de prueba de Azure Firewall.'
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: ffc9d3c15f045079585ea2aeceab278cf0349041
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991969"
---
# <a name="create-an-azure-firewall-test-environment"></a>Creación de un entorno de prueba de Azure Firewall

[!INCLUDE [firewall-preview-notice](../../../includes/firewall-preview-notice.md)]

En los ejemplos de los artículos de Azure Firewall se supone que ya tiene habilitada la versión preliminar pública de Azure Firewall. Para más información, consulte [Enable the Azure Firewall public preview](../public-preview.md) (Habilitar la versión preliminar pública de Azure Firewall).

Este script de ejemplo crea un firewall y un entorno de red de prueba. La red tiene una red virtual, con tres subredes: *AzureFirewallSubnet*, *ServersSubnet* y *JumpboxSubnet*. Las subredes ServersSubnet y JumpboxSubnet tienen un servidor de Windows Server de 2 núcleos en ellas.

El firewall está en AzureFirewallSubnet y se configura con una colección de reglas de aplicación con una única regla que permite el acceso a www.microsoft.com.

Se crea una ruta definida por el usuario que señala el tráfico de red desde ServersSubnet a través del firewall, en donde se aplican las reglas de firewall.

Puede ejecutar el script desde Azure [Cloud Shell](https://shell.azure.com/powershell) o desde una instalación de PowerShell local. 

Si ejecuta PowerShell de forma local, este script requiere la versión del módulo de AzureRM PowerShell 6.4.0 o posterior. Ejecute `Get-Module -ListAvailable AzureRM` para ver cuál es la versión instalada. 

Puede usar `PowerShellGet` si necesita actualizar. Esta versión está integrada en Windows 10 y Windows Server 2016.

> [!NOTE]
>Otras versiones de Windows le pedirán que instale `PowerShellGet` antes de utilizarla. Puede ejecutar `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` para determinar si está instalada en el sistema. Si la salida está en blanco, deberá instalar la versión más reciente de [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Para más información, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0)

Cualquier instalación de Azure PowerShell existente hecha con el instalador de plataforma web entrará en conflicto con la instalación de PowerShellGet y se debe eliminar.

Además, debe instalar la versión preliminar de AzureRM.Network (versión 6.4.0). Si tiene un módulo anterior, ejecute `Uninstall-Module AzureRM.Network -Force` para eliminarlo. A continuación, ejecute:

 `Install-Module -Name AzureRM.Network -Repository PSGallery -RequiredVersion 6.4.0-preview -AllowPrerelease -Force`

para instalar la versión 6.4.0.

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

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

