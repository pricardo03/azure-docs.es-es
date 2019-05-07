---
title: 'Aplicación de escritorio que llama a las API web (pasar a producción): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de escritorio que llama a web API (pasar a producción)
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
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ca66a41f26c54bf04273682d14889a36b688c70
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075136"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Aplicación de escritorio que llama a web API: pasar a producción

Este artículo proporcionan detalles para mejorar aún más la aplicación y moverla a producción.

## <a name="handling-errors-in-desktop-applications"></a>Control de errores en las aplicaciones de escritorio

En los distintos flujos, ha aprendido cómo controlar los errores para los flujos silenciosos (como se muestra en los fragmentos de código). También hemos visto que hay casos donde es la interacción necesaria (consentimiento incremental y el acceso condicional).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Cómo hacer que el consentimiento del usuario por adelantado para varios recursos

> [!NOTE]
> Obtención del consentimiento para works varios de los recursos para la plataforma Microsoft identity, pero no para Azure Active Directory (Azure AD) B2C. B2C de Azure AD admite solo consentimiento del administrador, no el consentimiento de usuario.

El punto de conexión de la plataforma (v2.0) de Microsoft identity no podrá obtener un token para varios recursos a la vez. Por lo tanto, el `scopes` parámetro solo puede contener ámbitos para un único recurso. Asegúrese de que el usuario previamente da su consentimiento a varios recursos mediante el uso de la `extraScopesToConsent` parámetro.

Por ejemplo, si tiene dos recursos, que tiene dos ámbitos de cada uno:

- `https://mytenant.onmicrosoft.com/customerapi` -con 2 ámbitos `customer.read` y `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` -con 2 ámbitos `vendor.read` y `vendor.write`

Debe usar el `.WithAdditionalPromptToConsent` modificador que tiene el `extraScopesToConsent` parámetro.

Por ejemplo:

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

Esta llamada obtendrá un token de acceso para la primera API web.

Cuando necesite llamar a la API web de segundo, puede llamar:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Cuenta personal de Microsoft requiere reconsenting cada vez que se ejecuta la aplicación

Para los usuarios de cuentas personales de Microsoft, reprompting consentimiento en cada llamada de cliente nativo (aplicación de escritorio o móviles) para autorizar a es el comportamiento previsto. Identidad del cliente nativo es inherentemente inseguro (al contrario que la aplicación cliente confidencial que un secreto con la plataforma Microsoft Identity para demostrar su identidad de exchange). Ha elegido la plataforma de identidad de Microsoft mitigar este inseguridad para servicios de consumidor pidiendo al usuario su consentimiento, cada vez que la aplicación está autorizada.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
