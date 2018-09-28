---
title: Creación de grupos de administración para organizar los recursos de Azure
description: Aprenda a crear grupos de administración de Azure para administrar varios recursos.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2018
ms.author: rithorn
ms.openlocfilehash: c2d4317bcbf70a0cebf6ab1915968eeb9ef8b4c6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992634"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Creación de grupos de administración para la organización y la administración de recursos

Los grupos de administración son contenedores que ayudan a administran el acceso, las directivas y el cumplimiento de varias suscripciones. Cree estos contenedores para construir una jerarquía eficaz y eficiente que pueda usarse con [Azure Policy](../policy/overview.md) y los [controles de acceso basados en roles de Azure](../../role-based-access-control/overview.md). Para obtener más información sobre los grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](overview.md).

El primer grupo de administración creado en el directorio puede tardar hasta 15 minutos en completarse. Hay procesos que se ejecutan la primera vez para configurar el servicio de grupos de administración dentro de Azure para su directorio. Recibirá una notificación cuando finalice el proceso.

## <a name="create-a-management-group"></a>Creación de un grupo de administración

Puede crear el grupo de administración mediante el portal, PowerShell o la CLI de Azure. Actualmente no se pueden usar plantillas de Resource Manager para crear grupos de administración.

### <a name="create-in-portal"></a>Crear en el portal

1. Inicie sesión en [Azure Portal](http://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. En la página principal, seleccione **Nuevo grupo de administración**.

   ![Grupo principal](./media/main.png)

1. Rellene el campo de identificador de grupo de administración.

   - El **identificador de grupo de administración** es el identificador único de directorio que se usa para enviar comandos en este grupo de administración. Este identificador no es editable una vez creado el grupo, dado que se usa en todo el sistema de Azure para identificar ese grupo.
   - El campo de nombre para mostrar es el nombre que se muestra en Azure Portal. Un nombre para mostrar independiente es un campo opcional al crear el grupo de administración y se puede cambiar en cualquier momento.  

   ![Crear](./media/create_context_menu.png)  

1. Seleccione **Guardar**.

### <a name="create-in-powershell"></a>Crear en PowerShell

En PowerShell, use los cmdlets Add-AzureRmManagementGroups:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso'
```

**GroupName** es un identificador único que se crea. Este identificador lo usan otros comandos para hacer referencia a este grupo y no se puede cambiar más adelante.

Si quisiera que el grupo de administración mostrara un nombre diferente en Azure Portal, agregaría el parámetro **DisplayName** con la cadena. Por ejemplo, si quisiera crear un grupo de administración con el nombre de grupo de Contoso y el nombre para mostrar de "Grupo de Contoso", usaría el siguiente cmdlet:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId 'ContosoTenant'
```

Use el parámetro **ParentId** para que este grupo de administración se cree en una administración diferente.

### <a name="create-in-azure-cli"></a>Crear en la CLI de Azure

En al CLI de Azure, usará el comando az account management-group create.

```azurecli-interactive
az account management-group create --group-name 'Contoso'
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Organización de los recursos con grupos de administración de Azure](overview.md)
- [Cambio, eliminación y administración de los grupos de administración](manage.md)
- [Instalación del módulo de Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Revisión de las especificaciones de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalación de la extensión de la CLI de Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)