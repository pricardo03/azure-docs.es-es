---
title: 'Configuración de la información de seguridad para usar una aplicación autenticadora: Azure Active Directory | Microsoft Docs'
description: Configure su información de seguridad para verificar su identidad con la aplicación Microsoft Authenticator.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 142d3442db9156876430b196a8e15b506db80018
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814467"
---
# <a name="set-up-security-info-to-use-an-authenticator-app-preview"></a>Configuración de la información de seguridad para usar una aplicación autenticadora (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Para configurar la información de seguridad tiene que iniciar sesión en su cuenta profesional o educativa y completar el proceso de registro. Si nunca ha configurado la información de seguridad, se le pedirá que lo haga ahora.

## <a name="set-up-the-microsoft-authenticator-app"></a>Configuración de la aplicación Microsoft Authenticator

Dependiendo de la configuración de su organización, puede que se le pida que configure la aplicación Microsoft Authenticator cuando inicie sesión. En caso contrario, para comenzar la configuración de la aplicación Microsoft Authenticator en la información de seguridad, siga los pasos que encontrará en [Administración de la información de seguridad](security-info-manage-settings.md).

Para descargar la aplicación Microsoft Authenticator y obtener más información sobre ella, vea [Introducción a la aplicación Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

>[!Note]
>Si no desea usar la aplicación Microsoft Authenticator, puede elegir una aplicación diferente durante la configuración. En este artículo se usa la aplicación Microsoft Authenticator. Si no ve la opción de la aplicación autenticadora, es posible que su organización no le permita usar una aplicación autenticadora para la verificación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-use-the-microsoft-authenticator-app"></a>Para usar la aplicación Microsoft Authenticator

1. Seleccione la opción **Aplicación autenticadora**.

    Se abre el asistente para **obtener la aplicación**.

    ![Asistente para obtener la aplicación, pantalla inicial](media/security-info/security-info-auth-app-wizard.png)

    Si no desea usar la aplicación Microsoft Authenticator, haga clic en el vínculo **Quiero usar otra aplicación de autenticación** en la pantalla **Obtener la aplicación**.

2. Una vez instalada la aplicación Microsoft Authenticator, seleccione **Siguiente**.

    Si se le pide, permita las notificaciones, agregue una cuenta nueva y seleccione **Cuenta profesional o educativa**.

3. Seleccione **Next** (Siguiente).

    Aparece la pantalla **Escanear el código QR**.

    ![Examinar el código QA con la aplicación Authenticator](media/security-info/security-info-scan-qr.png)

4. Abra la aplicación Microsoft Authenticator, seleccione **Agregar cuenta** en el icono **Personalizar y controlar** en la esquina superior derecha y, a continuación, seleccione **Cuenta profesional o educativa**. 

5. Si tiene una aplicación de lector de códigos QR, escanee el código proporcionado. Si no tiene una aplicación de lector de códigos, puede seleccionar el vínculo **No puedo escanear el código QR** y escribir manualmente la dirección URL y el código en la aplicación Microsoft Authenticator.

6. Use la aplicación Microsoft Authenticator para aprobar la notificación de activación de la aplicación.

    La información de seguridad se actualiza para usar la aplicación Microsoft Authenticator a fin de verificar su identidad al usar la verificación en dos pasos o el autoservicio de restablecimiento de contraseña.

    >[!Note]
    >Si la organización lo permite, también obtendrá un código de verificación junto con la notificación de la aplicación Microsoft Authenticator. Si desea que el código sea el método predeterminado, siga las instrucciones de [Administración de la información de seguridad](security-info-setup-auth-app.md).

## <a name="additional-security-info-options"></a>Opciones de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).
    
    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).