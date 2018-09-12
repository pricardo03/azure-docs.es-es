---
title: 'Configuración de la información de seguridad para usar la mensajería de texto: Azure Active Directory | Microsoft Docs'
description: Configure la información de seguridad para verificar su identidad mediante un mensaje de texto (SMS).
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: e5edfc92fc23c3235e4d4b9ca23f83c7adc3accf
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158428"
---
# <a name="set-up-security-info-to-use-text-messaging-preview"></a>Configuración de la información de seguridad para usar la mensajería de texto (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Para configurar la información de seguridad tiene que iniciar sesión en su cuenta profesional o educativa y completar el proceso de registro. Si nunca ha configurado la información de seguridad, se le pedirá que lo haga ahora.

## <a name="set-up-text-messaging"></a>Configuración de mensajería de texto

Dependiendo de la configuración de su organización, puede que se le pida que agregue la mensajería de texto a su información de seguridad al iniciar sesión. En caso contrario, para comenzar la configuración de la mensajería de texto en la información de seguridad, siga los pasos que encontrará en [Administración de la información de seguridad](security-info-manage-settings.md).

La opción de los mensajes de texto es una parte de la opción telefónica, por lo que deberá realizar exactamente la misma configuración que para usar un número de teléfono, salvo que, en lugar de que Microsoft le llame, elegirá la opción de usar un mensaje de texto. Si no ve la opción telefónica, es posible que su organización no le permite usar un número de teléfono para la verificación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-use-a-text-message"></a>Para usar un mensaje de texto

1. Seleccione la opción **Teléfono**.

    Aparece el asistente **Set up your phone** (Configure su teléfono).

    ![Configuración del código de país o región y número de teléfono](media/security-info/security-info-keep-secure-setup-text.png)

2. Elija su **país o región** en el cuadro de lista desplegable, escriba el número de teléfono (incluido el código de área, si procede) en el cuadro para el **número de teléfono** y seleccione la opción **Text me a code** (Enviarme un mensaje de texto con un código) y, a continuación, seleccione **Siguiente**.

    Recibirá un mensaje de texto con un código que deberá escribir en una página de verificación.

    ![Página de verificación para escribir el código del mensaje de texto](media/security-info/security-info-keep-secure-verify-text-msg.png)

    La información de seguridad se actualiza para enviarle un mensaje de texto a fin de verificar su identidad al usar la verificación en dos pasos o el autoservicio de restablecimiento de contraseña.

    >[!Note]
    >Si desea recibir una llamada de teléfono en lugar de un mensaje de texto, siga los pasos descritos en el artículo [Configuración de la información de seguridad para usar las llamadas de teléfono](security-info-setup-phone-number.md).

## <a name="additional-security-info-options"></a>Opciones de información de seguridad adicionales

Tiene opciones para determinar cómo su organización se pone en contacto con usted para verificar su identidad, basándose en lo que está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo configurar y usar la aplicación Microsoft Authenticator, vea [Configuración de la información de seguridad para usar una aplicación autenticadora](security-info-setup-auth-app.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).
   
    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).