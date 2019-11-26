---
title: Obtención de consentimiento para varios recursos (Biblioteca de autenticación de Microsoft para .NET)
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo un usuario puede recibir consentimiento previo para varios recursos con la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3a3d50e94f76341dc349eda997d6e25ca96dec0
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802691"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>El usuario obtiene consentimiento para varios recursos con MSAL.NET
El punto de conexión de la plataforma de identidad de Microsoft no le permitirá obtener un token para varios recursos a la vez. Cuando use la Biblioteca de autenticación de Microsoft para .NET (MSAL.NET), el parámetro de ámbitos del método de adquisición de token solo debe contener ámbitos para un recurso único. Sin embargo, puede dar su consentimiento para varios recursos por adelantado si especifica otros ámbitos con el método de generador de `.WithExtraScopeToConsent`.

> [!NOTE]
> Obtener el consentimiento para varios recursos funciona para la plataforma de identidad de Microsoft, pero no para Azure AD B2C. Azure AD B2C solo admite el consentimiento del administrador, no el consentimiento del usuario.

Por ejemplo, si tiene dos recursos que tienen dos ámbitos cada uno:

- https:\//mytenant.onmicrosoft.com/customerapi (con 2 ámbitos, `customer.read` y `customer.write`)
- https:\//mytenant.onmicrosoft.com/vendorapi (con 2 ámbitos, `vendor.read` y `vendor.write`)

Debe usar el modificador `.WithExtraScopeToConsent` que tiene el parámetro *extraScopesToConsent*, tal como se muestra en el ejemplo siguiente:

```csharp
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

Con esto, obtendrá un token de acceso para la primera API web. Luego, cuando necesite acceso a la segunda API web, podrá adquirir silenciosamente el token desde la caché de tokens:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
