---
title: 'Envío de notificaciones por correo electrónico en PIM: Azure Active Directory | Microsoft Docs'
description: Se describen las notificaciones por correo electrónico en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/05/2019
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9aa9da83a7a6e97f5b721dad550831fe2645ffd3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60289186"
---
# <a name="email-notifications-in-pim"></a>Notificaciones por correo electrónico en PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) le permite saber cuándo se producen eventos importantes, como el momento en que se asigna o se activa un rol. PIM le mantiene informado mediante el envío de notificaciones de correo electrónico, a usted y a otros participantes. Estos mensajes de correo electrónico también podrían incluir vínculos a tareas pertinentes, tales como la activación o renovación de un rol. En este artículo se describe el aspecto de estos correos electrónicos, cuándo se envían y quién los recibe.

## <a name="sender-email-address-and-subject-line"></a>Dirección de correo electrónico del remitente y línea de asunto

Los correos electrónicos que se envían desde PIM a Azure AD y los roles de recursos de Azure tienen como remitente la siguiente dirección de correo electrónico :

- Dirección de correo electrónico:  **azure-noreply\@microsoft.com**
- Nombre para mostrar: Microsoft Azure

Estos mensajes de correo electrónico incluyen un prefijo **PIM** en la línea de asunto. Este es un ejemplo:

- PIM: a Alain Charon se le ha asignado el rol de lector de copias de seguridad de forma permanente

## <a name="pim-emails-for-azure-ad-roles"></a>Correos electrónicos de PIM para roles de Azure AD

PIM envía mensajes de correo electrónico cuando se producen los eventos siguientes para roles de Azure AD:

- Cuando una activación de roles con privilegios está pendiente de aprobación
- Cuando se completa una solicitud de activación de roles con privilegios
- Cuando se habilita Azure AD PIM

El destinatario de estos correos electrónicos para roles de Azure AD depende de su rol, el evento y las notificaciones de configuración:

| Usuario | La activación de roles está pendiente de aprobación | La solicitud de activación de roles está completa | PIM está habilitado |
| --- | --- | --- | --- |
| Administrador de roles con privilegios</br>(Activado/apto) | Sí</br>(solo si no se especifican aprobadores explícitos) | Sí* | Sí |
| Administrador de seguridad</br>(Activado/apto) | Sin | Sí* | Sí |
| Administrador global</br>(Activado/apto) | Sin | Sí* | Sí |

\* Si el parámetro [**Notificaciones**](pim-how-to-change-default-settings.md#notifications) se establece en **Habilitar**.

Este es un correo electrónico de ejemplo que se envía cuando un usuario activa un rol de Azure AD para la organización ficticia Contoso.

![Nuevo correo electrónico de PIM para roles de Azure AD](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-pim-digest-email-for-azure-ad-roles"></a>Correo electrónico semanal de resumen de PIM para roles de Azure AD

Se envía un correo electrónico de resumen semanal de PIM para los roles de Azure AD a los administradores de roles con privilegios, los administradores de seguridad y los administradores globales que han habilitado PIM. Este correo electrónico semanal proporciona una instantánea de las actividades de PIM para la semana, así como las asignaciones de roles con privilegios. Solo está disponible para los inquilinos en la nube pública. Este es un ejemplo de correo electrónico:

![Correo electrónico semanal de resumen de PIM para roles de Azure AD](./media/pim-email-notifications/email-directory-weekly.png)

Este correo electrónico incluye cuatro iconos:

| Icono | DESCRIPCIÓN |
| --- | --- |
| **Users activated** (Activaciones de usuarios) | Número de veces que los usuarios activaron su rol apto dentro del inquilino. |
| **Users made permanent** (Usuarios convertidos en permanentes) | Número de veces que los usuarios con una asignación apta se hicieron permanentes. |
| **Role assignments in PIM** (Asignaciones de roles en PIM) | Número de veces que los usuarios recibieron un rol apto en PIM. |
| **Role assignments outside of PIM** (Asignaciones de roles fuera de PIM) | Número de veces que los usuarios recibieron un rol permanente fuera de PIM (dentro de Azure AD). |

En la sección **Overview of your top roles** (Información general de los roles principales) figuran los cinco principales roles en el inquilino según el número total de administradores permanentes y aptos para cada rol. El vínculo **Realizar acción** abre el [Asistente de PIM](pim-security-wizard.md) donde puede convertir administradores permanentes en administradores aptos en lotes.

## <a name="pim-emails-for-azure-resource-roles"></a>Correos electrónicos de PIM para roles de recursos de Azure

PIM envía mensajes de correo electrónico a los propietarios y administradores de acceso de usuario cuando se producen los siguientes eventos para los roles de recursos de Azure:

- Cuando una asignación de roles está pendiente de aprobación
- Cuando se asigna un rol
- Cuando un rol está a punto de expirar
- Cuando un rol se puede ampliar
- Cuando un rol se está renovando por un usuario final
- Cuando se completa una solicitud de activación de rol

PIM envía mensajes de correo electrónico a los usuarios finales cuando se producen los siguientes eventos para los roles de recursos de Azure:

- Cuando se asigna un rol al usuario
- Cuando expira un rol del usuario
- Cuando se amplía un rol del usuario
- Cuando se completa una solicitud de activación de rol del usuario

Este es un correo electrónico de ejemplo que se envía cuando se asigna a un usuario un rol de recursos de Azure para la organización ficticia Contoso.

![Nuevo correo electrónico de PIM para roles de recursos de Azure](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de Azure AD en PIM](pim-how-to-change-default-settings.md)
- [Aprobación o rechazo de solicitudes para los roles de Azure AD en PIM](azure-ad-pim-approval-workflow.md)
