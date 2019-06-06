---
title: Configuración de la aplicación cliente (biblioteca de autenticación de Microsoft) | Azure
description: Obtenga información sobre las opciones de configuración de cliente público y cliente confidencial aplicaciones en la biblioteca de autenticación de Microsoft (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5e175a8cdd1622add90bd80df63303fe914ab9c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430810"
---
# <a name="application-configuration-options"></a>Opciones de configuración de aplicación

En el código, inicializa un nuevo cliente público o confidencial (o el agente de usuario para MSAL.js) la aplicación para autenticar y obtener tokens. Puede establecer un número de opciones de configuración al inicializar la aplicación de cliente en la biblioteca de autenticación de Microsoft (MSAL). Estas opciones se dividen en dos grupos:

- Opciones de registro, incluidos:
    - [Autoridad](#authority) (formado por el proveedor de identidades [instancia](#cloud-instance) e inicio de sesión [audiencia](#application-audience) para la aplicación y, posiblemente, el identificador del inquilino).
    - [Id. de cliente](#client-id).
    - [URI de redirección](#redirect-uri).
    - [secreto de cliente](#client-secret) (para aplicaciones de cliente confidencial).
- [Opciones de registro](#logging), incluido el nivel de registro, control de los datos personales y el nombre del componente mediante la biblioteca.

## <a name="authority"></a>Autoridad
La entidad es una dirección URL que indica un directorio que MSAL puede solicitar tokens de. Autoridades comunes son:

- https://login.microsoftonline.com/&lt; inquilino&gt;/, donde &lt;inquilino&gt; es el identificador de inquilino del inquilino de Azure Active Directory (Azure AD) o un dominio asociado a este inquilino de Azure AD. Se usa solo para iniciar sesión en los usuarios de una organización específica.
- https://login.microsoftonline.com/common/. Se usa para iniciar sesión en los usuarios con el trabajo y cuentas educativas o cuentas personales de Microsoft.
- https://login.microsoftonline.com/organizations/. Se usa para iniciar sesión en los usuarios con cuentas profesionales y educativas.
- https://login.microsoftonline.com/consumers/. Se usa para los usuarios inicien sesión con sólo cuentas personales de Microsoft (anteriormente conocidas como cuentas de Windows Live ID).

La configuración de la entidad debe ser coherente con lo que se declara en el portal de registro de aplicación.

La dirección URL de la entidad se compone de la instancia y el público.

Puede ser la entidad:
- Una entidad en la nube de Azure AD.
- una entidad de Azure AD B2C. Consulte [características B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Una entidad de servicios de federación de Active Directory (ADFS). Consulte [compatible con ADFS](https://aka.ms/msal-net-adfs-support).

Las entidades en la nube de Azure AD tienen dos partes:
- el proveedor de identidades *instancia*
- El inicio de sesión *audiencia* para la aplicación

La instancia y la audiencia pueden concatenan y se proporcionan como la dirección URL de la entidad. En versiones anteriores a MSAL 3 de MSAL.NET. *x*, tenía que componen la autoridad usted mismo, basándose en la nube que desea tener como destino y la audiencia de inicio de sesión.  Este diagrama muestra cómo se compone la dirección URL de la entidad de certificación:

![¿Cómo se compone la dirección URL de la entidad de certificación](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Instancia en la nube
El *instancia* se utiliza para especificar si la aplicación que inicia sesión a los usuarios de la nube pública de Azure o de nubes nacionales. Con MSAL en el código, puede establecer la instancia de Azure en la nube mediante el uso de una enumeración o pasando la dirección URL para el [instancia de la nube nacional](authentication-national-cloud.md#azure-ad-authentication-endpoints) como el `Instance` miembro (si lo sabe).

MSAL.NET se iniciará una excepción explícita si ambos `Instance` y `AzureCloudInstance` se especifican.

Si no se especifica una instancia, la aplicación dirigirá a la instancia de nube pública de Azure (la instancia de la dirección URL `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Audiencia de la aplicación

La audiencia de inicio de sesión depende de las necesidades empresariales de la aplicación:
- Si es una línea de desarrollador de negocio (LOB), probablemente podrá producir una aplicación de inquilino único que se usará solo en su organización. En ese caso, deberá especificar la organización, ya sea por su identificador de inquilino (Id. de la instancia de Azure AD) o por un nombre de dominio asociado con la instancia de Azure AD.
- Si es un ISV, debe iniciar sesión en los usuarios con sus cuentas profesionales y educativas de cualquier organización o en algunas organizaciones (aplicación multiinquilino). Pero también puede tener los usuarios iniciar sesión con sus cuentas personales de Microsoft.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Cómo especificar la audiencia en la configuración de código
Con MSAL en el código, especificar la audiencia mediante uno de los siguientes valores:
- La enumeración de audiencia de autoridad de Azure AD
- El identificador del inquilino, que puede ser:
  - Un GUID (identificador de la instancia de Azure AD), para las aplicaciones de inquilino único
  - Un nombre de dominio asociado a la instancia de Azure AD (también para aplicaciones de inquilino único)
- Uno de estos marcadores de posición como un identificador de inquilino en lugar de la enumeración de audiencia de autoridad de Azure AD:
    - `organizations` para una aplicación multiinquilino
    - `consumers` Para iniciar sesión en los usuarios sólo con sus cuentas personales
    - `common` Para iniciar sesión en los usuarios con su trabajo y cuentas educativas o sus cuentas personales de Microsoft

MSAL producirá una excepción significativa si especifica la audiencia de autoridad de Azure AD y el identificador del inquilino.

Si no se especifica una audiencia, dirigirá la aplicación Azure AD y las cuentas Microsoft personales como una audiencia. (Es decir, se comportará como si `common` se especificaron.)

### <a name="effective-audience"></a>Audiencia efectivo
La audiencia para su aplicación eficaz será el mínimo (si hay una intersección) de la audiencia que establezca en la aplicación y la audiencia que se especifica en el registro de aplicación. De hecho, el [registros de aplicaciones](https://aka.ms/appregistrations) experiencia le permite especificar la audiencia (los tipos de cuenta compatibles) para la aplicación. Para más información, consulte [Inicio rápido: Registrar una aplicación con la plataforma Microsoft identity](quickstart-register-app.md).

Actualmente, la única manera de obtener una aplicación para iniciar sesión en los usuarios con cuentas personales de Microsoft es configurar ambas configuraciones:
- Establezca la audiencia del registro de aplicación en `Work and school accounts and personal accounts`.
- Establecer el público en su código y configuración para `AadAuthorityAudience.PersonalMicrosoftAccount` (o `TenantID` = "consumers").

## <a name="client-id"></a>Id. de cliente
El identificador de cliente es el identificador de aplicación único (cliente) asignado a la aplicación cuando se registró la aplicación de Azure AD.

## <a name="redirect-uri"></a>URI de redireccionamiento
La redirección URI es el URI que se le enviará el proveedor de identidades que para realizar copias de los tokens de seguridad.

### <a name="redirect-uri-for-public-client-apps"></a>URI de redirección para aplicaciones de cliente público
Si eres un desarrollador de aplicaciones de cliente público que está usando MSAL:
- No es necesario pasar `RedirectUri` porque se calcula automáticamente, MSAL. Esta redirección URI se establece en uno de estos valores, dependiendo de la plataforma:
   - `urn:ietf:wg:oauth:2.0:oob` para todas las plataformas de Windows
   - `msal{ClientId}://auth` para iOS y Xamarin Android

- Deberá configurar la redirección URI en [registros de aplicaciones](https://aka.ms/appregistrations):

   ![URI de redirección en registros de aplicaciones](media/msal-client-application-configuration/redirect-uri.png)

Puede invalidar el URI de redireccionamiento mediante el `RedirectUri` propiedad (por ejemplo, si utiliza agentes). Estos son algunos ejemplos de URI de redireccionamiento para ese escenario:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth.{Bundle.ID}://auth";

Para obtener más información, consulte el [documentación para iOS y Android](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>URI de redirección de las aplicaciones cliente confidencial
Para las aplicaciones web, el URI de redireccionamiento (o URI de respuesta) es el URI que se utilizará para enviar el token a la aplicación de Azure AD. Esto puede ser la dirección URL de la API web app/Web si la aplicación confidencial es uno de ellos. La redirección URI debe estar registrada en el registro de la aplicación. Este registro es especialmente importante al implementar una aplicación que inicialmente ha probado localmente. A continuación, deberá agregar la dirección URL de respuesta de la aplicación implementada en el portal de registro de aplicación.

Para las aplicaciones de demonio, no es necesario especificar un URI de redirección.

## <a name="client-secret"></a>Secreto del cliente
Esta opción especifica el secreto de cliente para la aplicación cliente confidencial. Este secreto (contraseña de aplicación) es proporcionada por el portal de registro de aplicación o proporciona a Azure AD durante el registro de aplicación con PowerShell AzureAD, AzureRM de PowerShell o CLI de Azure.

## <a name="logging"></a>Registro
Las otras opciones de configuración Habilitar el registro y solución de problemas. Consulte la [registro](msal-logging.md) para obtener información sobre cómo usarlas.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre [crear instancias de las aplicaciones cliente mediante el uso de MSAL.NET](msal-net-initializing-client-applications.md).

Obtenga información sobre [crear instancias de las aplicaciones cliente mediante el uso de MSAL.js](msal-js-initializing-client-applications.md).
