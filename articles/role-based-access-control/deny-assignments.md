---
title: Descripción de las asignaciones de denegación en Azure RBAC | Microsoft Docs
description: Obtenga información sobre las asignaciones de denegación en el control de acceso basado en rol (RBAC) para los recursos de Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 8ef3a2ec44c5eff80d3a50a6c56805667e164ba8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980175"
---
# <a name="understand-deny-assignments"></a>Descripción de las asignaciones de denegación

Similar a una asignación de roles, una *asignación de denegación* enlaza un conjunto de acciones de denegación con un usuario, grupo o entidad de servicio en un ámbito determinado con el fin de denegar el acceso. Una asignación de denegación también puede excluir las entidades de seguridad y evitar la herencia para los ámbitos secundarios, algo diferente de las asignaciones de roles. Actualmente, las asignaciones de denegación son de **solo lectura** y únicamente pueden establecerse mediante Azure. En este artículo se describe cómo se definen las asignaciones de denegación.

## <a name="deny-assignment-properties"></a>Propiedades de la asignación de denegación

 Una asignación de denegación tiene las siguientes propiedades:

> [!div class="mx-tableFixed"]
> | Propiedad | Obligatorio | Escriba | DESCRIPCIÓN |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | SÍ | string | El nombre para mostrar de la asignación de denegación. Los nombres deben ser únicos para un ámbito determinado. |
> | `Description` | Sin  | string | La descripción de la asignación de denegación. |
> | `Permissions.Actions` | Al menos una Actions o una DataActions | String[] | Una matriz de cadenas que especifican las operaciones de administración a las que la asignación de denegación bloquea el acceso. |
> | `Permissions.NotActions` | Sin  | String[] | Una matriz de cadenas que especifican las operaciones de administración que deben excluirse de la asignación de denegación. |
> | `Permissions.DataActions` | Al menos una Actions o una DataActions | String[] | Una matriz de cadenas que especifican las operaciones de datos a las que la asignación de denegación bloquea el acceso. |
> | `Permissions.NotDataActions` | Sin  | String[] | Una matriz de cadenas que especifican las operaciones de datos que deben excluirse de la asignación de denegación. |
> | `Scope` | Sin  | string | Una cadena que especifica el ámbito al que se aplica la asignación de denegación. |
> | `DoNotApplyToChildScopes` | Sin  | boolean | Especifica si la asignación de denegación se aplica a los ámbitos secundarios. El valor predeterminado es False. |
> | `Principals[i].Id` | SÍ | String[] | Una matriz de identificadores de objetos de entidades de seguridad de Azure AD (usuario, grupo o entidad de servicio) a los que se aplica la asignación de denegación. Establecido en un GUID vacío `00000000-0000-0000-0000-000000000000` para representar a todos los usuarios. |
> | `Principals[i].Type` | Sin  | String[] | Una matriz de tipos de objetos representados por Principals[i].Id. Establecido en `Everyone` para representar a todos los usuarios. |
> | `ExcludePrincipals[i].Id` | Sin  | String[] | Una matriz de identificadores de objetos de entidades de seguridad de Azure AD (usuario, grupo o entidad de servicio) a los que no se aplica la asignación de denegación. |
> | `ExcludePrincipals[i].Type` | Sin  | String[] | Una matriz de tipos de objetos representados por ExcludePrincipals[i].Id. |
> | `IsSystemProtected` | Sin  | boolean | Especifica si Azure ha creado esta asignación de denegación y no se puede editar ni eliminar. Actualmente, el sistema protege todas las asignaciones de denegación. |

## <a name="everyone-principal"></a>Entidad de seguridad para todos los usuarios

A fin de respaldar las asignaciones de denegación, se ha introducido la entidad de seguridad para todos los usuarios. La entidad de seguridad para todos los usuarios representa a todos los usuarios, grupos y entidades de servicio en un directorio de Azure AD. Si el identificador de la entidad de seguridad es un GUID cero `00000000-0000-0000-0000-000000000000` y el tipo de entidad de seguridad es `Everyone`, la entidad de seguridad representa a todos los usuarios. La entidad de seguridad para todos los usuarios se puede combinar con `ExcludePrincipals` para denegar el acceso a todos los usuarios excepto a algunos. La entidad de seguridad para todos los usuarios tiene las siguientes restricciones:

- Solo se puede utilizar en `Principals` y no se puede usar en `ExcludePrincipals`.
- `Principals[i].Type` se debe establecer en `Everyone`.

## <a name="next-steps"></a>Pasos siguientes

* [List deny assignments using RBAC and the REST API](deny-assignments-rest.md) (Lista de asignaciones de denegación mediante RBAC y la API REST)
* [Descripción de definiciones de roles](role-definitions.md)
