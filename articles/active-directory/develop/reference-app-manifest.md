---
title: Descripción del manifiesto de aplicación de Azure Active Directory | Microsoft Docs
description: Cobertura detallada del manifiesto de aplicación de Azure Active Directory, que representa la configuración de identidad de una aplicación en un inquilino de Azure AD, y se usa para facilitar la autorización de OAuth, la experiencia de consentimiento y mucho más.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: f15bd1fa95cbbd998d23ac6d946db4ecd93b1e65
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76703038"
---
# <a name="azure-active-directory-app-manifest"></a>Manifiesto de aplicación de Azure Active Directory

El manifiesto de la aplicación contiene una definición de todos los atributos de un objeto de la aplicación en la plataforma de identidad de Microsoft. También sirve como mecanismo para actualizar el objeto de la aplicación. Para más información sobre la entidad Application y su esquema, consulte la [documentación sobre la entidad Application de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity).

Los atributos de una aplicación pueden configurarse en Azure Portal o mediante programación con la [API de REST](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) o [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications). Sin embargo, hay algunos escenarios en los que será necesario editar el manifiesto de la aplicación para poder configurar los atributos de una aplicación. Entre los escenarios se incluyen los siguientes:

* Si registró la aplicación como una cuenta multiinquilino de Azure AD y una cuenta de Microsoft personal, no podrá cambiar las cuentas de Microsoft compatibles en la interfaz de usuario. En su lugar, tendrá que utilizar el editor de manifiesto de la aplicación para cambiar los tipos de cuenta compatibles.
* Si necesita definir permisos y roles compatibles con la aplicación, tendrá que modificar el manifiesto de la aplicación.

## <a name="configure-the-app-manifest"></a>Configuración del manifiesto de la aplicación

Para configurar el manifiesto de la aplicación:

