---
title: Configuración de una regla de restricción de IP con una regla de firewall de aplicaciones web para Azure Front Door Service
description: Aprenda a configurar una regla de firewall de aplicaciones web para restringir las direcciones IP de un punto de conexión Azure Front Door Service.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: a610a2c01a1e935c55942b621e5b3799cb002fc0
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698643"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Configuración de una regla de restricción de IP con un firewall de aplicaciones web para Azure Front Door Service
Este artículo muestra cómo configurar las reglas de restricción de IP en un firewall de aplicaciones web (WAF) para Azure Front Door Service mediante la CLI de Azure, Azure PowerShell o una plantilla de Azure Resource Manager.

Una regla de control de acceso basado en la dirección IP es una regla de WAF personalizada que permite controlar el acceso a las aplicaciones web. Para ello, especifica una lista de direcciones IP o rangos de direcciones IP en formato de Enrutamiento entre dominios sin clase (CIDR).

De forma predeterminada, a la aplicación web se puede acceder desde Internet. Si desea limitar el acceso a los clientes de una lista de direcciones IP conocidas o de determinados rangos de direcciones IP, debe crear dos reglas de coincidencia de IP. La primera regla de coincidencia de IP contiene la lista de direcciones IP como valores coincidentes y establece la acción en **Permitir**. La segunda, con menor prioridad, bloquea las restantes direcciones IP mediante el uso del operador **ALL** y el establecimiento de la acción en **Bloquear**. Después de que se aplique una regla de restricción de IP, las solicitudes que provengan de direcciones que no se encuentren en esta lita de permitidos reciben una respuesta 403 Prohibido.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Configuración de una directiva de WAF con la CLI de Azure

### <a name="prerequisites"></a>Requisitos previos
Antes de empezar a configurar una directiva de restricción de IP, configure el entorno de CLI y crear un perfil de Azure Front Door Service.

#### <a name="set-up-the-azure-cli-environment"></a>Configuración del entorno de la CLI de Azure
1. Instale la [CLI de Azure](/cli/azure/install-azure-cli) o use Azure Cloud Shell. Azure Cloud Shell es un shell de Bash gratuito que puede ejecutarse directamente en Azure Portal. Tiene la CLI de Azure preinstalada y configurada para utilizarla con la cuenta. Seleccione el botón **Probar** en los siguientes comandos de la CLI y, después, inicie sesión con su cuenta de Azure en la sesión de Cloud Shell que se abre. Después de que se inicie la sesión, escriba `az extension add --name front-door` para agregar la extensión de Azure Front Door Service.
 2. Si usa la CLI localmente en Bash, inicie sesión en Azure mediante `az login`.

#### <a name="create-an-azure-front-door-service-profile"></a>Creación de un perfil de Azure Front Door Service
Para crear un perfil de Azure Front Door Service, siga las instrucciones que se describen en [Inicio rápido: Creación de una instancia de Front Door para una aplicación web global de alta disponibilidad](quickstart-create-front-door.md).

### <a name="create-a-waf-policy"></a>Creación de una directiva de WAF

Para crear una directiva de WAF utilice el comando [az network waf-policy create](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-create). En el ejemplo que se indica a continuación, reemplace el nombre de la directiva *IPAllowPolicyExampleCLI* por un nombre de directiva único.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Adición de una regla de control de acceso por IP personalizada

Use el comando [az network waf-policy custom-rule create](/cli/azure/ext/front-door/network/front-door/waf-policy/rule?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-rule-create) para agregar una regla de control de acceso por IP personalizada para la directiva de WAF que acaba de crear.

En los siguientes ejemplos:
-  Reemplace *IPAllowPolicyExampleCLI* por la directiva única que creó anteriormente.
-  Reemplace *ip-address-range-1*, *ip-address-range-2* por su propio rango.

En primer lugar, cree la regla de permiso de IP para las direcciones especificadas.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ("<ip-address-range-1>","<ip-address-range-2>") \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
A continuación, cree una regla de **bloqueo de todo** con menor prioridad que la regla de **permiso**. Vuelva a reemplazar *IPAllowPolicyExampleCLI* en el ejemplo siguiente por la directiva que ha creado anteriormente.

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
    
### <a name="find-the-id-of-a-waf-policy"></a>Búsqueda del identificador de una directiva de WAF 
Para buscar el identificador de una directiva de WAF utilice el comando [az network waf-policy show](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-show). Reemplace *IPAllowPolicyExampleCLI* en el ejemplo siguiente por la directiva que ha creado anteriormente.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vinculación de una directiva de WAF a un host de front-end de Azure Front Door Service

