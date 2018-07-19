---
title: Visualización de los miembros de un rol de administrador y los permisos de los roles en Azure Active Directory | Microsoft Docs
description: Ahora puede ver y administrar los miembros de un rol de administrador de Azure AD en el portal. Para aquellos que administran con frecuencia las asignaciones de roles.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/10/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 5a42f48e85eea95211b36e0c08dcb0edb4928a20
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38489929"
---
# <a name="view-members-and-descriptions-of-administrator-roles-in-azure-active-directory"></a>Ver miembros y las descripciones de los roles de administrador en Azure Active Directory

Ahora puede ver y administrar todos los miembros de los roles de administrador en el portal de Azure Active Directory. Si administra las asignaciones de roles con frecuencia, probablemente preferirá esta experiencia. Y si alguna vez se ha preguntado "¿qué es lo que realmente hacen estos roles?", puede ver una lista detallada de los permisos para cada uno de los roles de administrador de Azure AD.

También es fácil ver sus propios permisos. Haga clic en **Su rol** para obtener acceso rápido a la página de usuario para ver una lista de todos los roles asignados activos. Haga clic en el botón de puntos suspensivos a la derecha de cada fila para abrir la descripción detallada del rol.

![lista de roles en el portal de Azure AD](./media/directory-manage-roles-portal/role-list.png)

Seleccione la fila completa para ver la lista de miembros asignados. Puede hacer clic en **Administrar en PIM** para obtener funcionalidades de administración adicionales. Los administradores de roles con privilegios pueden cambiar las asignaciones "Permanentes" (siempre activas en el rol) a "Elegibles" (en el rol solo cuando se elevan). Si no dispone de PIM, todavía puede hacer clic en **Administrar en PIM** para registrarse y obtener una versión de prueba. Privileged Identity Management requiere un [Plan de licencias de Azure AD Premium P2](../privileged-identity-management/subscription-requirements.md).

![lista de miembros de un rol de administrador](./media/directory-manage-roles-portal/member-list.png)

Si es un administrador Global o un administrador de roles con privilegios, puede fácilmente agregar o quitar miembros, filtrar la lista o seleccionar un miembro para ir a la página de usuario y ver sus roles asignados activos. 

## <a name="detailed-role-permissions-in-the-portal"></a>Permisos de rol detallados en el portal

Cuando esté viendo los miembros de un rol, haga clic en **Descripción** para ver una lista completa de los permisos otorgados por la asignación de roles. En la página se incluyen vínculos a documentación relevante que le guiarán a través de la administración de los roles de directorio.

![lista de permisos para un rol de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de Administrador, vea [Asignación de roles de administrador](directory-assign-admin-roles.md).
* Para los permisos de usuario predeterminados, vea una [comparación de los permisos de usuario miembro y de invitado predeterminados](../fundamentals/users-default-permissions.md).
