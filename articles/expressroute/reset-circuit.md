---
title: 'Restablecimiento de un circuito con errores: ExpressRoute: PowerShell: Azure | Microsoft Docs'
description: Este artículo le ayudará a restablecer un circuito de ExpressRoute que se encuentra en un estado con errores.
services: expressroute
author: anzaman
ms.service: expressroute
ms.topic: article
ms.date: 11/28/2018
ms.author: anzaman
ms.custom: seodec18
ms.openlocfilehash: 7b88ba6e00cbec05263fe5bc8e795cda95beee04
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093682"
---
# <a name="reset-a-failed-expressroute-circuit"></a>Restablecimiento de un circuito de ExpressRoute con errores

Cuando una operación de un circuito de ExpressRoute no se completa correctamente, el circuito puede entrar en un estado "con errores". Este artículo le ayudará a restablecer un circuito de Azure ExpressRoute con errores.

## <a name="reset-a-circuit"></a>Restablecimiento de un circuito

1. Instale la versión más reciente de los cmdlets de PowerShell de Azure Resource Manager. Para más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/install-azurerm-ps).

2. Abra la consola de PowerShell con privilegios elevados y conéctese a su cuenta. Use el siguiente ejemplo para conectarse:

  ```azurepowershell-interactive
  Connect-AzureRmAccount
  ```
3. Si tiene varias suscripciones de Azure, compruebe las suscripciones de la cuenta.

  ```azurepowershell-interactive
  Get-AzureRmSubscription
  ```
4. Especifique la suscripción que desea usar.

  ```azurepowershell-interactive
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Ejecute los comandos siguientes para restablecer un circuito que se encuentra en un estado con errores:

  ```azurepowershell-interactive
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

El circuito ahora debería funcionar correctamente. Abra una incidencia de soporte técnico con [Soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) si el circuito sigue en un estado con errores.

## <a name="next-steps"></a>Pasos siguientes

Si sigue teniendo problemas, abra una incidencia de soporte técnico dirigida al [soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
