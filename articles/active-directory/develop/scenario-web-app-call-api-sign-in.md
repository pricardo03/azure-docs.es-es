---
title: 'Aplicación Web que llama a web API (inicio de sesión): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación Web que llama a web API (inicio de sesión)
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074566"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Aplicación Web que llama a web API: inicio de sesión

Ya sabe cómo agregar el inicio de sesión a la aplicación web. Obtenga información sobre esto en [aplicación Web que inicia sesión usuarios: agregar inicio de sesión](scenario-web-app-sign-user-sign-in.md).

¿Qué es diferente de aquí, es que cuando el usuario ha iniciado horizontalmente, de esta aplicación o desde cualquier aplicación, que desea quitar de la caché de tokens, los tokens asociados con el usuario.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Interceptar la devolución de llamada después de cierre de sesión: cierre de sesión único

La aplicación puede interceptar la después `logout` eventos, por ejemplo para borrar la entrada de la caché del token asociada con la cuenta que cerrar la sesión. Veremos en la segunda parte de este tutorial (acerca de la aplicación Web de una llamada a una API Web), que la aplicación web almacenará los tokens de acceso para el usuario en una memoria caché. Interceptar la después `logout` devolución de llamada permite que la aplicación web quitar el usuario de la caché de tokens. Este mecanismo se ilustra en la `AddMsal()` método [StartupHelper.cs L137 143](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L137-L143)

El **Logout Url** que ha registrado para la aplicación le permite implementar el cierre de sesión único. La plataforma Microsoft identity `logout` llamará el punto de conexión de la **Logout URL** registrado con la aplicación. Esta llamada se produce si el cierre de sesión se inició desde la aplicación web, o desde otra aplicación web o el explorador. Para obtener más información, consulte [cierre de sesión único](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc#single-sign-out) en la documentación conceptual.

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
> [Adquirir un token para la aplicación web](scenario-web-app-call-api-acquire-token.md)
