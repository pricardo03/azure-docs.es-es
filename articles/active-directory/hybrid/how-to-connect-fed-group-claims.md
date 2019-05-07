---
title: Configurar notificaciones de grupo para las aplicaciones con Azure Active Directory | Microsoft Docs
description: Información sobre cómo configurar notificaciones de grupo para su uso con Azure AD.
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
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138702"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Configurar notificaciones de grupo para las aplicaciones con Azure Active Directory (versión preliminar)

Azure Active Directory puede proporcionar una información de pertenencia del grupo de usuarios en los tokens para su uso dentro de las aplicaciones.  Se admiten dos patrones principales:

- Grupos identificados por su atributo de identificador (OID) de objeto de Azure Active Directory (disponible con carácter general)
- Los grupos que identifican por sAMAccountName o GroupSID atributos para grupos de Active Directory (AD) sincronizado y usuarios (versión preliminar)

> [!IMPORTANT]
> Hay una serie de advertencias a tener en cuenta para esta funcionalidad de vista previa:
>
>- Soporte técnico para el uso de atributos de identificador (SID) de sAMAccountName y seguridad sincronizado desde local está diseñado para permitir mover las aplicaciones existentes de AD FS y otros proveedores de identidades. Grupos administrados de Azure AD no contienen los atributos necesarios para emitir estas notificaciones.
>- En organizaciones más grandes el número de grupos a los que un miembro de un usuario puede superar el límite que Azure Active Directory a agregar a un token. 150 grupos para un token de SAML y 200 para un JWT. Esto puede producir resultados imprevisibles. Si se trata de un problema potencial se recomienda realizar pruebas y si es necesario esperar a que se agregue mejoras para que pueda restringir las notificaciones para los grupos relevantes para la aplicación.  
>- Para el desarrollo de nuevas aplicaciones o, en casos donde la aplicación puede configurarse para él, y donde no se requiere la compatibilidad con grupos anidados, se recomienda que la autorización en la aplicación se basa en roles de aplicación en lugar de grupos.  Esto limita la cantidad de información que necesita para pasar al token, es más seguro y separa la asignación de usuario de configuración de la aplicación.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-identity-providers"></a>Notificaciones de grupo de aplicaciones que migran desde AD FS y otros proveedores de identidades

Muchas aplicaciones que están configuradas para autenticar con AD FS se basan en información de pertenencia a grupo en forma de atributos de grupo de Windows de AD.   Estos atributos son el atributo sAMAccountName de grupo, que puede ser el nombre de dominio por completo o el identificador de seguridad del grupo de Windows (GroupSID).  Cuando la aplicación está federada con AD FS, AD FS usa la función TokenGroups para recuperar las pertenencias a grupos del usuario.

Para que coincida con el token que recibiría una aplicación de AD FS, se pueden emitir notificaciones de grupo y la función que contiene el atributo sAMAccountName de dominio completo en lugar de objectID de Azure Active Directory del grupo.

Los formatos admitidos para las notificaciones de grupo son:

- **Grupo de Azure Active Directory ObjectId** (disponible para todos los grupos)
- **SAMAccountName** (disponible para los grupos sincronizados desde Active Directory)
- **NetbiosDomain\sAMAccountName** (disponible para los grupos sincronizados desde Active Directory)
- **DNSDomainName\sAMAccountName** (disponible para los grupos sincronizados desde Active Directory)
- **En el identificador de seguridad del grupo local** (disponible para los grupos sincronizados desde Active Directory)

> [!NOTE]
> sAMAccountName y atributos en el SID de grupo local sólo están disponibles en los objetos de grupo que se sincronizan desde Active Directory.   No están disponibles en los grupos creados en Azure Active Directory u Office 365.   Las aplicaciones configuradas en Azure Active Directory para obtener sincronizados de forma local en atributos de grupo de ellos obtención únicamente en grupos sincronizados.

## <a name="options-for-applications-to-consume-group-information"></a>Opciones para las aplicaciones consumir información de grupo

Una para las aplicaciones obtener información de grupo consiste en llamar al extremo de grupos de gráficos con el fin de recuperar la pertenencia a grupos del usuario autenticado. Esta llamada asegura todos los grupos de de que un usuario es miembro de están disponibles incluso cuando hay implicados un gran número de grupos y la aplicación necesita para enumerar todos los grupos del usuario es miembro.  Enumeración de grupo, a continuación, es independiente de las limitaciones de tamaño del token.

