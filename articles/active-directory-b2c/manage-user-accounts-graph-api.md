---
title: Uso de Graph API en Azure Active Directory B2C
description: Procedimiento para administrar los usuarios de un inquilino de Azure AD B2C llamando a Graph API de Azure AD y mediante una identidad de aplicación para automatizar el proceso.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a9e55edcb7c107a3dfa91f61aaa1fea64bc62f21
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851059"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Usar Graph API de Azure AD

Los inquilinos de Azure Active Directory B2C (Azure AD B2C) pueden tener miles o millones de usuarios. Esto supone que muchas tareas comunes de administración de inquilinos necesitan realizarse mediante programación. Un ejemplo importante es la administración de usuarios.

Quizá necesite migrar un almacén de usuario existente a un inquilino de B2C. Puede que desee hospedar un registro de usuarios en su propia página y crear cuentas de usuario en su directorio de Azure AD B2C en segundo plano. Estos tipos de tareas requieren la capacidad de crear, leer, actualizar y eliminar cuentas de usuario. Puede realizar estas tareas mediante Graph API de Azure AD.

Para los inquilinos de B2C, existen dos modos de comunicación principales con Graph API.

* Para las tareas **interactivas**, que se ejecutan una vez, deberá actuar como una cuenta de administrador en el inquilino de B2C al ejecutar las tareas. Este modo requiere que un administrador inicie sesión con credenciales antes de que este pueda realizar llamadas a Graph API.
* Para las tareas **automatizadas** y continuas, tiene que usar algún tipo de cuenta de servicio a la que conceda los privilegios necesarios para realizar tareas de administración. En Azure AD, puede hacerlo mediante el registro de una aplicación y la autenticación en Azure AD. Esto se realiza con un *identificador de aplicación* que usa la [concesión de credenciales de cliente OAuth 2.0](../active-directory/develop/service-to-service.md). En este caso, la aplicación actúa como tal, no como un usuario, para llamar a Graph API.

En este artículo, aprenderá a realizar el caso de uso automatizado. Compilará un `B2CGraphClient` de .NET 4.5, que realiza operaciones de crear, leer, actualizar y eliminar. El cliente tendrá una interfaz de línea de comandos (CLI) de Windows que permite invocar diversos métodos. Sin embargo, el código se escribe para que se comporte de forma no interactiva y automatizada.

## <a name="prerequisites"></a>Prerequisites

Para poder crear aplicaciones o usuarios, necesita un inquilino de Azure AD B2C. Si todavía no tienen ninguno, [Cree un inquilino de Azure Active Directory B2C](tutorial-create-tenant.md).

## <a name="register-an-application"></a>Registro de una aplicación

