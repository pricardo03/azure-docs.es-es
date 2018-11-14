---
title: Configuración de tokens, sesiones e inicio de sesión único en Azure Active Directory B2C | Microsoft Docs
description: Configuración de tokens, sesiones e inicio de sesión único en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0058ce8316fa8202cf53eaa1048a44b77efdecb5
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012452"
---
# <a name="token-session-and-single-sign-on-configuration-in-azure-active-directory-b2c"></a>Configuración de tokens, sesiones e inicio de sesión único en Azure Active Directory B2C.

Esta característica ofrece un control más preciso, [por directivas](active-directory-b2c-reference-policies.md), de:

- La vigencia de los tokens de seguridad emitidos por Azure Active Directory (Azure AD) B2C.
- La duración de las sesiones de aplicación web administradas por Azure AD B2C.
- Formatos de notificaciones importantes en los tokens de seguridad emitidos por Azure AD B2C.
- El comportamiento de inicio de sesión único (SSO) entre varias aplicaciones y directivas en el inquilino Azure AD B2C.

Para las directivas integradas, puede usar esta característica en el directorio de Azure AD B2C como sigue:

1. Haga clic en **Sign-up or sign-in policies**(Directivas de registro o de inicio de sesión). *Nota: Esta característica se puede usar en cualquier tipo de directiva, no solo en \*\*directivas de registro o de inicio de sesión*\*\*.
2. Abra una directiva haciendo clic en ella. Por ejemplo, haga clic en **B2C_1_SiUpIn**.
3. Haga clic en **Editar** en la parte superior del menú.
4. Haga clic en **Token, session & single sign-on config** (Configuración de tokens, sesión e inicio de sesión único).
5. Realice los cambios deseados. Obtenga información acerca de las propiedades disponibles en las secciones siguientes.
6. Haga clic en **OK**.
7. Haga clic en **Guardar** en la parte superior del menú.

## <a name="token-lifetimes-configuration"></a>Configuración de la vigencia de los tokens

Azure AD B2C admite el [protocolo de autorización de OAuth 2.0](active-directory-b2c-reference-protocols.md) para habilitar un acceso seguro a los recursos protegidos. Para implementar esta compatibilidad, Azure AD B2C emite varios [tokens de seguridad](active-directory-b2c-reference-tokens.md). 

Las siguientes propiedades se utilizan para administrar la vigencia de los tokens de seguridad emitidos por Azure AD B2C:

- **Vigencia (en minutos) de token de acceso y de identificador**: la vigencia del token de portador de OAuth 2.0 que se utiliza para obtener acceso a un recurso protegido.
    - Valor predeterminado: 60 minutos.
    - Mínimo (incluido) = 5 minutos.
    - Máximo (incluido) = 1440 minutos.
- **Vigencia del token de actualización (en días)**: el período máximo en que un token de actualización se puede utilizar para adquirir un nuevo token de acceso o de identificador (y opcionalmente, un nuevo token de actualización, si se hubiera concedido el ámbito `offline_access` a la aplicación).
    - Valor predeterminado = 14 días.
    - Mínimo (incluido) = 1 día.
    - Máximo (incluido) = 90 días.
- **Vigencia (en días) de la ventana deslizante del token de actualización**: después de transcurrido este período de tiempo, el usuario está obligado a volver a autenticarse, independientemente de cuál sea el período de validez del último token de actualización obtenido por la aplicación. Solo se pueden proporcionar si el conmutador se establece en **Bounded**(Enlazado). Es preciso que sea mayor o igual que el valor de **Vigencia del token de actualización (en días)** . Si el conmutador se establece en **Unbounded**(Sin enlazar), no se puede proporcionar un valor específico.
    - Valor predeterminado = 90 días.
    - Mínimo (incluido) = 1 día.
    - Máximo (incluido) = 365 días.

Los siguientes casos de uso se habilitan mediante estas propiedades:

- Permiso para que un usuario pueda permanecer conectado en una aplicación móvil indefinidamente, siempre que esté continuamente activo en la misma. Establezca **Vigencia (en días) de la ventana deslizante del token de actualización** en **Unbounded** (Sin enlazar) en la directiva de inicio de sesión.
- Cumpla los requisitos de cumplimiento normativo y seguridad de la industria mediante el establecimiento de la vigencia adecuada del token de acceso.

Estas opciones no están disponibles para las directivas de restablecimiento de contraseña. 

## <a name="token-compatibility-settings"></a>Configuración de compatibilidad de tokens

Las siguientes propiedades permiten a los clientes participar según sea necesario:

- **Notificación de emisor (iss)**: esta propiedad identifica el inquilino de Azure AD B2C que emitió el token.
    - `https://<domain>/{B2C tenant GUID}/v2.0/`: este es el valor predeterminado.
    - `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: este valor incluye los identificadores tanto del inquilino B2C como de la directiva utilizada en la solicitud de token. Si una aplicación o biblioteca necesita que Azure AD B2C sea compatible con la [especificación OpenID Connect Discovery 1.0](http://openid.net/specs/openid-connect-discovery-1_0.html), use este valor.
- **Notificación de asunto (sub)**: esta propiedad identifica la entidad para la que el token valida la información.
    - **ObjectID**: esta propiedad es el valor predeterminado. Rellena el identificador de objeto del usuario del directorio de la notificación `sub` del token.
    - **No se admite**: esta propiedad solo se proporciona para que haya compatibilidad con las versiones anteriores y se recomienda cambiar a **ObjectID** lo antes posible.
- **Notificación que representa el identificador de la directiva**: esta propiedad identifica el tipo de notificación en el que se rellena el identificador de directiva utilizado en la solicitud de token.
    - **tfp**: esta propiedad es el valor predeterminado.
    - **acr**: esta propiedad solo se proporciona por motivos de compatibilidad con versiones anteriores.

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

Estas opciones no están disponibles para las directivas de restablecimiento de contraseña.

## <a name="single-sign-on-sso-configuration"></a>Configuración de inicio de sesión único (SSO)

Si tiene varias aplicaciones y directivas en el inquilino de B2C, puede administrar las interacciones del usuario a través de ellas con la propiedad **Configuración de inicio de sesión único** . Puede establecer la propiedad en uno de los siguientes valores:

- **Inquilino**: esta es la configuración predeterminada. Esta configuración permite que varias aplicaciones y directivas en el inquilino B2C compartan la misma sesión de usuario. Por ejemplo, una vez que un usuario inicia sesión en una aplicación, puede también iniciar sesión perfectamente en otra llamada Contoso Pharmacy simplemente con acceder a ella.
- **Aplicación**: este valor permite mantener una sesión de usuario exclusivamente para una aplicación, independientemente de otras aplicaciones. Por ejemplo, si desea que el usuario inicie sesión en Contoso Pharmacy (con las mismas credenciales), aunque ya haya iniciado sesión en Contoso Shopping, otra aplicación en el mismo inquilino B2C. 
- **Directiva**: este valor permite mantener una sesión de usuario exclusivamente para una directiva, independientemente de las aplicaciones que la utilicen. Por ejemplo, si el usuario ya ha iniciado sesión y completado un paso de autenticación multifactor (MFA), puede tener acceso a zonas de una mayor seguridad de varias aplicaciones mientras no expire la sesión asociada a la directiva.
- **Deshabilitado**: este valor obliga al usuario a realizar todo el proceso cada vez que se ejecuta la directiva. Por ejemplo, esto permite que varios usuarios se registren en la aplicación (en un escenario de escritorio compartido), incluso si un único usuario permanece conectado durante todo el tiempo.

Estas opciones no están disponibles para las directivas de restablecimiento de contraseña. 

