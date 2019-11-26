---
title: Obtenga información sobre cómo proporcionar notificaciones opcionales a la aplicación de Azure AD
titleSuffix: Microsoft identity platform
description: Guía en la que se agregan notificaciones personalizadas o adicionales a los tokens SAML 2.0 y web JSON (JWT) que emite Azure Active Directory.
documentationcenter: na
author: rwike77
services: active-directory
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/03/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b74e680979ccbcc94f8a49e993c6d64797ab80b1
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803410"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Procedimientos para: Proporcionar notificaciones opcionales a la aplicación de Azure AD

Los desarrolladores de aplicaciones pueden usar las notificaciones opcionales en sus aplicaciones de Azure AD para especificar las notificaciones que desean que los tokens envíen a las aplicaciones. 

Estas notificaciones opcionales sirven para:

- Seleccionar las notificaciones adicionales que se incluirán en los tokens para la aplicación.
- Cambiar el comportamiento de ciertas notificaciones que Azure AD devuelve en tokens.
- Agregar notificaciones personalizadas para la aplicación y acceder a ellas.

Para obtener las listas de notificaciones estándar, vea la documentación de notificaciones de [token de acceso](access-tokens.md) y de [id_token](id-tokens.md). 

Aunque las notificaciones opcionales se admiten en los tokens de formato de las versiones 1.0 y 2.0, así como en los tokens SAML, estos tokens proporcionan la mayoría de sus valores al pasar de la versión 1.0 a la 2.0. Uno de los objetivos del [punto de conexión de la plataforma de identidad de Microsoft de la versión 2.0](active-directory-appmodel-v2-overview.md) es conseguir tamaños de token menores para garantizar el rendimiento óptimo de los clientes. Como resultado, varias notificaciones que antes se incluían en los tokens de identificación y acceso ya no aparecen en los de la versión 2.0 y deben solicitarse específicamente para cada aplicación.

**Tabla 1: Aplicabilidad**

| Tipo de cuenta | Tokens de la versión 1.0 | Tokens de la versión 2.0  |
|--------------|---------------|----------------|
| Cuenta personal de Microsoft  | N/D  | Compatible |
| Cuenta de Azure AD      | Compatible | Compatible |

## <a name="v10-and-v20-optional-claims-set"></a>Conjunto de notificaciones opcionales de las versiones 1.0 y 2.0

