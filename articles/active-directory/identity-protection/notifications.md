---
title: Notificaciones de Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información sobre cómo contribuyen las notificaciones a sus actividades de investigación.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3dcc7d2d03b3c4a3f4c8c772f38aa3785080d986
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335351"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Notificaciones de Azure Active Directory Identity Protection

Azure AD Identity Protection envía dos tipos de correos electrónicos de notificación automatizados para ayudar a administrar el riesgo del usuario y los eventos de riesgo:

- Correo electrónico de los usuarios en riesgo detectados
- Correo electrónico de resumen semanal

En este artículo se proporciona una introducción de ambos correos electrónicos de notificación.

## <a name="users-at-risk-detected-email"></a>Correo electrónico de los usuarios en riesgo detectados

En respuesta a una cuenta detectada en riesgo, Azure AD Identity Protection genera una alerta de correo electrónico con el asunto **Usuarios en riesgo detectados**. El correo electrónico incluye un vínculo al informe **[Usuarios marcados en riesgo](../reports-monitoring/concept-user-at-risk.md)** . Como práctica recomendada, debería investigar inmediatamente los usuarios en peligro.

La configuración de esta alerta permite especificar a qué nivel de riesgo del usuario desea que se genere la alerta. Se generará el correo electrónico al alcanzar el nivel de riesgo del usuario especificado. Sin embargo, no recibirá nuevas alertas por correo electrónico de usuarios en riesgo detectados para este usuario una vez que pase a este nivel de riesgo del usuario. Por ejemplo, si establece la directiva para alertar en el riesgo de usuario medio y el usuario John pasa a riesgo medio, recibirá el correo electrónico de usuarios en riesgo detectados para John. Sin embargo, no recibirá una segunda alerta de usuarios en riesgo detectados si John pasa a riesgo alto o tiene eventos de riesgo adicionales.

![Correo electrónico de los usuarios en riesgo detectados](./media/notifications/01.png)

### <a name="configuration"></a>Configuración

Como administrador, puede establecer:

- **El nivel de riesgo de usuario que desencadena la generación de este correo electrónico**: de forma predeterminada, el nivel de riesgo se establece en "Alto" riesgo.
- **Los destinatarios de este correo electrónico**: de forma predeterminada, los destinatarios incluyen todos los administradores globales. Los administradores globales también pueden agregar otros administradores globales, administradores de seguridad y Lectores de seguridad como destinatarios.  

Para abrir el cuadro de diálogo relacionado, haga clic en **Alertas** en la sección **Configuración** de la página **Identity Protection**.

![Correo electrónico de los usuarios en riesgo detectados](./media/notifications/05.png)

## <a name="weekly-digest-email"></a>Correo electrónico de resumen semanal

El correo electrónico de resumen semanal contiene un sumario de nuevos eventos de riesgo.  
Incluye:

- Usuarios en riesgo
- Actividades sospechosas
- Puntos vulnerables detectados
- Vínculos a los informes relacionados en Identity Protection

    ![Corrección](./media/notifications/400.png "Corrección")

### <a name="configuration"></a>Configuración

Como administrador, puede desactivar la opción de envío de un correo electrónico de resumen semanal.

![Riesgos de usuario](./media/notifications/62.png "Riesgos de usuario")

Para abrir el cuadro de diálogo relacionado, haga clic en **Resumen semanal** en la sección **Configuración** de la página **Identity Protection**.

![Correo electrónico de los usuarios en riesgo detectados](./media/notifications/04.png)

## <a name="see-also"></a>Otras referencias

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
