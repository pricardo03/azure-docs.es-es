---
title: 'Permisos de rol de administrador personalizado para la administración del registro de aplicaciones: Azure Active Directory | Microsoft Docs'
description: Permisos de rol de administrador personalizado para delegar la administración de identidades.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ff6755f1391ff19e65df669fb51967a904f4f
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707437"
---
# <a name="application-registration-subtypes-and-permissions-in-azure-active-directory"></a>Permisos y subtipos del registro de aplicaciones en Azure Active Directory

Este artículo contiene los permisos del registro de aplicaciones disponibles actualmente para las definiciones de rol personalizado en Azure Active Directory (Azure AD).

## <a name="single-tenant-v-multi-tenant-permissions"></a>Permisos de un solo inquilino frente a permisos de varios inquilinos

Los permisos de rol personalizado difieren para aplicaciones de un solo inquilino y de varios inquilinos. Las aplicaciones de un solo inquilino únicamente están disponibles para los usuarios de la organización Azure AD en la que se registra la aplicación. Las aplicaciones de varios inquilinos están disponibles para todas las organizaciones de Azure AD. Las aplicaciones de un solo inquilino se definen como las que tienen **Tipos de cuenta compatibles** establecidos en "Solo las cuentas de este directorio organizativo". En Graph API, las aplicaciones de un solo inquilino tienen la propiedad signInAudience establecida en "AzureADMyOrg".

## <a name="application-registration-subtypes-and-permissions"></a>Permisos y subtipos del registro de aplicaciones

Vea la [información general sobre los roles personalizados](roles-custom-overview.md) para una explicación de lo que significa el subtipo de términos generales, el permiso y el conjunto de propiedades. La siguiente información es específica de los registros de aplicaciones.

### <a name="subtypes"></a>Subtipos

Solo hay un subtipo de registro de aplicaciones: applications.myOrganization. Por ejemplo, microsoft.directory/applications.myOrganization/basic/update. Este subtipo se establece en la página **Autenticación** para un registro de aplicaciones específico y corresponde a establecer la propiedad signInAudience en "AzureADMyOrg" mediante Graph API o PowerShell. El subtipo restringe el permiso a los registros de aplicaciones que se marcan como accesibles solo por las cuentas de la organización (aplicaciones de un solo inquilino).

Puede usar el permiso restringido para conceder permisos de lectura o administración a las aplicaciones internas solo sin conceder permisos de lectura o administración a las aplicaciones a las que tienen acceso las cuentas de otras organizaciones.

Hay versiones de applications.myOrganization de todos los permisos de lectura y actualización, así como del permiso de eliminación. No hay ninguna versión de applications.myOrganization de creación en este momento. Los permisos estándar (por ejemplo, microsoft.directory/applications/basic/update) conceden permisos de lectura o administración para todos los tipos de registro de aplicaciones.

![Declaración de una aplicación de un solo inquilino o una aplicación de varios inquilinos](./media/roles-custom-available-permissions/supported-account-types.png)

Los detalles de los siguientes permisos para la versión preliminar de roles personalizados se muestran en [Permisos de rol personalizado disponibles en Azure Active Directory](roles-custom-available-permissions.md).

### <a name="create-and-delete"></a>Creación y eliminación

Hay dos permisos disponibles para conceder la capacidad de crear registros de aplicaciones:

- **microsoft.directory/applications/createAsOwner**
- **microsoft.directory/applications/create**

Si se asignan ambos permisos, el permiso de creación tendrá prioridad. Aunque el permiso createAsOwner no agrega automáticamente el creador como primer propietario, los propietarios se pueden especificar durante la creación del registro de aplicaciones al usar Graph API o los cmdlets de PowerShell.

La creación de permisos concede acceso al comando **Nuevo registro**.

[Estos permisos conceden acceso al comando del portal Nuevo registro](./media/roles-create-custom/new-custom-role.png)

Hay dos permisos disponibles para conceder la capacidad de crear registros de aplicaciones:

#### <a name="microsoftdirectoryapplicationsdelete"></a>microsoft.directory/applications/delete

Permite eliminar registros de aplicaciones independientemente del subtipo; es decir, aplicaciones de un solo inquilino y de varios inquilinos.

#### <a name="microsoftdirectoryapplicationsmyorganizationdelete"></a>microsoft.directory/applications.myOrganization/delete

