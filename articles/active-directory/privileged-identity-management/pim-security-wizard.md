---
title: Asistente para la seguridad de los roles de Azure AD en PIM | Microsoft Docs
description: Describe el asistente para la seguridad que puede usar con la finalidad de convertir las asignaciones de roles de Azure AD con privilegios permanentes en aptos mediante Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 337e4ee0c0b60e4b505fa9e4a2654b1a30b4c71f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194137"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Asistente para la seguridad de los roles de Azure AD en PIM

Si es la primera persona que ejecuta Azure AD Privileged Identity Management (PIM) en su organización, se le presentará un asistente. El asistente le ayuda a comprender los riesgos de seguridad de las identidades con privilegios y a usar PIM para reducirlos. No tiene que realizar cambios en las asignaciones de roles existentes en el asistente; si lo prefiere, puede hacerlo más adelante.

## <a name="wizard-overview"></a>Información general sobre el asistente

Antes de que su organización comienza a usar PIM, todas las asignaciones de roles son permanentes: los usuarios siempre tienen estos roles aunque en el momento actual no necesiten sus privilegios. El primer paso del asistente muestra una lista de roles con privilegios elevados y cuántos usuarios tienen actualmente esos roles. Puede explorar en profundidad un rol determinado para conocer mejor a los usuarios en caso de que alguno de ellos no le resulte familiar.

El segundo paso del asistente le ofrece la oportunidad de cambiar las asignaciones de roles del administrador.  

> [!WARNING]
> Es importante que tenga al menos un administrador global y más de un administrador de roles con privilegios con una cuenta de organización (no una cuenta Microsoft). Si solo hay un administrador de roles con privilegios, la organización no podrá administrar PIM si esa cuenta se elimina.
> Además, mantenga las asignaciones de roles permanente si un usuario tiene una cuenta de Microsoft (una cuenta que usan para iniciar sesión en servicios de Microsoft como Skype y Outlook.com). Si tiene pensado exigir MFA para la activación de ese rol, ese usuario se bloqueará.

## <a name="run-the-wizard"></a>Ejecutar el asistente

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD** y, luego, en **Asistente**.

    ![Roles de AD Azure: Asistente](./media/pim-security-wizard/wizard-start.png)

1. Haga clic en **1 Detectar roles con privilegios**.

1. Revise la lista de roles con privilegios para ver qué usuarios son permanentes o aptos.

    ![Detección de usuarios de roles con privilegios](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Haga clic en **Siguiente** para seleccionar los miembros que quiere convertir en aptos.

    ![Conversión de los miembros en aptos](./media/pim-security-wizard/convert-members-eligible.png)

1. Una vez haya seleccionado los miembros, haga clic en **Siguiente**.

    ![Revisión de los cambios](./media/pim-security-wizard/review-changes.png)

1. Haga clic en **Aceptar** para convertir las asignaciones permanentes en aptas.

    Cuando se complete la conversión, verá una notificación.

    ![Notificaciones](./media/pim-security-wizard/notification-completion.png)

Si necesita convertir otras asignaciones de roles con privilegios en aptas, puede volver a ejecutar el asistente. Si desea usar la interfaz de PIM en lugar del asistente, consulte [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [Concesión de acceso a otros administradores para administrar PIM](pim-how-to-give-access-to-pim.md)
