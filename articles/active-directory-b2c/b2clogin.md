---
title: 'Establecimiento de direcciones URL de redireccionamiento a b2clogin.com: Azure Active Directory B2C | Microsoft Docs'
description: Aprenda a usar b2clogin.com en las direcciones URL de redireccionamiento para Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 8188e36278bad9c93f709a5d7d9f831d1c19e6b4
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486854"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Establecimiento de direcciones URL de redireccionamiento en b2clogin.com para Azure Active Directory B2C

Cuando se configura un proveedor de identidades para registrarse e iniciar sesión en la aplicación Azure Active Directory (Azure AD) B2C, debe especificar una dirección URL de redireccionamiento. En el pasado, se utilizaba login.microsoftonline.com, ahora debería utilizar b2clogin.com.

El uso de b2clogin.com ofrece ventajas adicionales, como:

- Se reduce el espacio que los servicios de Microsoft consumen en el encabezado de las cookies.
- Las direcciones URL ya no incluyen una referencia a Microsoft. Por ejemplo, `https://your-tenant-name.b2clogin.com/tenant-id/oauth2/authresp`.

>[!NOTE]
> Puede usar el nombre del inquilino y el GUID del inquilino como sigue:
> * `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` (que aún se refiere a `onmicrosoft.com`)
> * `https://your-tenant-name.b2clogin.com/your-tenant-guid` (en cuyo caso hay ninguna referencia a Microsoft en absoluto)
>
> Sin embargo, no puede usar un _dominio personalizado_ para Azure del inquilino B2C de Active Directory, por ejemplo, `https://your-tenant-name.b2clogin.com/your-custom-domain-name` sería _no_ funcione.

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
