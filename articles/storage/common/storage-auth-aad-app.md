---
title: Autenticar con Azure Active Directory para acceder a los datos de blob y cola desde la aplicación cliente
description: Use Azure Active Directory para autenticar desde dentro de una aplicación cliente y adquirir un token de OAuth 2.0 para autorizar las solicitudes a Azure Blob storage y Queue storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/05/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e45fe20e93d81c1cfd1f868b40f76743558758bb
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754949"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>Autenticar con Azure Active Directory desde una aplicación para el acceso a los blobs y colas

Una ventaja clave del uso de Azure Active Directory (Azure AD) con Azure Blob storage o almacenamiento de colas es que ya no necesitan las credenciales que se almacenará en el código. En su lugar, puede solicitar un token de acceso de OAuth 2.0 desde la plataforma de identidad de Microsoft (anteriormente Azure AD). Azure AD autentica la entidad de seguridad (usuario, grupo o entidad de servicio) que ejecuta la aplicación. Si la autenticación se realiza correctamente, Azure AD devuelve el token de acceso a la aplicación y la aplicación, a continuación, puede usar el token de acceso para autorizar las solicitudes a Azure Blob storage o almacenamiento en cola.

En este artículo se muestra cómo configurar su aplicación nativa o aplicación web para la autenticación con Azure AD. El ejemplo de código incluye .NET, pero otros lenguajes utilizan un enfoque similar.

