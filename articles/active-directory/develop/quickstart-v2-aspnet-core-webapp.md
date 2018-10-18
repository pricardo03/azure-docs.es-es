---
title: Guía de inicio rápido de la aplicación web de ASP.NET Core para Azure AD v2.0 | Microsoft Docs
description: Aprenda a implementar el inicio de sesión de Microsoft en una aplicación web de ASP.NET Core mediante OpenID Connect.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4ab3d0b74e8305d67af862020197c69b15221086
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830232"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Adición del inicio de sesión con Microsoft a una aplicación web de ASP.NET Core

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Esta guía de inicio rápido contiene un código de ejemplo que muestra la forma en que una aplicación web de ASP.NET Core puede iniciar sesión en cuentas personales (hotmail.com, live.com, etc.), profesionales y educativas desde cualquier instancia de Azure Active Directory.

![Funcionamiento de la aplicación de ejemplo que se genera en esta guía de inicio rápido](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registro de una aplicación y descarga de una aplicación de inicio rápido
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registro y configuración de una aplicación y el código de ejemplo
> #### <a name="step-1-register-your-application"></a>Paso 1: Registro de la aplicación
> 
> 1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app).
> 1. Escriba un nombre de aplicación, asegúrese de que la opción de **Guided Setup** (Configuración guiada) no está activada y haga clic en **Create** (Crear).
> 1. Haga clic en `Add Platform` y, después, seleccione `Web`.
> 1. Asegúrese de que la casilla **Permitir flujo implícito** está *seleccionada*.
> 1. En **Direcciones URL de redireccionamiento**, escriba `http://localhost:3110/`.
> 1. Desplácese hasta la parte inferior de la página y haga clic en **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de esta guía de inicio rápido funcione, es preciso agregar una dirección URL de respuesta como `http://localhost:3110/`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-aspnet-core-webapp/green-check.png) La aplicación está configurada con este atributo

#### <a name="step-2-download-your-aspnet-core-project"></a>Paso 2: Descarga del proyecto de ASP.NET Core

- [Descarga del proyecto de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Paso 3: Configuración del proyecto

1. Extraiga el archivo ZIP en la carpeta local más próxima a la carpeta raíz (por ejemplo, **C:\Azure-Samples**).
1. Si usa Visual Studio 2017, abra el proyecto en Visual Studio (opcional)
1. Edite **appsettings.json** y sustituya el valor de `ClientId` por el identificador de la aplicación que acaba de registrar:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Si la aplicación es una *aplicación de inquilino único* (que tiene como destino solo las cuentas en el directorio actual), busque el valor de `TenantId` en el archivo **appsettings.json** y reemplace `common` por su **identificador de inquilino** o **nombre de inquilino** (por ejemplo, contoso.microsoft.com). El nombre del inquilino se puede obtener en la **página Información general**.

## <a name="more-information"></a>Más información

En esta sección se proporciona información general acerca del código necesario para el inicio de sesión de usuarios. Esto puede ser útil para comprender cómo funciona el código, los argumentos principales y también si desea agregar el inicio de sesión a una aplicación ASP.NET Core existente.

### <a name="startup-class"></a>Clase Startup

El middleware *Microsoft.AspNetCore.Authentication* usa una clase Startup que se ejecuta cuando se inicializa el proceso de hospedaje:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddAzureAd(options => Configuration.Bind("AzureAd", options))
    .AddCookie();

    services.AddMvc();
}
```

El método `AddAuthentication` configura el servicio para agregar la autenticación basada en cookies (que se usa en escenarios del explorador y establece el desafío en OpenIdConnect). 

La línea que contiene `.AddAzureAd` agrega la autenticación de Azure Active Directory a la aplicación.

Además, el archivo **AzureAdAuthenticationBuilderExtensions.cs** agrega el método de extensión a la canalización de AzureAd Authentication. Este método de extensión configura los atributos necesarios para Autenticación de Azure AD. El método `Configure` de la interfaz `IConfigureNamedOptions` contiene lo siguiente:

```csharp
public void Configure(string name, OpenIdConnectOptions options)
{
    options.ClientId = _azureOptions.ClientId;
    options.Authority = $"{_azureOptions.Instance}common/v2.0";   // V2 specific
    options.UseTokenLifetime = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters.ValidateIssuer = false;     // accept several tenants (here simplified)
}
```
> |Where  |  |
> |---------|---------|
> |ClientId     |El identificador de la aplicación registrada en Azure Portal|
> |Autoridad | El punto de conexión STS para que el usuario se autentique. Normalmente es https://login.microsoftonline.com/{tenant}/v2.0 para la nube pública, donde {tenant} es el nombre del inquilino, el identificador del inquilino o *common* para hacer referencia al punto de conexión común (que se usa para las aplicaciones multiinquilino)|
> |UseTokenLifetime |Indica que la cookie de autenticación debe coincidan con la del token de autenticación|
> |RequireHttpsMetadata     |Requiere HTTPS para la autoridad o la dirección de los metadatos. Se recomienda cambiar este valor a `True`|
> |TokenValidationParameters     | Una lista de parámetros para la validación del token. En este caso, `ValidateIssuer` se establece en `false` para indicar que puede aceptar inicios desde cualquier cuenta personal, profesional o educativa|

### <a name="initiate-an-authentication-challenge"></a>Inicio de un desafío de autenticación

Puede forzar a un usuario a iniciar sesión mediante la solicitud de un desafío de autenticación en el controlador como **AccountController.cs**:

```csharp
public IActionResult SignIn()
{
    var redirectUrl = Url.Action(nameof(HomeController.Index), "Home");
    return Challenge(
        new AuthenticationProperties { RedirectUri = redirectUrl },
        OpenIdConnectDefaults.AuthenticationScheme);
}
```

> [!TIP]
> Solicitar un desafío de autenticación mediante el método anterior es opcional y habitualmente se utiliza cuando se desea que exista una vista disponible tanto para los usuarios autenticados como para los que no lo están. Puede proteger los controladores mediante el método que se describe en la sección siguiente.

### <a name="protect-a-controller-or-a-controllers-method"></a>Protección de un controlador o del método de un controlador

Puede proteger un controlador o los métodos de un controlador mediante el atributo `[Authorize]`. Este atributo restringe el acceso al controlador o a los métodos, ya que solo permite usuarios autenticados (lo que significa que se puede iniciar el desafío de autenticación para acceder al controlador si el usuario no está autenticado).

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte el repositorio de GitHub de la guía de inicio rápido de ASP.NET Core (lo que incluye las instrucciones necesarias para agregar autenticación a una aplicación web de ASP.NET Core totalmente nueva:

> [!div class="nextstepaction"]
> [Ejemplo de código de una aplicación web de ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]