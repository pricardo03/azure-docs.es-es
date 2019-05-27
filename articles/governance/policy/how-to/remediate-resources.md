---
title: Corrección de recursos no compatibles
description: Este procedimiento le guiará a través de la corrección de los recursos que no son conformes con las directivas de Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: d6753b319bc5bc4cbda18fe486695e5b0266acae
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66169648"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Corregir los recursos no conformes con Azure Policy

Los recursos que no son conformes para un directiva **deployIfNotExists** se pueden colocar en un estado conforme a través de **Corrección**. Se realiza la corrección indicando a Azure Policy para ejecutar el **deployIfNotExists** efecto de la directiva asignada en los recursos existentes. Este artículo muestra los pasos necesarios para comprender y realizar la corrección con Azure Policy.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="how-remediation-security-works"></a>Cómo funciona la seguridad de corrección

Cuando la directiva de Azure se ejecuta la plantilla el **deployIfNotExists** definición de directiva, lo hace mediante una [identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md).
Directiva de Azure crea una identidad administrada para cada asignación, pero debe tener información sobre los roles para conceder a la identidad administrada. Si faltan roles en la identidad administrada, este error se muestra durante la asignación de la directiva o una iniciativa. Al usar el portal, Azure Policy concederá automáticamente la identidad administrada los roles enumerados una vez que se inició la asignación.

