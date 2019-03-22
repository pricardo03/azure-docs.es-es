---
title: 'En el registro combinado para SSPR de Azure AD y MFA (versión preliminar): Azure Active Directory'
description: Autenticación multifactor de Azure AD y la contraseña de autoservicio de restablecimiento de registro (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2865c19e747ca1c5b0a6cda84b8be18bfaeb9335
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317670"
---
# <a name="combined-security-information-registration-preview"></a>Información del registro de seguridad combinados (versión preliminar)

Antes del registro combinado, los usuarios registrados los métodos de autenticación para Azure Multi-factor Authentication (MFA) y el restablecimiento de contraseña de autoservicio (SSPR) a través de dos experiencias diferentes. Las personas comprendían que se usaron métodos similares para Azure MFA y SSPR, pero debía registrar por separado para cada característica. Ahora, con el registro combinado, los usuarios pueden registrar una vez y obtener las ventajas de Azure MFA y SSPR.

![Combinar información de seguridad - Mi perfil que muestra información de seguridad registrado para un usuario, incluidos Microsoft Authenticator y teléfono para un usuario de ejemplo en el directorio.](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Antes de habilitar la nueva experiencia, revise esta documentación centrada en el administrador y la documentación centrada en usuario para asegurarse de que entiende la funcionalidad y el impacto de esta característica. Basar su formación en la documentación del usuario para preparar los usuarios para la nueva experiencia y ayudan a garantizar un lanzamiento satisfactorio.

|     |
| --- |
| Información del registro de seguridad combinada para el restablecimiento de contraseña de autoservicio de Azure Multi-factor Authentication y Azure AD es una característica de versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

> [!IMPORTANT]
> Si un usuario está habilitado para la versión preliminar original y la experiencia de registro combinado mejorada, verá la nueva experiencia. Los usuarios que están habilitados para ambas experiencias solo verá la nueva experiencia de mi perfil. El nuevo perfil de mi se alinea con la apariencia y comportamiento del registro combinado y ofrece una experiencia perfecta para los usuarios. Los usuarios pueden ver mi perfil yendo a [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

Las páginas de MyProfile están localizadas en función de la configuración de idioma actual en la máquina acceda a la página. Microsoft almacena el más reciente lenguaje utilizado en la caché del explorador, por lo que intenta obtener acceso a continuará representar en el último idioma utilizado. Borrar la memoria caché hará que las páginas que se va a volver a representar. Si desea forzar la adición de un idioma específico un `?lng=de-DE` al final de la dirección URL donde `de-DE` está establecido para el idioma adecuado código obligará a las páginas que se va a representar en ese idioma.

![Mi interfaz de perfil que muestra información de seguridad y capacidad de usuario configurar SSPR u otros métodos de comprobación de seguridad adicional.](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-converged-registration"></a>Métodos disponibles en el registro convergente

En este momento, el registro combinado admite los siguientes métodos y las acciones para esos métodos:

|   | Register  | Change | Eliminar |
| --- | --- | --- | --- |
| Microsoft Authenticator | Sí (5 máx.) | Sin  | Sí |
| Otra aplicación de autenticador | Sí (5 máx.) | Sin  | Sí |
| Token de hardware | Sin  | No | Sí |
| Teléfono | Sí | Sí | Sí |
| Teléfono alternativo | Sí | Sí | Sí |
| Teléfono del trabajo | Sin  | No | Sin  |
| Email | Sí | Sí | Sí |
| Preguntas de seguridad | Sí | Sin  | Sí |
| Contraseñas de aplicación | Sí | Sin  | Sí |

> [!NOTE]
> Las contraseñas de aplicación solo están disponibles para los usuarios que se han impuesto para MFA. Las contraseñas de aplicación no están disponibles para los usuarios que están habilitados para MFA a través de una directiva de acceso condicional.

Los usuarios pueden establecer las opciones siguientes como método predeterminado para MFA:

- Microsoft Authenticator: notificación
- Hardware o la aplicación de autenticador de tokens: código
- llamada de teléfono
- mensaje de texto

Puesto que seguimos agregar más métodos de autenticación tales a Azure AD, esos métodos estará disponibles en el registro combinado.

## <a name="combined-registration-modes"></a>Modos de registro combinado

Existen dos "modos" del registro combinado: interrumpir y administrar.

Modo de interrupción, es una experiencia de asistente, que se muestra a un usuario al registrar o actualizar su información de seguridad en el inicio de sesión.

Administrar el modo es parte del perfil de usuario y les permite administrar su información de seguridad.

En ambos modos, si un usuario se ha registrado previamente un método que puede utilizarse para MFA, deberá realizar MFA para poder tener acceso a su información de seguridad.

### <a name="interrupt-mode"></a>Modo de interrupción

Registro combinado respeta las directivas SSPR y MFA, si ambos están habilitados para el inquilino. Control de estas directivas, si se interrumpe un usuario para registrar durante el inicio de sesión y qué métodos hay disponibles para registrar.

La lista siguiente incluye varios escenarios donde un usuario puede que deba registrar o actualizar su información de seguridad:

* Registro MFA que se aplica a través de Identity Protection: Se pedirá a los usuarios a registrarse durante el inicio de sesión. Registrar los métodos de MFA y los métodos SSPR (si el usuario está habilitado para SSPR).
* Registro MFA que se aplica a través de MFA por usuario: Se pedirá a los usuarios a registrarse durante el inicio de sesión. Registrar los métodos de MFA y los métodos SSPR (si el usuario está habilitado para SSPR).
* Registro MFA que se aplica a través de acceso condicional y otras directivas: Se piden a los usuarios que se registren al tener acceso a un recurso que requiere MFA. Los usuarios registrarán los métodos de MFA y los métodos SSPR (si el usuario está habilitado para SSPR).
* Aplicadas el registro de SSPR: Los usuarios deben registrar durante el inicio de sesión. Se registran sólo los métodos SSPR
* Fuerza la actualización SSPR: Los usuarios deben revisar su información de seguridad en un intervalo establecido por el administrador. Los usuarios se muestran su información y pueden elegir "Tiene buen aspecto" o realizar cambios si es necesario.

Cuando se aplica el registro, los usuarios ven el número mínimo de los métodos necesarios para cumplir las directivas SSPR y MFA de más a menos seguro.

Ejemplo:

* Un usuario está habilitado para SSPR. La directiva SSPR requiere dos métodos para restablecer y ha habilitado el teléfono, correo electrónico y el código de aplicación móvil.
   * Este usuario debe registrar dos métodos.
      * Authenticator y teléfono se muestran de forma predeterminada.
      * El usuario puede elegir registrar de correo electrónico en lugar de la aplicación authenticator o el teléfono.

El siguiente diagrama describe los métodos que se muestran para un usuario cuando se interrumpe para registrar durante el inicio de sesión:

![Combinar el diagrama de flujo de información de seguridad que explica el número de métodos requeridos cuando se necesita más información al iniciar sesión. Esto puede cambiar si solo se requiere MFA o SSPR solo](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Si tiene habilitado el SSPR y MFA, se recomienda que aplique el registro MFA.

Si la directiva SSPR requiere que los usuarios que revisen su información de seguridad a intervalos regulares, los usuarios se interrumpió durante el inicio de sesión y se muestra a todos sus métodos registrados. Puede elegir "Tiene buen aspecto" si la información está actualizada o pueden elegir "Editar información" para realizar cambios.

### <a name="manage-mode"></a>Modo de administrar

Pueden tener acceso los usuarios administrar el modo de ello, vaya a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) o elija "Información de seguridad" en mi perfil. Desde allí, los usuarios pueden agregar métodos, eliminar o cambiar los métodos existentes, cambiar su método de forma predeterminada y mucho más.

## <a name="key-usage-scenarios"></a>Escenarios de uso clave

### <a name="set-up-security-info-during-sign-in"></a>Configurar la información de seguridad durante el inicio de sesión

Un administrador ha registro exigido.

Un usuario no ha configurado toda la información de seguridad requeridos y navega al portal de Azure. Después de escribir su nombre de usuario y contraseña, se solicita al usuario para configurar la información de seguridad. El usuario, a continuación, sigue los pasos que se muestra en el Asistente para configurar la información de seguridad necesarios. El usuario puede configurar los métodos que no sea lo que se muestra de forma predeterminada, si la configuración permite. Al final del asistente, el usuario revisa los métodos que se configuraron y su método predeterminado para MFA. Para completar el proceso de instalación, el usuario confirma la información y continúa en el portal de Azure.

### <a name="set-up-security-info-from-my-profile"></a>Configurar la información de seguridad desde mi perfil

Un administrador no aplica el registro.

Un usuario que no ha configurado aún en toda la información de seguridad necesarios se desplaza a [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). El usuario, a continuación, elige **información de seguridad** desde el panel de navegación izquierdo. Desde allí, el usuario decide agregar un método, selecciona cualquiera de los métodos disponibles para ellos y sigue los pasos para configurar ese método. Cuando termine, el usuario ve el método que acaba de configurar en la página de información de seguridad.

### <a name="delete-security-info-from-my-profile"></a>Eliminar información de seguridad de mi perfil

Un usuario que previamente haya configurado al menos un método navega a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). El usuario decide eliminar uno de los métodos previamente registrados. Cuando termine, el usuario ya no ve ese método en la página de información de seguridad.

### <a name="change-default-method-from-my-profile"></a>Cambiar el método predeterminado de mi perfil

Un usuario que previamente haya configurado al menos un método que puede usarse para MFA navega a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). El usuario cambia su método predeterminado actual a un método distinto. Cuando termine, el usuario ve su nuevo método de forma predeterminada en la página de información de seguridad.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar el registro combinado en el inquilino](howto-registration-mfa-sspr-combined.md)

[Métodos disponibles para MFA y SSPR](concept-authentication-methods.md)

[Configuración del restablecimiento de contraseña de autoservicio](howto-sspr-deployment.md)

[Configurar Azure Multi-factor Authentication](howto-mfa-getstarted.md)
