---
title: 'Administración de la información de seguridad: Azure Active Directory | Microsoft Docs'
description: Aprenda a administrar la información de seguridad, incluida la forma de trabajar con las opciones de verificación en dos pasos.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: f9a77c1f3fbce295a40db366619d7485d44efc4f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162723"
---
# <a name="manage-your-security-info-preview"></a>Administración de la información de seguridad (versión preliminar)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Puede usar la información de seguridad para iniciar sesión en su cuenta profesional o educativa o para restablecer su contraseña.

Al iniciar sesión, y según la configuración de su organización, es posible que vea una casilla que dice **Don't ask again for X days** (No preguntar de nuevo en los próximos X días). Esta casilla le permite mantener la sesión iniciada en el dispositivo durante el número de días que el administrador permita, sin solicitar una nueva verificación.

## <a name="change-your-info"></a>Cambio de la información
Puede actualizar o agregar información de seguridad o cambiar su valor predeterminado, según lo que el administrador y su organización permitan.

### <a name="to-change-your-info"></a>Para cambiar la información

1. Inicie sesión en su cuenta profesional o educativa.

2. Vaya a myapps.microsoft.com, seleccione su nombre de la esquina superior derecha de la página y luego seleccione **Perfil**.

3. En el área **Administrar cuenta**, seleccione **Editar la información de seguridad**.

    ![Pantalla de perfil con el vínculo Editar información de seguridad resaltado](media/security-info/security-info-profile.png)

4. Use el método predeterminado para aprobar el acceso y ver los detalles actuales de la información de seguridad si el administrador ha configurado esta experiencia para su organización.

5. En la página **Garantizar la seguridad de la cuenta**, puede:

    - Seleccionar la opción **Agregar información de seguridad** para agregar métodos adicionales.

    - Seleccione **Cambiar valor predeterminado** para cambiar el método predeterminado.

    - Seleccione el icono de **lápiz** situado junto a un método existente para actualizar tu información.

    ![Pantalla de información de seguridad con información existente editable](media/security-info/security-info-edit.png)

6. Después de realizar los cambios, puede salir de la página y estos se guardarán.

Si no ve estas opciones o no puede acceder a la página myapps.microsoft.com, es posible que su organización use opciones personalizadas o una página personalizada. Deberá ponerse en contacto con su administrador para obtener más ayuda.

## <a name="manage-your-security-info-for-a-lost-or-potentially-compromised-device"></a>Administración de la información de seguridad para un dispositivo perdido o potencialmente en peligro

Si pierde el dispositivo o este está en peligro, tendrá que repetir el proceso de verificación en todos los dispositivos de confianza anteriores.

### <a name="to-manage-your-security-info-for-lost-or-potentially-compromised-devices"></a>Para administrar la información de seguridad para un dispositivo perdido o potencialmente en peligro

1. Inicie sesión en su cuenta profesional o educativa.

2. Vaya a myapps.microsoft.com, seleccione su nombre de la esquina superior derecha de la página y luego seleccione **Perfil**.

3. En el área **Administrar cuenta**, seleccione **No aplicar MFA en los dispositivos recordados**.
    
    Si elige esta opción, tendrá que pasar por el proceso de autenticación multifactor de nuevo después de iniciar sesión.

    ![Pantalla de perfil con el vínculo no aplicado resaltado](media/security-info/security-info-forget.png)

## <a name="common-problems-and-solutions-with-your-security-info"></a>Problemas comunes y soluciones con la información de seguridad

Este artículo le ayuda a solucionar problemas con la información de seguridad, incluidos los problemas relacionados con la verificación en dos pasos.

