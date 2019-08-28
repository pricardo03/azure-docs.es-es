---
title: 'Información general de la configuración de los métodos de verificación en dos fases: Azure Active Directory | Microsoft Docs'
description: Información general sobre cómo configurar los métodos de verificación en dos fases.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 08/12/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83bba343ac73fd0df575ae1e8a83c589a0bc15ac
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69616160"
---
# <a name="set-up-your-two-factor-verification-methods-overview"></a>Información general sobre la configuración de los métodos de verificación en dos fases

La organización ha activado la verificación en dos fases, lo cual significa que el inicio de sesión de la cuenta profesional o educativa requiere ahora una combinación del nombre de usuario, la contraseña y un dispositivo móvil o un teléfono. La organización ha activado esta verificación adicional porque es más segura que solo una contraseña, porque se basa en dos formas de autenticación: algo que usted sabe y algo que usted tiene. La verificación en dos fases ayuda a impedir que hackers malintencionados finjan ser usted, ya que, incluso si conocen su contraseña, lo más probable es que no tengan también su dispositivo.

>[!Important]
>Este contenido está diseñado para usuarios. Si es un administrador, puede encontrar más información acerca de cómo configurar y administrar su entorno de Azure Active Directory (Azure AD) en la [documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory).

## <a name="who-decides-if-you-use-this-feature"></a>¿Quién decide si usa esta característica?

En función del tipo de cuenta, la organización puede decidir que se debe usar una comprobación en dos fases o el usuario puede decidir por sí mismo si se usa o no.

- **Cuenta profesional o educativa.** Si se usa una cuenta profesional o educativa (por ejemplo, alain@contoso.com), la organización es la que decide si se debe usar una comprobación en dos fases, junto con los métodos de comprobación concretos. Dado que su organización ha decidido que debe usar esta característica, no hay ninguna manera de desactivarla individualmente.

- **Cuenta personal de Microsoft.** Se puede elegir configurar una comprobación en dos fases para las cuentas personales de Microsoft (por ejemplo, alain@outlook.com). Si tiene problemas con la verificación en dos fases y su cuenta personal de Microsoft, consulte [Activar o desactivar la verificación en dos pasos para la cuenta de Microsoft](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off). Dado que usted decide si usar esta característica, puede activarla y desactivarla siempre que lo desee.

## <a name="access-the-additional-security-verification-page"></a>Acceso a la página Comprobación de seguridad adicional

Una vez que la organización se active y configure la verificación en dos fases, recibirá un mensaje que le indicará que proporcione más información para ayudar a mantener su cuenta segura.

![Solicitud de más información necesaria](media/multi-factor-authentication-verification-methods/multi-factor-authentication-initial-prompt.png)

### <a name="to-access-the-additional-security-verification-page"></a>Para acceder a la página Comprobación de seguridad adicional

1. Seleccione **Siguiente** en el mensaje de **Más información necesaria**.

    Aparece la página **Comprobación de seguridad adicional**.

2. En la página **Comprobación de seguridad adicional**, debe decidir qué método de verificación en dos fases se va a usar para verificar que es quien pretende ser, después de iniciar sesión en su cuenta profesional o educativa. Puede seleccionar:

    | Método de contacto | DESCRIPCIÓN |
    | --- | --- |
    | Aplicación móvil | <ul><li>**Recibir notificaciones de comprobación.** Esta opción inserta una notificación en la aplicación del autenticador en su smartphone o tableta. Vea la notificación y, si es legítima, seleccione **Autenticar** en la aplicación. Puede que su trabajo o escuela requiera que escriba un PIN para autenticarse.</li><li>**Usar el código de verificación.** En este modo, la aplicación del autenticador genera un código de verificación que se actualiza cada 30 segundos. Escriba el código de verificación más reciente en la pantalla de inicio de sesión.<br>La aplicación Microsoft Authenticator está disponible para [Android](https://go.microsoft.com/fwlink/?linkid=866594) e [iOS](https://go.microsoft.com/fwlink/?linkid=866594).</li></ul> |
    | Teléfono de autenticación | <ul><li>**Llamada de teléfono** realiza una llamada de voz automatizada al número de teléfono que proporcione. El usuario responde a la llamada y pulsa la tecla de almohadilla (#) en el teclado del teléfono para autenticarse.</li><li>**Mensaje de texto** envía un mensaje de texto que contiene un código de verificación. Según la solicitud en el texto, responda al mensaje de texto o escriba el código de comprobación proporcionado en la interfaz de inicio de sesión.</li></ul> |
    | Teléfono del trabajo | Realiza una llamada de voz automatizada al número de teléfono que proporcione. El usuario responde a la llamada y pulsa la tecla de almohadilla (#) en el teclado del teléfono para autenticarse. |

## <a name="next-steps"></a>Pasos siguientes

Después de acceder a la página de **Comprobación de seguridad adicional**, debe seleccionar y configurar el método de verificación en dos fases:

- [Configuración de un dispositivo móvil como método de comprobación](multi-factor-authentication-setup-phone-number.md)

- [Configuración de un teléfono como método de comprobación](multi-factor-authentication-setup-office-phone.md)

- [Configuración de la aplicación Microsoft Authenticator como método de comprobación](multi-factor-authentication-setup-auth-app.md)

## <a name="related-resources"></a>Recursos relacionados

- [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md)

- [Administración de las contraseñas de aplicación](multi-factor-authentication-end-user-app-passwords.md)

- [Inicio de sesión con la verificación en dos fases](multi-factor-authentication-end-user-signin.md)

- [Obtención de ayuda con la verificación en dos fases](multi-factor-authentication-end-user-troubleshoot.md) 
