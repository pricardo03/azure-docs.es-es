---
ms.assetid: ''
title: Configurar firewalls y redes virtuales de Azure Key Vault
description: Instrucciones detalladas paso a paso para configurar los firewalls y las redes virtuales de Azule Key Vault
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/31/2018
ms.author: ambapat
ms.openlocfilehash: c58fc56742c0a11771bdd84e4195e6f476622a3b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346777"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Configurar firewalls y redes virtuales de Azure Key Vault

En esta guía se describen las instrucciones paso a paso para configurar los firewalls y las redes virtuales de Key Vault para restringir el acceso al almacén de claves. Los [puntos de conexión de servicio de red virtual para Key Vault](key-vault-overview-vnet-service-endpoints.md) le permiten restringir el acceso a Key Vault a la red virtual especificada y a una serie de rangos de direcciones IPv4 (versión 4 del protocolo de Internet).

> [!IMPORTANT]
> Una vez que las reglas del firewall y la red virtual están en vigor, todas las operaciones del [plano de datos](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) de Key Vault SOLAMENTE se pueden realizar cuando las solicitudes del autor de la llamada se originan desde redes virtuales o rangos de direcciones IPV4 permitidos. Esto también se aplica al acceso al almacén de claves desde Azure Portal. Aunque un usuario puede ir a un almacén de claves desde Azure Portal, es posible que no pueda enumerar las claves, secretos o certificados si su máquina cliente no está en la lista de dispositivos permitidos. Esto también afecta al "selector de Key Vault" de otros servicios de Azure. Los usuarios pueden ver la lista de almacenes de claves, pero no pueden enumerar las claves si las reglas del firewall limitan su máquina cliente.

## <a name="azure-portal"></a>Azure Portal

1. Vaya al almacén de claves que quiere proteger.
2. Haga clic en **Firewalls y redes virtuales**.
3. Haga clic en **Redes seleccionadas** en **Permitir el acceso desde**.
4. Para agregar redes virtuales existentes a las reglas de firewall y de red virtual, haga clic en **+ Agregar redes virtuales existentes**.
5. En la nueva hoja que aparecerá, seleccione la suscripción, las redes virtuales y subredes a las cuales desea otorgar acceso a este almacén de claves. Si las redes virtuales y subredes que seleccione no tienen puntos de conexión de servicio habilitados, verá un mensaje que indica "Las siguientes redes no tienen habilitados los puntos de conexión de servicio...". Haga clic en **Habilitar** después de confirmar que quiere habilitar los puntos de conexión de servicio para la lista las redes virtuales y subredes. Esta operación puede tardar hasta 15 minutos en surtir efecto.
6. Asimismo, también puede también agregar nuevas redes virtuales y subredes y habilitar los puntos de conexión de servicio para las redes virtuales y subredes recién creadas; para ello, haga clic en **+ Agregar nueva red virtual** y siga las indicaciones.
7. En **Redes IP**, puede agregar rangos de direcciones IPv4; para ello, escriba los rangos de direcciones IPv4 en la [notación CIDR (enrutamiento de interdominios sin clases)](https://tools.ietf.org/html/rfc4632) o en las direcciones IP individuales.
8. Haga clic en **Save**(Guardar).

## <a name="azure-cli-20"></a>CLI de Azure 2.0

1. [Instale la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) e [inicie sesión](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Enumere las reglas de red virtual disponibles; si no ha establecido ninguna regla para este almacén de claves, la lista estará vacía.
```azurecli
az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
```

3. Habilite el punto de conexión de servicio para Key Vault en una red virtual y subred existentes.
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
```

4. Agregue una regla de red para una red virtual y subred.
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
```

5. Agregue un rango de direcciones IP para permitir el tráfico.
```azurecli
az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
```

6. Si cualquier servicio de confianza necesita obtener acceso a este almacén de claves, establezca la opción "Omitir" en Azure Services.
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
```

7. A continuación, el importante paso final: active las reglas de red estableciendo la acción predeterminada en "Denegar".
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
```

## <a name="azure-powershell"></a>Azure PowerShell

1. Instale la última versión de [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [inicie sesión](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Enumere las reglas de red virtual disponibles; si no ha establecido ninguna regla para este almacén de claves, la lista estará vacía.
```PowerShell
(Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
```

3. Habilite el punto de conexión de servicio para Key Vault en una red virtual y subred existentes.
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
```

4. Agregue una regla de red para una red virtual y subred.
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
```

5. Agregue un rango de direcciones IP para permitir el tráfico.
```PowerShell
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
```

6. Si cualquier servicio de confianza necesita obtener acceso a este almacén de claves, establezca la opción "Omitir" en Azure Services.
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
```

7. A continuación, el importante paso final: active las reglas de red estableciendo la acción predeterminada en "Denegar".
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
```

## <a name="references"></a>Referencias

* Comandos de la CLI 2.0 de Azure: [az keyvault network-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Cmdlets de Azure PowerShell: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Add-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [Update-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Pasos siguientes

* [Puntos de conexión de servicio de red virtual para Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Protección de Key Vault](key-vault-secure-your-key-vault.md)