---
title: Visualización de miembros de un rol de administrador en Azure Active Directory | Microsoft Docs
description: Ahora puede ver y administrar miembros de un rol de administrador de Azure AD en el portal. Para aquellos que administran con frecuencia las asignaciones de roles.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/02/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 39f7d69482845af48b1dce8c2e51f4acf77bf4fb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448067"
---
# <a name="view-all-members-of-an-administrator-role-in-azure-active-directory"></a>Visualización de todos los miembros de un rol de administrador en Azure Active Directory

Ahora puede ver y administrar a todos los miembros de los roles de administrador en el portal de Azure Active Directory. Si administra las asignaciones de roles con frecuencia, probablemente preferirá esta experiencia. Y si alguna vez se ha preguntado "¿qué es lo que estos roles realmente hacen?", puede ver una lista detallada de los permisos para cada uno de los roles de administrador de Azure AD.

También es fácil ver también sus propios permisos. Haga clic en **Su rol** para obtener acceso rápido a la página de usuario para obtener una lista de todos los roles asignados activos. Haga clic en el botón de puntos suspensivos a la derecha de cada fila para abrir la descripción detallada de la función.

![lista de roles en el portal de Azure AD](./media/directory-manage-roles-portal/role-list.png)

Seleccione la fila completa para ver la lista de miembros asignados. Puede seleccionar **Administrar en PIM** para obtener funcionalidades de administración adicionales. Los administradores de roles con privilegios puede cambiar las asignaciones "Permanentes" (siempre activas en el rol) a "Elegibles" (en el rol solo cuando se elevan). Si no dispone de PIM, todavía puede seleccionar **Administrar en PIM** para registrarse y obtener una versión de prueba. Privileged Identity Management requiere un [Plan de licencias de Azure AD Premium P2](./privileged-identity-management/subscription-requirements.md).

![lista de miembros de un rol de administrador](./media/directory-manage-roles-portal/member-list.png)

Si es un administrador global o un administrador de roles con privilegios, puede agregar o quitar miembros, filtrar la lista o seleccionar un miembro fácilmente para ir a la página de usuario y ver sus roles asignados activos. 

## <a name="role-details-in-the-portal"></a>Detalles del rol en el portal

Cuando esté viendo los miembros de un rol, seleccione **Descripción** para ver una lista completa de permisos otorgados por la asignación de roles. La página incluye vínculos a documentación relevante que le guiarán a través de administración de roles de directorio.

![lista de permisos para un rol de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](users-groups-roles/directory-assign-admin-roles.md) (Asignación de roles de administrador).
* Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](./fundamentals/users-default-permissions.md).
