---
title: Introducción a la aplicación Microsoft Authenticator en Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de la aplicación Microsoft Authenticator, lo que incluye qué es, cómo funciona y qué información se incluye en esta sección del contenido.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/24/2019
ms.author: lizross
ms.openlocfilehash: 2379f1ff4fb4385015cc6077cb923cab998d1d11
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175220"
---
# <a name="what-is-the-microsoft-authenticator-app"></a>¿Qué es la aplicación Microsoft Authenticator?
La aplicación Microsoft Authenticator le ayuda a iniciar sesión en sus cuentas si usa la comprobación en dos fases. La comprobación en dos fases le ayuda a acceder a sus cuentas de manera más segura, en especial cuando se examina información confidencial. Como las contraseñas se pueden olvidar, poner en peligro o robar, la comprobación en dos fases es un paso de seguridad adicional que ayuda a proteger su cuenta ya que es más difícil que otras personas accedan a ella.

Puede usar la aplicación Microsoft Authenticator de varias formas, por ejemplo:

- Para proporcionar un aviso de un segundo método de comprobación después de iniciar sesión con su nombre de usuario y contraseña.

- Para proporcionar inicio de sesión sin necesidad de una contraseña, mediante su nombre de usuario y su dispositivo móvil con su huella digital, cara o PIN.

