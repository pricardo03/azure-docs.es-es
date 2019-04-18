---
title: 'Información general de tokens: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre los tokens usados en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 11361bc6ab75e873e1b4081dcfc6492abc093b54
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680271"
---
# <a name="overview-of-tokens-in-azure-active-directory-b2c"></a>Información general de tokens de Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure B2C de Active Directory (Azure AD) B2C emite varios tipos de tokens de seguridad a medida que procesa cada [flujo de autenticación](active-directory-b2c-apps.md). Este documento describe el formato, las características de seguridad y el contenido de cada tipo de token.

## <a name="token-types"></a>Tipos de token

B2C de Azure AD admite el [protocolos OAuth 2.0 y OpenID Connect](active-directory-b2c-reference-protocols.md), que hace uso de tokens para la autenticación y acceso seguro a recursos. Todos los tokens usados en Azure AD B2C son [tokens web JSON (Jwt)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) que contiene aserciones de información sobre el portador y el asunto del token.

Los tokens siguientes se utilizan en la comunicación con Azure AD B2C:

- *Token de identificador* -JWT un que contiene las notificaciones que puede usar para identificar a los usuarios en la aplicación. Este token se envía de forma segura en las solicitudes HTTP para la comunicación entre dos componentes de la misma aplicación o servicio. Puede usar las notificaciones en un token de identificador como considere oportuno. Se suelen usar para mostrar información de la cuenta o para tomar decisiones de control de acceso en una aplicación. Los tokens de identificador están firmados, pero no están cifrados. Cuando la aplicación o API recibe un token de identificador, debe validar la firma para demostrar que es auténtico. La aplicación o la API también debe validar algunas notificaciones del token para demostrar que es válido. Según los requisitos de escenario, las notificaciones validadas por una aplicación pueden variar, pero la aplicación debe realizar algunas validaciones de notificación comunes en todos los escenarios.
- *Token de acceso* -JWT un que contiene las notificaciones que puede usar para identificar los permisos concedidos a las API. Los tokens de acceso están firmados, pero no están cifrados. Los tokens de acceso se utilizan para proporcionar acceso a los servidores de recursos y API.  Cuando la API recibe un token de acceso, debe validar la firma para demostrar que es auténtico. La API también debe validar algunas notificaciones del token para demostrar que es válido. Según los requisitos de escenario, las notificaciones validadas por una aplicación pueden variar, pero la aplicación debe realizar algunas validaciones de notificación comunes en todos los escenarios.
- *Token de actualización* -se usan tokens de actualización para adquirir nuevos tokens de identificador y tokens de acceso en un flujo de OAuth 2.0. Proporcionan la aplicación con acceso a largo plazo a los recursos en nombre de los usuarios sin necesidad de interacción con los usuarios. Los tokens de actualización son opacos para su aplicación. Se emitidos por Azure AD B2C y pueden inspeccionarse y sólo los interpretan Azure AD B2C. Son de larga duración, pero la aplicación no debe escribirse con la expectativa de que un token de actualización dure un período de tiempo específico. Los tokens de actualización pueden invalidarse en cualquier momento por varios motivos. La única forma de la aplicación sepa si un token de actualización es válido es intentar canjearlo realizando una solicitud de token a Azure AD B2C. Cuando se canjea un token de actualización para un nuevo token, recibe un nuevo token de actualización en la respuesta del token. Guarde el nuevo token de actualización. Reemplaza el token de actualización que usó anteriormente en la solicitud. Esta acción ayuda a garantizar que los tokens de actualización sigan siendo válidos mientras sea posible. 

## <a name="endpoints"></a>Puntos de conexión

Un [registrar aplicación](tutorial-register-applications.md) recibe los tokens y se comunica con Azure AD B2C mediante el envío de solicitudes a estos extremos:

- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize`
- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token`

Los tokens de seguridad que recibe la aplicación de Azure AD B2C pueden proceder de la `/authorize` o `/token` puntos de conexión. Cuando se adquieren los tokens de identificador de la `/authorize` punto de conexión, se realizan utilizando el [flujo implícito](active-directory-b2c-reference-spa.md), que a menudo se usa para los usuarios iniciar sesión en aplicaciones web basadas en javascript. Cuando se adquieren los tokens de identificador del punto de conexión `/token`, el procedimiento se realizan utilizando el [flujo de código confidencial](active-directory-b2c-reference-oidc.md), que mantiene el token oculto al explorador.

