---
title: 'Iniciar sesión sin contraseña con la aplicación Microsoft Authenticator (versión preliminar): Azure Active Directory'
description: Inicio de sesión en Azure AD mediante la aplicación Microsoft Authenticator sin usar la contraseña (versión preliminar pública)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47e8541b82a1cd38f07684508a96b9789df20e92
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370396"
---
# <a name="password-less-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Inicio de sesión telefónico sin contraseña con la aplicación Microsoft Authenticator (versión preliminar pública)

La aplicación Microsoft Authenticator se puede utilizar para iniciar sesión en cualquier cuenta de Azure AD sin utilizar una contraseña. Similar a la tecnología de [Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator usa la autenticación basada en claves para habilitar una credencial de usuario que está asociada a un dispositivo y usa un sistema biométrico o un PIN.

![Ejemplo de un inicio de sesión de explorador pidiendo al usuario que apruebe el inicio de sesión](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

En lugar de ver la petición de una contraseña después de escribir un nombre de usuario, una persona que ha habilitado el inicio de sesión por teléfono en la aplicación Microsoft Authenticator verá un mensaje que le solicita pulsar un número en la aplicación. En la aplicación, el usuario debe hacer coincidir el número, elegir Aprobar y, después, proporcionar el PIN o información biométrica para que la autenticación se complete.

## <a name="enable-my-users"></a>Habilitación de los usuarios

En la versión preliminar pública, un administrador debe agregar una directiva mediante Powershell para permitir el uso de la credencial en el inquilino. Revise la sección "Problemas conocidos" antes de realizar este paso.

### <a name="tenant-prerequisites"></a>Requisitos previos del inquilino

* Azure Active Directory
* Usuarios finales habilitados para Azure Multi-Factor Authentication
* Los usuarios pueden registrar sus dispositivos

### <a name="steps-to-enable"></a>Pasos para la habilitación

1. Asegúrese de tener instalada la versión más reciente de la versión preliminar pública del módulo de PowerShell de Azure Active Directory V2. Puede que desee desinstalarla y reinstalarla para confirmarlo; para ello, ejecute los comandos siguientes:

    ```powershell
    Uninstall-Module -Name AzureADPreview
    Install-Module -Name AzureADPreview
    ```

2. Autentíquese en el inquilino de Azure AD para usar el módulo de PowerShell de Azure AD V2. La cuenta utilizada debe ser un administrador de seguridad o un administrador global.

    ```powershell
    Connect-AzureAD
    ```

3. Cree la directiva de inicio de sesión de autenticador:

    ```powershell
    New-AzureADPolicy -Type AuthenticatorAppSignInPolicy -Definition '{"AuthenticatorAppSignInPolicy":{"Enabled":true}}' -isOrganizationDefault $true -DisplayName AuthenticatorAppSignIn
    ```

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>¿Cómo habilitan los usuarios finales el inicio de sesión telefónico?

En la versión preliminar pública, no hay ninguna manera para exigir a los usuarios que creen o utilicen esta nueva credencial. Un usuario final solo se encontrará con un inicio de sesión sin contraseña una vez que un administrador haya habilitado su inquilino y el usuario haya actualizado la aplicación Microsoft Authenticator para habilitar el inicio de sesión telefónico.

> [!NOTE]
> Esta funcionalidad existe en la aplicación desde marzo de 2017, por lo que existe la posibilidad de que cuando la directiva está habilitada para un inquilino, los usuarios puedan encontrarse con este flujo de inicio de sesión inmediatamente. Tenga esto en cuenta y prepare a los usuarios para este cambio.
>

1. Inscripción en Azure Multi-factor Authentication
1. Versión más reciente de Microsoft Authenticator instalada en dispositivos que ejecutan iOS 8.0 o superior o Android 6.0 o superior.
1. Cuenta profesional o educativa con notificaciones push agregada a la aplicación. La documentación del usuario final se puede encontrar en [https://aka.ms/authappstart](https://aka.ms/authappstart).

Una vez que el usuario tiene la cuenta de MFA con notificaciones push configurada en la aplicación Microsoft Authenticator, puede seguir los pasos descritos en el artículo [Inicie sesión con su teléfono, no con su contraseña](../user-help/microsoft-authenticator-app-phone-signin-faq.md) para completar el registro de inicio de sesión telefónico.

## <a name="known-issues"></a>Problemas conocidos

### <a name="ad-fs-integration"></a>Integración de AD FS

Cuando un usuario ha habilitado la credencial sin contraseña de Microsoft Authenticator, la autenticación predeterminada para ese usuario siempre será enviar una notificación para su aprobación. Esta lógica impide que los usuarios de un inquilino híbrido sean dirigidos a ADFS para la comprobación de inicio de sesión sin que el usuario tome un paso adicional para hacer clic en "Usar la contraseña en su lugar". En este proceso también se omitirán las directivas de acceso condicional del entorno local y los flujos de autenticación de paso a través. La excepción a este proceso se da cuando se especifica un login_hint, entonces un usuario será reenviado de manera automática a AD FS y se omitirá la opción de usar la credencial sin contraseña.

### <a name="azure-mfa-server"></a>Servidor de Azure MFA

Los usuarios finales que están habilitados para MFA a través de un servidor de Azure MFA local de la organización pueden crear y usar una credencial de inicio de sesión único telefónica sin contraseña. Si el usuario intenta actualizar varias instalaciones (más de 5) de Microsoft Authenticator con la credencial, este cambio puede dar lugar a un error.  

### <a name="device-registration"></a>Registro de dispositivos

Uno de los requisitos previos para crear esta nueva credencial segura es que el dispositivo en el que reside esté registrado en el inquilino de Azure AD para un usuario individual. Debido a las restricciones del registro de dispositivos, solo se puede registrar un dispositivo en un inquilino. Este límite significa que solo se puede habilitar una cuenta profesional o educativa en la aplicación Microsoft Authenticator para el inicio de sesión telefónico.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre el registro de dispositivos](../devices/overview.md#getting-devices-under-the-control-of-azure-ad)

[Más información sobre Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
