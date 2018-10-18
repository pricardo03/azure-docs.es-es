---
title: 'Envío de notificaciones por correo electrónico en PIM: Azure | Microsoft Docs'
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
ms.component: pim
ms.date: 09/07/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: de1d29d3ab1b370257c3a2d6b6ff9f677197fc2a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303071"
---
# <a name="email-notifications-in-pim"></a>Notificaciones por correo electrónico en PIM

Cuando se producen eventos clave en Azure AD Privileged Identity Management (PIM), se envían notificaciones por correo electrónico. Por ejemplo, PIM envía mensajes de correo electrónico con los siguientes eventos:

- Cuando una activación de roles con privilegios está pendiente de aprobación
- Cuando se completa una solicitud de activación de roles con privilegios
- Cuando se activa un rol con privilegios
- Cuando se asigna un rol con privilegios
- Cuando se habilita Azure AD PIM

Se envían notificaciones por correo electrónico a los siguientes administradores:

- Administrador de roles con privilegios
- Administrador de seguridad

También se envían notificaciones por correo electrónico al usuario final que tiene el rol con privilegios para los siguientes eventos:

- Cuando se completa una solicitud de activación de roles con privilegios
- Cuando se asigna un rol con privilegios

A partir de finales de julio de 2018, las notificaciones por correo electrónico enviadas a través de PIM tendrán una nueva dirección de correo electrónico de remitente y un nuevo diseño visual. Esta actualización afectará a PIM para Azure AD y a PIM para recursos de Azure. Todos los eventos que hayan desencadenado anteriormente una notificación por correo electrónico seguirán enviando un mensaje de correo electrónico. Algunos correos electrónicos tendrán contenido actualizado que proporciona información más dirigida.

## <a name="sender-email-address"></a>Dirección de correo electrónico del remitente

A partir de finales de julio de 2018, las notificaciones por correo electrónico tienen la siguiente dirección:

- Dirección de correo electrónico: **azure-noreply@microsoft.com**
- Nombre para mostrar: Microsoft Azure

Anteriormente, las notificaciones por correo electrónico tenían la siguiente dirección:

- Dirección de correo electrónico: **azureadnotifications@microsoft.com**
- Nombre para mostrar: servicio de notificación de Microsoft Azure AD

## <a name="email-subject-line"></a>Línea de asunto del correo electrónico

A partir de finales de julio de 2018, en las notificaciones por correo electrónico para los roles de recurso de Azure AD y Azure aparecerá el prefijo **PIM** prefijo en la línea del asunto. Este es un ejemplo:

- PIM: a Alain Charon se le ha asignado el rol de lector de copias de seguridad de forma permanente.

## <a name="pim-emails-for-azure-ad-roles"></a>Correos electrónicos de PIM para roles de Azure AD

A partir de finales de julio de 2018, las notificaciones por correo electrónico de PIM para roles de Azure AD tienen un diseño actualizado. Este es un correo electrónico de ejemplo que se envía cuando un usuario activa un rol con privilegios para la organización ficticia de Contoso.

![Nuevo correo electrónico de PIM para roles de Azure AD](./media/pim-email-notifications/email-directory-new.png)

Anteriormente, cuando un usuario activaba un rol con privilegios, el correo electrónico era similar al siguiente.

![Correo electrónico de PIM antiguo para roles de Azure AD](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>Correos electrónicos de PIM para roles de recursos de Azure

A partir de finales de julio de 2018, las notificaciones por correo electrónico de PIM para roles de recursos de Azure tienen un diseño actualizado. Este es un correo electrónico de ejemplo que se envía cuando se asigna a un usuario un rol con privilegios para la organización ficticia de Contoso.

![Nuevo correo electrónico de PIM para roles de recursos de Azure](./media/pim-email-notifications/email-resources-new.png)

Anteriormente, cuando se asignaba a un usuario un rol con privilegios, el correo electrónico era similar al siguiente.

![Correo electrónico de PIM antiguo para roles de recursos de Azure](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de directorio de Azure AD en PIM](pim-how-to-change-default-settings.md)
- [Aprobación o rechazo de solicitudes para los roles de directorio de Azure AD en PIM](azure-ad-pim-approval-workflow.md)
