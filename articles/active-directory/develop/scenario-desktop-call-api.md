---
title: 'Aplicación de escritorio que llama a las API web: llamada a una API web - Plataforma de identidad de Microsoft'
description: 'Obtener información sobre cómo compilar una aplicación de escritorio que llama a las API web: llamada a una API web'
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
ms.openlocfilehash: 4abaf234d3b216e0f67501e5d2f2f5c3f874c5d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111240"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Aplicación de escritorio que llama a las API web: llamada a una API web

Ahora que dispone de un token, puede llamar a una API web protegida.

## <a name="calling-a-web-api-from-net"></a>Llamada a una API web desde .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Llamada a varias API: consentimiento incremental y acceso condicional

Si necesita llamar a varias API para el mismo usuario, una vez que obtenga un token para la primera API, simplemente puede llamar a `AcquireTokenSilent` y la mayoría de las veces obtendrá un token para las otras API de forma silenciosa.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Los casos donde se requiere interacción son los siguientes:

- Cuando el usuario ha dado su consentimiento para la primera API, pero ahora debe dar su consentimiento para más ámbitos (consentimiento incremental).
- Cuando la primera API no requería autenticación multifactor, pero la siguiente sí.

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
> [Paso a producción](scenario-desktop-production.md)
