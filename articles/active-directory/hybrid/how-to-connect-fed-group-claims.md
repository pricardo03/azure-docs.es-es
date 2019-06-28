---
title: Configurar notificaciones de grupo para aplicaciones con Azure Active Directory | Microsoft Docs
description: Información sobre cómo configurar notificaciones de grupo para usar con Azure AD.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 19a8400a076825f17501fabdb3f38ea05915822e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65138702"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Configurar notificaciones de grupo para aplicaciones con Azure Active Directory (versión preliminar pública)

Azure Active Directory puede proporcionar información de suscripción a un grupo de usuarios en tokens que se pueden usar en las aplicaciones.  Se admiten dos patrones principales:

- Grupos que se identifican por su atributo de identificador de objetos (OID) de Azure Active Directory (disponible con carácter general)
- Grupos que se identifican por los atributos sAMAccountName o GroupSID para grupos y usuarios sincronizados de Active Directory (AD) (versión preliminar pública)

> [!IMPORTANT]
> Hay una serie de advertencias a tener en cuenta para esta funcionalidad de la versión preliminar:
>
>- La compatibilidad con el uso de los atributos del identificador de seguridad (SID) y de sAMAccountName sincronizados de forma local, está diseñada para permitir el traslado de aplicaciones existentes desde AD FS y otros proveedores de identidad. Los grupos administrados en Azure AD no contienen los atributos necesarios para emitir estas notificaciones.
>- En organizaciones más grandes, la cantidad de grupos de los que un usuario es miembro puede exceder el límite que Azure Active Directory agregará a un token. 150 grupos para un token de SAML y 200 para uno de JWT. Esto puede llevar a resultados impredecibles. Si este es un problema potencial, recomendamos realizar pruebas y, si es necesario, esperar hasta que agreguemos mejoras que le permitan restringir las notificacionesa los grupos relevantes para la aplicación.  
>- En cuanto al desarrollo de nuevas aplicaciones, o en los casos en que la aplicación se puede configurar y donde no se requiere una compatibilidad de grupo anidada, le recomendamos que la autorización desde la aplicación se base en roles de aplicación en vez de en grupos.  Gracias a ello, limitará la cantidad de información que se necesita para acceder al token; además, es más seguro y podrá separar la asignación de usuarios de la configuración de la aplicación.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-identity-providers"></a>Notificaciones de grupo para aplicaciones que migran de AD FS y otros proveedores de identidad

Muchas aplicaciones que están configuradas para autenticarse con AD FS dependen de la información de la suscripción del grupo en forma de atributos de grupo de Windows AD.   Estos atributos son el grupo sAMAccountName, que puede ser calificado en función del nombre de dominio, o el identificador de seguridad de grupo de Windows (GroupSID).  Cuando la aplicación está federada con AD FS, AD FS usa la función TokenGroups para recuperar las suscripciones del grupo para el usuario.

Para que coincida con el token que recibiría una aplicación de AD FS, se pueden emitir notificaciones de grupo y rol que contengan el dominio calificado sAMAccountName en lugar del valor de objectID de Azure Active Directory del grupo.

Los formatos que se admiten en las notificaciones de grupo son:

- **ObjectId de grupo Azure Active Directory** (disponible para todos los grupos)
- **sAMAccountName** (disponible para grupos sincronizados desde Active Directory)
- **NetbiosDomain\sAMAccountName** (disponible para grupos sincronizados desde Active Directory)
- **DNSDomainName\sAMAccountName** (disponible para grupos sincronizados desde Active Directory)
- **Identificador de seguridad de grupo local** (disponible para grupos sincronizados desde Active Directory)

> [!NOTE]
> Los atributos sAMAccountName y del grupo local SID solo están disponibles en los objetos de grupo sincronizados desde Active Directory.   No están disponibles en grupos creados en Azure Active Directory u Office365.   Las aplicaciones configuradas en Azure Active Directory para obtener atributos de grupo locales sincronizados solo se obtienen para grupos sincronizados.

## <a name="options-for-applications-to-consume-group-information"></a>Opciones para que las aplicaciones consuman información de grupo

