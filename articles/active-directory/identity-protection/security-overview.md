---
title: Información general de seguridad de Azure Active Directory (Azure AD) Identity Protection | Microsoft Docs
description: Obtenga información sobre cómo la "Información general de seguridad" le ofrece una visión general de la postura de seguridad de su organización.
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: dca184e5b94d69845bfa6e2d83753f53554cadeb
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56266754"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Información general de seguridad de Azure Active Directory Identity Protection

[Información general de seguridad](https://aka.ms/IdentityProtectionRefresh) le ofrece una visión general de la postura de seguridad de su organización. Le ayuda a identificar posibles ataques y a comprender la eficacia de las directivas.

"Información general de seguridad" se divide en dos secciones:

- Las tendencias, a la izquierda, proporcionan una escala de tiempo de riesgo en su organización.

- Los iconos situados a la derecha ponen en relieve los problemas clave en curso de su organización y sugieren cómo tomar medidas rápidamente.


![Introducción a la seguridad](./media/security-overview/01.png)
  
## <a name="trends"></a>Tendencias

### <a name="new-risky-users-detected"></a>Se detectaron nuevos usuarios de riesgo

En este gráfico se muestra el número de nuevos usuarios de riesgo que se detectaron durante el período de tiempo seleccionado. Puede filtrar la vista de este gráfico por el nivel de riesgo de usuario (bajo, medio, alto). Mantenga el puntero sobre los incrementos de fecha UTC para ver el número de usuarios de riesgo detectados a lo largo de ese día. Al hacer clic en este gráfico se le conducirá al informe "Usuarios de riesgo". Para corregir los usuarios que están en riesgo, considere la posibilidad de cambiar su contraseña.

### <a name="new-risky-sign-ins-detected"></a>Se detectaron nuevos inicios de sesión de riesgo.

En este gráfico se muestra el número de inicios de sesión de riesgo que se detectaron durante el período de tiempo seleccionado. Puede filtrar la vista de este gráfico por el tipo de inicio de sesión de riesgo (en tiempo real o agregado) y el nivel de inicio de sesión de riesgo (bajo, medio, alto). Los inicios de sesión no protegidos son inicios de sesión de riesgo en tiempo real que no fueron cuestionados por MFA. (Nota: Los inicios de sesión de riesgo debido a detecciones sin conexión no pueden protegerse en tiempo real mediante directivas de riesgo de inicio de sesión). Mantenga el puntero sobre los incrementos de fecha UTC para ver el número de inicios de sesión de riesgo detectados a lo largo de ese día. Al hacer clic en este gráfico se le conducirá al informe "Inicios de sesión de riesgo".

## <a name="tiles"></a>Iconos
 
### <a name="high-risk-users"></a>Usuarios de alto riesgo

El icono de "Usuarios de alto riesgo" muestra el recuento de usuarios más reciente cuya identidad tiene alta probabilidad de riesgo. Estos deben investigarse prioritariamente. Al hacer clic en el icono de "Usuarios de alto riesgo" se le redirigirá a una vista filtrada del informe "Usuarios de riesgo" que muestra solo los usuarios con un nivel de riesgo alto. Al usar este informe, puede obtener más información y corregir estos usuarios con un restablecimiento de contraseña.

![Introducción a la seguridad](./media/security-overview/02.png)


### <a name="medium-risk-users"></a>Usuarios de riesgo medio
El icono de "Usuarios de riesgo medio" muestra el recuento de usuarios más reciente cuya identidad tiene probabilidad de riesgo medio. Al hacer clic en el icono de "Usuarios de riesgo medio" se le redirigirá a una vista filtrada del informe "Usuarios de riesgo" que muestra solo los usuarios con un nivel de riesgo medio. Al usar este informe, puede investigar más y corregir estos usuarios.

### <a name="unprotected-risky-sign-ins"></a>Inicios de sesión de riesgo sin protección

El icono de "Inicios de sesión de riesgo sin protección" muestra el recuento de la semana pasada de inicios de sesión correctos y en tiempo real que no están bloqueados ni se sometieron a MFA a causa de las directivas de acceso condicional, las directivas de riesgo o MFA por usuario. Se trata de inicios de sesión que están potencialmente en peligro que se realizaron correctamente y no se sometieron a MFA. Para proteger estos inicios de sesión en el futuro, aplique una directiva de riesgo de inicio de sesión. Al hacer clic en el icono "Inicios de sesión de riesgo sin protección" se le redirigirá a la hoja de configuración de la directiva de riesgo de inicio de sesión para que pueda configurarla para requerir MFA en un inicio de sesión con un determinado nivel de riesgo.


### <a name="legacy-authentication"></a>Autenticación heredada

El icono "Autenticación heredada" muestra el recuento de la semana pasada de autenticaciones heredadas de su organización. Los protocolos de autenticación heredados no admiten métodos de seguridad modernos como una MFA. Para evitar la autenticación heredada, puede aplicar una directiva de acceso condicional. Al hacer clic en el icono "Autenticación heredada" se le redirigirá a la "Puntuación segura de identidad'.


### <a name="identity-secure-score"></a>Puntuación segura de identidad

La Puntuación segura de identidad mide y compara su nivel de seguridad según los patrones del sector. Si hace clic en el icono "Puntuación segura de identidad (versión preliminar)", se le redirigirá a la hoja "Puntuación segura de identidad (versión preliminar)" donde puede obtener más información sobre cómo mejorar su nivel de seguridad.


## <a name="next-steps"></a>Pasos siguientes

- [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

- [Habilitación de Azure Active Directory Identity Protection](enable.md)

