---
title: Sesión y solo inicio de sesión en configuración - Azure Active Directory B2C | Microsoft Docs
description: Sesión y configuración de inicio de sesión único en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: daff37ca803a7eebdc07a78d6d02f5cee36de2f5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703813"
---
# <a name="session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Sesión y la configuración de inicio de sesión único en Azure Active Directory B2C

Esta característica ofrece un control más preciso, [por directivas](active-directory-b2c-reference-policies.md), de lo siguiente:

- La duración de las sesiones de aplicación web administradas por Azure AD B2C.
- El comportamiento de inicio de sesión único (SSO) entre varias aplicaciones y flujos de usuario en el inquilino de Azure AD B2C.

## <a name="session-behavior"></a>Comportamiento de la sesión

Azure AD B2C admite el [protocolo de autenticación OpenID Connect](active-directory-b2c-reference-oidc.md) para habilitar el inicio de sesión seguro en las aplicaciones web. Puede usar las propiedades siguientes para administrar sesiones de la aplicación web:

- **Vigencia (en minutos) de la sesión de la aplicación web**: la vigencia de la cookie de sesión de Azure AD B2C almacenada en el explorador del usuario tras una autenticación correcta.
    - Valor predeterminado: 1440 minutos.
    - Mínimo (incluido) = 15 minutos.
    - Máximo (incluido) = 1440 minutos.
- **Tiempo de espera de la sesión de la aplicación web**: si este conmutador se establece en **Absolute** (Absoluto), el usuario se ve obligado a volver a autenticarse una vez transcurrido el período especificado en **Vigencia (en minutos) de la sesión de la aplicación web**. Si este conmutador se establece en **Rolling** (Gradual) (el valor predeterminado), el usuario permanecerá conectado siempre que esté activo en su aplicación web.

Los siguientes casos de uso se habilitan mediante estas propiedades:

- Conformidad con los requisitos de cumplimiento normativo y seguridad de la industria mediante el establecimiento de la duración adecuada de la sesión de la aplicación web.
- Obligación de volver a autenticarse después de un período de tiempo establecido durante la interacción del usuario con una zona de alta seguridad de la aplicación web. 

Estas opciones no están disponibles para flujos de usuario de restablecimiento de contraseña.

## <a name="single-sign-on-sso-configuration"></a>Configuración de inicio de sesión único (SSO)

Si tiene varias aplicaciones y flujos de usuario en el inquilino B2C, puede administrar las interacciones del usuario a través de ellos con la propiedad **Configuración de inicio de sesión único**. Puede establecer la propiedad en uno de los siguientes valores:

- **Inquilino**: esta es la configuración predeterminada. Esta configuración permite que varias aplicaciones y flujos de usuario del inquilino B2C compartan la misma sesión de usuario. Por ejemplo, una vez que un usuario inicia sesión en una aplicación, puede también iniciar sesión perfectamente en otra llamada Contoso Pharmacy simplemente con acceder a ella.
- **Aplicación**: este valor permite mantener una sesión de usuario exclusivamente para una aplicación, independientemente de otras aplicaciones. Por ejemplo, si desea que el usuario inicie sesión en Contoso Pharmacy (con las mismas credenciales), aunque ya haya iniciado sesión en Contoso Shopping, otra aplicación en el mismo inquilino B2C. 
- **Directiva**: este valor permite mantener una sesión de usuario exclusivamente para un flujo de usuario, independientemente de las aplicaciones que lo usen. Por ejemplo, si el usuario ya ha iniciado sesión y ha realizado un paso de autenticación multifactor (MFA), se le puede dar acceso a zonas de mayor seguridad de varias aplicaciones mientras no expire la sesión asociada al flujo de usuario.
- **Deshabilitado**: este valor obliga al usuario a ejecutar todo el flujo de usuario cada vez que se ejecuta la directiva.

Estas opciones no están disponibles para flujos de usuario de restablecimiento de contraseña. 

