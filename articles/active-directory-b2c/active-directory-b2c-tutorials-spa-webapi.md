---
title: 'Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial sobre cómo usar Active Directory B2C para proteger una API web de ASP.NET e invocarla desde una aplicación de una sola página.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 609e7bc4104a445c5d77109c9470fb487b2f1336
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507747"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C

En este tutorial se muestra cómo llamar a un recurso de API web ASP.NET Core protegido con Azure Active Directory (Azure AD) B2C desde una aplicación de página única.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos y requisitos previos en [Tutorial: Habilitación de la autenticación de una aplicación de página única con cuentas mediante Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md).

## <a name="add-a-web-api-application"></a>Incorporar una aplicación de API web

Los recursos de API web tienen que registrarse en el inquilino antes de que puedan aceptar y responder a solicitudes de recursos protegidos por aplicaciones cliente que presenten un token de acceso.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
6. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
7. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `https://localhost:5000`.
8. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
9. Haga clic en **Create**(Crear).
10. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, algunos usuarios pueden tener tanto acceso de lectura como de escritura, mientras que otros usuarios pueden tener permisos de solo lectura. En este tutorial, definirá los permisos de lectura para la API web.

1. Seleccione **Aplicaciones** y, a continuación, *webapi1*.
2. Seleccione **Ámbitos publicados**.
3. Como **ámbito**, escriba `Hello.Read` y para la descripción, escriba `Read access to hello`.
4. Como **ámbito**, escriba `Hello.Write` y para la descripción, escriba `Write access to hello`.
5. Haga clic en **Save**(Guardar).

Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente permiso para la API web.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde una aplicación, deberá conceder permisos de aplicación a la API. En el tutorial de requisitos previos, ha creado una aplicación web en Azure AD B2C denominada *webapp1*. Puede usar esta aplicación para llamar a la API web.

1. Seleccione **Aplicaciones** y, a continuación, seleccione *webapp1*.
2. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
3. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
4. En el menú desplegable **Seleccionar ámbitos**, seleccione los ámbitos **Hello.Read** y **Hello.Write** que definió previamente.
5. Haga clic en **OK**.

Su **aplicación de una sola página de ejemplo** está registrada para llamar a la API **Hello Core API** protegida. Un usuario se autentica con Azure AD B2C para utilizar la aplicación de página única. La aplicación de página única obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.

## <a name="configure-the-sample"></a>Configuración del ejemplo

Ahora que se ha registrado la API web y tiene ámbitos definidos, debe configurar el código de la API web para usar el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación web de ejemplo .NET Core que puede descargar desde GitHub. [Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) o clone la aplicación web de ejemplo desde GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Configuración de la API web

1. Abra la solución **B2C-WebAPI.sln** en Visual Studio.
2. Abra el archivo **appsettings.json**. Actualice los valores siguientes para configurar la API web para usar su inquilino:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<application-ID>",
        "Policy": "B2C_1_signupsignin1>",
        "ScopeRead": "Hello.Read"  
      },
    ```

#### <a name="enable-cors"></a>Habilitación de CORS

Para permitir que la aplicación de página única llame a la API web de ASP.NET Core, debe habilitar [CORS](https://docs.microsoft.com/aspnet/core/security/cors).

1. En **Startup.cs**, agregue CORS al método `ConfigureServices()`.

    ```C#
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. En **Startup.cs**, configure CORS en el método `Configure()`.

    ```C#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

3. Abra el archivo **launchSettings.json** en **Propiedades**, busque la configuración **iisSettings** *applicationURL* y establezca el número de puerto como el registrado para la dirección URL de la respuesta de la API `http://localhost:5000`.

### <a name="configure-the-single-page-application"></a>Configuración de la aplicación de página única

La aplicación de página única usa Azure AD B2C para realizar la suscripción de los usuarios e iniciar su sesión, y llama a la API web de ASP.NET Core protegida. Debe actualizar la aplicación de página única para que llame a la API web de .NET Core.

Para cambiar la configuración de la aplicación:

1. Abra el archivo `index.html` .
2. Configure el ejemplo con la información de registro del inquilino de Azure AD B2C. En el siguiente código, agregue el nombre de inquilino a **b2cScopes** y cambie el valor de **webApi** por el valor de *applicationURL* que grabó anteriormente:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<application-ID>',
        authority: "https://<your-tenant-name>.b2clogin.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/api/Hello.Read"],
        webApi: 'http://localhost:5000/api/values',
    };
    ```

## <a name="run-the-spa-application-and-web-api"></a>Ejecución de la aplicación SPA y la API web

Deberá ejecutar la aplicación de página única de Node.js y la API web de .NET Core.

### <a name="run-the-aspnet-core-web-api"></a>Ejecución de la API web ASP.NET Core 

Presione **F5** para depurar la solución **B2C-WebAPI.sln** en Visual Studio.

Cuando se inicia el proyecto, se muestra una página web en el explorador predeterminado que anuncia que la API web está disponible para las solicitudes.

### <a name="run-the-single-page-app"></a>Ejecución de la aplicación de una sola página

1. Inicie un símbolo del sistema de Node.js.
2. Cambie al directorio que contiene el ejemplo de Node.js. Por ejemplo, `cd c:\active-directory-b2c-javascript-msal-singlepageapp`.
3. Ejecute los comandos siguientes:
    ```
    npm install && npm update
    node server.js
    ```

    La ventana de la consola muestra el número de puerto donde se hospeda la aplicación.
    
    ```
    Listening on port 6420...
    ```

4. Use un explorador para ir a la dirección `http://localhost:6420` y ver la aplicación.
5. Inicie sesión con la dirección de correo electrónico y la contraseña que usó en [Authenticate users with Azure Active Directory B2C in a single page application (JavaScript)](active-directory-b2c-tutorials-spa.md) (Autenticación de los usuarios con Azure Active Directory B2C en una aplicación de una sola página [JavaScript]).
6. Haga clic en **Call API** (Llamar a la API).

Después de registrarse o iniciar sesión con una cuenta de usuario, el ejemplo llama a la API web protegida y devuelve un resultado.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Incorporar una aplicación de API web
> * Configurar los ámbitos para una API web
> * Conceder permisos a la API web
> * Configurar el ejemplo para que use la aplicación

> [!div class="nextstepaction"]
> [Tutorial: Adición de proveedores de identidades a las aplicaciones en Azure Active Directory B2C](tutorial-add-identity-providers.md)
