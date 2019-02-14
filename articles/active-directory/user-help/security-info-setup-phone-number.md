---
title: 'Configuración de la información de seguridad para usar llamadas de teléfono: Azure Active Directory | Microsoft Docs'
description: Configurar la información de seguridad para comprobar su identidad mediante un dispositivo móvil o número de teléfono del trabajo.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c8daa50886c583f26a2d931854d7132eb2ece4b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177213"
---
# <a name="set-up-security-info-to-use-phone-calls-preview"></a>Configuración de la información de seguridad para usar llamadas de teléfono (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Para configurar la información de seguridad tiene que iniciar sesión en su cuenta profesional o educativa y completar el proceso de registro. Si nunca ha configurado la información de seguridad, se le pedirá que lo haga ahora.

## <a name="set-up-phone-calls"></a>Configuración de llamadas telefónicas

Dependiendo de la configuración de su organización, puede que se le pida que agregue un número de teléfono a su información de seguridad al iniciar sesión. En caso contrario, para comenzar la configuración de las llamadas de teléfono en la información de seguridad, siga los pasos que encontrará en [Administración de la información de seguridad](security-info-manage-settings.md).

>[!Note]
>Información de seguridad no admite el uso de extensiones telefónicas. Incluso si agrega el formato correcto +1 4255551234X12345, las extensiones se quitan antes de realizarse la llamada.<br>Si no ve la opción de llamadas de teléfono, es posible que su organización no le permite usar llamadas de teléfono para la comprobación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-use-your-phone-number"></a>Para usar su número de teléfono

1. Seleccione la opción **Teléfono**.

    Aparece el asistente **Set up your phone** (Configure su teléfono).

    ![Configuración del código de país o región y número de teléfono](media/security-info/security-info-keep-secure-setup-phone.png)

2. Elija su **país o región** en el cuadro de lista desplegable, escriba el número de teléfono (incluido el código de área, si procede) en el cuadro para el **número de teléfono** y seleccione la opción **Call me** (Llamarme) y, a continuación, seleccione **Siguiente**.

    Recibirá una llamada de teléfono para asegurarse de que ha escrito el número de teléfono correcto. En ese momento, se le pedirá que presione la tecla almohadilla (#) para confirmar y completar la configuración.

    ![Mire la pantalla del teléfono, y compruebe que muestra que la llamada se ha respondido correctamente](media/security-info/security-info-keep-secure-verify-phone-call.png)

    La información de seguridad se actualiza para usar su número de teléfono para comprobar su identidad al usar la verificación en dos pasos o el restablecimiento de contraseña.

    >[!Note]
    >Si desea recibir un mensaje de texto en lugar de una llamada de teléfono a tu dispositivo móvil, siga los pasos descritos en el artículo [Set up security info to use text messaging (SMS)](security-info-setup-text-msg.md) (Configuración de la información de seguridad para usar mensajería de texto).

## <a name="additional-security-info-options"></a>Opciones de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un mensaje de texto (SMS), consulte [Set up security info to use text messaging (SMS)](security-info-setup-text-msg.md) (Configuración de la información de seguridad para usar mensajería de texto).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).
    
    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