## <a name="claims"></a>Notificaciones

Con Azure AD B2C, tendrá un control preciso sobre el contenido de los tokens. Puede configurar [flujos de usuario](active-directory-b2c-reference-policies.md) y [directivas personalizadas](active-directory-b2c-overview-custom.md) para enviar ciertos conjuntos de datos de usuario en las notificaciones que son necesarias para la aplicación. Estas notificaciones pueden incluir propiedades estándar como **displayName** y **emailAddress**. Las aplicaciones pueden usar estas notificaciones para autenticar usuarios y solicitudes de manera segura. 

Las notificaciones en tokens de identificador no se devuelven en ningún orden concreto. En cualquier momento, se pueden introducir nuevas notificaciones en tokens de identificador. La aplicación no debe interrumpir cuando se introducen nuevas notificaciones. También puede incluir [atributos de usuario personalizados](active-directory-b2c-reference-custom-attr.md) en sus notificaciones.

En la tabla siguiente se enumera las notificaciones que puede esperar de los tokens de identificador y tener acceso a los tokens emitidos por Azure AD B2C.

| NOMBRE | Notificación | Valor de ejemplo | DESCRIPCIÓN |
| ---- | ----- | ------------- | ----------- |
| Público | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Identifica al destinatario previsto del token. Para Azure AD B2C, la audiencia es el identificador de aplicación. La aplicación debe validar este valor y rechazar el token si no coincide. Audiencia es sinónimo de recursos. |
| Emisor | `iss` |`https://{tenant}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Identifica el servicio de token de seguridad (STS) que construye y devuelve el token. También identifica el directorio en el que se autenticó al usuario. La aplicación debe validar la notificación del emisor para asegurarse de que el token proviene del punto de conexión adecuado. |
| Emitido a las | `iat` | `1438535543` | La hora en que se emitió el token, que se representa en tiempo de época. |
| Fecha de expiración | `exp` | `1438539443` | La hora en que el token deja de ser válido, que se representa en tiempo de época. La aplicación debe usar esta notificación para comprobar la validez de la duración del token. |
| No antes de | `nbf` | `1438535543` | Hora a la que el token pasa a ser válido, representada en tiempo de época. Este tiempo suele ser el mismo que el tiempo que se emitió el token. La aplicación debe usar esta notificación para comprobar la validez de la duración del token. |
| Versión | `ver` | `1.0` | La versión del token de identificador, tal como se define por Azure AD B2C. |
| Código hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Hash de código que se incluye en un token de identificador solo cuando el token se emite junto con un código de autorización de OAuth 2.0. Los códigos hash se pueden usar para validar la autenticidad de un código de autorización. Para obtener más información sobre cómo realizar esta validación, vea el [especificación de OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html).  |
| Hash de token de acceso | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Hash de token de acceso incluido en un token de identificador solo cuando el token se emite junto con un token de acceso de OAuth 2.0. El hash de token de acceso se puede usar para validar la autenticidad de un token de acceso. Para obtener más información sobre cómo realizar esta validación, vea el [especificación de OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html)  |
| Valor de seguridad | `nonce` | `12345` | El valor de seguridad es una estrategia que se usa para mitigar los ataques de reproducción de tokens. La aplicación puede especificar un valor nonce en una solicitud de autorización mediante el `nonce` parámetro de consulta. El valor que proporcione en la solicitud se genera sin modificar en el `nonce` de notificación de un token de identificador solo. Esta notificación permite que la aplicación comprobar el valor con respecto al valor especificado en la solicitud. La aplicación debe realizar la validación durante el proceso de validación del token de identificador. |
| Asunto | `sub` | `884408e1-2918-4cz0-b12d-3aa027d7563b` | La entidad de seguridad sobre la que el token declara información como el usuario de una aplicación. Este valor es inmutable y no se puede reasignar ni volver a usar. Se puede usar para realizar comprobaciones de autorización de forma segura, por ejemplo, cuando el token se usa para acceder a un recurso. De manera predeterminada, la notificación del asunto se rellena con el identificador de objeto del usuario del directorio. |
| Referencia de clase de contexto de autenticación | `acr` | No aplicable | Usar solo con las directivas anteriores. |
| Directiva de marco de confianza | `tfp` | `b2c_1_signupsignin1` | El nombre de la directiva que se usó para adquirir el token de identificador. |
| Hora de autenticación | `auth_time` | `1438535543` | La hora en el que un usuario especificó por última vez las credenciales, se representa en tiempo de época. |
| Ámbito | `scp` | `Read`| Los permisos concedidos al recurso para un token de acceso. Varios permisos concedidos están separados por un espacio. |
| Entidad autorizada | `azp` | `975251ed-e4f5-4efd-abcb-5f1a8f566ab7` | El **id. de la aplicación** cliente que inició la solicitud. |

## <a name="configuration"></a>Configuración

Las siguientes propiedades se utilizan para [administrar la vigencia de tokens de seguridad](configure-tokens.md) emitidos por Azure AD B2C:

- **Vigencia (en minutos) de token de acceso y de identificador**: la vigencia del token de portador de OAuth 2.0 que se utiliza para obtener acceso a un recurso protegido. El valor predeterminado es 60 minutos. El mínimo (incluido) es 5 minutos. El máximo (inclusive) es 1440 minutos.

- **Duración del token de actualización (días)** : el período de tiempo máximo antes de que puede usarse un token de actualización para adquirir un nuevo acceso o el token de identificador. El período de tiempo también cubre la adquisición de un nuevo token de actualización si la aplicación se ha concedido el `offline_access` ámbito. El valor predeterminado es 14 días. El mínimo (incluido) es un día. El máximo (inclusive) es de 90 días.

- **Duración de ventana deslizante de token de actualización (días)** : después de este período de tiempo determinado el usuario está obligado a volver a autenticar, cualquiera que sea el período de validez del último token de actualización obtenido por la aplicación. Solo se pueden proporcionar si el conmutador se establece en **Bounded**(Enlazado). Es preciso que sea mayor o igual que el valor de **Vigencia del token de actualización (en días)** . Si el conmutador se establece en **Unbounded**(Sin enlazar), no se puede proporcionar un valor específico. El valor predeterminado es 90 días. El mínimo (incluido) es un día. El máximo (inclusive) es 365 días.

Los siguientes casos de uso se habilitan mediante estas propiedades:

- Permiso para que un usuario pueda permanecer conectado en una aplicación móvil indefinidamente, siempre que esté continuamente activo en la misma. Establezca **Duración de la ventana deslizante del token de actualización (días)** en **Unbounded** (Sin enlazar) en el flujo de usuario de inicio de sesión.
- Cumpla los requisitos de cumplimiento normativo y seguridad de la industria mediante el establecimiento de la vigencia adecuada del token de acceso.

Estas opciones no están disponibles para flujos de usuario de restablecimiento de contraseña. 

## <a name="compatibility"></a>Compatibilidad

Las siguientes propiedades se utilizan para [administrar la compatibilidad de token](configure-tokens.md):

- **Notificación de emisor (iss)**: esta propiedad identifica el inquilino de Azure AD B2C que emitió el token. El valor predeterminado es `https://<domain>/{B2C tenant GUID}/v2.0/`. El valor de `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` incluyen los identificadores para el inquilino de Azure AD B2C y el flujo de usuario que se usó en la solicitud de token. Si su aplicación o biblioteca necesita Azure AD B2C sea compatible con la [especificación de OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html), use este valor.

