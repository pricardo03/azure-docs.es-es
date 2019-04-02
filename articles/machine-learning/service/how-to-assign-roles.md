---
title: Administrar roles en un área de trabajo de Azure Machine Learning
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo obtener acceso a un área de trabajo del servicio de Azure Machine Learning mediante el control de acceso basado en roles (RBAC).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 2/20/2019
ms.custom: seodec18
ms.openlocfilehash: 56813d9a075e1c327fb5612c50ea72f067ec505d
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805608"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Administrar el acceso a un área de trabajo de Azure Machine Learning

En este artículo, aprenderá a administrar el acceso a un área de trabajo de Azure Machine Learning. [Control de acceso basado en roles (RBAC)](/azure/role-based-access-control/overview) se usa para administrar el acceso a recursos de Azure. Los usuarios de Azure Active Directory se asignan roles específicos, que conceden acceso a los recursos. Azure proporciona roles integrados y la capacidad para crear roles personalizados.

## <a name="default-roles"></a>Roles predeterminados

Un área de trabajo de Azure Machine Learning es un recurso de Azure. Al igual que otros recursos de Azure, cuando se crea una nueva área de trabajo de Azure Machine Learning, viene con tres roles predeterminados. Puede agregar usuarios al área de trabajo y asignarlas a uno de estos roles integrados.

| Rol | Nivel de acceso |
| --- | --- |
| **Lector** | Acciones de solo lectura en el área de trabajo. Los lectores pueden enumerar y ver los recursos en un área de trabajo, pero no se puede crear o actualizar estos recursos. |
| **Colaborador** | Ver, crear, editar o eliminar (si procede) activos en un área de trabajo. Por ejemplo, colaboradores pueden crear un experimento, crear o adjuntar un clúster de cálculo, envía una ejecución e implementar un servicio web. |
| **Propietario** | Acceso al área de trabajo, incluida la capacidad de ver, crear, editar o eliminar (si procede) completo activos en un área de trabajo. Además, puede cambiar las asignaciones de roles. |

> [!IMPORTANT]
> Rol de acceso puede ampliarse a varios niveles en Azure. Por ejemplo, no un usuario con acceso de propietario a un área de trabajo puede tener acceso de propietario para el grupo de recursos que contiene el área de trabajo. Para obtener más información, consulte [cómo RBAC funciona](/azure/role-based-access-control/overview#how-rbac-works).

Para obtener más información sobre los roles integrados específicos, consulte [roles integrados para Azure](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Administrar el acceso de área de trabajo

Si es propietario de un área de trabajo, puede agregar y quitar funciones del área de trabajo. También puede asignar roles a los usuarios. Use los siguientes vínculos para descubrir cómo administrar el acceso:
- [Interfaz de usuario de Azure portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [CLI de Azure](/azure/role-based-access-control/role-assignments-cli)
- [API DE REST](/azure/role-based-access-control/role-assignments-rest)
- [Plantillas del Administrador de recursos de Azure](/azure/role-based-access-control/role-assignments-template)

Si ha instalado el [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md), también puede usar un comando CLI para asignar roles a los usuarios.

```azurecli-interactive 
az ml workspace share -n <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

El `user` campo es la dirección de correo electrónico de un usuario existente en la instancia de Azure Active Directory donde se encuentra la suscripción de área de trabajo principal. Este es un ejemplo de cómo utilizar este comando:

```azurecli-interactive 
az ml workspace share -n my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Crear un rol personalizado

Si los roles integrados no son suficientes, puede crear roles personalizados. Podrían tener roles personalizados leer, escribir, eliminar y permisos de recursos en esa área de trabajo de proceso. Puede realizar el rol disponible en un nivel de área de trabajo específica, un nivel de grupo de recursos específico o un nivel de suscripción específica.

> [!NOTE]
> Debe ser un propietario del recurso en ese nivel para crear roles personalizados dentro de ese recurso.

Para crear un rol personalizado, primero construya un archivo JSON de definición de rol que especifica el permiso y el ámbito para el rol. El ejemplo siguiente define un rol personalizado denominado "Científico de datos" con ámbito en un nivel de área de trabajo específica:

`data_scientist_role.json` :
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

Puede cambiar el `AssignableScopes` campo para establecer el ámbito de este rol personalizado en el nivel de suscripción, el nivel de grupo de recursos o un nivel de área de trabajo específica.

Este rol personalizado puede hacer lo mismo en el área de trabajo, excepto las siguientes acciones:

- No se puede crear o actualizar un recurso de proceso.
- No puede eliminar un recurso de proceso.
- No se puede agregar, eliminar o modificar las asignaciones de roles.
- No puede eliminar el área de trabajo.

Para implementar este rol personalizado, use el siguiente comando de CLI de Azure:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Después de la implementación, este rol está disponible en el área de trabajo especificada. Ahora puede agregar y asignar este rol en el portal de Azure. O bien, puede asignar este rol a un usuario mediante el `az ml workspace share` comando de CLI:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```


Para obtener más información, consulte [roles personalizados para recursos de Azure](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la seguridad empresarial](concept-enterprise-security.md)
- [Ejecutar de forma segura experimentos e inferencia dentro de una red virtual](how-to-enable-virtual-network.md)
- [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
