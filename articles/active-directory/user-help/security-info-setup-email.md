---
title: 'Configuración de la información de seguridad para usar correo electrónico: Azure Active Directory | Microsoft Docs'
description: Configure la información de seguridad para comprobar su identidad mediante una dirección de correo electrónico profesional o educativa.
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
ms.openlocfilehash: 3f98b4e34c4c76efc33395530ef016b26e1ad523
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163046"
---
# <a name="set-up-security-info-to-use-email-preview"></a>Configuración de la información de seguridad para usar el correo electrónico (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Para configurar la información de seguridad tiene que iniciar sesión en su cuenta profesional o educativa y completar el proceso de registro. Si nunca ha configurado la información de seguridad, se le pedirá que lo haga ahora.

## <a name="set-up-email"></a>Configuración del correo electrónico

Dependiendo de la configuración de su organización, puede que se le pida que agregue una dirección de correo electrónico a su información de seguridad al iniciar sesión. En caso contrario, para comenzar la configuración del correo electrónico en la información de seguridad, siga los pasos que encontrará en [Administración de la información de seguridad](security-info-manage-settings.md).

>[!Note]
>Es recomendable usar una cuenta de correo electrónico que no requiera la contraseña de red para acceder.<br>Si no ve la opción de correo electrónico, es posible que su organización no le permita usar un correo electrónico para la comprobación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-use-your-email-address"></a>Para usar su dirección de correo electrónico

1. Seleccione la opción **Correo electrónico** y, después, escriba la dirección de correo electrónico en el cuadro. Esta dirección de correo electrónico no puede ser la dirección de correo electrónico profesional o educativa.

     ![Página Información de seguridad, con el cuadro de entrada de correo electrónico](media/security-info/security-info-keep-secure-setup-email.png)

2. Busque un correo electrónico de Microsoft para su organización, escriba el código de verificación incluido en el cuadro **Verifica tu correo electrónico** y, a continuación, seleccione **Listo**.

     ![Página Información de seguridad, con el cuadro de entrada del código de verificación de correo electrónico](media/security-info/security-info-verify-email.png)

    >[!Note]
    >Si no ve un correo electrónico de Microsoft en nombre de su organización, asegúrese de que ha escrito correctamente su dirección de correo electrónico y, a continuación, compruebe las carpetas de correo no deseado.

3. En la página **Garantizar la seguridad de la cuenta**, seleccione **Listo**.

    La información de seguridad se actualiza para usar la dirección de correo electrónico para comprobar su identidad al usar la verificación en dos pasos.

## <a name="additional-security-info-options"></a>Opciones de información de seguridad adicionales

Tiene opciones para determinar cómo su organización se pone en contacto con usted para verificar su identidad, basándose en lo que está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).
    
    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).