---
title: Descripción de las asignaciones de denegación para recursos de Azure | Microsoft Docs
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 53716fa343df25026dcc668ed8483673d934d1ad
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339131"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Descripción de las asignaciones de denegación para recursos de Azure

De forma similar a una asignación de roles, una *asignación de denegación* asocia un conjunto de acciones de denegación a un usuario, grupo o entidad de servicio en un ámbito determinado con el fin de denegar el acceso. Las asignaciones de denegación impiden que los usuarios realicen acciones concretas en recursos de Azure, aunque una asignación de roles les conceda acceso. Algunos proveedores de recursos ya incluyen asignaciones de denegación de Azure. Actualmente, las asignaciones de denegación son de **solo lectura** y únicamente Microsoft puede establecerlas.

En cierto modo, las asignaciones de denegación son diferentes a las asignaciones de roles. Las asignaciones de denegación pueden excluir las entidades de seguridad e impedir la herencia a los ámbitos secundarios. Las asignaciones de denegación también se aplican a las asignaciones de [administrador de suscripciones clásicas](rbac-and-directory-admin-roles.md).

En este artículo se describe cómo se definen las asignaciones de denegación.

## <a name="deny-assignment-properties"></a>Propiedades de la asignación de denegación

 Una asignación de denegación tiene las siguientes propiedades:

> [!div class="mx-tableFixed"]
> | Propiedad | Obligatorio | Type | DESCRIPCIÓN |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Sí | string | El nombre para mostrar de la asignación de denegación. Los nombres deben ser únicos para un ámbito determinado. |
> | `Description` | Sin  | string | La descripción de la asignación de denegación. |
> | `Permissions.Actions` | Al menos una Actions o una DataActions | String[] | Una matriz de cadenas que especifican las operaciones de administración a las que la asignación de denegación bloquea el acceso. |
> | `Permissions.NotActions` | Sin  | String[] | Una matriz de cadenas que especifican las operaciones de administración que deben excluirse de la asignación de denegación. |
> | `Permissions.DataActions` | Al menos una Actions o una DataActions | String[] | Una matriz de cadenas que especifican las operaciones de datos a las que la asignación de denegación bloquea el acceso. |
> | `Permissions.NotDataActions` | Sin  | String[] | Una matriz de cadenas que especifican las operaciones de datos que deben excluirse de la asignación de denegación. |
> | `Scope` | Sin  | string | Una cadena que especifica el ámbito al que se aplica la asignación de denegación. |
> | `DoNotApplyToChildScopes` | Sin  | boolean | Especifica si la asignación de denegación se aplica a los ámbitos secundarios. El valor predeterminado es False. |
> | `Principals[i].Id` | Sí | String[] | Matriz de identificadores de objetos de entidad de seguridad de Azure AD (usuario, grupo o entidad de servicio) a los que se aplica la asignación de denegación. Establézcala en un GUID `00000000-0000-0000-0000-000000000000` vacío para representar a todas las entidades de seguridad. |
> | `Principals[i].Type` | Sin  | String[] | Una matriz de tipos de objetos representados por Principals[i].Id. Establézcala en `SystemDefined` para representar a todas las entidades de seguridad. |
> | `ExcludePrincipals[i].Id` | Sin  | String[] | Matriz de identificadores de objetos de entidades de seguridad de Azure AD (usuario, grupo o entidad de servicio) a los que no se aplica la asignación de denegación. |
> | `ExcludePrincipals[i].Type` | Sin  | String[] | Una matriz de tipos de objetos representados por ExcludePrincipals[i].Id. |
> | `IsSystemProtected` | Sin  | boolean | Especifica si Azure ha creado esta asignación de denegación y no se puede editar ni eliminar. Actualmente, el sistema protege todas las asignaciones de denegación. |

## <a name="system-defined-principal"></a>Entidad de seguridad definida por el sistema

Con objeto de admitir las asignaciones de denegación, se ha introducido la **entidad de seguridad definida por el sistema**. Esta entidad de seguridad representa a todos los usuarios, grupos, entidades de servicio y identidades administradas de un directorio de Azure AD. Si el identificador de la entidad de seguridad es un GUID cero `00000000-0000-0000-0000-000000000000` y el tipo de entidad de seguridad es `SystemDefined`, la entidad de seguridad representa a todas las entidades de seguridad. `SystemDefined` se puede combinar con `ExcludePrincipals` para denegar todas las entidades de seguridad, salvo algunos usuarios. `SystemDefined` tiene las siguientes restricciones:

- Solo se puede utilizar en `Principals` y no se puede usar en `ExcludePrincipals`.
- `Principals[i].Type` se debe establecer en `SystemDefined`.

## <a name="next-steps"></a>Pasos siguientes

* [Enumeración de las asignaciones de denegación para recursos de Azure mediante la API de REST](deny-assignments-rest.md)
* [Descripción de definiciones de roles para los recursos de Azure](role-definitions.md)
