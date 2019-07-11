---
title: Compilar una API web de .NET que se integra con Azure AD para su autenticación y autorización | Microsoft Docs
description: Cómo crear una API web de .NET MVC que se integra con Azure AD para su autenticación y autorización.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: ryanwi
ms.reviewer: jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83f5b08e5fee17c0ea5577d4d56d4d3208a818e3
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625307"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Inicio rápido: compilación de una API web de .NET que se integra con Azure AD para su autenticación y autorización

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Si va a crear una aplicación que proporciona acceso a recursos protegidos, debe saber cómo proteger dichos recursos frente a accesos injustificados. Azure Active Directory (Azure AD) facilita la protección de una API web mediante tokens de acceso portadores de OAuth 2.0 con solo unas pocas líneas de código.

En las aplicaciones web ASP.NET, puede realizar esta protección con la implementación de Microsoft del middleware OWIN controlado por la comunidad incluido en .NET Framework 4.5. En este caso, usaremos OWIN para compilar una API web de "Lista de tareas pendientes":

* Designar las API que están protegidas.
* Validar que las llamadas de API web contengan un token de acceso válido.

En este tutorial de inicio rápido, creará la To Do List API y aprenderá a hacer lo siguiente:

1. Registrar una aplicación con Azure AD.
2. Configurar la aplicación para que use la canalización de autenticación OWIN.
3. Configurar una aplicación cliente para llamar a la API web.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, complete estos requisitos previos:

* Descargue el [esqueleto de la aplicación](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) o el [ejemplo completado](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Cualquiera de los dos es una solución de Visual Studio 2013.
* Necesitará a un inquilino de Azure AD en el que registrar la aplicación. Si aún no tiene uno, [descubra cómo conseguirlo](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>Paso 1: Registro de una aplicación con Azure AD

Para proteger su aplicación, primero debe crear una aplicación en el inquilino y proporcionar a Azure AD algunos elementos de información clave.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Elija el inquilino de Azure AD; para ello, haga clic en su cuenta en la esquina superior derecha de la página y, después, haga clic en el menú de navegación **Cambiar directorio** y seleccione el inquilino adecuado.
    * Omita este paso si tiene solo un inquilino de Azure AD en su cuenta o si ya ha seleccionado el inquilino de Azure AD adecuado.

3. En el panel de navegación izquierdo, elija **Azure Active Directory**.
4. Seleccione **Registros de aplicaciones** y luego **Nuevo registro**.
5. Cuando se abra la página **Registrar una aplicación**, escriba el nombre de su aplicación.
En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
6. Seleccione la plataforma **Web** en la sección **URI de redireccionamiento** y establezca el valor en `https://localhost:44321/` (la ubicación a la que Azure AD devolverá tokens).
7. Cuando termine, seleccione **Registrar**. En la página de **Información general** de la aplicación, anote el valor en **Id. de aplicación (cliente)** .
6. Seleccione **Exponer una API** y haga clic en **Establecer** para actualizar el URI de identificación de la aplicación. Especifique un identificador específico del inquilino. Por ejemplo, escriba: `https://contoso.onmicrosoft.com/TodoListService`.
7. Guarde la configuración. Deje abierto el portal, ya que también deberá registrar su aplicación cliente en breve.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Paso 2: Configuración de la aplicación para que use la canalización de autenticación OWIN

Para validar solicitudes y tokens entrantes, debe configurar la aplicación para que se comunique con Azure AD.

1. Para comenzar, abra la solución y agregue los paquetes NuGet de middleware OWIN al proyecto TodoListService mediante la Consola del Administrador de paquetes.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Agregue una Clase de inicio OWIN al proyecto de ServicioListaTodo llamado `Startup.cs`.  Haga clic con el botón derecho en el proyecto, seleccione **Agregar > Nuevo elemento** y luego busque **OWIN**. El middleware OWIN invocará el método `Configuration(…)` al iniciarse la aplicación.

3. Cambie la declaración de clase a `public partial class Startup`. Ya hemos implementado parte de esta clase en otro archivo. En el método `Configuration(…)`, realice una llamada a `ConfigureAuth(…)` para configurar la autenticación para su aplicación web.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Abra el archivo `App_Start\Startup.Auth.cs` e implemente el método `ConfigureAuth(…)`. Los parámetros que proporciona en `WindowsAzureActiveDirectoryBearerAuthenticationOptions` servirán como coordenadas para que su aplicación se comunique con Azure AD. Para usarlas, deberá usar clases en el espacio de nombres `System.IdentityModel.Tokens`.

    ```csharp
    using System.IdentityModel.Tokens;
    ```

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                 Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                 TokenValidationParameters = new TokenValidationParameters
                 {
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                 }
            });
    }
    ```

5. Use los atributos `[Authorize]` como ayuda para proteger los controladores y las acciones con la autenticación de portador de JSON Web Token (JWT). Agregue a la clase `Controllers\TodoListController.cs` una etiqueta de autorización, para obligar al usuario a iniciar sesión antes de acceder a esa página.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Cuando un autor de llamada autorizado invoca correctamente una de las API `TodoListController` , es posible que la acción deba tener acceso a información sobre este. OWIN proporciona acceso a las notificaciones dentro del token portador a través del objeto `ClaimsPrincpal` .  

6. Un requisito común de las API web es validar los "ámbitos" presentes en el token para garantizar que el usuario acepta los permisos necesarios para obtener acceso al servicio Lista de tareas pendientes.

    ```csharp
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. Abra el archivo `web.config` en la raíz del proyecto TodoListService y escriba sus valores de configuración en la sección `<appSettings>`.
    * `ida:Tenant` es el nombre del inquilino de Azure AD, por ejemplo, contoso.onmicrosoft.com.
    * `ida:Audience` es el URI de id. de aplicación de la aplicación que escribió en Azure Portal.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>Paso 3: Configuración de una aplicación cliente y ejecución del servicio

