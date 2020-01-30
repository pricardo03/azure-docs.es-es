---
title: Registro de aplicaciones móviles que llaman a las API web | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web (configuración del código de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 33510015f4f05661ad2ea041b1fd3da0e8bfb1ed
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702086"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Aplicación móvil que llama a las API web: registro de la aplicación

En este artículo se incluyen las instrucciones del registro de la aplicación para crear una aplicación móvil.

## <a name="supported-accounts-types"></a>Tipos de cuentas compatibles

Los tipos de cuenta que se admiten en aplicaciones móviles dependen de la experiencia que quiera habilitar y los flujos que quiera usar.

### <a name="audience-for-interactive-token-acquisition"></a>Público para la adquisición interactiva de tokens

La mayoría de las aplicaciones móviles usan la autenticación interactiva. En ese caso, puede iniciar sesión de usuarios desde cualquier [tipo de cuenta](quickstart-register-app.md#register-a-new-application-using-the-azure-portal).

### <a name="audience-for-integrated-authentication-usernamepassword-and-b2c"></a>Audiencia para la autenticación integrada, el nombre de usuario y contraseña y B2C

- Si piensa utilizar la autenticación integrada de Windows (posible en aplicaciones de UWP) o el nombre de usuario o la contraseña, la aplicación debe iniciar sesión de usuarios en su propio inquilino (desarrollador LOB), o bien en organizaciones de Azure Active Directory (escenario de ISV). Estos flujos de autenticación no son compatibles con las cuentas personales de Microsoft.
- Si inicia sesión de usuarios con identidades sociales que pasan una autoridad y directiva B2C, solo puede usar la autenticación interactiva y de nombre de usuario y contraseña. El nombre de usuario y la contraseña solo se admite actualmente en Xamarin.iOS, Xamarin.Android y UWP.

Para obtener una visión general, consulte [Escenarios y flujos de autenticación compatibles](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows) y [Escenarios, plataformas y lenguajes compatibles](authentication-flows-app-scenarios.md#scenarios-and-supported-platforms-and-languages).

## <a name="platform-configuration-and-redirect-uris"></a>Configuración de la plataforma e identificadores URI de redirección  

### <a name="interactive-authentication"></a>Autenticación interactiva

Al compilar una aplicación móvil con autenticación interactiva, el paso de registro más importante es el URI de redirección. Este se puede establecer a través de la [configuración de la plataforma en la hoja Autenticación](https://aka.ms/MobileAppReg).

Esta experiencia permitirá a la aplicación obtener el inicio de sesión único (SSO) a través de Microsoft Authenticator (y el Portal de empresa de Intune en Android), así como las directivas de administración de dispositivos de soporte técnico.

Tenga en cuenta que hay una experiencia de versión preliminar en el portal de registro de aplicaciones para ayudarle a calcular el URI de respuesta asincrónico para aplicaciones iOS y Android:

1. En el registro de la aplicación, elija **Autenticación** y seleccione **Probar la nueva experiencia**
   ![image](https://user-images.githubusercontent.com/13203188/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. Seleccione **Agregar plataforma**
   ![image](https://user-images.githubusercontent.com/13203188/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Cuando se admita la lista de plataformas, seleccione **iOS**
   ![image](https://user-images.githubusercontent.com/13203188/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Escriba el identificador del conjunto que se solicita y, a continuación, presione **Registrar**
   ![image](https://user-images.githubusercontent.com/13203188/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

5. El URI de redirección se calcula automáticamente.
   ![image](https://user-images.githubusercontent.com/13203188/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

Si prefiere configurar manualmente el URI de redirección, puede hacerlo mediante el manifiesto de aplicación. El formato recomendado es el siguiente:

- ***iOS***: `msauth.<BUNDLE_ID>://auth` (por ejemplo "msauth.com.empresa.aplicación://auth")
- ***Android***: `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - El hash de firma de Android puede generarse utilizando la combinación de teclas de liberación o de depuración a través del comando KeyTool.

### <a name="username-password"></a>Nombre de usuario y contraseña

Si la aplicación solo utiliza el nombre de usuario y contraseña, no es necesario que registre ningún URI de redirección para la aplicación. De hecho, este flujo hace un recorrido de ida y vuelta al punto de conexión de la Plataforma de identidad de Microsoft v2.0 y no se volverá a llamar a su aplicación en ningún URI específico. Sin embargo, tiene que expresar que la aplicación es una aplicación cliente pública. Para lograr esta configuración, vaya a la sección **Autenticación** de su aplicación y, en la subsección **Configuración avanzada**, elija **Sí** en la afirmación **Treat application as a public client** (Tratar la aplicación como un cliente público) (en el párrafo **Tipo de cliente predeterminado**).

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones móviles llaman a las API en nombre del usuario con sesión iniciada. La aplicación debe solicitar permisos delegados, que también se denominan ámbitos. En función de la experiencia deseada, puede hacerse estáticamente a través de Azure Portal o dinámicamente en tiempo de ejecución. El registro estático de permisos permite a los administradores aprobar fácilmente su aplicación y se recomienda.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración del código](scenario-mobile-app-configuration.md)
