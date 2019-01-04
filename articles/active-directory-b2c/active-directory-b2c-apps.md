---
title: Tipos de aplicaciones que se pueden usar en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre los tipos de aplicaciones que puede usar en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 91102b9fe57b2291ce1d1678b71b3a8b0b834864
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721976"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Tipos de aplicaciones que se pueden usar en Active Directory B2C

Azure Active Directory (Azure AD) B2C admite la autenticación para una diversas arquitecturas de aplicaciones modernas. Todas ellas se basan en los protocolos estándar del sector [OAuth 2.0](active-directory-b2c-reference-protocols.md) u [OpenID Connect](active-directory-b2c-reference-protocols.md). Este documento describe los tipos de aplicaciones que puede compilar, independientemente del lenguaje o la plataforma que prefiera. También ayuda a entender los escenarios de alto nivel antes de empezar a compilar aplicaciones.

Toda aplicación que utilice Azure AD B2C debe estar registrada en su [inquilino de Azure AD B2C](active-directory-b2c-get-started.md) que use [Azure Portal](https://portal.azure.com/). El proceso de registro de la aplicación recopila y asigna valores, tales como:

* Un **identificador de la aplicación** que identifica la aplicación de forma única.
* Un **URI de redirección** que puede usarse para devolver las respuestas a la aplicación.

Cada solicitud que se envía a Azure AD B2C especifica un **flujo de usuario**, que es una directiva que controla el comportamiento de Azure AD. También puede utilizar estos puntos de conexión para crear un conjunto de experiencias de usuario muy personalizable. Ofrecemos un conjunto de flujos de usuario que lo ayudarán a configurar directivas comunes, incluidas las de registro, de inicio de sesión y de edición de perfiles. También se pueden crear directivas personalizadas. Si no está familiarizado con las directivas, debe leer acerca del [marco de directiva extensible](active-directory-b2c-reference-policies.md) de Azure AD B2C antes de continuar.

La interacción de cada aplicación sigue un patrón de nivel alto similar:

1. La aplicación dirige al usuario al punto de conexión v2.0 para ejecutar una [directiva](active-directory-b2c-reference-policies.md).
2. El usuario completa la directiva según la definición de la misma.
3. La aplicación recibe un token de seguridad del punto de conexión v2.0.
4. La aplicación utiliza el token de seguridad para acceder a información o recursos protegidos.
5. El servidor de recursos valida el token de seguridad para asegurarse de que se puede conceder acceso.
6. La aplicación actualiza periódicamente el token de seguridad.

Estos pasos pueden diferir ligeramente según el tipo de aplicación que está compilando.

## <a name="web-applications"></a>Aplicaciones web

Para las aplicaciones web (incluidas .NET, PHP, Java, Ruby, Python y Node.js) que se hospedan en un servidor y a las que se tiene acceso a través de un explorador, Azure AD B2C admite [OpenID Connect](active-directory-b2c-reference-protocols.md) para todas las experiencias de usuario. Esto incluye el inicio de sesión, el registro y la administración de perfiles. En la implementación de OpenID Connect de Azure AD B2C, su aplicación web inicia estas experiencias de usuario mediante la emisión de solicitudes de autenticación a Azure AD. El resultado de la solicitud es un elemento `id_token`. Este token de seguridad representa la identidad del usuario. También proporciona información sobre el usuario en forma de notificaciones:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Obtenga más información sobre todos los tipos de tokens y notificaciones disponibles para una aplicación en la [referencia de token de Azure AD B2C](active-directory-b2c-reference-tokens.md).

En una aplicación web, cada ejecución de una [directiva](active-directory-b2c-reference-policies.md) realiza estos pasos generales:

1. El usuario se desplaza a la aplicación web.
2. La aplicación web redirige al usuario a Azure AD B2C que indicando la directiva que se ejecutará.
3. El usuario complete la directiva.
4. Azure AD B2C devuelve un `id_token` al explorador.
5. El `id_token` se publica en el URI de redirección.
6. El `id_token` se valida y se establece una cookie de sesión.
7. Se devuelve al usuario una página segura.

La validación de `id_token` mediante una clave de firma pública que se recibe de Azure AD es suficiente para comprobar la identidad del usuario. Esto también establece una cookie de sesión que puede usarse para identificar al usuario en las solicitudes de página posteriores.

Para ver este escenario en acción, pruebe uno de los ejemplos de código de inicio de sesión de aplicación web en nuestra [sección Introducción](active-directory-b2c-overview.md).

Además de facilitar un inicio de sesión simple, es posible que una aplicación de servidor web también necesite acceder a algún servicio web back-end. En este caso, la aplicación web puede realizar un [flujo de OpenID Connect](active-directory-b2c-reference-oidc.md) ligeramente distinto y adquirir tokens mediante códigos de autorización y tokens de actualización. Este escenario se describe en la siguiente sección [API web](#web-apis).

## <a name="web-apis"></a>API web

También puede usar Azure AD B2C para proteger los servicios web, como la API web RESTful de su aplicación. Las API web puede usar OAuth 2.0 para proteger sus datos mediante la autenticación de las solicitudes HTTP entrantes con tokens. El llamador de una API web anexa un token en el encabezado de autorización de una solicitud HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

A continuación, la API web puede usar el token para comprobar la identidad del llamador de la API y extraer información sobre el llamador de notificaciones que se codifican en el token. Aprenda más sobre todos los tipos de tokens y notificaciones disponibles para una aplicación en la [referencia de token de Azure AD B2C](active-directory-b2c-reference-tokens.md).

Una API web puede recibir tokens de muchos tipos de clientes, incluidas aplicaciones web, aplicaciones móviles y de escritorio, aplicaciones de una página, demonios del lado del servidor e incluso otras API web. Por ejemplo, veamos el flujo completo de una aplicación web que llama a una API web:

1. La aplicación web ejecuta una directiva y el usuario completa la experiencia de usuario.
2. Azure AD B2C devuelve un `access_token` y un código de autorización al explorador.
3. El explorador publica el `access_token` y un código de autorización en el URI de redirección.
4. El servidor web valida el `access token` y establece una cookie de sesión.
5. El `access_token` se proporciona a Azure AD B2C con el código de autorización, el identificador de cliente de aplicación y las credenciales.
6. El `access_token` y `refresh_token` se devuelven al servidor web.
7. Se llama a la API web con el `access_token` en un encabezado de autorización.
8. La API web valida el token.
9. Se devuelven datos seguros al servidor web.

Para más información sobre los códigos de autorización, los tokens de actualización y los pasos para obtener tokens, lea acerca del [protocolo OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

Para más información sobre cómo proteger una API web con Azure AD B2C, consulte los tutoriales de API web en nuestra sección [Introducción](active-directory-b2c-overview.md).

## <a name="mobile-and-native-applications"></a>Aplicaciones móviles y aplicaciones nativas

Las aplicaciones instaladas en dispositivos, como aplicaciones móviles y de escritorio, suelen necesitar el acceso a servicios back-end o a las API web en nombre de los usuarios. Puede agregar experiencias de administración de identidades personalizadas a sus aplicaciones nativas y llamar de forma segura a servicios back-end con Azure AD B2C y el [flujo de código de autorización de OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

En este flujo, la aplicación ejecuta [directivas](active-directory-b2c-reference-policies.md) y recibe un elemento `authorization_code` de Azure AD después de que el usuario complete la directiva. El elemento `authorization_code` representa el permiso de la aplicación para llamar a servicios de back-end en nombre del usuario que inició la sesión. La aplicación podrá intercambiar el elemento `authorization_code` en segundo plano para `id_token` y `refresh_token`.  La aplicación puede usar el elemento `id_token` para autenticarse en una API web de back-end en las solicitudes HTTP. También puede usar el elemento `refresh_token` para obtener un nuevo `id_token` cuando caduca uno más antiguo.

## <a name="current-limitations"></a>Limitaciones actuales

### <a name="application-not-supported"></a>Aplicación no admitida 

#### <a name="daemonsserver-side-applications"></a>Demonios o aplicaciones del lado del servidor

Las aplicaciones que contienen procesos de larga duración o que funcionan sin la presencia de un usuario también necesitan un modo de acceder a los recursos protegidos, como las API web. Estas aplicaciones pueden autenticar y obtener tokens con la identidad de la aplicación (en lugar de una identidad delegada del usuario) mediante el flujo de credenciales de cliente de OAuth 2.0. El flujo de credenciales de cliente no es igual al flujo en nombre de y este no se debe usar para la autenticación de servidor a servidor.

Si bien Azure AD B2C no admite actualmente el flujo de credenciales de cliente, puede configurar el flujo de credenciales de cliente mediante Azure AD. Un inquilino de Azure AD B2C comparte algunas funcionalidades con los inquilinos empresariales de Azure AD.  El flujo de credenciales de cliente se admite con la funcionalidad de Azure AD que tiene el inquilino de Azure AD B2C. 

Para configurar el flujo de credenciales de cliente, consulte [Azure Active Directory v2.0 y el flujo de credenciales de cliente de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Una autenticación correcta genera la recepción de un token con formato para que Azure AD lo pueda usar tal como se describe en [Referencia de tokens de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

#### <a name="web-api-chains-on-behalf-of-flow"></a>Cadenas de la API web (flujo en nombre de)

Muchas arquitecturas incluyen una API web que necesita llamar a otra API web de nivel inferior, ambas protegidas mediante Azure AD B2C. Este escenario es habitual en los clientes nativos que tienen una API web back-end. Esto llama a un servicio en línea de Microsoft, como Graph API de Azure AD.

Este escenario de API web encadenadas puede admitirse mediante la concesión de credenciales de portador JWT de OAuth 2.0, también conocido como flujo "en nombre de".  Sin embargo, el flujo "en nombre de" no está implementado actualmente en Azure AD B2C.

### <a name="reply-url-values"></a>Valores de dirección URL de respuesta

Actualmente, las aplicaciones registradas en Azure AD B2C están restringidas a un conjunto limitado de valores de direcciones URL de respuesta. La dirección URL de respuesta de aplicaciones y servicios web debe comenzar por el esquema `https`, y todos los valores de dicha dirección deben compartir un único dominio DNS. Por ejemplo, no puede registrar una aplicación web con una de estas direcciones URL de respuesta:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

El sistema de registro compara el nombre DNS completo de la dirección URL de respuesta existente con el nombre DNS de la dirección URL de respuesta que va a agregar. La solicitud para agregar el nombre DNS presentará un error si se cumple alguna de las condiciones siguientes:

- Si el nombre DNS completo de la nueva dirección URL de respuesta no coincide con el nombre DNS de la dirección URL de respuesta existente.
- Si el nombre DNS completo de la nueva dirección URL de respuesta no es un subdominio de la dirección URL de respuesta existente.

Por ejemplo, si la aplicación tiene esta dirección URL de respuesta:

`https://login.contoso.com`

Puede agregarla del modo siguiente:

`https://login.contoso.com/new`

En este caso, el nombre DNS es una coincidencia exacta. O bien, puede hacer lo siguiente:

`https://new.login.contoso.com`

En este caso, hace referencia a un subdominio DNS de login.contoso.com. Si desea que una aplicación tenga login-east.contoso.com y login-west.contoso.com como direcciones URL de respuesta, debe agregar dichas direcciones en este orden:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Puede agregar las dos últimas porque son subdominios de la primera URL de respuesta, contoso.com. 

Al crear aplicaciones móviles o nativas, definirá un **URI de redirección** en lugar de una **dirección URL de reproducción**. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:

- **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario verá un cuadro de diálogo de **elección de aplicación**. Si el usuario realiza una elección incorrecta, no será posible iniciar sesión.
- **Completo**: el identificador URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//contoso/` sirve y `//contoso` produce un error. Asegúrese de que no haya ningún carácter especial, como el carácter de subrayado, en el URI de redirección.

### <a name="faulted-apps"></a>Aplicaciones con errores

Las aplicaciones de Azure AD B2C NO se deben editar:

- En otros portales de administración de aplicaciones, como el  [Portal de registro de aplicaciones](https://apps.dev.microsoft.com/).
- Con Graph API o PowerShell.

Si edita la aplicación de Azure AD B2C fuera de Azure Portal, se convierte en una aplicación con errores y ya no se puede usar con Azure AD B2C. Tendrá que eliminarla y volver a crearla.

Para eliminar la aplicación, vaya al [Portal de registro de aplicaciones](https://apps.dev.microsoft.com/) y elimínela desde allí. Para que la aplicación esté visible, debe ser el propietario de la misma (y no solo un administrador del inquilino).

