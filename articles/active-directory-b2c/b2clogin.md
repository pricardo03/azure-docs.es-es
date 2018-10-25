---
title: Establecimiento de direcciones URL de redireccionamiento en b2clogin.com para Azure Active Directory B2C | Microsoft Docs
description: Aprenda a usar b2clogin.com en las direcciones URL de redireccionamiento para Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8e06cf1a443d4fd158e29ef4b53206a83800dfe9
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803059"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Establecimiento de direcciones URL de redireccionamiento en b2clogin.com para Azure Active Directory B2C

Cuando se configura un proveedor de identidades para registrarse e iniciar sesión en la aplicación Azure Active Directory (Azure AD) B2C, debe especificar una dirección URL de redireccionamiento. En el pasado, se utilizaba login.microsoftonline.com, ahora debería utilizar b2clogin.com.

El uso de b2clogin.com ofrece ventajas adicionales, como:

- Las cookies ya no se comparten con otros servicios de Microsoft.
- Las direcciones URL ya no incluyen una referencia a Microsoft. Por ejemplo, `https://your-tenant-name.b2clogin.com/tfp/your-tenant-ID/policyname/v2.0/.well-known/openid-configuration`.

Para usar b2clogin.com, establezca las URL de redireccionamiento en las aplicaciones del proveedor de identidades para usar b2clogin.com. También puede configurar la aplicación Azure AD B2C para que utilice b2clogin.com como referencia de directivas y puntos de conexión de token. Si se utiliza MSAL, debe establecer la propiedad **ValidateAuthority** en `false`.

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

La aplicación de Azure AD B2C probablemente se refiere a `login.microsoftonline.com` en varios lugares, tales como las referencias de directivas y puntos de conexión de token.  Asegúrese de que el punto de conexión de autorización, el punto de conexión del token y el emisor se han actualizado para utilizar `your-tenant-name.b2clogin.com`.  

## <a name="set-the-validateauthority-property"></a>Establecimiento de la propiedad ValidateAuthority

Si usa MSAL, establezca **ValidateAuthority** a `false`. En el ejemplo siguiente se muestra cómo se puede establecer la propiedad:

```
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

 Para más información, consulte la [clase ClientApplicationBase](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase?view=azure-dotnet).