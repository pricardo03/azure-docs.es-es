---
title: Inicio de sesión por los usuarios y llamada a Microsoft Graph API desde un escritorio de .NET | Microsoft Docs
description: Aprenda a crear una aplicación de escritorio de Windows .NET que se integra con Azure AD para el inicio de sesión y llama a las API protegidas de Azure AD mediante OAuth 2.0.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f9389a7c0e80f075c01f2236fa1bdf9dc9544ac6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987448"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Inicio de sesión por los usuarios y llamada a Microsoft Graph API desde un escritorio de .NET

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Para los clientes nativos de .NET que necesitan acceder a recursos protegidos, Azure Active Directory (Azure AD) proporciona la Biblioteca de autenticación de Active Directory (ADAL). ADAL facilita a cualquier aplicación la obtención de tokens de acceso. 

En esta guía de inicio rápido, aprenderá a compilar una aplicación de lista de tareas pendientes de WPF de .NET con la que podrá:

* Obtener tokens de acceso para llamar a Graph API de Azure AD mediante el protocolo de autenticación OAuth 2.0.
* Buscar un directorio para los usuarios con un alias determinado
* Cerrar la sesión de los usuarios

Para crear la aplicación de trabajo completa, tendrá que:

1. Registrar la aplicación con Azure AD
2. Instalar y configurar ADAL.
3. Usar ADAL para obtener tokens de Azure AD.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, complete estos requisitos previos:

* [Descargue el esqueleto de la aplicación](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) o [el ejemplo completado](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).
* Necesita un inquilino de Azure AD en el que pueda crear usuarios y registrar una aplicación. Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Paso 1: Registro de la aplicación DirectorySearcher

Para habilitar una aplicación para que obtenga tokens, regístrela en un inquilino de Azure AD y concédale permiso de acceso a Graph API de Azure AD:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la barra superior, seleccione su cuenta y, en la lista **Directorio**, elija el inquilino de Active Directory en el que desee registrar la aplicación.
3. Seleccione **Todos los servicios** en el panel de navegación izquierdo y elija **Azure Active Directory**.
4. En **Registros de aplicaciones**, elija **Agregar**.
5. Siga las indicaciones y cree una aplicación cliente **nativa**.
    * El **nombre** de la aplicación servirá de descripción de la aplicación para los usuarios finales.
    * El **URI de redirección** es una combinación de esquema y cadena que utilizará Azure AD para devolver las respuestas de los tokens. Escriba un valor específico para la aplicación, por ejemplo, `http://DirectorySearcher`.

6. Una vez que haya completado el registro, AAD asignará a su aplicación un identificador de aplicación único. Necesitará este valor en las secciones siguientes, así que cópielo de la página de la aplicación.
7. En la página **Configuración**, elija **Permisos necesarios** y luego **Agregar**. Seleccione **Microsoft Graph** como API y en **Permisos delegados**, agregue el permiso **Leer datos de directorio**. El establecimiento de este permiso permitirá a la aplicación consultar Graph API para los usuarios.

## <a name="step-2-install-and-configure-adal"></a>Paso 2: Instalación y configuración de ADAL

Ahora que tiene una aplicación en Azure AD, puede instalar ADAL y escribir el código relacionado con la identidad. Para que ADAL se comunique con Azure AD, hay que proporcionarle información sobre el registro de la aplicación.

1. Para empezar, agregue ADAL al proyecto `DirectorySearcher` mediante la Consola del Administrador de paquetes.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1. En el proyecto `DirectorySearcher`, abra `app.config`.
1. Reemplace los valores de los elementos de la sección `<appSettings>` para que reflejen los valores especificados en Azure Portal. El código hará referencia a estos valores cada vez que use ADAL.
  * `ida:Tenant` es el dominio del inquilino de Azure AD, por ejemplo, contoso.onmicrosoft.com.
  * `ida:ClientId` es el identificador de cliente de la aplicación que copió del portal.
  * `ida:RedirectUri` es la dirección URL de redirección que ha registrado en el portal.

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Paso 3: Uso de ADAL para obtener tokens de Azure AD

El principio básico detrás ADAL es que cada vez que una aplicación necesita un token de acceso, no tiene más que llamar a `authContext.AcquireTokenAsync(...)` y ADAL se encarga del resto.

1. En el proyecto `DirectorySearcher`, abra `MainWindow.xaml.cs`.
1. Busque el método `MainWindow()`. 
1. Inicialice `AuthenticationContext` de la aplicación (la clase principal de ADAL). `AuthenticationContext` es el lugar en el que se pasan a ADAL las coordenadas necesarias para comunicarse con Azure AD e indicarle cómo almacenar en caché los tokens.

    ```csharp
    public MainWindow()
    {
        InitializeComponent();

        authContext = new AuthenticationContext(authority, new FileCache());

        CheckForCachedToken();
    }
    ```

