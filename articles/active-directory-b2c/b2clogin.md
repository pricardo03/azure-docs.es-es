---
title: 'Establecimiento de direcciones URL de redireccionamiento en b2clogin.com: Azure Active Directory B2C'
description: Aprenda a usar b2clogin.com en las direcciones URL de redireccionamiento para Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: dbc366daac89f44d4b084081590124f81ff9cc9c
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533745"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Establecimiento de direcciones URL de redireccionamiento en b2clogin.com para Azure Active Directory B2C

Cuando se configura un proveedor de identidades para registrarse e iniciar sesión en la aplicación Azure Active Directory B2C (Azure AD B2C), debe especificar una dirección URL de redireccionamiento. Ya no debe hacer referencia a *login.microsoftonline.com* en las aplicaciones y API. En su lugar, use *b2clogin.com* para todas las aplicaciones nuevas y migre las aplicaciones existentes de *login.microsoftonline.com* a *b2clogin.com*.

## <a name="benefits-of-b2clogincom"></a>Ventajas de b2clogin.com

Cuando use *b2clogin.com* como la dirección URL de redireccionamiento:

* Se reduce el espacio que los servicios de Microsoft consumen en el encabezado de las cookies.
* Las direcciones URL ya no incluyen una referencia a Microsoft.
* El código de cliente de JavaScript se admite (actualmente en [versión preliminar](user-flow-javascript-overview.md)) en páginas personalizadas. Debido a las restricciones de seguridad, el código JavaScript y los elementos de formulario HTML se quitan de las páginas personalizadas si se usa *login.microsoftonline.com*.

## <a name="overview-of-required-changes"></a>Información general de los cambios necesarios

Puede que tenga que realizar varias modificaciones para migrar las aplicaciones a *b2clogin.com*:

* Cambie las direcciones URL de redireccionamiento en las aplicaciones del proveedor de identidades para hacer referencia a *b2clogin.com*.
* Actualice la aplicación Azure AD B2C para que utilice *b2clogin.com* como referencias de flujos de usuario y puntos de conexión de token.
* Actualice cualquier **origen permitido** que haya definido en la configuración de CORS para la [personalización de la interfaz de usuario](active-directory-b2c-ui-customization-custom-dynamic.md).

## <a name="change-identity-provider-redirect-urls"></a>Cambie las direcciones URL de redireccionamiento del proveedor de identidades

En el sitio web de cada proveedor de identidades en el que ha creado una aplicación, cambie todas las direcciones URL de confianza para se redirijan a `your-tenant-name.b2clogin.com` en lugar de a *login.microsoftonline.com*.

Hay dos formatos que puede usar para las direcciones URL de redireccionamiento de b2clogin.com. La primera proporciona la ventaja de no tener "Microsoft" en ninguna parte de la dirección URL mediante el identificador de inquilino (un GUID) en lugar del nombre de dominio del inquilino:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

La segunda opción usa el nombre de dominio del inquilino con el formato `your-tenant-name.onmicrosoft.com`. Por ejemplo:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

En ambos formatos:

* Reemplace `{your-tenant-name}` por el nombre del inquilino de Azure AD B2C.
* Quite `/te` si existe en la dirección URL.

## <a name="update-your-applications-and-apis"></a>Actualización de las aplicaciones y de las API

El código de las aplicaciones habilitadas para Azure AD B2C y para las API puede hacer referencia a `login.microsoftonline.com` en varios lugares. Por ejemplo, el código podría tener referencias a los flujos de usuario y los puntos de conexión de token. Actualice lo siguiente para hacer referencia a `your-tenant-name.b2clogin.com`:

* Punto de conexión de autorización
* Punto de conexión de token
* Emisor de tokens

Por ejemplo, el punto de conexión de la autoridad de la directiva de registro o de inicio de sesión de Contoso ahora sería:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

## <a name="microsoft-authentication-library-msal"></a>Biblioteca de autenticación de Microsoft (MSAL)

### <a name="validateauthority-property"></a>Propiedad ValidateAuthority

Si usa [MSAL.NET][msal-dotnet] v2 o versiones anteriores, establezca la propiedad **ValidateAuthority** en `false` en la creación de instancias del cliente para permitir redireccionamientos a *b2clogin.com*. Esta configuración no es necesaria para MSAL.NET v3 y versiones posteriores.

```CSharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

Si usa [MSAL para JavaScript][msal-js]:

```JavaScript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md