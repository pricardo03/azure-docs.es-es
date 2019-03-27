---
title: Autenticar con Azure Active Directory para acceder a los datos de blob y cola desde las aplicaciones | Microsoft Docs
description: Use Azure Active Directory para autenticar desde dentro de una aplicación y, a continuación, autorizar las solicitudes a los blobs y colas.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8fdc4445e28a420b6b4f7935443d7d991d9e8a4d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446059"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>Autenticar con Azure Active Directory desde una aplicación para el acceso a los blobs y colas

Una ventaja clave del uso de Azure Active Directory (Azure AD) con Azure Storage es que ya no necesita almacenar las credenciales en el código. En su lugar, puede solicitar a Azure AD un token de acceso de OAuth 2.0. Azure AD controla la autenticación de la entidad de seguridad (un usuario, grupo o entidad de servicio) ejecutando la aplicación. Si la autenticación se realiza correctamente, Azure AD devuelve el token de acceso a la aplicación y la aplicación puede entonces usar el token de acceso para autorizar las solicitudes de Azure Storage.

Este artículo muestra cómo configurar la aplicación para la autenticación con Azure AD. El ejemplo de código incluye .NET, pero otros lenguajes utilizan un enfoque similar.

Antes de autenticar a una entidad de seguridad de la aplicación de Azure Storage, configure la configuración de acceso basado en roles (RBAC) de control para esa entidad de seguridad. Azure Storage define los roles RBAC que abarcan los permisos para los contenedores y las colas. Cuando el rol RBAC se asigna a una entidad de seguridad, esta recibe acceso a ese recurso. Para obtener más información, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).

Para información general sobre el flujo de concesión de código OAuth 2.0, consulte [Autorización del acceso a aplicaciones web de Azure Active Directory mediante el flujo de concesión de código OAuth 2.0](../../active-directory/develop/v1-protocols-oauth-code.md).

Para autorizar las operaciones de blob y cola con un token de OAuth, debe usar HTTPS.

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Asignar un rol RBAC a una entidad de seguridad de Azure AD

Para autenticar a una entidad de seguridad de la aplicación de Azure Storage, primero configure los valores del control de acceso basado en roles (RBAC) para esa entidad de seguridad. Azure Storage define los roles RBAC que abarcan los permisos para los contenedores y las colas. Cuando el rol RBAC se asigna a una entidad de seguridad, esta recibe acceso a ese recurso. Para obtener más información, consulte [administrar derechos de acceso a datos de Azure Blob y cola con RBAC](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registro de la aplicación con un inquilino de Azure AD

El primer paso para usar Azure AD con el fin de autorizar el acceso a los recursos de almacenamiento es registrar la aplicación cliente en un inquilino de Azure AD. El registro de la aplicación le permite llamar a la [Biblioteca de autenticación de Active Directory](../../active-directory/active-directory-authentication-libraries.md) (ADAL) de Azure desde el código. La ADAL proporciona una API para autenticar con Azure AD desde la aplicación. Registrar la aplicación también le permite autorizar llamadas desde esa aplicación a las API de Azure Storage con un token de acceso.

Al registrar la aplicación, facilita información acerca de la aplicación a Azure AD. Azure AD proporciona un identificador de cliente (también denominado *identificador de aplicación*) que se utiliza para asociar la aplicación con Azure AD en runtime. Para más información sobre el identificador de cliente, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md).

Siga los pasos que aparecen en la sección [Incorporación de una aplicación](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md) en [Integración de aplicaciones con Azure Active Directory](../../active-directory/active-directory-integrating-applications.md) para registrar la aplicación de Azure Storage. Si registra la aplicación como una aplicación nativa, puede especificar cualquier URI válido para el **URI de redirección**. No es necesario que el valor sea un punto de conexión real.

![Captura de pantalla que muestra cómo registrar la aplicación de almacenamiento con Azure AD](./media/storage-auth-aad-app/app-registration.png)

Una vez que ha registrado su aplicación, verá el identificador de la aplicación (o identificador de cliente) en **Configuración**:

![Captura de pantalla muestra el identificador de cliente](./media/storage-auth-aad-app/app-registration-client-id.png)

