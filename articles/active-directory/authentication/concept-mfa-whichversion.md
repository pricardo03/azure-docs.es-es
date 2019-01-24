---
title: ¿Servidor o servicio MFA de Azure, en el entorno local o en la nube?
description: Como administrador de Azure AD, necesito saber qué versión de MFA debo implementar.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 50340218f80dfb0b443075a3a4f85e7a5ed86159
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54433283"
---
# <a name="which-version-of-azure-mfa-is-right-for-my-organization"></a>¿Qué versión de Azure MFA se ajusta mi organización?

Para decidir dónde y cómo implementar Multi-Factor Authentication (MFA), tiene que responder a tres cuestiones básicas.

* [Qué es lo que quiero proteger](#what-am-i-trying-to-secure)
* [Dónde se encuentran los usuarios](#where-are-the-users-located)
* [¿Qué características necesito?](#what-features-do-i-need)

Cada una de las siguientes secciones ofrece información detallada para ayudarle a responder las preguntas anteriores.

## <a name="what-am-i-trying-to-secure"></a>¿Qué es lo que quiero proteger?

Para determinar la solución correcta de verificación en dos pasos, tiene que responder primero a la pregunta de qué es lo que intenta proteger con un factor adicional de autenticación. ¿Es una aplicación que está en Azure? ¿O un sistema de acceso remoto? Mediante la determinación de lo que intenta proteger, podrá dar respuesta a la pregunta de dónde hay que habilitar Multi-Factor Authentication.

| ¿Qué intenta proteger? | MFA en la nube | Servidor MFA |
| --- |:---:|:---:|
| Aplicaciones propias de Microsoft |● |● |
| Aplicaciones de SaaS en la galería de aplicaciones |● |  |
| Aplicaciones web que se publican a través del proxy de aplicación de Azure AD |● |  |
| Aplicaciones de IIS que no se publican a través del proxy de aplicación de Azure AD | |● |
| Acceso remoto como VPN, RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Dónde se encuentran los usuarios

A continuación, en función del lugar en que se encuentren los usuarios de la organización, podemos determinar la solución correcta que se debe utilizar, ya sea en la nube o en el entorno local, con el servidor MFA.

| Ubicación del usuario | MFA en la nube | Servidor MFA |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD y AD local mediante la federación con AD FS |● |● |
| Azure AD y AD local con Azure AD Connect: sin sincronización de hash de contraseñas ni autenticación de paso a través |● |● |
| Azure AD y AD local con Azure AD Connect: con sincronización de hash de contraseñas o autenticación de paso a través |● | |
| Active Directory local | |● |

## <a name="what-features-do-i-need"></a>¿Qué características necesito?

En la tabla siguiente se comparan las características disponibles en Multi-Factor Authentication en la nube con las de Servidor Multi-Factor Authentication.

| Característica | MFA en la nube | Servidor MFA |
| --- |:---:|:---:|
| Notificación de aplicación móvil como segundo factor | ● | ● |
| Código de verificación de aplicación móvil como segundo factor | ● | ● |
| Llamada de teléfono como segundo factor | ● | ● |
| SMS unidireccional como segundo factor | ● | ● |
| Tokens de hardware como segundo factor | ● (Versión preliminar pública) | ● |
| Contraseñas de aplicaciones para clientes de Office 365 que no admiten MFA | ● | |
| Control de administración sobre los métodos de autenticación | ● | ● |
| Modo de PIN | | ● |
| Alerta de fraude | ● | ● |
| Informes de MFA | ● | ● |
| Omisión por única vez | | ● |
| Saludos personalizados para las llamadas de teléfono | ● | ● |
| Identificador de llamada personalizable para llamadas telefónicas | ● | ● |
| IP de confianza | ● | ● |
| Recordar MFA para dispositivos de confianza | ● | |
| Acceso condicional | ● | ● |
| Memoria caché |  | ● |

## <a name="next-steps"></a>Pasos siguientes

Ahora que comprende la diferencia entre Azure Multi-Factor Authentication en la nube o el servidor MFA local, es momento de configurar y usar Azure Multi-Factor Authentication. **Seleccione el icono que represente su escenario**

<center>

[![MFA en la nube](./media/concept-mfa-whichversion/cloud2.png)](howto-mfa-getstarted.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [![Servidor MFA](./media/concept-mfa-whichversion/server2.png)](howto-mfaserver-deploy.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
