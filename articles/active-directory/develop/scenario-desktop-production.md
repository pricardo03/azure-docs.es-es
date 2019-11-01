---
title: 'Aplicación de escritorio que llama a las API web: paso a producción - Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de escritorio que llama a las API web (paso a producción)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5331f01c5dc6acf01f567dbe4c332853bf7aa47e
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175554"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Aplicación de escritorio que llama a las API web: paso a producción

Este artículo proporciona detalles para mejorar aún más la aplicación y moverla a producción.

## <a name="handling-errors-in-desktop-applications"></a>Control de errores en aplicaciones de escritorio

En los distintos flujos, ha aprendido a controlar los errores para los flujos silenciosos (como se muestra en los fragmentos de código). También ha visto que hay casos donde se necesita una interacción (consentimiento incremental y acceso condicional).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Cómo tener el consentimiento del usuario por adelantado para varios recursos

> [!NOTE]
> Obtener el consentimiento para varios recursos funciona para la plataforma de identidad de Microsoft, pero no para Azure Active Directory (Azure AD) B2C. Azure AD B2C solo admite el consentimiento del administrador, no el consentimiento del usuario.

El punto de conexión de la plataforma de identidad de Microsoft (v2.0) no le permitirá obtener un token para varios recursos a la vez. Por lo tanto, el parámetro `scopes` solo puede contener ámbitos para un único recurso. Asegúrese de que el usuario da su consentimiento con anterioridad a varios recursos mediante el parámetro `extraScopesToConsent`.

Por ejemplo, si tiene dos recursos, los cuales tienen cada uno dos ámbitos:

- `https://mytenant.onmicrosoft.com/customerapi`: con 2 ámbitos `customer.read` y `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi`: con 2 ámbitos `vendor.read` y `vendor.write`

Debe usar el modificador `.WithAdditionalPromptToConsent` que tiene el parámetro `extraScopesToConsent`.

Por ejemplo:

### <a name="in-msalnet"></a>En MSAL.NET

```CSharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>En MSAL para iOS y macOS

Objective-C:

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.onmicrosoft.com/customerapi/customer.read",
                                @"https://mytenant.onmicrosoft.com/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                              @"https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift:

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

Con esta llamada obtendrá un token de acceso para la primera API web.

Cuando necesite llamar a la segunda API web, puede llamar a la API `AcquireTokenSilent`:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>La cuenta personal de Microsoft requiere volver a solicitar el consentimiento cada vez que se ejecuta la aplicación

Para los usuarios de cuentas personales de Microsoft, el comportamiento previsto consiste en una llamada que vuelve a solicitar el consentimiento en cada llamada de cliente nativo (aplicación de escritorio o móvil) para autorizarlo. La identidad del cliente nativo es insegura de forma inherente (al contrario que la aplicación cliente confidencial, que intercambia un secreto con la plataforma de identidad de Microsoft para demostrar su identidad). La plataforma de identidad de Microsoft eligió mitigar esta inseguridad para servicios del consumidor pidiendo al usuario su consentimiento, cada vez que se autoriza la aplicación.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