Una forma en que las aplicaciones pueden obtener información de grupo es llamar al punto de conexión de los grupos de Graph para recuperar la suscripción de grupo para el usuario autenticado. Esta llamada garantiza que todos los grupos de los que un usuario es miembro estén disponibles, incluso cuando hay una gran cantidad de grupos involucrados y la aplicación necesita enumerar todos los grupos de los que el usuario es miembro.  La enumeración de grupos es entonces independiente de las limitaciones de tamaño del token.

Sin embargo, si una aplicación existente espera consumir información de grupo a través de las notificaciones que el token que recibe, Azure Active Directory se puede configurar con una serie de opciones de notificaciones diferentes para satisfacer las necesidades de la aplicación.  Considere las opciones siguientes:

- Cuando se usa la suscripción de grupo para propósitos de autorización desde la aplicación, es preferible usar el valor de ObjectID del grupo, que es fijo y único en Azure Active Directory y está disponible para todos los grupos.
- Si usa el grupo local sAMAccountName para la autorización, use nombres calificados de dominio, ya que hay menos posibilidades de que surjan problemas si los nombres entran en algún conflicto. sAMAccountName por sí solo puede ser único dentro de un dominio de Active Directory, pero si más de un dominio de Active Directory está sincronizado con un inquilino de Azure Active Directory, existe la posibilidad de que más de un grupo tenga el mismo nombre.
- Puede usar los [Roles de aplicación](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) para proporcionar una capa de direccionamiento indirecto entre la suscripción del grupo y la aplicación.   A continuación, la aplicación toma decisiones de autorización internas basadas en las notificaciones de roles del token.
- Si la aplicación está configurada para obtener atributos de grupo que se sincronizan desde Active Directory y un grupo no contiene esos atributos, no se incluirá en las notificaciones.
- Las notificaciones de grupo de los token incluyen grupos anidados.   Si un usuario es miembro de GroupB, y GroupB es a su vez miembro de GroupA, las notificaciones de grupo del usuario contendrán GroupA y GroupB. En cuanto a las organizaciones que cuentan con un uso intensivo de grupos anidados y tienen usuarios con un gran número de suscripciones a grupos, la cantidad de grupos enumerados en el token puede aumentar el tamaño de ese token.   Azure Active Directory limita la cantidad de grupos que se emitirá en un token a 150 para las aserciones de SAML y a 200 para JWT, para evitar que los token se vuelvan demasiado grandes.  Si un usuario es miembro de un número mayor de grupos que el límite establecido, los grupos se emiten y se crea un enlace al punto de conexión de Graph para obtener información del grupo.

> Requisitos previos para usar atributos de grupo sincronizados desde Active Directory:   Los grupos se deben sincronizar desde Active Directory mediante Azure AD Connect.

Hay dos pasos para configurar Azure Active Directory y así poder emitir nombres de grupo para los grupos de Active Directory.

1. **Sincronice los nombres de los grupos desde Active Directory** Antes de que Azure Active Directory pueda emitir los nombres de los grupos o el SID del grupo local en las notificaciones de grupo o rol, los atributos necesarios deben sincronizarse desde Active Directory.  Debe ejecutar la versión 1.2.70 de Azure AD Connect, o cualquier versión posterior.   Antes de la versión 1.2.70, Azure AD Connect sincronizará los objetos del grupo desde Active Directory, pero no incluirá los atributos de nombre de grupo requeridos de forma predeterminada.  Debe actualizar a la versión actual.

2. **Configure el registro de la aplicación en Azure Active Directory para incluir notificaciones de grupo en los token** Las notificaciones de grupo pueden configurarse en la sección Aplicaciones de empresa del portal de una aplicación de Galería o de SSO de SAML que no sea de la galería, o mediante el Manifiesto de la aplicación que se encuentra en la sección de registros de la aplicación.  Para configurar las notificaciones de grupo en el manifiesto de la aplicación, consulte "Configuración del registro de la aplicación de Azure Active Directory para los atributos de grupo" a continuación.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Configuración de las notificaciones de grupo para aplicaciones SAML mediante la configuración de SSO

