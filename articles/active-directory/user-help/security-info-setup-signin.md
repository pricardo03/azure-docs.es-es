---
title: 'Configuración de la información de seguridad (versión preliminar) desde su mensaje de inicio de sesión: Azure Active Directory | Microsoft Docs'
description: Cómo configurar la información de seguridad de una cuenta profesional o educativa, si se solicita desde la página de inicio de sesión de una organización.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: overview
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2bb1ec22d4113c7b9cecbc428bf70ac91d9a7e75
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56457913"
---
# <a name="set-up-your-security-info-preview-from-the-sign-in-page-prompt"></a>Configuración de la información de seguridad (versión preliminar) desde el mensaje de la página de inicio de sesión
Puede seguir estos pasos si se le pide que configure la información de seguridad inmediatamente después de que iniciar sesión con su cuenta profesional o educativa.

Solo verá este mensaje si no ha configurado la información de seguridad que requiere su organización. Si ya ha configurado la información de seguridad, pero desea realizar cualquier cambio, puede seguir los pasos descritos en los distintos artículos de procedimientos basados en métodos. Para más información, consulte [Add or update your security info overview](security-info-add-update-methods-overview.md) (Introducción a la adición o actualización de la información de seguridad).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="sign-in-to-your-work-or-school-account"></a>Inicio de sesión con una cuenta profesional o educativa
Después de iniciar sesión con su cuenta profesional o educativa, verá un mensaje en el que se le pide que proporcione más información para que se le permita acceder a su cuenta.

![Mensaje en el que se pide más información](media/security-info/securityinfo-prompt.png)

## <a name="set-up-your-security-info-using-the-wizard"></a>Configuración de la información de seguridad mediante el asistente
Siga estos pasos para configurar la información de seguridad de su cuenta profesional o educativa desde el mensaje.

>[!Important]
>Esto es solo un ejemplo del proceso. En función de los requisitos de la organización, el administrador puede haber instalado distintos métodos de comprobación que necesitará configurar durante este proceso. Para este ejemplo, se requieren dos métodos, la aplicación Microsoft Authenticator y un número de teléfono móvil para realizar una llamada o enviar un mensaje de texto de comprobación.

1. Después de seleccionar **Siguiente** en el mensaje, aparece el **Asistente para mantener la cuenta segura**, en el que se muestra el primer método que tanto el administrador como la organización requieren que configure. En este ejemplo, es la aplicación Microsoft Authenticator.

    >[!Note]
    >Si desea usar una aplicación de autenticación que no sea Microsoft Authenticator, seleccione el vínculo **Quiero usar otra aplicación de autenticación**.
    
    >Si su organización le permite elegir un método que no sea la aplicación de autenticación, puede seleccionar el vínculo **I want to set up a different method** (Quiero configurar otro método).

    ![El Asistente para mantener la cuenta segura, que muestra la página de descarga de la aplicación de autenticación](media/security-info/securityinfo-prompt-get-auth-app.png)

2. Seleccione **Descargar ahora** para descargar e instalar la aplicación Microsoft Authenticator en un dispositivo móvil y, después, seleccione **Siguiente**. Para más información acerca de cómo descargar e instalar la aplicación, consulte [Download and install the Microsoft Authenticator app](user-help-auth-app-download-install.md) (Descargar e instalar la aplicación Microsoft Authenticator).

    ![El Asistente para mantener la cuenta segura, que muestra la página de configuración de una cuenta de la aplicación de autenticación](media/security-info/securityinfo-prompt-auth-app-setup-acct.png)

3. Permanezca en la página **Configuración de la cuenta** mientras configura la aplicación Microsoft Authenticator en el dispositivo móvil.

4. Abra la aplicación Microsoft Authenticator, seleccione que permita las notificaciones (si se le solicita), **Agregar cuenta** en el icono **Customize and control** (Personalizar y controlar) de la esquina superior derecha y, después, seleccione **Cuenta profesional o educativa**.

