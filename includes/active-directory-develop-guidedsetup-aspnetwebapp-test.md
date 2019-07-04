---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 8795c9ab0a4dbb76327d0ead48ed33fb0cff9e86
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186924"
---
## <a name="test-your-code"></a>Prueba del código

Para probar la aplicación en Visual Studio, pulse **F5** para ejecutar el proyecto. El explorador se abre en la ubicación http://<span></span>localhost:{port}, donde verá el botón **Iniciar sesión con Microsoft**. Seleccione el botón para iniciar el proceso de inicio de sesión.

Cuando esté listo para realizar la prueba, use una cuenta profesional o educativa de Microsoft Azure Active Directory (Azure AD) o una cuenta de Microsoft personal (<span>live.</span>com, <span>outlook.</span>com) para iniciar sesión.

![Iniciar sesión con Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Iniciar sesión en la cuenta de Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Visualización de los resultados de la aplicación

Después de iniciar sesión, se redirige al usuario a la página principal del sitio web. Esta página principal es la dirección URL HTTPS especificada en la información de registro de la aplicación en el Portal de registro de aplicaciones de Microsoft. Asimismo, la página principal incluye un mensaje de bienvenida *"Hello \<User>,"* un vínculo para cerrar la sesión y otro para ver las notificaciones de usuario. El vínculo para las notificaciones de usuario conduce al controlador *Claims* que creó anteriormente.

### <a name="browse-to-see-the-users-claims"></a>Ir a las notificaciones de usuario

Para ver las notificaciones de usuario, seleccione el vínculo para ir a la vista del controlador que solo está disponible para los usuarios autenticados.

#### <a name="view-the-claims-results"></a>Ver los resultados de las notificaciones

Una vez en la vista del controlador, verá una tabla que contiene las propiedades básicas del usuario:

|Propiedad |Valor |DESCRIPCIÓN |
|---|---|---|
|**Nombre** |Nombre completo del usuario | Nombre y apellidos del usuario.
|**Nombre de usuario** |usuario<span>@domain.com</span> | Nombre de usuario que se usa para identificar al usuario.
|**Asunto** |Asunto |Cadena que identifica al usuario de forma exclusiva en la web.|
|**Id. de inquilino** |Guid | **Guid** que representa de forma única la organización de Azure AD del usuario.|

Además, podrá ver una tabla con todas las notificaciones que se encuentran en la solicitud de autenticación. Para obtener más información, consulte la [lista de notificaciones que se encuentran en un token de identificación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Probar el acceso a un método que tiene un atributo Authorize (opcional)

Para probar el acceso como usuario anónimo a un controlador protegido con el atributo `Authorize`, siga estos pasos:

1. Seleccione el vínculo para cerrar la sesión del usuario y complete el proceso de cierre de sesión.
2. En el explorador, escriba http://<span></span>localhost:{port}/claims para obtener acceso al controlador que está protegido con el atributo `Authorize`.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Resultados esperados después de obtener acceso a un controlador protegido

Se le pedirá que se autentique para poder usar la vista del controlador protegido.

## <a name="advanced-options"></a>Opciones avanzadas

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Proteger todo el sitio web
Para proteger todo el sitio web, vaya al archivo **Global.asax** y agregue el atributo `AuthorizeAttribute` al filtro `GlobalFilters` que se encuentra en el método `Application_Start`:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Restringir quién puede iniciar sesión a la aplicación

De forma predeterminada, cuando compile la aplicación que creó con esta guía, la aplicación podrá aceptar inicios de sesión de cuentas personales (como outlook.com, live.com y otras), así como cuentas profesionales y educativas de cualquier empresa u organización que se haya integrado con Azure Active Directory. Esta es una opción recomendada para las aplicaciones SaaS.

Existen varias opciones para restringir el acceso de los usuarios al proceso de inicio de sesión de la aplicación:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Opción 1: Restricción de los usuarios de una instancia de Active Directory de solo una organización para que inicien sesión en la aplicación (único inquilino)

Esta opción es un escenario común para las *aplicaciones de línea de negocio*: Si desea que la aplicación acepte inicios de sesión solo de cuentas que pertenezcan a una instancia concreta de Azure Active Directory (incluidas las *cuentas invitadas* de esa instancia), haga lo siguiente:

1. En el archivo **web.config**, reemplace el valor del parámetro `Tenant` de `Common` por el nombre de inquilino de la organización, por ejemplo, `contoso.onmicrosoft.com`.
2. A continuación, en la [clase OWIN Startup](#configure-the-authentication-pipeline), establezca el argumento `ValidateIssuer` en `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Opción 2: restringir el acceso a la aplicación a los usuarios de una lista específica de organizaciones

Puede restringir el acceso de inicio de sesión únicamente a aquellas cuentas de usuario que formen parte de una organización de Azure AD que se encuentre en una lista de organizaciones permitidas:
1. A continuación, en la [clase OWIN Startup](#configure-the-authentication-pipeline), establezca el argumento `ValidateIssuer` en `true`.
2. Establezca el valor del parámetro `ValidIssuers` en la lista de organizaciones permitidas.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Opción 3: Usar un método personalizado para validar emisores

Puede implementar un método personalizado para validar los emisores con el parámetro **IssuerValidator**. Para más información acerca de cómo usar este parámetro, puede encontrar más detalles en la [clase TokenValidationParameters](/previous-versions/visualstudio/dn464192(v=vs.114)).
