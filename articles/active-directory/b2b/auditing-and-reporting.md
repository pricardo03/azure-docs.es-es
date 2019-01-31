---
title: Auditoría y generación de informes para usuarios de colaboración B2B de Azure Active Directory | Microsoft Docs
description: Las propiedades de un usuario invitado son configurables en la colaboración B2B de Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 06622c093ca90b3873365e6c93c40fc7221a6398
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075200"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>Auditoría y generación de informes para usuarios de colaboración 2B
Con usuarios invitados, cuenta con funcionalidades de auditoría similares a las que tiene con los usuarios miembros. 

## <a name="access-reviews"></a>Revisiones de acceso
Puede utilizar revisiones de acceso para comprobar periódicamente si los usuarios invitados necesitan todavía acceso a los recursos. La característica **Revisiones de acceso** está disponible en **Azure Active Directory** en **Administrar** > **Relaciones de organización**. (También puede buscar "revisiones de acceso" en **Todos los servicios** en Azure Portal). Para aprender a usar las revisiones de acceso, consulte [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Registros de auditoría

Los registros de auditoría de Azure AD proporcionan registros de las actividades del sistema y de los usuarios, incluidas las actividades iniciadas por los usuarios invitados. Para acceder a los registros de auditoría, en **Azure Active Directory**, en **Supervisión**, seleccione **Registros de auditoría**. A continuación, se muestra un ejemplo del historial de canje e invitación de los usuario Sam Oogle al que se acaba de invitar:

![Registro de auditoría](./media/auditing-and-reporting/audit-log.png)

Puede profundizar en cada uno de estos eventos para obtener los detalles. Por ejemplo, echemos un vistazo a los detalles de aceptación.

![Detalles de actividad](./media/auditing-and-reporting/activity-details.png)

También puede exportar estos registros de Azure AD y utilizar la herramienta de informes que prefiera para obtener informes personalizados.

### <a name="next-steps"></a>Pasos siguientes

- [Propiedades de usuario de la colaboración B2B](user-properties.md)

