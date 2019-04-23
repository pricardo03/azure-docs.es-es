---
title: Cómo administrar las asignaciones con PowerShell
description: Obtenga información sobre cómo administrar las asignaciones de plano técnico con el módulo de PowerShell de plano técnico de Azure oficial, Az.Blueprint.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d8eacffe4b792eda5d81051f6aa65caa3292c896
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59785697"
---
# <a name="how-to-manage-assignments-with-powershell"></a>Cómo administrar las asignaciones con PowerShell

Una asignación del plano técnico se puede administrar mediante el **Az.Blueprint** módulo Azure PowerShell. El módulo admite capturar, crear, actualizar y quitar las asignaciones. El módulo también puede recuperar los detalles en la definición del plano técnico existente. En este artículo se explica cómo instalar el módulo y empezar a usarlo.

## <a name="add-the-azblueprint-module"></a>Agregar el módulo Az.Blueprint

Para habilitar Azure PowerShell administrar la asignación del plano técnico, se debe agregar el módulo. Este módulo se puede usar con PowerShell y PowerShell Core instalados localmente, con [Azure Cloud Shell](https://shell.azure.com) o con la [imagen de Docker de Azure PowerShell](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Requisitos básicos

El módulo de planos técnicos de Azure requiere el software siguiente:

- Azure PowerShell versión 1.5.0 o superior. Si todavía no está instalado, siga [estas instrucciones](/powershell/azure/install-az-ps).
- PowerShellGet 2.0.1 o una versión posterior. Si no está instalado o actualizado, siga [estas instrucciones](/powershell/gallery/installing-psget).

### <a name="install-the-module"></a>Instalación del módulo

El módulo de planos de PowerShell es **Az.Blueprint**.

1. Ejecute el siguiente comando como **administrador** en un símbolo del sistema de PowerShell:

   ```azurepowershell-interactive
   # Install the Blueprints module from PowerShell Gallery
   Install-Module -Name Az.Blueprint
   ```

   > [!NOTE]
   > Si **Az.Accounts** está ya instalado, puede que sea necesario usar `-AllowClobber` para forzar la instalación.

1. Valide que el módulo se ha importado y que sea la versión correcta (0.1.0):

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.Blueprint module
   Get-Command -Module 'Az.Blueprint' -CommandType 'Cmdlet'
   ```

## <a name="get-blueprint-definitions"></a>Obtiene las definiciones de plano técnico

El primer paso para trabajar con una asignación a menudo es obtener una referencia a una definición de plano técnico.
El `Get-AzBlueprint` obtiene uno o más definiciones del plano técnico. El cmdlet puede obtener la definición del plano técnico de un grupo de administración con `-ManagementGroupId {mgId}` o una suscripción con `-SubscriptionId {subId}`. El **nombre** parámetro Obtiene una definición del plano técnico, pero se debe utilizar con **ManagementGroupId** o **SubscriptionId**. **Versión** puede utilizarse con **nombre** ser más explícito acerca de qué definición de plano técnico se devuelve. En lugar de **versión**, el conmutador `-LatestPublished` grabs versión publicación más recientemente.

En el ejemplo siguiente se usa `Get-AzBlueprint` para obtener todas las versiones de una definición del plano técnico denominado "101-planos técnicos-definition-subscription' de una suscripción específica que se representan como `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get all versions of the blueprint definition in the specified subscription
$blueprints = Get-AzBlueprint -SubscriptionId '{subId}' -Name '101-blueprints-definition-subscription'

# Display the blueprint definition object
$blueprints
```

La salida de ejemplo para una definición de plano técnico con varias versiones tiene este aspecto:

```output
Name                 : 101-blueprints-definition-subscription
Id                   : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprints/101
                       -blueprints-definition-subscription
DefinitionLocationId : {subId}
Versions             : {1.0, 1.1}
TimeCreated          : 2019-02-25
TargetScope          : Subscription
Parameters           : {storageAccount_storageAccountType, storageAccount_location,
                       allowedlocations_listOfAllowedLocations, [Usergrouporapplicationname]:Reader_RoleAssignmentName}
ResourceGroups       : ResourceGroup
```

El [blueprint parámetros](../concepts/parameters.md#blueprint-parameters) definición en el plano técnico se puede ampliar para proporcionar más información.

```azurepowershell-interactive
$blueprints.Parameters
```

```output
Key                                                    Value
---                                                    -----
storageAccount_storageAccountType                      Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
storageAccount_location                                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
allowedlocations_listOfAllowedLocations                Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
[Usergrouporapplicationname]:Reader_RoleAssignmentName Microsoft.Azure.Commands.Blueprint.Models.PSParameterDefinition
```

## <a name="get-blueprint-assignments"></a>Obtener la asignación del plano técnico

Si la asignación del plano técnico ya existe, puede obtener una referencia a él con el `Get-AzBlueprintAssignment` cmdlet. El cmdlet toma **SubscriptionId** y **nombre** como parámetros opcionales. Si **SubscriptionId** no se especifica, se usa el contexto de la suscripción actual.

En el ejemplo siguiente se usa `Get-AzBlueprintAssignment` para obtener una asignación de plano técnico única denominada "Asignación-bloqueo-resource-groups" de una suscripción específica que se representan como `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -SubscriptionId '{subId}' -Name 'Assignment-lock-resource-groups'

# Display the blueprint assignment object
$blueprintAssignment
```

La salida de ejemplo para una asignación del plano técnico tiene este aspecto:

```output
Name              : Assignment-lock-resource-groups
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssignme
                    nts/Assignment-lock-resource-groups
Scope             : /subscriptions/{subId}
LastModified      : 2019-02-19
LockMode          : AllResourcesReadOnly
ProvisioningState : Succeeded
Parameters        :
ResourceGroups    : ResourceGroup
```

## <a name="create-blueprint-assignments"></a>Crear la asignación del plano técnico

Si aún no existe la asignación del plano técnico, puede crearla con el `New-AzBlueprintAssignment` cmdlet. Este cmdlet usa los siguientes parámetros:

- **Nombre** [obligatorio]
  - Especifica el nombre de la asignación del plano técnico
  - Debe ser único y no existe en **SubscriptionId**
- **Instancia de blueprint** [obligatorio]
  - Especifica la definición del plano técnico para asignar
  - Use `Get-AzBlueprint` para obtener el objeto de referencia
- **Ubicación** [obligatorio]
  - Especifica la región del asignado por el sistema identidad y suscripción implementación objeto administrado que se creará en
- **Suscripción** (opcional)
  - Especifica la suscripción de en que la asignación se implementa
  - Si no se proporciona, el valor predeterminado es el contexto de la suscripción actual
- **Bloqueo** (opcional)
  - Define el [bloqueo de recursos del plano](../concepts/resource-locking.md) que se usará para los recursos implementados
  - Opciones admitidas: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
  - Si no se proporciona, valor predeterminado es _ninguno_
- **SystemAssignedIdentity** (optional)
  - Seleccione para crear una identidad administrada asignado por el sistema para la asignación e implementar los recursos
  - Valor predeterminado para el conjunto de parámetros "identity"
  - No se puede usar con **UserAssignedIdentity**
- **UserAssignedIdentity** (optional)
  - Especifica la identidad administrada asignada por el usuario que se usará para la asignación y para implementar los recursos
  - Parte del conjunto de parámetro "identity"
  - No se puede usar con **SystemAssignedIdentity**
- **Parámetro** (opcional)
  - Un [tabla hash](/powershell/module/microsoft.powershell.core/about/about_hash_tables) de pares clave/valor de configuración [parámetros dinámicos](../concepts/parameters.md#dynamic-parameters) en la asignación del plano técnico
  - De manera predeterminada para un parámetro dinámico es el **defaultValue** en la definición
  - Si un parámetro no se proporciona y no tiene ningún **defaultValue**, el parámetro no es opcional

    > [!NOTE]
    > **Parámetro** no es compatible con secureStrings.

- **ResourceGroupParameter** (opcional)
  - Un [tabla hash](/powershell/module/microsoft.powershell.core/about/about_hash_tables) de artefactos de grupo de recursos
  - Cada marcador de posición del artefacto de grupo de recursos tendrá una pares clave/valor para establecer dinámicamente el **nombre** o **ubicación** en dicho artefacto del grupo de recursos
  - Si el parámetro de un grupo de recursos no se proporciona y no tiene ningún **defaultValue**, el parámetro del grupo de recursos no es opcional

En el ejemplo siguiente se crea una nueva asignación de versión "1.1" de la definición de plano técnico 'mi instancia de blueprint' capturada con `Get-AzBlueprint`, Establece la ubicación del objeto administrada identidad y la asignación a 'westus2', que bloquea los recursos con  _AllResourcesReadOnly_y establece las tablas hash para ambos **parámetro** y **ResourceGroupParameter** en la suscripción específica que se representan como `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
```

La salida de ejemplo para crear una asignación del plano técnico tiene este aspecto:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesReadyOnly
ProvisioningState : Creating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="update-blueprint-assignments"></a>Actualizar las asignaciones de plano técnico

A veces es necesario actualizar una asignación del plano técnico que ya se ha creado. El `Set-AzBlueprintAssignment` cmdlet controla esta acción. El cmdlet toma la mayor parte de los mismos parámetros que el `New-AzBlueprintAssignment` hace un cmdlet, lo que permite a todo lo que se estableció en la asignación en actualizarse. Las excepciones a esto son la _nombre_, _Blueprint_, y _SubscriptionId_. Solo los valores proporcionados se actualizan.

Para comprender qué ocurre cuando se actualiza una asignación del plano técnico, consulte [reglas para actualizar las asignaciones de](./update-existing-assignments.md#rules-for-updating-assignments).

- **Nombre** [obligatorio]
  - Especifica el nombre de la asignación del plano técnico para actualizar
  - Usado para buscar la asignación para la actualización, no para cambiar la asignación
- **Instancia de blueprint** [obligatorio]
  - Especifica la definición del plano técnico de la asignación del plano técnico
  - Use `Get-AzBlueprint` para obtener el objeto de referencia
  - Usado para buscar la asignación para la actualización, no para cambiar la asignación
- **Ubicación** (opcional)
  - Especifica la región del asignado por el sistema identidad y suscripción implementación objeto administrado que se creará en
- **Suscripción** (opcional)
  - Especifica la suscripción de en que la asignación se implementa
  - Si no se proporciona, el valor predeterminado es el contexto de la suscripción actual
  - Usado para buscar la asignación para la actualización, no para cambiar la asignación
- **Bloqueo** (opcional)
  - Define el [bloqueo de recursos del plano](../concepts/resource-locking.md) que se usará para los recursos implementados
  - Opciones admitidas: _None_, _AllResourcesReadOnly_, _AllResourcesDoNotDelete_
- **SystemAssignedIdentity** (optional)
  - Seleccione para crear una identidad administrada asignado por el sistema para la asignación e implementar los recursos
  - Valor predeterminado para el conjunto de parámetros "identity"
  - No se puede usar con **UserAssignedIdentity**
- **UserAssignedIdentity** (optional)
  - Especifica la identidad administrada asignada por el usuario que se usará para la asignación y para implementar los recursos
  - Parte del conjunto de parámetro "identity"
  - No se puede usar con **SystemAssignedIdentity**
- **Parámetro** (opcional)
  - Un [tabla hash](/powershell/module/microsoft.powershell.core/about/about_hash_tables) de pares clave/valor de configuración [parámetros dinámicos](../concepts/parameters.md#dynamic-parameters) en la asignación del plano técnico
  - De manera predeterminada para un parámetro dinámico es el **defaultValue** en la definición
  - Si un parámetro no se proporciona y no tiene ningún **defaultValue**, el parámetro no es opcional

    > [!NOTE]
    > **Parámetro** no es compatible con secureStrings.

- **ResourceGroupParameter** (opcional)
  - Un [tabla hash](/powershell/module/microsoft.powershell.core/about/about_hash_tables) de artefactos de grupo de recursos
  - Cada marcador de posición del artefacto de grupo de recursos tendrá una pares clave/valor para establecer dinámicamente el **nombre** o **ubicación** en dicho artefacto del grupo de recursos
  - Si el parámetro de un grupo de recursos no se proporciona y no tiene ningún **defaultValue**, el parámetro del grupo de recursos no es opcional

En el ejemplo siguiente se actualiza la asignación de versión "1.1" de la definición de plano técnico 'mi instancia de blueprint' capturada con `Get-AzBlueprint` cambiando el modo de bloqueo:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'

# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
```

La salida de ejemplo para crear una asignación del plano técnico tiene este aspecto:

```output
Name              : my-blueprint-assignment
Id                : /subscriptions/{subId}/providers/Microsoft.Blueprint/blueprintAssi
                    gnments/my-blueprint-assignment
Scope             : /subscriptions/{subId}
LastModified      : 2019-03-13
LockMode          : AllResourcesDoNotDelete
ProvisioningState : Updating
Parameters        : {storageAccount_storageAccountType}
ResourceGroups    : ResourceGroup
```

## <a name="remove-blueprint-assignments"></a>Quitar la asignación del plano técnico

Cuando se trata de una asignación del plano técnico va a quitar, el `Remove-AzBlueprintAssignment` cmdlet controla esta acción. El cmdlet acepta **nombre** o **InputObject** para especificar que la asignación para quitar del plano. **SubscriptionId** es _requiere_ y se debe proporcionar en todos los casos.

El ejemplo siguiente recupera una asignación del plano técnico existente con `Get-AzBlueprintAssignment` y, a continuación, quita la suscripción específica que se representan como `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the blueprint assignment in the specified subscription
$blueprintAssignment = Get-AzBlueprintAssignment -Name 'Assignment-lock-resource-groups'

# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $blueprintAssignment -SubscriptionId '{subId}'
```

## <a name="end-to-end-code-example"></a>Ejemplo de código to-end

Reunir todos los pasos, en el ejemplo siguiente se obtiene la definición del plano técnico, a continuación, crea, actualiza y quita una asignación del plano técnico de la suscripción específica que se representan como `{subId}`:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

#region GetBlueprint
# Get version '1.1' of the blueprint definition in the specified subscription
$bpDefinition = Get-AzBlueprint -SubscriptionId '{subId}' -Name 'my-blueprint' -Version '1.1'
#endregion

#region CreateAssignment
# Create the hash table for Parameters
$bpParameters = @{storageAccount_storageAccountType='Standard_GRS'}

# Create the hash table for ResourceGroupParameters
# ResourceGroup is the resource group artifact placeholder name
$bpRGParameters = @{ResourceGroup=@{name='storage_rg';location='westus2'}}

# Create the new blueprint assignment
$bpAssignment = New-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Location 'westus2' -Lock AllResourcesReadyOnly `
    -Parameter $bpParameters -ResourceGroupParameter $bpRGParameters
#endregion CreateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region UpdateAssignment
# Update the existing blueprint assignment
$bpAssignment = Set-AzBlueprintAssignment -Name 'my-blueprint-assignment' -Blueprint $bpDefinition `
    -SubscriptionId '{subId}' -Lock AllResourcesDoNotDelete
#endregion UpdateAssignment

# Wait for the blueprint assignment to finish deployment prior to the next steps

#region RemoveAssignment
# Remove the existing blueprint assignment
Remove-AzBlueprintAssignment -InputObject $bpAssignment -SubscriptionId '{subId}'
#endregion
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre el [ciclo de vida del plano técnico](../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../concepts/resource-locking.md).
- Puede consultar la información de [solución de problemas generales](../troubleshoot/general.md) para resolver los problemas durante la asignación de un plano técnico.