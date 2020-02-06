---
title: Administración de roles del área de trabajo
titleSuffix: Azure Machine Learning
description: Conozca cómo acceder a un área de trabajo de Azure Machine Learning mediante el control de acceso basado en rol (RBAC).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 11/06/2019
ms.custom: seodec18
ms.openlocfilehash: aba613911328b1272ebb07eeae633932cb4a442f
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76935367"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Administración del acceso a un área de trabajo de Azure Machine Learning
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, obtendrá información sobre cómo administrar el acceso a un área de trabajo de Azure Machine Learning. El [control de acceso basado en rol (RBAC)](/azure/role-based-access-control/overview) se utiliza para administrar el acceso a los recursos de Azure. Se asignan roles específicos a los usuarios de Azure Active Directory que les conceden acceso a los recursos. Azure proporciona tanto roles integrados como la capacidad de crear roles personalizados.

## <a name="default-roles"></a>Roles predeterminados

Un área de trabajo de Azure Machine Learning es un recurso de Azure. Al igual que otros recursos de Azure, cuando se crea un área de trabajo de Azure Machine Learning nueva, viene con tres roles predeterminados. Puede agregar usuarios al área de trabajo y asignarlos a uno de estos roles integrados.

| Role | Nivel de acceso |
| --- | --- |
| **Lector** | Acciones de solo lectura en el área de trabajo. Los lectores pueden enumerar y ver los recursos de un área de trabajo, pero no pueden crear ni actualizar estos recursos. |
| **Colaborador** | Ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Por ejemplo, los colaboradores pueden crear un experimento, crear o conectar un clúster de proceso, enviar una ejecución e implementar un servicio web. |
| **Propietario** | Acceso total al área de trabajo, incluida la posibilidad de ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Además, puede cambiar las asignaciones de roles. |

> [!IMPORTANT]
> El acceso de los roles puede tener un ámbito de varios niveles en Azure. Por ejemplo, un usuario con acceso de propietario a un área de trabajo podría no tener acceso de propietario al grupo de recursos que contiene el área de trabajo. Para más información, consulte [Cómo funciona RBAC](/azure/role-based-access-control/overview#how-rbac-works).

Para más información sobre roles integrados específicos, consulte [Roles integrados de Azure](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Administración del acceso al área de trabajo

Si es propietario de un área de trabajo, puede agregar y eliminar roles del área de trabajo. También puede asignar roles a los usuarios. Use los siguientes vínculos para descubrir cómo administrar el acceso:
- [Interfaz de usuario de Azure Portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [CLI de Azure](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Plantillas del Administrador de recursos de Azure](/azure/role-based-access-control/role-assignments-template)

Si ha instalado la [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md), también puede usar un comando de la CLI para asignar roles a los usuarios.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

El campo `user` es la dirección de correo electrónico de un usuario existente en la instancia de Azure Active Directory donde se encuentra la suscripción principal del área de trabajo. Este es un ejemplo de cómo utilizar este comando:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Creación de un rol personalizado

Si los roles integrados no son suficientes, puede crear roles personalizados. Los roles personalizados pueden tener permisos para leer, escribir, eliminar y procesar recursos de ese área de trabajo. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico.

> [!NOTE]
> Debe ser un propietario del recurso en ese nivel para crear roles personalizados en ese recurso.

Para crear un rol personalizado, primero debe construir un archivo JSON de definición de rol que especifica el permiso y el ámbito del rol. El ejemplo siguiente define un rol personalizado llamado "Data Scientist" (Científico de datos) con ámbito en un nivel de área de trabajo específica:

`data_scientist_role.json`:
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

Puede cambiar el campo `AssignableScopes` para establecer el ámbito de este rol personalizado en el nivel de suscripción, el nivel de grupo de recursos o un nivel de área de trabajo específica.

Este rol personalizado puede hacer todo en el área de trabajo, excepto las siguientes acciones:

- No puede crear o actualizar un recurso de proceso.
- No puede eliminar un recurso de proceso.
- No puede agregar, eliminar o modificar asignaciones de roles.
- No puede eliminar el área de trabajo.

Para implementar este rol personalizado, use el siguiente comando de la CLI de Azure:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Después de la implementación, este rol está disponible en el área de trabajo especificada. Ahora puede agregar y asignar este rol en Azure Portal. O bien, puede asignar este rol a un usuario mediante el comando de la CLI `az ml workspace share`:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Para obtener información acerca de los roles personalizados, consulte [Roles personalizados en los recursos de Azure](/azure/role-based-access-control/custom-roles).

Para más información sobre las operaciones (acciones) que se pueden usar con roles personalizados, consulte [Operaciones del proveedor de recursos](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la seguridad en la empresa](concept-enterprise-security.md)
- [Ejecución de experimentos y realización de inferencias/puntuaciones de forma segura en una red virtual](how-to-enable-virtual-network.md)
- [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
- [Operaciones del proveedor de recursos](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)