1. Busque el método `Search(...)`, al que se llama cuando el usuario selecciona el botón **Buscar** en la interfaz de usuario de la aplicación. Este método realiza una solicitud GET a Graph API de Azure AD para realizar una consulta sobre los usuarios cuyo UPN comienza con el término de búsqueda especificado.
1. Para realizar una consulta a Graph API, incluya access_token en el encabezado `Authorization` de la solicitud, que es donde entra ADAL.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        // Validate the Input String
        if (string.IsNullOrEmpty(SearchText.Text))
        {
            MessageBox.Show("Please enter a value for the To Do item name");
            return;
        }

        // Get an Access Token for the Graph API
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
            UserNameLabel.Content = result.UserInfo.DisplayableId;
            SignOutButton.Visibility = Visibility.Visible;
        }
        catch (AdalException ex)
        {
            // An unexpected error occurred, or user canceled the sign in.
            if (ex.ErrorCode != "access_denied")
                MessageBox.Show(ex.Message);

            return;
        }

        ...
    }
    ```

    Cuando la aplicación solicita un token mediante una llamada a `AcquireTokenAsync(...)`, ADAL intentará devolver un token sin solicitar al usuario las credenciales.
    * Si ADAL determina que el usuario debe iniciar sesión para obtener un token, mostrará un cuadro de diálogo de inicio de sesión, recopilará las credenciales del usuario y devolverá un token tras una autenticación correcta. 
    * Si ADAL no puede devolver un token por alguna razón, mostrará una `AdalException`.

1. Observe que el objeto `AuthenticationResult` contiene un objeto `UserInfo` que puede usarse para recopilar información puede necesitar la aplicación. En DirectorySearcher, `UserInfo` se usa para personalizar la interfaz de usuario de la aplicación con el identificador del usuario.
1. Cuando el usuario selecciona el botón **Cerrar sesión**, asegúrese de que la próxima llamada a `AcquireTokenAsync(...)` le solicitará que inicie sesión. Eso es algo que se puede hacer fácilmente con ADAL, solo hay que borrar la caché del token:

    ```csharp
    private void SignOut(object sender = null, RoutedEventArgs args = null)
    {
        // Clear the token cache
        authContext.TokenCache.Clear();

        ...
    }
    ```

    Si el usuario no hace clic en el botón **Cerrar sesión**, debe mantener la sesión del usuario para la próxima vez que ejecute DirectorySearcher. Cuando se inicie la aplicación, puede comprobar la caché de tokens de ADAL para un token existente y actualizar la interfaz de usuario en consecuencia.

1. En el método `CheckForCachedToken()`, realice otra llamada a `AcquireTokenAsync(...)`, pero esta vez pasando en el parámetro `PromptBehavior.Never`. `PromptBehavior.Never` indicará a ADAL que no se debe solicitar al usuario que inicie sesión. En lugar de ello, ADAL debe iniciar una excepción si no puede devolver un token.

    ```csharp
    public async void CheckForCachedToken() 
    {
        // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "user_interaction_required")
            {
                // An unexpected error occurred.
                MessageBox.Show(ex.Message);
            }

            // If user interaction is required, proceed to main page without singing the user in.
            return;
        }

        // A valid token is in the cache
        SignOutButton.Visibility = Visibility.Visible;
        UserNameLabel.Content = result.UserInfo.DisplayableId;
    }
    ```

Felicidades. Ahora tiene una aplicación de WPF de .NET operativa que puede autenticar usuarios, realizar llamadas seguras a las API web mediante OAuth 2.0 y obtener información básica sobre el usuario. Si todavía no lo ha hecho, ahora es el momento de completar el inquilino con algunos usuarios. Ejecute la aplicación DirectorySearcher e inicie sesión con uno de esos usuarios. Busque otros usuarios según su UPN. Cierre la aplicación y vuelva a ejecutarla. Observe que la sesión del usuario permanece intacta. Cierre la sesión y vuelva a iniciarla como otro usuario.

ADAL facilita la incorporación de estas características de identidad comunes a la aplicación. Se encarga del trabajo duro, lo que incluye la administración de la caché, compatibilidad con el protocolo OAuth, presentación del usuario con una interfaz de usuario de inicio de sesión, actualización de los tokens caducados, etc. Todo lo que necesita saber es una única llamada de API, `authContext.AcquireTokenAsync(...)`.

Como referencia, consulte el ejemplo completado (sin sus valores de configuración) [en GitHub](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a proteger una API web mediante tokens de acceso de portador de OAuth 2.0.
> [!div class="nextstepaction"]
> [Protección de una API Web .NET con Azure AD &gt;&gt;](quickstart-v1-dotnet-webapi.md)
