---
title: Cambio, eliminación y administración de los grupos de administración de Azure
description: Aprenda a mantener y actualizar la jerarquía de grupos de administración.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 10dfa9812a0546f3a8c57e28227851b6f72657fc
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582424"
---
# <a name="manage-your-resources-with-management-groups"></a>Administración de los recursos con grupos de administración

Los grupos de administración son contenedores que ayudan a administran el acceso, las directivas y el cumplimiento de varias suscripciones. Puede cambiar, eliminar y administrar estos contenedores para que las jerarquías puedan usarse con [Azure Policy](../policy/overview.md) y los [controles de acceso basado en roles de Azure (RBAC)](../../role-based-access-control/overview.md). Para más información acerca de los grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](overview.md).

Para realizar cambios en un grupo de administración, debe tener un rol de propietario o colaborador en el grupo de administración. Para ver qué permisos tiene, seleccione el grupo de administración y, a continuación, seleccione **IAM**. Para más información sobre los roles de RBAC, consulte [Administración del acceso y los permisos con RBAC](../../role-based-access-control/overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Cambio del nombre de un grupo de administración

Puede cambiar el nombre del grupo de administración mediante el portal, PowerShell o la CLI de Azure.

### <a name="change-the-name-in-the-portal"></a>Cambiar el nombre en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración cuyo nombre quiere cambiar.

1. Seleccione la opción **Cambiar nombre de grupo** en la parte superior de la página.

   ![Cambiar nombre de grupo](./media/detail_action_small.png)

1. Cuando se abra el menú, escriba el nuevo nombre que desea mostrar.

   ![Cambiar nombre de grupo](./media/rename_context.png)

1. Seleccione **Guardar**.

### <a name="change-the-name-in-powershell"></a>Cambiar el nombre en PowerShell

Para actualizar el nombre para mostrar, use **Update-AzureRmManagementGroup**. Por ejemplo, para cambiar el nombre de un grupo de administración de "Contoso TI" a "Grupo de Contoso", ejecute el siguiente comando:

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Cambiar el nombre en la CLI de Azure

En la CLI de Azure, use el comando de actualización.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Eliminación de un grupo de administración

Para eliminar un grupo de administración, deben cumplirse los siguientes requisitos:

1. No deben existir grupos de administración secundarios ni suscripciones en el grupo de administración.

   - Para mover una suscripción fuera de un grupo de administración, vea [Mover la suscripción a otro grupo de administración](#Move-subscriptions-in-the-hierarchy).

   - Para mover un grupo de administración a otro grupo de administración, consulte [Mover grupos de administración en la jerarquía](#Move-management-groups-in-the-hierarchy).

1. Tiene permisos de escritura sobre el rol de propietario o colaborador para grupos de administración en el grupo de administración. Para ver qué permisos tiene, seleccione el grupo de administración y, a continuación, seleccione **IAM**. Para más información sobre los roles de RBAC, consulte [Administración del acceso y los permisos con RBAC](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Eliminar en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que desea eliminar.

1. Seleccionar **Eliminar**

   - Si el icono está deshabilitado, al mantener el selector del mouse sobre el icono se muestra el motivo.

   ![Eliminar grupo](./media/delete.png)

1. Se abre una ventana para que confirme si quiere eliminar el grupo de administración.

   ![Eliminar grupo](./media/delete_confirm.png)

1. Seleccione **Sí**.

### <a name="delete-in-powershell"></a>Eliminar en PowerShell

Use el comando **Remove-AzureRmManagementGroup** de PowerShell para eliminar grupos de administración.

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Eliminación en la CLI de Azure

Con la CLI de Azure, use el comando az account management-group delete.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Visualización de los grupos de administración

Puede ver cualquier grupo de administración sobre el que tenga un rol de RBAC directo o heredado.  

### <a name="view-in-the-portal"></a>Ver en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Se carga la página de jerarquía de grupos de administración, en la que puede explorar todos los grupos de administración y suscripciones a los que tiene acceso. Si selecciona el nombre del grupo, descenderá un nivel en la jerarquía. La navegación funciona de la misma forma que un explorador de archivos.

   ![Principal](./media/main.png)

1. Para ver los detalles del grupo de administración, seleccione el vínculo **(detalles)** situado junto al título de este. Si este vínculo no está disponible, no tiene permisos para ver ese grupo de administración.  

### <a name="view-in-powershell"></a>Ver en PowerShell

Use el comando Get-AzureRmManagementGroup para recuperar todos los grupos.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

Para obtener información de un único grupo de administración, use el parámetro -GroupName.

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="view-in-azure-cli"></a>Ver en la CLI de Azure

Use el comando list para recuperar todos los grupos.  

```azurecli-interactive
az account management-group list
```

Para obtener información de un único grupo de administración, use el comando show.

```azurecli-interactive
az account management-group show --name 'Contoso'
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Movimiento de las suscripciones en la jerarquía

Uno de los motivos de crear un grupo de administración es agrupar las suscripciones. Solo los grupos de administración y las suscripciones pueden convertirse en secundarios de otro grupo de administración. Una suscripción que se mueve a un grupo de administración hereda todas las directivas y accesos de usuario del grupo de administración primario.

Para mover la suscripción, hay dos permisos que debe tener:

- Rol de "propietario" en la suscripción secundaria.
- Rol de "propietario" o "colaborador" en el nuevo grupo de administración primario.
- Rol de "propietario" o "colaborador" en el grupo de administración primario anterior.

Para ver qué permisos tiene, seleccione el grupo de administración y, a continuación, seleccione **IAM**. Para más información sobre los roles de RBAC, consulte [Administración del acceso y los permisos con RBAC](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Mover las suscripciones en el portal

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Agregar una suscripción existente a un grupo de administración

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario.

1. En la parte superior de la página, haga clic en **Agregar suscripción**.

1. Seleccione la suscripción de la lista con el identificador correcto.

   ![Secundario](./media/add_context_sub.png)

1. Seleccione "Guardar".

#### <a name="remove-a-subscription-from-a-management-group"></a>Quitar una suscripción de un grupo de administración

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario actual.  

1. Seleccione los puntos suspensivos al final de la fila correspondiente a la suscripción de la lista que quiere mover.

   ![Move](./media/move_small.png)

1. Seleccione **Mover**.

1. En el menú que se abre, seleccione el **grupo de administración primario**.

   ![Move](./media/move_small_context.png)

1. Seleccione **Guardar**.

### <a name="move-subscriptions-in-powershell"></a>Mover las suscripciones en PowerShell

Para mover una suscripción en PowerShell, use el comando New-AzureRmManagementGroupSubscription.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Para quitar el vínculo entre la suscripción y el grupo de administración, use el comando Remove-AzureRmManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Mover las suscripciones en la CLI de Azure

Para mover una suscripción en la CLI, utilice el comando add.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Para quitar la suscripción del grupo de administración, use el comando subscription remove.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Movimiento de grupos de administración en la jerarquía  

Al mover un grupo de administración primario, todos los recursos secundarios que incluyen grupos de administración, suscripciones, grupos de recursos y recursos se mueven con el elemento primario.

### <a name="move-management-groups-in-the-portal"></a>Mover grupos de administración en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario.

1. En la parte superior de la página, haga clic en **Agregar grupo de administración**.

1. En el menú que se abre, seleccione si quiere un grupo de administración nuevo o usar uno existente.

   - Al seleccionar Nuevo se crea un grupo de administración.
   - Al seleccionar un grupo existente se mostrará una lista desplegable de todos los grupos de administración que se pueden mover a este grupo de administración.  

   ![mover](./media/add_context_MG.png)

1. Seleccione **Guardar**.

### <a name="move-management-groups-in-powershell"></a>Mover grupos de administración en PowerShell

Use el comando Update-AzureRmManagementGroup de PowerShell para mover un grupo de administración a un grupo diferente.

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Mover grupos de administración en la CLI de Azure

Use el comando update para mover un grupo de administración con CLI de Azure.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="audit-management-groups-using-activity-logs"></a>Auditoría de los grupos de administración mediante registros de actividad

Para realizar el seguimiento de los grupos de administración mediante esta API, use la [API de registro de actividad de inquilinos](/rest/api/monitor/tenantactivitylogs). Actualmente no es posible usar PowerShell, la CLI ni Azure Portal para realizar el seguimiento de las actividades de los grupos de administración.

1. Como administrador de inquilinos del inquilino de Azure AD, [eleve los privilegios de acceso](../../role-based-access-control/elevate-access-global-admin.md) y asigne un rol de lector al usuario de auditoría con el ámbito `/providers/microsoft.insights/eventtypes/management`.
1. Como usuario de auditoría, llame a la [API de registro de actividad de inquilinos](/rest/api/monitor/tenantactivitylogs) para ver las actividades del grupo de administración. Va a filtrar **Microsoft.Management** por proveedor de recursos para todas las actividades del grupo de administración.  Ejemplo:

```xml
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> Para llamar cómodamente a esta API desde la línea de comandos, pruebe [ARMClient](https://github.com/projectkudu/ARMClient).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](create.md)
- [Cambio, eliminación y administración de los grupos de administración](manage.md)
- [Revisión de grupos de administración en el módulo de recursos de Azure PowerShell](https://aka.ms/mgPSdocs)
- [Revisión de grupos de administración en la API REST](https://aka.ms/mgAPIdocs)
- [Revisión de grupos de administración en la CLI de Azure](https://aka.ms/mgclidoc)