Permite eliminar los registros de aplicaciones restringidos a aquellos a los que solo se puede acceder con las cuentas de la organización o con las aplicaciones de un solo inquilino (subtipo de myOrganization).

![Estos permisos conceden acceso al comando de registro Eliminar aplicación](./media/roles-custom-available-permissions/delete-app-registration.png)

> [!NOTE]
> Al asignar un rol que contiene permisos de creación, la asignación de roles debe realizarse en el ámbito del directorio. Un permiso de creación asignado en un ámbito de recursos no concede la posibilidad de crear registros de aplicaciones.

### <a name="read"></a>Lectura

Todos los usuarios miembros de la organización pueden leer la información de registro de aplicaciones de forma predeterminada. Sin embargo, los usuarios invitados y las entidades de servicio de la aplicación no pueden. Si pretende asignar un rol a una aplicación o a un usuario invitado, debe incluir los permisos de lectura correspondientes.

#### <a name="microsoftdirectoryapplicationsallpropertiesread"></a>microsoft.directory/applications/allProperties/read

Capacidad para leer todas las propiedades de aplicaciones de un solo inquilino y de varios inquilinos fuera de propiedades confidenciales, como las credenciales.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesread"></a>microsoft.directory/applications.myOrganization/allProperties/read

Concede los mismos permisos que microsoft.directory/applications/allProperties/read, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsstandardread-grants-access-to-all-fields-on-the-application-registration-branding-page"></a>microsoft.directory/applications/standard/read: concede acceso a todos los campos de la página de personalización de marca del registro de aplicaciones.

