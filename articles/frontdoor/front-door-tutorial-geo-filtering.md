---
title: 'Tutorial: Configuración del filtrado geográfico en un dominio para Azure Front Door Service | Microsoft Docs'
description: En este tutorial aprenderá a crear una directiva de filtrado geográfico simple y a asociarla con el host de front-end de Front Door existente
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2018
ms.author: sharadag
ms.openlocfilehash: 68da9a0255cde6cbad5c675901c80193888bf255
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214885"
---
# <a name="how-to-set-up-a-geo-filtering-policy-for-your-front-door"></a>Configuración de una directiva de filtrado geográfico para Front Door
En este tutorial se muestra cómo usar Azure PowerShell para crear una directiva de filtrado geográfico de ejemplo y a asociarla con el host de front-end de Front Door existente. Esta directiva de filtrado geográfico de ejemplo bloqueará las solicitudes de todos los países, salvo Estados Unidos.

## <a name="1-set-up-your-powershell-environment"></a>1. Configuración del entorno de PowerShell
Azure PowerShell ofrece un conjunto de cmdlets que usan el modelo [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) para administrar los recursos de Azure. 

Puede instalar [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) en el equipo local y usarlo en cualquier sesión de PowerShell. Siga las instrucciones de la página para iniciar sesión con sus credenciales de Azure e instalar AzureRM.
```
# Connect to Azure with an interactive dialog for sign-in
Connect-AzureRmAccount
Install-Module -Name AzureRM
```
> [!NOTE]
>  [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) pronto será compatible.

Antes de instalar el módulo de Front Door asegúrese de que tiene instalada la última versión de PowerShellGet. Ejecute el comando siguiente y vuelva a abrir PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

Instale el módulo AzureRM.FrontDoor. 

```
Install-Module -Name AzureRM.FrontDoor -AllowPrerelease
```

## <a name="2-define-geo-filtering-match-conditions"></a>2. Definición de las condiciones de coincidencia del filtrado geográfico
En primer lugar, cree una condición de coincidencia de ejemplo que seleccione las solicitudes que no procedan "EE.UU.". Cuando vaya a crear una condición de coincidencia, consulte en la [guía](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoormatchconditionobject) de PowerShell los parámetros necesarios para hacerlo. [Aquí](front-door-geo-filtering.md) se proporciona un código de país de dos letras para la asignación de país.

```
$nonUSGeoMatchCondition = New-AzureRmFrontDoorMatchConditionObject -MatchVariable RemoteAddr -OperatorProperty GeoMatch -NegateCondition $true -MatchValue "US"
```
 
## <a name="3-add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>3. Adición de una condición de coincidencia de filtrado geográfico a una regla con Action y Priority

Luego, cree un objeto CustomRule `nonUSBlockRule` en función de la condición de coincidencia, un valor de Action y un valor de Priority.  Un objeto CustomRule puede tener varios MatchCondition.  En este ejemplo, el valor de Action es Block y el de Priority es 1, la prioridad más alta.

```
$nonUSBlockRule = New-AzureRmFrontDoorCustomRuleObject -Name "geoFilterRule" -RuleType MatchRule -MatchCondition $nonUSGeoMatchCondition -Action Block -Priority 1
```

Cuando vaya a crear un objeto CustomRuleObject, consulte en la [guía](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorcustomruleobject) de PowerShell los parámetros necesarios para hacerlo.

## <a name="4-add-rules-to-a-policy"></a>4. Adición de reglas a una directiva
En este paso se crea un objeto de directiva `geoPolicy` que contiene el elemento `nonUSBlockRule` del paso anterior en el grupo de recursos especificado. Use `Get-AzureRmResourceGroup` para encontrar ResourceGroupName $resourceGroup.

```
$geoPolicy = New-AzureRmFrontDoorFireWallPolicy -Name "geoPolicyAllowUSOnly" -resourceGroupName $resourceGroup -Customrule $nonUSBlockRule  -Mode Prevention -EnabledState Enabled
```

Cuando vaya a crear una directiva, consulte en la [guía](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorfirewallpolicy) de PowerShell los parámetros necesarios para hacerlo.

## <a name="5-link-policy-to-a-front-door-frontend-host"></a>5. Vinculación de una directiva a un host de front-end de Front Door
Los últimos pasos son vincular el objeto de directiva de protección a un host de front-end de Front Door existente y actualizar las propiedades de Front Door. En primer lugar, recupere el objeto Front Door mediante el uso de [Get AzureRmFrontDoor](https://docs.microsoft.com/azure/frontdoor/get-azurermfrontdoor) y, después, establezca la propiedad WebApplicationFirewallPolicyLink de su front-end en el valor de resourceId de `geoPolicy`.

```
$geoFrontDoorObjectExample = Get-AzureRmFrontDoor -ResourceGroupName $resourceGroup
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Use el siguiente [comando](https://docs.microsoft.com/azure/frontdoor/set-azurermfrontdoor) para actualizar el objeto Front Door.

```
Set-AzureRmFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> Con establecer una sola vez la propiedad WebApplicationFirewallPolicyLink se vinculará una directiva de protección a un host de front-end de Front Door. Las posteriores actualizaciones de la directiva se aplicarán al host del front-end.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de la [seguridad de la capa de aplicaciones con Front Door](front-door-application-security.md).
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
