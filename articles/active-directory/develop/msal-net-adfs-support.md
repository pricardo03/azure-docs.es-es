---
title: AD FS se admite en la biblioteca de autenticación de Microsoft para .NET | Azure
description: Obtenga información sobre la compatibilidad de los servicios de federación de Active Directory (AD FS) en la biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
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
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676734"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Compatibilidad con los servicios de federación de Active Directory en MSAL.NET
Servicios de federación de Active Directory (AD FS) en Windows Server le permite agregar OpenID Connect y autenticación basada en OAuth 2.0 y autorización para aplicaciones que está desarrollando y tener esas aplicaciones autenticar a los usuarios directamente en AD FS. Para obtener más información, lea [escenarios de AD FS para desarrolladores](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Biblioteca de autenticación de Microsoft para .NET (MSAL.NET) admite dos escenarios de autenticación en AD FS:

- MSAL.NET se comunica con Azure Active Directory, que a su vez es *federado* con AD FS.
- Charlas MSAL.NET *directamente* a una entidad de AD FS, donde la versión de AD FS es la compatibilidad con OpenID Connect (a partir de AD FS 2019). Conexión directa con AD FS permite MSAL.NET para autenticarse con aplicaciones que se ejecutan [Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL se conecta a Azure AD, que está federado con AD FS
MSAL.NET admite la conexión a Azure AD, que inicia sesión en usuarios administrados (los usuarios administrados en Azure AD) o (a los usuarios administrados por otro proveedor de identidades, como AD FS) de los usuarios federados. MSAL.NET no conoce el hecho de que los usuarios están federados. En cuanto lo es, se comunica con Azure AD.

El [autoridad](msal-client-application-configuration.md#authority) use en este caso es la autoridad habitual (nombre de host de la entidad de certificación + inquilino, común u organizaciones).

### <a name="acquiring-a-token-interactively"></a>Adquirir un token de forma interactiva
Cuando se llama a la `AcquireTokenInteractive` método, la experiencia del usuario suele ser:

1. El usuario escribe su identificador de cuenta.
2. Azure AD brevemente muestra el mensaje "Redirigiendo a la página de su organización".
3. El usuario se redirige a la página de inicio de sesión del proveedor de identidades. Normalmente se personaliza la página de inicio de sesión con el logotipo de la organización.

Versiones compatibles de AD FS en este escenario federado son v2 de AD FS, AD FS v3 (Windows Server 2012 R2) y v4 de AD FS (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Al adquirir un token mediante AcquireTokenByIntegratedAuthentication o AcquireTokenByUsernamePassword
Al adquirir un token mediante el `AcquireTokenByIntegratedAuthentication` o `AcquireTokenByUsernamePassword` métodos, MSAL.NET obtiene ponerse en contacto con el proveedor de identidades basado en el nombre de usuario.  MSAL.NET recibe un [token SAML 1.1](reference-saml-tokens.md) después de ponerse en contacto con el proveedor de identidades.  MSAL.NET, a continuación, proporciona el token SAML a Azure AD como una aserción de usuario (similar a la [en nombre de flujo](msal-authentication-flows.md#on-behalf-of)) para obtener un token JWT.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL se conecta directamente a AD FS
MSAL.NET admite la conexión a AD FS de 2019, que es Open ID Connect compatible. Cuando se conecta directamente a AD FS, la entidad que desea usar para compilar la aplicación es similar a `https://mysite.contoso.com/adfs/`.

Actualmente, no hay ningún plan para admitir una conexión directa con AD FS 2016 o AD FS v2 (que no se incluyen compatibilidad con OpenID Connect). Si necesita admitir los escenarios que requieren una conexión directa con AD FS 2016, use la versión más reciente de [Azure Active Directory Authentication Library](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Cuando se ha actualizado su sistema local para AD FS 2019, podrá usar MSAL.NET.
