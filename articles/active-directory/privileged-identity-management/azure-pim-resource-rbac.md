---
title: Ver quién tiene roles de recursos de Azure en PIM | Microsoft Docs
description: Vea quién tiene roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: aee172bc6fc77aaac8d2d52037a481fdb976d308
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188975"
---
# <a name="view-who-has-azure-resource-roles-in-pim"></a>Ver quién tiene roles de recursos de Azure en PIM

Con Privileged Identity Management (PIM) de Azure Active Directory, puede administrar, controlar y supervisar el acceso a los recursos de Azure dentro de su organización. Esto incluye las suscripciones, los grupos de recursos e incluso las máquinas virtuales. Todos los recursos de Azure Portal que aprovechan la funcionalidad de control de acceso basado en rol (RBAC) de Azure también pueden aprovechar todas las ventajas de las funcionalidades de seguridad y administración del ciclo de vida que ofrece PIM de Azure AD. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM para recursos de Azure ayuda a los administradores de recursos

- Consulte qué usuarios y grupos son roles asignados a los recursos de Azure que administra
- Habilite el acceso "Just-In-Time", a petición, para administrar recursos como las suscripciones, los grupos de recursos, etc.
- Anule el acceso de los usuarios /grupos de recursos asignados automáticamente con una nueva configuración de asignación de tiempo
- Asigne acceso temporal a los recursos para tareas rápidas u horarios de guardia
- Establezca Multi-Factor Authentication para los recursos en cualquier rol incorporado o personalizado 
- Obtenga informes sobre la actividad de los recursos relativa al acceso durante una sesión de usuario activa
- Reciba alertas cuando se asigne acceso a los recursos para nuevos usuarios o grupos, así como cuando activen asignaciones aptas

## <a name="view-activation-and-azure-resource-activity"></a>Visualización de la activación y la actividad de los recursos de Azure

Si necesita ver las acciones que realizó un usuario específico en varios recursos, puede revisar la actividad de los recursos de Azure asociada a un período de activación específico (para usuarios aptos). Primero, seleccione el usuario de la vista Miembros o de la lista de miembros de un rol específico. Como resultado se muestra una gráfica de las acciones del usuario en los recursos de Azure por fecha y las activaciones de rol recientes durante ese período.

![](media/azure-pim-resource-rbac/user-details.png)

Al seleccionar la activación de un rol específico se muestran los detalles de la activación del rol y la actividad de los recursos de Azure correspondiente al tiempo de actividad de ese usuario.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="review-who-has-access-in-a-subscription"></a>Comprobación de quién tiene acceso a una suscripción

Para revisar las asignaciones de rol en la suscripción, seleccione la pestaña Miembros en el panel de navegación izquierdo o Roles y elija un rol específico para revisar los miembros. 

Seleccione Revisar en la barra de acción para ver las revisiones de acceso existentes y seleccione Agregar para crear una nueva.

![](media/azure-pim-resource-rbac/owner.png)

[Más información sobre las revisiones de acceso](pim-how-to-perform-security-review.md)

>[!NOTE]
Actualmente, solo se admiten revisiones para los tipos de recursos de suscripción.

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de recursos de Azure en PIM](pim-resource-roles-assign-roles.md)
- [Aprobación o rechazo de solicitudes de roles de recursos de Azure en PIM](pim-resource-roles-approval-workflow.md)
- [Roles integrados de Azure](../../role-based-access-control/built-in-roles.md)
