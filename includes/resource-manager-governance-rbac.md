---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: rockboyfor
manager: digimobile
ms.service: azure-resource-manager
ms.topic: include
origin.date: 02/16/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f77a5d482c3f8632a3d86bd8e027fbb4418168c3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122913"
---
Desea asegurarse de que los usuarios de una organización tengan el nivel adecuado de acceso a estos recursos. No desea conceder acceso ilimitado a los usuarios, pero también es necesario garantizar que puedan realizar su trabajo. El control de acceso basado en rol (RBAC) le permite administrar los usuarios con permiso para completar acciones específicas en un ámbito. Un rol define un conjunto de acciones permitidas. Asigne el rol a un ámbito y especifique los usuarios que pertenecen a ese rol para el ámbito.

Al planear su estrategia de control de acceso, conceda a los usuarios los privilegios mínimos para que realicen su trabajo. La siguiente imagen muestra un patrón sugerido para la asignación de RBAC.

![Ámbito](./media/resource-manager-governance-rbac/role-examples.png)

Hay tres roles que se aplican a todos los recursos: Propietario, Colaborador y Lector. Todas las cuentas asignadas al rol Propietario deben estar estrechamente controladas y usarse con poca frecuencia. Los usuarios que solo tienen que observar el estado de las soluciones deben concederse el rol Lector.

A la mayoría de usuarios se les concede [roles específicos de recursos](../articles/role-based-access-control/built-in-roles.md) o [roles personalizados](../articles/role-based-access-control/custom-roles.md), ya sea en el nivel de suscripción o de grupo de recursos. Estos roles definen estrechamente las acciones permitidas. Al asignar usuarios a estos roles, concede el acceso necesario a los usuarios sin permitir demasiado control. Puede asignar una cuenta a más de un rol y que el usuario obtenga los permisos combinados de los roles. Conceder acceso en el nivel de recursos suele ser demasiado restrictivo para los usuarios, pero puede funcionar para un proceso automatizado diseñado para tareas específicas.

### <a name="who-can-assign-roles"></a>Quién puede asignar roles

Para crear y quitar las asignaciones de rol, los usuarios deben tener acceso a `Microsoft.Authorization/roleAssignments/*`. Esta acción se concede mediante los roles Propietario o Administrador de acceso de usuario.
<!--ms.date: 04/30/2018-->