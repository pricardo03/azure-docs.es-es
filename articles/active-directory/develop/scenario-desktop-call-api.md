---
title: 'Aplicación de escritorio que llama a web API (llamando a una API web): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de escritorio que llama a web API (llamando a una API web)
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
ms.openlocfilehash: 6fb240b54c3d698ead9d3427f603acca2b53745a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075931"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Aplicación de escritorio que llama a web API: llamar a una API web

Ahora que tiene un token, puede llamar a una API web protegida.

## <a name="calling-a-web-api-from-net"></a>Una llamada a una API web de .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Llamar a varias API - consentimiento Incremental y el acceso condicional

Si necesita llamar a varias API para el mismo usuario, una vez que se obtuvo un token para la primera API, simplemente puede llamar a `AcquireTokenSilent`, y obtendrá un token para las otras API de forma silenciosa la mayoría del tiempo.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Los casos donde no se requiere interacción es cuando:

- El usuario dado su consentimiento para la primera API, pero ahora debe dar su consentimiento para varios ámbitos (consentimiento incremental)
- La primera API no requiere autenticación de varios factores, pero sí de lo siguiente.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Pasar a producción](scenario-desktop-production.md)
