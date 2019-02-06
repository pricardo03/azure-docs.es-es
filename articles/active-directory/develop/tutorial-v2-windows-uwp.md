---
title: Introducción a UWP de Azure AD v2.0 | Microsoft Docs
description: Cómo pueden llamar las aplicaciones de la Plataforma universal de Windows (UWP) a una API que requiera tokens de acceso mediante el punto de conexión de Azure Active Directory v2.0.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: a0c2d13b2ac6715db047d56e998294688d9b65cf
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097068"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Llamar a Microsoft Graph API desde una aplicación de la Plataforma universal de Windows (XAML)

> [!div renderon="docs"]
> [!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía se explica cómo una aplicación de la Plataforma universal de Windows (UWP) puede solicitar un token de acceso y, a continuación, llamar a Microsoft Graph API. La guía también se aplica a otras API que requieren tokens de acceso del punto de conexión de Azure Active Directory v2.0.

Al final de esta guía, su aplicación llama a una API protegida mediante cuentas personales. Algunos ejemplos son outlook.com, live.com, etc. Su aplicación también llama a cuentas profesionales y educativas de cualquier empresa y organización que tenga Azure Active Directory.

>[!NOTE]
> En esta guía es necesario instalar el desarrollo de Visual Studio 2017 con Universal Windows Platform. Consulte [Prepárese](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) para obtener instrucciones sobre cómo descargar y configurar Visual Studio para desarrollar las aplicaciones de la Plataforma universal de Windows.

## <a name="how-this-guide-works"></a>Funcionamiento de esta guía

![Gráfico Funcionamiento de esta guía](./media/tutorial-v2-windows-uwp/uwp-intro.png)

La guía crea una aplicación de UWP de ejemplo que consulta una API web o de Microsoft Graph que acepte tokens desde un punto de conexión de Azure Active Directory v2.0. En este escenario, se agrega un token a las solicitudes HTTP a través del encabezado de autorización. La Biblioteca de autenticación de Microsoft (MSAL) administra la adquisición y renovación de los tokens.

## <a name="nuget-packages"></a>Paquetes NuGet

Esta guía utiliza los siguientes paquetes NuGet:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticación de Microsoft|

## <a name="set-up-your-project"></a>Configurar su proyecto

En esta sección se proporcionan instrucciones paso a paso para integrar una aplicación .NET de escritorio de Windows (XAML) con *Iniciar sesión en Microsoft*. A continuación, puede consultar API web que requieren un token, como Microsoft Graph API.

Esta guía crea una aplicación que muestra un botón que consulta Graph API, un botón para cerrar la sesión y cuadros de texto que muestran los resultados de las llamadas.

> [!NOTE]
> ¿Desea descargar este proyecto de Visual Studio de ejemplo en su lugar? [Descargue un proyecto](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) y vaya al paso [Registro de aplicaciones](#register-your-application "Paso de registro de aplicaciones") para configurar el código de ejemplo antes de ejecutarlo.


### <a name="create-your-application"></a>Creación de la aplicación

1. Abra Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.
2. En **Plantillas**, seleccione **Visual C#**.
3. Seleccione **Aplicación vacía (Windows universal)**.
4. Asigne un nombre a la aplicación y seleccione **Aceptar**.
5. Si se le solicita, seleccione cualquier versión de tipo **Destino** y **Mínima** y seleccione **Aceptar**.

    >![Versiones mínima y de destino](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Adición de la biblioteca de autenticación de Microsoft a su proyecto
1. En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del administrador de paquetes**.
2. Copie y pegue el siguiente comando en la ventana de la **consola del administrador de paquetes**:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre -Version 1.1.4-preview0002
    ```

> [!NOTE]
> Este comando instala la [biblioteca de autenticación de Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL adquiere, almacena en memoria caché y actualiza los tokens de usuario que obtienen acceso a las API protegidas por Azure Active Directory v2.0.

> [!NOTE]
> En este tutorial no se usa todavía la versión más reciente de MSAL.NET, pero estamos trabajando para actualizarla.

## <a name="initialize-msal"></a>Inicializar MSAL
Este paso le ayuda a crear una clase para administrar la interacción con MSAL, como la administración de tokens.

1. Abra el archivo **App.xaml.cs** y agregue la referencia de MSAL a la clase:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Agregue las dos líneas siguientes a la clase de la aplicación (dentro del bloque <code>sealed partial class App : Application</code>):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>Creación de la IU de la aplicación

Se crea un archivo **MainPage.xaml** automáticamente como parte de la plantilla de proyecto. Abra este archivo y siga las instrucciones:

* Reemplace el nodo **Grid** de la aplicación por el código siguiente:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Uso de MSAL para obtener un token de Microsoft Graph API

En esta sección se muestra cómo usar MSAL para obtener un token de Microsoft Graph API.

1.  En **MainPage.xaml.cs**, agregue la referencia de MSAL a la clase:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Reemplace el código de su clase <code>MainPage</code> por el código siguiente:

    ```csharp
    public sealed partial class MainPage : Page
    {
        // Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        // Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };
    
        public MainPage()
        {
            this.InitializeComponent();
        }
    
        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;
    
            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
    
                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }
    
            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

### <a name="more-information"></a>Más información

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

La llamada al método `AcquireTokenAsync` genera una ventana que pide al usuario que inicie sesión. Las aplicaciones suelen requerir a los usuarios que inicien sesión de forma interactiva la primera vez que necesitan acceder a un recurso protegido. Es posible que también deban iniciar sesión cuando se produce un error en una operación silenciosa para adquirir un token. Por ejemplo, si ha caducado la contraseña de usuario.

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

El método `AcquireTokenSilentAsync` controla las renovaciones y las adquisiciones de tokens sin la interacción del usuario. Tras ejecutarse `AcquireTokenAsync` por primera vez y pedírsele al usuario sus credenciales, el método `AcquireTokenSilentAsync` debe utilizarse para solicitar tokens para llamadas posteriores, ya que adquiere tokens en silencio. MSAL se encargará de la memoria caché de tokens y la renovación. 

En última instancia, se producirá un error en el método `AcquireTokenSilentAsync`. El error puede deberse a que los usuarios hayan cerrado sesión o cambiado su contraseña en otro dispositivo. Si MSAL detecta que el problema puede solucionarse requiriendo una acción interactiva, desencadena una excepción `MsalUiRequiredException`. La aplicación puede abordar esta excepción de dos maneras:

* Puede realizar una llamada en `AcquireTokenAsync` inmediatamente. Esta llamada provoca que se solicite al usuario que inicie sesión. Este patrón se da normalmente en aplicaciones en línea en las que no hay ningún contenido sin conexión disponible para el usuario. El ejemplo generado por esta instalación guiada sigue este patrón. Lo ve en acción la primera vez que ejecuta el ejemplo. 
    * Dado que ningún usuario ha usado la aplicación, `PublicClientApp.Users.FirstOrDefault()` contendrá un valor NULL y se iniciará una excepción `MsalUiRequiredException`.
    * El código del ejemplo se ocupa entonces de la excepción llamando a `AcquireTokenAsync`. Esta llamada provoca que se solicite al usuario que inicie sesión.

* O, en su lugar, presenta una indicación visual a los usuarios de que se requiere un inicio de sesión interactivo. A continuación, se puede seleccionar el momento adecuado para iniciar sesión. Asimismo, la aplicación puede volver a probar el método `AcquireTokenSilentAsync` más tarde. Este patrón se usa con frecuencia cuando los usuarios pueden utilizar otra funcionalidad de aplicación sin interrupciones. Por ejemplo, si hay contenido sin conexión disponible en la aplicación. En este caso, los usuarios pueden decidir cuándo desean iniciar sesión para acceder al recurso protegido o para actualizar la información obsoleta. De lo contrario, la aplicación puede decidir probar el método `AcquireTokenSilentAsync` de nuevo cuando se restablezca la red después de no haber estado disponible temporalmente.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Llamada a Microsoft Graph API con el token que acaba de obtener

* Agregue el siguiente método nuevo al archivo **MainPage.xaml.cs**. El método se utiliza para realizar una solicitud de `GET` a Graph API con un encabezado [de autorización]:

    ```csharp
    /// <summary>
    /// Perform an HTTP GET request to a URL using an HTTP Authorization header
    /// </summary>
    /// <param name="url">The URL</param>
    /// <param name="token">The token</param>
    /// <returns>String containing the results of the GET operation</returns>
    public async Task<string> GetHttpContentWithToken(string url, string token)
    {
        var httpClient = new System.Net.Http.HttpClient();
        System.Net.Http.HttpResponseMessage response;
        try
        {
            var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
            // Add the token in Authorization header
            request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
            response = await httpClient.SendAsync(request);
            var content = await response.Content.ReadAsStringAsync();
            return content;
        }
        catch (Exception ex)
        {
            return ex.ToString();
        }
    }
    ```

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Más información acerca de cómo realizar una llamada de REST a una API protegida

En esta aplicación de ejemplo, el método `GetHttpContentWithToken` se usa para realizar una solicitud HTTP `GET` a un recurso protegido que requiere un token. A continuación, el método devuelve el contenido al autor de llamada. Este método agrega el token adquirido al encabezado de **autorización HTTP**. En este ejemplo, el recurso es el punto de conexión **me** de Microsoft Graph API, que muestra información del perfil del usuario.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adición de un método para cerrar la sesión del usuario

* Para cerrar la sesión del usuario, agregue el método siguiente a **MainPage.xaml.cs**:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        if (App.PublicClientApp.Users.Any())
        {
            try
            {
                App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
                this.ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                this.SignOutButton.Visibility = Visibility.Collapsed;
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    }
    ```

### <a name="more-information-on-sign-out"></a>Más información sobre el cierre de sesión

El método `SignOutButton_Click` quita al usuario de la memoria caché de usuario MSAL. Este método indica de forma eficaz a MSAL que se olvide del usuario actual. A continuación, una futura solicitud de adquisición de un token solo se realizará correctamente si se crea para ser interactiva.
La aplicación de este ejemplo admite un solo usuario. Sin embargo, MSAL admite situaciones donde se puede iniciar sesión en más de una cuenta al mismo tiempo. Un ejemplo de esto es una aplicación de correo electrónico donde un usuario tiene varias cuentas.

## <a name="display-basic-token-information"></a>Visualización de información de token básica

* Para mostrar información básica sobre el token, agregue el método siguiente al archivo **MainPage.xaml.cs**:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
            TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
            TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Más información

Los id. de token adquiridos a través de **OpenID Connect** también contienen un pequeño subconjunto de información relativa al usuario. `DisplayBasicTokenInfo` muestra información básica incluida en el token. Ejemplos de esto son el nombre para mostrar del usuario y su identificador, así como la fecha de expiración del token y la cadena que representa al propio token de acceso. Si selecciona el botón **Call Microsoft Graph API** (Llamar a la API de Microsoft Graph) varias veces, verá que el mismo token se reutilizó para solicitudes posteriores. También puede ver que la fecha de expiración se amplía si MSAL decide que es el momento de renovar el token.

## <a name="register-your-application"></a>Registrar su aplicación

Ahora tiene que registrar la aplicación en el Portal de registro de aplicaciones de Microsoft:
1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registrar una aplicación.
2. Escriba un nombre para la aplicación.
3. Asegúrese de que *no está seleccionada* la opción de **configuración guiada**.
4. Seleccione **Add Platforms** (Agregar plataformas), seleccione **Native Application** (Aplicación nativa) y, luego, seleccione **Save** (Guardar).
5. Copie el GUID en el **id. de aplicación**, vuelva a Visual Studio, abra **App.xaml.cs** y reemplace `your_client_id_here` con el id. de aplicación que acaba de registrar:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Habilitar la autenticación integrada en dominios federados (opcional)

Para habilitar la autenticación integrada de Windows cuando se utiliza con un dominio federado de Azure Active Directory, el manifiesto de la aplicación debe habilitar funcionalidades adicionales:

1. Haga doble clic en **Package.appxmanifest**.
2. Seleccione la pestaña **Funcionalidades** y asegúrese de que las siguientes opciones de configuración estén habilitadas:

    - Autenticación empresarial
    - Redes privadas (cliente y servidor)
    - Certificados de usuario compartidos

3. Abra **App.xaml.cs** y agregue la siguiente línea en el constructor de aplicaciones:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Autenticación integrada de Windows no está configurada de forma predeterminada para este ejemplo. Las aplicaciones que requieren las funcionalidades *Autenticación empresarial* o *Certificados de usuario compartidos* requieren un mayor nivel de comprobación de Microsoft Store. Además, no todos los desarrolladores desean llevar a cabo un nivel de comprobación más alto. Habilite esta opción solo si necesita usar la autenticación integrada de Windows con un dominio de Azure Active Directory federado.

## <a name="test-your-code"></a>Prueba del código

Para probar la aplicación, seleccione F5 para ejecutar el proyecto en Visual Studio. Aparecerá la ventana principal:

![Interfaz de usuario de la aplicación](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Cuando esté listo para realizar la prueba, seleccione **Llamar a Microsoft Graph API**. A continuación, use una cuenta profesional de Microsoft Azure Active Directory o una cuenta Microsoft, como live.com u outlook.com, para iniciar sesión. Si es la primera vez, verá una ventana que solicita al usuario que inicie sesión:

![Página de inicio de sesión](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>Consentimiento
La primera vez que inicie sesión en la aplicación, verá una pantalla de consentimiento similar a la siguiente. Seleccione **Sí** para consentir el acceso de forma explícita:

![Acceso a pantalla de consentimiento](./media/tutorial-v2-windows-uwp/consentscreen.png)
### <a name="expected-results"></a>Resultados esperados
Verá la información de perfil de usuario que devolvió la llamada de Microsoft Graph API en la pantalla de **resultados de llamadas de la API**:

![Pantalla de resultados de llamada a la API](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

También verá información básica sobre el token adquirido a través de `AcquireTokenAsync` o `AcquireTokenSilentAsync` en el cuadro de **información de token**:

|Propiedad  |Formato  |DESCRIPCIÓN |
|---------|---------|---------|
|**Nombre** |Nombre completo del usuario|Nombre y apellidos del usuario.|
|**Nombre de usuario** |<span>user@domain.com</span> |El nombre de usuario que identifica al usuario.|
|**Expiración del token** |DateTime |La hora a la que expira el token. MSAL amplía la fecha de expiración al renovar el token según sea necesario.|
|**Token de acceso** |string |Cadena de token que se envía a las solicitudes HTTP que requieran un *encabezado de autorización*.|

#### <a name="see-whats-in-the-access-token-optional"></a>Ver el contenido del token de acceso (opcional)
Si quiere, copie el valor en **Token de acceso** y péguelo en https://jwt.ms para descodificarlo y ver la lista de notificaciones.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Más información sobre los ámbitos y permisos delegados

Microsoft Graph API requiere el ámbito *user.read* para leer el perfil del usuario. Este ámbito se agrega automáticamente de forma predeterminada en todas las aplicaciones que se van a registrar en el Portal de registro de aplicaciones. Otras API de Microsoft Graph, así como las API personalizadas para el servidor back-end, pueden requerir ámbitos adicionales. Microsoft Graph API requiere el ámbito *Calendars.read* para mostrar los calendarios del usuario.

Para tener acceso a los calendarios del usuario en el contexto de una aplicación, agregue el permiso delegado *Calendars.Read* a la información del registro de la aplicación. A continuación, agregue el ámbito *Calendars.Read* a la llamada `acquireTokenSilent`.

> [!NOTE]
> Es posible que se pida al usuario algún consentimiento adicional a medida que aumente el número de ámbitos.

## <a name="known-issues"></a>Problemas conocidos

### <a name="issue-1"></a>Problema 1
Puede recibir uno de los siguientes mensajes de error al iniciar sesión en la aplicación en un dominio federado de Azure Active Directory:
 - No se encontró un certificado de cliente válido en la solicitud.
 - No se encontraron certificados válidos en el almacén de certificados del usuario.
 - Vuelva a intentarlo mediante un método de autenticación diferente.

**Causa:** las funcionalidades de Enterprise y los certificados no están habilitados.

**Solución:** siga los pasos en [autenticación integrada en dominios federados](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Problema 2
Puede habilitar la [autenticación integrada en dominios federados](#enable-integrated-authentication-on-federated-domains-optional) e intentar usar Windows Hello en un equipo con Windows 10 para iniciar sesión en un entorno con autenticación multifactor configurada. Se presenta la lista de certificados. Sin embargo, si decide usar su PIN, nunca se presentará Anclar ventana.

**Causa:** este problema es una limitación conocida del agente de autenticación web en aplicaciones de UWP que se ejecutan en el escritorio de Windows 10. Funciona bien en Windows 10 Mobile.

**Solución alternativa**: seleccione **Iniciar sesión con otras opciones**. A continuación, seleccione **Iniciar sesión con un nombre de usuario y contraseña**. Seleccione **Proporcionar la contraseña**. A continuación, pase por el proceso de autenticación telefónica.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