>[!Important]
>Este contenido está diseñado para usuarios. Si es un administrador, puede encontrar más información acerca de cómo configurar y administrar su entorno de Azure Active Directory (Azure AD) en la [documentación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory).<br><br>Si tiene problemas para iniciar sesión en su cuenta, puede encontrar ayuda en [Cuando no consigues iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429).  Obtenga más información acerca de lo que debe hacer cuando recibe el mensaje ["Esa cuenta de Microsoft no existe"](https://support.microsoft.com/help/13811) al intentar iniciar sesión en su cuenta Microsoft.

## <a name="terminology"></a>Terminología
|Término|DESCRIPCIÓN|
|----|-----------|
|Comprobación en dos fases |Un proceso de comprobación que requiere que se usen específicamente únicamente dos factores de comprobación, como por ejemplo una contraseña y un PIN. La aplicación Microsoft Authenticator admite tanto la comprobación en dos fases estándar como el inicio de sesión sin contraseña.|
|Multi-Factor Authentication (MFA)|Toda comprobación en dos fases es una autenticación multifactor, que requiere que use *al menos* dos factores de comprobación, según los requisitos de su organización.|
|Cuenta Microsoft (también denominada MSA)|Cree sus propias cuentas personales para poder acceder a los productos y servicios en la nube de Microsoft orientados al consumidor, como Outlook, OneDrive, Xbox LIVE u Office 365. Su cuenta Microsoft se crean y almacenan en el sistema de cuentas de identidad de consumidor de Microsoft que ejecuta Microsoft.|
|Cuenta profesional o educativa|Su organización crea una cuenta profesional o educativa (como alain@contoso.com) para permitirle acceder a recursos internos y potencialmente restringidos, como Microsoft Azure, Windows Intune y Office 365.|

## <a name="how-two-factor-verification-works-with-the-app"></a>Funcionamiento de la comprobación en dos fases con la aplicación
Así funciona la comprobación mediante dos factores con la aplicación Microsoft Authenticator:

- **Notificación.** Escriba el nombre de usuario y la contraseña en el dispositivo en el que va a iniciar sesión con su cuenta profesional o educativa o con su cuenta personal de Microsoft y, después, la aplicación Microsoft Authenticator envía una notificación que le pide que **apruebe el inicio de sesión**. Elija **Aprobar** si reconoce el intento de inicio de sesión. De lo contrario, elija **Rechazar**. Si elige **Rechazar**, también puede marcar la solicitud como fraudulenta.

- **Código de verificación.** Escriba el nombre de usuario y la contraseña en el dispositivo en que va a iniciar sesión con su cuenta profesional o educativa, o con su cuenta personal de Microsoft y, después, copie el código de verificación asociado en la pantalla **Cuentas** de la aplicación Microsoft Authenticator.

- **Inicio de sesión sin contraseña.** Escriba el nombre de usuario en el dispositivo en que va a iniciar una sesión con su cuenta profesional o educativa o su cuenta personal de Microsoft y, después, use su dispositivo móvil para comprobar que es usted mediante el uso de la huella digital, la cara o un PIN. Para este método, no es preciso que escriba su contraseña.

### <a name="whether-to-use-your-devices-biometric-capabilities"></a>Si usa las funcionalidades biométricas del dispositivo
Si usa un PIN para completar el proceso de autenticación, puede configurar la aplicación Microsoft Authenticator para usar en su lugar las funcionalidades de reconocimiento facial (biométrica) o de la huella del dispositivo. Puede realizar esta configuración la primera vez que use la aplicación Authenticator para verificar su cuenta; para ello, seleccione la opción para usar las funcionalidades biométricas del dispositivo como identificación en lugar del PIN.

## <a name="who-decides-if-you-use-this-feature"></a>¿Quién decide si usa esta característica?
En función del tipo de cuenta, la organización puede decidir que se debe usar una comprobación en dos fases o el usuario puede decidir por sí mismo si se usa o no.

- **Cuenta profesional o educativa.** Si se usa una cuenta profesional o educativa (por ejemplo, alain@contoso.com), la organización es la que decide si se debe usar una comprobación en dos fases, junto con los métodos de comprobación concretos. Para más información acerca de la autenticación multifactor, consulte [¿Qué relevancia tiene Azure Multi-Factor Authentication para mí?](multi-factor-authentication-end-user.md). Para más información acerca de cómo configurar la información de seguridad para usar la aplicación Microsoft Authenticator, consulte [Configuración de la información de seguridad para usar una aplicación autenticadora (versión preliminar)](security-info-setup-auth-app.md).

- **Cuenta personal de Microsoft.** Se puede elegir configurar una comprobación en dos fases para las cuentas personales de Microsoft (por ejemplo, alain@outlook.com).

- **Cuenta personal que no sea de Microsoft.** Puede elegir configurar una comprobación en dos fases para sus cuentas personales que no sean de Microsoft (por ejemplo, alain@gmail.com). Es posible que las cuentas que no sean de Microsoft no usen el término comprobación en dos fases, pero la característica debería encontrarse dentro de la configuración de **Seguridad** o **Inicio de sesión**.

## <a name="in-this-section"></a>En esta sección

|Artículo |Descripción |
|------|------------|
|[Descarga e instalación de la aplicación](microsoft-authenticator-app-how-to.md)|Describe dónde y cómo obtener e instalar la aplicación Microsoft Authenticator para dispositivos con Android, iOS y Windows Phone.|
|[Adición de cuentas profesionales o educativas](microsoft-authenticator-app-add-work-account.md)|Se describe cómo agregar varias cuentas profesionales o educativas, y personales a la aplicación Microsoft Authenticator.|
|[Adición de cuentas personales](microsoft-authenticator-app-add-personal-account.md)|Se describe cómo agregar cuentas personales tanto de Microsoft como no de Microsoft a la aplicación Microsoft Authenticator.|
|[Adición manual de cuentas](microsoft-authenticator-app-add-account-manual.md)|Se describe cómo agregar manualmente cuentas a la aplicación Microsoft Authenticator si no se puede leer el código QR.|
|[Inicio de sesión mediante la aplicación](microsoft-authenticator-app-phone-signin-faq.md)|Describe cómo iniciar sesión en varias cuentas mediante la aplicación Microsoft Authenticator.|
|[Credenciales de la cuenta de copia de seguridad y recuperación](microsoft-authenticator-app-backup-and-recovery.md)| Proporciona información acerca de cómo realizar copias de seguridad y recuperar las credenciales de su cuenta con la aplicación Microsoft Authenticator.|
|[Preguntas frecuentes de la aplicación Microsoft Authenticator](microsoft-authenticator-app-faq.md)|Proporciona respuestas a las preguntas más frecuentes acerca de la aplicación.|
