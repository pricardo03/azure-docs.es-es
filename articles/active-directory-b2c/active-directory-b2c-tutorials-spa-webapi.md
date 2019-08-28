---
title: 'Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C'
description: Aprenda a usar Active Directory B2C para proteger una API web de ASP.NET e invocarla desde una aplicación de una sola página de Node.js.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: b53ce30f4c49580bcd8ad3e259adf0300d8bd4a6
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68369312"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C

En este tutorial se muestra cómo llamar a un recurso de API web ASP.NET Core protegido con Azure Active Directory (Azure AD) B2C desde una aplicación de página única.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

## <a name="prerequisites"></a>Requisitos previos

* Complete los pasos y requisitos previos en [Tutorial: Habilitación de la autenticación en una aplicación de página única mediante Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md).
* Visual Studio 2019 o una versión posterior, o Visual Studio Code
* .NET Core 2.2 o una versión posterior
* Node.js

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

Los recursos de API web tienen que registrarse en el inquilino antes de que puedan aceptar y responder a solicitudes de recursos protegidos por aplicaciones cliente que presenten un token de acceso.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
1. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
1. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `https://localhost:5000`.
1. En **URI de id. de aplicación**, escriba un identificador de punto de conexión de API para el URI que se muestra. Para el tutorial, escriba `api`, de modo que el URI completo sea similar a `https://contosotenant.onmicrosoft.com/api`.
1. Haga clic en **Create**(Crear).
1. Seleccione la aplicación *webapi1* para abrir su página de propiedades.
1. En la página de propiedades, registre el **identificador de aplicación** que aparece. Lo necesitará en un paso posterior al configurar la aplicación web.

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, algunos usuarios pueden tener tanto acceso de lectura como de escritura, mientras que otros usuarios pueden tener permisos de solo lectura. En este tutorial, definirá los permisos de lectura y escritura para la API web.

1. Seleccione **Aplicaciones** y *webapi1* para abrir su página de propiedades si aún no está abierta.
1. Seleccione **Ámbitos publicados**.
1. En **ÁMBITO** escriba `Hello.Read` y en **DESCRIPCIÓN**, `Read access to hello`.
1. En **ÁMBITO** escriba `Hello.Write` y en **DESCRIPCIÓN**, `Write access to hello`.
1. Seleccione **Guardar**.
1. Registre el **VALOR DE ÁMBITO COMPLETO** para el ámbito `Hello.Read`, que se usará en un paso posterior al configurar la aplicación de página única. El valor de ámbito completo es similar a `https://yourtenant.onmicrosoft.com/api/Hello.Read`.

Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente permiso para la API web.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde otra aplicación, deberá conceder permisos de la API web a la aplicación.

En el tutorial de requisitos previos, ha creado una aplicación web denominada *webapp1*. En este tutorial, configurará esa aplicación para que llame a la API web que creó en un apartado anterior, *webapi1*.

1. Vaya al inquilino de B2C en Azure Portal.
1. Seleccione **Aplicaciones** y, a continuación, seleccione *webapp1*.
1. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
1. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
1. En el menú desplegable **Seleccionar ámbitos**, seleccione los ámbitos **Hello.Read** y **Hello.Write** que definió previamente.
1. Haga clic en **OK**.

La aplicación web de página única está registrada para llamar a la API web protegida. Un usuario se autentica con Azure AD B2C para utilizar la aplicación de página única. La aplicación de página única obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-sample"></a>Configuración del ejemplo

Ahora que se ha registrado la API web y tiene ámbitos definidos, debe configurar el código de la API web para usar el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación web de ejemplo con .NET Core que se descarga desde GitHub.

[Descargue un \*archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) o clone el proyecto de API web de ejemplo desde GitHub.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Configuración de la API web

