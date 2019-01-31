---
title: Comparación del punto de conexión v2.0 con el punto de conexión v1.0 de Azure AD| Microsoft Docs
description: Conozca las diferencias entre el punto de conexión v2.0 y el punto de conexión v1.0 de Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2018
ms.author: celested
ms.reviewer: hirsin, andret, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, dadobali, negoe
ms.custom: aaddev
ms.openlocfilehash: 82984687e1d51598b87c85564ebda74bf4b2daa2
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098054"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Comparación del punto de conexión v2.0 con el punto de conexión v1.0 de Azure AD

Al desarrollar una nueva aplicación, es importante conocer las diferencias entre los puntos de conexión v1.0 y v2.0 de Azure Active Directory (Azure AD). En este artículo se tratan las principales diferencias entre los puntos de conexión y algunas limitaciones existentes para el punto de conexión v2.0.

> [!NOTE]
> No todas las características y escenarios de Azure AD son compatibles con la versión 2.0 del punto de conexión. Para determinar si debe usar el punto de conexión v2.0, lea acerca de las [limitaciones de v2.0](#limitations).

## <a name="who-can-sign-in"></a>Quién puede iniciar sesión

![Quién puede iniciar sesión con los puntos de conexión v1.0 y v2.0](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* El punto de conexión v1.0 solo permite iniciar sesión en la aplicación con cuentas profesionales y educativas (Azure AD).
* El punto de conexión v2.0 permite iniciar sesión con cuentas profesionales y educativas de Azure AD y con cuentas Microsoft personales, como hotmail.com, outlook.com y msn.com.
* Tanto los puntos de conexión v1.0 como v2.0 aceptan inicios de sesión de *[usuarios invitados](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* de un directorio de Azure AD para las aplicaciones configuradas como *[inquilino único](single-and-multi-tenant-apps.md)* o para las aplicaciones configuradas como *multiinquilino* para el punto de conexión específico del inquilino (`https://login.microsoftonline.com/{TenantId_or_Name}`).

El punto de conexión v2.0 le permite escribir aplicaciones que aceptan inicios de sesión desde cuentas Microsoft personales y cuentas profesionales y educativas. Esto le permite escribir la aplicación con independencia absoluta de la cuenta. Por ejemplo, si su aplicación llama a [Microsoft Graph](https://graph.microsoft.io), las cuentas profesionales tendrán a su disposición algunos datos y funcionalidades adicionales, como los sitios de SharePoint o datos de directorio. Pero, para muchas acciones, como [leer correo de un usuario](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_messages), el mismo código puede acceder al correo electrónico tanto para cuentas personales como profesionales y educativas.

En el caso del punto de conexión v2.0, puede usar la Biblioteca de autenticación de Microsoft para obtener acceso a entornos educativos, de consumidor y de empresa. El punto de conexión v1.0 de Azure AD acepta inicios de sesión solo desde cuentas profesionales y educativas.

## <a name="incremental-and-dynamic-consent"></a>Consentimiento incremental y dinámico

Las aplicaciones que usan el punto de conexión v1.0 de Azure AD tienen que especificar los permisos de OAuth 2.0 necesarios de antemano, por ejemplo:

![Interfaz de usuario de registro de permisos](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Los permisos establecidos directamente en el registro de la aplicación son **estáticos**. Aunque los permisos estáticos de la aplicación definidos en Azure Portal mantenían un código ordenado y sencillo, presenta algunos problemas para los desarrolladores:

* Una aplicación tiene que solicitar todos los permisos que podría necesitar alguna vez tras el primer inicio de sesión del usuario. Esto puede originar una lista larga de permisos, lo que desanimaría a los usuarios finales de aprobar el acceso de la aplicación en el inicio de sesión inicial.

* La aplicación tiene que conocer por adelantado todos los recursos a los que va a tener acceso alguna vez. Era difícil crear aplicaciones que podrían tener acceso a un número arbitrario de recursos.

Con el punto de conexión v2.0, puede ignorar los permisos estáticos definidos en la información de registro de la aplicación en Azure Portal y solicitar permisos incrementales en su lugar, lo que significa solicitar un conjunto mínimo de permisos por adelantado y acumular más con el tiempo a medida que el cliente usa características de la aplicación. Para ello, puede especificar los ámbitos que la aplicación necesita en cualquier momento incluyendo los nuevos ámbitos en el parámetro `scope` cuando solicite un token de acceso. No es necesario predefinirlos en la información de registro de la aplicación. Si el usuario aún no ha dado su consentimiento a los ámbitos nuevos que se agregan a la solicitud, se le pedirá que dé su consentimiento solo a los nuevos permisos. Para obtener más información, consulte sobre [permisos, consentimiento y ámbitos](v2-permissions-and-consent.md).

Permitir que una aplicación solicite permisos dinámicamente mediante el parámetro `scope` da a los desarrolladores un control total sobre la experiencia del usuario. También puede adelantar la experiencia de consentimiento y pedir todos los permisos en una solicitud de autorización inicial. Si su aplicación requiere un gran número de permisos, puede elegir recopilarlos del usuario de forma incremental, a medida que intentan usar determinadas características de la aplicación con el tiempo.

El consentimiento del administrador que se realiza en nombre de una organización sigue requriendo los permisos estáticos registrados para la aplicación, por lo que debería establecer esos permisos para las aplicaciones en el portal de registro de aplicaciones si necesita que un administrador dé su consentimiento en nombre de toda la organización. Esto reduce los ciclos que el administrador de la organización necesita para instalar la aplicación.

## <a name="scopes-not-resources"></a>Ámbitos, no recursos

Las aplicaciones que usan el punto de conexión v1.0 pueden comportarse como un **recurso** o como un destinatario de tokens. Un recurso puede definir varios **ámbitos** o **oAuth2Permissions** que comprende, lo que permite a las aplicaciones cliente solicitar tokens para ese recurso para un conjunto determinado de ámbitos. Piense en Graph API de Azure AD como ejemplo de un recurso:

* Identificador de recursos o `AppID URI`: `https://graph.windows.net/`
* Los ámbitos, o `oAuth2Permissions`: `Directory.Read`, `Directory.Write`, etc.

Todo esto se cumple para el punto de conexión v2.0. Una aplicación todavía se puede comportar como recurso, definir ámbitos y ser identificada por un URI. Las aplicaciones cliente todavía pueden solicitar acceso a esos ámbitos. Sin embargo, ha cambiado la manera en que un cliente solicita esos permisos. 

Para el punto de conexión v1.0, una solicitud de autorización de OAuth 2.0 para Azure AD podría haber tenido un aspecto similar al siguiente:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Aquí, el parámetro **resource** indicaba para qué recurso la aplicación cliente solicitaba autorización. Azure AD calculaba los permisos requeridos por la aplicación en función de la configuración estática en Azure Portal y emitía tokens en consecuencia. 

Para las aplicaciones que usan el punto de conexión v2.0, la misma solicitud de autorización de OAuth 2.0 tendrá el siguiente aspecto:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Aquí, el parámetro **scope** indica para qué recursos y permisos está solicitando autorización la aplicación. El recurso deseado sigue estando presente en la solicitud; está incluido en cada uno de los valores del parámetro scope. El uso del parámetro scope de esta manera permite que el punto de conexión v2.0 respete en mayor medida la especificación de OAuth 2.0 y se alinee más estrechamente con prácticas comunes del sector. También permite que las aplicaciones realicen un [consentimiento incremental](#incremental-and-dynamic-consent): solo solicita permisos cuando la aplicación los requiere, en lugar de hacerlo por adelantado.

## <a name="well-known-scopes"></a>Ámbitos conocidos

### <a name="offline-access"></a>Acceso sin conexión

Es posible que las aplicaciones que usan el punto de conexión v2.0 requieran el uso de un nuevo permiso conocido en las aplicaciones: el ámbito `offline_access`. Todas las aplicaciones deberán solicitar este permiso si necesitan tener acceso a los recursos en nombre de un usuario durante un período de tiempo prolongado, incluso cuando es posible que el usuario no haya estado usando la aplicación activamente. El ámbito `offline_access` se mostrará al usuario en cuadros de diálogo de consentimiento como **Obtener acceso a los datos en cualquier momento**, que el usuario debe aceptar. Solicitar el permiso `offline_access` permitirá a su aplicación web recibir refresh_tokens de OAuth 2.0 desde el extremo de v2.0. Los token de actualización son de larga duración y se pueden intercambiar por nuevos tokens de actualización de OAuth 2.0 para períodos de acceso más prolongados.

Si la aplicación no solicita el ámbito `offline_access`, no recibirá tokens de actualización. Esto significa que cuando se canjea un código de autorización en el flujo del código de autorización de OAuth 2.0, solo se recibirá un token de acceso desde el punto de conexión `/token`. Ese token de acceso sigue siendo válido durante un breve período de tiempo (normalmente una hora), pero finalmente expirará. En ese momento, la aplicación tendrá que redirigir el usuario de nuevo al punto de conexión `/authorize` para recuperar un nuevo código de autorización. Durante esta redirección, es posible o no que el usuario necesite escribir sus credenciales de nuevo o volver a dar el consentimiento a permisos, según el tipo de aplicación.

Para más información sobre OAuth 2.0, `refresh_tokens` y `access_tokens`, consulte la [referencia del protocolo v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, perfil y correo electrónico

Tradicionalmente, el flujo más básico de inicio de sesión de OpenID Connect en Azure AD ofrecería gran cantidad de información sobre el usuario en el *id_token* resultante. Las notificaciones de un id_token pueden incluir el nombre de usuario, el nombre de usuario preferido, la dirección de correo electrónico, el id. de objeto, etc.

La información a la que el ámbito `openid` permite acceder a la aplicación está restringida. El ámbito `openid` solo permitirá que el usuario inicie sesión en la aplicación y que esta reciba un identificador específico de la aplicación para el usuario. Si quiere obtener información personal acerca del usuario en la aplicación, esta tendrá que solicitar permisos adicionales al usuario. Dos nuevos ámbitos, `email` y `profile`, le permitirán solicitar permisos adicionales.

* El ámbito `email` permite que la aplicación acceda a la dirección de correo electrónico principal del usuario a través de la notificación `email` en id_token, suponiendo que el usuario tiene una dirección de correo electrónico direccionable. 
* El ámbito `profile` ofrece a la aplicación acceso a toda la demás información básica sobre el usuario, como su nombre, el nombre de usuario preferido, el identificador de objeto y demás en el id_token.

Estos ámbitos le permiten codificar la aplicación en un modo de divulgación mínima, así que puede pedir al usuario solo el conjunto de información que la aplicación necesita para hacer su trabajo. Para obtener más información sobre estos ámbitos, consulte [la referencia de los ámbitos de la versión 2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Notificaciones de token

El punto de conexión de v2.0 emite un conjunto más pequeño de notificaciones en sus tokens de manera predeterminada para mantener el tamaño reducido de las cargas. Si tiene aplicaciones y servicios que tienen una dependencia de una notificación concreta en un token de v1.0 que ya no se proporciona de forma predeterminada en un token de v2.0, considere el uso de la característica [notificaciones opcionales](active-directory-optional-claims.md) para incluir esa notificación.

## <a name="limitations"></a>Limitaciones

Hay que tener en cuenta algunas restricciones cuando se usa el punto de conexión v2.0.

Cuando compile aplicaciones que se integran con la plataforma de identidad de Microsoft, debe decidir si los protocolos de autenticación y el punto de conexión v2.0 satisfacen sus necesidades. La plataforma y el punto de conexión v1.0 se siguen admitiendo totalmente y, en algunos aspectos, tienen más características que la versión 2.0. Sin embargo, la versión 2.0 [presenta ventajas importantes](azure-ad-endpoint-comparison.md) para los desarrolladores.

Esta es nuestra recomendación simplificada para desarrolladores en este momento:

* Si desea o necesita admitir cuentas Microsoft personales en la aplicación, o está escribiendo una nueva aplicación, use v2.0. Pero antes, asegúrese de comprender las limitaciones que se analizan en este artículo.
* Si va a migrar o actualizar una aplicación que se basa en SAML, no puede usar v2.0. En su lugar, consulte la [guía de v1.0](v1-overview.md).

Desarrollaremos el punto de conexión de la versión 2.0 para eliminar las restricciones que mencionamos en este artículo, por lo que siempre debe usar este punto de conexión. Mientras tanto, use este artículo para determinar si el punto de conexión v2.0 es correcto para usted. Actualizaremos este artículo constantemente para reflejar el estado actual del punto de conexión de la versión 2.0. Consúltelo de nuevo para volver a evaluar los requisitos en relación con las funcionalidades de v2.0.

### <a name="restrictions-on-app-registrations"></a>Restricciones en los registros de aplicaciones

Para cada aplicación que desee integrar con el punto de conexión v2.0, puede crear un registro de aplicación en el [portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Como alternativa, puede registrar una aplicación con la experiencia [**Registros de aplicaciones (versión preliminar)**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) en Azure Portal. Las aplicaciones de cuenta Microsoft existentes no son compatibles con el portal de la versión preliminar, pero están todas las aplicaciones AAD, con independencia de dónde o cuándo se registraron. 

Los registros de aplicaciones que admiten cuentas profesionales y educativas y cuentas personales tienen las siguientes limitaciones:

* Solo se permiten dos secretos de aplicación por cada identificador de aplicación.
* Una aplicación que no se registró en un inquilino solo puede administrarse por la cuenta que la registró. No se puede compartir con otros desarrolladores. Este es el caso de la mayoría de aplicaciones que se registraron con una cuenta Microsoft personal en el portal de registros para aplicaciones. Si desea compartir el registro de la aplicación con varios desarrolladores, registre la aplicación en un inquilino mediante la sección **Registros de aplicaciones (versión preliminar)** de Azure Portal.
* Existen varias restricciones para el formato de la URL de redireccionamiento permitido. Para más información sobre las URL de redireccionamiento, consulte la sección siguiente.

### <a name="restrictions-on-redirect-urls"></a>Restricciones en las URL de redireccionamiento

Las aplicaciones registradas para v2.0 están restringidas a un conjunto limitado de valores de URL de redireccionamiento. La URL de redireccionamiento de aplicaciones y servicios web debe comenzar por el esquema `https`, y todos los valores de URL de redireccionamiento deben compartir un único dominio DNS.  El sistema de registro compara el nombre DNS completo de la URL de redireccionamiento existente con el nombre DNS de la URL de redireccionamiento que va a agregar. `http://localhost` también se admite como una dirección URL de redireccionamiento.  

La solicitud para agregar el nombre DNS presentará un error si se cumple alguna de las condiciones siguientes:  

* El nombre DNS completo de la nueva URL de redireccionamiento no coincide con el nombre DNS de la URL de redireccionamiento existente.
* El nombre DNS completo de la nueva URL de redireccionamiento no es un subdominio de la URL de redireccionamiento existente.

#### <a name="example-1"></a>Ejemplo 1

Si la aplicación tiene una dirección URL de redireccionamiento de `https://login.contoso.com`, puede agregar una dirección URL de redireccionamiento donde el nombre DNS coincide exactamente, como se muestra en el ejemplo siguiente:

`https://login.contoso.com/new`

O bien, puede hacer referencia a un subdominio DNS de login.contoso.com, tal como se muestra en el ejemplo siguiente:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Ejemplo 2

Si desea que una aplicación tenga `login-east.contoso.com` y `login-west.contoso.com` como direcciones URL de redireccionamiento, debe agregar las URL de redireccionamiento en el orden siguiente:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Puede agregar las dos últimas porque son subdominios de la primera URL de redireccionamiento, contoso.com. Esta limitación se eliminará en una próxima versión.

Además, tenga en cuenta que solo puede tener 20 URL de respuesta para una aplicación en particular; este límite se aplica a todos los tipos de aplicaciones que admite el registro (SPA, cliente nativo, aplicación web y servicio).  

Para obtener información sobre cómo registrar una aplicación para su uso con v2.0, consulte estas guías de inicio rápido:

* [Register an app using the Application Registration Portal](quickstart-v2-register-an-app.md) (Registro de una aplicación con el portal de registro de aplicaciones)
* [Register an app using the App registrations (Preview) experience](quickstart-register-app.md) (Registro de una aplicación con los registros de aplicaciones [versión preliminar])

### <a name="restrictions-on-libraries-and-sdks"></a>Restricciones en las bibliotecas y SDK

En este momento, la compatibilidad del punto de conexión v2.0 con las bibliotecas es limitada. Si desea usar el punto de conexión v2.0 en una aplicación de producción, tiene las opciones siguientes:

* Si está creando una aplicación web, puede utilizar sin ningún riesgo nuestro software intermedio del lado del servidor disponible con carácter general, para realizar el inicio de sesión y la validación de tokens. Incluye el software intermedio OWIN OpenID Connect para ASP.NET y el complemento NodeJS Passport. Para ver ejemplos de código que usan middleware de Microsoft, consulte la sección [Introducción a v2.0](v2-overview.md#getting-started).
* Si crea una aplicación de escritorio o para dispositivos móviles, puede usar una de las bibliotecas de autenticación de Microsoft (MSAL) de versión preliminar. Estas bibliotecas están en una versión de versión preliminar compatible con producción, por lo que su uso en aplicaciones de producción es seguro. Puede obtener más información sobre los términos y condiciones de la versión preliminar y las bibliotecas disponibles en la [referencia de bibliotecas de autenticación](reference-v2-libraries.md).
* En el caso de otras plataformas no cubiertas por las bibliotecas de Microsoft, pueden integrarse con el punto de conexión v2.0 enviando y recibiendo mensajes de protocolo directamente en el código de su aplicación. Los protocolos v2.0 OpenID Connect y OAuth [se documentan explícitamente](active-directory-v2-protocols.md) para ayudarle a realizar dicha integración.
* Por último, puede usar las bibliotecas de código abierto de OpenID Connect y OAuth para integrarse con el punto de conexión v2.0. El punto de conexión v2.0 debe ser compatible con muchas bibliotecas de código abierto de los protocolos sin cambios. La disponibilidad de estos tipos de bibliotecas varía según el lenguaje y la plataforma. Los sitios web [OpenID Connect](https://openid.net/connect/) y [OAuth 2.0](https://oauth.net/2/) mantienen una lista de las implementaciones populares. Para más información, consulte [Azure Active Directory v2.0 y bibliotecas de autenticación](reference-v2-libraries.md) y la lista de bibliotecas de cliente de código abierto y los ejemplos que se probaron con el punto de conexión v2.0.
* Como referencia, el punto de conexión `.well-known` para el punto de conexión v2.0 común es `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`. Reemplace `common` con el identificador de inquilino para obtener los datos específicos para el inquilino.  

### <a name="protocol-changes"></a>Cambios en el protocolo

El punto de conexión v2.0 no admite SAML ni WS-Federation; solo admite OpenID Connect y OAuth 2.0.  Los cambios importantes en los protocolos OAuth 2.0 desde el punto de conexión v1.0 son: 

* La notificación `email` se devuelve si se configura una notificación opcional **o** scope=email se ha especificado en la solicitud. 
* El parámetro `scope` ahora se admite en lugar del parámetro `resource`.  
* Muchas respuestas se han modificado para que sean más compatibles con la especificación OAuth 2.0; por ejemplo, devolver correctamente `expires_in` como int en lugar de una cadena.  

Para comprender mejor el alcance de la funcionalidad de protocolo que se admite en el punto de conexión v2.0, consulte nuestra [referencia a los protocolos OpenID Connect y OAuth 2.0](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>Restricciones de SAML

Si ha usado la biblioteca de autenticación de Active Directory (ADAL) en aplicaciones de Windows, es posible que haya aprovechado la autenticación integrada de Windows, que usa la concesión de aserción de Lenguaje de marcado de aserción de seguridad (SAML). Con esta concesión, los usuarios de los inquilinos de Azure AD federado pueden autenticarse de manera silenciosa con su instancia local de Active Directory sin escribir las credenciales. El punto de conexión v2.0 no admite la concesión de aserción de SAML.
