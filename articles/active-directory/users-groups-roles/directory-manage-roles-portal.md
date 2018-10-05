---
title: Visualización y asignación de permisos de roles de administrador en Azure Active Directory | Microsoft Docs
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
ms.date: 09/25/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: cb2e5286eb8e910b555e221242a735f00dff4778
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182831"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Visualización y asignación de roles de administrador en Azure Active Directory

Ahora puede ver y administrar todos los miembros de los roles de administrador en el portal de Azure Active Directory. Si administra las asignaciones de roles con frecuencia, probablemente preferirá esta experiencia. Y si alguna vez se ha preguntado "¿qué es lo que realmente hacen estos roles?", puede ver una lista detallada de los permisos para cada uno de los roles de administrador de Azure AD.

También es fácil ver sus propios permisos. Haga clic en **Su rol** para obtener acceso rápido a la página de usuario para ver una lista de todos los roles asignados activos. Haga clic en el botón de puntos suspensivos a la derecha de cada fila para abrir la descripción detallada del rol.

![lista de roles en el portal de Azure AD](./media/directory-manage-roles-portal/role-list.png)

Seleccione la fila de un rol para ver los usuarios asignados a este. Puede hacer clic en **Administrar en PIM** para obtener funcionalidades de administración adicionales. Los administradores de roles con privilegios pueden cambiar las asignaciones "Permanentes" (siempre activas en el rol) a "Elegibles" (en el rol solo cuando se elevan). Si no dispone de PIM, todavía puede hacer clic en **Administrar en PIM** para registrarse y obtener una versión de prueba. Privileged Identity Management requiere un [Plan de licencias de Azure AD Premium P2](../privileged-identity-management/subscription-requirements.md).

![lista de miembros de un rol de administrador](./media/directory-manage-roles-portal/member-list.png)

Si es un administrador global o un administrador de roles con privilegios, puede agregar o quitar miembros, filtrar la lista o seleccionar un miembro fácilmente para ver sus roles asignados activos.

## <a name="view-role-permissions"></a>Visualización de permisos de los roles

Cuando esté viendo los miembros de un rol, haga clic en **Descripción** para ver una lista completa de los permisos otorgados por la asignación de roles. En la página se incluyen vínculos a documentación relevante que le guiarán a través de la administración de los roles de directorio.

![lista de permisos para un rol de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](directory-assign-admin-roles.md) (Asignación de roles de administrador).
* Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).
