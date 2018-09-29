---
title: 'Introducción a la aplicación Microsoft Authenticator: Azure Active Directory | Microsoft Docs'
description: Aprenda a actualizar a la última versión de Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 590f9e2a9cf531a1124f9cb132791f2956227d9c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046584"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Introducción a la aplicación Microsoft Authenticator

La aplicación Microsoft Authenticator ayuda a prevenir el acceso no autorizado a cuentas y a detener las transacciones fraudulentas al aportar un nivel adicional de seguridad para su cuenta profesional o educativa (por ejemplo, alain@contoso.com) o a su cuenta Microsoft personal (por ejemplo, alain@outlook.com).

Cuando se usa la aplicación para la verificación en dos pasos, puede funcionar de dos formas:

- **Notificación.** La aplicación envía una notificación al dispositivo. Asegúrese de que la notificación es correcta y luego seleccione **Verificar**. Si no reconoce la notificación, seleccione **Denegar**.

- **Código de verificación.** Después de escribir su nombre de usuario y contraseña, puede abrir la aplicación y copiar el código de verificación proporcionado en la pantalla **Cuentas** de la ventana de inicio de sesión. El código de verificación actúa como una forma adicional de autenticación.

## <a name="opt-in-for-two-step-verification"></a>Suscripción a la comprobación en dos pasos

Su organización decide si utilizar la verificación en dos pasos con su cuenta profesional o educativa. El administrador le permitirá saber qué métodos de verificación se deben configurar y usar. Para más información, consulte [¿Qué relevancia tiene Azure Multi-Factor Authentication para mí?](multi-factor-authentication-end-user.md)

