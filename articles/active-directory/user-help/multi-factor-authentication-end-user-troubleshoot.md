---
title: 'Solución de problemas de la verificación en dos pasos: Azure Active Directory | Microsoft Docs'
description: Proporciona instrucciones para los usuarios sobre qué hacer si encuentran un problema con Azure Multi-Factor Authentication y la verificación en dos pasos.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.component: user-help
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: lizross
ms.reviewer: kexia
ms.openlocfilehash: fd722da236f3e1fff18e2561756635cd95f098e0
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346530"
---
# <a name="get-help-with-two-step-verification"></a>Obtener ayuda con la verificación en dos pasos

La verificación en dos pasos es una característica de seguridad que la organización usa para proteger las cuentas. La verificación en dos pasos es más segura que solo una contraseña, porque se basa en dos formas de autenticación: algo que usted sabe y algo que usted tiene. Lo que sabe es la contraseña, mientras que lo que tiene es un teléfono o un dispositivo. Usar la verificación en dos pasos puede ayudar a detener a los hackers malintencionados a iniciar sesión como usuario, incluso si obtienen la contraseña.

Si bien Microsoft ofrece la verificación en dos pasos, es su organización la que decide si usará la característica. No puede optar por no usarla si su organización la quiere, de la misma manera que no puede dejar de usar una contraseña para proteger su cuenta.

>[!Note]
>Si busca más información sobre cómo usar la verificación en dos pasos con su cuenta Microsoft personal, consulte el artículo [Acerca de la verificación en dos pasos](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="why-do-i-need-to-use-another-verification-method"></a>¿Por qué es necesario usar otro método de comprobación?

Existen varios motivos por los cuales debería usar un método de comprobación alternativo con su cuenta. Por ejemplo: 

- **Olvidó traer el teléfono o el dispositivo.** Algunos días deja el teléfono en casa, pero todavía necesita iniciar sesión en el trabajo. En primer lugar, debe intentar iniciar sesión con un método en el que no sea necesario el teléfono.

- **Perdió el teléfono o tiene un número de teléfono nuevo.** Si perdió el teléfono o tiene un número de teléfono nuevo, puede iniciar sesión con un método distinto o pedirle al administrador que borre su configuración. Le recomendamos que informe al administrador que el teléfono se perdió o que se lo robaron, para que puedan hacer las actualizaciones correspondientes en su cuenta. Una vez que borran su configuración, se le pedirá [registrarse en la verificación en dos pasos](multi-factor-authentication-end-user-first-time.md) la próxima vez que inicie sesión.

- **No recibe la llamada telefónica ni el texto de autenticación.** Son varias las razones que podrían explicar por qué no recibe el texto ni la llamada telefónica. Si ha recibido mensajes o llamadas en su teléfono anteriormente, es probable que se trate de un problema con el proveedor del teléfono y no de un problema con la cuenta. Si suele tener retrasos debido a una mala calidad de la señal, recomendamos que use la [aplicación Microsoft Authenticator](microsoft-authenticator-app-how-to.md) en su dispositivo móvil. Esta aplicación puede generar códigos de seguridad aleatorios para iniciar sesión, sin necesidad de una conexión a Internet ni de una señal de celular.<br><br>Si intenta recibir un mensaje de texto, pídale a un amigo que le envíe un mensaje de texto de prueba para asegurarse de que puede recibirlo y, si recibió varios mensajes, asegúrese de que usa el código de mensaje más reciente.

- **Las contraseñas de la aplicación no funcionan.** Las contraseñas de aplicación reemplaza la contraseña normal para aplicaciones de escritorio más antiguas que no admiten la verificación en dos pasos. En primer lugar, asegúrese de escribir correctamente la contraseña. Si el problema no se soluciona, intente iniciar sesión para [crear una contraseña de aplicación](multi-factor-authentication-end-user-app-passwords.md) o ponerse en contacto con el administrador para [eliminar las contraseñas de aplicación existentes](../authentication/howto-mfa-userdevicesettings.md) para que pueda crear una nueva.

## <a name="sign-in-using-another-verification-method"></a>Inicio de sesión con otro método de comprobación

1. Inicie sesión en la cuenta de manera habitual y elija el vínculo **Sign in another way** (Iniciar sesión de otro modo) en la página **Two-step verification** (Verificación en dos pasos).

    ![Cambio del método de comprobación de inicio de sesión](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Si no ve el vínculo **Sign in another way** (Iniciar sesión de otro modo) significa que no ha configurado ningún otro método de comprobación. Tendrá que ponerse en contacto con el administrador para que lo ayude a iniciar sesión en la cuenta. Después de iniciar sesión, asegúrese de agregar métodos de comprobación adicionales. Para más información sobre cómo agregar métodos de comprobación, consulte el artículo [Administración de la configuración de la verificación en dos pasos](multi-factor-authentication-end-user-manage-settings.md).<br><br>Si ve el vínculo pero todavía no ve ningún otro método de comprobación, tendrá que ponerse en contacto con el administrador para ayudarlo a iniciar sesión en su cuenta.

2. Elija el método de comprobación alternativo y siga con el proceso de verificación en dos pasos.

3. Una vez de vuelta en su cuenta, puede actualizar los métodos de comprobación (si es necesario). Para más información sobre cómo agregar o cambiar los métodos, consulte el artículo [Administración de la configuración de la verificación en dos pasos en dos pasos](multi-factor-authentication-end-user-manage-settings.md).

## <a name="i-didnt-find-an-answer-to-my-problem"></a>No encuentro una respuesta a mi problema.

Si ya intentó estos pasos pero sigue encontrando problemas, póngase en contacto con el administrador para más ayuda.

## <a name="related-topics"></a>Temas relacionados

* [Administración de la configuración de la comprobación en dos pasos](multi-factor-authentication-end-user-manage-settings.md)

* [Preguntas más frecuentes de la aplicación Microsoft Authenticator](microsoft-authenticator-app-faq.md)
