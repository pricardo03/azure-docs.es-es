---
title: Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM | Microsoft Docs
description: Aprenda a aprobar o rechazar solicitudes para los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c661f2662f48c5aaece142cb4a2223ab8a6d0853
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666598"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM

Con Azure AD Privileged Identity Management (PIM), puede configurar roles para requerir la aprobación para la activación y elegir uno o varios usuarios o grupos como aprobadores delegados. Siga los pasos que se describen en este artículo para aprobar o denegar solicitudes para los roles de recursos de Azure.

## <a name="view-pending-requests"></a>Ver solicitudes en espera

Como aprobador delegado, recibirá una notificación por correo electrónico cuando una solicitud de rol de recursos de Azure está pendiente de su aprobación. Puede ver estas solicitudes pendientes en PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Aprobar solicitudes**.

    ![Recursos de Azure: Aprobar solicitudes](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    En la sección **Solicitudes de activación de roles**, verá una lista de solicitudes pendientes de su aprobación.

## <a name="approve-requests"></a>Aprobar solicitudes

1. Busque la solicitud que desea aprobar y haga clic en ella. Aparece un panel de aprobación.

    ![Panel Aprobar solicitudes](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. En el cuadro **Justificación**, escriba un motivo.

1. Haga clic en **Approve** (Aprobar).

    Aparecerá una notificación con su aprobación.

    ![Aprobar la notificación](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>Denegar solicitudes

1. Busque la solicitud que desea denegar y haga clic en ella. Aparece un panel de aprobación.

    ![Panel Aprobar solicitudes](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. En el cuadro **Justificación**, escriba un motivo.

1. Haga clic en **Denegar**.

    Aparecerá una notificación con su denegación.

## <a name="workflow-notifications"></a>Notificaciones de flujo de trabajo

A continuación proporcionamos información sobre las notificaciones de flujo de trabajo:

- Todos los miembros de la lista de aprobadores reciben una notificación por correo electrónico cuando una solicitud de un rol está pendiente de su revisión. Las notificaciones por correo electrónico incluyen un vínculo directo a la solicitud donde el aprobador puede aprobarla o rechazarla.
- Las solicitudes las resuelve el primer miembro de la lista que aprueba o rechaza.
- Cuando un aprobador responde a la solicitud, se envía una notificación a todos los miembros de la lista de aprobadores.
- Los administradores de recursos reciben una notificación cuando un miembro aprobado se activa en su rol.

>[!Note]
>Un administrador de recursos que cree que un miembro aprobado no debe estar activo puede quitar la asignación de rol activo en PIM. Aunque los administradores de recursos no reciben notificación de solicitudes pendientes, salvo que sean miembros de la lista de aprobadores, pueden ver y cancelar las solicitudes pendientes de todos los usuarios al ver las solicitudes pendientes en PIM. 

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en PIM](pim-resource-roles-renew-extend.md)
- [Notificaciones por correo electrónico en PIM](pim-email-notifications.md)
- [Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM](azure-ad-pim-approval-workflow.md)
