---
title: 'Aplicación web que llama a las API web (inicio de sesión): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación web que llame a API web (inicio de sesión)
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
ms.openlocfilehash: 663cea72eb620217ad5fa8925d3bb00eedbf890c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074566"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Aplicación web que llama a las API web: inicio de sesión

Ya sabe cómo agregar el inicio de sesión a una aplicación web. Obtuvo información sobre esto en [Aplicación web que permite iniciar sesión a los usuarios (inicio de sesión)](scenario-web-app-sign-user-sign-in.md).

Lo diferente de aquí es que, cuando el usuario ha cerrado la sesión, desde esta aplicación o desde cualquier aplicación, querrá quitar de la caché de tokens los tokens asociados con el usuario.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Interceptar la devolución de llamada después del cierre de sesión: cierre de sesión único

La aplicación puede interceptar el evento posterior a `logout`, por ejemplo para borrar la entrada de la caché de tokens asociada con la cuenta que ha cerrado la sesión. Veremos en la segunda parte de este tutorial (acerca de la aplicación web que llama a una API web), que la aplicación web almacenará los tokens de acceso para el usuario en una memoria caché. Interceptar la devolución de llamada posterior a `logout` permite que la aplicación web quite al usuario de la caché de tokens. Este mecanismo se ilustra en el método `AddMsal()` de [StartupHelper.cs L137 143](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L137-L143).

La **URL de cierre de sesión** que ha registrado para la aplicación le permite implementar el cierre de sesión único. El punto de conexión `logout` de la Plataforma de identidad de Microsoft llamará a la **URL de cierre de sesión** registrada en la aplicación. Esta llamada se produce si el cierre de sesión se ha iniciado desde la aplicación web, o desde otra aplicación web o el explorador. Para más información, consulte [Cierre de sesión único](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc#single-sign-out) en la documentación conceptual.

```CSharp
public static IServiceCollection AddMsal(this IServiceCollection services, IEnumerable<string> initialScopes)
{
    services.AddTokenAcquisition();

    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
     ...
        // Handling the sign-out: removing the account from MSAL.NET cache
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            // Remove the account from MSAL.NET token cache
            var _tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
            await _tokenAcquisition.RemoveAccount(context);
        };
    });
    return services;
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquisición de un token para la aplicación web](scenario-web-app-call-api-acquire-token.md)