![Identidad administrada - función ausente](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Si un recurso modificado por **deployIfNotExists** está fuera del ámbito de la asignación de directiva o la plantilla tiene acceso a propiedades en los recursos fuera del ámbito de la asignación de directiva, debe [concederse manualmente acceso](#manually-configure-the-managed-identity) a la identidad administrada de la asignación o se producirá un error en la implementación de actualizaciones.

## <a name="configure-policy-definition"></a>Configurar la definición de directiva

El primer paso es definir los roles que **deployIfNotExists** necesita en la definición de directiva para implementar correctamente el contenido de la plantilla incluida. En la propiedad **details**, agregue una propiedad **roleDefinitionIds**. Esta propiedad es una matriz de cadenas que coinciden con los roles de su entorno. Para obtener un ejemplo completo, vea el [ejemplo deployIfNotExists](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** utiliza el identificador de recurso completo y no toma el **roleName** corto del rol. Para obtener el identificador del rol “Contributor” en su entorno, use el siguiente código:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Configurar manualmente la identidad administrada

Al crear un trabajo mediante el portal, Azure Policy genera la identidad administrada y le concede los roles definidos en **roleDefinitionIds**. En las siguientes condiciones, los pasos para crear la identidad administrada y asignarle permisos se deben realizar manualmente:

- Al usar el SDK (por ejemplo, Azure PowerShell)
- Cuando la plantilla modifica un recurso fuera del ámbito de asignación
- Cuando la plantilla lee un recurso fuera del ámbito de asignación

> [!NOTE]
> Azure PowerShell y .NET son el únicos SDK que actualmente admiten esta funcionalidad.

### <a name="create-managed-identity-with-powershell"></a>Creación de una identidad administrada con PowerShell

Para crear una identidad administrada durante la asignación de la directiva, debe definirse **Location** y debe usarse **AssignIdentity**. En el siguiente ejemplo se obtiene la definición de la directiva integrada **Implementar cifrado de datos transparente de SQL DB**, se establece el grupo de recursos de destino y después se crea la asignación.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

La variable `$assignment` contiene ahora el identificador principal de la identidad administrada junto con los valores estándar devueltos cuando se crea una asignación de directiva. Se puede acceder a ella a través de `$assignment.Identity.PrincipalId`.

### <a name="grant-defined-roles-with-powershell"></a>Conceder roles definidos con PowerShell

La nueva identidad administrada debe completar la replicación a través de Azure Active Directory antes de se le puedan conceder los roles necesarios. Una vez se completa la replicación, el siguiente ejemplo itera la definición de directiva en `$policyDef` para **roleDefinitionIds** y usa [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) para conceder la nueva identidad administrada a los roles.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Conceder roles definidos a través de Azure Portal

Hay dos maneras de conceder a la identidad administrada de una asignación los roles definidos mediante Azure Portal, mediante **Access control (IAM)** o editando la asignación de directiva o iniciativa y haciendo clic en **Guardar**.

Para agregar un rol a la identidad administrada de la asignación, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

1. Seleccione **Asignaciones** en el panel izquierdo de la página de Azure Policy.

1. Busque la asignación que tiene una identidad administrada y haga clic en el nombre.

1. Busque la propiedad **Id. de asignación** en la página Editar. El identificador de asignación será algo como:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   El nombre de la identidad administrada es la última parte del identificador del recurso de asignación, que es `2802056bfc094dfb95d4d7a5` en este ejemplo. Copie esta parte del identificador de recursos de asignación.

1. Navegue hasta el contenedor primario del recurso o recursos (grupo de recursos, suscripción, grupo de administración) al que se debe agregar manualmente la definición de función.

1. Haga clic en el vínculo **Control de acceso (IAM)** en la página de recursos y haga clic en **+ Agregar asignación de rol** en la parte superior de la página de control de acceso.

1. Seleccione el rol adecuado que coincide con un **roleDefinitionIds** de la definición de la directiva.
   Deje **Asignar acceso a** con el valor predeterminado de “Usuario de Azure AD, grupo o aplicación”. En el cuadro **Seleccionar**, pegue o escriba la parte del identificador del recurso de asignación que buscó anteriormente. Una vez finalizada la búsqueda, haga clic en el objeto con el mismo nombre que el identificador seleccionado y haga clic en **Guardar**.

## <a name="create-a-remediation-task"></a>Crear una tarea de corrección

### <a name="create-a-remediation-task-through-portal"></a>Crear una tarea de corrección a través del portal

Durante la evaluación, el efecto de asignación de directiva con **deployIfNotExists** determina si hay recursos no conformes. Cuando se encuentran los recursos no conformes, se proporcionan los detalles en la página **Corrección**. La opción para desencadenar una **tarea de corrección** está junto a la lista de directivas que tienen recursos no página. Esta opción crea una implementación desde la plantilla **deployIfNotExists**.

Para crear un **tarea de corrección**, siga estos pasos:

1. Inicie el servicio Azure Policy en Azure Portal. Para ello, haga clic en **Todos los servicios** y, a continuación, busque y seleccione **Directiva**.

   ![Búsqueda de la directiva en todos los servicios](../media/remediate-resources/search-policy.png)

1. Seleccione **Corrección** en el lado izquierdo de la página Azure Policy.

   ![Seleccione la corrección en la página de directiva](../media/remediate-resources/select-remediation.png)

1. Todas las asignaciones de directivas **deployIfNotExists** con recursos no conformes se incluyen en la tabla de datos y la pestaña **Directivas para resolver**. Haga clic en una directiva con recursos que no son conformes. Se abre la página **Nueva tarea de corrección**.

   > [!NOTE]
   > Una forma alternativa de abrir la página **Tareas de corrección** consiste en buscar y hacer clic en la directiva desde la página **Cumplimiento** y después hacer clic en el botón **Crear tarea de corrección**.

1. En la página **Nueva tarea de corrección**, filtre los recursos para corregir mediante la elipse **Ámbito** para seleccionar los recursos secundarios a partir de los cuales se asignó la directiva (incluidos los objetos de recursos individuales). Además, utilice la lista desplegable **Ubicaciones** para filtrar más los recursos. Solo los recursos enumerados en la tabla se corregirán.

   ![Corrección: seleccione los recursos que corregir](../media/remediate-resources/select-resources.png)

1. Inicie la tarea de corrección cuando se hayan filtrado los recursos, para ello, haga clic en **Corregir**. Se abrirá la página de cumplimiento de directivas en la pestaña **Tareas de corrección** para mostrar el estado del progreso de las tareas.

   ![Corrección: progreso de las tareas de corrección](../media/remediate-resources/task-progress.png)

1. Haga clic en el **tarea de corrección** en la página de cumplimiento de directiva para obtener detalles sobre el progreso. El filtro usado en la tarea se muestra junto con una lista de los recursos que se van a corregir.

1. En la página **Tarea de corrección**, haga clic con el botón derecho en un recurso para ver el recurso o la implementación de la tarea de corrección. Al final de la fila, haga clic en **Eventos relacionados** para ver detalles, como un mensaje de error.

   ![Corrección: menú contextual de tarea de recurso](../media/remediate-resources/resource-task-context-menu.png)

Los recursos implementados mediante una **tarea de corrección** se agregan a la pestaña **Recursos implementados** en la página de cumplimiento de la directiva.

### <a name="create-a-remediation-task-through-azure-cli"></a>Crear una tarea de corrección a través de la CLI de Azure

Para crear un **tareas de corrección** con la CLI de Azure, use el `az policy remediation` comandos. Reemplace `{subscriptionId}` con el identificador de suscripción y `{myAssignmentId}` con su **deployIfNotExists** identificador de asignación de directiva.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Para otros comandos de corrección y ejemplos, vea el [corrección de directivas az](/cli/azure/policy/remediation) comandos.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Crear una tarea de corrección a través de PowerShell de Azure

Para crear un **tareas de corrección** con Azure PowerShell, use el `Start-AzPolicyRemediation` comandos. Reemplace `{subscriptionId}` con el identificador de suscripción y `{myAssignmentId}` con su **deployIfNotExists** identificador de asignación de directiva.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Para otros cmdlets de corrección y ejemplos, vea el [Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights) módulo.

## <a name="next-steps"></a>Pasos siguientes

- Revise los ejemplos en [ejemplos de Azure Policy](../samples/index.md).
- Revise la [estructura de definición de Azure Policy](../concepts/definition-structure.md).
- Vea la [Descripción de los efectos de directivas](../concepts/effects.md).
- Comprender cómo [crear mediante programación las directivas](programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](getting-compliance-data.md).
- Compruebe que un grupo de administración con [organizar los recursos con grupos de administración de Azure](../../management-groups/overview.md).