Sin embargo, si una aplicación existente ya espera consumir información de grupo a través de notificaciones en el token que recibe, Azure Active Directory puede configurarse con un número de notificaciones diferentes opciones para satisfacer las necesidades de la aplicación.  Considere las opciones siguientes:

- Cuando se usa la pertenencia a grupos para fines de autorización en la aplicación es preferible usar el valor de ObjectID de grupo, que es inmutable y único en Azure Active Directory y está disponible para todos los grupos.
- Si usa el sAMAccountName de grupo local para la autorización, use nombres de dominio completo;  con menos probabilidades de que se deriven de situaciones había conflicto de nombres. SAMAccountName por sí solo puede ser único dentro de un dominio de Active Directory, pero si más de un dominio de Active Directory está sincronizado con un inquilino de Azure Active Directory, es posible para más de un grupo que tienen el mismo nombre.
- Considere el uso de [Roles de aplicación](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) para proporcionar una capa de direccionamiento indirecto entre la pertenencia a grupos y la aplicación.   A continuación, la aplicación toma decisiones de autorización interno según almejas de rol en el token.
- Si la aplicación está configurada para obtener los atributos de grupo que se sincronizan desde Active Directory y un grupo no contiene los atributos no se incluirán en las notificaciones.
- Notificaciones de grupo en los tokens de incluir grupos anidados.   Si un usuario es miembro de GroupB y GroupB es miembro del grupo, las notificaciones de grupo para el usuario contendrá tanto GroupB como grupo. Para las organizaciones con un uso intensivo de los grupos anidados y los usuarios con un gran número de las pertenencias a grupos el número de grupos que aparecen en el token puede aumentar el tamaño del token.   Azure Active Directory limita el número de grupos que emitirá en un token a 150 para las aserciones de SAML y 200 para JWT para evitar que los tokens demasiado grande.  Si un usuario es miembro de un número mayor de grupos que el límite, se emiten los grupos y un vínculo al extremo de Graph para obtener información de grupo.

> Requisitos previos para usar atributos de grupo se sincronizan desde Active Directory:   Los grupos deben estar sincronizados desde Active Directory con Azure AD Connect.

Hay dos pasos para configurar Azure Active Directory para emitir los nombres de grupo para grupos de Active Directory.

1. **Sincronizar los nombres de grupo de Active Directory** antes de Azure Active Directory pueden emitir los nombres de grupo o en el grupo local de notificaciones SID de grupo o rol, los atributos necesarios deben sincronizarse desde Active Directory.  Debe ejecutar Azure AD Connect versión 1.2.70 o posterior.   Antes de la versión 1.2.70 Azure AD Connect sincronizará los objetos de grupo de Active Directory, pero no incluye los atributos de nombre de grupo necesario de forma predeterminada.  Debe actualizar a la versión actual.

2. **Configurar el registro de la aplicación en Azure Active Directory para incluir las notificaciones de grupo en los tokens** notificaciones de grupo pueden ser configurado en la sección aplicaciones empresariales del portal para una aplicación de galería o SSO de SAML ajena a la galería, o mediante el manifiesto de aplicación en la sección de registros de aplicación.  Para configurar notificaciones de grupo en la aplicación de manifiesto, vea "Configurar el registro de aplicación Azure Active Directory para los atributos de grupo" a continuación.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Configurar notificaciones de grupo para las aplicaciones de SAML con la configuración de SSO

Para configurar notificaciones de grupo para una aplicación de galería o SAML ajena a la galería, abra las aplicaciones empresariales, haga clic en la aplicación en la lista y seleccione Configuración de inicio de sesión único.

Seleccione el icono de edición situado junto a "Grupos devuelven en el símbolo (token)"

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Utilice los botones de radio para seleccionar los grupos que deben incluirse en el token

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Número de selección | DESCRIPCIÓN |
|----------|-------------|
| **Todos los grupos** | Emite los grupos de seguridad y de distribución se enumeran.   También hace que el usuario está asignado a emitirse en una notificación de 'wids' Roles del directorio y los roles de aplicación que el usuario está asignado a emitirse en la notificación de roles. |
| **Grupos de seguridad** | Emite los grupos de seguridad que del usuario es miembro de la notificación de grupos |
| **Listas de distribución** | Emite el usuario es miembro de las listas de distribución |
| **Roles de directorio** | Si el usuario está asignado a roles de directorio que se emiten como un 'wids' notificación (grupos de notificación no se emiten) |

