---
title: Administración de usuarios y dispositivos por parte de los administradores - Azure MFA - Azure Active Directory
description: Describe cómo los administradores cambian las opciones de la configuración del usuario, como por ejemplo forzar a los usuarios realizar nuevamente el proceso proofup.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 190d697dca56fa51d92987f32db0146aa79881aa
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70162394"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Administrar la configuración de usuario con Azure Multi-Factor Authentication en la nube

Como administrador, puede administrar las siguientes opciones de configuración de usuario y de dispositivo:

* Requerir a los usuarios que vuelvan a proporcionar métodos de contacto
* Eliminar contraseñas de aplicación
* Requerir MFA en todos los dispositivos de confianza

## <a name="manage-authentication-methods"></a>Administración de los métodos de autenticación

Como administrador que tiene asignado el rol de administrador de autenticación, puede requerir a los usuarios que restablezcan su contraseña, que vuelvan a registrarse para MFA o que revoquen las sesiones de MFA existentes desde su objeto de usuario.

![Administración de los métodos de autenticación desde Azure Portal](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Elija el usuario en el que quiere realizar una acción y seleccione **Métodos de autenticación**.
   - **Restablecer contraseña** restablecerá la contraseña del usuario y asignará una contraseña temporal que se debe cambiar en el siguiente inicio de sesión.
   - **Requerir volver a registrar MFA** hará que cuando el usuario inicie sesión la próxima vez, se le solicite que configure un nuevo método de autenticación MFA.
   - **Revocar sesiones de MFA** borra las sesiones de MFA recordadas del usuario y le pide que realice MFA la próxima vez que la directiva lo exija en el dispositivo.

## <a name="require-users-to-provide-contact-methods-again"></a>Requerir a los usuarios que vuelvan a proporcionar métodos de contacto

Esta configuración obliga al usuario a volver a realizar el proceso de registro. Las aplicaciones sin explorador continúan funcionando si el usuario tiene las contraseñas de aplicación para ellas.  Puede eliminar las contraseñas de aplicación de los usuarios seleccionando también **Eliminar todas las contraseñas de aplicación existentes generadas por los usuarios seleccionados**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Cómo requerir a los usuarios seleccionados que vuelvan a proporcionar métodos de contacto

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
3. En la derecha, seleccione **Multi-factor Authentication** en la barra de herramientas. Se abre la página de Multi-Factor Authentication.
4. Active la casilla junto al usuario o usuarios que desea administrar. Aparecerá una lista de opciones de paso rápido a la derecha.
5. Seleccione **Administrar configuración de usuario**.
6. Active la casilla **Requerir a los usuarios seleccionados que vuelvan a proporcionar métodos de contacto**.
   ![Requerir a los usuarios que vuelvan a proporcionar métodos de contacto](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Haga clic en **guardar**.
8. Haga clic en **Cerrar**.

Las organizaciones pueden seguir estos pasos con PowerShell utilizando la siguiente información como guía para borrar el atributo `StrongAuthenticationMethods`:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>Eliminar las contraseñas de aplicación existentes de los usuarios

Esta configuración elimina todas las contraseñas de aplicación que ha creado un usuario. Las aplicaciones sin explorador asociadas con estas contraseñas de aplicación dejan de funcionar hasta que se cree una nueva contraseña de aplicación.

### <a name="how-to-delete-users-existing-app-passwords"></a>Cómo eliminar las contraseñas de aplicación existentes de los usuarios

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
3. En la derecha, seleccione **Multi-factor Authentication** en la barra de herramientas. Se abre la página de Multi-Factor Authentication.
4. Active la casilla junto al usuario o usuarios que desea administrar. Aparecerá una lista de opciones de paso rápido a la derecha.
5. Seleccione **Administrar configuración de usuario**.
6. Active la casilla **Eliminar todas las contraseñas de aplicación existentes generadas por los usuarios seleccionados**.
   ![Eliminar todas contraseñas de aplicación existentes](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. Haga clic en **guardar**.
8. Haga clic en **Cerrar**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Restauración de MFA en todos los dispositivos recordados de un usuario

Una de las características configurables de Azure Multi-Factor Authentication es dar la opción a los usuarios de marcar los dispositivos como de confianza. Para más información, vea [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Los usuarios pueden optar por no usar la verificación en dos pasos durante un número de días configurable en sus dispositivos habituales. Si se compromete la seguridad de la cuenta o se pierde un dispositivo de confianza, necesita tener la posibilidad de quitar el estado de confianza y volver a solicitar la verificación en dos pasos.

Cuando esté activada, se solicita a los usuarios de **Restaurar autenticación multifactor en todos los dispositivos recordados** que realicen la verificación en dos pasos la próxima vez que inicien sesión, incluso si se marcan su dispositivo como de confianza.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Cómo restaurar MFA en todos los dispositivos suspendidos para un usuario

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la parte izquierda, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
3. En la derecha, seleccione **Multi-factor Authentication** en la barra de herramientas. Se abre la página de Multi-Factor Authentication.
4. Active la casilla junto al usuario o usuarios que desea administrar. Aparecerá una lista de opciones de paso rápido a la derecha.
5. Seleccione **Administrar configuración de usuario**.
6. Active la casilla **Restaurar autenticación multifactor en todos los dispositivos recordados**
   ![Restaurar autenticación multifactor en todos los dispositivos recordados ](./media/howto-mfa-userdevicesettings/rememberdevices.png).
7. Haga clic en **guardar**.
8. Haga clic en **Cerrar**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
- Si los usuarios necesitan ayuda, diríjalos a la [Guía de usuario para la verificación en dos pasos](../user-help/multi-factor-authentication-end-user.md)