Para información general sobre el flujo de concesión de código OAuth 2.0, consulte [Autorización del acceso a aplicaciones web de Azure Active Directory mediante el flujo de concesión de código OAuth 2.0](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Asignar un rol RBAC a una entidad de seguridad de Azure AD

Para autenticar a una entidad de seguridad de la aplicación de Azure Storage, primero configure los valores del control de acceso basado en roles (RBAC) para esa entidad de seguridad. Almacenamiento de Azure define los roles RBAC integrados que abarcan los permisos para los contenedores y las colas. Cuando el rol RBAC se asigna a una entidad de seguridad, esta recibe acceso a ese recurso. Para obtener más información, consulte [administrar derechos de acceso a datos de Azure Blob y cola con RBAC](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registro de la aplicación con un inquilino de Azure AD

El primer paso para usar Azure AD para autorizar el acceso a los recursos de almacenamiento es registrar la aplicación cliente con un inquilino de Azure AD desde el [portal Azure](https://portal.azure.com). Al registrar la aplicación cliente, proporcionar información acerca de la aplicación a Azure AD. Azure AD proporciona un identificador de cliente (también denominado *identificador de aplicación*) que se utiliza para asociar la aplicación con Azure AD en runtime. Para más información sobre el identificador de cliente, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md).

Para registrar la aplicación de Azure Storage, siga los pasos mostrados en [inicio rápido: Registrar una aplicación con la plataforma Microsoft identity](../../active-directory/develop/quickstart-configure-app-access-web-apis.md). La siguiente imagen muestra la configuración común para registrar una aplicación web:

![Captura de pantalla que muestra cómo registrar la aplicación de almacenamiento con Azure AD](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Si registra la aplicación como una aplicación nativa, puede especificar cualquier URI válido para el **URI de redirección**. Para aplicaciones nativas, este valor no tiene que ser una dirección URL real. Para aplicaciones web, el URI de redirección debe ser un URI válido, ya que especifica la dirección URL a la que se proporcionan los tokens.

Una vez que ha registrado su aplicación, verá el identificador de la aplicación (o identificador de cliente) en **Configuración**:

![Captura de pantalla muestra el identificador de cliente](./media/storage-auth-aad-app/app-registration-client-id.png)

Para más información sobre cómo registrar una aplicación con Azure AD, consulte [Integración de aplicaciones con Azure Active Directory](../../active-directory/develop/quickstart-v2-register-an-app.md).

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Concesión de permisos a la aplicación registrada para Azure Storage

A continuación, conceda los permisos de aplicación para llamar a las API de almacenamiento de Azure. Este paso permite que la aplicación autorizar las solicitudes a Azure Storage con Azure AD.

1. En el **Introducción** página de la aplicación registrada, seleccione **permisos de API de vista**.
1. En el **permisos de API** sección, seleccione **agregar un permiso** y elija **API que usa mi organización**.
1. En el **API que usa mi organización** sección, busque "Azure Storage" y seleccione **Azure Storage** en la lista de resultados para mostrar el **permisos de solicitud API** panel.

    ![Captura de pantalla que muestra los permisos para el almacenamiento](media/storage-auth-aad-app/registered-app-permissions-1.png)

1. En **qué tipo de permisos requiere la aplicación?** , tenga en cuenta que es el tipo de permiso disponibles **permisos delegados**. Esta opción se selecciona de forma predeterminada.
1. En el **seleccionar permisos** sección de la **permisos de solicitud de API** panel, active la casilla de verificación junto a **user_impersonation**, a continuación, haga clic en **agregar permisos**.
1. El **permisos de API** panel ahora se muestra que la aplicación de Azure AD tiene acceso a Microsoft Graph y el almacenamiento de Azure. Los permisos se conceden automáticamente a Microsoft Graph al registrar primero la aplicación con Azure AD.

    ![Captura de pantalla muestra registrar permisos de aplicación](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="libraries-for-token-acquisition"></a>Bibliotecas de adquisición de tokens

Una vez que haya registrado la aplicación y lo concedió permisos de acceso a datos en Azure Blob storage o almacenamiento en cola, puede agregar código a la aplicación para autenticar a una entidad de seguridad y adquirir un token de OAuth 2.0. Para autenticar y adquirir el token, puede usar uno de los [bibliotecas de autenticación de Microsoft identity platform](../../active-directory/develop/reference-v2-libraries.md) u otra biblioteca de código abierto que admite OpenID Connect 1.0. La aplicación, a continuación, puede usar el token de acceso para autorizar una solicitud en cola o el almacenamiento de blobs de Azure.

Para obtener una lista de escenarios para el que se admite la adquisición de tokens, consulte el [escenarios](https://aka.ms/msal-net-scenarios) sección de la [biblioteca de autenticación de Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) repositorio de GitHub.

## <a name="net-code-example-create-a-block-blob"></a>Ejemplo de código .NET: Creación de un blob en bloques

El ejemplo de código muestra cómo obtener un token de acceso de Azure AD. El token de acceso se utiliza para autenticar el usuario especificado y, a continuación, autorizar una solicitud para crear un blob en bloques. Para que este ejemplo funcione, siga primero los pasos descritos en las secciones anteriores.

> [!NOTE]
> Como propietario de la cuenta de Azure Storage, no se le asignan automáticamente permisos para tener acceso a datos. Tiene que asignarse a sí mismo de forma explícita un rol RBAC para Azure Storage. Puede asignarlo al nivel de su suscripción, grupo de recursos, cuenta de almacenamiento o un contenedor o cola.
>
> Por ejemplo, para ejecutar el código de ejemplo en una cuenta de almacenamiento de la que sea propietario y bajo su propia identidad de usuario, tiene que asignarse a sí mismo el rol RBAC de Colaborador de datos de blob. De lo contrario, la llamada para crear el blob dará un error con el código de estado HTTP 403 (Prohibido). Para obtener más información, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Valores conocidos para la autenticación con Azure AD

Para autenticar una entidad de seguridad con Azure AD, tiene que incluir algunos valores conocidos en el código.

#### <a name="azure-ad-authority"></a>Autoridad de Azure AD

Para la nube pública de Microsoft, la autoridad base de Azure AD es la siguiente, donde *tenant-id* es el identificador de inquilino de Active Directory (o identificador de directorio):

`https://login.microsoftonline.com/<tenant-id>/`

El identificador de inquilino identifica el inquilino de Azure AD que se usará para la autenticación. Para recuperar el identificador del inquilino, siga los pasos descritos en la sección titulada **obtener el identificador de inquilino de Azure Active Directory**.

#### <a name="storage-resource-id"></a>Identificador de recurso de Storage

Use el identificador de recurso de Azure Storage para adquirir un token para autorizar las solicitudes a Azure Storage:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Obtención del identificador de inquilino para Azure Active Directory

Para obtener el identificador de inquilino, siga estos pasos:

1. En Azure Portal, seleccione Active Directory.
2. Haga clic en **Propiedades**.
3. Copie el valor GUID proporcionado para el **identificador de directorio**. Este valor también se denomina identificador de inquilino.

![Captura de pantalla que muestra cómo copiar el identificador del inquilino](./media/storage-auth-aad-app/aad-tenant-id.png)

## <a name="set-up-a-basic-web-app-to-authenticate-to-azure-ad"></a>Configurar una aplicación web básica para autenticarse en Azure AD

Cuando la aplicación accede a almacenamiento de Azure, etc. como nombre del usuario. Para probar este ejemplo de código, necesita una web de aplicación que solicita el usuario puede iniciar sesión con una identidad de Azure AD. Puede descargar este [ejemplo de código](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) para probar una aplicación web básica que se autentica con su cuenta de Azure AD.

### <a name="completed-sample"></a>Ejemplo completado

Una versión funcional y completa del código de ejemplo se muestra en este artículo puede descargarse desde [GitHub](http://aka.ms/aadstorage). Revisar y ejecutar el ejemplo completo pueden ser útiles para comprender los ejemplos de código.

### <a name="add-references-and-using-statements"></a>Incorporación de referencias y uso de instrucciones  

Desde Visual Studio, instale la biblioteca de cliente de Azure Storage. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. Escriba los siguientes comandos en la ventana de consola para instalar los paquetes necesarios de la biblioteca de cliente de Azure Storage para. NET:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

A continuación, agregue las siguientes instrucciones using al archivo HomeController.cs:

```csharp
using System;
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="create-a-block-blob"></a>Creación de un blob en bloques

Agregue el siguiente fragmento de código para crear un blob en bloques:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/sample-container/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> Para autorizar las operaciones de blob y cola con un token de OAuth 2.0, debe usar HTTPS.

En el ejemplo anterior, la biblioteca cliente de .NET controla la autorización de la solicitud para crear el blob en bloques. Bibliotecas de cliente de Azure Storage para otros lenguajes también controlan la autorización de la solicitud para usted. Sin embargo, si llama a una operación de Azure Storage con un token de OAuth mediante la API de REST, deberá autorizar la solicitud con el token de OAuth.

Para llamar a las operaciones Blob y Queue service con tokens de acceso de OAuth, pase el token de acceso del encabezado **Autorización** con el esquema **Portador** y especifique una versión de servicio 2017-11-09 o superior, como se muestra en el ejemplo siguiente:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Obtención de un token de OAuth de Azure AD

A continuación, agregue un método que solicita un token de Azure AD. El token de solicitud será en nombre del usuario y utilizamos el método GetTokenOnBehalfOfUser.

Para solicitar el token, necesitará los siguientes valores de registro de la aplicación,

- Id. de inquilino (o directorio)
- Id. de cliente (o aplicación)
- URI de redirección de cliente

Recuerde que, si solo ha iniciado sesión y solicita un token para el `storage.azure.com` recursos, deberá presentar al usuario una interfaz de usuario donde el usuario puede dar su consentimiento para esta acción en su nombre. Para facilitar que deberá detectar el `MsalUiRequiredException` y agregar la funcionalidad para solicitar el consentimiento del usuario, como se muestra en el ejemplo siguiente:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties = BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

El consentimiento es el proceso por el cual un usuario concede autorización para que una aplicación acceda a recursos protegidos en su nombre. La plataforma de identidad de Microsoft 2.0 es compatible con consentimiento incremental, lo que significa que una entidad de seguridad puede solicitar inicialmente un conjunto mínimo de permisos y agregar los permisos con el tiempo según sea necesario. Cuando el código solicita un token de acceso, especifique el ámbito de permisos que necesita la aplicación en un momento dado, en el `scope` parámetro. El método siguiente crea las propiedades de autenticación para solicitar el consentimiento incremental:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes, MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET / MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope, scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de la plataforma Microsoft identity, vea [plataforma Microsoft identity](https://docs.microsoft.com/azure/active-directory/develop/).
- Para obtener más información acerca de los roles RBAC para el almacenamiento de Azure, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para obtener información sobre el uso de identidades administradas por los recursos de Azure con Azure Storage, consulte [autenticar el acceso a los blobs y colas con Azure Active Directory y las identidades administradas para Azure Resources](storage-auth-aad-msi.md).