Para poder ver el servicio de lista de tareas pendientes en acción, debe configurar el cliente de lista de tareas pendientes para que pueda obtener tokens de Azure AD y realizar llamadas al servicio.

1. Vuelva a [Azure Portal](https://portal.azure.com).
1. Cree un registro de aplicación nuevo en el inquilino de Azure AD.  Escriba un **nombre** que describa la aplicación a los usuarios, escriba `http://TodoListClient/` para el valor **URI de redireccionamiento** y seleccione **Cliente público (dispositivos móviles y escritorio)** en la lista desplegable.
1. Cuando termine el registro, Azure AD asignará un identificador de aplicación único a la aplicación. Necesitará este valor en las secciones siguientes, así que cópielo de la página de la aplicación.
1. Seleccione **Permisos de API** y, luego, **Agregar un permiso**.  Busque y seleccione el servicio Lista de tareas pendientes, agregue el permiso **user_impersonation Access TodoListService** en **Permisos delegados** y, a continuación, haga clic en **Agregar permisos**.
1. En Visual Studio, abra `App.config` en el proyecto TodoListClient y escriba sus valores de configuración en la sección `<appSettings>`.

    * `ida:Tenant` es el nombre del inquilino de Azure AD, por ejemplo, contoso.onmicrosoft.com.
    * `ida:ClientId` es el id. de aplicación que copió de Azure Portal.
    * `todo:TodoListResourceId` es el URI de id. de aplicación de la aplicación del servicio de lista de tareas pendientes que escribió en Azure Portal.

1. Por último, compile y ejecute cada proyecto.
1. Si aún no lo ha hecho, ahora es el momento de crear un nuevo usuario en su inquilino con un dominio *.onmicrosoft.com.
1. Inicie sesión en el cliente de lista de tareas pendientes con ese usuario y agregue algunas tareas a la lista de tareas pendientes del usuario.

## <a name="next-steps"></a>Pasos siguientes

* Como referencia, descargue el ejemplo completado (sin sus valores de configuración) de [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Ya puede acceder a más escenarios de identidad.
