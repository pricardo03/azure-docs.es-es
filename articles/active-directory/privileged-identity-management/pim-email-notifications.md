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
ms.date: 07/24/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 1a1635e0ba16bdc0fa6dc90aa0e1a417f43e04ac
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190183"
---
# <a name="email-notifications-in-pim"></a>Notificaciones por correo electrónico en PIM

Cuando se producen eventos clave en Azure AD Privileged Identity Management (PIM), se envían notificaciones por correo electrónico al usuario o administrador pertinentes. Por ejemplo, PIM envía mensajes de correo electrónico con los siguientes eventos:

- Cuando una activación de roles con privilegios está pendiente de aprobación
- Cuando se aprueba una solicitud de activación de roles con privilegios
- Cuando se activa un rol con privilegios
- Cuando se asigna un rol con privilegios
- Cuando se habilita Azure AD PIM

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