Establezca el identificador *WebApplicationFirewallPolicyLink* de Azure Front Door Service en el identificador de la directiva mediante el uso del comando [az network front-door update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update). Reemplace *IPAllowPolicyExampleCLI* por la directiva única que creó anteriormente.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
En este ejemplo, la directiva de WAF se aplica a **FrontendEndpoints [0]** . Puede vincular la directiva de WAF a cualquiera de los front-ends.
> [!Note]
> La propiedad **WebApplicationFirewallPolicyLink** solo es preciso establecerla una vez para vincular una directiva de WAF a un front-end de Azure Front Door Service. Las posteriores actualizaciones de la directiva se aplican al front-end.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Configuración de una directiva de WAF con Azure PowerShell

### <a name="prerequisites"></a>Requisitos previos
Antes de empezar a configurar una directiva de restricción de IP, configure el entorno de PowerShell y crear un perfil de Azure Front Door Service.

#### <a name="set-up-your-powershell-environment"></a>Configuración del entorno de PowerShell
Azure PowerShell proporciona un conjunto de cmdlets que usan el modelo de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) para administrar los recursos de Azure.

Puede instalar [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) en el equipo local y usarlo en cualquier sesión de PowerShell. Siga las instrucciones de la página para iniciar sesión en PowerShell con las credenciales de Azure y, después, instale el módulo Az.

1. Utilice el siguiente comando para conectarse a Azure y, después, use un cuadro de diálogo interactivo para iniciar sesión.
    ```
    Connect-AzAccount
    ```
 2. Antes de instalar un módulo de Azure Front Door Service asegúrese de que tiene instalada la última versión del módulo PowerShellGet. Ejecute el siguiente comando y vuelva a abrir PowerShell.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. Instale el módulo Az.FrontDoor con el siguiente comando. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Creación de un perfil de Azure Front Door Service
Para crear un perfil de Azure Front Door Service, siga las instrucciones que se describen en [Inicio rápido: Creación de una instancia de Front Door para una aplicación web global de alta disponibilidad](quickstart-create-front-door.md).

### <a name="define-an-ip-match-condition"></a>Definición de una condición de coincidencia IP
Use el comando [New AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) para definir una condición de coincidencia de IP.
En el siguiente ejemplo, reemplace *ip-address-range-1*, *ip-address-range-2* por su propio rango.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue "ip-address-range-1", "ip-address-range-2"
```
Cree una regla de *condición de coincidencia plena de* IP mediante el comando siguiente:
```powershell
$IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty Any        
  ```
    
### <a name="create-a-custom-ip-allow-rule"></a>Creación de una regla de permiso de IP personalizada

Use el comando [New AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) para definir una acción y establecer una prioridad. En el ejemplo siguiente, se permitirán las solicitudes de IP del cliente que coincidan con la lista.

```powershell
$IPAllowRule = New-AzFrontDoorCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Allow -Priority 1
```
Cree una regla de **bloqueo de todo** con menor prioridad que la regla de **permiso** de IP.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>Configuración de una directiva de WAF
Busque el nombre del grupo de recursos que contiene el perfil de Azure Front Door Service. Para ello, debe usar `Get-AzResourceGroup`. A continuación, configure una directiva de WAF con la regla de **bloqueo de todas** las IP, para lo que debe usar [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Vinculación de una directiva de WAF a un host de front-end de Azure Front Door Service

Vincule un objeto de directiva de WAF a host de front-end existente y actualice las propiedades de Azure Front Door Service. En primer lugar, recupere el objeto Azure Front Door Service, para lo que debe usar [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). A continuación, establezca la propiedad **WebApplicationFirewallPolicyLink** en el identificador de recurso de *$IPAllowPolicyExamplePS*, que se creó en el paso anterior, mediante el uso del comando [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor).

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> En este ejemplo, la directiva de WAF se aplica a **FrontendEndpoints [0]** . Una directiva de WAF se puede vincular a cualquiera de los front-ends. La propiedad **WebApplicationFirewallPolicyLink** solo es preciso establecerla una vez para vincular una directiva de WAF a un front-end de Azure Front Door Service. Las posteriores actualizaciones de la directiva se aplican al front-end.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Configuración de una directiva de WAF con una plantilla de Resource Manager
Para ver la plantilla que crea una directiva de Azure Front Door Service y una directiva de WAF con reglas de restricción de IP personalizadas, vaya a [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear de un perfil de Azure Front Door Service](quickstart-create-front-door.md).
