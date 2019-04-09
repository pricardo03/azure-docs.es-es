---
title: 'Creación de grupos de administración para organizar los recursos de Azure: gobernanza de Azure'
description: Aprenda a crear grupos de administración de Azure para administrar varios recursos mediante el portal, Azure PowerShell y la CLI de Azure.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2019
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 2dd2a6e071533deef47a6482bfb9ed92953864ba
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259823"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Creación de grupos de administración para la organización y la administración de recursos

Los grupos de administración son contenedores que ayudan a administran el acceso, las directivas y el cumplimiento de varias suscripciones. Cree estos contenedores para construir una jerarquía eficaz y eficiente que pueda usarse con [Azure Policy](../policy/overview.md) y los [controles de acceso basados en roles de Azure](../../role-based-access-control/overview.md). Para obtener más información sobre los grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](overview.md).

El primer grupo de administración creado en el directorio puede tardar hasta 15 minutos en completarse. Hay procesos que se ejecutan la primera vez para configurar el servicio de grupos de administración dentro de Azure para su directorio. Recibirá una notificación cuando finalice el proceso.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="create-a-management-group"></a>Creación de un grupo de administración

Puede crear el grupo de administración mediante el portal, PowerShell o la CLI de Azure. Actualmente no se pueden usar plantillas de Resource Manager para crear grupos de administración.

### <a name="create-in-portal"></a>Crear en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. En la página principal, seleccione **Nuevo grupo de administración**.

   ![Página para trabajar con grupos de administración](./media/main.png)

1. Rellene el campo de identificador de grupo de administración.

   - El **identificador de grupo de administración** es el identificador único de directorio que se usa para enviar comandos en este grupo de administración. Este identificador no es editable una vez creado, dado que se usa en todo el sistema de Azure para identificar este grupo. El [grupo de administración raíz](index.md#root-management-group-for-each-directory) se crean automáticamente con un identificador que es el identificador de Azure Active Directory. Para todos los grupos de administración, asignar un identificador único.
   - El campo de nombre para mostrar es el nombre que se muestra en Azure Portal. Un nombre para mostrar independiente es un campo opcional al crear el grupo de administración y se puede cambiar en cualquier momento.  

   ![Panel de opciones para crear un nuevo grupo de administración](./media/create_context_menu.png)  

1. Seleccione **Guardar**.

### <a name="create-in-powershell"></a>Crear en PowerShell

Para PowerShell, use el [New AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) para crear un nuevo grupo de administración.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** es un identificador único que se crea. Este identificador se usa por otros comandos para hacer referencia a este grupo y no puede cambiarse más adelante.

Si desea que el grupo de administración para mostrar un nombre diferente en el portal de Azure, agregue el **DisplayName** parámetro. Por ejemplo, para crear un grupo de administración con el nombre del grupo de Contoso y el nombre para mostrar del grupo de"Contoso", use el siguiente cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

En los ejemplos anteriores, se crea el nuevo grupo de administración en el grupo de administración raíz. Para especificar otro grupo de administración como el elemento primario, utilice el **ParentId** parámetro.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Crear en la CLI de Azure

CLI de Azure, use el [crear grupo de administración de cuenta az](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) comando para crear un nuevo grupo de administración.

```azurecli-interactive
az account management-group create --name Contoso
```

El **nombre** es un identificador único que se va a crear. Este identificador se usa por otros comandos para hacer referencia a este grupo y no puede cambiarse más adelante.

Si desea que el grupo de administración para mostrar un nombre diferente en el portal de Azure, agregue el **nombre_para_mostrar** parámetro. Por ejemplo, para crear un grupo de administración con el nombre del grupo de Contoso y el nombre para mostrar del grupo de"Contoso", use el comando siguiente:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

En los ejemplos anteriores, se crea el nuevo grupo de administración en el grupo de administración raíz. Para especificar otro grupo de administración como el elemento primario, utilice el **primario** parámetro y proporcione el nombre del grupo primario.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](create.md)
- [Cómo cambiar, eliminar o administrar los grupos de administración](manage.md)
- [Revise los grupos de administración en el módulo de recursos de Azure PowerShell](/powershell/module/az.resources#resources)
- [Revise los grupos de administración en la API de REST](/rest/api/resources/managementgroups)
- [Revise los grupos de administración en la CLI de Azure](/cli/azure/account/management-group)