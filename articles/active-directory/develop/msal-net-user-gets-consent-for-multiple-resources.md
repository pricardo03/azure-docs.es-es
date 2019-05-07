---
title: Obtener el consentimiento para varios recursos (biblioteca de autenticación de Microsoft para. NET) | Azure
description: Obtenga información sobre cómo un usuario puede obtener el consentimiento previo para varios recursos mediante la biblioteca de autenticación de Microsoft para .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45bad9a03e8eff6f22ebb99fd2ef4bcd5fecf9b5
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158826"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Los usuarios obtienen su consentimiento para varios recursos mediante MSAL.NET
El punto de conexión de plataforma de identidad de Microsoft no podrá obtener un token para varios recursos a la vez. Cuando se usa la biblioteca de autenticación de Microsoft para .NET (MSAL.NET), el parámetro de ámbitos en el método de token de adquisición solo debe contener ámbitos para un único recurso. Sin embargo, puede previamente da su consentimiento a varios recursos por adelantado mediante la especificación de ámbitos adicionales mediante el `.WithExtraScopeToConsent` método del generador.

> [!NOTE]
> Obtención del consentimiento para works varios de los recursos para la plataforma Microsoft identity, pero no para Azure AD B2C. B2C de Azure AD admite solo consentimiento del administrador, no el consentimiento de usuario.

Por ejemplo, si tiene dos recursos que tienen 2 establece el ámbito de cada uno:

- https://mytenant.onmicrosoft.com/customerapi (con 2 ámbitos `customer.read` y `customer.write`)
- https://mytenant.onmicrosoft.com/vendorapi (con 2 ámbitos `vendor.read` y `vendor.write`)

Debe usar el `.WithExtraScopeToConsent` modificador que tiene el *extraScopesToConsent* parámetro tal como se muestra en el ejemplo siguiente:

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

Obtendrá un token de acceso para la primera API web. A continuación, cuando necesite tener acceso a la segunda API web en modo silencioso puede adquirir el token de la caché de tokens:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
