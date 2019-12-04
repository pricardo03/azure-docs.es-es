---
title: 'Llamada a una instancia de ASP.NET Web API protegida por Azure AD: plataforma de identidad de Microsoft'
description: En esta guía de inicio rápido, aprenderá a llamar a una ASP.NET Web API protegida por Azure Active Directory desde una aplicación de escritorio de Windows (WPF). El cliente de WPF autentica a un usuario, solicita un token de acceso y llama a la API web.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.collection: M365-identity-device-management
ms.openlocfilehash: c558d45702498e6c1164d7ee1731e80ff195349e
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328554"
---
# <a name="build-a-multi-tenant-daemon-with-the-microsoft-identity-platform-endpoint"></a>Compilación de un demonio multiinquilino con el punto de conexión de la plataforma de identidad de Microsoft

En este tutorial, aprenderá a usar la plataforma de identidad de Microsoft para acceder a los datos de los clientes empresariales de Microsoft en un proceso no interactivo de larga duración. En el demonio de ejemplo se usa la [concesión de credenciales de cliente de OAuth2](v2-oauth2-client-creds-grant-flow.md) para adquirir un token de acceso, que luego se utiliza para llamar a [Microsoft Graph](https://graph.microsoft.io) y acceder a los datos de la organización.

La aplicación se compila como una aplicación ASP.NET MVC y usa el middleware OpenID Connect de OWIN para iniciar la sesión de los usuarios.  Su componente "demonio" en este ejemplo es un controlador de API, que, cuando se invoca, extrae una lista de usuarios en el inquilino de Azure AD del cliente de Microsoft Graph.  Este controlador `SyncController.cs` se desencadena mediante una llamada AJAX en la aplicación web y usa la [biblioteca de autenticación de Microsoft (MSAL) para .NET](msal-overview.md) para adquirir un token de acceso para Microsoft Graph.

Para información sobre una aplicación de demonio de consola más sencilla, consulte el [inicio rápido del demonio de .NET Core](quickstart-v2-netcore-daemon.md).

## <a name="scenario"></a>Escenario

Dado que la aplicación es una aplicación multiinquilino que cualquier cliente empresarial de Microsoft puede usar, debe proporcionar a los clientes una forma de "registrar" la aplicación o "conectarla" a los datos de la empresa.  Durante el flujo de conexión, el administrador de una empresa concede primero los **permisos de aplicación** directamente a la aplicación, de forma que pueda acceder a los datos de la empresa de manera no interactiva, sin la presencia de usuarios que han iniciado la sesión.  La mayoría de la lógica de este ejemplo muestra cómo lograr este flujo de conexión mediante el punto de conexión de [consentimiento del administrador](v2-permissions-and-consent.md#using-the-admin-consent-endpoint) de la plataforma de identidad.

![Topología](./media/tutorial-v2-aspnet-daemon-webapp/topology.png)

Para más información sobre los conceptos que se usan en este ejemplo, asegúrese de leer la [documentación del protocolo de credenciales de cliente del punto de conexión de la plataforma de identidad](v2-oauth2-client-creds-grant-flow.md).

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el ejemplo de este inicio rápido, necesitará lo siguiente:

- [Visual Studio 2017 o 2019](https://visualstudio.microsoft.com/downloads/)
- Un inquilino de Azure Active Directory (Azure AD). Para más información sobre cómo obtener un inquilino de Azure AD, consulte [cómo obtener un inquilino de Azure AD](quickstart-create-new-tenant.md).
- Una o varias cuentas de usuario en el inquilino de Azure AD. Este ejemplo no funcionará con un cuenta Microsoft (anteriormente cuenta de Windows Live). Por lo tanto, si ha iniciado sesión en [Azure Portal](https://portal.azure.com) con una cuenta Microsoft y nunca ha creado una cuenta de usuario en el directorio, deberá hacerlo ahora.

## <a name="clone-or-download-this-repository"></a>Clonación o descarga de este repositorio

Desde el shell o la línea de comandos, ejecute:

```Shell
git clone https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2.git
```

También puede [descargar el ejemplo en un archivo ZIP](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/archive/master.zip).

## <a name="register-the-sample-application-with-your-azure-ad-tenant"></a>Registro de la aplicación de ejemplo en un inquilino de Azure AD

En este ejemplo hay un proyecto. Para registrarlo, puede:

- Seguir los pasos [Registro del ejemplo en el inquilino de Azure Active Directory](#register-the-sample-application-with-your-azure-ad-tenant) y [Configuración del ejemplo para usar el inquilino de Azure AD](#choose-the-azure-ad-tenant-for-the-applications).
- O bien, usar scripts de PowerShell que realizan las siguientes acciones:
  - Crean **automáticamente** las aplicaciones de Azure AD y los objetos relacionados (contraseñas, permisos, dependencias).
  - Modifican los archivos de configuración de los proyectos de Visual Studio.

Si quiere usar esta automatización:

1. En Windows, ejecute PowerShell y vaya a la raíz del directorio clonado.
1. En PowerShell, ejecute:

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   ```

1. Ejecute el script para crear la aplicación de Azure AD y configure el código de la aplicación de ejemplo en consecuencia.
1. En PowerShell, ejecute:

   ```PowerShell
   .\AppCreationScripts\Configure.ps1
   ```

   > En la sección de [scripts de creación de aplicaciones](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/blob/master/AppCreationScripts/AppCreationScripts.md), se describen otras formas de ejecutar los scripts.

1. Abra la solución de Visual Studio y haga clic en Iniciar para ejecutar el código.

Si no quiere usar esta automatización, siga los pasos que se indican a continuación.

### <a name="choose-the-azure-ad-tenant-for-the-applications"></a>Elección del inquilino de Azure AD para las aplicaciones

Como primer paso, debe hacer lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Si su cuenta existe en más de un inquilino de Azure AD, seleccione su perfil en la esquina superior derecha del menú de la parte superior de la página y luego **cambie de directorio**.
   Cambie la sesión del portal por el inquilino de Azure AD deseado.

### <a name="register-the-client-app-dotnet-web-daemon-v2"></a>Registro de la aplicación cliente (dotnet-web-daemon-v2)

1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
1. Seleccione **Nuevo registro**.
1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `dotnet-web-daemon-v2`.
   - En la sección **Tipos de cuenta compatibles**, seleccione **Cuentas en cualquier directorio organizativo**.
   - En la sección URI de redirección (opcional), seleccione **Web** en el cuadro combinado y escriba los siguientes URI de redirección:
       - `https://localhost:44316/`
       - `https://localhost:44316/Account/GrantPermissions` Si hay más de un URI de redirección, deberá agregarlos desde la pestaña **Autenticación** más adelante, después de que la aplicación se haya creado correctamente.
1. Seleccione **Registrar** para crear la aplicación.
1. En la página **Información general** de la aplicación, busque el valor de **Id. de aplicación (cliente)** y regístrelo para usarlo más tarde. Lo necesitará para configurar el archivo de configuración de Visual Studio para este proyecto.
1. En la lista de páginas de la aplicación, seleccione **Autenticación**.
   - En la sección **Configuración avanzada**, establezca **Dirección URL de cierre de sesión** en `https://localhost:44316/Account/EndSession`.
   - En la sección **Configuración avanzada** | **Concesión implícita**, marque **Tokens de acceso** y **Tokens de id.** , ya que en este ejemplo es necesario habilitar el [flujo de concesión implícito](v2-oauth2-implicit-grant-flow.md) para iniciar la sesión del usuario y llamar a una API.
1. Seleccione **Guardar**.
1. En la página **Certificados y secretos**, en la sección **Secretos de cliente**, elija **Nuevo secreto de cliente**.

   - Escriba una descripción de la clave (`app secret` de la instancia).
   - Seleccione una duración de la clave: **En 1 año**, **En 2 años** o **Nunca expira**.
   - Cuando presione el botón **Agregar**, el valor de la clave se mostrará, se copiará y se guardará en una ubicación segura.
   - Necesitará esta clave más adelante para configurar el proyecto en Visual Studio. Este valor de clave no se volverá a mostrar ni a recuperar de ninguna otra forma, de modo que regístrelo en cuanto esté visible en Azure Portal.
1. En la lista de páginas de la aplicación, seleccione **Permisos de API**.
   - Haga clic en el botón **Agregar un permiso** y, a continuación,
   - Asegúrese de que la pestaña **API de Microsoft** esté seleccionada.
   - En la sección *API de Microsoft más usadas*, haga clic en **Microsoft Graph**.
   - En la sección **Permisos de aplicación**, asegúrese de que estén marcados los permisos correctos: **User.Read.All**
   - Seleccione el botón **Agregar permisos**.

## <a name="configure-the-sample-to-use-your-azure-ad-tenant"></a>Configuración del ejemplo para usar el inquilino de Azure AD

En los pasos siguientes, "ClientID" es igual que "Application ID" o "AppId".

Abra la solución en Visual Studio para configurar los proyectos.

### <a name="configure-the-client-project"></a>Configuración del proyecto de cliente

Si ha usado los scripts de instalación, se habrán aplicado los siguientes cambios.

1. Abra el archivo `UserSync\Web.Config`.
1. Busque la clave de la aplicación `ida:ClientId` y reemplace el valor existente por el identificador de la aplicación (clientId) `dotnet-web-daemon-v2` copiada de Azure Portal.
1. Busque la clave de la aplicación `ida:ClientSecret` y reemplace el valor existente por la clave que guardó durante la creación de la aplicación `dotnet-web-daemon-v2` en Azure Portal.

## <a name="run-the-sample"></a>Ejecución del ejemplo

Limpie la solución, vuelva a compilarla y ejecute la aplicación UserSync. Para comenzar, inicie sesión como administrador en el inquilino de Azure AD.  Si aún no tiene un inquilino de Azure AD, [siga estas instrucciones](quickstart-create-new-tenant.md) para obtener uno.

Cuando inicie sesión, la aplicación le pedirá primero permiso para iniciar su sesión y leer su perfil de usuario.  Este consentimiento permite a la aplicación asegurarse de que es un usuario empresarial.

![Consentimiento del usuario](./media/tutorial-v2-aspnet-daemon-webapp/firstconsent.png)

Luego, la aplicación intentará sincronizar una lista de usuarios de su inquilino de Azure AD mediante Microsoft Graph.  Si no puede hacerlo, le pedirá a usted (el administrador del inquilino) que conecte el inquilino a la aplicación.

La aplicación le pedirá entonces permiso para leer la lista de usuarios del inquilino.

![Consentimiento del administrador](./media/tutorial-v2-aspnet-daemon-webapp/adminconsent.PNG)

**Después de conceder permiso, se cerrará la sesión de la aplicación**. Esto se hace para garantizar que todos los tokens de acceso existentes para Graph se quitan de la caché de tokens. Tras volver a iniciar la sesión, el token nuevo obtenido tendrá los permisos necesarios para realizar llamadas a MS Graph.
Al conceder el permiso, la aplicación podrá consultar los usuarios en cualquier momento.  Para comprobarlo, haga clic en el botón **Sincronizar usuarios** de la página de usuarios; se actualiza la lista de usuarios.  Pruebe a agregar o quitar un usuario y volver a sincronizar la lista (pero tenga en cuenta que solo se sincroniza la primera página de usuarios).

## <a name="about-the-code"></a>Sobre el código

El código de interés para este ejemplo se encuentra en los siguientes archivos:

- Inicio de sesión inicial: `App_Start\Startup.Auth.cs`, `Controllers\AccountController.cs`.  En concreto, las acciones del controlador tienen un atributo Authorize, que obliga al usuario a iniciar sesión. La aplicación usa el [flujo de código de autorización](v2-oauth2-auth-code-flow.md) para iniciar la sesión del usuario.
- Sincronizar la lista de usuarios con el almacén local en memoria: `Controllers\SyncController.cs`
- Sincronizar la lista de usuarios desde el almacén local en memoria: `Controllers\UserController.cs`
- Adquirir permisos del administrador de inquilinos mediante el punto de conexión de consentimiento del administrador: `Controllers\AccountController.cs`

## <a name="recreate-this-sample-app"></a>Volver a crear esta aplicación de ejemplo

1. En Visual Studio, cree un proyecto `ASP.NET Web Application (.NET Framework)` de `Visual C#`. En la siguiente pantalla, elija la plantilla de proyecto `MVC`. También, agregue referencias de carpetas y núcleos para `Web API`, puesto que agregaría un controlador de Web API más adelante.  Deje el modo de autenticación elegido del proyecto como predeterminado, es decir, `No Authentication`".
2. Seleccione el proyecto en la ventana **Explorador de soluciones** y presione la tecla **F4** para mostrar las propiedades del proyecto. En las propiedades del proyecto, configure **SSL habilitado** como `True`. Anote el valor de **Dirección URL de SSL**. Lo necesitará al configurar el registro de esta aplicación en Azure Portal.
3. Agregue los siguientes paquetes NuGet del middleware OWIN para ASP.Net: `Microsoft.Owin.Security.ActiveDirectory`, `Microsoft.Owin.Security.Cookies` and `Microsoft.Owin.Host.SystemWeb`, `Microsoft.IdentityModel.Protocol.Extensions`, `Microsoft.Owin.Security.OpenIdConnect` and `Microsoft.Identity.Client`. 
4. En la carpeta `App_Start`, cree una clase `Startup.Auth.cs`. Quite `.App_Start` del nombre del espacio de nombres.  Reemplace el código de la clase `Startup` por el código del mismo archivo de la aplicación de ejemplo.  Asegúrese de usar la definición de clase completa.  La definición cambia de `public class Startup` a `public partial class Startup`.
5. En `Startup.Auth.cs`, agregue instrucciones `using` como sugiere Visual Studio IntelliSense para resolver las referencias que faltan.
6. Haga clic con el botón derecho en el proyecto, seleccione Agregar, seleccione "Clase" y, en el cuadro de búsqueda, escriba "OWIN".  La "clase de inicio OWIN" aparece como una selección; selecciónela y asigne a la clase el nombre `Startup.cs`.
7. En `Startup.cs`, reemplace el código de la clase `Startup` por el código del mismo archivo de la aplicación de ejemplo.  De nuevo, observe que la definición cambia de `public class Startup` a `public partial class Startup`.
8. En la carpeta, agregue una nueva clase denominada `MsGraphUser.cs`.  Reemplace la implementación por el contenido del archivo del mismo nombre del ejemplo.
9. Agregue un nuevo valor de **Controlador MVC 5: en blanco** llamado `AccountController`. Reemplace la implementación por el contenido del archivo del mismo nombre del ejemplo.
10. Agregue un nuevo valor de **Controlador MVC 5: en blanco** llamado `UserController`. Reemplace la implementación por el contenido del archivo del mismo nombre del ejemplo.
11. Agregue un nuevo valor de **Controlador de Web API 2 - en blanco** llamado `SyncController`. Reemplace la implementación por el contenido del archivo del mismo nombre del ejemplo.
12. En la interfaz de usuario, en la carpeta `Views\Account`, agregue tres **Vistas vacías (sin modelo)** llamadas `GrantPermissions`, `Index` y `UserMismatch`, y otra llamada `Index` a la carpeta `Views\User`. Reemplace la implementación por el contenido del archivo del mismo nombre del ejemplo.
13. Actualice `Shared\_Layout.cshtml` y `Home\Index.cshtml` para vincular correctamente las distintas vistas.

## <a name="deploy-this-sample-to-azure"></a>Implementación de este ejemplo en Azure

Este proyecto tiene proyectos de WebApp y Web API. Para implementarlos en sitios web de Azure, necesitará hacer lo siguiente para cada uno:

- crear un sitio web de Azure;
- publicar las instancias de Web App o Web API en el sitio web; y
- actualizar sus clientes para llamar al sitio web en lugar de a IIS Express.

### <a name="create-and-publish-the-dotnet-web-daemon-v2-to-an-azure-web-site"></a>Creación y publicación de `dotnet-web-daemon-v2` en un sitio web de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Haga clic en `Create a resource` en la esquina superior izquierda, seleccione **Web** --> **Web App** y proporcione a su sitio web un nombre, por ejemplo, `dotnet-web-daemon-v2-contoso.azurewebsites.net`.
1. Después, seleccione `Subscription`, `Resource Group``App service plan and Location`. `OS` será **Windows** y `Publish` será **Código**.
1. Haga clic en `Create` y espere a que se cree la instancia de App Service.
1. Cuando reciba la notificación `Deployment succeeded`, haga clic en `Go to resource` para ir a la instancia de App Service recién creada.
1. Después de crear el sitio web, búsquelo en el **Panel** y haga clic en él para abrir la pantalla **App Services** **Información general**.
1. En la pestaña **Información general** de App Service, haga clic en el vínculo **Obtener perfil de publicación** para descargar el perfil de publicación y guárdelo.  También se pueden usar otros mecanismos de implementación, como el control de código fuente.
1. Cambie a Visual Studio y vaya al proyecto dotnet-web-daemon-v2.  Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Publicar**.  Haga clic en **Importar perfil** en la barra inferior e importe el perfil de publicación que descargó anteriormente.
1. Haga clic en **Configurar** y, en `Connection tab`, actualice la dirección URL de destino de modo que sea `https` en la dirección URL de la página principal, por ejemplo, [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net). Haga clic en **Next**.
1. En la pestaña Configuración, asegúrese de que `Enable Organizational Authentication` NO está seleccionado.  Haga clic en **Save**(Guardar). Haga clic en **Publicar** en la pantalla principal.
1. Visual Studio publicará el proyecto y abrirá automáticamente un explorador con la dirección URL del proyecto.  Si ve la página web predeterminada del proyecto, significa que la publicación se realizó correctamente.

### <a name="update-the-azure-ad-tenant-application-registration-for-dotnet-web-daemon-v2"></a>Actualice el registro de aplicación del inquilino de Azure AD a `dotnet-web-daemon-v2`.

1. Vuelva a [Azure Portal](https://portal.azure.com).
En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory** y, después, elija **Registros de aplicaciones**.
1. En la pantalla resultante, seleccione la aplicación `dotnet-web-daemon-v2`.
1. En la página **Autenticación** de la aplicación, actualice los campos de dirección URL de cierre de sesión con la dirección del servicio, por ejemplo, [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net).
1. En la pestaña *Personalización de marca*, actualice el valor de **URL de página principal** con la dirección del servicio de aplicación, por ejemplo [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net). Guarde la configuración.
1. Agregue la misma dirección URL a la lista de valores del menú *Autenticación-> URI de redirección*. Si tiene varias direcciones URL de redirección, asegúrese de que haya una nueva entrada que use el URI de App Service para cada una.

## <a name="community-help-and-support"></a>Ayuda y soporte técnico de la comunidad

Use [Stack Overflow](http://stackoverflow.com/questions/tagged/msal) para obtener soporte técnico de la comunidad.
Primero plantee sus preguntas en Stack Overflow y examine los problemas existentes para ver si algún usuario ha hecho esa pregunta antes.
Asegúrese de que sus preguntas o comentarios se etiquetan con [`adal` `msal` `dotnet`].

Si encuentra un error en el ejemplo, genere el problema en la [sección de problemas de GitHub](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/issues).

Si encuentra un error en MSAL.NET, genere el problema en la sección de [problemas de GitHub para MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues).

Para proporcionar una recomendación, visite la siguiente [página de UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory).

## <a name="next-steps"></a>Pasos siguientes
Más información sobre los distintos [flujos de autenticación y escenarios de aplicaciones](authentication-flows-app-scenarios.md) que admite la plataforma de identidad de Microsoft.

Para más información, consulte la siguiente documentación conceptual:

- [Inquilinos en Azure Active Directory](single-and-multi-tenant-apps.md)
- [Descripción de las experiencias de consentimiento de la aplicación de Azure AD](application-consent-experience.md)
- [Uso de Inicio de sesión de cualquier usuario de Azure Active Directory mediante el patrón de aplicación multiinquilino](howto-convert-app-to-be-multi-tenant.md)
- [Descripción del consentimiento del usuario y del administrador](howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)
- [Objetos de aplicación y de entidad de servicio de Azure Active Directory](app-objects-and-service-principals.md)
- [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](quickstart-register-app.md)
- [Inicio rápido: Configuración de una aplicación cliente para tener acceso a las API web](quickstart-configure-app-access-web-apis.md)
- [Adquisición de un token para una aplicación con flujos de credenciales de cliente](msal-client-applications.md)

Para información sobre una aplicación de demonio de consola multiinquilino más sencilla, consulte el [inicio rápido del demonio de .NET Core](quickstart-v2-netcore-daemon.md).