Una vez que tenga un inquilino de Azure AD B2C, deberá registrar la aplicación de administración desde [Azure Portal](https://portal.azure.com). También debe concederle los permisos necesarios para realizar tareas de administración en nombre de la aplicación de administración o el script automatizado.

### <a name="register-application-in-azure-active-directory"></a>Registro de una aplicación en Azure Active Directory

Para usar Graph API de Azure AD con el inquilino de B2C, debe registrar una aplicación mediante el flujo de trabajo de registros de aplicaciones de Azure Active Directory.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="assign-api-access-permissions"></a>Asignación de permisos de acceso de API

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>Creación de un secreto de cliente

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Ahora tiene una aplicación con permisos para *crear*, *leer* y *actualizar* usuarios en el inquilino de Azure AD B2C. Continúe a la siguiente sección para agregar los permisos de *actualización de contraseñas* y *eliminación* de usuarios.

## <a name="add-user-delete-and-password-update-permissions"></a>Agregar los permisos de actualización de contraseñas y eliminación de usuarios

El permiso *Leer y escribir datos de directorio* que concedió antes **NO** permite eliminar usuarios ni actualizar sus contraseñas.

Si quiere que la aplicación pueda eliminar usuarios o actualizar contraseñas, debe concederle el rol *Administrador de usuarios*.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorio y suscripción** en la barra de herramientas del portal y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Administrar**, seleccione **Roles y administradores**.
1. Seleccione el rol **Administrador de usuarios**.
1. Seleccione **Agregar asignación**.
1. En el cuadro de texto **Seleccionar**, escriba el nombre de la aplicación que registró antes; por ejemplo, *managementapp1*. Seleccione su aplicación cuando aparezca en los resultados de búsqueda.
1. Seleccione **Agregar**. Los permisos pueden tardar unos minutos en propagarse por completo.

Ahora, la aplicación Azure AD B2C tiene los permisos adicionales necesarios para eliminar usuarios o actualizar sus contraseñas en el inquilino de B2C.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

El ejemplo de código es una aplicación de consola de .NET que usa la [Biblioteca de autenticación de Active Directory (ADAL)](../active-directory/develop/active-directory-authentication-libraries.md) para interactuar con Graph API de Azure AD. En su código se muestra cómo llamar a la API para administrar usuarios mediante programación en un inquilino de Azure AD B2C.

Puede [descargar el archivo de ejemplo ](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip) (\*.zip) o clonar el repositorio de GitHub:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Después de obtener el ejemplo de código, configúrelo para su entorno y, a continuación, compile el proyecto:

1. Abra la solución `B2CGraphClient\B2CGraphClient.sln` en Visual Studio.
1. En el proyecto **B2CGraphClient**, abra el archivo *App.config*.
1. Reemplace la sección `<appSettings>` con el código XML siguiente. A continuación, reemplace `{your-b2c-tenant}` con el nombre de su inquilino y `{Application ID}` y `{Client secret}` con los valores que registró antes.

    ```xml
    <appSettings>
        <add key="b2c:Tenant" value="{your-b2c-tenant}.onmicrosoft.com" />
        <add key="b2c:ClientId" value="{Application ID}" />
        <add key="b2c:ClientSecret" value="{Client secret}" />
    </appSettings>
    ```

1. Compile la solución. Haga clic con el botón derecho en la solución **B2CGraphClient** del Explorador de soluciones y, después, seleccione **Recompilar solución**.

Si la compilación se realiza correctamente, puede encontrar la aplicación de consola `B2C.exe` en `B2CGraphClient\bin\Debug`.

## <a name="review-the-sample-code"></a>Revisión del código de ejemplo

Para usar B2CGraphClient, abra un símbolo del sistema (`cmd.exe`) y cambie al directorio `Debug` del proyecto. A continuación, ejecute el comando `B2C Help`.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

El comando `B2C Help` muestra una breve descripción de los subcomandos disponibles. Cada vez que invoque uno de estos subcomandos, `B2CGraphClient` envía una solicitud a Graph API de Azure AD.

En las secciones siguientes se describe cómo el código de la aplicación realiza llamadas a Graph API de Azure AD.

### <a name="get-an-access-token"></a>Obtención de un token de acceso

Cualquier solicitud a Graph API de Azure AD requiere un token de acceso para la autenticación. `B2CGraphClient` usa la Biblioteca de autenticación de Active Directory (ADAL) para ayudarle a obtener tokens de acceso. ADAL facilita la adquisición de tokens al proporcionar una API auxiliar y ocuparse de algunos detalles importantes, como el almacenamiento en caché de tokens de acceso. Sin embargo, no tiene que usar ADAL para obtener tokens. En lugar de eso, para obtener tokens, puede elaborar solicitudes HTTP manualmente.

> [!NOTE]
> Debe usar ADAL v2 o posterior con el fin de obtener los tokens de acceso que se pueden utilizar con Graph API de Azure AD. No se puede usar ADAL v1.

Cuando se ejecuta `B2CGraphClient`, se crea una instancia de la clase `B2CGraphClient`. El constructor para esta clase configura el scaffolding de autenticación de ADAL:

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

A continuación, usaremos el comando `B2C Get-User` como ejemplo.

Cuando se invoca `B2C Get-User` sin argumentos adicionales, la aplicación llama al método `B2CGraphClient.GetAllUsers()`. A continuación, `GetAllUsers()` llama a `B2CGraphClient.SendGraphGetRequest()`, que envía una solicitud HTTP GET a Graph API de Azure AD. Antes de que `B2CGraphClient.SendGraphGetRequest()` envíe la solicitud GET, primero obtiene un token de acceso mediante ADAL:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);
    ...
```

Puede obtener un token de acceso para Graph API mediante una llamada al método `AuthenticationContext.AcquireToken()` de ADAL. ADAL devuelve un `access_token` que representa la identidad de la aplicación.

### <a name="read-users"></a>Lectura de usuarios

Si quiere obtener una lista de usuarios o un usuario determinado de Graph API de Azure AD, puede enviar una solicitud HTTP `GET` al punto de conexión `/users`. Una solicitud para todos los usuarios de un inquilino tiene este aspecto:

```HTTP
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Para ver esta solicitud, ejecute:

 ```cmd
 B2C Get-User
 ```

Hay dos aspectos importantes que se deben tener en cuenta:

* El token de acceso adquirido mediante ADAL se agrega al encabezado `Authorization` según el esquema `Bearer`.
* Para los inquilinos de B2C, debe usar el parámetro de consulta `api-version=1.6`.

Estos dos detalles se controlan en el método `B2CGraphClient.SendGraphGetRequest()` :

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Creación de cuentas de usuario consumidor

Al crear cuentas de usuario en el inquilino de B2C, puede enviar una solicitud HTTP `POST` al punto de conexión `/users`. La siguiente solicitud HTTP `POST` muestra un usuario de ejemplo que se va a crear en el inquilino.

La mayoría de las propiedades de esta solicitud son necesarias para crear usuarios consumidores. Los comentarios `//` se han incluido con fines ilustrativos: no los incluya en una solicitud real.

