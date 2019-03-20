---
title: Administrar grupos de Azure Resource Manager mediante la CLI de Azure | Microsoft Docs
description: Use la CLI de Azure para administrar los grupos de Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: c9e6cc68cbd629642849d323d4271722f3232aa3
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56824940"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Administrar grupos de recursos de Azure Resource Manager mediante la CLI de Azure

Aprenda a usar la CLI de Azure con [Azure Resource Manager](resource-group-overview.md) para administrar los grupos de recursos de Azure. Para administrar los recursos de Azure, consulte [los recursos de administración de Azure mediante la CLI de Azure](./manage-resources-cli.md).

Otros artículos acerca de cómo administrar grupos de recursos:

- [Administrar grupos de recursos de Azure mediante el portal de Azure](./manage-resources-portal.md)
- [Administrar grupos de recursos de Azure mediante Azure PowerShell](./manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>¿Qué es un grupo de recursos?

Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. El grupo de recursos puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. Para decidir cómo asignar los recursos a los grupos de recursos, tenga en cuenta lo que más conviene a su organización. Por lo general, se recomienda agregar recursos que compartan el mismo ciclo de vida al mismo grupo de recursos para que los pueda implementar, actualizar y eliminar con facilidad como un grupo.

Los grupos de recursos almacenan metadatos acerca de los recursos. Por consiguiente, al especificar la ubicación del grupo de recursos, se especifica el lugar en que se almacenan dichos metadatos. Por motivos de compatibilidad, es posible que sea preciso asegurarse de que los datos se almacenan en una región concreta.

Los grupos de recursos almacenan metadatos acerca de los recursos. Al especificar una ubicación para el grupo de recursos, especificar dónde se almacenan dichos metadatos.

## <a name="create-resource-groups"></a>Crear grupos de recursos

El siguiente script CLI crea un grupo de recursos y, a continuación, muestra el grupo de recursos.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location $location
```

## <a name="list-resource-groups"></a>Enumeración de grupos de recursos

El script CLI siguiente enumera los grupos de recursos en su suscripción.

```azurecli-interactive
az group list
```

Para obtener un grupo de recursos:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Eliminar grupos de recursos

El siguiente script CLI elimina un grupo de recursos:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Para obtener más información acerca de cómo Azure Resource Manager ordena la eliminación de recursos, consulte [eliminación del grupo de recursos de Azure Resource Manager](./resource-group-delete.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Implementar recursos en un grupo de recursos

Consulte [implementar recursos en un grupo de recursos](./manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Implementar un grupo de recursos y recursos

Puede crear un grupo de recursos y la implementación de recursos en el grupo mediante una plantilla de Resource Manager. Para más información, consulte [Creación de un grupo de recursos e implementación de recursos](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="move-to-another-resource-group-or-subscription"></a>Mover a otro grupo de recursos o suscripción

Puede mover los recursos en el grupo a otro grupo de recursos. Para obtener más información, consulte [mover recursos](./manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>Grupos de recursos de bloqueo

El bloqueo impide que otros usuarios de su organización eliminen o modifiquen los recursos críticos, como la suscripción de Azure, el grupo de recursos o recurso accidentalmente. 

La siguiente secuencia de comandos bloquea un grupo de recursos, por lo que no se puede eliminar el grupo de recursos.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

El siguiente script obtiene todos los bloqueos para un grupo de recursos:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

El siguiente script elimina un bloqueo:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Para obtener más información, consulte [Bloqueo de recursos con el Administrador de recursos de Azure](resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Grupos de recursos de etiqueta

Puede aplicar etiquetas a los recursos y grupos de recursos para organizar de manera lógica los recursos. Para obtener información, consulte [mediante etiquetas para organizar los recursos de Azure](./resource-group-using-tags.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Exportar grupos de recursos en plantillas

Después de configurar correctamente el grupo de recursos, puede ver la plantilla de Resource Manager para el grupo de recursos. Exportar la plantilla ofrece dos ventajas:

- Automatice las implementaciones futuras de la solución porque la plantilla contiene toda la infraestructura completa.
- Obtenga información sobre la sintaxis de la plantilla mediante la búsqueda en JavaScript Object Notation (JSON) que representa la solución.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

La secuencia de comandos muestra la plantilla en la consola.  Copie el JSON y guárdelo como un archivo.

Para obtener más información, consulte [exportar grupos de recursos](./manage-resource-groups-portal.md#export-resource-groups-to-templates).

## <a name="manage-access-to-resource-groups"></a>Administrar el acceso a los grupos de recursos

El [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) es la forma en que se administra el acceso a los recursos en Azure. Para obtener más información, consulte [administrar el acceso mediante la CLI de Azure y RBAC](../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre Azure Resource Manager, consulte [Introducción a Azure Resource Manager](./resource-group-overview.md).
- Para obtener información sobre la sintaxis de la plantilla de Resource Manager, consulte [entender la estructura y sintaxis de plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, consulte [referencia de plantilla](/azure/templates/).