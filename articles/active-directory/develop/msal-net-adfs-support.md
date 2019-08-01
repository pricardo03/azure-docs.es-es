---
title: Compatibilidad para AD FS en la biblioteca de autenticación de Microsoft para .NET | Azure
description: Aprenda sobre la compatibilidad para los Servicios de federación de Active Directory (AD FS) en la biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5963c5e83b5af3848edd934328caa1f095bd184
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676734"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Compatibilidad para los Servicios de federación de Active Directory (AD FS) en MSAL.NET.
Los Servicios de federación de Active Directory (AD FS) en Windows Server le permiten agregar autenticación y autorización basadas en OpenID Connect y OAuth 2.0 a las aplicaciones que desarrolle para que en ellas los usuarios se autentiquen directamente en AD FS. Para más información, consulte [Escenarios de AD FS para desarrolladores](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

La biblioteca de autenticación de Microsoft para .NET (MSAL.NET) admite dos escenarios de autenticación en AD FS:

- MSAL.NET se comunica con Azure Active Directory, que a su vez está *federado* con AD FS.
- MSAL.NET se comunica *directamente* con una autoridad de AD FS, donde la versión de este último está conforme con OpenID Connect (a partir de AD FS 2019). La conexión directa con AD FS permite a MSAL.NET autenticarse en aplicaciones que se ejecutan [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>Conexión de MSAL a Azure AD, federado con AD FS
MSAL.NET admite la conexión a Azure AD, que inicia sesión de usuarios administrados (usuarios administrados en Azure AD) o de usuarios federados (usuarios administrados por otro proveedor de identidades, como AD FS). MSAL.NET no reconoce la federación de los usuarios. En lo que a él respecta, se comunica con Azure AD.

La [autoridad](msal-client-application-configuration.md#authority) que usa en este caso es la habitual (nombre de host de la autoridad + inquilino, common u organizations).

### <a name="acquiring-a-token-interactively"></a>Adquisición de un token de forma interactiva
Cuando se llama al método `AcquireTokenInteractive`, la experiencia del usuario suele ser:

1. El usuario especifica sus identificador de cuenta.
2. Azure AD muestra brevemente el mensaje "Taking you to your organization's page" (Le redirigimos a la página de su organización).
3. Al usuario se le redirige a la página de inicio de sesión del proveedor de identidades. Normalmente, la página de inicio de sesión se personaliza con el logotipo de la organización.

Las versiones compatibles de AD FS en este escenario federado son AD FS v2 y AD FS v3 (Windows Server 2012 R2) y AD FS v4 (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Adquisición de un token mediante AcquireTokenByIntegratedAuthentication o AcquireTokenByUsernamePassword
Al adquirir un token mediante los métodos `AcquireTokenByIntegratedAuthentication` o `AcquireTokenByUsernamePassword`, MSAL.NET hace que la conexión del proveedor de identidades se base en el nombre de usuario.  MSAL.NET recibe un [token SAML 1.1](reference-saml-tokens.md) después de ponerse en contacto con el proveedor de identidades.  A continuación, proporciona el token SAML a Azure AD como aserción de usuario (similar al [flujo con derechos delegados](msal-authentication-flows.md#on-behalf-of)) para obtener un token JWT.

## <a name="msal-connects-directly-to-ad-fs"></a>Conexión directa de MSLA a AD FS
MSAL.NET admite la conexión a AD FS 2019, conforme con Open ID Connect. Con la conexión directa a AD FS, la autoridad que desee usar para compilar la aplicación se parece a `https://mysite.contoso.com/adfs/`.

Actualmente, no hay ningún plan para admitir una conexión directa con AD FS 2016 ni AD FS v2 (que no son conformes con OpenID Connect). Si necesita admitir los escenarios que requieran una conexión directa con AD FS 2016, use la versión más reciente de la [biblioteca de autenticación de Azure Active Directory](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Podrá usar MSAL.NET una vez actualizado el sistema local a AD FS 2019.
