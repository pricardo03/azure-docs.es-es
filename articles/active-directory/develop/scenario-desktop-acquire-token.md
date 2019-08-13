---
title: 'Aplicación de escritorio que llama a las API web: adquisición de un token para la aplicación - Plataforma de identidad de Microsoft'
description: Obtener información sobre cómo compilar una aplicación de escritorio que llama a otras API web (para adquirir un token para la aplicación)
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e952b011eb760ebc9dcf5fe7250cf56ec67465f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562339"
---
# <a name="desktop-app-that-calls-web-apis---acquire-a-token"></a>Aplicación de escritorio que llama a las API web: adquisición de un token

Una vez que ha compilado `IPublicClientApplication`, la usará para adquirir un token que luego usará para llamar a una API web.

## <a name="recommended-pattern"></a>Patrón recomendado

La API web se define mediante sus `scopes`. Sea cual sea la experiencia que proporciona en la aplicación, el patrón que querría usar es el siguiente:

- Intentar sistemáticamente obtener un token de la caché de tokens mediante una llamada a `AcquireTokenSilent`
- Si se produce un error en esta llamada, use el flujo `AcquireToken` que quiera utilizar (representado aquí por `AcquireTokenXX`).

```CSharp
AuthenticationResult result;
var accounts = await app.GetAccountsAsync();
IAccount account = ChooseAccount(accounts); // for instance accounts.FirstOrDefault
                                            // if the app manages is at most one account  
try
{
 result = await app.AcquireTokenSilent(scopes, account)
                   .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
  result = await app.AcquireTokenXX(scopes, account)
                    .WithOptionalParameterXXX(parameter)
                    .ExecuteAsync();
}
```

A continuación se exponen los detalles de las distintas formas de adquirir tokens en una aplicación de escritorio.

## <a name="acquiring-a-token-interactively"></a>Adquisición de un token de forma interactiva

El ejemplo siguiente muestra un código mínimo para obtener un token de forma interactiva para leer el perfil del usuario con Microsoft Graph.

```CSharp
string[] scopes = new string[] {"user.read"};
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="mandatory-parameters"></a>Parámetros obligatorios

`AcquireTokenInteractive` solo tiene un parámetro obligatorio ``scopes``, que contiene una enumeración de cadenas que definen los ámbitos para los que se requiere un token. Si el token es para Microsoft Graph, los ámbitos necesarios se pueden encontrar en la sección denominada "Permisos" en la referencia de API de cada API de Microsoft Graph. Por ejemplo, para [enumerar los contactos del usuario](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts), necesitará usar el ámbito "User.Read", "Contacts.Read". Vea también [Referencia de permisos de Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

En Android, también deberá especificar la actividad primaria (mediante `.WithParentActivityOrWindow`, vea más abajo) para que el token vuelva a esa actividad principal tras la interacción. Si no se especifica, se producirá una excepción al llamar a `.ExecuteAsync()`.

### <a name="specific-optional-parameters"></a>Parámetros opcionales específicos

#### <a name="withparentactivityorwindow"></a>WithParentActivityOrWindow

Al ser interactiva, la interfaz de usuario es importante. `AcquireTokenInteractive` tiene un parámetro opcional específico que permite especificar, para las plataformas que lo admiten, el elemento primario de la interfaz de usuario. Cuando se utiliza en una aplicación de escritorio, `.WithParentActivityOrWindow` tiene un tipo diferente en función de la plataforma:

```CSharp
// net45
WithParentActivityOrWindow(IntPtr windowPtr)
WithParentActivityOrWindow(IWin32Window window)

// Mac
WithParentActivityOrWindow(NSWindow window)

// .Net Standard (this will be on all platforms at runtime, but only on NetStandard at build time)
WithParentActivityOrWindow(object parent).
```

Comentarios:

- En .NET Standard, el valor `object` esperado es `Activity` en Android, `UIViewController` en iOS, `NSWindow` en MAC y `IWin32Window` o `IntPr` en Windows.
- En Windows, debe llamar a `AcquireTokenInteractive` desde el subproceso de interfaz de usuario para que el explorador integrado obtenga el contexto de sincronización de la interfaz de usuario adecuado.  No llamar desde el subproceso de interfaz de usuario podría provocar que los mensajes no se suministren correctamente o escenarios de interbloqueo con la interfaz de usuario. Una manera de llamar a MSAL desde el subproceso de interfaz de usuario si ya no está en el subproceso de interfaz de usuario es utilizar `Dispatcher` en WPF.
- Si usa WPF, para obtener una ventana de un control WPF puede usar la clase `WindowInteropHelper.Handle`. La llamada entonces procede de un control WPF (`this`):
  
  ```CSharp
  result = await app.AcquireTokenInteractive(scopes)
                    .WithParentActivityOrWindow(new WindowInteropHelper(this).Handle)
                    .ExecuteAsync();
  ```

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()` se usa para controlar la interactividad con el usuario especificando un símbolo del sistema.

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

La clase define las constantes siguientes:

