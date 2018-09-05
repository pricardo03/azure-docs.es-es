---
title: Descripción del manifiesto de aplicación de Azure Active Directory | Microsoft Docs
description: Cobertura detallada del manifiesto de aplicación de Azure Active Directory, que representa la configuración de identidad de una aplicación en un inquilino de Azure AD, y se usa para facilitar la autorización de OAuth, la experiencia de consentimiento y mucho más.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/27/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja, justhu
ms.openlocfilehash: f336771da334ffd964ecd032654b8549b7a5e847
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127027"
---
# <a name="azure-active-directory-app-manifest"></a>Manifiesto de aplicación de Azure Active Directory

Las aplicaciones que se integran con Azure Active Directory (Azure AD) deben registrarse en un inquilino de Azure AD. Puede configurar la aplicación en [Azure Portal](https://portal.azure.com); para ello, seleccione **Azure Active Directory**, seleccione la aplicación que quiere configurar y, luego, seleccione **Manifiesto**.

## <a name="manifest-reference"></a>Referencia de manifiesto

> [!NOTE]
> Si no puede ver las descripciones, maximice la ventana del explorador o desplácese o pase el dedo para ver las descripciones.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Clave  | Tipo de valor | Valor de ejemplo | DESCRIPCIÓN  |
|---------|---------|---------|---------|
| `appID` | Cadena de identificador | `"601790de-b632-4f57-9523-ee7cb6ceba95"` | Especifica el identificador único de la aplicación que Azure AD asigna a una aplicación. |
| `appRoles` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code> | Especifica la colección de roles que una aplicación puede declarar. Dichos roles se pueden asignar a usuarios, grupos o entidades de servicio. |
| `availableToOtherTenants`| boolean | `true` | Si este valor se establece en true, la aplicación está disponible para otros inquilinos. Si se establece en false, la aplicación solo está disponible para el inquilino en que está registrada. Para más información, consulte [Inicio de sesión de cualquier usuario de Azure Active Directory mediante el patrón de aplicación multiempresa](howto-convert-app-to-be-multi-tenant.md). |
| `displayName` | string | `MyRegisteredApp` | El nombre para mostrar de la aplicación. |
| `errorURL` | string | `http://MyRegisteredAppError` | La dirección URL de los errores detectados en una aplicación. |
| `groupMembershipClaims` | string | `1` | Una máscara de bits que configura la notificación `groups` emitida en un token de acceso OAuth 2.0 o de usuario que la aplicación espera. Los valores de máscara de bits son:<br>0: Ninguno<br>1: Grupos de seguridad y roles de Azure AD<br>2: Reservado<br>4: Reservado<br>Si la máscara de bits se establece en 7, se obtendrán todos los grupos de seguridad, grupos de distribución y los roles de directorio de Azure AD a los que pertenezca el usuario que inició sesión. |
| `optionalClaims` | string | `null` | Las notificaciones opcionales que el servicio de token de seguridad devuelve en el token para esta aplicación específica. Para más información, consulte las [notificaciones opcionales](active-directory-optional-claims.md). |
| `acceptMappedClaims` | boolean | `true` | Si este valor se establece en `true`, permite que la aplicación use la asignación de notificaciones sin especificar ninguna clave de firma personalizada. |
| `homepage` | string | `http://MyRegisteredApp` | Especifica la dirección URL a la página principal de la aplicación. |
| `informationalUrls` | string | <code>{<br>&nbsp;&nbsp;&nbsp;"privacy":"http://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"http://MyRegisteredApp/termsofservice"<br>}</code> | Especifica los vínculos a los términos del servicio y la declaración de privacidad de la aplicación. Las condiciones del servicio y la declaración de privacidad se exponen a los usuarios mediante la experiencia de consentimiento del usuario. Para más información, consulte [Condiciones del servicio y declaración de privacidad de las aplicaciones registradas de Azure Active Directory](howto-add-terms-of-service-privacy-statement.md). |
| `identifierUris` | Matriz de cadena | `http://MyRegistererdApp` | Los URI definidos por el usuario que identifican de manera única una aplicación web en su inquilino de Azure AD o en un dominio personalizado comprobado si la aplicación tiene varios inquilinos. |
| `keyCredentials` | Tipo de matriz | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> | Contiene referencias a credenciales asignadas por la aplicación, secretos compartidos basados en cadena y certificados X.509. Estas credenciales se utilizan cuando se solicitan tokens de acceso (cuando la aplicación actúa como cliente, en lugar de como recurso). |
| `knownClientApplications` | Tipo de matriz | `[GUID]` | Se usa para el consentimiento de unión si dispone de una solución que contenga dos partes, una aplicación cliente y una aplicación de API web personalizada. Si especifica el valor de appID de la aplicación cliente en este valor, el usuario solo tendrá que dar su consentimiento una vez a la aplicación cliente. Azure AD sabrá que dar el consentimiento al cliente significa que se consiente implícitamente la API web y aprovisionará automáticamente entidades de servicio para el cliente y la API web a la vez. Tanto el cliente como la aplicación de API web deben estar registrados en el mismo inquilino. |
| `logoutUrl` | string | `http://MyRegisteredAppLogout` | La dirección URL para cerrar la sesión de la aplicación. |
| `oauth2AllowImplicitFlow` | boolean | `false` | Especifica si esta aplicación web puede solicitar tokens de flujo implícitos de OAuth 2.0. El valor predeterminado es false. Estar marca se utiliza para las aplicaciones que usan el explorador, como las aplicaciones de página única de Javascript. |
| `oauth2AllowUrlPathMatching` | boolean | `false` | Especifica si, como parte de las solicitudes de token de OAuth 2.0, Azure AD permitirá que las rutas del identificador URI de redirección coincidan con el valor de replyUrls de la aplicación. El valor predeterminado es false. |
| `oauth2Permissions` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code> | Especifica la colección de ámbitos de permiso de OAuth 2.0 que la aplicación de API (recurso) web expone a las aplicaciones cliente. Estos ámbitos de permisos pueden concederse a las aplicaciones cliente durante el consentimiento. |
| `oauth2RequiredPostResponse` | boolean | `false` | Especifica si, como parte de las solicitudes de tokens de OAuth 2.0, Azure AD permitirá solicitudes POST, frente a las solicitudes GET. El valor predeterminado es false, que especifica solo se permitirán solicitudes GET. |
| `objectId` | Cadena de identificador | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` | El identificador único de la aplicación en el directorio. Este identificador no es el que se usa para identificar la aplicación en cualquier transacción del protocolo. Se usa para hacer referencia al objeto en las consultas del directorio. |
| `parentalControlSettings` | string | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>}</code> | `countriesBlockedForMinors` especifica los países en los que la aplicación se bloquea para menores de edad.<br>`legalAgeGroupRule` especifica la regla de grupo de edad legal que se aplica a los usuarios de la aplicación. Se puede establecer en `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` o `BlockMinors`.  |
| `passwordCredentials` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>]</code> | Consulte la descripción de la propiedad `keyCredentials`. |
| `publicClient` | boolean | `false` | Especifica si una aplicación es un cliente público, como una aplicación instalada que se ejecuta en un dispositivo móvil. El valor predeterminado es false. |
| `replyUrls` | Matriz de cadena | `"http://localhost"` | Esta propiedad multivalor contiene la lista de valores de redirect_uri registrados que Azure AD aceptará como destinos cuando se devuelvan tokens. |
| `requiredResourceAccess` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>&nbsp;}<br>]</code> | Con el consentimiento dinámico, `requiredResourceAccess` controla la experiencia de consentimiento del administrador y la experiencia de consentimiento del usuario para usuarios que usan consentimiento estático. Sin embargo, no se controla la experiencia de consentimiento del usuario para el caso general.<br>`resourceAppId` es el único identificador del recurso al que la aplicación necesita acceso. Este valor debe ser igual que el valor de appId declarado en la aplicación del recurso de destino.<br>`resourceAccess` es una matriz que enumera los ámbitos de permiso de OAuth 2.0 y los roles de aplicación que necesita la aplicación del recurso especificado. Contiene los valores `id` y `type` de los recursos especificados. |
| `samlMetadataUrl` | string | `http://MyRegisteredAppSAMLMetadata` | La dirección URL de los metadatos de SAML de la aplicación. |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la relación entre los objetos de aplicación y de entidad de servicio de una aplicación, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory (Azure AD)](app-objects-and-service-principals.md).
* Consulte el [glosario del desarrollador de Azure Active Directory](developer-glossary.md) para ver definiciones de algunos de los conceptos fundamentales para el desarrollador de Azure Active Directory (AD).

Use la siguiente sección de comentarios para especificar opiniones que ayuden a afinar y dar forma al contenido.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

