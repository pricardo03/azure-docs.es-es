---
title: Comparación del punto de conexión v2.0 con el punto de conexión v1.0 de Azure AD | Microsoft Docs
description: Conozca las diferencias entre el punto de conexión v2.0 y el punto de conexión v1.0 de Azure AD.
services: active-directory
documentationcenter: ''
author: andretms
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: andret
ms.reviewer: hirsin, celested
ms.custom: aaddev
ms.openlocfilehash: 02c7edc84d2ac3a91c33d8f266d022db5cd5cb40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948972"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-v10-endpoint"></a>Comparación del punto de conexión v2.0 con el punto de conexión v1.0 de Azure AD

Al desarrollar una nueva aplicación, es importante conocer las diferencias entre los puntos de conexión v1.0 y v2.0. A continuación se muestran las diferencias principales, así como algunas limitaciones del punto de conexión v2.0.

> [!NOTE]
> No todas las características ni escenarios de Azure AD son compatibles con el punto de conexión v2.0. Para determinar si debe usar el punto de conexión v2.0, lea acerca de las [limitaciones de v2.0](#limitations).

## <a name="who-can-sign-in"></a>Quién puede iniciar sesión

![Quién puede iniciar sesión con los puntos de conexión v1.0 y v2.0](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* El punto de conexión v1.0 solo permite iniciar sesión en la aplicación con cuentas profesionales y educativas (Azure AD).

* El punto de conexión v2.0 iniciar sesión con cuentas profesionales y educativas de Azure Active Directory y con cuentas personales (MSA) (hotmail.com, outlook.com, msn.com).

* Tanto los puntos de conexión v1.0 como v2.0 aceptan inicios de sesión de *[usuarios invitados](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* de un directorio de Azure AD para las aplicaciones configuradas como *[inquilino único](single-and-multi-tenant-apps.md)* o para las aplicaciones configuradas como *multiinquilino* para el punto de conexión específico del inquilino (`https://login.microsoftonline.com/{TenantId_or_Name}`).

El punto de conexión v2.0 le permite escribir aplicaciones que aceptan el inicio de sesión desde cuentas tanto personales y profesionales y educativas, lo que le ofrece la posibilidad de escribir una aplicación completamente independiente de la cuenta. Por ejemplo, si su aplicación llama a [Microsoft Graph](https://graph.microsoft.io), las cuentas profesionales tendrán a su disposición algunos datos y funcionalidades adicionales, como los sitios de SharePoint o datos de Directory. Pero, para muchas acciones, como [leer correo de un usuario](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), el mismo código puede acceder al correo electrónico tanto para cuentas personales como profesionales y educativas.

En el caso del punto de conexión v2.0, puede usar una sola biblioteca (MSAL) para obtener acceso a entornos educativos, de consumidor y de empresa.

 El punto de conexión v1.0 de Azure AD acepta inicios de sesión solo desde cuentas profesionales y educativas.

## <a name="incremental-and-dynamic-consent"></a>Consentimiento incremental y dinámico

Las aplicaciones que usan el punto de conexión v1.0 de Azure AD tienen que especificar los permisos de OAuth 2.0 necesarios de antemano, por ejemplo:

![Interfaz de usuario de registro de permisos](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

Los permisos establecidos directamente en el registro de la aplicación son **estáticos**. Aunque los permisos estáticos de la aplicación se definían en Azure Portal y mantenían un código ordenado y sencillo, presentaba algunos problemas para los desarrolladores:

* Una aplicación tiene que conocer todos los permisos que va a consumir alguna vez durante la creación de la aplicación. Agregar permisos con el tiempo era un proceso difícil.

* La aplicación tiene que conocer por adelantado todos los recursos a los que va a tener acceso alguna vez. Era difícil crear aplicaciones que podrían tener acceso a un número arbitrario de recursos.

* Una aplicación tiene que solicitar todos los permisos que podría necesitar alguna vez tras el primer inicio de sesión del usuario. En algunos casos, esto llevaría a una lista larga de permisos, lo que desanimaría a los usuarios finales de aprobar el acceso de la aplicación en el inicio de sesión inicial.

Con el punto de conexión v2.0, puede omitir los permisos definidos estáticamente en la información de registro de la aplicación en Azure Portal y especificar los permisos que la aplicación necesita **dinámicamente** en tiempo de ejecución, durante el uso normal de la aplicación e independientemente de los permisos definidos estáticamente en la información de registro de la aplicación.

Para ello, puede especificar los ámbitos que la aplicación necesita en un momento dado en tiempo de aplicación. Incluya los nuevos ámbitos en el parámetro `scope` cuando solicite un token de acceso. No es necesario predefinirlos en la información de registro de la aplicación.

Si el usuario aún no ha dado su consentimiento a los ámbitos nuevos que se agregan a la solicitud, se le pedirá que dé su consentimiento solo a los nuevos permisos. Para obtener más información, puede leer sobre [permisos, consentimiento y ámbitos](v2-permissions-and-consent.md).

Permitir que una aplicación solicite permisos dinámicamente mediante el parámetro `scope` da a los desarrolladores un control total sobre la experiencia del usuario. Si lo desea, también puede elegir adelantar la experiencia de consentimiento y pedir todos los permisos en una solicitud de autorización inicial. O bien, si su aplicación requiere un gran número de permisos, puede elegir recopilarlos del usuario de forma incremental, a medida que intentan usar determinadas características de la aplicación con el tiempo.

Tenga en cuenta que consentimiento del administrador que se realiza en nombre de una organización sigue usando los permisos estáticos registrados para la aplicación, por lo que se recomienda que establezca esos permisos para las aplicaciones con el punto de conexión v2.0 si necesita que un administrador dé su consentimiento en nombre de toda la organización. Esto reduce los ciclos que el administrador de la organización necesita para instalar la aplicación.

## <a name="scopes-not-resources"></a>Ámbitos, no recursos

Las aplicaciones que usan el punto de conexión v1.0 pueden comportarse como un **recurso** o como un destinatario de tokens. Un recurso puede definir varios **ámbitos** o **oAuth2Permissions** que comprende, lo que permite a las aplicaciones cliente solicitar tokens para ese recurso para un conjunto determinado de ámbitos. Piense en Graph API de Azure AD como ejemplo de un recurso:

* Identificador de recursos o `AppID URI`: `https://graph.windows.net/`

* Los ámbitos, o `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, etc.

Todo esto se cumple para el punto de conexión v2.0. Una aplicación todavía se puede comportar como recurso, definir ámbitos y ser identificada por un URI. Las aplicaciones cliente todavía pueden solicitar acceso a esos ámbitos. Sin embargo, ha cambiado la manera en que un cliente solicita esos permisos. Para el punto de conexión v1.0, una solicitud de autorización de OAuth 2.0 para Azure AD podría haber tenido un aspecto similar al siguiente:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

donde el parámetro **resource** indicaba para qué recurso la aplicación cliente solicitaba autorización. Azure AD calculaba los permisos requeridos por la aplicación en función de la configuración estática en Azure Portal y emitía tokens en consecuencia. Para las aplicaciones que usan el punto de conexión v2.0, la misma solicitud de autorización de OAuth 2.0 tendrá el siguiente aspecto:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Donde el parámetro **scope** indica para qué recursos y permisos está solicitando autorización la aplicación. El recurso deseado sigue estando presente en la solicitud; simplemente está incluido en cada uno de los valores del parámetro scope. El uso del parámetro scope de esta manera permite que el punto de conexión v2.0 respete en mayor medida la especificación de OAuth 2.0 y se alinee más estrechamente con prácticas comunes del sector. También permite que las aplicaciones realicen un [consentimiento incremental](#incremental-and-dynamic-consent), tal y como se describió anteriormente.

## <a name="well-known-scopes"></a>Ámbitos conocidos

### <a name="offline-access"></a>Acceso sin conexión

Es posible que las aplicaciones que usan el punto de conexión v2.0 requieran el uso de un nuevo permiso conocido en las aplicaciones: el ámbito `offline_access`. Todas las aplicaciones deberán solicitar este permiso si necesitan tener acceso a los recursos en nombre de un usuario durante un período de tiempo prolongado, incluso cuando es posible que el usuario no haya estado usando la aplicación activamente. El ámbito `offline_access` se mostrará al usuario en cuadros de diálogo de consentimiento como **Obtener acceso a los datos en cualquier momento**, que el usuario debe aceptar. Solicitar el permiso `offline_access` permitirá a su aplicación web recibir refresh_tokens de OAuth 2.0 desde el extremo de v2.0. Los token de actualización son de larga duración y se pueden intercambiar por nuevos tokens de actualización de OAuth 2.0 para períodos de acceso más prolongados.

Si la aplicación no solicita el ámbito `offline_access`, no recibirá tokens de actualización. Esto significa que cuando se canjea un código de autorización en el flujo del código de autorización de OAuth 2.0, solo se recibirá un token de acceso desde el punto de conexión `/token`. Ese token de acceso seguirá siendo válido durante un breve período de tiempo (normalmente una hora), pero finalmente expirará. En ese momento, la aplicación tendrá que redirigir el usuario de nuevo al punto de conexión `/authorize` para recuperar un nuevo código de autorización. Durante esta redirección, es posible o no que el usuario necesite escribir sus credenciales de nuevo o volver a dar el consentimiento a permisos, según el tipo de aplicación.

Para más información sobre OAuth 2.0, `refresh_tokens` y `access_tokens`, consulte la [referencia del protocolo v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>OpenID, perfil y correo electrónico

Tradicionalmente, el flujo más básico de inicio de sesión de OpenID Connect en Azure AD ofrecería gran cantidad de información sobre el usuario en el *id_token* resultante. Las notificaciones de un *id_token* pueden incluir el nombre de usuario, el nombre de usuario preferido, la dirección de correo electrónico, el identificador de objeto, etc.

La información a la que el ámbito `openid` permite acceder a la aplicación está restringida. El ámbito `openid` solo permitirá que el usuario inicie sesión en la aplicación y que esta reciba un identificador específico de la aplicación para el usuario. Si quiere obtener información personal acerca del usuario en la aplicación, esta tendrá que solicitar permisos adicionales al usuario. Dos nuevos ámbitos, `email` y `profile`, le permitirán solicitar permisos adicionales.

El ámbito `email` permite que la aplicación acceda a la dirección de correo electrónico principal del usuario a través de la notificación `email` en id_token. El ámbito `profile` ofrece a la aplicación acceso a toda la demás información básica sobre el usuario: su nombre, el nombre de usuario preferido, identificador de objeto y demás.

Esto le permite codificar la aplicación en un modo de divulgación mínima, puede pedir al usuario solo el conjunto de información que la aplicación necesita para hacer su trabajo. Para obtener más información sobre estos ámbitos, consulte [la referencia de los ámbitos de la versión 2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Notificaciones de token

Las notificaciones en los tokens emitidos por el punto de conexión v2.0 no serán idénticas a los tokens emitidos por los puntos de conexión de Azure AD generalmente disponibles. Las aplicaciones que migren al nuevo servicio no deben asumir que una determinada notificación existirá en id_tokens o access_tokens. Encontrará más información sobre los diferentes tipos de token que se usan en el punto de conexión v2.0 en la [referencia de token de acceso](access-tokens.md) y en la [referencia de `id_token`](id-tokens.md).

## <a name="limitations"></a>Limitaciones

Hay que tener en cuenta algunas restricciones cuando se usa el punto de conexión v2.0.

Cuando compile aplicaciones que se integran con la plataforma de identidad de Microsoft, debe decidir si los protocolos de autenticación y el punto de conexión v2.0 satisfacen sus necesidades. La plataforma y el punto de conexión v1.0 se siguen admitiendo totalmente y, en algunos aspectos, tienen más características que la versión 2.0. Sin embargo, la versión 2.0 [presenta ventajas importantes](azure-ad-endpoint-comparison.md) para los desarrolladores.

Esta es nuestra recomendación simplificada para desarrolladores en este momento:

* Si tiene que admitir cuentas Microsoft personales en la aplicación, use la versión 2.0. Pero antes, asegúrese de comprender las limitaciones que se analizan en este artículo.

* Si la aplicación solo tiene que admitir cuentas profesionales y educativas de Microsoft, no use la versión 2.0. En su lugar, consulte la [guía de v1.0](azure-ad-developers-guide.md).

Desarrollaremos el punto de conexión v2.0 para eliminar las restricciones que mencionamos en este artículo, por lo que siempre debe usar este punto de conexión. Mientras tanto, use este artículo para determinar si el punto de conexión v2.0 es correcto para usted. Actualizaremos este artículo constantemente para reflejar el estado actual del punto de conexión v2.0. Consúltelo de nuevo para volver a evaluar los requisitos en relación con las funcionalidades de v2.0.

### <a name="restrictions-on-app-types"></a>Restricciones en los tipos de aplicación

Actualmente, el punto de conexión v2.0 no admite los siguientes tipos de aplicaciones. Para ver una descripción de los tipos de aplicaciones compatibles, consulte [Tipos de aplicaciones en v2.0](v2-app-types.md).

#### <a name="standalone-web-apis"></a>API web independiente

Puede usar el punto de conexión v2.0 para [compilar una API web protegida con OAuth 2.0](v2-app-types.md#web-apis). Sin embargo, esa API web puede recibir tokens solo desde una aplicación con el mismo identificador de aplicación. No se puede obtener una API web desde un cliente con un identificador de aplicación distinto. El cliente no podrá solicitar ni obtener permisos para su API web.

Para ver cómo compilar una API web que acepte tokens de un cliente con el mismo identificador de aplicación, consulte los ejemplos de API web del punto de conexión v2.0 en la sección [Introducción a v2.0](v2-overview.md#getting-started).

### <a name="restrictions-on-app-registrations"></a>Restricciones en los registros de aplicaciones

Actualmente, para cada aplicación que desee integrar con el punto de conexión v2.0, debe crear un registro de aplicación en el nuevo [portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Las aplicaciones existentes de Azure AD o de la cuenta de Microsoft no son compatibles con el punto de conexión v2.0. Las aplicaciones registradas en cualquier otro portal que no sea el portal de registro de aplicaciones no son compatibles con el punto de conexión v2.0.

Además, los registros de aplicaciones que crea en el [portal de registro de aplicaciones](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tienen las siguientes salvedades:

* Solo se permiten dos secretos de aplicación por cada identificador de aplicación.

* Un registro de aplicaciones realizado por un usuario con una cuenta personal de Microsoft solo se puede ver y administrar mediante una sola cuenta de desarrollador. No se puede compartir entre varios desarrolladores. Si quiere compartir el registro de aplicaciones entre varios desarrolladores, puede crear la aplicación iniciando sesión en el portal de registro con una cuenta de Azure AD.

* Existen varias restricciones para el formato de la URL de redireccionamiento permitido. Para más información sobre las URL de redireccionamiento, consulte la sección siguiente.

### <a name="restrictions-on-redirect-urls"></a>Restricciones en las URL de redireccionamiento

Las aplicaciones registradas en el portal de registro de aplicaciones están restringidas a un conjunto limitado de valores de URL de redireccionamiento. La URL de redireccionamiento de aplicaciones y servicios web debe comenzar por el esquema `https`, y todos los valores de URL de redireccionamiento deben compartir un único dominio DNS. Por ejemplo, no puede registrar una aplicación web con una de estas URL de redireccionamiento:

* `https://login-east.contoso.com`  
* `https://login-west.contoso.com`

El sistema de registro compara el nombre DNS completo de la URL de redireccionamiento existente con el nombre DNS de la URL de redireccionamiento que va a agregar. La solicitud para agregar el nombre DNS presentará un error si se cumple alguna de las condiciones siguientes:  

* Si el nombre DNS completo de la nueva URL de redireccionamiento no coincide con el nombre DNS de la URL de redireccionamiento existente.

* Si el nombre DNS completo de la nueva URL de redireccionamiento no es un subdominio de la URL de redireccionamiento existente.

Por ejemplo, si la aplicación tiene la siguiente URL de redireccionamiento:

`https://login.contoso.com`

Puede agregarla del modo siguiente:

`https://login.contoso.com/new`

En este caso, el nombre DNS es una coincidencia exacta. O bien, puede hacer lo siguiente:

`https://new.login.contoso.com`

En este caso, hace referencia a un subdominio DNS de login.contoso.com. Si desea que una aplicación tenga `login-east.contoso.com` y `login-west.contoso.com` como URL de redireccionamiento, debe agregar las URL de redireccionamiento en este orden:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Puede agregar las dos últimas porque son subdominios de la primera URL de redireccionamiento, contoso.com. Esta limitación se eliminará en una próxima versión.

Tenga en cuenta también que solo puede tener 20 direcciones URL de respuesta para una aplicación concreta.

Para información sobre cómo registrar una aplicación en el portal de registro de aplicaciones, consulte [Cómo registrar una aplicación con el punto de conexión v2.0](quickstart-v2-register-an-app.md).

### <a name="restrictions-on-libraries-and-sdks"></a>Restricciones en las bibliotecas y SDK

En este momento, la compatibilidad del punto de conexión v2.0 con las bibliotecas es limitada. Si desea usar el punto de conexión v2.0 en una aplicación de producción, tiene las opciones siguientes:

* Si compila una aplicación web, puede usar sin riesgo el software intermedio de lado servidor con carácter de disponibilidad general de Microsoft para realizar el inicio de sesión y la validación de tokens. Incluye el software intermedio OWIN Open ID Connect para ASP.NET y el complemento NodeJS Passport. Para ver ejemplos de código que usan middleware de Microsoft, consulte la sección [Introducción a v2.0](v2-overview.md#getting-started).

* Si crea una aplicación de escritorio o para dispositivos móviles, puede usar una de las bibliotecas de autenticación de Microsoft (MSAL) de versión preliminar. Estas bibliotecas están en una versión de versión preliminar compatible con producción, por lo que su uso en aplicaciones de producción es seguro. Puede obtener más información sobre los términos y condiciones de la versión preliminar y las bibliotecas disponibles en la [referencia de bibliotecas de autenticación](reference-v2-libraries.md).

* En el caso de otras plataformas no cubiertas por las bibliotecas de Microsoft, pueden integrarse con el punto de conexión v2.0 enviando y recibiendo mensajes de protocolo directamente en el código de su aplicación. Los protocolos v2.0 OpenID Connect y OAuth [se documentan explícitamente](active-directory-v2-protocols.md) para ayudarle a realizar dicha integración.

* Por último, puede usar las bibliotecas de código abierto de Open ID Connect y OAuth para integrarse con el punto de conexión v2.0. El protocolo v2.0 debe ser compatible con muchas bibliotecas de código abierto de los protocolos sin cambios importantes. La disponibilidad de estos tipos de bibliotecas varía según el lenguaje y la plataforma. Los sitios web [Open ID Connect](http://openid.net/connect/) y [OAuth 2.0](http://oauth.net/2/) mantienen una lista de las implementaciones populares. Para más información, consulte [Azure Active Directory v2.0 y bibliotecas de autenticación](reference-v2-libraries.md) y la lista de bibliotecas de cliente de código abierto y los ejemplos que se probaron con el punto de conexión v2.0.

* Como referencia, el punto de conexión `.well-known` para el punto de conexión v2.0 común es `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`.  Reemplace `common` con el identificador de inquilino para obtener los datos específicos para el inquilino.  

### <a name="restrictions-on-protocols"></a>Restricciones en los protocolos

El punto de conexión v2.0 no admite SAML ni WS-Federation; solo admite Open ID Connect y OAuth 2.0. No todas las características y capacidades los protocolos OAuth se han incorporado al punto de conexión v2.0.

Las siguientes funcionalidades y características de protocolo actualmente *no están disponibles* en el punto de conexión v2.0:

* Actualmente, la notificación `email` solo se devuelve si se configura una notificación opcional y el ámbito es scope=email especificado en la solicitud. Sin embargo, este comportamiento cambiará cuando el punto de conexión v2.0 se actualice para cumplir aún más los estándares Open ID Connect y OAuth 2.0.

* El punto de conexión v2.0 no admite la emisión de notificaciones de roles o grupos en los tokens de identificador.

* La [concesión de credenciales de contraseña de propietario del recurso OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.3) no es compatible con el punto de conexión v2.0.

Además, el punto de conexión v2.0 no admite ninguna forma de los protocolos SAML o WS-Federation.

Para comprender mejor el alcance de la funcionalidad de protocolo que se admite en el punto de conexión v2.0, consulte nuestra [referencia a los protocolos OpenID Connect y OAuth 2.0](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>Restricciones de SAML

Si ha usado la biblioteca de autenticación de Active Directory (ADAL) en aplicaciones de Windows, es posible que haya aprovechado la autenticación integrada de Windows, que usa la concesión de aserción de Lenguaje de marcado de aserción de seguridad (SAML). Con esta concesión, los usuarios de los inquilinos de Azure AD federado pueden autenticarse de manera silenciosa con su instancia local de Active Directory sin escribir las credenciales. Actualmente, el punto de conexión v2.0 no admite la concesión de aserción de SAML.
