---
title: Configurar una regla de restricción de IP con una regla de firewall de aplicación web para el servicio de puerta de entrada de Azure
description: Obtenga información sobre cómo configurar una regla de firewall de aplicación web para restringir las direcciones IP de un extremo de servicio de puerta de entrada de Azure existente.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 88c5c284f26203ff3d6c39810a7b2810c1ebbc5a
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743162"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Configurar una regla de restricción de IP con un firewall de aplicaciones web para el servicio de puerta de entrada de Azure
Este artículo muestra cómo configurar las reglas de restricción de IP en firewall de aplicaciones web (WAF) para el servicio de puerta de entrada de Azure mediante la CLI de Azure, Azure PowerShell o una plantilla de Azure Resource Manager.

Una regla de control de acceso basado en la dirección IP es una regla de WAF personalizada que le permite controlar el acceso a sus aplicaciones web. Esto hace especificando una lista de direcciones IP o intervalos de direcciones IP en formato de enrutamiento entre dominios sin clase (CIDR).

De forma predeterminada, la aplicación web es accesible desde internet. Si desea limitar el acceso a los clientes desde una lista de direcciones IP conocidas o intervalos de direcciones IP, debe crear dos reglas de coincidencia de IP. La primera regla de coincidencia de IP contiene la lista de direcciones IP como valores coincidentes y establece la acción en **permitir**. Un segundo, con prioridad más baja, bloquea todas las demás direcciones IP mediante el uso de la **todas** operador y la configuración de la acción en **bloque**. Después de aplica una regla de restricción de IP, las solicitudes que se originan de direcciones que están fuera de esta lista de permitidos reciben una respuesta 403 Prohibido.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Configurar una directiva de WAF con la CLI de Azure

### <a name="prerequisites"></a>Requisitos previos
Antes de empezar a configurar una directiva de restricción de IP, configure el entorno de CLI y crear un perfil de servicio de puerta de entrada de Azure.

#### <a name="set-up-the-azure-cli-environment"></a>Configurar el entorno de CLI de Azure
1. Instalar el [CLI de Azure](/cli/azure/install-azure-cli), o usar Azure Cloud Shell. Azure Cloud Shell es un shell de Bash gratuito que puede ejecutarse directamente en Azure Portal. Tiene la CLI de Azure preinstalada y configurada para utilizarla con la cuenta. Seleccione el **Pruébelo** botón en los comandos CLI que siga y, a continuación, inicie sesión en su cuenta de Azure en la sesión de Cloud Shell que se abre. Una vez iniciada la sesión, escriba `az extension add --name front-door` para agregar la extensión de servicio de puerta de entrada de Azure.
 2. Si usa la CLI localmente en Bash, inicie sesión en Azure mediante el uso de `az login`.

#### <a name="create-an-azure-front-door-service-profile"></a>Crear un perfil de servicio de puerta de entrada de Azure
Crear un perfil de servicio de puerta de entrada de Azure siguiendo las instrucciones descritas en [inicio rápido: Crear una puerta de entrada para una aplicación web global altamente disponible](quickstart-create-front-door.md).

### <a name="create-a-waf-policy"></a>Crear una directiva de WAF

Crear una directiva de WAF mediante el [waf-directiva de red az crear](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) comando. En el ejemplo que se indica a continuación, reemplace el nombre de la directiva *IPAllowPolicyExampleCLI* con un nombre de directiva único.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Agregar una regla de control de acceso IP personalizada

Use la [crear az red waf-policy regla personalizada](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) comando para agregar una IP access control regla personalizada para la directiva de WAF que acaba de crear.

En los ejemplos siguientes:
-  Reemplace *IPAllowPolicyExampleCLI* con la directiva única que creó anteriormente.
-  Reemplace *ip-direcciones-intervalo de-1*, *ip-direcciones-intervalo de-2* con su propio intervalo.

En primer lugar, cree la dirección IP que la regla para las direcciones especificadas.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch "<ip-address-range-1>","<ip-address-range-2>" \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
A continuación, cree un **bloquee todo** regla con una prioridad menor que el anterior **permitir** regla. De nuevo, reemplace *IPAllowPolicyExampleCLI* en el ejemplo siguiente con la única directiva que creó anteriormente.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
    
### <a name="find-the-id-of-a-waf-policy"></a>Buscar el identificador de una directiva de WAF 
Busque el identificador de la directiva de un WAF mediante el [show de waf-directiva de red az](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) comando. Reemplace *IPAllowPolicyExampleCLI* en el ejemplo siguiente con la única directiva que creó anteriormente.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vincular una directiva de WAF en un host de front-end de servicio de puerta de entrada de Azure

