---
title: Obtenga información sobre cómo integrar la Biblioteca de autenticación de Microsoft (MSAL) en Azure AD B2C
description: La Biblioteca de autenticación de Microsoft (MSAL) permite que los desarrolladores de aplicaciones la integren en Azure AD B2C y adquieran tokens para llamar a API web protegidas. Estas API web pueden ser Microsoft Graph, otras API de Microsoft, API web de terceros o su propia API web.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87bd8834f840c2246bf3adc1d1f9cd9b8f635915
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191017"
---
# <a name="integrate-microsoft-authentication-library-msal-with-azure-active-directory-b2c"></a>Integración de la Biblioteca de autenticación de Microsoft (MSAL) con Azure Active Directory B2C

La Biblioteca de autenticación de Microsoft (MSAL) permite a los desarrolladores de aplicaciones autenticar a los usuarios con identidades sociales y locales mediante [Azure Active Directory (Azure AD) B2C](https://docs.microsoft.com/azure/active-directory-b2c/). Azure Active Directory (Azure AD) B2C es un servicio de administración de identidades que le permite personalizar y controlar la manera en que los clientes se registran, inician sesión y administran sus perfiles al usar las aplicaciones.

Azure Active Directory (Azure AD) B2C también permite marcar y personalizar la interfaz de usuario (UI) de sus aplicaciones para proporcionar una experiencia perfecta a su cliente.

Este tutorial muestra cómo usar la Biblioteca de autenticación de Microsoft (MSAL) para integrarla en Azure Active Directory (Azure AD) B2C.


## <a name="prerequisites"></a>Requisitos previos

Si aún no ha creado su propio [inquilino de Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), cree una ahora. Puede usar un inquilino de Azure AD B2C existente. 

## <a name="javascript"></a>Javascript

En los siguientes pasos se muestra cómo una aplicación de página única puede usar Azure AD B2C para el registro del usuario, el inicio de sesión y llamar a una API web protegida.

### <a name="step-1-register-your-application"></a>Paso 1: Registrar su aplicación

Para implementar la autenticación, primero debe registrar la aplicación. Para registrar la aplicación, siga la sección de [registro de la aplicación](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) para ver los pasos detallados.

### <a name="steps-2-download-applications"></a>Paso 2: Descargar las aplicaciones

Descargue un archivo ZIP o clone el ejemplo de GitHub.
>Clonación de git https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git

### <a name="steps-3-authentication"></a>Paso 3: Authentication

1. Abra el archivo index.html en el ejemplo.

2. Configure el ejemplo con el identificador y la clave de la aplicación que registró anteriormente mientras registraba la aplicación. Cambie las siguientes líneas de código reemplazando los valores por los nombres del directorio y las API:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>",
        b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/hello/demo.read"],
        webApi: 'http://localhost:5000/hello',
  };

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

El nombre del [flujo de usuario](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) que se ha utilizado en este tutorial es B2C_1_signupsignin1. Si va a usar otro, úselo en el valor de autoridad.


### <a name="configure-application-to-use-b2clogincom"></a>Configuración de la aplicación para usar `b2clogin.com`

Puede usar `b2clogin.com` en lugar de `login.microsoftonline.com` como dirección URL de redireccionamiento cuando se configura un proveedor de identidades para registrarse e iniciar sesión en la aplicación Azure Active Directory (Azure AD) B2C.

**`b2clogin.com`**, es decir, 
`https://your-tenant-name.b2clogin.com/your-tenant-guid`, se usa para lo siguiente:

- Se reduce el espacio que los servicios de Microsoft consumen en el encabezado de las cookies.
- Las direcciones URL ya no incluyen una referencia a Microsoft. Por ejemplo, la aplicación de Azure AD B2C probablemente hace referencia en login.microsoftonline.com.


 Para usar "b2clogin.com", deberá actualizar la configuración de la aplicación.  

1. Actualice ValidateAuthority: establezca la propiedad **ValidateAuthority** en `false`. Si **ValidateAuthority** se establece en "false", se permiten los redireccionamientos a b2clogin.com.

En el ejemplo siguiente se muestra cómo se puede establecer la propiedad:
```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> La aplicación de Azure AD B2C probablemente hace referencia a login.microsoftonline.com en varios lugares, tales como las referencias de flujos de usuario y puntos de conexión de token. Asegúrese de que el punto de conexión de autorización, el punto de conexión del token y el emisor se han actualizado para utilizar tenant-name.b2clogin.com.

Siga este [ejemplo de MSAL JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) sobre cómo usar la versión preliminar de la Biblioteca de autenticación de Microsoft para JavaScript (msal.js) con el fin de obtener un token de acceso y llamar a una API protegida por Azure AD B2C.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Directivas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Personalización de la interfaz de usuario](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)