![Este permiso concede acceso a la página de personalización de marca del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-branding.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationstandardread"></a>microsoft.directory/applications.myOrganization/standard/read

Concede los mismos permisos que microsoft.directory/applications/standard/read, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsownersread"></a>microsoft.directory/applications/owners/read

Permite leer la propiedad de propietarios en aplicaciones de un solo inquilino y de varios inquilinos. Concede acceso a todos los campos de la página de propietarios del registro de aplicaciones:

![Este permiso concede acceso a la página de propietarios del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-owners.png)

Concede acceso a las siguientes propiedades de la entidad de la aplicación:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppCreatedDateTime
- AppData
- AppId
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- WebApp
- WwwHomepage

### <a name="update"></a>Actualizar

#### <a name="microsoftdirectoryapplicationsallpropertiesupdate"></a>microsoft.directory/applications/allProperties/update

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesupdate"></a>microsoft.directory/applications.myOrganization/allProperties/update

Concede los mismos permisos que microsoft.directory/applications/allProperties/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsaudienceupdate"></a>microsoft.directory/applications/audience/update

Concede acceso a todos los campos de la página de autenticación del registro de aplicaciones:

![Este permiso concede acceso a la página de autenticación del registro de aplicaciones](./media/roles-custom-available-permissions/supported-account-types.png)

Concede acceso a las siguientes propiedades del recurso de la aplicación:

- AvailableToOtherTenants
- SignInAudience

#### <a name="microsoftdirectoryapplicationsmyorganizationaudienceupdate"></a>microsoft.directory/applications.myOrganization/audience/update

Concede los mismos permisos que microsoft.directory/applications/audience/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsauthenticationupdate"></a>microsoft.directory/applications/authentication/update

Capacidad de actualizar la dirección URL de respuesta, la dirección URL de cierre de sesión, el flujo implícito y las propiedades de dominio del publicador en aplicaciones de un solo inquilino y de varios inquilinos. Concede acceso a todos los campos de la página de autenticación del registro de aplicaciones, excepto a los tipos de cuenta compatibles:

![Concede acceso a la autenticación del registro de aplicaciones, pero no a los tipos de cuenta compatibles](./media/roles-custom-available-permissions/supported-account-types.png)

 Concede acceso a las siguientes propiedades del recurso de la aplicación:

- AcceptMappedClaims
- AccessTokenAcceptedVersion
- AddIns
- GroupMembershipClaims
- IsDeviceOnlyAuthSupported
- OAuth2LegacyUrlPathMatching
- OauthOidcResponsePolicyBitmap
- OptionalClaims
- OrgRestrictions
- PublicClient
- UseCustomTokenSigningKey

#### <a name="microsoftdirectoryapplicationsmyorganizationauthenticationupdate"></a>microsoft.directory/applications.myOrganization/authentication/update

Concede los mismos permisos que microsoft.directory/applications/authentication/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsbasicupdate"></a>microsoft.directory/applications/basic/update

Capacidad para actualizar el nombre, el logotipo, la dirección URL de la página principal, la dirección URL de los términos de servicio y las propiedades de la dirección URL de la declaración de privacidad en aplicaciones de un solo inquilino y de varios inquilinos. Concede acceso a todos los campos de la página de personalización de marca del registro de aplicaciones:

![Este permiso concede acceso a la página de personalización de marca del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-branding.png)

Concede acceso a las siguientes propiedades del recurso de la aplicación:

- AllowActAsForAllClients
- AllowPassthroughUsers
- AppAddress
- AppBrandingElements
- AppCategory
- AppData
- AppId
- AppInformationalUrl
- ApplicationTag
- AppLogoUrl
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- CreatedOnBehalfOf
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- IntegerExtensionAttribute
- KnownClientApplications
- LargeIntegerExtensionAttribute
- LegalAgeGroupRule
- LocalizedAppBrandingElements
- MainLogo
- MsaAppId
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- WebApp
- WwwHomepage

#### <a name="microsoftdirectoryapplicationsmyorganizationbasicupdate"></a>microsoft.directory/applications.myOrganization/basic/update

Concede los mismos permisos que microsoft.directory/applications/basic/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationscredentialsupdate"></a>microsoft.directory/applications/credentials/update

Capacidad para actualizar las propiedades de certificados y secretos de cliente en aplicaciones de un solo inquilino y de varios inquilinos. Concede acceso a todos los campos de la página de certificados y secretos del registro de aplicaciones:

![Este permiso concede acceso a la página de certificados y registros del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-secrets.png)

Concede acceso a las siguientes propiedades del recurso de la aplicación:
- AsymmetricKey
- EncryptedSecretKey
- KeyDescription
- SharedKeyReference
- TokenEncryptionKeyId

#### <a name="microsoftdirectoryapplicationsmyorganizationcredentialsupdate"></a>microsoft.directory/applications.myOrganization/credentials/update

Concede los mismos permisos que microsoft.directory/applications/credentials/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationsownersupdate"></a>microsoft.directory/applications/owners/update

Capacidad de actualizar la propiedad de propietario en un solo inquilino y en varios inquilinos. Concede acceso a todos los campos de la página de propietarios del registro de aplicaciones:

![Este permiso concede acceso a la página de propietarios del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-owners.png)

Concede acceso a las siguientes propiedades del recurso de la aplicación:
- Propietarios

#### <a name="microsoftdirectoryapplicationsmyorganizationownersupdate"></a>microsoft.directory/applications.myOrganization/owners/update

Concede los mismos permisos que microsoft.directory/applications/owners/update, pero solo para las aplicaciones de un solo inquilino.

#### <a name="microsoftdirectoryapplicationspermissionsupdate"></a>microsoft.directory/applications/permissions/update

Capacidad para actualizar los permisos delegados, los permisos de aplicación, las aplicaciones cliente autorizadas, los permisos necesarios y para otorgar las propiedades de consentimiento en aplicaciones de un solo inquilino y de varios inquilinos. No concede la capacidad de realizar el consentimiento. Concede acceso a todos los campos de las páginas Permisos de API y Exponer una API del registro de aplicaciones:

![Este permiso concede acceso a la página Permisos de API del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-api-permissions.png)

![Este permiso concede acceso a la página Exponer una API del registro de aplicaciones](./media/roles-custom-available-permissions/app-registration-expose-api.png)

Concede acceso a las siguientes propiedades del recurso de la aplicación:

- AppIdentifierUri
- Entitlement
- PreAuthorizedApplications
- RecordConsentConditions
- RequiredResourceAccess

#### <a name="microsoftdirectoryapplicationsmyorganizationpermissionsupdate"></a>microsoft.directory/applications.myOrganization/permissions/update

Concede los mismos permisos que microsoft.directory/applications/permissions/update, pero solo para las aplicaciones de un solo inquilino.

## <a name="required-license-plan"></a>Plan de licencia necesario

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Pasos siguientes

- Cree roles personalizados mediante [Azure Portal, PowerShell de Azure AD y Graph API](roles-create-custom.md)
- [Visualización de las asignaciones de un rol personalizado](roles-view-assignments.md)
