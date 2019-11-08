---
title: Asociar una directiva de Firewall de aplicaciones web a una Azure Application Gateway existente
description: Más información sobre cómo asociar una directiva de Firewall de aplicaciones web a una Azure Application Gateway existente.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: b455849c889c463fbda305e690f998d58fab0d8f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511529"
---
# <a name="associate-a-waf-policy-with-an-existing-application-gateway"></a>Asociar una directiva de WAF de Application Gateway existente

Puedes usar Azure PowerShell para [crear una directiva de WAF](create-waf-policy-ag.md), pero puede que ya tengas una Application Gateway y que solo quieras asociar una directiva de WAF a él. En este artículo, hace justamente eso; crear una directiva de WAF y asociarla a una Application Gateway ya existente. 

1. Obtenga una Directiva de firewall de Application Gateway. Si no tiene una Directiva de firewall existente, consulte el paso 2. 

   ```azurepowershell-interactive
      Connect-AzAccount
      Select-AzSubscription -Subscription "<sub id>"`

      #Get Application Gateway and existing policy object or create new`
      $gw = Get-AzApplicationGateway -Name <Waf v2> -ResourceGroupName <RG name>`
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```

2. (Opcional) Crear una Directiva de firewall.

   ```azurepowershell-interactive
      New-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>'
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```
   > [!NOTE]
   > Si crear esta Directiva de WAF para pasar de una configuración de WAF a una Directiva de WAF, la Directiva debe ser una copia exacta de la configuración anterior. Esto significa que cada exclusión, regla personalizada, grupo de reglas deshabilitado, etc. debe ser exactamente igual que en la configuración de WAF.
3. (Opcional) Puede configurar la Directiva WAF para que se adapte a sus necesidades. Esto incluye las reglas personalizadas, la deshabilitación de reglas y grupos de reglas, exclusiones, la configuración de límites de carga de archivos, etc. Si omite este paso, se seleccionarán todos los valores predeterminados. 
   
4. Guarde la directiva y adjúntela al Application Gateway. 
   
   ```azurepowershell-interactive
      #Save the policy itself
      Set-AzApplicationGatewayFirewallPolicy -InputObject $policy`
   
      #Attach the policy to an Application Gateway
      $gw.FirewallPolicy = $policy`
   
      #Save the Application Gateway
      Set-AzApplicationGateway -ApplicationGateway $gw`
   ```

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre las Reglas personalizadas.](/configure-waf-custom-rules.md)