|Problema|Solución|
|-------|--------|
|No tengo mi teléfono conmigo|Es posible que no tenga el teléfono consigo en todo momento, pero que, a pesar de ello, desee iniciar sesión en su cuenta profesional o educativa. Para solucionar este problema, puede iniciar sesión mediante un método de autenticación diferente que no requiera el teléfono como, por ejemplo, el correo electrónico o el número de teléfono de la oficina. Para agregar métodos adicionales a la información de seguridad, puede seguir los pasos descritos en la sección [Cambio de la información](#change-your-info).|
|Perdí el teléfono o me lo robaron|Por desgracia, a veces se produce la pérdida o robo del teléfono. En este caso, es muy recomendable hacérselo saber a su organización para que el personal de TI pueda restablecer las contraseñas de las aplicaciones y borrar cualquier dispositivo recordado de la lista de dispositivos de confianza. También puede olvidar sus propios dispositivos de confianza siguiendo los pasos que se indican en la sección [Administración de la información de seguridad para un dispositivo perdido o potencialmente en peligro](#manage-your-security-info-for-a-lost-or-potentially-compromised-device).|
|Tengo un nuevo número de teléfono|Existen dos formas de solucionar este problema. Puede iniciar sesión mediante un método de autenticación alternativo que no requiera el número de teléfono como, por ejemplo, el correo electrónico, o si eso no es una opción, puede ponerse en contacto con el personal de TI de su organización y pedirles que borren su configuración. Para agregar métodos adicionales a la información de seguridad, puede seguir los pasos descritos en la sección [Cambio de la información](#change-your-info).|
|Mi método predeterminado es incorrecto|Puede actualizar el método predeterminado en las opciones de seguridad. Para obtener detalles concretos, puede ir a la sección [Cambio de la información](#change-your-info).|
|No recibo ningún mensaje o llamada en el dispositivo móvil|Si ha recibido mensajes o llamadas en su dispositivo móvil anteriormente, es probable que el problema tenga que ver con el proveedor de telefonía, no con la cuenta. Asegúrese de que tiene buena cobertura y de que puede recibir mensajes de texto y llamadas telefónicas. Puede pedirle a un amigo que le llame o que le envíe un mensaje de texto como prueba.<br><br>Si puede recibir correctamente mensajes y llamadas pero sigue sin haber recibido la notificación, pruebe a usar un método diferente. Para agregar métodos adicionales a la información de seguridad, puede seguir los pasos descritos en la sección [Cambio de la información](#change-your-info). Si no tiene otro método que agregar, puede ponerse en contacto con el equipo de soporte técnico de su empresa y pedirles que borren su configuración para poder configurar los métodos la próxima vez que inicie sesión.<br><br>Si suele tener problemas debido a una mala calidad de la señal, le recomendamos que use la aplicación Microsoft Authenticator en su dispositivo móvil. La aplicación puede generar códigos de seguridad aleatorios que usa para iniciar sesión, los cuales no requieren señal telefónica ni conexión a Internet. Para más información sobre la aplicación Microsoft Authenticator, consulte el artículo [Introducción a la aplicación Microsoft Authenticator](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).|
|Ninguna de las opciones de esta tabla solucionó el problema|Si probó estos pasos para solucionar los problemas pero estos no desaparecen, póngase en contacto con el soporte técnico de la empresa y ellos le ayudarán.|

## <a name="next-steps"></a>Pasos siguientes

- Consulte los aspectos de la información de seguridad en [Introducción a la información de seguridad (versión preliminar)](user-help-security-info-overview.md).

- Obtenga información sobre la verificación en dos pasos en el artículo [Introducción a la verificación en dos pasos](user-help-two-step-verification-overview.md). 

- Siga uno de estos artículos de procedimientos para obtener información sobre cómo configurar los dispositivos en el área de información de seguridad:

    - [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador)

    - [Set up security info to use phone calls](security-info-setup-phone-number.md) (Configuración de la información de seguridad para usar las llamadas de teléfono)

    - [Set up security info to use text messaging](security-info-setup-text-msg.md) (Configuración de la información de seguridad para usar mensajería de texto)

    - [Set up security info to use email](security-info-setup-email.md) (Configuración de la información de seguridad para usar el correo electrónico)

    - [Set up security info to use security questions](security-info-setup-questions.md) (Configuración de la información de seguridad para usar preguntas de seguridad)

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](user-help-reset-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).