```HTTP
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                           // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",            // can be 'emailAddress' or 'userName'
            "value": "consumer@fabrikam.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Consumer User",            // a value that can be used for displaying to the end user
    "mailNickname": "cuser",                   // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false  // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Para ver la solicitud, ejecute uno de los siguientes comandos:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

El comando `Create-User` toma como parámetro de entrada un archivo JSON que contiene una representación JSON de un objeto de usuario. Hay dos archivos JSON de ejemplo en el ejemplo de código: `usertemplate-email.json` y `usertemplate-username.json`. Puede modificar estos archivos para satisfacer sus necesidades. Además de los campos obligatorios anteriores, se incluyen varios campos opcionales en los archivos.

Para obtener más información sobre los campos obligatorios y opcionales, consulte [Entity and complex type reference | Graph API reference](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference) (Referencia de entidades y tipos complejos | Referencia de Graph API).

Puede ver cómo se construye la solicitud POST en `B2CGraphClient.SendGraphPostRequest()`:

* Adjunta un token de acceso al encabezado `Authorization` de la solicitud.
* Establece `api-version=1.6`.
* Incluye el objeto de usuario JSON en el cuerpo de la solicitud.

> [!NOTE]
> Si las cuentas que se van a migrar desde un almacén de usuario existente tienen menos seguridad de contraseña que la [seguridad de contraseña exigida por Azure AD B2C](user-flow-password-complexity.md), puede deshabilitar el requisito de contraseña segura mediante el valor `DisableStrongPassword` de la propiedad `passwordPolicies`. Por ejemplo, puede modificar la solicitud de creación de usuario anterior de la siguiente manera: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Actualización de cuentas de usuario consumidor

Al actualizar objetos de usuario, el proceso es similar al que se utiliza para crear objetos de usuario, pero usa el método HTTP `PATCH`:

```HTTP
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer"    // this request updates only the user's displayName
}
```

Intente actualizar un usuario modificando algunos valores en los archivos JSON y, a continuación, use `B2CGraphClient` para ejecutar uno de estos comandos:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Consulte el método `B2CGraphClient.SendGraphPatchRequest()` para obtener más información sobre cómo enviar esta solicitud.

### <a name="search-users"></a>Búsqueda de usuarios

Puede buscar usuarios en el inquilino de B2C de las siguientes maneras:

* Haga referencia al **identificador de objeto** del usuario.
* Haga referencia a su identificador de inicio de sesión: la propiedad `signInNames`.
* Haga referencia a cualquiera de los parámetros de OData válidos. Por ejemplo, "givenName", "surname", "displayName", etc.

Ejecute uno de los comandos siguientes para buscar un usuario:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Por ejemplo:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27consumer@fabrikam.com%27)
B2C get-user $filter=givenName%20eq%20%27John%27
B2C get-user $filter=surname%20eq%20%27Doe%27
B2C get-user $filter=displayName%20eq%20%27John%20Doe%27
```

### <a name="delete-users"></a>Eliminación de usuarios

Para eliminar usuarios, use el método HTTP `DELETE` y construya la URL con el identificador de objeto del usuario:

```HTTP
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Para ver un ejemplo, ejecute este comando y vea la solicitud Delete que se imprime en la consola:

```cmd
B2C Delete-User <object-id-of-user>
```

Consulte el método `B2CGraphClient.SendGraphDeleteRequest()` para obtener más información sobre cómo enviar esta solicitud.

Puede realizar muchas otras acciones con Graph API de Azure AD además de la administración de usuarios. La [referencia de Graph API de Azure AD](/previous-versions/azure/ad/graph/api/api-catalog) proporciona detalles sobre cada acción, junto con solicitudes de ejemplo.

## <a name="use-custom-attributes"></a>Uso de atributos personalizados

La mayoría de las aplicaciones de consumidor necesita almacenar algún tipo de información de perfil de usuario personalizada. Una manera de hacerlo es definir un atributo personalizado en el inquilino de B2C. Puede tratar este atributo de la misma manera que trataría cualquier otra propiedad en un objeto de usuario. Puede actualizar el atributo, eliminarlo, consultarlo, enviarlo como notificación en tokens de inicio de sesión, etc.

Para definir un atributo personalizado en el inquilino de B2C, consulte la [referencia de atributos personalizados de B2C](user-flow-custom-attributes.md).

Puede ver los atributos personalizados definidos en el inquilino de B2C mediante los siguientes comandos `B2CGraphClient`:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

El resultado muestra los detalles de cada atributo personalizado. Por ejemplo:

```json
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Puede usar el nombre completo, por ejemplo `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, como propiedad en los objetos de usuario. Actualice el archivo JSON con la nueva propiedad, un valor para la propiedad y ejecute:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

## <a name="next-steps"></a>Pasos siguientes

Con `B2CGraphClient`, tiene una aplicación de servicio que puede administrar sus usuarios del inquilino de B2C mediante programación. `B2CGraphClient` usa su propia identidad de aplicación para autenticarse en Graph API de Azure AD. También adquiere tokens mediante un secreto de cliente.

Según vaya incorporando esta funcionalidad a su aplicación, debe recordar algunos puntos clave para las aplicaciones B2C:

* Conceda a la aplicación los permisos necesarios en el inquilino.
* Cuando llame a Graph API, use `api-version=1.6`.
* Al crear y actualizar los usuarios consumidores, hay algunas propiedades obligatorias, tal como se describió anteriormente.
