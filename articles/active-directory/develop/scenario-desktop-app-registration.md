---
title: 'Registro de una aplicación de escritorio que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación de escritorio que llame a las API web (registro de la aplicación).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94f7f2dfdbf404a092773857a0f7727618cd429a
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965541"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Aplicación de escritorio que llama a las API web: registro de la aplicación

En este artículo se tratan los detalles del registro de una aplicación de escritorio.

## <a name="supported-accounts-types"></a>Tipos de cuentas compatibles

Los tipos de cuenta admitidos en la aplicación de escritorio dependen de la experiencia que quiera destacar. Debido a esta relación, los tipos de cuenta admitidos dependen de los flujos que desee usar.

### <a name="audience-for-interactive-token-acquisition"></a>Público para la adquisición interactiva de tokens

Si su aplicación de escritorio usa la autenticación interactiva, puede iniciar sesión de usuarios de cualquier [tipo de cuenta](quickstart-register-app.md#register-a-new-application-using-the-azure-portal).

### <a name="audience-for-desktop-app-silent-flows"></a>Audiencia para flujos silenciosos de la aplicación de escritorio

- Para usar la autenticación integrada de Windows o el nombre de usuario o la contraseña, la aplicación debe iniciar sesión de usuarios en su propio inquilino (desarrollador LOB), o bien en organizaciones de Azure Active Directory (escenario de ISV). Estos flujos de autenticación no son compatibles con las cuentas personales de Microsoft.
- Si quiere utilizar el flujo de código del dispositivo, aún no puede iniciar sesión de los usuarios con sus cuentas personales de Microsoft.
- Si inicia sesión de usuarios con identidades sociales que pasan una autoridad y directiva B2C, solo puede usar la autenticación interactiva y de nombre de usuario y contraseña.

## <a name="redirect-uris"></a>URI de redirección

Los URI de redireccionamiento que se utilizarán en la aplicación de escritorio dependerán del flujo que quiera utilizar.

- Si usa la **autenticación interactiva** o el **flujo de código de dispositivo** , le recomendamos que use `https://login.microsoftonline.com/common/oauth2/nativeclient`. Para lograr esta configuración, haga clic en la dirección URL correspondiente de la sección **Autenticación** de su aplicación.
  
  > [!IMPORTANT]
  > Hoy en día, MSAL.NET usa otro URI de redirección de forma predeterminada en las aplicaciones de escritorio que se ejecutan en Windows (`urn:ietf:wg:oauth:2.0:oob`). En el futuro, querremos cambiar este comportamiento predeterminado y, por lo tanto, le recomendamos que use `https://login.microsoftonline.com/common/oauth2/nativeclient`.

- Si va a compilar una aplicación nativa de Objective-C o Swift para macOS, será conveniente registrar el valor de redirectUri en función del identificador de lote de la aplicación en el formato siguiente: **msauth.<su.app.bundle.ID>://auth** (reemplazar <su.app.bundle.ID> con el identificador de lote de la aplicación)
- Si la aplicación solo utiliza la autenticación integrada de Windows o el nombre de usuario o contraseña, no es necesario que registre ningún URI de redirección para la aplicación. Estos flujos hacen un recorrido de ida y vuelta al punto de conexión de la plataforma de identidad de Microsoft v2.0 y no se volverá a llamar a su aplicación en ningún URI específico.
- Para distinguir el flujo de código de dispositivo, la autenticación integrada de Windows y el nombre de usuario y la contraseña de un flujo de la aplicación cliente confidencial, que tampoco tiene URI de redireccionamiento (corresponde al flujo de credenciales de cliente usado en aplicaciones de demonio), deberá expresar que se trata de una aplicación cliente pública. Para lograr esta configuración, vaya a la sección **Autenticación** de su aplicación. A continuación, en la subsección **Configuración avanzada**, en el párrafo **Tipo de cliente predeterminado**, seleccione **Sí** para **tratar la aplicación como cliente público**.

  ![Permitir el cliente público](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones de escritorio llaman a las API en nombre del usuario que tiene la sesión iniciada. Tienen que solicitar permisos delegados. Sin embargo, no pueden solicitar permisos de la aplicación, que sólo se controlan en las [aplicaciones de demonio](scenario-daemon-overview.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de escritorio: configuración de la aplicación](scenario-desktop-app-configuration.md)