Para configurar las notificaciones de grupo de una aplicación SAML de la galería o que no pertenezca a ella, abra Aplicaciones de empresa, haga clic en la aplicación de la lista y seleccione Configuración de inicio de sesión único.

Seleccione el icono de edición junto a "Grupos devueltos en el token".

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Use los botones de radio para seleccionar qué grupos deben incluirse en el token.

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Número de selección | DESCRIPCIÓN |
|----------|-------------|
| **Todos los grupos** | Emite grupos de seguridad y listas de distribución.   También hace que los roles de directorio que el usuario tenga asignados se emitan en una notificación “wids”, y cualquier rol de aplicación que el usuario tenga asignado se emita en la notificación de roles. |
| **Grupos de seguridad** | Emite grupos de seguridad de los que el usuario es miembro en la notificación de grupos. |
| **Listas de distribución** | Emite listas de distribución de las que el usuario es miembro. |
| **Roles de directorio** | Si al usuario se le asignan roles de directorio, se emiten como una notificación “wids” (no se emitirá la notificación de grupos). |

Por ejemplo, para emitir todos los grupos de seguridad a los que pertenece el usuario, seleccione Grupos de seguridad.

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

Para emitir grupos mediante los atributos de Active Directory sincronizados desde Active Directory en lugar de los valores de objectID de Azure AD, seleccione el formato requerido en el menú desplegable.  Esto reemplazará el id. de objeto en las notificaciones con valores de cadena que contengan nombres de grupo.   Solo los grupos sincronizados desde Active Directory se incluirán en las notificaciones.

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

### <a name="advanced-options"></a>Opciones avanzadas

La forma en que se emiten las notificaciones de grupo se puede modificar mediante la configuración en Opciones avanzadas.

Personalice el nombre de la notificación del grupo:  Si se selecciona esta opción, se puede especificar un tipo de notificación diferente para las notificaciones de grupo.   Escriba el tipo de notificación en el campo Nombre y el espacio de nombres opcional para la notificación en el campo de espacio de nombres.

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Algunas aplicaciones requieren que la información de suscripción del grupo aparezca en la notificación “rol”. Opcionalmente, puede emitir los grupos de usuarios como roles marcando la casilla “Emitir notificaciones de rol”.

![Interfaz de usuario de notificaciones](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Si se usa la opción de emitir datos de grupo como roles, solo los grupos aparecerán en la notificación de rol.  Cualquier rol de aplicación que el usuario haya asignado no aparecerá en la notificación de rol.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Configurar el registro de aplicación de Azure AD para atributos de grupo

Las notificaciones de grupo también se pueden configurar en la sección [Notificaciones opcionales](../../active-directory/develop/active-directory-optional-claims.md) del [Manifiesto de la aplicación](../../active-directory/develop/reference-app-manifest.md).

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

   | Esquema de notificaciones opcional | Valor |
   |----------|-------------|
   | **name:** | Debe ser "grupos" |
   | **source:** | No se usa. Omitir o especificar null |
   | **essential:** | No se usa. Omitir o especificar falso |
   | **additionalProperties:** | Lista de propiedades adicionales.  Las opciones válidas son "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”, "emit_as_roles" |

   En additionalProperties solo se necesita un valor de "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”.  Si hay más de uno, se usa el primero y omiten los demás.

   Algunas aplicaciones requieren información de grupo sobre el usuario en la notificación de rol.  Para cambiar el tipo de notificación de una notificación de grupo a una notificación de rol, agregue "emit_as_roles" a propiedades adicionales.  Los valores de grupo se emitirán en la notificación de rol.

   > [!NOTE]
   > Si se usa "emit_as_roles", cualquier rol de aplicación configurado que se asigne al usuario no aparecerá en la notificación de rol.

### <a name="examples"></a>Ejemplos

Emita grupos como nombres de grupo en tokens de acceso OAuth con el formato dnsDomainName\SAMAccountName.

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

## <a name="next-steps"></a>Pasos siguientes

[¿Qué es la identidad híbrida?](whatis-hybrid-identity.md)