Para más información sobre cómo registrar una aplicación con Azure AD, consulte [Integración de aplicaciones con Azure Active Directory](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Concesión de permisos a la aplicación registrada para Azure Storage

A continuación, deberá conceder los permisos de aplicación para llamar a las API de Azure Storage. Este paso permite que la aplicación autorizar las llamadas a Azure Storage con Azure AD.

1. En el panel de navegación izquierdo de Azure Portal, elija **Todos los servicios** y busque **Registros de aplicaciones**.
2. Busque el nombre de la aplicación registrada que creó en el paso anterior.
3. Seleccione la aplicación registrada y haga clic en **Configuración**. En la sección **Acceso de API**, seleccione **Permisos necesarios**.
4. En la hoja **Permisos necesarios**, haga clic en el botón **Agregar**.
5. En **Seleccionar una API**, busque "Azure Storage" y seleccione **Azure Storage** en la lista de resultados.

    ![Captura de pantalla que muestra los permisos para el almacenamiento](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. En **Seleccionar permisos**, active la casilla junto a **Acceder a Azure Storage** y haga clic en **Seleccionar**.
7. Haga clic en **Done**(Listo).

La ventana **Permisos necesarios** muestra ahora que la aplicación de Azure AD tiene acceso tanto a Azure Active Directory como a Azure Storage. La primera vez que registra la aplicación con Azure AD, se conceden permisos a Azure AD automáticamente.

![Captura de pantalla muestra registrar permisos de aplicación](media/storage-auth-aad-app/registered-app-permissions-2.png)

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

El identificador de inquilino identifica el inquilino de Azure AD que se usará para la autenticación. Para recuperar el identificador de inquilino, siga los pasos descritos en **Obtención del identificador de inquilino para Azure Active Directory**.

#### <a name="storage-resource-id"></a>Identificador de recurso de Storage

Use el identificador de recurso de Azure Storage para adquirir un token para autenticar las solicitudes en Azure Storage:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Obtención del identificador de inquilino para Azure Active Directory

Para obtener el identificador de inquilino, siga estos pasos:

1. En Azure Portal, seleccione Active Directory.
2. Haga clic en **Propiedades**.
3. Copie el valor GUID proporcionado para el **identificador de directorio**. Este valor también se denomina identificador de inquilino.

![Captura de pantalla que muestra cómo copiar el identificador del inquilino](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Incorporación de referencias y uso de instrucciones  

Desde Visual Studio, instale la biblioteca de cliente de Azure Storage. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**. Escriba el siguiente comando en la consola para instalar la versión más reciente de la biblioteca de cliente para .NET:

```
Install-Package WindowsAzure.Storage
```

Instale también la versión más reciente de ADAL:

```
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

Agregue lo siguiente al código mediante instrucciones:

```dotnet
using System.Globalization;
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Obtención de un token de OAuth de Azure AD

A continuación, agregue un método que solicita un token de Azure AD. Para solicitar el token, llame al método [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync). Asegúrese de que tiene los siguientes valores de los pasos realizados anteriormente:

- Identificador de inquilino (directorio)
- Identificador de cliente (aplicación)
- URI de redirección de cliente

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/";
    const string AuthInstance = "https://login.microsoftonline.com/{0}/";
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthInstance, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>Creación del blob en bloques

Por último, utilice el token de acceso para crear nuevas credenciales de almacenamiento y usar estas credenciales para crear el blob. Tenga en cuenta que para autorizar las operaciones de blob y cola con un token de OAuth, debe usar HTTPS.:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);

blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
```

> [!NOTE]
> La integración de Azure AD con Azure Storage requiere que se utilice HTTPS para las operaciones de Azure Storage.

En el ejemplo anterior, la biblioteca cliente de .NET controla la autorización de la solicitud para crear el blob en bloques. Otras bibliotecas cliente de almacenamiento también controlan la autorización de la solicitud por usted. Sin embargo, si llama a una operación de Azure Storage con un token de OAuth mediante la API de REST, deberá autorizar la solicitud con el token de OAuth.   

Para llamar a las operaciones Blob y Queue service con tokens de acceso de OAuth, pase el token de acceso del encabezado **Autorización** con el esquema **Portador** y especifique una versión de servicio 2017-11-09 o superior, como se muestra en el ejemplo siguiente:

```
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

Para obtener más información acerca de cómo autorizar las operaciones de almacenamiento de Azure de REST, consulte [autenticar con Azure Active Directory](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los roles RBAC para el almacenamiento de Azure, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para obtener información sobre el uso de identidades administradas por los recursos de Azure con Azure Storage, consulte [autenticar el acceso a los blobs y colas de Azure administra las identidades para Azure Resources](storage-auth-aad-msi.md).
- Para obtener información sobre cómo iniciar sesión PowerShell y CLI de Azure con una identidad de Azure AD, consulte [usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI](storage-auth-aad-script.md).