El conjunto de notificaciones opcionales disponibles de forma predeterminada para que las usen las aplicaciones se enumeran a continuación. Para agregar notificaciones opcionales personalizadas para la aplicación, consulte las [extensiones de directorio](#configuring-directory-extension-optional-claims) a continuación. Al agregar notificaciones al **token de acceso**, esto se aplicará a los tokens de acceso solicitados *para* la aplicación (una API web), no *por* la aplicación. Esto garantiza que, independientemente del cliente que accede a la API, los datos correctos están presentes en el token de acceso que usan para autenticarse en la API.

> [!NOTE]
> La mayoría de estas notificaciones puede incluirse en los JWT para los tokens v1.0 y v2.0, pero no en los tokens SAML, excepto donde se indique en la columna Tipo de token. Las cuentas de consumidor admiten un subconjunto de estas notificaciones, marcadas en la columna "Tipo de usuario".  Muchas de las notificaciones aquí incluidas no se aplican a los usuarios consumidores (no tienen inquilino, por lo que `tenant_ctry` no tiene ningún valor).  

**Tabla 2: Conjunto de notificaciones opcionales de las versiones 1.0 y 2.0**

| NOMBRE                       |  DESCRIPCIÓN   | Tipo de token | Tipo de usuario | Notas  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Momento de la última autenticación del usuario. Consulte las especificaciones de Open ID Connect| JWT        |           |  |
| `tenant_region_scope`      | Región del inquilino de los recursos | JWT        |           | |
| `home_oid`                 | Para los usuarios invitados, identificador de objeto del usuario en el inquilino del usuario principal.| JWT        |           | |
| `sid`                      | Identificador de sesión que se usa para el cierre de cada sesión de usuario. | JWT        |  Cuentas personal y de Azure AD.   |         |
| `platf`                    | Plataforma de dispositivo    | JWT        |           | Restringido a los dispositivos administrados que pueden verificar el tipo de dispositivo|
| `verified_primary_email`   | Procede del valor PrimaryAuthoritativeEmail del usuario      | JWT        |           |         |
| `verified_secondary_email` | Procede del valor SecondaryAuthoritativeEmail del usuario   | JWT        |           |        |
| `enfpolids`                | Identificadores de las directivas en vigor. Lista de los identificadores de las directivas que se evaluaron para el usuario actual | JWT |  |  |
| `vnet`                     | Información del especificador de la red virtual | JWT        |           |      |
| `fwd`                      | Dirección IP.| JWT    |   | Agrega la dirección IPv4 original del cliente solicitante (cuando se encuentra en una red virtual) |
| `ctry`                     | País del usuario | JWT |  | Azure AD devuelve la notificación opcional `ctry` si existe y el valor de la notificación es un código de país estándar de dos letras, como FR, JP, SZ, etc. |
| `tenant_ctry`              | País del inquilino de los recursos | JWT | | |
| `xms_pdl`          | Ubicación de datos preferida   | JWT | | En los inquilinos de varias regiones geográficas, este es el código de 3 letras que muestra la región geográfica en la que se encuentra el usuario. Para más información, vea la [documentación de Azure AD Connect acerca de la ubicación de datos preferida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation).<br/>Por ejemplo: `APC` para Asia Pacífico. |
| `xms_pl`                   | Idioma preferido del usuario  | JWT ||El idioma preferido del usuario, si se establece. Se origina desde su inquilino principal, en escenarios de acceso de invitado. Con formato LL-CC ("en-us"). |
| `xms_tpl`                  | Idioma preferido del inquilino| JWT | | El idioma preferido del inquilino de recursos, si se establece. Con formato LL ("en"). |
| `ztdid`                    | Identificador de implementación sin interacción | JWT | | La identidad del dispositivo usada en [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot). |
| `email`                    | Correo electrónico direccionable de este usuario, si tiene uno.  | JWT, SAML | MSA, Azure AD | Si el usuario es un invitado en el inquilino, este valor se incluye de forma predeterminada.  Para los usuarios administrados (aquellos dentro del inquilino), se debe solicitar a través de esta notificación opcional o, en la versión 2.0, con el ámbito OpenID.  Para los usuarios administrados, se debe establecer la dirección de correo electrónico en el [portal de administración de Office](https://portal.office.com/adminportal/home#/users).| 
| `groups`| Formato opcional de las notificaciones de grupo |JWT, SAML| |Se usa junto con la configuración de GroupMembershipClaims en el [manifiesto de la aplicación](reference-app-manifest.md), que se debe establecer también. Para más información, vea las [notificaciones de grupo](#Configuring-group-optional claims) abajo. Para más información sobre las notificaciones de grupo, vea [Cómo configurar notificaciones de grupo](../hybrid/how-to-connect-fed-group-claims.md).
| `acct`             | Estado de la cuenta de los usuarios de un inquilino. | JWT, SAML | | Si el usuario es miembro del inquilino, el valor es `0`. Si es un invitado, el valor es `1`. |
| `upn`                      | Notificación UserPrincipalName. | JWT, SAML  |           | Aunque esta notificación se incluye automáticamente, puede especificarla como opcional para adjuntar propiedades adicionales y así modificarle el comportamiento para los usuarios invitados  |

### <a name="v20-optional-claims"></a>Notificaciones opcionales de la versión 2.0

Estas notificaciones siempre se incluyen en los tokens de la versión 1.0 de Azure AD, pero no en los de la versión 2.0, a menos que se solicite. Estas notificaciones solo son válidas en los JWT (tokens de identificación y de acceso). 

**Tabla 3: Notificaciones opcionales exclusivas de la versión 2.0**

| Notificación de JWT     | NOMBRE                            | DESCRIPCIÓN                                | Notas |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | Dirección IP                      | Dirección IP del cliente desde el que se inició sesión.   |       |
| `onprem_sid`  | Identificador de seguridad local |                                             |       |
| `pwd_exp`     | Tiempo de expiración de la contraseña        | Fecha y hora a la que expira la contraseña. |       |
| `pwd_url`     | Cambiar dirección URL de contraseña             | Dirección URL que el usuario puede visitar para cambiar la contraseña.   |   |
| `in_corp`     | Dentro de red corporativa        | Indica si el cliente ha iniciado sesión desde la red corporativa. En caso contrario, la notificación no se incluye.   |  En función de la configuración de las [IP de confianza](../authentication/howto-mfa-mfasettings.md#trusted-ips) de MFA.    |
| `nickname`    | Alias                        | Nombre adicional del usuario, aparte del nombre y del apellido. | 
| `family_name` | Apellido                       | Proporciona el apellido del usuario según está definido en el objeto de usuario. <br>"family_name": "Miller" | Se admite en MSA y Azure AD.   |
| `given_name`  | Nombre                      | Proporciona el nombre de pila o "dado" del usuario, tal como se establece en el objeto de usuario.<br>"given_name": "Frank"                   | Se admite en MSA y Azure AD.  |
| `upn`         | Nombre principal de usuario | Un identificador del usuario que se puede usar con el parámetro username_hint.  No es un identificador duradero para el usuario y no debe usarse con datos de clave. | Consulte a continuación las [propiedades adicionales](#additional-properties-of-optional-claims) de la configuración de la notificación. |

### <a name="additional-properties-of-optional-claims"></a>Propiedades adicionales de las notificaciones opcionales

Algunas notificaciones opcionales se pueden configurar para cambiar la manera de devolver la notificación. Estas propiedades adicionales se usan principalmente para ayudarle a migrar aplicaciones locales con diferentes expectativas de datos (por ejemplo, `include_externally_authenticated_upn_without_hash` le ayudará con los clientes que no pueden admitir almohadillas [`#`] en el UPN).

**Tabla 4: Valores para configurar las notificaciones opcionales**

| Nombre de propiedad  | Nombre de la propiedad adicional | DESCRIPCIÓN |
|----------------|--------------------------|-------------|
| `upn`          |                          | Puede usarse en respuestas SAML y JWT y para los token v1.0 y v2.0. |
|                | `include_externally_authenticated_upn`  | Incluye el nombre principal de usuario invitado tal como se almacenó en el inquilino de recursos. Por ejemplo: `foo_hometenant.com#EXT#@resourcetenant.com` |             
|                | `include_externally_authenticated_upn_without_hash` | Igual que antes, excepto que las almohadillas (`#`) se reemplazan con guiones bajos (`_`); por ejemplo, `foo_hometenant.com_EXT_@resourcetenant.com`. |

#### <a name="additional-properties-example"></a>Ejemplo de propiedades adicionales

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Este objeto OptionalClaims hace que el token de identificador devuelto al cliente incluya otro UPN con el inquilino de inicio adicional y la información de recursos de inquilino. La notificación `upn` solo cambia en el token si el usuario es un invitado en el inquilino (que usa un IDP diferente para la autenticación). 

## <a name="configuring-optional-claims"></a>Configuración de notificaciones opcionales

Puede configurar notificaciones opcionales para la aplicación al modificar el manifiesto de esta (consulte el ejemplo siguiente). Para más información, vea el artículo explicativo [Manifiesto de aplicación de Azure Active Directory](reference-app-manifest.md).

> [!IMPORTANT]
> Los tokens de acceso **siempre** se generan mediante el manifiesto del recurso, no del cliente,  con lo cual en la solicitud `...scope=https://graph.microsoft.com/user.read...`, el recurso es Graph.  Por lo tanto, el token de acceso se crea usando el manifiesto de Graph, no el manifiesto del cliente.  Cambiar el manifiesto de la aplicación nunca hará que los tokens de Graph tengan un aspecto diferente.  Para confirmar que los cambios realizados en `accessToken` han surtido efecto, solicite un token para la aplicación, no otra aplicación.  

**Esquema de ejemplo:**

```json
"optionalClaims":  
   {
      "idToken": [
            {
                  "name": "auth_time", 
                  "essential": false
             }
      ],
      "accessToken": [
             {
                    "name": "ipaddr", 
                    "essential": false
              }
      ],
      "saml2Token": [
              {
                    "name": "upn", 
                    "essential": false
               },
               {
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": false
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>Tipo OptionalClaims

Indica las notificaciones opcionales solicitadas por una aplicación. Una aplicación puede configurar que las notificaciones opcionales se devuelvan en los tres tipos de tokens (de identificación, de acceso y SAML 2) que reciba desde el servicio de token de seguridad. La aplicación puede configurar un conjunto diferente de notificaciones opcionales que se devuelva en cada tipo de token. La propiedad OptionalClaims de la entidad de la aplicación es un objeto OptionalClaims.

**Tabla 5: Propiedades del tipo OptionalClaims**

| NOMBRE        | type                       | DESCRIPCIÓN                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token de identificación de JWT. |
| `accessToken` | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token de acceso de JWT. |
| `saml2Token`  | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token SAML de JWT.   |

### <a name="optionalclaim-type"></a>Tipo OptionalClaim

Contiene una notificación opcional asociada a una aplicación o una entidad de servicio. Las propiedades idToken, accessToken y saml2Token del tipo [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) son una colección de OptionalClaim.
Si lo admite una notificación concreta, también puede modificar el comportamiento de OptionalClaim con el campo AdditionalProperties.

**Tabla 6: Propiedades del tipo OptionalClaim**

| NOMBRE                 | type                    | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | Nombre de la notificación opcional.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | Origen (objeto de directorio) de la notificación. Hay unas notificaciones predefinidas y otras definidas por el usuario desde las propiedades de extensión. Si el valor de origen es null, es una notificación opcional predefinida. Si el valor de origen es user, el valor de la propiedad name es la propiedad de extensión del objeto de usuario. |
| `essential`            | Edm.Boolean             | Si el valor es true, la notificación especificada por el cliente es necesaria garantizar una experiencia de autorización sin problemas para la tarea concreta solicitada por el usuario final. El valor predeterminado es false.                                                                                                             |
| `additionalProperties` | Colección (Edm.String) | Propiedades adicionales de la notificación. Si existe una propiedad en esta colección, modifica el comportamiento de la notificación opcional especificada en la propiedad name.                                                                                                                                               |
## <a name="configuring-directory-extension-optional-claims"></a>Configuración de notificaciones opcionales de extensión de directorio

Además del conjunto de notificaciones opcionales estándar, también se pueden configurar tokens para incluir las extensiones de esquema de directorio. Para más información, vea [Extensiones de esquema de directorio](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions). Esta característica es útil para adjuntar información de usuario adicional que puede usar la aplicación, por ejemplo, un identificador adicional o la opción de configuración importante que el usuario haya establecido. 

> [!Note]
> - Las extensiones de esquema de directorio son una característica exclusiva de Azure AD, por lo que si el manifiesto de aplicación solicita una extensión personalizada y un usuario de MSA inicia sesión en la aplicación, estas extensiones no se devolverán.
> - Las notificaciones opcionales de Azure AD solo funcionan con la extensión de Azure AD; no funcionarán con la extensión de directorio de Microsoft Graph. Ambas API requieren el permiso `Directory.ReadWriteAll`, que solo pueden otorgar los administradores.

### <a name="directory-extension-formatting"></a>Formato de extensión de directorio

Para los atributos de extensión, utilice el nombre completo de la extensión (con el formato `extension_<appid>_<attributename>`) en el manifiesto de aplicación. `<appid>` debe coincidir con el identificador de la aplicación que solicita la notificación. 

En el JWT, estas notificaciones se emitirán con el siguiente formato de nombre: `extn.<attributename>`.

En los tokens SAML, estas notificaciones se emitirán con el siguiente formato de identificador uniforme de recursos: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`.

## <a name="configuring-group-optional-claims"></a>Configuración de notificaciones opcionales de grupo

   > [!NOTE]
   > La capacidad para emitir nombres de grupo para usuarios y grupos sincronizados de forma local se encuentra en fase de versión preliminar pública.

En esta sección se describen las opciones de configuración de notificaciones opcionales para cambiar los atributos de grupo usados en las notificaciones de grupo, desde el atributo objectID de grupo predeterminado a los atributos que se sincronizan desde una instalación local de Active Directory de Windows.

> [!IMPORTANT]
> Vea [Configurar notificaciones de grupo de aplicaciones con Azure AD](../hybrid/how-to-connect-fed-group-claims.md) para obtener más detalles, incluidas algunas salvedades importantes sobre la versión preliminar pública de las notificaciones de grupo de atributos locales.

1. En el portal -> Azure Active Directory -> Registros de aplicaciones -> Seleccionar aplicación -> Manifiesto

2. Habilite las notificaciones de suscripción a grupos cambiando el valor de grupoMembershipClaim.

   Los valores válidos son:

   - "All"
   - "SecurityGroup"
   - "DistributionList"
   - "DirectoryRole"

   Por ejemplo:

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   De forma predeterminada, los valores de ObjectID de grupo se emitirán en el valor de la notificación de grupo.  Para modificar el valor de la notificación para que contenga los atributos del grupo local, o para cambiar el tipo de notificación a rol, use la configuración de notificaciones opcionales de la siguiente manera:

3. Establezca las notificaciones opcionales para la configuración de nombre de grupo.

   Si quiere que los grupos del token contengan los atributos de grupo locales de AD en la sección de notificaciones opcionales, especifique a qué tipo de token se debe aplicar la notificación, el nombre de la notificación opcional solicitada y las propiedades adicionales que quiera usar.  Pueden aparecer varios tipos de token:

   - idToken para el token de OIDC ID
   - accessToken para el token de acceso OAuth/OIDC
   - Saml2Token para los token de SAML.

   > [!NOTE]
   > El tipo Saml2Token se aplica a los token con formato SAML 1.1 y SAML 2.0

   Para cada tipo de token relevante, modifique las notificaciones de grupos para usar la sección OptionalClaims en el manifiesto. El esquema de OptionalClaims es el siguiente:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Esquema de notificaciones opcionales | Valor |
   |----------|-------------|
   | **name:** | Debe ser "grupos" |
   | **source:** | No se usa. Omitir o especificar null |
   | **essential:** | No se usa. Omitir o especificar falso |
   | **additionalProperties:** | Lista de propiedades adicionales.  Las opciones válidas son "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”, "emit_as_roles" |

   En additionalProperties solo se necesita un valor de "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”.  Si hay más de uno, se usa el primero y omiten los demás.

   Algunas aplicaciones requieren información de grupo sobre el usuario en la notificación de rol.  Para cambiar el tipo de notificación de una notificación de grupo a una notificación de rol, agregue "emit_as_roles" a propiedades adicionales.  Los valores de grupo se emitirán en la notificación de rol.

   > [!NOTE]
   > Si se usa "emit_as_roles", cualquier rol de aplicación configurado que se asigne al usuario no aparecerá en la notificación de rol.

**Ejemplos:** Emita grupos como nombres de grupo en tokens de acceso OAuth con el formato dnsDomainName\sAMAccountName.

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Para emitir los nombres de grupo que se devolverán en el formato netbiosDomain\samAccountName como la notificación de roles de los token de SAML y OIDC ID:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }

 ```

## <a name="optional-claims-example"></a>Ejemplo de notificaciones opcionales

En esta sección, conocerá un escenario para ver cómo puede usar la característica de notificaciones opcionales para su aplicación.
Hay varias opciones disponibles para actualizar las propiedades de configuración de identidad de la aplicación y así habilitar y configurar las notificaciones opcionales:
-   Puede modificar el manifiesto de aplicación. En el ejemplo siguiente se utilizará este método para realizar la configuración. Lea primero el documento [Manifiesto de aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) para una introducción al manifiesto.
-   También es posible escribir una aplicación que use [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) para actualizarla. La [referencia de tipos complejos y entidades](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) de la guía de referencia de Graph API puede ayudarle con la configuración de las notificaciones opcionales.

**Ejemplo:** En el ejemplo siguiente, modificará un manifiesto de aplicación para agregar notificaciones a los tokens de acceso, de identificación y SAML destinados a la aplicación.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Seleccione **Registros de aplicaciones** en el lado izquierdo.
1. Busque la aplicación para la que desea configurar notificaciones opcionales en la lista y haga clic en ella.
1. En la página de la aplicación, haga clic en **Manifiesto** para abrir el editor de manifiestos en línea. 
1. Puede editar directamente el manifiesto mediante este editor. El manifiesto sigue el esquema de la [entidad de la aplicación](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest) y da formato al manifiesto automáticamente al guardarlo. Se agregarán los elementos nuevos a la propiedad `OptionalClaims`.

    ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
            "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
            "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }

    ```

    En este caso, se agregaron distintas notificaciones opcionales para cada tipo de token que la aplicación puede recibir. Los tokens de identificación contendrán el nombre principal de usuario de los usuarios federados en la forma completa (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Ahora, los tokens de acceso que soliciten otros clientes para esta aplicación incluirán la notificación auth_time. Los tokens SAML contendrán ahora la extensión del esquema de directorio skypeId (en este ejemplo, el identificador de esta aplicación es ab603c56068041afb2f6832e2a17e237). Los tokens SAML expondrán el identificador de Skype como `extension_skypeId`.

1. Cuando haya terminado de actualizar el manifiesto, haga clic en **Guardar** para guardarlo.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las notificaciones estándares que proporciona Azure AD.

- [Tokens de identificador](id-tokens.md)
- [Tokens de acceso](access-tokens.md)
