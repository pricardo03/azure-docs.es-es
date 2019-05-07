---
title: 'Aplicación de escritorio que llama a web API (registro de aplicación): la plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación de escritorio que llama a web API (registro de aplicación)
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
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5da934709274d90668d94dfea3a9c223e191d032
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076066"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Aplicación de escritorio que llama a web API: registro de la aplicación

En este artículo contiene las particularidades del registro de aplicación para una aplicación de escritorio.

## <a name="supported-accounts-types"></a>Tipos de las cuentas admitidas

La experiencia que desea luminosas dependen de los tipos de cuenta compatibles en la aplicación de escritorio, y por lo tanto, en los flujos que desea utilizar.

### <a name="audience-for-interactive-token-acquisition"></a>Audiencia de adquisición de tokens interactivo

Si su aplicación de escritorio usa la autenticación interactiva, puede iniciar sesión en los usuarios desde cualquier [tipo de cuenta](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)

### <a name="audience-for-desktop-app-silent-flows"></a>Audiencia de flujos silenciosa de aplicación de escritorio

- Si piensa utilizar la autenticación de Windows integrada o nombre de usuario/contraseña, la aplicación necesita iniciar sesión en los usuarios en su propio inquilino (desarrollador LOB), o bien en organizaciones de Azure Active directory (escenario de ISV). Estos flujos de autenticación no se admiten para las cuentas personales de Microsoft
- Si desea usar el flujo de código de dispositivo, no puedes iniciar sesión en los usuarios con sus cuentas personales de Microsoft aún
- Si inicia sesión en los usuarios con identidades de redes sociales pasando una autoridad de B2C y una directiva, sólo puede utilizar la autenticación interactiva y la contraseña de nombre de usuario.

## <a name="redirect-uris"></a>URI de redirección

Vuelva a los URI para usar en la aplicación de escritorio de redireccionamiento dependerá el flujo que desea usar.

- Si usa la autenticación interactiva, deseará usar `https://login.microsoftonline.com/common/oauth2/nativeclient`. Conseguirá esta configuración, haga clic en la dirección URL correspondiente en el **autenticación** sección de la aplicación
  
  > [!IMPORTANT]
  > Hoy en día MSAL.NET usa otro URI de redirección de forma predeterminada en las aplicaciones de escritorio que se ejecutan en Windows (`urn:ietf:wg:oauth:2.0:oob`). En el futuro queremos cambiar este comportamiento predeterminado y, por lo tanto, recomendamos que use `https://login.microsoftonline.com/common/oauth2/nativeclient`

- Si la aplicación solo utiliza la autenticación de Windows integrada, nombre de usuario/contraseña o el flujo de código de dispositivo, no es necesario registrar un URI de redirección para la aplicación. De hecho, estos flujos no ida y vuelta al extremo de v2.0 de plataforma de identidad de Microsoft y la aplicación no llamarse en cualquier URI específico. Con el fin de distinguir desde un flujo de la aplicación cliente confidencial, que no tiene URI de redireccionamiento cualquier (la credencial flujo de cliente puede usado en aplicaciones de demonio), deberá express que la aplicación es una aplicación cliente pública. Esta configuración se logra yendo a la **autenticación** sección para la aplicación y, en el **configuración avanzada** subsección, elija **Sí**, a la pregunta **Tratar la aplicación como un cliente público** (en el **tipo de cliente predeterminado** párrafo)

  ![Permitir que a un cliente público](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones de escritorio, llamar a las API en nombre del usuario con sesión iniciada. Deben solicitar permisos delegados. No se solicitan permisos de la aplicación (que sólo se controlan en [aplicaciones demonio](scenario-daemon-overview.md))

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de escritorio: configuración de la aplicación](scenario-desktop-app-configuration.md)
