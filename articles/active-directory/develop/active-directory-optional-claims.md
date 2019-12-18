---
title: Proporcionar notificaciones opcionales a las aplicaciones de Azure AD | Azure
titleSuffix: Microsoft identity platform
description: Procedimientos para agregar notificaciones personalizadas o adicionales a los tokens SAML 2.0 y JSON Web Token (JWT) que emite Azure Active Directory.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/08/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcaf347eb91f8777b56bb2ea4d26985b2d75f645
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74967259"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Procedimientos para: Proporcionar notificaciones opcionales a la aplicación de Azure AD

Los desarrolladores de aplicaciones pueden usar notificaciones opcionales en sus aplicaciones de Azure AD para especificar qué notificaciones desean incluir en los tokens que se envían a las aplicaciones. 

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
| `groups`| Formato opcional de las notificaciones de grupo |JWT, SAML| |Se usa junto con la configuración de GroupMembershipClaims en el [manifiesto de la aplicación](reference-app-manifest.md), que se debe establecer también. Para más información, vea las [notificaciones de grupo](#configuring-groups-optional-claims) abajo. Para más información sobre las notificaciones de grupo, vea [Cómo configurar notificaciones de grupo](../hybrid/how-to-connect-fed-group-claims.md).
| `acct`             | Estado de la cuenta de los usuarios de un inquilino. | JWT, SAML | | Si el usuario es miembro del inquilino, el valor es `0`. Si es un invitado, el valor es `1`. |
| `upn`                      | Notificación UserPrincipalName. | JWT, SAML  |           | Aunque esta notificación se incluye automáticamente, puede especificarla como opcional para adjuntar propiedades adicionales y así modificarle el comportamiento para los usuarios invitados  |

## <a name="v20-specific-optional-claims-set"></a>Conjunto de notificaciones opcionales específicas de la versión 2.0

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

> [!IMPORTANT]
> Los tokens de acceso **siempre** se generan mediante el manifiesto del recurso, no del cliente,  con lo cual en la solicitud `...scope=https://graph.microsoft.com/user.read...`, el recurso es Graph.  Por lo tanto, el token de acceso se crea usando el manifiesto de Graph, no el manifiesto del cliente.  Cambiar el manifiesto de la aplicación nunca hará que los tokens de Graph tengan un aspecto diferente.  Para confirmar que los cambios realizados en `accessToken` han surtido efecto, solicite un token para la aplicación, no otra aplicación.  

Puede configurar notificaciones opcionales para la aplicación mediante la interfaz de usuario o el manifiesto.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Seleccione **Azure Active Directory** en el menú de la izquierda.
1. En la sección **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.

**Configuración de notificaciones opcionales mediante la interfaz de usuario:**

[![Muestra cómo configurar notificaciones opcionales mediante la interfaz de usuario.](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. En la sección **Administrar**, seleccione **Configuración del token (versión preliminar)** .
2. Seleccione **Agregar notificación opcional**.
3. Seleccione el tipo de token que desea configurar.
4. Seleccione las notificaciones opcionales que va a agregar.
5. Haga clic en **Agregar**.

**Configuración de notificaciones opcionales mediante el manifiesto de aplicación:**

[![Muestra cómo configurar notificaciones opcionales mediante el manifiesto de aplicación.](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. En la sección **Administrar**, seleccione **Manifiesto**. Se abrirá un editor de manifiestos basado en web que le permitirá editar el manifiesto. Si lo desea, puede seleccionar **Descargar**, editar el manifiesto de forma local y, a continuación, usar **Cargar** para volver a aplicarlo a la aplicación. Para más información sobre el manifiesto de aplicación, consulte el [artículo Descripción del manifiesto de aplicación de Azure AD](reference-app-manifest.md).

    La siguiente entrada del manifiesto de aplicación agrega las notificaciones opcionales auth_time, ipaddr y upn a los tokens de identificador, acceso y SAML.

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

2. When finished, click **Save**. Now the specified optional claims will be included in the tokens for your application.    


### OptionalClaims type

Declares the optional claims requested by an application. An application can configure optional claims to be returned in each of three types of tokens (ID token, access token, SAML 2 token) it can receive from the security token service. The application can configure a different set of optional claims to be returned in each token type. The OptionalClaims property of the Application entity is an OptionalClaims object.

**Table 5: OptionalClaims type properties**

| Name        | Type                       | Description                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Collection (OptionalClaim) | The optional claims returned in the JWT ID token. |
| `accessToken` | Collection (OptionalClaim) | The optional claims returned in the JWT access token. |
| `saml2Token`  | Collection (OptionalClaim) | The optional claims returned in the SAML token.   |

### OptionalClaim type

Contains an optional claim associated with an application or a service principal. The idToken, accessToken, and saml2Token properties of the [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) type is a collection of OptionalClaim.
If supported by a specific claim, you can also modify the behavior of the OptionalClaim using the AdditionalProperties field.

**Table 6: OptionalClaim type properties**

| Name                 | Type                    | Description                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | The name of the optional claim.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | The source (directory object) of the claim. There are predefined claims and user-defined claims from extension properties. If the source value is null, the claim is a predefined optional claim. If the source value is user, the value in the name property is the extension property from the user object. |
| `essential`            | Edm.Boolean             | If the value is true, the claim specified by the client is necessary to ensure a smooth authorization experience for the specific task requested by the end user. The default value is false.                                                                                                             |
| `additionalProperties` | Collection (Edm.String) | Additional properties of the claim. If a property exists in this collection, it modifies the behavior of the optional claim specified in the name property.                                                                                                                                               |
## Configuring directory extension optional claims

In addition to the standard optional claims set, you can also configure tokens to include directory schema extensions. For more info, see [Directory schema extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions). This feature is useful for attaching additional user information that your app can use – for example, an additional identifier or important configuration option that the user has set. See the bottom of this page for an example.

> [!NOTE]
> - Directory schema extensions are an Azure AD-only feature, so if your application manifest requests a custom extension and an MSA user logs into your app, these extensions will not be returned.
> - Azure AD optional claims only work with Azure AD Graph extensions and do not work with Microsoft Graph directory extensions. Both APIs require the `Directory.ReadWriteAll` permission, which can only be consented by admins.

### Directory extension formatting

When configuring directory extension optional claims using the application manifest, use the full name of the extension (in the format: `extension_<appid>_<attributename>`). The `<appid>` must match the ID of the application requesting the claim. 

Within the JWT, these claims will be emitted with the following name format:  `extn.<attributename>`.

Within the SAML tokens, these claims will be emitted with the following URI format: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## Configuring groups optional claims

   > [!NOTE]
   > The ability to emit group names for users and groups synced from on-premises is Public Preview.

This section covers the configuration options under optional claims for changing the group attributes used in group claims from the default group objectID to attributes synced from on-premises Windows Active Directory. You can configure groups optional claims for your application through the UI or application manifest.

> [!IMPORTANT]
> See [Configure group claims for applications with Azure AD](../hybrid/how-to-connect-fed-group-claims.md) for more details including important caveats for the public preview of group claims from on-premises attributes.

**Configuring groups optional claims through the UI:**
1. Sign in to the [Azure portal](https://portal.azure.com)
1. After you've authenticated, choose your Azure AD tenant by selecting it from the top right corner of the page
1. Select **Azure Active Directory** from the left hand menu
1. Under the **Manage** section, select **App registrations**
1. Select the application you want to configure optional claims for in the list
1. Under the **Manage** section, select **Token configuration (preview)**
2. Select **Add groups claim**
3. Select the group types to return (**All Groups**, **SecurityGroup** or **DirectoryRole**). The **All Groups** option includes **SecurityGroup**, **DirectoryRole** and **DistributionList**
4. Optional: click on the specific token type properties to modify the groups claim value to contain on premises group attributes or to change the claim type to a role
5. Click **Save**

**Configuring groups optional claims through the application manifest:**
1. Sign in to the [Azure portal](https://portal.azure.com)
1. After you've authenticated, choose your Azure AD tenant by selecting it from the top right corner of the page
1. Select **Azure Active Directory** from the left hand menu
1. Select the application you want to configure optional claims for in the list
1. Under the **Manage** section, select **Manifest**
3. Add the following entry using the manifest editor:

   The valid values are:

   - "All" (this option includes SecurityGroup, DirectoryRole and DistributionList)
   - "SecurityGroup"
   - "DirectoryRole"

   For example:

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

**Ejemplos:**

1) Emita grupos como nombres de grupo en tokens de acceso OAuth con el formato dnsDomainName\sAMAccountName.

    
    **Configuración en la interfaz de usuario:**

    [![Muestra cómo configurar notificaciones opcionales mediante la interfaz de usuario.](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)


    **Entrada en el manifiesto de aplicación:**
    ```json
        "optionalClaims": {
            "accessToken": [{
            "name": "groups",
            "additionalProperties": ["dns_domain_and_sam_account_name"]
            }]
        }
    ```

 
    
2) Emita los nombres de grupo que se devolverán en el formato NetBIOSDomain\sAMAccountName como la notificación de roles de los token de SAML y OIDC ID.

    **Configuración en la interfaz de usuario:**

    [![Muestra cómo configurar notificaciones opcionales mediante la interfaz de usuario.](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Entrada en el manifiesto de aplicación:**
    
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
    ``` 
     

## <a name="optional-claims-example"></a>Ejemplo de notificaciones opcionales

En esta sección, conocerá un escenario para ver cómo puede usar la característica de notificaciones opcionales para su aplicación.
Hay varias opciones disponibles para actualizar las propiedades de configuración de identidad de la aplicación y así habilitar y configurar las notificaciones opcionales:
-    Puede usar la interfaz de usuario de **Configuración del token (versión preliminar)** (consulte el ejemplo a continuación).
-    Puede usar el **Manifiesto** (consulte el ejemplo a continuación). Lea primero el documento [Manifiesto de aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) para una introducción al manifiesto.
-   También es posible escribir una aplicación que use [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) para actualizarla. La [referencia de tipos complejos y entidades](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) de la guía de referencia de Graph API puede ayudarle con la configuración de las notificaciones opcionales.

**Ejemplo:** en el siguiente ejemplo usará la interfaz de usuario de **Configuración del token (versión preliminar)** y el **Manifiesto** para agregar notificaciones opcionales a los tokens de acceso, identificador y SAML previstos para la aplicación. Se agregarán distintas notificaciones opcionales para cada tipo de token que la aplicación puede recibir:
-    Los tokens de identificación contendrán el nombre principal de usuario de los usuarios federados en la forma completa (`<upn>_<homedomain>#EXT#@<resourcedomain>`).
-    Los tokens de acceso que soliciten otros clientes para esta aplicación incluirán la notificación auth_time.
-    Los tokens SAML contendrán ahora la extensión del esquema de directorio skypeId (en este ejemplo, el identificador de esta aplicación es ab603c56068041afb2f6832e2a17e237). Los tokens SAML expondrán el identificador de Skype como `extension_skypeId`.

**Configuración en la interfaz de usuario:**

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.

1. Seleccione **Azure Active Directory** en el menú de la izquierda.

1. En la sección **Administrar**, seleccione **Registros de aplicaciones**.

1. Busque la aplicación para la que desea configurar notificaciones opcionales en la lista y haga clic en ella.

1. En la sección **Administrar**, haga clic en **Configuración del token (versión preliminar)** .

1. Seleccione **Agregar notificación opcional**, elija el tipo de token **Id.** , seleccione **upn** en la lista de notificaciones y, a continuación, haga clic en **Agregar**.

1. Seleccione **Agregar notificación opcional**, elija el tipo de token **Acceso**, seleccione **auth_time** en la lista de notificaciones y, a continuación, haga clic en **Agregar**.

1. En la pantalla de información general de Configuración del token, haga clic en el icono de lápiz situado junto a **upn**, haga clic en el control de alternancia **Autenticado externamente** y, a continuación, haga clic en **Guardar**.

1. Seleccione **Agregar notificación opcional**, elija el tipo de token **SAML**, seleccione **extn.skypeID** en la lista de notificaciones (solo es aplicable si ha creado un objeto de usuario de Azure AD denominado skypeID) y, a continuación, haga clic en **Agregar**.

    [![Muestra cómo configurar notificaciones opcionales mediante la interfaz de usuario.](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Configuración en el manifiesto:**
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Seleccione **Azure Active Directory** en el menú de la izquierda.
1. Busque la aplicación para la que desea configurar notificaciones opcionales en la lista y haga clic en ella.
1. En la sección **Administrar**, haga clic en **Manifiesto** para abrir el editor de manifiestos insertado.
1. Puede editar directamente el manifiesto mediante este editor. El manifiesto sigue el esquema de la entidad de la aplicación (https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest) y da formato al manifiesto automáticamente al guardarlo. Se agregarán los elementos nuevos a la propiedad `OptionalClaims`.

    ```json
            "optionalClaims": {
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
        ``` 


1. When you're finished updating the manifest, click **Save** to save the manifest.

## Next steps

Learn more about the standard claims provided by Azure AD.

- [ID tokens](id-tokens.md)
- [Access tokens](access-tokens.md)
