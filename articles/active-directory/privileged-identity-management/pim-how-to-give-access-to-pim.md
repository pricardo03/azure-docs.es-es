---
title: 'Concesión de acceso a otros administradores para administrar PIM: Azure | Microsoft Docs'
description: Aprenda a conceder acceso a otros administradores para administrar Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0d8cb77cda97c3141987fa8b0ea39541ace6db3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191748"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>Concesión de acceso a otros administradores para administrar PIM

El administrador global que habilita Privileged Identity Management (PIM) de Azure AD para una organización, obtiene automáticamente asignaciones de roles y acceso a PIM. Nadie más obtiene acceso de escritura de forma predeterminada, ni siquiera otros administradores globales. Otros administradores globales, administradores de seguridad y lectores de seguridad tienen acceso de solo lectura a PIM. Para conceder acceso a PIM, el primer usuario puede asignar a otros el rol **Administrador de roles con privilegios**.

> [!NOTE]
> La administración de PIM requiere Azure MFA. Dado que las cuentas de Microsoft no se pueden registrar en Azure MFA, un usuario que inicia sesión con una cuenta de Microsoft no puede tener acceso a PIM.

Asegúrese de que siempre haya al menos dos usuarios en un rol de administrador de roles con privilegios, por si se diera el caso de que a un usuario se le impida el acceso o su cuenta se elimine.

## <a name="grant-access-to-manage-pim"></a>Concesión de acceso para administrar PIM

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Roles**.

    ![Roles de directorio de Azure AD de PIM: roles](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Haga clic en el rol **Administrador de rol con privilegios** para abrir la página de miembros.

    ![Administrador de roles con privilegios: miembros](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Haga clic en **Agregar miembro** para abrir el panel Agregar miembros administrados.

1. Haga clic en **Seleccionar miembros** para abrir el panel de selección de miembros.

    ![Administrador de roles con privilegios: seleccionar miembros](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Seleccione un miembro y, luego, haga clic en **Seleccionar**.

1. Haga clic en **Aceptar** para que el miembro sea apto para el rol **Administrador de roles con privilegios**.

    Cuando se asigna un nuevo rol a alguien en PIM, esta persona se configura automáticamente como **Apta** para activar el rol.

1. Para convertir el miembro en permanente, haga clic en el usuario en la lista de miembros del administrador de roles con privilegios.

1. Haga clic en **Más** y, a continuación, en **Hacer permanente** para que la asignación sea permanente.

    ![Administrador de roles con privilegios: hacer permanente](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Envíe un vínculo al usuario para [empezar a usar PIM](pim-getting-started.md).

## <a name="remove-access-to-manage-pim"></a>Quitar el acceso para administrar PIM

Antes de quitar a alguien del rol de administrador de roles con privilegios, asegúrese siempre de que seguirá habiendo al menos dos usuarios asignados.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Roles**.

1. Haga clic en el rol **Administrador de rol con privilegios** para abrir la página de miembros.

1. Agregue una marca de verificación junto al usuario que quiera quitar y, a continuación, haga clic en **Quitar miembro**.

    ![Administrador de rol con privilegios: quitar miembros](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. En el mensaje que aparece y le pregunta si quiere quitar el miembro del rol, haga clic en **Sí**.

## <a name="next-steps"></a>Pasos siguientes

- [Primer uso de PIM](pim-getting-started.md)