- ``SelectAccount``: forzará el STS para que presente el cuadro de diálogo de selección de cuenta que contiene las cuentas para las que el usuario tiene una sesión. Esta opción es útil cuando los desarrolladores de aplicaciones quieren permitir que el usuario elija entre diferentes identidades. Esta opción dirige a MSAL para que envíe ``prompt=select_account`` al proveedor de identidades. Esta opción es el valor predeterminado y funciona bien para proporcionar la mejor experiencia posible en función de la información disponible (cuenta, presencia de una sesión para el usuario, etc. ). No la cambie a menos que tenga buena razón para hacerlo.
- ``Consent``: permite que el desarrollador de la aplicación obligue al usuario a que se le pida consentimiento incluso si ya lo otorgó anteriormente. En este caso, MSAL envía `prompt=consent` al proveedor de identidades. Esta opción se puede usar en algunas aplicaciones centradas en la seguridad donde la gobernanza de la organización exige que se presente al usuario el cuadro de diálogo de consentimiento cada vez que se utiliza la aplicación.
- ``ForceLogin``: permite que el desarrollador de la aplicación haga que el servicio solicite al usuario las credenciales incluso si esta petición al usuario no es necesaria. Esta opción puede ser útil si se produce un error al adquirir un token, para permitir que el usuario vuelva a iniciar sesión. En este caso, MSAL envía `prompt=login` al proveedor de identidades. De nuevo, se ha utilizado en algunas aplicaciones centradas en la seguridad donde la gobernanza de la organización exige que el usuario vuelva a iniciar sesión cada vez que accede a determinadas partes de una aplicación.
- ``Never`` (solo para .NET 4.5 y WinRT) no preguntará al usuario, pero intentará usar en su lugar la cookie almacenada en la vista web incrustada oculta (vea a continuación: Vistas web en MSAL.NET). Esta opción puede producir un error, en cuyo caso `AcquireTokenInteractive` generará una excepción para notificar que es necesaria una interacción de la interfaz de usuario y deberá utilizar otro parámetro `Prompt`.
- ``NoPrompt``: No enviará ninguna solicitud al proveedor de identidades. Esta opción solo es útil para las directivas de perfil de edición de Azure AD B2C (consulte las [características específicas de B2C](https://aka.ms/msal-net-b2c-specificities)).

#### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Este modificador se usa en un escenario avanzado donde quiere que el usuario dé previamente su consentimiento a varios recursos por adelantado (y no quiere utilizar el consentimiento incremental, que normalmente se utiliza con MSAL.NET / la plataforma de identidad de Microsoft). Para obtener más detalles, vea [Cómo tener el consentimiento del usuario por adelantado para varios recursos](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources).

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="withcustomwebui"></a>WithCustomWebUi

##### <a name="withcustomwebui-is-an-extensibility-point"></a>WithCustomWebUi es un punto de extensibilidad.

`WithCustomWebUi` es un punto de extensibilidad que le permite proporcionar su propia interfaz de usuario en aplicaciones cliente públicas, y permitir que el usuario vaya a través del punto de conexión /Authorize del proveedor de identidades y que inicie sesión y dé su consentimiento. Después, MSAL.NET puede canjear el código de autenticación y obtener un token. Por ejemplo, se usa en Visual Studio para hacer que las aplicaciones de Electron (por ejemplo los comentarios de VS) proporcionen interacción web, pero dejar que MSAL.NET realice la mayoría del trabajo. También puede usarlo si quiere proporcionar automatización de la interfaz de usuario. En las aplicaciones cliente públicas, MSAL.NET utiliza la norma PKCE ([7636 RFC: clave de prueba para el intercambio de código por clientes públicos de OAuth](https://tools.ietf.org/html/rfc7636)) para asegurarse de que se respeta la seguridad: Solo MSAL.NET puede canjear el código.

  ```CSharp
  using Microsoft.Identity.Client.Extensions;
  ```

##### <a name="how-to-use-withcustomwebui"></a>Cómo usar WithCustomWebUi

Para poder usar `.WithCustomWebUI`, deberá:
  
  1. Implementar la interfaz `ICustomWebUi` (vea [este artículo](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/src/Microsoft.Identity.Client/Extensibility/ICustomWebUI.cs#L32-L70)). Básicamente, deberá implementar un método `AcquireAuthorizationCodeAsync` que acepte la dirección URL del código de autorización (que se calcula mediante MSAL.NET), lo que permite que el usuario vaya a través de la interacción con el proveedor de identidades y, después, devuelva la dirección URL por la que el proveedor de identidades habría vuelvo a llamar a la implementación (incluido el código de autorización). Si tiene problemas, la implementación debería lanzar una excepción `MsalExtensionException` para cooperar bien con MSAL.
  2. En su llamada a `AcquireTokenInteractive`, puede usar el modificador `.WithCustomUI()` pasando la instancia de su interfaz de usuario web personalizada.

     ```CSharp
     result = await app.AcquireTokenInteractive(scopes)
                       .WithCustomWebUi(yourCustomWebUI)
                       .ExecuteAsync();
     ```

##### <a name="examples-of-implementation-of-icustomwebui-in-test-automation---seleniumwebui"></a>Ejemplos de implementación de ICustomWebUi en automatización de prueba: SeleniumWebUI

El equipo de MSAL.NET ha vuelto a escribir nuestras pruebas de interfaz de usuario para aprovechar este mecanismo de extensibilidad. Si le interesa, puede echar un vistazo a la clase [SeleniumWebUI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/053a98d16596be7e9ca1ab916924e5736e341fe8/tests/Microsoft.Identity.Test.Integration/Infrastructure/SeleniumWebUI.cs#L15-L160) en el código fuente MSAL.NET

#### <a name="other-optional-parameters"></a>Otros parámetros opcionales

Obtenga más información sobre los demás parámetros opcionales para `AcquireTokenInteractive` en la documentación de referencia para [AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods).

## <a name="integrated-windows-authentication"></a>Autenticación integrada de Windows

Si quiere iniciar la sesión de un usuario de dominio en una máquina unida a un dominio o a Azure AD, deberá usar:

```csharp
AcquireTokenByIntegratedWindowsAuth(IEnumerable<string> scopes)
```

### <a name="constraints"></a>Restricciones

- AcquireTokenByIntegratedWindowsAuth (IWA) solo es útil para usuarios **federados**, es decir, los usuarios creados en Active Directory y respaldados por Azure Active Directory. Los usuarios creados directamente en AAD, sin el respaldo de AD (**usuarios administrados**) no pueden usar este flujo de autenticación. Esta limitación no afecta al flujo de usuario y contraseña.
- IWA es para las aplicaciones escritas para las plataformas .NET Framework, .NET Core y UWP.
- IWA no omite la MFA (autenticación multifactor). Si se ha configurado MFA, IWA podría producir un error si es necesario un desafío de MFA, porque MFA necesita interacción del usuario.
  > [!NOTE]
  > Esto es complicado. IWA no es interactiva, pero MFA requiere interactividad del usuario. El usuario no controla cuándo el proveedor de identidades solicita que se realice MFA, el administrador de inquilinos sí. Por lo que se ha observado, MFA se requiere al iniciar sesión desde un país o región diferente, cuando no está conectado a través de una VPN a una red corporativa y, a veces, incluso cuando se conecta a través de una VPN. No espere un conjunto de reglas determinista, Azure Active Directory usa la inteligencia artificial para aprender continuamente si se requiere MFA. Si se produce un error de IWA, debería recurrir a una solicitud de usuario (autenticación interactiva o flujo de código de dispositivo).

- La autoridad que se pasa en `PublicClientApplicationBuilder` debe ser:
  - Con inquilino (del tipo `https://login.microsoftonline.com/{tenant}/` donde `tenant` es el GUID que representa el identificador del inquilino o un dominio asociado con el inquilino).
  - Para las cuentas profesionales y educativas (`https://login.microsoftonline.com/organizations/`).

  > No se admiten las cuentas de Microsoft personales (no se pueden usar los inquilinos /common ni /consumers).

- Debido a que la autenticación integrada de Windows es un flujo silencioso:
  - el usuario de la aplicación debe haber consentido anteriormente para usar la aplicación,
  - o bien el administrador de inquilinos debe haber consentido anteriormente a todos los usuarios en el inquilino para que usen la aplicación.
  - En otras palabras:
    - ya sea que, como desarrollador, ha hecho clic el botón **Conceder** en Azure Portal usted mismo;
    - o que un administrador de inquilinos hizo clic en el botón **Conceder o revocar consentimiento del administrador para {dominio del inquilino}** en la pestaña **Permisos de API** del registro para la aplicación. (Vea [Adición de credenciales a la aplicación web](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-web-apis));
    - o se ha proporcionado una manera para que los usuarios den su consentimiento a la aplicación. (Vea [Solicitud de consentimiento de usuario individual](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-individual-user-consent));
    - o bien se ha proporcionado una forma para que el administrador de inquilinos dé su consentimiento para la aplicación. (Vea [Consentimiento de administrador](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)).

- Este flujo está habilitado para el escritorio. NET, .NET Core y aplicaciones de la Plataforma universal de Windows (UWP). En .NET Core solo está disponible la sobrecarga al tomar el nombre de usuario, ya que la plataforma .NET Core no puede pedir el nombre de usuario al sistema operativo.
  
Para más información sobre el consentimiento, consulte [Permisos y consentimientos de la Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent).

### <a name="how-to-use-it"></a>Cómo usarlo

Normalmente solo necesita un parámetro (`scopes`). Pero dependiendo de cómo haya configurado las directivas el administrador de Windows, es posible que no se permita que las aplicaciones en el equipo de Windows busquen el usuario que ha iniciado sesión. En ese caso, utilice un segundo método `.WithUsername()` y pase el nombre de usuario del usuario que ha iniciado sesión como un formato UPN: `joe@contoso.com`.

El ejemplo siguiente presenta el caso más reciente, con explicaciones de la clase de excepciones que se pueden obtener y sus mitigaciones:

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
      .ExecuteAsync();
 }
 else
 {
  try
  {
   result = await app.AcquireTokenByIntegratedWindowsAuth(scopes)
      .ExecuteAsync(CancellationToken.None);
  }
  catch (MsalUiRequiredException ex)
  {
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'.Send an interactive authorization request for this user and resource.

   // you need to get user consent first. This can be done, if you are not using .NET Core (which does not have any Web UI)
   // by doing (once only) an AcquireToken interactive.

   // If you are using .NET core or don't want to do an AcquireTokenInteractive, you might want to suggest the user to navigate
   // to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read

   // AADSTS50079: The user is required to use multi-factor authentication.
   // There is no mitigation - if MFA is configured for your tenant and AAD decides to enforce it,
   // you need to fallback to an interactive flows such as AcquireTokenInteractive or AcquireTokenByDeviceCode
   }
   catch (MsalServiceException ex)
   {
    // Kind of errors you could have (in ex.Message)

    // MsalServiceException: AADSTS90010: The grant type is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.
    // you used common.
    // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted or otherwise organizations

    // MsalServiceException: AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
    // Explanation: this can happen if your application was not registered as a public client application in Azure AD
    // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   }
   catch (MsalClientException ex)
   {
      // Error Code: unknown_user Message: Could not identify logged in user
      // Explanation: the library was unable to query the current Windows logged-in user or this user is not AD or AAD
      // joined (work-place joined users are not supported).

      // Mitigation 1: on UWP, check that the application has the following capabilities: Enterprise Authentication,
      // Private Networks (Client and Server), User Account Information

      // Mitigation 2: Implement your own logic to fetch the username (e.g. john@contoso.com) and use the
      // AcquireTokenByIntegratedWindowsAuth form that takes in the username

      // Error Code: integrated_windows_auth_not_supported_managed_user
      // Explanation: This method relies on an a protocol exposed by Active Directory (AD). If a user was created in Azure
      // Active Directory without AD backing ("managed" user), this method will fail. Users created in AD and backed by
      // AAD ("federated" users) can benefit from this non-interactive method of authentication.
      // Mitigation: Use interactive authentication
   }
 }


 Console.WriteLine(result.Account.Username);
}
```

Para obtener la lista de posibles modificadores en AcquireTokenByIntegratedWindowsAuthentication, vea [AcquireTokenByIntegratedWindowsAuthParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyintegratedwindowsauthparameterbuilder?view=azure-dotnet-preview#methods).

## <a name="username--password"></a>Nombre de usuario y contraseña

También puede adquirir un token proporcionando el nombre de usuario y la contraseña. Este flujo es limitado y no se recomienda, pero hay casos de uso donde es necesario.

### <a name="this-flow-isnt-recommended"></a>Este flujo no es recomendable.

**No se recomienda** este flujo porque no es seguro que la aplicación le pida al usuario la contraseña. Para obtener más información sobre este problema, consulte [este artículo](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). El flujo preferido para adquirir un token de forma silenciosa en equipos unidos a un dominio de Windows es la [autenticación integrada de Windows](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Integrated-Windows-Authentication). En caso contrario, también puede usar el [flujo de código de dispositivo](https://aka.ms/msal-net-device-code-flow).

> [!NOTE] 
> Aunque esto es útil en algunos casos (escenarios de DevOps), si quiere usar el nombre de usuario y contraseña en escenarios interactivos donde proporciona su propia interfaz de usuario, debería pensar cómo evitarlo. A usar el nombre de usuario y contraseña se están descuidando varias cosas:
>
> - Inquilinos principales de identidad moderna: se busca y se reproduce la contraseña. Dado que tenemos este concepto de secreto compartido que se puede interceptar,
> es incompatible con el inicio de sesión sin contraseña.
> - Los usuarios que necesitan realizar MFA no podrán iniciar sesión (ya que no hay ninguna interacción).
> - Los usuarios no podrán realizar el inicio de sesión único.

### <a name="constraints"></a>Restricciones

También se aplican las restricciones siguientes:

- El flujo de usuario y contraseña no es compatible con el acceso condicional ni la autenticación multifactor. Como consecuencia, si la aplicación se ejecuta en un inquilino de Azure AD donde el administrador de inquilinos requiere la autenticación multifactor, no se puede usar este flujo. Muchas organizaciones hacen eso.
- Funciona únicamente para las cuentas profesionales y educativas (no las MSA).
- El flujo está disponible en el escritorio de .NET y .NET Core, pero no en UWP.

### <a name="b2c-specifics"></a>Características específicas de B2C

[Más información sobre el uso de ROPC con B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics#resource-owner-password-credentials-ropc-with-b2c).

### <a name="how-to-use-it"></a>Cómo usarlo

`IPublicClientApplication` contiene el método `AcquireTokenByUsernamePassword`.

El ejemplo siguiente presenta un caso simplificado:

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuilder.Create(clientId)
       .WithAuthority(authority)
       .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAsync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password
    securePassword.AppendChar(c);  // keystroke by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                     securePassword)
                      .ExecuteAsync();
  }
  catch(MsalException)
  {
   // See details below
  }
 }
 Console.WriteLine(result.Account.Username);
}
```

El ejemplo siguiente presenta el caso más reciente, con explicaciones de la clase de excepciones que se pueden obtener y sus mitigaciones:

```CSharp
static async Task GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication app;
 app = PublicClientApplicationBuilder.Create(clientId)
                                   .WithAuthority(authority)
                                   .Build();
 var accounts = await app.GetAccountsAsync();

 AuthenticationResult result = null;
 if (accounts.Any())
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
                    .ExecuteAync();
 }
 else
 {
  try
  {
   var securePassword = new SecureString();
   foreach (char c in "dummy")        // you should fetch the password keystroke
    securePassword.AppendChar(c);  // by keystroke

   result = await app.AcquireTokenByUsernamePassword(scopes,
                                                    "joe@contoso.com",
                                                    securePassword)
                    .ExecuteAsync();
  }
  catch (MsalUiRequiredException ex) when (ex.Message.Contains("AADSTS65001"))
  {
   // Here are the kind of error messages you could have, and possible mitigations

   // ------------------------------------------------------------------------
   // MsalUiRequiredException: AADSTS65001: The user or administrator has not consented to use the application
   // with ID '{appId}' named '{appName}'. Send an interactive authorization request for this user and resource.

   // Mitigation: you need to get user consent first. This can be done either statically (through the portal),
   /// or dynamically (but this requires an interaction with Azure AD, which is not possible with
   // the username/password flow)
   // Statically: in the portal by doing the following in the "API permissions" tab of the application registration:
   // 1. Click "Add a permission" and add all the delegated permissions corresponding to the scopes you want (for instance
   // User.Read and User.ReadBasic.All)
   // 2. Click "Grant/revoke admin consent for <tenant>") and click "yes".
   // Dynamically, if you are not using .NET Core (which does not have any Web UI) by
   // calling (once only) AcquireTokenInteractive.
   // remember that Username/password is for public client applications that is desktop/mobile applications.
   // If you are using .NET core or don't want to call AcquireTokenInteractive, you might want to:
   // - use device code flow (See https://aka.ms/msal-net-device-code-flow)
   // - or suggest the user to navigate to a URL to consent: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ErrorCode: invalid_grant
   // SubError: basic_action
   // MsalUiRequiredException: AADSTS50079: The user is required to use multi-factor authentication.
   // The tenant admin for your organization has chosen to oblige users to perform multi-factor authentication.
   // Mitigation: none for this flow
   // Your application cannot use the Username/Password grant.
   // Like in the previous case, you might want to use an interactive flow (AcquireTokenInteractive()),
   // or Device Code Flow instead.
   // Note this is one of the reason why using username/password is not recommended;
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // Message = "AADSTS70002: Error validating credentials.
   // AADSTS50126: Invalid username or password
   // In the case of a managed user (user from an Azure AD tenant opposed to a
   // federated user, which would be owned
   // in another IdP through ADFS), the user has entered the wrong password
   // Mitigation: ask the user to re-enter the password
   // ------------------------------------------------------------------------

   // ------------------------------------------------------------------------
   // ex.ErrorCode: invalid_grant
   // subError: null
   // MsalServiceException: ADSTS50034: To sign into this application the account must be added to
   // the {domainName} directory.
   // or The user account does not exist in the {domainName} directory. To sign into this application,
   // the account must be added to the directory.
   // The user was not found in the directory
   // Explanation: wrong username
   // Mitigation: ask the user to re-enter the username.
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_request")
  {
   // ------------------------------------------------------------------------
   // AADSTS90010: The grant type is not supported over the /common or /consumers endpoints.
   // Please use the /organizations or tenant-specific endpoint.
   // you used common.
   // Mitigation: as explained in the message from Azure AD, the authority you use in the application needs
   // to be tenanted or otherwise "organizations". change the
   // "Tenant": property in the appsettings.json to be a GUID (tenant Id), or domain name (contoso.com)
   // if such a domain is registered with your tenant
   // or "organizations", if you want this application to sign-in users in any Work and School accounts.
   // ------------------------------------------------------------------------

  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "unauthorized_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS700016: Application with identifier '{clientId}' was not found in the directory '{domain}'.
   // This can happen if the application has not been installed by the administrator of the tenant or consented
   // to by any user in the tenant.
   // You may have sent your authentication request to the wrong tenant
   // Cause: The clientId in the appsettings.json might be wrong
   // Mitigation: check the clientId and the app registration
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException ex) when (ex.ErrorCode == "invalid_client")
  {
   // ------------------------------------------------------------------------
   // AADSTS70002: The request body must contain the following parameter: 'client_secret or client_assertion'.
   // Explanation: this can happen if your application was not registered as a public client application in Azure AD
   // Mitigation: in the Azure portal, edit the manifest for your application and set the `allowPublicClient` to `true`
   // ------------------------------------------------------------------------
  }
  catch (MsalServiceException)
  {
   throw;
  }

  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user_type")
  {
   // Message = "Unsupported User Type 'Unknown'. Please see https://aka.ms/msal-net-up"
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "user_realm_discovery_failed")
  {
   // The user is not recognized as a managed user, or a federated user. Azure AD was not
   // able to identify the IdP that needs to process the user. That's for instance the case
   // if you use a phone number
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "unknown_user")
  {
   // the username was probably empty
   // ex.Message = "Could not identify the user logged into the OS. See https://aka.ms/msal-net-iwa for details."
   throw new ArgumentException("U/P: Wrong username", ex);
  }
  catch (MsalClientException ex) when (ex.ErrorCode == "parsing_wstrust_response_failed")
  {
   // ------------------------------------------------------------------------
   // In the case of a Federated user (that is owned by a federated IdP, as opposed to a managed user owned in an Azure AD tenant)
   // ID3242: The security token could not be authenticated or authorized.
   // The user does not exist or has entered the wrong password
   // ------------------------------------------------------------------------
  }
 }

 Console.WriteLine(result.Account.Username);
}
```

Para obtener más información sobre todos los modificadores que se pueden aplicar a `AcquireTokenByUsernamePassword`, vea [AcquireTokenByUsernamePasswordParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokenbyusernamepasswordparameterbuilder?view=azure-dotnet-preview#methods).

## <a name="command-line-tool-without-web-browser"></a>Herramienta de línea de comandos (sin un explorador web)

### <a name="device-code-flow-why-and-how"></a>Flujo de código de dispositivo. ¿Por qué? ¿Y cómo?

Si va a escribir una herramienta de línea de comandos (que no tiene controles web) y no puede o no quiere usar los flujos anteriores, deberá usar `AcquireTokenWithDeviceCode`.

La autenticación interactiva con Azure AD requiere un explorador web (para obtener más detalles, vea [Uso de exploradores web](https://aka.ms/msal-net-uses-web-browser)). Pero para autenticar a usuarios en dispositivos o sistemas operativos que no proporcionan un explorador web, el flujo de código de dispositivo permite al usuario usar otro dispositivo (por ejemplo, otro equipo o un teléfono móvil) para iniciar sesión de forma interactiva. Al usar flujo de código de dispositivo, la aplicación obtiene los tokens a través de un proceso de dos pasos diseñado especialmente para estos dispositivos o sistemas operativos. Entre algunos ejemplos de estas aplicaciones se incluyen aquellas que se ejecutan en IoT o herramientas de línea de comandos (CLI). La idea es la siguiente:

1. Cada vez que se requiera autenticación del usuario, la aplicación proporciona un código y pide al usuario que use otro dispositivo (por ejemplo, un smartphone conectado a Internet) para ir a una dirección URL (por ejemplo, `https://microsoft.com/devicelogin`), donde se le pedirá al usuario que escriba el código. Una vez hecho eso, la página web llevará al usuario por una experiencia de autenticación normal, como peticiones de consentimiento y autenticación multifactor, si es necesario.

2. Tras una autenticación correcta, la aplicación de línea de comandos recibirá los tokens necesarios a través de un canal posterior y los usará para realizar las llamadas a la API web que necesita.

### <a name="code"></a>Código

`IPublicClientApplication` contiene un método denominado `AcquireTokenWithDeviceCode`.

```CSharp
 AcquireTokenWithDeviceCode(IEnumerable<string> scopes,
                            Func<DeviceCodeResult, Task> deviceCodeResultCallback)
```

Este método toma como parámetros:

- Los `scopes` para los que solicitar un token de acceso
- Una devolución de llamada que recibirá el `DeviceCodeResult`

  ![imagen](https://user-images.githubusercontent.com/13203188/56024968-7af1b980-5d11-11e9-84c2-5be2ef306dc5.png)

El código de ejemplo siguiente presenta el caso más reciente, con explicaciones de la clase de excepciones que se pueden obtener y sus mitigaciones:

```CSharp
static async Task<AuthenticationResult> GetATokenForGraph()
{
 string authority = "https://login.microsoftonline.com/contoso.com";
 string[] scopes = new string[] { "user.read" };
 IPublicClientApplication pca = PublicClientApplicationBuilder
      .Create(clientId)
      .WithAuthority(authority)
      .Build();

 AuthenticationResult result = null;
 var accounts = await app.GetAccountsAsync();

 // All AcquireToken* methods store the tokens in the cache, so check the cache first
 try
 {
  result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
       .ExecuteAsync();
 }
 catch (MsalUiRequiredException ex)
 {
  // A MsalUiRequiredException happened on AcquireTokenSilent.
  // This indicates you need to call AcquireTokenInteractive to acquire a token
  System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
 }

 try
 {
  result = await app.AcquireTokenWithDeviceCode(scopes,
      deviceCodeCallback =>
  {
       // This will print the message on the console which tells the user where to go sign-in using
       // a separate browser and the code to enter once they sign in.
       // The AcquireTokenWithDeviceCode() method will poll the server after firing this
       // device code callback to look for the successful login of the user via that browser.
       // This background polling (whose interval and timeout data is also provided as fields in the
       // deviceCodeCallback class) will occur until:
       // * The user has successfully logged in via browser and entered the proper code
       // * The timeout specified by the server for the lifetime of this code (typically ~15 minutes) has been reached
       // * The developing application calls the Cancel() method on a CancellationToken sent into the method.
       //   If this occurs, an OperationCanceledException will be thrown (see catch below for more details).
       Console.WriteLine(deviceCodeResult.Message);
       return Task.FromResult(0);
  }).ExecuteAsync();

  Console.WriteLine(result.Account.Username);
  return result;
 }
 catch (MsalServiceException ex)
 {
  // Kind of errors you could have (in ex.Message)

  // AADSTS50059: No tenant-identifying information found in either the request or implied by any provided credentials.
  // Mitigation: as explained in the message from Azure AD, the authoriy needs to be tenanted. you have probably created
  // your public client application with the following authorities:
  // https://login.microsoftonline.com/common or https://login.microsoftonline.com/organizations

  // AADSTS90133: Device Code flow is not supported under /common or /consumers endpoint.
  // Mitigation: as explained in the message from Azure AD, the authority needs to be tenanted

  // AADSTS90002: Tenant <tenantId or domain you used in the authority> not found. This may happen if there are
  // no active subscriptions for the tenant. Check with your subscription administrator.
  // Mitigation: if you have an active subscription for the tenant this might be that you have a typo in the
  // tenantId (GUID) or tenant domain name.
 }
 catch (OperationCanceledException ex)
 {
  // If you use a CancellationToken, and call the Cancel() method on it, then this may be triggered
  // to indicate that the operation was cancelled.
  // See https://docs.microsoft.com/dotnet/standard/threading/cancellation-in-managed-threads
  // for more detailed information on how C# supports cancellation in managed threads.
 }
 catch (MsalClientException ex)
 {
  // Verification code expired before contacting the server
  // This exception will occur if the user does not manage to sign-in before a time out (15 mins) and the
  // call to `AcquireTokenWithDeviceCode` is not cancelled in between
 }
}
```

## <a name="file-based-token-cache"></a>Caché de tokens basada en archivos

En MSAL.NET, se proporciona una caché de tokens en memoria de forma predeterminada.

### <a name="serialization-is-customizable-in-windows-desktop-apps-and-web-appsweb-apis"></a>La serialización de las aplicaciones de escritorio de Windows y las aplicaciones web o API web es personalizable.

En el caso de .NET Framework y .NET Core, si no hace nada más, la caché de tokens en memoria permanece durante la duración de la aplicación. Para entender por qué no se proporciona la serialización de fábrica, recuerde que las aplicaciones de escritorio/principales de MSAL .NET pueden ser aplicaciones de consola o de Windows (que tendrían acceso al sistema de archivos), **pero también** aplicaciones web o API web. Estas aplicaciones web y API web pueden usar algunos mecanismos de caché específicos, como bases de datos, memorias caché distribuidas, cachés de Redis Cache, etc. Para que haya una aplicación de la caché de tokens de forma constante en el escritorio de .NET o en .NET Core, es preciso personalizar la serialización.

Las clases e interfaces implicadas en la serialización de la caché de tokens son de los tipos siguientes:

- ``ITokenCache``, que define eventos para suscribirse a las solicitudes de serialización de la caché de tokens, así como métodos para serializar o deserializar la memoria caché en diversos formatos (ADAL v3.0, MSAL 2.x y MSAL 3.x = ADAL v5.0).
- ``TokenCacheCallback`` es una devolución de llamada que se pasa a los eventos para que pueda controlar la serialización. Se les llamará con argumentos del tipo ``TokenCacheNotificationArgs``.
- ``TokenCacheNotificationArgs`` sólo proporciona el ``ClientId`` de la aplicación y una referencia al usuario para el que está disponible el token.

  ![imagen](https://user-images.githubusercontent.com/13203188/56027172-d58d1480-5d15-11e9-8ada-c0292f1800b3.png)

> [!IMPORTANT]
> MSAL.NET crea automáticamente las cachés de tokens y le ofrece la caché de `IToken` cuando se llama a las propiedades `UserTokenCache` y `AppTokenCache` de una aplicación. Se supone que no va a implementar la interfaz usted mismo. Su responsabilidad al implementar una serialización de caché de tokens personalizada es:
>
> - Reaccione ante "eventos" `BeforeAccess` y `AfterAccess` (o sus homólogos *asincrónicos*). El delegado de `BeforeAccess` es el responsable de deserializar la memoria caché, mientras que `AfterAccess` es el de serializarla.
> - Una parte de estos eventos almacena o carga blobs, que se pasan a través del argumento del evento al almacenamiento que se desee.

Las estrategias son diferentes en función de si se escribe una serialización de la caché de tokens para un aplicación cliente pública (escritorio) o un aplicación cliente confidencial (aplicación web/API web o aplicación de demonio).

A partir de MSAL V2.x tiene varias opciones, dependiendo de si quiere serializar la memoria caché solo para el formato de MSAL.NET (caché de formato unificado común a MSAL, pero también a todas las plataformas), o si también quiere admitir la serialización de caché de tokens [heredada](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization) de ADAL V3.

La personalización de la serialización de la caché de tokens para compartir el estado de SSO entre ADAL.NET 3.x, ADAL.NET 5.x y MSAL.NET se explica en una parte del ejemplo siguiente: [active-directory-dotnet-v1-to-v2](https://github.com/Azure-Samples/active-directory-dotnet-v1-to-v2).

### <a name="simple-token-cache-serialization-msal-only"></a>Serialización de la caché de tokens simple (solo MSAL)

A continuación encontrará un ejemplo de una implementación sencilla de serialización personalizada de una caché de tokens para aplicaciones de escritorio. En este caso, la caché de tokens de usuario es un archivo que está en la misma carpeta que la aplicación.

Después de compilar la aplicación, para habilitar la serialización llame a ``TokenCacheHelper.EnableSerialization()`` y use la aplicación `UserTokenCache`.

```CSharp
app = PublicClientApplicationBuilder.Create(ClientId)
    .Build();
TokenCacheHelper.EnableSerialization(app.UserTokenCache);
```

Esta clase auxiliar es similar al siguiente fragmento de código:

```CSharp
static class TokenCacheHelper
 {
  public static void EnableSerialization(ITokenCache tokenCache)
  {
   tokenCache.SetBeforeAccess(BeforeAccessNotification);
   tokenCache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// Path to the token cache
  /// </summary>
  public static readonly string CacheFilePath = System.Reflection.Assembly.GetExecutingAssembly().Location + ".msalcache.bin3";

  private static readonly object FileLock = new object();


  private static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeMsalV3(File.Exists(CacheFilePath)
            ? ProtectedData.Unprotect(File.ReadAllBytes(CacheFilePath),
                                      null,
                                      DataProtectionScope.CurrentUser)
            : null);
   }
  }

  private static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     // reflect changesgs in the persistent store
     File.WriteAllBytes(CacheFilePath,
                         ProtectedData.Protect(args.TokenCache.SerializeMsalV3(),
                                                 null,
                                                 DataProtectionScope.CurrentUser)
                         );
    }
   }
  }
 }
```

Hay una versión preliminar de un serializador basado en archivos de la caché de tokens con calidad de producto para aplicaciones cliente públicas (para aplicaciones de escritorio que se ejecutan en Windows, Mac y Linux) disponible en la biblioteca de código abierto [Microsoft.Identity.Client.Extensions.Msal](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Msal). La puede incluir en sus aplicaciones desde el siguiente paquete NuGet: [Microsoft.Identity.Client.Extensions.Msal](https://www.nuget.org/packages/Microsoft.Identity.Client.Extensions.Msal/).

> [!NOTE]
> Declinación de responsabilidades. La biblioteca Microsoft.Identity.Client.Extensions.Msal es una extensión sobre MSAL.NET. Las clases de estas bibliotecas podrían llegar a MSAL.NET en el futuro, tal como están o con cambios importantes.

### <a name="dual-token-cache-serialization-msal-unified-cache--adal-v3"></a>Serialización de la caché de tokens dual (caché unificada de MSAL y ADAL V3)

Si quiere implementar la serialización de la caché de tokens tanto con el formato de caché unificado (común a ADAL.NET 4.x y MSAL.NET 2.x, y a otras MSAL de la misma generación, o anteriores, que se encuentran en la misma plataforma), puede que el código siguiente le ayude:

```CSharp
string appLocation = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location;
string cacheFolder = Path.GetFullPath(appLocation) + @"..\..\..\..");
string adalV3cacheFileName = Path.Combine(cacheFolder, "cacheAdalV3.bin");
string unifiedCacheFileName = Path.Combine(cacheFolder, "unifiedCache.bin");

IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
                                    .Build();
FilesBasedTokenCacheHelper.EnableSerialization(app.UserTokenCache,
                                               unifiedCacheFileName,
                                               adalV3cacheFileName);

```

Esta vez la clase auxiliar es similar al siguiente código:

```CSharp
using System;
using System.IO;
using System.Security.Cryptography;
using Microsoft.Identity.Client;

namespace CommonCacheMsalV3
{
 /// <summary>
 /// Simple persistent cache implementation of the dual cache serialization (ADAL V3 legacy
 /// and unified cache format) for a desktop applications (from MSAL 2.x)
 /// </summary>
 static class FilesBasedTokenCacheHelper
 {
  /// <summary>
  /// Get the user token cache
  /// </summary>
  /// <param name="adalV3CacheFileName">File name where the cache is serialized with the
  /// ADAL V3 token cache format. Can
  /// be <c>null</c> if you don't want to implement the legacy ADAL V3 token cache
  /// serialization in your MSAL 2.x+ application</param>
  /// <param name="unifiedCacheFileName">File name where the cache is serialized
  /// with the Unified cache format, common to
  /// ADAL V4 and MSAL V2 and above, and also across ADAL/MSAL on the same platform.
  ///  Should not be <c>null</c></param>
  /// <returns></returns>
  public static void EnableSerialization(ITokenCache cache, string unifiedCacheFileName, string adalV3CacheFileName)
  {
   UnifiedCacheFileName = unifiedCacheFileName;
   AdalV3CacheFileName = adalV3CacheFileName;

   cache.SetBeforeAccess(BeforeAccessNotification);
   cache.SetAfterAccess(AfterAccessNotification);
  }

  /// <summary>
  /// File path where the token cache is serialized with the unified cache format
  /// (ADAL.NET V4, MSAL.NET V3)
  /// </summary>
  public static string UnifiedCacheFileName { get; private set; }

  /// <summary>
  /// File path where the token cache is serialized with the legacy ADAL V3 format
  /// </summary>
  public static string AdalV3CacheFileName { get; private set; }

  private static readonly object FileLock = new object();

  public static void BeforeAccessNotification(TokenCacheNotificationArgs args)
  {
   lock (FileLock)
   {
    args.TokenCache.DeserializeAdalV3(ReadFromFileIfExists(AdalV3CacheFileName));
    try
    {
     args.TokenCache.DeserializeMsalV3(ReadFromFileIfExists(UnifiedCacheFileName));
    }
    catch(Exception ex)
    {
     // Compatibility with the MSAL v2 cache if you used one
     args.TokenCache.DeserializeMsalV2(ReadFromFileIfExists(UnifiedCacheFileName));
    }
   }
  }

  public static void AfterAccessNotification(TokenCacheNotificationArgs args)
  {
   // if the access operation resulted in a cache update
   if (args.HasStateChanged)
   {
    lock (FileLock)
    {
     WriteToFileIfNotNull(UnifiedCacheFileName, args.TokenCache.SerializeMsalV3());
     if (!string.IsNullOrWhiteSpace(AdalV3CacheFileName))
     {
      WriteToFileIfNotNull(AdalV3CacheFileName, args.TokenCache.SerializeAdalV3());
     }
    }
   }
  }

  /// <summary>
  /// Read the content of a file if it exists
  /// </summary>
  /// <param name="path">File path</param>
  /// <returns>Content of the file (in bytes)</returns>
  private static byte[] ReadFromFileIfExists(string path)
  {
   byte[] protectedBytes = (!string.IsNullOrEmpty(path) && File.Exists(path))
       ? File.ReadAllBytes(path) : null;
   byte[] unprotectedBytes = encrypt ?
       ((protectedBytes != null) ? ProtectedData.Unprotect(protectedBytes, null, DataProtectionScope.CurrentUser) : null)
       : protectedBytes;
   return unprotectedBytes;
  }

  /// <summary>
  /// Writes a blob of bytes to a file. If the blob is <c>null</c>, deletes the file
  /// </summary>
  /// <param name="path">path to the file to write</param>
  /// <param name="blob">Blob of bytes to write</param>
  private static void WriteToFileIfNotNull(string path, byte[] blob)
  {
   if (blob != null)
   {
    byte[] protectedBytes = encrypt
      ? ProtectedData.Protect(blob, null, DataProtectionScope.CurrentUser)
      : blob;
    File.WriteAllBytes(path, protectedBytes);
   }
   else
   {
    File.Delete(path);
   }
  }

  // Change if you want to test with an un-encrypted blob (this is a json format)
  private static bool encrypt = true;
 }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Llamada a una API web desde la aplicación de escritorio](scenario-desktop-call-api.md)
