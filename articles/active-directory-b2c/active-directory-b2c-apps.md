---
title: Tipos de aplicaciones que se pueden usar en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre los tipos de aplicaciones que puede usar en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e42bc63b0c2b6edf4dc0de204bbac5fe90071a67
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480519"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Tipos de aplicaciones que se pueden usar en Active Directory B2C

Azure Active Directory (Azure AD) B2C admite la autenticación para una diversas arquitecturas de aplicaciones modernas. Todas ellas se basan en los protocolos estándar del sector [OAuth 2.0](active-directory-b2c-reference-protocols.md) u [OpenID Connect](active-directory-b2c-reference-protocols.md). Este documento describe los tipos de aplicaciones que puede compilar, independientemente del lenguaje o la plataforma que prefiera. También ayuda a entender los escenarios de alto nivel antes de empezar a compilar aplicaciones.

Toda aplicación que utilice Azure AD B2C debe estar registrada en su [inquilino de Azure AD B2C](active-directory-b2c-get-started.md) que use [Azure Portal](https://portal.azure.com/). El proceso de registro de la aplicación recopila y asigna valores, tales como:

* Un **identificador de la aplicación** que identifica la aplicación de forma única.
* Un **URI de redirección** que puede usarse para dirigir las respuestas de nuevo a la aplicación.

En cada solicitud que se envía a Azure AD B2C se especifica una **directiva**. Una directiva controla el comportamiento de Azure AD. También puede utilizar estos puntos de conexión para crear un conjunto de experiencias de usuario muy personalizable. Algunas directivas comunes son las de registro, las de inicio de sesión y las de edición de perfiles. Si no está familiarizado con las directivas, debe leer acerca del [marco de directiva extensible](active-directory-b2c-reference-policies.md) de Azure AD B2C antes de continuar.

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

> [!NOTE]
> Azure AD B2C actualmente solo admite API web a las que acceden sus propios clientes conocidos. Por ejemplo, la aplicación completa puede incluir una aplicación iOS, una aplicación Android y una API web back-end. Esta arquitectura es totalmente compatible. Actualmente no se permite que un cliente de un asociado, como otra aplicación de iOS, tenga acceso a la misma API web. Todos los componentes de la aplicación deben compartir un identificador de aplicación único.
>
>

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

> [!NOTE]
> Actualmente, Azure AD B2C solo admite tokens que se usan para acceder a un servicio web back-end propio de la aplicación. Por ejemplo, la aplicación completa puede incluir una aplicación iOS, una aplicación Android y una API web back-end. Esta arquitectura es totalmente compatible. Actualmente no se permite que la aplicación iOS tenga acceso a una API web de un asociado mediante tokens de acceso de OAuth 2.0. Todos los componentes de la aplicación deben compartir un identificador de aplicación único.
>
>

## <a name="current-limitations"></a>Limitaciones actuales

Azure AD B2C no admite actualmente los siguientes tipos de aplicaciones, pero están dentro de los planes futuros. 

### <a name="daemonsserver-side-applications"></a>Demonios o aplicaciones del lado del servidor

Las aplicaciones que contienen procesos de larga duración o que funcionan sin la presencia de un usuario también necesitan un modo de acceder a los recursos protegidos, como las API web. Estas aplicaciones pueden autenticar y obtener tokens con la identidad de la aplicación (en lugar de una identidad delegada del usuario) mediante el flujo de credenciales de cliente de OAuth 2.0. El flujo de credenciales de cliente no es igual al flujo en nombre de y este no se debe usar para la autenticación de servidor a servidor.

Si bien Azure AD B2C no admite actualmente el flujo de credenciales de cliente, puede configurar el flujo de credenciales de cliente mediante Azure AD. Un inquilino de Azure AD B2C comparte algunas funcionalidades con los inquilinos empresariales de Azure AD.  El flujo de credenciales de cliente se admite con la funcionalidad de Azure AD que tiene el inquilino de Azure AD B2C. 

Para configurar el flujo de credenciales de cliente, consulte [Azure Active Directory v2.0 y el flujo de credenciales de cliente de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Una autenticación correcta genera la recepción de un token con formato para que Azure AD lo pueda usar tal como se describe en [Referencia de tokens de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="web-api-chains-on-behalf-of-flow"></a>Cadenas de la API web (flujo en nombre de)

Muchas arquitecturas incluyen una API web que necesita llamar a otra API web de nivel inferior, ambas protegidas mediante Azure AD B2C. Este escenario es habitual en los clientes nativos que tienen una API web back-end. Esto llama a un servicio en línea de Microsoft, como Graph API de Azure AD.

Este escenario de API web encadenadas puede admitirse mediante la concesión de credenciales de portador JWT de OAuth 2.0, también conocido como flujo "en nombre de".  Sin embargo, el flujo "en nombre de" no está implementado actualmente en Azure AD B2C.