- **Notificación de asunto (sub)**: esta propiedad identifica la entidad para la que el token valida la información. El valor predeterminado es **ObjectID**, que rellena el `sub` de notificación en el token con el identificador de objeto del usuario. El valor de **no admite** solo se proporciona por motivos de compatibilidad. Se recomienda que empiece a **ObjectID** tan pronto como pueda.

- **Notificación que representa el Id. de directiva** -esta propiedad identifica el tipo de notificación en la que se rellena el nombre de directiva utilizado en la solicitud de token. El valor predeterminado es `tfp`. El valor de `acr` solo se proporciona por motivos de compatibilidad.

## <a name="pass-through"></a>Paso a través

Cuando se inicia un recorrido del usuario, Azure AD B2C recibe un token de acceso de un proveedor de identidades. Azure AD B2C usa ese token para recuperar información sobre el usuario. Le [habilita una notificación en el flujo de usuario](idp-pass-through-user-flow.md) o [definir una notificación en la directiva personalizada](idp-pass-through-custom.md) para pasar el token a través de las aplicaciones que registre en Azure AD B2C. La aplicación debe usar un [flujo de usuario v2](user-flow-versions.md) para aprovechar las ventajas de pasar el token como una notificación.

B2C de Azure AD actualmente solo admite pasar el token de acceso de los proveedores de identidades de OAuth 2.0, como Facebook y Google. Para todos los demás proveedores de identidades, la notificación se devuelve en blanco. 

