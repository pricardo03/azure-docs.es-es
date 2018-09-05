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
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7bf1e437e97fdb4d929af23bd7b2a9abb49268df
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189165"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM

Con Privileged Identity Management, puede configurar roles para requerir la aprobación para la activación y elegir uno o varios usuarios o grupos como aprobadores delegados.

## <a name="view-pending-approvals-requests"></a>Ver aprobaciones o solicitudes pendientes

Como un aprobador delegado, recibirá notificaciones por correo electrónico cuando una solicitud está pendiente de aprobación. Para ver estas solicitudes en el portal de PIM, en el nuevo panel de navegación, seleccione la pestaña "Solicitudes de aprobación pendientes" en la barra de navegación izquierda.

![](media/azure-ad-pim-approval-workflow/image023.png)

Ahí se mostrará una lista de solicitudes pendientes de aprobación:

![](media/azure-ad-pim-approval-workflow/image024.png)

## <a name="approve-or-deny-requests-for-role-elevation-single-andor-bulk"></a>Aprobación o rechazo de solicitudes de elevación de rol (de forma individual o masiva)

Seleccione las solicitudes que desea aprobar o rechazar y haga clic en el botón de la barra de acciones correspondiente a su decisión:

![](media/azure-ad-pim-approval-workflow/image025.png)

## <a name="provide-justification-for-my-approvaldenial"></a>Proporcionar una justificación de la aprobación o el rechazo

Se abrirá una nueva hoja para aprobar o rechazar varias solicitudes a la vez. Escriba una justificación de la decisión que ha tomado y haga clic en los botones para aprobar o rechazar en la parte inferior de la hoja:

![](media/azure-ad-pim-approval-workflow/image029.png)

Una vez completado el proceso de solicitud, el símbolo de estado reflejará la decisión adoptada (en este ejemplo, la decisión es aprobar):

![](media/azure-ad-pim-approval-workflow/image031.png)

## <a name="next-steps"></a>Pasos siguientes

- [Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM](pim-resource-roles-approval-workflow.md)
- [Notificaciones por correo electrónico en PIM](pim-email-notifications.md)