1. Abra el archivo *B2C-WebApi/**appsettings.json***  en Visual Studio o Visual Studio Code.
1. Modifique el bloque `AzureAdB2C` para que refleje el nombre del inquilino, el identificador de la aplicación de API web, el nombre de la directiva de registro o de inicio de sesión y los ámbitos que definió anteriormente. El bloque debe ser similar al siguiente ejemplo (con los valores `Tenant` y `ClientId` adecuados):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "Hello.Read",
      "ScopeWrite": "Hello.Write"
    },
    ```

#### <a name="enable-cors"></a>Habilitación de CORS

Para permitir que la aplicación de página única llame a la API web de ASP.NET Core, debe habilitar [CORS](https://docs.microsoft.com/aspnet/core/security/cors) en la API web.

1. En *Startup.cs*, agregue CORS al método `ConfigureServices()`.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. También en el método `ConfigureServices()`, establezca el valor `jwtOptions.Authority` en el siguiente URI de emisor de token.

    Reemplace `<your-tenant-name>` por el nombre del inquilino de B2C.

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. En el método `Configure()`, configure CORS.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Solo Visual Studio) En el Explorador de soluciones, en **Propiedades**, abra el archivo *launchSettings.json* y busque el bloque `iisExpress`.
1. (Solo Visual Studio) Actualice el valor `applicationURL` con el número de puerto que especificó al registrar la aplicación *webapi1* en un paso anterior. Por ejemplo:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Configuración de la aplicación de página única

La aplicación de página única (SPA) del [tutorial anterior](active-directory-b2c-tutorials-spa.md) de la serie usa Azure AD B2C para el registro y el inicio de sesión y llama a la API web de ASP.NET Core protegida por el inquilino de demostración *frabrikamb2c*.

En esta sección actualizará la aplicación de página única para que llame a la API web de ASP.net Core protegida por *su* inquilino de Azure AD B2C que ejecuta en la máquina local.

Para cambiar la configuración en la aplicación de página única:

1. Abra el archivo *index.html* del proyecto [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa] que descargó o clonó en el tutorial anterior.
1. Configure el ejemplo con el URI para el ámbito *Hello.Read* que creó anteriormente y la dirección URL de la API web.
    1. En la definición de `appConfig`, reemplace el valor `b2cScopes` por el URI completo para el ámbito (el **VALOR DE ÁMBITO COMPLETO** que registró anteriormente).
    1. Cambie el valor `webApi` por el valor `applicationURL` que especificó en la sección anterior.

    La definición de `appConfig` debe ser similar al siguiente bloque de código (con el nombre del inquilino en el lugar de `<your-tenant-name>`):

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/Hello.Read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>Ejecución de la aplicación de página única y la API web

Por último, ejecute la API web de ASP.NET Core y la aplicación de página única de Node.js en la máquina local. A continuación, inicie sesión en la aplicación de página única y presione un botón para iniciar una solicitud a la API protegida.

Aunque ambas aplicaciones se ejecutan localmente en este tutorial, usan Azure AD B2C para el registro y el inicio de sesión seguros y para conceder acceso a la API web protegida.

### <a name="run-the-aspnet-core-web-api"></a>Ejecución de la API web de ASP.NET Core

En Visual Studio, presione **F5** para compilar y depurar la solución *B2C-WebAPI.sln*. Cuando se inicia el proyecto, se muestra una página web en el explorador predeterminado que anuncia que la API web está disponible para las solicitudes.

Si prefiere usar la CLI de `dotnet` en lugar de Visual Studio:

1. Abra una ventana de consola y cambie al directorio que contiene el archivo *\*.csproj*. Por ejemplo:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. Compile y ejecute la API web mediante la ejecución de `dotnet run`.

    Cuando la API esté en funcionamiento, debería ver una salida similar a la siguiente (para el tutorial, puede omitir cualquier advertencia `NETSDK1059` sin problema):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Ejecución de la aplicación de una sola página

1. Abra una ventana de consola y cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Ejecute los comandos siguientes:

    ```console
    npm install && npm update
    node server.js
    ```

    La ventana de la consola muestra el número de puerto donde se hospeda la aplicación.

    ```console
    Listening on port 6420...
    ```

1. Vaya a `http://localhost:6420` en el explorador para ver la aplicación.
1. Inicie sesión con la dirección de correo electrónico y la contraseña que usó en el [tutorial anterior](active-directory-b2c-tutorials-spa.md). Tras iniciar sesión correctamente, debería ver el mensaje `User 'Your Username' logged-in`.
1. Seleccione el botón **Call Web API** (Llamar a API web). La aplicación de página única obtiene una concesión de autorización de Azure AD B2C y accede a la API web protegida para mostrar el contenido de su página de índice:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

Ahora que ha visto una aplicación de página única solicitar un recurso de una API web protegida, entremos en detalles sobre cómo interactúan estos tipos de aplicaciones entre sí y con Azure AD B2C.

> [!div class="nextstepaction"]
> [Tipos de aplicaciones que se pueden usar en Active Directory B2C >](active-directory-b2c-apps.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
