---
title: Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a aprobar o rechazar solicitudes para los roles de directorio de Azure AD en Azure AD Privileged Identity Management (PIM).
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
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1c4b3819494ecb4c0897edeb08e2ce45d867b626
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179079"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM

Con Azure AD Privileged Identity Management (PIM), puede configurar roles para requerir la aprobación para la activación y elegir uno o varios usuarios o grupos como aprobadores delegados. Siga los pasos que se describen en este artículo para aprobar o denegar solicitudes de roles de directorio de Azure AD.

## <a name="view-pending-requests"></a>Ver solicitudes en espera

Como aprobador delegado, recibirá una notificación por correo electrónico cuando una solicitud de rol de directorio de Azure AD está pendiente de su aprobación. Puede ver estas solicitudes pendientes en PIM.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de directorio de Azure AD**.

1. Haga clic en **Aprobar solicitudes**.

    ![Roles de directorio de Azure AD de PIM: roles](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

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
