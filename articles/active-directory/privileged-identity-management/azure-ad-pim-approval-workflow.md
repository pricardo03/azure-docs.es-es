---
title: Aprobar o rechazar solicitudes para los roles de Azure AD en PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo aprobar o denegar las solicitudes de roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47e9033b33c70a72a1685696f75a442e88eed033
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60289776"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Aprobar o rechazar solicitudes para los roles de Azure AD en PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), puede configurar roles para requerir la aprobación para la activación y elija uno o varios usuarios o grupos como aprobadores delegados. Los aprobadores delegados tienen 24 horas para aprobar las solicitudes. Si no se aprueba una solicitud en un plazo de 24 horas, el usuario apto debe volver a enviar una nueva solicitud. El período de tiempo de aprobación de 24 horas no es configurable.

Siga los pasos descritos en este artículo para aprobar o rechazar solicitudes de roles de Azure AD.

## <a name="view-pending-requests"></a>Ver solicitudes en espera

Como un aprobador delegado, recibirá una notificación por correo electrónico cuando una solicitud de rol de Azure AD está pendiente de aprobación. Puede ver estas solicitudes pendientes en PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Aprobar solicitudes**.

    ![Roles de Azure PIM AD: Roles](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Verá una lista de solicitudes pendientes de su aprobación.

## <a name="approve-requests"></a>Aprobar solicitudes

1. Seleccione las solicitudes que quiera aprobar y luego haga clic en **Aprobar** para abrir el panel Aprobar las solicitudes seleccionadas.

    ![Lista de solicitudes de aprobación de PIM](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. En el cuadro **Razón de la aprobación**, escriba una razón.

    ![PIM: Aprobar las solicitudes seleccionadas](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Haga clic en **Approve** (Aprobar).

    El símbolo de estado se actualizará con la aprobación.

    ![PIM: Aprobar las solicitudes seleccionadas](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Denegar solicitudes

1. Seleccione las solicitudes que quiera denegar y luego haga clic en **Denegar** para abrir el panel Denegar las solicitudes seleccionadas.

    ![Lista de solicitudes de aprobación de PIM](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. En el cuadro **Razón de la denegación**, escriba una razón.

    ![PIM: Denegar las solicitudes seleccionadas](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Haga clic en **Denegar**.

    El símbolo de estado se actualizará con la denegación.

## <a name="next-steps"></a>Pasos siguientes

- [Notificaciones por correo electrónico en PIM](pim-email-notifications.md)
- [Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM](pim-resource-roles-approval-workflow.md)
