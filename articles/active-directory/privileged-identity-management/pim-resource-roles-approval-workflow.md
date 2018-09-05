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
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e5cda31af5ac95e7ebe2b858b1d7355ea3f2a6bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189811"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM

Para aprobar o denegar una solicitud es preciso ser miembro de la lista de aprobadores. 

1. En PIM, seleccione **Aprobar solicitudes** en la pestaña del menú izquierdo y busque la solicitud.

   ![Panel "Aprobar solicitudes"](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Seleccione la solicitud, proporcione una justificación para la decisión y seleccione **Aprobar** o **Denegar**. Se resuelve la solicitud.

   ![Solicitud seleccionada con información detallada](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Notificaciones de flujo de trabajo

Estos son algunos datos sobre las notificaciones de flujo de trabajo:

- Todos los miembros de la lista de aprobadores reciben una notificación por correo electrónico cuando una solicitud de un rol está pendiente de su revisión. Las notificaciones por correo electrónico incluyen un vínculo directo a la solicitud donde el aprobador puede aprobarla o rechazarla.
- Las solicitudes las resuelve el primer miembro de la lista que aprueba o rechaza. 
- Cuando un aprobador responde a la solicitud, se envía una notificación a todos los miembros de la lista de aprobadores. 
- Los administradores de recursos reciben una notificación cuando un miembro aprobado se activa en su rol. 

>[!Note]
>Un administrador de recursos que cree que un miembro aprobado no debe estar activo puede quitar la asignación de rol activo en PIM. Aunque los administradores de recursos no reciben notificación de solicitudes pendientes, salvo que sean miembros de la lista de aprobadores, pueden ver y cancelar las solicitudes pendientes de todos los usuarios al ver las solicitudes pendientes en PIM. 

## <a name="next-steps"></a>Pasos siguientes

- [Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM](azure-ad-pim-approval-workflow.md)
- [Notificaciones por correo electrónico en PIM](pim-email-notifications.md)
