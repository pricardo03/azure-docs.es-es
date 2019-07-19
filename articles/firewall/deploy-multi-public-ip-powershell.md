---
title: Implementación de Azure Firewall con varias direcciones IP públicas mediante Azure PowerShell
description: En este artículo, aprenderá a implementar una instancia de Azure Firewall con varias direcciones IP públicas mediante Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/2/2019
ms.author: victorh
ms.openlocfilehash: a5a53766df3338bb36913b589ebda970de55ec94
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491930"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Implementación de una instancia de Azure Firewall con varias direcciones IP públicas mediante Azure PowerShell

> [!IMPORTANT]
> Azure Firewall con varias direcciones IP públicas está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede implementar una instancia de Azure Firewall con hasta 100 direcciones IP públicas.

Esta característica permite los siguientes escenarios:

- **DNAT**: puede traducir varias instancias de puerto estándar para los servidores back-end. Por ejemplo, si tiene dos direcciones IP públicas, puede traducir el puerto TCP 3389 (RDP) para ambas direcciones IP.
- **SNAT**: hay disponibles puertos adicionales para las conexiones SNAT salientes, lo que reduce la posibilidad de que se agoten los puertos SNAT. En este momento, Azure Firewall selecciona aleatoriamente la dirección IP pública de origen que se usará para una conexión. Si dispone de algún filtro de nivel inferior de la red, deberá permitir todas las direcciones IP públicas asociadas con el firewall.

Los ejemplos de Azure PowerShell siguiente muestran cómo puede agregar, quitar y configurar direcciones IP públicas para Azure Firewall.

> [!NOTE]
> Durante la versión preliminar pública, si agrega o quita una dirección IP pública en un firewall en ejecución, la conectividad de entrada existente con las reglas DNAT podría no funcionar durante entre 40 y 120 segundos. No puede quitar la primera dirección IP pública asignada al firewall, a menos que este se desasigne o se elimine.

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Incorporación de una dirección IP pública en un firewall existente

En este ejemplo, la dirección IP pública *azFwPublicIp1* se asocia al firewall.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Eliminación de una dirección IP pública de un firewall existente

En este ejemplo, la dirección IP pública *azFwPublicIp1* se desasocia del firewall.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>Creación de un firewall con dos o más direcciones IP públicas

Este ejemplo crea un firewall asociado a una red virtual *vnet* con dos direcciones IP públicas.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
