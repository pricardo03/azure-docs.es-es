---
title: Autenticación de usuario con Azure Active Directory
description: Como administrador de Azure AD, ¿cómo puedo proteger la autenticación del usuario al tiempo que se reduce el efecto en el usuario final?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: d14b44a45d9001e945e559dcd080683220c6a0b7
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076407"
---
# <a name="what-methods-are-available-for-authentication"></a>¿Qué métodos están disponibles para la autenticación?

Escuchamos en las noticias que las contraseñas se roban y las identidades se ponen en peligro. Requerir un segundo factor además de una contraseña aumenta de forma inmediata la seguridad de una organización. Microsoft Azure Active Directory (Azure AD) incluye funciones, como Azure Multi-factor Authentication (Azure MFA) y el autoservicio de restablecimiento de contraseñas (SSPR) de Azure AD, para ayudar a los administradores a proteger las organizaciones y a los usuarios con métodos de autenticación adicionales.

Hay muchos escenarios, como inicio de sesión en una aplicación, restablecimiento de contraseña, habilitación de Windows Hello, etc. en los que se pedirá a los usuarios que proporcionen verificación adicional de que son quienes dicen ser.

La comprobación adicional puede producirse en forma de algún método de autenticación, como es:

* Con un código proporcionado en un mensaje de texto o correo electrónico
* Con una llamada de teléfono
* Con una notificación o código en su teléfono
* Respondiendo a sus preguntas de seguridad

![Página de inicio de sesión de ejemplo login.microsoftonline.com en Chrome](media/overview-authentication/overview-login.png)

El autoservicio del restablecimiento de contraseñas de Azure AD y Azure MFA proporcionan a los administradores control sobre la configuración de directivas, la supervisión y los informes empleando Azure AD y Azure Portal para proteger las organizaciones.

## <a name="self-service-password-reset"></a>Restablecimiento de la contraseña de autoservicio

El autoservicio de restablecimiento de contraseñas proporciona a los usuarios la capacidad de restablecer sus contraseñas, sin intervención de ningún administrador, en el momento y el lugar donde se precisa.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

El autoservicio de restablecimiento de contraseñas incluye:

* **Cambio de contraseña:** conozco mi contraseña, pero deseo cambiarla.
* **Restablecimiento de contraseña:** no puedo iniciar sesión y quiero restablecer la contraseña mediante uno o varios métodos de autenticación aprobados.
* **Desbloqueo de cuenta:** no puedo iniciar sesión porque mi cuenta está bloqueada y deseo desbloquearla con uno o varios métodos de autenticación aprobados.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Multi-Factor Authentication (MFA) es la solución de Microsoft de comprobación de dos pasos. El uso de métodos de autenticación aprobados por un administrador permite a Azure MFA ayudar a proteger el acceso a los datos y las aplicaciones, además de satisfacer la exigencia de un proceso de inicio de sesión simple.

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso es profundizar en el autoservicio de restablecimiento de contraseñas y en Azure Multi-factor Authentication, y configurarlos.

Para empezar a trabajar con el autoservicio de restablecimiento de contraseñas, consulte el [artículo sobre el inicio rápido para habilitar SSPR](quickstart-sspr.md).

Más información sobre el autoservicio de restablecimiento de contraseña en el artículo [Cómo funciona: Autoservicio de restablecimiento de contraseña de Azure AD](concept-sspr-howitworks.md)

Más información sobre Azure Multi-factor Authentication en el artículo [Cómo funciona: Azure Multi-Factor Authentication](concept-mfa-howitworks.md)
