---
title: 'Configuración de la información de seguridad para usar las preguntas de seguridad: Azure Active Directory | Microsoft Docs'
description: Configurar su información de seguridad para comprobar su identidad con preguntas de seguridad predefinidas.
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
ms.openlocfilehash: f003d7ad87cbcaca6ca22aab169a92403e1182e8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149516"
---
# <a name="set-up-security-info-to-use-pre-defined-security-questions-preview"></a>Configuración de la información de seguridad para usar las preguntas de seguridad predefinidas (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Para configurar la información de seguridad tiene que iniciar sesión en su cuenta profesional o educativa y completar el proceso de registro. Si nunca ha configurado la información de seguridad, se le pedirá que lo haga ahora.

## <a name="set-up-security-questions"></a>Configuración de preguntas de seguridad

Dependiendo de la configuración de su organización, puede que se le pida que agregue preguntas de seguridad a su información de seguridad al iniciar sesión. En caso contrario, para comenzar la configuración de las preguntas de seguridad en la información de seguridad, siga los pasos que encontrará en [Administración de la información de seguridad](security-info-manage-settings.md).

Si usa preguntas de seguridad, le recomendamos que las use junto con otro método. Las preguntas de seguridad pueden ser menos seguras que otros métodos, porque es posible que algunas personas sepan las respuestas a las preguntas de otro usuario.

>[!Note]
>Las preguntas de seguridad se almacenan de manera privada y segura en un objeto de usuario del directorio y solo las puede responder el usuario durante el registro. Un administrador no puede leer o modificar de ninguna forma sus preguntas o respuestas.<br>Si no ve la opción de preguntas de seguridad, es posible que su organización no le permite usar preguntas de seguridad para la comprobación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-choose-and-answer-your-security-questions"></a>Para elegir y responder a las preguntas de seguridad

1. Seleccione **Preguntas de seguridad** y, a continuación, elija qué preguntas de seguridad quiere responder. 

    El número de preguntas de seguridad que tendrá que elegir lo determina el administrador.

    ![Página de información de seguridad, selección de las preguntas de seguridad](media/security-info/security-info-keep-secure-setup-pick-questions.png)

2. Proporcione las respuestas a las preguntas que ha seleccionado y cuando termine seleccione **Listo**.

## <a name="additional-security-info-options"></a>Opciones de información de seguridad adicionales

Tiene opciones para determinar cómo su organización se pone en contacto con usted para verificar su identidad, basándose en lo que está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).
   
    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Si necesita actualizar la información de seguridad, siga las instrucciones del artículo [Administración de la información de seguridad](security-info-manage-settings.md).

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