1. Vaya a [Azure Portal](https://portal.azure.com). Busque y seleccione el servicio **Azure Active Directory**.
1. Seleccione **App registrations** (Registros de aplicaciones).
1. Seleccione la aplicación que desee configurar.
1. Desde la página **Introducción** de la aplicación, seleccione la sección **Manifiesto**. Se abrirá un editor de manifiestos basado en la Web que le permitirá editar el manifiesto desde el portal. Si lo desea, también puede seleccionar **Descargar** para editar el manifiesto de forma local y usar después **Cargar** para aplicarlo de nuevo a la aplicación.

## <a name="manifest-reference"></a>Referencia de manifiesto

> [!NOTE]
> Si no ve la columna **Valor de ejemplo** después de la **Descripción**, maximice la ventana del explorador y desplácese o pase el dedo hasta que vea la columna **Valor de ejemplo**.

| Clave  | Tipo de valor | Descripción  | Valor de ejemplo |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Int32 que acepta valores null | Especifica la versión del token de acceso que espera el recurso. Este parámetro cambia la versión y el formato de JWT generado independientemente del punto de conexión o cliente utilizado para solicitar el token de acceso.<br/><br/>El punto de conexión usado, v1.0 o v2.0, lo elige el cliente y solo afecta a la versión de id_tokens. Los recursos deben configurar explícitamente `accesstokenAcceptedVersion` para indicar el formato del token de acceso admitido.<br/><br/>Los valores posibles de `accesstokenAcceptedVersion` son 1, 2 o un valor null. Si el valor es NULL, el valor predeterminado es 1, lo que corresponde al punto de conexión v1.0. <br/><br/>Si `signInAudience` es `AzureADandPersonalMicrosoftAccount`, el valor debe ser `2`.  | `2` |
| `addIns` | Colección | Define el comportamiento personalizado que puede usar un servicio de consumo para llamar a una aplicación en contextos concretos. Por ejemplo, las aplicaciones que pueden representar secuencias de archivos pueden establecer la propiedad addIns para su funcionalidad "FileHandler". Este parámetro permitirá que servicios como Office 365 llamen a la aplicación en el contexto de un documento en el que esté trabajando el usuario. | <code>{<br>&nbsp;&nbsp;&nbsp;"id":"968A844F-7A47-430C-9163-07AE7C31D407"<br>&nbsp;&nbsp;&nbsp;"type": "FileHandler",<br>&nbsp;&nbsp;&nbsp;"properties": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{"key": "version", "value": "2" }<br>&nbsp;&nbsp;&nbsp;]<br>}</code>|
| `allowPublicClient` | Boolean | Especifica el tipo de aplicación de reserva. Azure AD deduce el tipo de aplicación a partir de replyUrlsWithType, de forma predeterminada. Hay algunos escenarios en los que Azure AD no puede determinar el tipo de aplicación cliente. Por ejemplo, uno de estos escenarios es el flujo [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) en el que la solicitud HTTP se realiza sin redireccionamiento de direcciones URL). En esos casos, Azure AD interpretará el tipo de aplicación en función del valor de esta propiedad. Si este valor se establece en true, el tipo de aplicación de reserva se establece como cliente público, como una aplicación instalada que se ejecuta en un dispositivo móvil. El valor predeterminado es false, lo que significa que el tipo de aplicación de reserva es un cliente confidencial, como una aplicación web. | `false` |
| `availableToOtherTenants` | Boolean | Es "true" si la aplicación se comparte con otros inquilinos; de lo contrario, es "false". <br><br> _Nota: Esto solo está disponible en la experiencia de registros de aplicaciones (heredados). Reemplazado por `signInAudience` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | |
| `appId` | String | Especifica el identificador único de la aplicación que Azure AD asigna a una aplicación. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Colección | Especifica la colección de roles que una aplicación puede declarar. Dichos roles se pueden asignar a usuarios, grupos o entidades de servicio. Para ver más ejemplos e información, consulte [Procedimiento para agregar roles de aplicación en la aplicación y recibirlos en el token](howto-add-app-roles-in-azure-ad-apps.md). | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `displayName` | String | El nombre para mostrar de la aplicación. <br><br> _Nota: Esto solo está disponible en la experiencia de registros de aplicaciones (heredados). Reemplazado por `name` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | `"MyRegisteredApp"` |
| `errorUrl` | String | No compatible. | |
| `groupMembershipClaims` | String | Configura la notificación `groups` emitida en un token de acceso OAuth 2.0 o de usuario que la aplicación espera. Para establecer este atributo, use uno de los siguientes valores de cadena válidos:<br/><br/>- `"None"`<br/>- `"SecurityGroup"` (para grupos de seguridad y roles de Azure AD)<br/>- `"All"` (se obtendrán todos los grupos de seguridad, grupos de distribución y los roles de directorio de Azure AD a los que pertenezca el usuario que inició sesión. | `"SecurityGroup"` |
| `homepage` | String | URL a la página principal de la aplicación. <br><br> _Nota: Esto solo está disponible en la experiencia de registros de aplicaciones (heredados). Reemplazado por `signInUrl` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | `"https://MyRegisteredApp"` |
| `objectId` | String | El identificador único de la aplicación en el directorio. <br><br> _Nota: Esto solo está disponible en la experiencia de registros de aplicaciones (heredados). Reemplazado por `id` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `optionalClaims` | String | Las notificaciones opcionales que el servicio de token de seguridad devuelve en el token para esta aplicación específica.<br>En este momento, las aplicaciones que admiten cuentas personales y cuentas de Azure AD (registradas mediante el portal de registro de aplicaciones) no pueden usar notificaciones opcionales. Sin embargo, las aplicaciones registradas solo para Azure AD mediante el punto de conexión v2.0 pueden obtener las notificaciones opcionales que han solicitado en el manifiesto. Para más información, consulte las [notificaciones opcionales](active-directory-optional-claims.md). | `null` |
| `id` | String | El identificador único de la aplicación en el directorio. Este identificador no es el que se usa para identificar la aplicación en cualquier transacción del protocolo. Se usa para hacer referencia al objeto en las consultas del directorio. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | Matriz de cadenas | Los URI definidos por el usuario que identifican de manera única una aplicación web en su inquilino de Azure AD o en un dominio personalizado comprobado si la aplicación tiene varios inquilinos. | <code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> |
| `informationalUrls` | String | Especifica los vínculos a los términos del servicio y la declaración de privacidad de la aplicación. Las condiciones del servicio y la declaración de privacidad se exponen a los usuarios mediante la experiencia de consentimiento del usuario. Para obtener más información, consulte [Cómo agregar condiciones del servicio y declaración de privacidad de las aplicaciones registradas de Azure AD](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Colección | Contiene referencias a credenciales asignadas por la aplicación, secretos compartidos basados en cadena y certificados X.509. ). | <code>[<br>&nbsp;{<br>&nbsp;&These credentials are used when requesting access tokens (when the app is acting as a client rather that as resourcenbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | Matriz de cadenas | Se usa para el consentimiento de unión si dispone de una solución que contenga dos partes, una aplicación cliente y una aplicación de API web personalizada. Si especifica el valor de appID de la aplicación cliente en este valor, el usuario solo tendrá que dar su consentimiento una vez a la aplicación cliente. Azure AD sabrá que el consentimiento al cliente significa consentir implícitamente a la API web. Aprovisionará automáticamente las entidades de servicio para el cliente y la API web al mismo tiempo. Tanto el cliente como la aplicación de API web deben estar registrados en el mismo inquilino. | `["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"]` |
| `logoUrl` | String | Leer el valor solo que apunta a la dirección URL de CDN para el logotipo que se cargó en el portal. | `"https://MyRegisteredAppLogo"` |
| `logoutUrl` | String | La dirección URL para cerrar la sesión de la aplicación. | `"https://MyRegisteredAppLogout"` |
| `name` | String | El nombre para mostrar de la aplicación. | `"MyRegisteredApp"` |
| `oauth2AllowImplicitFlow` | Boolean | Especifica si esta aplicación web puede solicitar tokens de acceso de flujo implícitos de OAuth2.0. El valor predeterminado es false. Esta marca se utiliza para las aplicaciones que usan el explorador, como las aplicaciones de página única de Javascript. Para más información, escriba `OAuth 2.0 implicit grant flow` en la tabla de contenido y vea los temas sobre el flujo implícito. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | Boolean | Especifica si esta aplicación web puede solicitar tokens de identificador de flujo implícitos de OAuth2.0. El valor predeterminado es false. Esta marca se utiliza para las aplicaciones que usan el explorador, como las aplicaciones de página única de Javascript. | `false` |
| `oauth2Permissions` | Colección | Especifica la colección de ámbitos de permiso de OAuth 2.0 que la aplicación de API (recurso) web expone a las aplicaciones cliente. Estos ámbitos de permisos pueden concederse a las aplicaciones cliente durante el consentimiento. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>] </code>|
| `oauth2RequiredPostResponse` | Boolean | Especifica si, como parte de las solicitudes de tokens de OAuth 2.0, Azure AD permitirá solicitudes POST, frente a las solicitudes GET. El valor predeterminado es false, que especifica solo se permitirán solicitudes GET. | `false` |
| `parentalControlSettings` | String | `countriesBlockedForMinors` especifica los países en los que la aplicación se bloquea para menores de edad.<br>`legalAgeGroupRule` especifica la regla de grupo de edad legal que se aplica a los usuarios de la aplicación. Se puede establecer en `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids` o `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> |
| `passwordCredentials` | Colección | Consulte la descripción de la propiedad `keyCredentials`. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> |
| `preAuthorizedApplications` | Colección | Enumera las aplicaciones y los permisos solicitados para el consentimiento implícito. Requiere que un administrador haya proporcionado su consentimiento a la aplicación. preAuthorizedApplications no requiere que el usuario dé su consentimiento para los permisos solicitados. Los permisos enumerados en preAuthorizedApplications no requieren el consentimiento del usuario. Sin embargo, los permisos solicitados adicionales que no aparecen en preAuthorizedApplications requieren el consentimiento del usuario. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `publicClient` | Boolean | Especifica si esta aplicación es un cliente público (como una aplicación instalada que se ejecuta en un dispositivo móvil). <br><br> _Nota: Esta propiedad está disponible en la experiencia de Registros de aplicaciones (heredados). Reemplazado por `allowPublicClient` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | |
| `publisherDomain` | String | El dominio del publicador comprobado para la aplicación. Solo lectura. | https://www.contoso.com |
| `replyUrls` | Matriz de cadena | Esta propiedad multivalor contiene la lista de valores de redirect_uri registrados que Azure AD aceptará como destinos cuando se devuelvan tokens. <br><br> _Nota: Esta propiedad está disponible en la experiencia de Registros de aplicaciones (heredados). Reemplazado por `replyUrlsWithType` en la experiencia [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908)._ | |
| `replyUrlsWithType` | Colección | Esta propiedad multivalor contiene la lista de valores de redirect_uri registrados que Azure AD aceptará como destinos cuando se devuelvan tokens. Cada valor del identificador URI debe contener un valor de tipo de aplicación asociado. Los valores de tipo admitidos son: <ul><li>`Web`</li><li>`InstalledClient`</li></ul><br> Más información sobre las [limitaciones y restricciones de los URI de respuesta](https://docs.microsoft.com/azure/active-directory/develop/reply-url). | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Colección | Con el consentimiento dinámico, `requiredResourceAccess` controla la experiencia de consentimiento del administrador y la experiencia de consentimiento del usuario para usuarios que usan consentimiento estático. Aunque este parámetro no controla la experiencia de consentimiento del usuario del caso general.<br>`resourceAppId` es el único identificador del recurso al que la aplicación necesita acceso. Este valor debe ser igual que el valor de appId declarado en la aplicación del recurso de destino.<br>`resourceAccess` es una matriz que enumera los ámbitos de permiso de OAuth 2.0 y los roles de aplicación que necesita la aplicación del recurso especificado. Contiene los valores `id` y `type` de los recursos especificados. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>] </code> |
| `samlMetadataUrl` | String | La dirección URL de los metadatos de SAML de la aplicación. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | String | Especifica la dirección URL a la página principal de la aplicación. | `https://MyRegisteredApp` |
| `signInAudience` | String | Especifica qué cuentas de Microsoft se admiten en la aplicación actual. Los valores admitidos son:<ul><li>**AzureADMyOrg**: usuarios con una cuenta profesional o educativa de Microsoft en el inquilino de Azure AD de la organización (por ejemplo, un inquilino único)</li><li>**AzureADMultipleOrgs**: usuarios con una cuenta profesional o educativa de Microsoft en el inquilino de Azure AD de la organización (por ejemplo, un multiinquilino)</li> <li>**AzureADandPersonalMicrosoftAccount**: los usuarios con una cuenta Microsoft personal o una cuenta profesional o educativa en el inquilino de Azure AD de la organización</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | Matriz de cadenas | Cadenas personalizadas que pueden utilizarse para clasificar e identificar la aplicación. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |

## <a name="common-issues"></a>Problemas comunes

### <a name="manifest-limits"></a>Límites de manifiesto

Un manifiesto de aplicación tiene varios atributos que se conocen como colecciones; por ejemplo, approles, keycredentials, knownClientApplications, identifierUris, rediretUris, requiredResourceAccess y oauth2Permissions. Dentro del manifiesto de aplicación completa para cualquier aplicación, el número total de entradas de todas las colecciones combinadas se ha limitado a 1200. Si previamente especifica 100 URI de redireccionamiento en el manifiesto de aplicación, solo le quedarán 1100 entradas restantes para usar entre todas las demás colecciones combinadas que componen el manifiesto.

> [!NOTE]
> Si intenta agregar más de 1200 entradas en el manifiesto de aplicación, es posible que aparezca un error **"No se pudo actualizar la aplicación xxxxxx. Detalles del error: El tamaño del manifiesto ha excedido su límite. Reduzca el número de valores y vuelva a intentar la solicitud".**

### <a name="unsupported-attributes"></a>Atributos no admitidos

El manifiesto de aplicación representa el esquema del modelo de aplicación subyacente de Azure AD. A medida que evoluciona el esquema subyacente, el editor de manifiestos se actualizará para reflejar el nuevo esquema de vez en cuando. Como resultado, es posible que observe nuevos atributos que se muestran en el manifiesto de aplicación. En raras ocasiones, es posible que observe un cambio semántico o sintáctico en los atributos existentes o quizás encontrará un atributo que existía previamente, pero ya no se admite. Por ejemplo, verá los nuevos atributos en los [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) que se conocen por otro nombre en la experiencia de Registros de aplicaciones (heredados).


| Registros de aplicaciones (heredados)| Registros de aplicaciones           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Para obtener descripciones de estos atributos, consulte la sección de [referencia de manifiesto](#manifest-reference).

Al intentar cargar un manifiesto descargado anteriormente, es posible que vea uno de los siguientes errores. Este error probablemente se deba a que el editor de manifiestos ahora admite una versión más reciente del esquema, que no coincide con el que está intentando cargar.

- "**No se pudo actualizar la aplicación xxxxxx. Detalle del error: Identificador de objeto no válido 'undefined'. []** ".
- "**No se pudo actualizar la aplicación xxxxxx. Detalle del error: Uno o más valores de propiedad especificados no son válidos. []** ".
- "**No se pudo actualizar la aplicación xxxxxx. Detalle del error: No se puede para establecer availableToOtherTenants en esta versión de API para la actualización. [].** ".
- "**No se pudo actualizar la aplicación xxxxxx. Detalle del error: No se permiten actualizaciones de la propiedad "replyUrls" para esta aplicación. Use la propiedad 'replyUrlsWithType' en su lugar. [].** ".
- "**No se pudo actualizar la aplicación xxxxxx. Detalle del error: Se encontró un valor sin nombre de tipo y no ningún tipo esperado disponible. Cuando se especifica el modelo, cada valor de la carga debe tener un tipo que se puede especificar en la carga, explícitamente por el autor de la llamada o inferido implícitamente del valor primario. []** "

Cuando vea uno de estos errores, se recomienda realizar las acciones siguientes:

1. Edite los atributos de forma individualmente en el editor de manifiestos en lugar de cargar un manifiesto descargado anteriormente. Use la tabla de [referencia de manifiesto](#manifest-reference) para comprender la sintaxis y semántica de atributos antiguos y nuevos para que se puedan editar correctamente los atributos que le interesan. 
1. Si el flujo de trabajo requiere guardar los manifiestos en el repositorio de origen para su uso posterior, se recomienda reorganizar los manifiestos guardados en el repositorio con el que ve en la experiencia **Registros de aplicaciones**.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la relación entre los objetos de aplicación y entidad de servicio de una aplicación, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](app-objects-and-service-principals.md).
* Consulte el [Glosario para desarrolladores de la plataforma de identidad de Microsoft](developer-glossary.md) para obtener las definiciones de algunos de los conceptos básicos para desarrolladores de la Plataforma de identidad de Microsoft.

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
