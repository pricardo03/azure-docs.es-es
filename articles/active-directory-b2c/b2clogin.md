---
title: 'Establecimiento de direcciones URL de redireccionamiento a b2clogin.com: Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a usar b2clogin.com en las direcciones URL de redireccionamiento para Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 080c1933f88d9e824969a42212de2eacd0f62e14
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927279"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Establecimiento de direcciones URL de redireccionamiento en b2clogin.com para Azure Active Directory B2C

Cuando se configura un proveedor de identidades para registrarse e iniciar sesión en la aplicación Azure Active Directory (Azure AD) B2C, debe especificar una dirección URL de redireccionamiento. En el pasado, se utilizaba login.microsoftonline.com, ahora debería utilizar b2clogin.com.

> [!NOTE]
> Puede usar el código de cliente de JavaScript (actualmente en versión preliminar) en b2clogin.com. Si usa login.microsoftonline.com, se quitará el código de JavaScript de la página personalizada. También se aplican restricciones de seguridad adicionales a login.microsoftonline.com, como quitar elementos de formulario HTML de la página personalizada. 

El uso de b2clogin.com ofrece ventajas adicionales, como:

- Se reduce el espacio que los servicios de Microsoft consumen en el encabezado de las cookies.
- Las direcciones URL ya no incluyen una referencia a Microsoft. Por ejemplo, `https://your-tenant-name.b2clogin.com/tenant-id/oauth2/authresp`.

> [!NOTE]
> Puede usar el nombre del inquilino y el GUID del inquilino como se muestra a continuación:
> * `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` (que aún hace referencia a `onmicrosoft.com`).
> * `https://your-tenant-name.b2clogin.com/your-tenant-guid` (en que no hay ninguna referencia a Microsoft).
>
> Sin embargo, no puede usar ningún _dominio personalizado_ para su inquilino de Azure Active Directory B2C, por ejemplo, `https://your-tenant-name.b2clogin.com/your-custom-domain-name` _no_ funcionaría.

Tenga en cuenta estas opciones que tal vez tenga que cambiar al usar b2clogin.com:

- Establezca las URL de redireccionamiento en las aplicaciones del proveedor de identidades para usar b2clogin.com. 
- Establezca la aplicación Azure AD B2C para que utilice b2clogin.com como referencias de flujos de usuario y puntos de conexión de token. 
- Si se utiliza MSAL, debe establecer la propiedad **ValidateAuthority** en `false`.
- Asegúrese de cambiar los **orígenes permitidos** que haya definido en la configuración de CORS para la [personalización de la interfaz de usuario](active-directory-b2c-ui-customization-custom-dynamic.md).  

## <a name="change-redirect-urls"></a>Cambio de direcciones URL de redireccionamiento

Para usar b2clogin.com, en la configuración de su aplicación de proveedor de identidades, busque y cambie la lista de direcciones URL de confianza para volver a dirigirla a Azure AD B2C.  En la actualidad, probablemente lo tenga configurado para la redirección a algún sitio de login.microsoftonline.com. 

Deberá cambiar la dirección URL de redireccionamiento para que `your-tenant-name.b2clogin.com` esté autorizada. Asegúrese de reemplazar `your-tenant-name` por el nombre del inquilino de Azure AD B2C y quite `/te` si existe en la dirección URL. Hay ligeras variaciones en esta dirección URL para cada proveedor de identidades, así que compruebe la página correspondiente para obtener la dirección URL exacta.

Puede encontrar información sobre la configuración de los proveedores de identidades en los siguientes artículos:

- [Cuenta Microsoft](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [OIDC personalizado](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Actualización de la aplicación

La aplicación de Azure AD B2C probablemente se refiere a `login.microsoftonline.com` en varios lugares, tales como las referencias de flujos de usuario y puntos de conexión de token.  Asegúrese de que el punto de conexión de autorización, el punto de conexión del token y el emisor se han actualizado para utilizar `your-tenant-name.b2clogin.com`.  

## <a name="set-the-validateauthority-property"></a>Establecimiento de la propiedad ValidateAuthority

Si usa MSAL, establezca la propiedad **ValidateAuthority** en `false`. Si **ValidateAuthority** se establece en `false`, se permiten las redirecciones a b2clogin.com. 

En el ejemplo siguiente se muestra cómo se puede establecer la propiedad:

En [MSAL para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

Y en [MSAL para JavaScript](https://github.com/AzureAD/microsoft-authentication-library-for-js):

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
