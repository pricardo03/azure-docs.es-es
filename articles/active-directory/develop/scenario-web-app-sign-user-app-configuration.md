---
title: 'Aplicación web que inicia la sesión de los usuarios (configuración del código): Plataforma de identidad de Microsoft'
description: Obtener información sobre cómo crear una aplicación web que inicie la sesión de los usuarios (configuración del código)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82e6cbcd01c87ddffb7eac8d0ea0faef85f41a13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66254007"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Aplicación web que inicia la sesión de los usuarios: configuración del código

Obtenga información sobre cómo configurar el código para la aplicación web que inicia la sesión de los usuarios.

## <a name="libraries-used-to-protect-web-apps"></a>Bibliotecas utilizadas para proteger Web Apps

<!-- This section can be in an include for Web App and Web APIs -->
Las bibliotecas utilizadas para proteger una aplicación web (y una API web) son:

| Plataforma | Biblioteca | DESCRIPCIÓN |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Extensiones de modelo de identidad para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Utilizadas directamente por ASP.NET y ASP.NET Core, Microsoft Identity Extensions para .NET propone un conjunto de DLL que se ejecutan tanto en .NET Framework como en .NET Core. Desde una aplicación web de ASP.NET o ASP.NET Core, puede controlar la validación del token mediante la clase **TokenValidationParameters** (en concreto, en algunos escenarios de ISV) |

## <a name="aspnet-core-configuration"></a>Configuración de ASP.NET Core

### <a name="application-configuration-files"></a>Archivos de configuración de la aplicación

En ASP.NET Core, una aplicación web que inicia la sesión de los usuarios con la Plataforma de identidad de Microsoft se configura mediante el archivo `appsettings.json`. La configuración que debe rellenar es:

- la nube `Instance` si desea que la aplicación se ejecute en las nubes nacionales
- el público en `tenantId`
- `clientId` para la aplicación, tal y como se copió de Azure Portal.

```JSon
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.microsoftonline.com/" for Azure Public cloud.
    // "https://login.microsoftonline.us/" for Azure US government.
    // "https://login.microsoftonline.de/" for Azure AD Germany
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD Audience among:
    // - the tenant Id as a a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account or Microsoft personal account
    // - "consumers" to sign-in users with Microsoft personal account only
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

En ASP.NET Core, hay otro archivo que contiene la dirección URL (`applicationUrl`) y el puerto SSL (`sslPort`) para la aplicación, así como diversos perfiles.

```JSon
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

En Azure Portal, los URI de respuesta que necesita registrar en la página **Autenticación** de la aplicación deben coincidir con estas direcciones URL; es decir, para los dos archivos de configuración anteriores, serían `https://localhost:44321/signin-oidc`, ya que applicationUrl es `http://localhost:3110` pero `sslPort` se especifica (44321) y `CallbackPath` es `/signin-oidc` tal y como se define en `appsettings.json`.
  
De la misma manera, el URI de cierre de sesión se establecería en `https://localhost:44321/signout-callback-oidc`.

### <a name="initialization-code"></a>Código de inicialización

En Web Apps de ASP.NET Core (y las API web), el código que realiza la inicialización de la aplicación se encuentra en el archivo `Startup.cs` y, para agregar autenticación con la Plataforma de identidad de Microsoft (anteriormente Azure AD) v2.0, necesitará agregar el siguiente código. Los comentarios que se encuentren en el código deben ser claros.

  > [!NOTE]
  > Si inicia el proyecto con el proyecto web de ASP.NET Core predeterminado dentro de Visual Studio o mediante `dotnet new mvc`, el método `AddAzureAD` está disponible de forma predeterminada porque los paquetes relacionados se cargan automáticamente. Sin embargo si crea un proyecto desde cero e intenta usar el siguiente código, le recomendamos que agregue el paquete NuGet **"Microsoft.AspNetCore.Authentication.AzureAD.UI"** al proyecto para hacer que el método `AddAzureAD` esté disponible.
  
```CSharp
 services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
         .AddAzureAD(options => configuration.Bind("AzureAd", options));

 services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
 {
  // The ASP.NET core templates are currently using Azure AD v1.0, and compute
  // the authority (as {Instance}/{TenantID}). We want to use the Microsoft Identity Platform v2.0 endpoint
  options.Authority = options.Authority + "/v2.0/";

  // If you want to restrict the users that can sign-in to specific organizations
  // Set the tenant value in the appsettings.json file to 'organizations', and add the
  // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection.
  // Otherwise validate the issuer
  options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ForAadInstance(options.Authority).ValidateAadIssuer;

  // Set the nameClaimType to be preferred_username.
  // This change is needed because certain token claims from Azure AD v1.0 endpoint
  // (on which the original .NET core template is based) are different in Azure AD v2.0 endpoint.
  // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
  // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
  options.TokenValidationParameters.NameClaimType = "preferred_username";
  ...
```

## <a name="aspnet-configuration"></a>Configuración de ASP.NET Core

En ASP.NET, la aplicación se configura a través del archivo `Web.Config`.

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

El código relacionado con la autenticación en la aplicación web ASP.NET o las API web se encuentra en el archivo `App_Start/Startup.Auth.cs`.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio y cierre de sesión](scenario-web-app-sign-user-sign-in.md)
