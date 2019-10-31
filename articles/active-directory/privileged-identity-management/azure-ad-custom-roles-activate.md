---
title: Activación de un rol personalizado de Azure AD en Privileged Identity Management (PIM) | Microsoft Docs
description: Procedimiento para activar un rol personalizado de Azure AD en Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ac8acf2193442de5c85e0a1134d46330821c77b
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756442"
---
# <a name="activate-an-azure-ad-custom-role-in-privileged-identity-management"></a>Activación de un rol personalizado de Azure AD en Privileged Identity Management

Privileged Identity Management en Azure Active Directory (Azure AD) ahora admite la asignación Just-In-Time y de duración limitada a los roles personalizados creados para la administración de aplicaciones en la experiencia administrativa de Administración de identidad y acceso. Para obtener más información sobre cómo crear roles personalizados para delegar la administración de aplicaciones en Azure AD, vea [Roles de administrador personalizados en Azure Active Directory (versión preliminar)](../users-groups-roles/roles-custom-overview.md).

> [!NOTE]
> Los roles personalizados de Azure AD no se integran con los roles de directorio integrados durante la versión preliminar. Una vez que la funcionalidad esté disponible con carácter general, la administración de roles tendrá lugar en la experiencia de roles integrados.

## <a name="activate-a-role"></a>Activación de un rol

Cuando necesite activar un rol personalizado de Azure AD, seleccione la opción de navegación Mis roles de Privileged Identity Management para solicitar la activación.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Abra Azure AD [Privileged Identity Management](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart).

1. Seleccione **Roles personalizados de Azure AD** para ver una lista de las asignaciones de roles personalizados de Azure AD válidas.

   ![Visualización de la lista de asignaciones de roles personalizados de Azure AD válidas](./media/azure-ad-custom-roles-activate/view-preview-roles.png)

1. En la página **Roles personalizados de Azure AD (versión preliminar)** , busque la asignación que necesite.
1. Seleccione **Activar el rol** para abrir la página **Activar**.
1. Si el rol requiere la autenticación multifactor, seleccione **Compruebe su identidad antes de proceder**. Se le requiere que se autentique solo una vez por sesión.
1. Seleccione **Comprobar mi identidad** y siga las instrucciones para realizar cualquier comprobación de seguridad adicional.
1. Para especificar un ámbito de aplicación personalizado, seleccione **Ámbito** para abrir el panel de filtro. Debe solicitar acceso a un rol en el ámbito mínimo necesario. Si la asignación está en un ámbito de aplicación, solo la puede activar en ese ámbito.

   ![Asignación de un ámbito de recurso de Azure AD a la asignación de roles](./media/azure-ad-custom-roles-activate/assign-scope.png)

1. Si es necesario, especifique una hora de inicio de activación personalizada. Cuando se usa, el miembro del rol se activa en el momento especificado.
1. En el campo **Razón**, escriba el motivo de la solicitud de activación. Se pueden hacer obligatorios o no en la configuración del rol.
1. Seleccione **Activar**.

Si el rol no requiere aprobación, se activa según la configuración y se agrega a la lista de roles activos. Si quiere usar el rol activado, comience con los pasos que se describen en [Asignación de un rol personalizado de Azure AD en Privileged Identity Management](azure-ad-custom-roles-assign.md).

Si el rol requiere aprobación para activarse, recibirá una notificación de Azure en la que se le informará de que la solicitud está pendiente de aprobación.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de un rol personalizado de Azure AD](azure-ad-custom-roles-assign.md)
- [Eliminación o actualización de una asignación de roles personalizados de Azure AD](azure-ad-custom-roles-update-remove.md)
- [Configuración de una asignación de roles personalizados de Azure AD](azure-ad-custom-roles-configure.md)
- [Definiciones de roles en Azure AD](../users-groups-roles/directory-assign-admin-roles.md)