Establecer el servicio de puerta de entrada de Azure *WebApplicationFirewallPolicyLink* identificador para el identificador de directiva mediante el uso de la [actualización de puerta principal de red az](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) comando. Reemplace *IPAllowPolicyExampleCLI* con la única directiva que creó anteriormente.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
En este ejemplo, se aplica la directiva de WAF a **FrontendEndpoints [0]** . Puede vincular la directiva de WAF a cualquiera de los servidores front-end.
> [!Note]
> Deberá establecer el **WebApplicationFirewallPolicyLink** propiedad una sola vez para vincular una directiva de WAF con un front-end de servicio de puerta de entrada de Azure. Las actualizaciones posteriores de las directivas se aplican automáticamente para el front-end.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Configurar una directiva de WAF con Azure PowerShell

### <a name="prerequisites"></a>Requisitos previos
Antes de empezar a configurar una directiva de restricción de IP, configure el entorno de PowerShell y crear un perfil de servicio de puerta de entrada de Azure.

#### <a name="set-up-your-powershell-environment"></a>Configuración del entorno de PowerShell
Azure PowerShell proporciona un conjunto de cmdlets que usan el [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelo para administrar recursos de Azure.

Puede instalar [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) en el equipo local y usarlo en cualquier sesión de PowerShell. Siga las instrucciones en la página de inicio de sesión PowerShell mediante el uso de las credenciales de Azure y, a continuación, instale el módulo de Az.

1. Conectarse a Azure mediante el comando siguiente y, a continuación, use un cuadro de diálogo interactivo para iniciar sesión.
    ```
    Connect-AzAccount
    ```
 2. Antes de instalar un módulo de servicio de puerta de entrada de Azure, asegúrese de que tiene la versión actual del módulo PowerShellGet instalado. Ejecute el siguiente comando y, a continuación, vuelva a abrir PowerShell.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Instale el módulo Az.FrontDoor mediante el comando siguiente. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Crear un perfil de servicio de puerta de entrada de Azure
Crear un perfil de servicio de puerta de entrada de Azure siguiendo las instrucciones descritas en [inicio rápido: Crear una puerta de entrada para una aplicación web global altamente disponible](quickstart-create-front-door.md).

### <a name="define-an-ip-match-condition"></a>Definir una condición de coincidencia IP
Use la [New AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) comando para definir una condición de coincidencia IP.
En el ejemplo siguiente, reemplace *ip-direcciones-intervalo de-1*, *ip-direcciones-intervalo de-2* con su propio intervalo.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Crear una dirección IP *todas condición de coincidencia* regla mediante el comando siguiente:
```powershell
$IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty Any        
  ```
    
### <a name="create-a-custom-ip-allow-rule"></a>Crear un personalizado direcciones IP permitidas de regla

Use la [New AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) comando para definir una acción y establecer una prioridad. En el ejemplo siguiente, se permitirán las solicitudes de direcciones IP de cliente que coincide con la lista.

```powershell
$IPAllowRule = New-AzFrontDoorCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Allow -Priority 1
```
Crear un **bloquee todo** regla con una prioridad menor que la dirección IP anterior **permitir** regla.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>Configurar una directiva de WAF
Busque el nombre del grupo de recursos que contiene el perfil del servicio de puerta de entrada de Azure mediante el uso de `Get-AzResourceGroup`. A continuación, configure una directiva de WAF con la dirección IP **bloquee todo** regla mediante el uso de [New AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vincular una directiva de WAF en un host de front-end de servicio de puerta de entrada de Azure

Vincular un objeto de directiva de WAF a un front-end host y la actualización de servicio de puerta de entrada de Azure propiedades existentes. En primer lugar, recupere el objeto de servicio de puerta de entrada de Azure mediante el uso de [Get AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). A continuación, establezca el **WebApplicationFirewallPolicyLink** propiedad para el identificador de recurso de *$IPAllowPolicyExamplePS*, creado en el paso anterior, mediante el uso de la [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)comando.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> En este ejemplo, se aplica la directiva de WAF a **FrontendEndpoints [0]** . Puede vincular una directiva de WAF a cualquiera de los servidores front-end. Deberá establecer el **WebApplicationFirewallPolicyLink** propiedad una sola vez para vincular una directiva de WAF con un front-end de servicio de puerta de entrada de Azure. Las actualizaciones posteriores de las directivas se aplican automáticamente para el front-end.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Configurar una directiva de WAF con una plantilla de Resource Manager
Para ver la plantilla que crea una directiva de servicio de puerta de entrada de Azure y una directiva de WAF con reglas de restricción de IP personalizadas, vaya a [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear un perfil de servicio de puerta de entrada de Azure](quickstart-create-front-door.md).