Por ejemplo, para emitir el usuario es miembro de todos los grupos de seguridad, seleccione los grupos de seguridad

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

Para emitir grupos mediante los atributos de Active Directory que se sincronizan desde Active Directory en lugar de ObjectID de Azure AD para seleccionar el formato requerido en la lista desplegable.  Esto reemplaza el identificador de objeto en las notificaciones con valores de cadena que contiene los nombres de grupo.   Solo los grupos sincronizados desde Active Directory se incluirán en las notificaciones.

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

### <a name="advanced-options"></a>Opciones avanzadas

La manera en que se emiten notificaciones de grupo puede modificarse mediante la configuración en Opciones avanzadas

Personalizar el nombre de la notificación de grupo:  Si selecciona, se puede especificar un tipo de notificación diferente para notificaciones de grupo.   Escriba el tipo de notificación en el campo de nombre y el espacio de nombres opcional para la notificación en el campo espacio de nombres.

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Algunas aplicaciones requieren la información de pertenencia a grupo aparecen en la notificación "rol". Opcionalmente, puede emitir los grupos del usuario como roles activando la casilla 'Emitir notificaciones de un rol de grupos'.

![notificaciones de interfaz de usuario](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

> [!NOTE]
> Si se usa la opción para emitir los datos de grupo como roles, grupos solo aparecerán en la notificación de rol.  El usuario está asignado los Roles de aplicación no aparecerá en la notificación de rol.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>Configurar el registro de aplicación de Azure AD para los atributos de grupo

También se pueden configurar notificaciones de grupo en el [notificaciones opcionales](../../active-directory/develop/active-directory-optional-claims.md) sección de la [Application Manifest](../../active-directory/develop/reference-app-manifest.md).

1. En el portal -> Azure Active Directory -> aplicación registros -> seleccione aplicación -> manifiesto

2. Habilitar notificaciones de pertenencia a grupo cambiando el groupMembershipClaim

   Los valores válidos son:

   - "Todo"
   - "SecurityGroup"
   - "DistributionList"
   - "DirectoryRole"

   Por ejemplo: 

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   De forma predeterminada, de que se emitirá el objectID del grupo en el grupo valor de notificación.  Para modificar el valor de notificación para contienen atributos de grupo local, o para cambiar el tipo de notificación al rol, use OptionalClaims configuración de la siguiente manera:

3. Establecer notificaciones opcionales de configuración de nombre de grupo.

   Si desea grupos en el token para contener el entorno local los atributos de grupo de AD en la sección de notificaciones opcionales especifican qué notificación opcional de tipo de token se debe aplicar a, el nombre de la notificación opcional solicitada y cualquier otra propiedad deseado.  Pueden aparecer varios tipos de token:

   - idToken para el token de identificador de OIDC
   - accessToken para el token de acceso de OAuth/OIDC
   - Saml2Token para los tokens SAML.

   > [!NOTE]
   > El tipo de Saml2Token aplica tanto a SAML1.1 SAML2.0 tokens con formato

   Para cada tipo de token pertinente, modifique la notificación de grupos para usar la sección OptionalClaims en el manifiesto. El esquema OptionalClaims es como sigue:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Esquema de notificaciones opcionales | `Value` |
   |----------|-------------|
   | **name:** | Debe ser "grupos" |
   | **Origen:** | No se usa. Omitir o especifique null |
   | **essential:** | No se usa. Omitir o especifique false |
   | **additionalProperties:** | Lista de propiedades adicionales.  Las opciones válidas son "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles" |

   En additionalProperties solo uno de "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" son necesarios.  Si hay más de uno, se usa la primera y omiten los demás.

   Algunas aplicaciones requieren información de grupo sobre el usuario en la notificación de rol.  Para cambiar el tipo de notificación a un grupo de notificación para una notificación de rol, agregue "emit_as_roles" para propiedades adicionales.  Los valores de grupo se emitirá en la notificación de rol.

   > [!NOTE]
   > Si se utiliza "emit_as_roles" configurado de los Roles de aplicación que el usuario está asignado a voluntad no aparecen en la notificación de rol

### <a name="examples"></a>Ejemplos

Emita grupos como nombres de grupo en los tokens de acceso de OAuth en formato dnsDomainName\SAMAccountName

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Para emitir los nombres de grupos que se devuelven en formato netbiosDomain\samAccountName como las funciones de notificación de SAML y los Tokens de identificador de OIDC:

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