5. Vuelva a la página **Configuración de la cuenta** en el equipo y seleccione **Siguiente**.

    Aparece la página **Digitalización del código QR** .

    ![Examinar el código QA con la aplicación Authenticator](media/security-info/securityinfo-prompt-auth-app-qrcode.png)

6. Lea el código que se proporciona, para lo que debe usar el lector de códigos QR de la aplicación Microsoft Authenticator, que apareció en el dispositivo móvil tras crear la cuenta profesional o educativa en el paso 5.

    La aplicación de autenticación debería agregar correctamente la cuenta profesional o educativa sin solicitarle más información. No obstante, si el lector de códigos QR no lee este, puede seleccionar el vínculo **Can't scan the QR code** (No se puede leer el código QR) y especificar manualmente la dirección URL y el código en la aplicación Microsoft Authenticator. Para más información acerca de cómo agregar manualmente un código, consulte [Agregar manualmente una cuenta a la aplicación](user-help-auth-app-add-account-manual.md).

7. Seleccione **Siguiente** en la página **Digitalización del código QR** del equipo.

    Se envía una notificación a la aplicación Microsoft Authenticator del dispositivo móvil para que pruebe la cuenta.

    ![Probar la cuenta con la aplicación de autenticación](media/security-info/securityinfo-prompt-test-app.png)

8. Apruebe la notificación en la aplicación Microsoft Authenticator y seleccione **Siguiente**.

    ![Notificación correcta, se establece la conexión entre la aplicación y su cuenta](media/security-info/securityinfo-prompt-auth-app-success.png).

    La información de seguridad se actualiza para usar la aplicación Microsoft Authenticator de forma predeterminada para comprobar su identidad cuando se usan la verificación en dos pasos o el restablecimiento de contraseña.

9. En la página de configuración del **teléfono**, elija si desea recibir un mensaje de texto o una llamada de teléfono y seleccione **Siguiente**. En este ejemplo vamos a usar mensajes de texto, por lo que debe usar el número de teléfono de un dispositivo que pueda aceptar mensajes de texto.

    ![Comienzo de la configuración del número de teléfono para los mensajes de texto](media/security-info/securityinfo-prompt-text-msg.png)

    Se envía un mensaje de texto al número de teléfono. Si prefiere recibir una llamada telefónica, el proceso es el mismo. Sin embargo, recibirá una llamada telefónica con instrucciones, en lugar de un mensaje de texto.

10. Especifique el código que proporciona el mensaje de texto que se ha enviado al dispositivo móvil y, después, seleccione **Siguiente**.

    ![Probar la cuenta con el mensaje de texto](media/security-info/securityinfo-prompt-text-msg-enter-code.png)

11. Revise que la notificación sea correcta y seleccione **Listo**.

    ![Notificación correcta](media/security-info/securityinfo-prompt-call-answered-success.png)

    La información de seguridad se actualiza para usar mensajes de texto como método de seguridad para comprobar su identidad al usar la verificación en dos pasos o el restablecimiento de contraseña.

12. Revise la página **Success** (Correcto) para comprobar que ha configurado correctamente tanto la aplicación Microsoft Authenticator como el método del teléfono (llamada de teléfono o mensaje de texto) para la información de seguridad y, después, seleccione **Listo**.

    ![Página que indica que el asistente ha completado la operación correctamente](media/security-info/securityinfo-prompt-setup-success.png)

## <a name="next-steps"></a>Pasos siguientes

- Para cambiar, eliminar o actualizar los métodos predeterminados de la información de seguridad, consulte:

    - [Configuración de la información de seguridad de una aplicación de autenticación](security-info-setup-auth-app.md).

    - [Configuración de la información de seguridad para usar la mensajería de texto](security-info-setup-text-msg.md).

    - [Configuración de la información de seguridad para usar las llamadas de teléfono](security-info-setup-phone-number.md).

    - [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

    - [Configuración de la información de seguridad para usar las preguntas de seguridad predefinidas](security-info-setup-questions.md).

- Para obtener información acerca de cómo iniciar sesión mediante el método especificado, consulte [Inicio de sesión con la información de seguridad o la verificación en dos pasos](user-help-sign-in.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).