---
title: Aplicación Web que inician sesión los usuarios (configuración de código), plataforma Microsoft identity
description: Obtenga información sobre cómo compilar una aplicación web que inician sesión los usuarios (configuración de código)
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
ms.openlocfilehash: b2204fe3e08b3c4b909ddc8b7ade4cec219d34fb
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406628"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Que inicia sesión los usuarios - configuración de código de aplicación Web

Obtenga información sobre cómo configurar el código de la aplicación Web que los usuarios inicia sesión.

## <a name="libraries-used-to-protect-web-apps"></a>Bibliotecas usadas para proteger las aplicaciones Web

<!-- This section can be in an include for Web App and Web APIs -->
Las bibliotecas que se utiliza para proteger una aplicación Web (y una API Web) son:

| Plataforma | Biblioteca | DESCRIPCIÓN |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Extensiones de modelo de identidad para .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Se usa directamente por ASP.NET y ASP.NET Core, extensiones de identidad de Microsoft para .NET propone un conjunto de archivos DLL que se ejecuta tanto en .NET Framework y .NET Core. Desde una aplicación Web de ASP.NET/ASP.NET Core, puede controlar la validación del token usando la **TokenValidationParameters** clase (en concreto, en algunos escenarios de ISV) |

## <a name="aspnet-core-configuration"></a>Configuración de ASP.NET Core

### <a name="application-configuration-files"></a>Archivos de configuración de aplicación

En ASP.NET Core, una aplicación inicio de sesión de usuarios Web con la plataforma Microsoft identity se configura mediante el `appsettings.json` archivo. Los valores que se deben rellenar son:

- la nube `Instance` si desea que la aplicación se ejecute en las nubes nacionales
- la audiencia en `tenantId`
- el `clientId` para su aplicación, tal y como se copió del portal de Azure.

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

En el portal de Azure, la respuesta a los identificadores URI que se deben registrar en el **autenticación** página de la aplicación debe coincidir con estas direcciones URL; es decir, los dos archivos de configuración anteriores, estarían `https://localhost:44321/signin-oidc` como el applicationUrl es `http://localhost:3110` pero la `sslPort` está especificado (44321) y el `CallbackPath` es `/signin-oidc` tal como se define en el `appsettings.json`.
  
En la misma manera, el URI de cierre de sesión se establecería en `https://localhost:44321/signout-callback-oidc`.

### <a name="initialization-code"></a>Código de inicialización

En las aplicaciones Web de ASP.NET Core (y las API Web), el código que realiza la inicialización de la aplicación se encuentra en el `Startup.cs` archivo, y para agregar la autenticación con Microsoft Identity platform (anteriormente Azure AD) v2.0, debe agregar el código siguiente. Los comentarios en el código deben ser claros.

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

## <a name="aspnet-configuration"></a>Configuración de ASP.NET

En ASP.NET, la aplicación se configura a través de la `Web.Config` archivo

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

El código relacionado con la autenticación en ASP.NET Web app / API Web se encuentra en el `App_Start/Startup.Auth.cs` archivo.

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
> [Inicie sesión y cierre la sesión](scenario-web-app-sign-user-sign-in.md)
