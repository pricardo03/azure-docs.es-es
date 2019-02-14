---
title: Delegación de invitaciones de la colaboración B2B de Azure Active Directory | Microsoft Docs
description: Las propiedades de un usuario de colaboración B2B de Azure Active Directory son configurables.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28ac32a97f4c42040d538829e9870a43b508f50e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164743"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegación de invitaciones de la colaboración B2B de Azure Active Directory

Con la colaboración negocio a negocio (B2B) de Azure Active Directory (Azure AD), no tiene que ser un administrador global para enviar invitaciones. Puede usar directivas y delegar invitaciones en usuarios cuyos roles les permitan enviar invitaciones. Una nueva forma importante de delegar las invitaciones de usuarios invitados es a través del rol de invitador invitado.

## <a name="guest-inviter-role"></a>Rol de invitador de personas
Podemos asignamos al usuario el rol de invitador invitado para enviar invitaciones. No tiene que ser miembro del rol administrador global para enviar invitaciones. De forma predeterminada, los usuarios normales también pueden invocar la API de invitación, a menos que un administrador global haya deshabilitado las invitaciones para los usuarios normales. Un usuario también puede invocar la API mediante Azure Portal o PowerShell.

Este es un ejemplo que muestra cómo usar PowerShell para agregar un usuario al rol Invitador de personas:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Controlar quién puede invitar

En Azure Active Directory, seleccione **Configuración de usuario**. En **Usuarios externos**, seleccione **Administrar la configuración de colaboración externa**.

> [!NOTE]
> La opción **Configuración de colaboración externa** también está disponible en la página **Relaciones de organización**. En Azure Active Directory, en **Administrar**, vaya a **Relaciones de organización** > **Configuración**.

![Configuración de colaboración externa](./media/delegate-invitations/control-who-to-invite.png)

Gracias a la colaboración B2B de Azure AD, el administrador de inquilinos puede establecer las siguientes directivas de invitación:

- Desactivar invitaciones
- Solo los administradores y usuarios del rol Invitador de personas pueden invitar
- Los administradores, el rol Invitador de personas y los miembros pueden invitar
- Todos los usuarios, incluidos los invitados, pueden invitar

De forma predeterminada, los inquilinos se establecen en #4. (Todos los usuarios, incluidos los invitados, pueden invitar a usuarios B2B).

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre la colaboración de B2B de Azure AD:

- [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
- [Incorporación de usuarios de colaboración B2B sin invitación](add-user-without-invite.md)
- [Incorporación de usuarios de colaboración B2B a un rol](add-guest-to-role.md)


