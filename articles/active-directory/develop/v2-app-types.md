---
title: Tipos de aplicaciones para v2.0 | Azure
description: Tipos de aplicaciones y escenarios admitidos por el punto de conexión v2.0 de Azure Active Directory.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e6ea0be84cc36ca6fa6547d01054599e37229d7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162941"
---
# <a name="application-types-for-v20"></a>Tipos de aplicaciones para v2.0

El punto de conexión v2.0 de Azure Active Directory (Azure AD) admite la autenticación de una variedad de arquitecturas de aplicaciones modernas, todas ellas basadas en los protocolos estándar del sector [OAuth 2.0 u OpenID Connect](active-directory-v2-protocols.md). En este artículo se describen los tipos de aplicaciones que puede crear mediante Azure AD v2.0, con independencia de su plataforma o idioma preferidos. La información de este artículo está diseñada para ayudarle a entender los escenarios de alto nivel antes de [empezar a trabajar con el código](v2-overview.md#getting-started).

> [!NOTE]
> No todas las características y escenarios de Azure Active Directory son compatibles con la versión 2.0 del punto de conexión. Para determinar si debe usar la versión 2.0 del punto de conexión, obtenga información sobre las [limitaciones de esta versión](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Conceptos básicos

Cada aplicación que use el punto de conexión v2.0 debe registrarla en el [portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com). El proceso de registro de la aplicación recopila y asigna algunos valores a la aplicación:

* Un **id. de aplicación** que identifica de forma única su aplicación
* Un **URI de redirección** que puede usarse para dirigir las respuestas de nuevo a la aplicación
* Algunos otros valores específicos de cada escenario.

Para más información, aprenda a [registrar una aplicación](quickstart-v2-register-an-app.md).

Una vez registrada, la aplicación se comunica con Azure AD mediante el envío de solicitudes al punto de conexión v2.0 de Azure AD. Proporcionamos bibliotecas y marcos de código abierto que controlan los detalles de estas solicitudes. También tiene la opción de implementar la lógica de autenticación por su cuenta mediante la creación de solicitudes a estos puntos de conexión:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>Aplicaciones de una página (JavaScript)

Muchas aplicaciones modernas tienen un front-end de aplicación de una página escrito principalmente en JavaScript. A menudo, se escribe con un marco como AngularJS, Ember.js o Durandal.js. El punto de conexión v2.0 de Azure AD admite estas aplicaciones mediante el [flujo implícito de OAuth 2.0](v2-oauth2-implicit-grant-flow.md).

En este flujo, la aplicación recibe tokens directamente del punto de conexión de autorización de v2.0, sin necesidad de realizar ningún intercambio de servidor a servidor. Toda la lógica de autenticación y el control de sesiones tiene lugar por completo en el cliente de JavaScript, sin redireccionamientos de página adicionales.

![Flujo de autenticación implícita](./media/v2-app-types/convergence_scenarios_implicit.png)

Para ver este escenario en acción, pruebe uno de los ejemplos de código de aplicación de una única página en nuestra sección de [introducción a v2.0](v2-overview.md#getting-started).

## <a name="web-apps"></a>Aplicaciones web

En las aplicaciones web (. NET, PHP, Java, Ruby, Python, Node) a las que el usuario accede a través de un explorador, puede usar [OpenID Connect](active-directory-v2-protocols.md) para el inicio de sesión de usuario. En OpenID Connect, la aplicación web recibe un identificador de token. Un token de id. es un token de seguridad que comprueba la identidad del usuario y proporciona información sobre el usuario en forma de notificaciones:

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Encontrará más información sobre los diferentes tipos de token que se usan en el punto de conexión v2.0 en la [referencia de token de acceso](access-tokens.md) y en la [referencia de `id_token`](id-tokens.md).

En las aplicaciones de servidor web, el flujo de autenticación de inicio de sesión realiza estos pasos de alto nivel:

![Flujo de autenticación de aplicaciones web](./media/v2-app-types/convergence_scenarios_webapp.png)

Puede confirmar la identidad del usuario mediante la validación del token de id. con una clave de firma pública recibida por el punto de conexión v2.0. Se establece una cookie de sesión, que puede usarse para identificar al usuario en las sucesivas solicitudes de página.

Para ver este escenario en acción, pruebe uno de los ejemplos de código de inicio de sesión de aplicación web en nuestra sección de [introducción a v2.0](v2-overview.md#getting-started).

Además del inicio de sesión sencillo, una aplicación web de servidor podría tener la necesidad de acceder a otros servivio web, como una API de REST. En este caso, la aplicación de servidor web participa en un flujo combinado de OpenID Connect y OAuth 2.0, mediante el [flujo de código de autorización de OAuth 2.0](active-directory-v2-protocols.md). Para más información sobre este escenario, lea acerca de cómo [comenzar con aplicaciones web y API web](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>API web

Puede usar el punto de conexión v2.0 para proteger los servicios web, como la API web RESTful de su aplicación. En lugar de tokens de id. y cookies de sesión, una API web usa un token de acceso de OAuth 2.0 para proteger sus datos y para autenticar las solicitudes entrantes. El llamador de una API web anexa un token de acceso en el encabezado de autorización de una solicitud HTTP, como esta:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

La API web usa el token de acceso para comprobar la identidad del llamador de API y extraer información sobre el llamador de notificaciones que se codifican en dicho token. Encontrará más información sobre los diferentes tipos de token que se usan en el punto de conexión v2.0 en la [referencia de token de acceso](access-tokens.md) y en la [referencia de `id_token`](id-tokens.md).

Una API web puede ofrecer a los usuarios la capacidad para administrar la participación/no participación de ciertas funcionalidades o datos mediante la exposición de permisos, conocidos también como [ámbitos](v2-permissions-and-consent.md). Para que una aplicación de llamada adquiera permiso para un ámbito, el usuario debe consentir el ámbito durante un flujo. El punto de conexión v2.0 solicita al usuario permiso y luego registra los permisos en todos los tokens de acceso que recibe la API web. La API web valida los tokens de acceso que recibe en cada llamada y realiza comprobaciones de autorización.

Una API web puede recibir tokens de acceso de todos los tipos de aplicaciones, incluidas las aplicaciones de servidor web, aplicaciones móviles y de escritorio, aplicaciones de una página, demonios del lado del servidor e incluso otras API web. El flujo de alto nivel de una API web se parece a este:

![Flujo de autenticación de API web](./media/v2-app-types/convergence_scenarios_webapi.png)

Para aprender a proteger una API web con tokens de acceso de OAuth2, consulte los ejemplos de código de API web en nuestra sección de [introducción a v2.0](v2-overview.md#getting-started).

En muchos casos, las API web también tienen que realizar solicitudes salientes a otras API web de bajada protegidas por Azure Active Directory. Para ello, las API web pueden aprovechar las ventajas del flujo **en nombre de** de Azure AD, que permite a la API web intercambiar un token de acceso entrante por otro token de acceso que se usará en las solicitudes salientes. El flujo "en nombre de" del punto de conexión v2.0 se describe [aquí con mayor detalle](v2-oauth2-on-behalf-of-flow.md).

## <a name="mobile-and-native-apps"></a>Aplicaciones móviles y nativas

Las aplicaciones instaladas en un dispositivo, como las aplicaciones móviles y de escritorio, suelen necesitar el acceso a servicios back-end o a las API web que almacenan datos y realizan varias funciones en nombre del usuario. Estas aplicaciones pueden agregar el inicio de sesión y la autorización a los servicios back-end mediante el [flujo de código de autorización de OAuth 2.0](v2-oauth2-auth-code-flow.md).

En este flujo, la aplicación recibe un código de autorización del punto de conexión v2.0 cuando el usuario inicia sesión. El código de autorización representa el permiso de la aplicación para llamar a servicios de back-end en nombre del usuario que inició la sesión. La aplicación podrá intercambiar el código de autorización en segundo plano para un token de acceso de OAuth 2.0 y un token de actualización. La aplicación puede usar el token de acceso para autenticar las API web en las solicitudes HTTP y utilizar el token de actualización para obtener nuevos tokens de acceso cuando expiren los antiguos.

![Flujo de autenticación de la aplicación nativa](./media/v2-app-types/convergence_scenarios_native.png)

## <a name="daemons-and-server-side-apps"></a>Demonios y aplicaciones de servidor

Las aplicaciones que contienen procesos de larga duración o que funcionan sin la interacción con un usuario también necesitan un modo de acceder a recursos protegidos, como las API web. Estas aplicaciones pueden autenticarse y obtener tokens mediante la identidad de la aplicación, en lugar de una identidad delegada del usuario, con el flujo de credenciales de cliente de OAuth 2.0. Puede demostrar la identidad de la aplicación mediante un certificado o secreto de cliente. Para obtener más información, consulte [Authenticating to Azure AD in daemon apps with certificates](https://azure.microsoft.com/resources/samples/active-directory-dotnet-daemon-certificate-credential/) (Autenticación en Azure AD en aplicaciones demonio con certificados).

En este flujo, la aplicación interactúa directamente con el punto de conexión `/token` para obtener los puntos de conexión:

![Flujo de autenticación de aplicación de demonio](./media/v2-app-types/convergence_scenarios_daemon.png)

Para compilar una aplicación demonio, consulte la [documentación de credenciales de cliente](v2-oauth2-client-creds-grant-flow.md) en nuestra sección de introducción o pruebe una [aplicación de ejemplo de .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