## <a name="validation"></a>Validación

Para validar un token, la aplicación debe comprobar la firma y notificaciones del token. Muchas bibliotecas de código abierto están disponibles para validar los Jwt, según su idioma preferido. Se recomienda explorar esas opciones en lugar de implementar su propia lógica de validación.

### <a name="validate-signature"></a>Validar la firma

Un JWT contiene tres segmentos, un *encabezado*, un *cuerpo*y un *firma*. El segmento de firma puede utilizarse para validar la autenticidad del token para que pueda confiar en la aplicación. Los tokens de Azure AD B2C se firman con algoritmos de cifrado asimétrico estándar del sector, como RSA 256. 

El encabezado del token contiene información acerca del método de cifrado y la clave utilizados para firmar el token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

El valor de la **alg** notificación es el algoritmo que se usó para firmar el token. El valor de la **kid** notificación es la clave pública que se usó para firmar el token. En cualquier momento, Azure AD B2C puede firmar un token mediante cualquiera de un conjunto de pares de claves pública y privada. B2C de Azure AD rota los posibles conjuntos de claves periódicamente. La aplicación debe escribirse para controlar automáticamente esos cambios de clave. Una frecuencia razonable para comprobar las actualizaciones de las claves públicas usadas por Azure AD B2C es cada 24 horas.

Azure AD B2C tiene un punto de conexión de metadatos OpenID Connect. Con este punto de conexión, las aplicaciones pueden solicitar información acerca de Azure AD B2C en tiempo de ejecución. En esta información se incluyen los extremos, los contenidos del token y las claves de firma de los token. El inquilino de Azure AD B2C contiene un documento de metadatos JSON para cada directiva. El documento de metadatos es un objeto JSON que contiene varias piezas de información útiles. Contienen los metadatos **jwks_uri**, que ofrece la ubicación del conjunto de claves públicas que se usan para firmar los tokens. Que la ubicación se proporciona aquí, pero la mejor capturar la ubicación dinámicamente mediante el documento de metadatos y análisis **jwks_uri**:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_signupsignin1
```
El documento JSON que se encuentra en esta dirección URL contiene toda la información de clave pública en uso en ese momento determinado. La aplicación puede usar la notificación `kid` en el encabezado de JWT para seleccionar en el documento JSON la clave pública que se ha usado en este documento para firmar un token concreto. Después, puede realizar la validación de la firma mediante la clave pública correcta y el algoritmo indicado.

El documento de metadatos para el `B2C_1_signupsignin1` directiva en el `contoso.onmicrosoft.com` inquilino se encuentra en:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_signupsignin1
```

Para determinar qué directiva se usó para firmar un token (y dónde debe ir para solicitar los metadatos), tiene dos opciones. En primer lugar, el nombre de la directiva se incluye en la notificación `acr` del token. Las notificaciones se pueden analizar fuera del cuerpo del JWT; para ello, descodifique la descodificación en base 64 del cuerpo y deserialice la cadena JSON resultante. El `acr` notificación es el nombre de la directiva que se usó para emitir el token. La otra opción consiste en codificar la directiva en el valor de la `state` parámetro al emitir la solicitud y descodificarla para determinar qué directiva se usó. Cualquiera de estos métodos es válido.

La descripción de cómo realizar la validación de la firma queda fuera del ámbito de este documento. Muchas bibliotecas de código abierto están disponibles para ayudarle a validar un token.

### <a name="validate-claims"></a>Validar las reclamaciones

Cuando las aplicaciones o API recibe un token de identificador, también debe realizar varias comprobaciones de las notificaciones del token de identificador. Se deben comprobar las notificaciones siguientes:

- **audiencia** -comprueba que el token de identificador estaba previsto que se asignará a la aplicación.
- **no antes** y **tiempo de expiración** -comprueba que el token de identificador no ha expirado.
- **emisor** -comprueba que haya emitido el token a la aplicación Azure AD B2C.
- **valor nonce** -una estrategia para mitigar ataques de reproducción de tokens.

Para obtener una lista completa de las validaciones que se debe realizar la aplicación, consulte el [especificación de OpenID Connect](https://openid.net).  

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo [usar los tokens de acceso](active-directory-b2c-access-tokens.md).

