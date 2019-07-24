---
title: Configuración de una respuesta personalizada para el firewall de aplicaciones web en Azure Front Door
description: Obtenga información sobre cómo configurar un mensaje y un código de respuesta personalizados cuando el firewall de aplicaciones web (WAF) bloquee una solicitud.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: tyao;kumud
ms.openlocfilehash: 146d17fe457751fb950f723c34826e43516e4e86
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165372"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Configuración de una respuesta personalizada para el firewall de aplicaciones web de Azure

De forma predeterminada, cuando el firewall de aplicaciones web (WAF) de Azure con Azure Front Door bloquea una solicitud debido a una regla coincidente, devuelve un código de estado 403 con el mensaje **La solicitud está bloqueada**. En este artículo se describe cómo configurar un mensaje de respuesta y un código de estado de respuesta personalizados cuando WAF bloquea una solicitud.

## <a name="set-up-your-powershell-environment"></a>Configuración del entorno de PowerShell
Azure PowerShell ofrece un conjunto de cmdlets que usan el modelo [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) para administrar los recursos de Azure. 

Puede instalar [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) en el equipo local y usarlo en cualquier sesión de PowerShell. Siga las instrucciones de la página para iniciar sesión con sus credenciales de Azure e instalar el módulo Az.PowerShell.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Conexión a Azure con un cuadro de diálogo interactivo para el inicio de sesión
```
Connect-AzAccount
Install-Module -Name Az
```
Asegúrese de tener instalada la versión actual de PowerShellGet. Ejecute el comando siguiente y vuelva a abrir PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Instalación del módulo Az.FrontDoor 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

En Azure, puede asignar recursos relacionados a un grupo de recursos. En este ejemplo, creará un grupo de recursos con el comando [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup).

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Creación de una directiva de WAF con respuesta personalizada 

A continuación se muestra un ejemplo de creación de una directiva de WAF con el código de estado de respuesta personalizado establecido en 405 y el mensaje **You are blocked** (Se le ha bloqueado) con el comando [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorWafPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Modifique la configuración del cuerpo de la respuesta o el código de respuesta personalizados de una directiva de WAF existente mediante el comando [Update-AzFrontDoorFireWallPolicy](/powershell/module/az.frontdoor/Update-AzFrontDoorWafPolicy).

```azurepowershell
# modify WAF response code
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Front Door](front-door-overview.md).