Para su cuenta Microsoft personal, puede configurar la verificación en dos pasos por su cuenta. Para obtener información e instrucciones detalladas, vea [Acerca de la verificación en dos pasos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

También puede usar la aplicación Microsoft Authenticator con cuentas que no sean de Microsoft. Puede que esas cuentas llamen a la característica de verificación en dos pasos de otra manera, pero lo más seguro es que la encuentre en la configuración de seguridad o de inicio de sesión. Para más información sobre cómo configurar estas cuentas que no son de Microsoft, vea los [vídeos de soporte técnico de Microsoft](https://www.youtube.com/playlist?list=PLyhj1WZ29G65QdD9NxTOAm8HwOS-OBUrX).

## <a name="install-the-app"></a>Instalación de la aplicación

La aplicación Microsoft Authenticator está disponible para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) y [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071). Para obtener los mejores resultados, debe permitir que la aplicación reciba notificaciones cuando se le pida. 

## <a name="add-accounts-to-the-app"></a>Incorporación de cuentas a la aplicación

Puede agregar cuentas profesionales o educativas o cuentas personales a la aplicación Microsoft Authenticator. 

### <a name="add-a-personal-microsoft-account"></a>Adición de una cuenta Microsoft personal

Para una cuenta Microsoft personal (una que use para iniciar sesión en Outlook.com, Xbox, Skype, etc.), lo único que debe hacer es iniciar sesión en ella, en la aplicación Microsoft Authenticator.

### <a name="add-a-work-or-school-account"></a>Adición de una cuenta profesional o educativa

1. Si es posible, vaya a la pantalla [Comprobación de seguridad adicional](http://aka.ms/mfasetup) en otro equipo o dispositivo. Para información sobre cómo ir a esta pantalla, consulte la sección sobre el [cambio de la configuración de seguridad](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) o póngase en contacto con su administrador.

    >[!Note]
    >Si el administrador ha activado la experiencia de versión preliminar de la información de seguridad, puede seguir las instrucciones de [Configuración de la información de seguridad para usar una aplicación autenticadora](security-info-setup-auth-app.md).

2. Active la casilla situada junto a **Aplicación Authenticator** y luego seleccione **Configurar**.

    ![Botón Configurar en la pantalla de configuración de la comprobación de seguridad](./media/microsoft-authenticator-app-how-to/auth-app-configure.png)

    En la pantalla **Configurar aplicación móvil** aparece un código QR que debe escanear con la aplicación Authenticator.

    ![Pantalla que proporciona el código QR](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Abra la aplicación Microsoft Authenticator. En la pantalla **Cuentas**, seleccione **Agregar cuenta** y luego seleccione **Cuenta profesional o educativa**.

4. Utilice la cámara del dispositivo para digitalizar el código QR y seleccione **Listo** para cerrar la pantalla de código QR.

    >[!Note]
    >Si la cámara no funciona correctamente, puede [escribir manualmente la dirección URL y el código QR](#add-an-account-to-the-app-manually).

    En la pantalla **Cuentas** de la aplicación aparece el nombre de su cuenta y un código de verificación de seis dígitos. Para mayor seguridad, el código de verificación cambia cada 30 segundos para impedir que se pueda usar el mismo código dos veces.  

    ![Pantalla Cuentas](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Incorporación manual de una cuenta a la aplicación

1. Vaya a la pantalla **Comprobación de seguridad adicional**. Para información sobre cómo ir a esta pantalla, consulte la sección sobre el [cambio de la configuración de seguridad](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).

2. Active la casilla situada junto a **Aplicación Authenticator** y luego seleccione **Configurar**.

    Aparece la pantalla **Configurar aplicación móvil**.

3. Copie el código y la información de la dirección URL de la pantalla **Configurar aplicación móvil** para poder escribirlas manualmente en el escáner de códigos QR.

4. Abra la aplicación Microsoft Authenticator. En la pantalla **Cuentas**, seleccione **Agregar cuenta** y luego seleccione **Cuenta profesional o educativa**.

5. En el escáner de códigos QR, elija **Escribir el código manualmente**.

    ![Pantalla para digitalizar un código QR](./media/microsoft-authenticator-app-how-to/auth-app-manual-code.png)
   
6. Escriba el código y la dirección URL de la pantalla con el código QR en la pantalla **Agregar una cuenta** y luego seleccione **Finalizar**.

    ![Pantalla para escribir el código y la dirección URL](./media/microsoft-authenticator-app-how-to/auth-app-code-url.png)

    En la pantalla **Cuentas** de la aplicación aparece el nombre de su cuenta y un código de verificación de seis dígitos. Para mayor seguridad, el código de verificación cambia cada 30 segundos para impedir que se pueda usar el mismo código dos veces.

### <a name="using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Uso de las funcionalidades de reconocimiento facial o de huella digital del dispositivo

Su organización puede requerir un PIN para completar la verificación de identidad. Puede configurar la aplicación Microsoft Authenticator para usar las funcionalidades de reconocimiento facial o de huella digital del dispositivo en lugar de un PIN. Puede realizar esta configuración la primera vez que use la aplicación Authenticator para verificar su cuenta; para ello, seleccione la opción para usar las funcionalidades biométricas del dispositivo como identificación en lugar del PIN.

## <a name="use-the-app-when-you-sign-in"></a>Uso de la aplicación al iniciar sesión

Después de agregar las cuentas a la aplicación, puede usar la aplicación para iniciar sesión en ellas.

Si eligió usar códigos de verificación en la aplicación, comenzará a verlos en la página **Cuentas**. Los códigos cambian cada 30 segundos, por lo que siempre tendrá un código nuevo cuando lo necesite. Pero no tiene que hacer nada con ellos hasta que inicie sesión y se le solicite que escriba un código de comprobación.

## <a name="next-steps"></a>Pasos siguientes

- Si tiene más preguntas generales sobre la aplicación, consulte [Preguntas más frecuentes de la aplicación Microsoft Authenticator](microsoft-authenticator-app-faq.md).

- Si desea más información sobre la verificación en dos pasos, vea [Configuración de mi cuenta para la verificación en dos pasos](multi-factor-authentication-end-user-first-time.md)

- Si desea más información sobre la información de seguridad, consulte [Manage your security info](security-info-manage-settings.md) (Administrar la información de seguridad)
