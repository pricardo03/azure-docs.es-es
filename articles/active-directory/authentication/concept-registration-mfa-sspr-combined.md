---
title: 'Combinar el registro de SSPR de Azure AD y Multi-factor Authentication (versión preliminar): Azure Active Directory'
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
ms.openlocfilehash: 4f3eec1f846f1b74ab3e19bca022d4e009540d1a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280016"
---
# <a name="combined-security-information-registration-preview"></a>Información del registro de seguridad combinados (versión preliminar)

Antes del registro combinado, los usuarios registrar los métodos de autenticación para Azure Multi-factor Authentication y restablecimiento de contraseña de autoservicio (SSPR) por separado. Las personas comprendían que se usaron métodos similares para la autenticación multifactor y SSPR pero tenían que registrarse para ambas características. Ahora, con el registro combinado, los usuarios pueden registrar una vez y obtener las ventajas de la autenticación multifactor y SSPR.

![Mi mostrando perfil registra información de seguridad de un usuario](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Antes de habilitar la nueva experiencia, revise esta documentación centrada en el administrador y la documentación centrada en usuario para asegurarse de que entiende la funcionalidad y el efecto de esta característica. Basar su formación en la documentación del usuario para preparar los usuarios para la nueva experiencia y ayudan a garantizar un lanzamiento satisfactorio.

|     |
| --- |
| Información del registro de seguridad combinada para la autenticación multifactor y restablecimiento de contraseña de autoservicio de Azure Active Directory (Azure AD) es una característica de versión preliminar pública de Azure AD. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!IMPORTANT]
> Los usuarios que están habilitados para la versión preliminar original y la experiencia de registro combinado mejorada verán el nuevo comportamiento. Los usuarios que están habilitados para ambas experiencias verán la nueva experiencia de mi perfil. El nuevo perfil de mi se alinea con la apariencia y comportamiento del registro combinado y ofrece una experiencia perfecta para los usuarios. Los usuarios pueden ver mi perfil yendo a [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com).

Mis páginas de perfil se localizan basándose en la configuración de idioma del equipo acceso a la página. Microsoft almacena el idioma más reciente utilizado en la caché del explorador, por lo que intenta acceder a las páginas se seguirá representando en el último idioma utilizado. Si desactiva la memoria caché, las páginas se volverá a representar. Si desea forzar un idioma específico, puede agregar `?lng=<language>` al final de la dirección URL, donde `<language>` es el código del idioma que desea representar.

![Configurar SSPR u otros métodos de comprobación de seguridad](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Métodos disponibles en el registro combinado

Combinar registro admite los siguientes métodos de autenticación y las acciones:

|   | Register  | Change | Eliminar |
| --- | --- | --- | --- |
| Microsoft Authenticator | Sí (máximo de 5) | Sin  | Sí |
| Otra aplicación de autenticador | Sí (máximo de 5) | Sin  | Sí |
| Token de hardware | Sin  | No | Sí |
| Teléfono | Sí | Sí | Sí |
| Teléfono alternativo | Sí | Sí | Sí |
| Teléfono del trabajo | Sin  | No | Sin  |
| Email | Sí | Sí | Sí |
| Preguntas de seguridad | Sí | Sin  | Sí |
| Contraseñas de aplicación | Sí | Sin  | Sí |

> [!NOTE]
> Las contraseñas de aplicación solo están disponibles para los usuarios que se han impuesto para la autenticación multifactor. Las contraseñas de aplicación no están disponibles para los usuarios que están habilitados para la autenticación multifactor a través de una directiva de acceso condicional.

Como el método de autenticación multifactor de forma predeterminada, los usuarios pueden establecer una de las opciones siguientes:

- Microsoft Authenticator: notificación.
- Hardware o la aplicación de autenticador de tokens: el código.
- Llamada de teléfono.
- mensaje de texto.

Puesto que seguimos agregar más métodos de autenticación a Azure AD, esos métodos estará disponibles en el registro combinado.

## <a name="combined-registration-modes"></a>Modos de registro combinado

Existen dos modos de registro combinado: interrumpir y administrar.

- **Modo de interrupción** es una experiencia de asistente, se presentan a los usuarios cuando registrar o actualizar su información de seguridad en el inicio de sesión.

- **Administrar el modo de** forma parte del perfil de usuario y permite a los usuarios administrar su información de seguridad.

En ambos modos, los usuarios que han registrado previamente un método que se puede usar para la autenticación multifactor necesitará realizar autenticación multifactor para que pueden acceder a su información de seguridad.

### <a name="interrupt-mode"></a>Modo de interrupción

Registro combinado respeta las directivas de autenticación multifactor y SSPR, si ambos están habilitados para el inquilino. Estas directivas controlan si se interrumpe un usuario para el registro durante el inicio de sesión y qué métodos hay disponibles para el registro.

Estos son algunos escenarios en los que los usuarios deban registrar o actualizar su información de seguridad:

* Registro de la autenticación multifactor forzado mediante Identity Protection: Los usuarios deben registrar durante el inicio de sesión. Registrar los métodos de autenticación multifactor y los métodos SSPR (si el usuario está habilitado para SSPR).
* Registro de la autenticación multifactor forzado mediante la autenticación de Multi-factor Authentication por usuario: Los usuarios deben registrar durante el inicio de sesión. Registrar los métodos de autenticación multifactor y los métodos SSPR (si el usuario está habilitado para SSPR).
* Registro de la autenticación multifactor forzado a través de acceso condicional y otras directivas: Se piden a los usuarios a registrarse cuando use un recurso que requiere autenticación multifactor. Registrar los métodos de autenticación multifactor y los métodos SSPR (si el usuario está habilitado para SSPR).
* Aplicadas el registro de SSPR: Los usuarios deben registrar durante el inicio de sesión. Se registran sólo los métodos SSPR.
* Fuerza la actualización SSPR: Los usuarios deben revisar su información de seguridad en un intervalo establecido por el administrador. Los usuarios se muestran su información y se pueden confirmar la información actual o realizar cambios si es necesario.

Cuando se aplica el registro, los usuarios se muestran el número mínimo de los métodos necesarios para cumplir las directivas de autenticación multifactor y SSPR de más a menos seguro.

Por ejemplo: 

* Un usuario está habilitado para SSPR. La directiva SSPR requiere dos métodos para restablecer y ha habilitado el teléfono, correo electrónico y el código de aplicación móvil.
   * Este usuario debe registrar dos métodos.
      * Se muestra al usuario Authenticator y teléfono de forma predeterminada.
      * El usuario puede elegir registrar de correo electrónico en lugar de la aplicación authenticator o el teléfono.

Este diagrama de flujo describe los métodos que se muestran para un usuario cuando se interrumpe registrar durante el inicio de sesión:

![Diagrama de flujo de información de seguridad combinada](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Si tiene una autenticación multifactor y SSPR habilitada, se recomienda que aplique el registro de autenticación multifactor.

Si la directiva SSPR requiere que los usuarios que revisen su información de seguridad a intervalos regulares, los usuarios se interrumpió durante el inicio de sesión y se muestra a todos sus métodos registrados. Si está actualizado o puede realizar cambios si es necesario, puede confirmar la información actual.

### <a name="manage-mode"></a>Modo de administrar

Pueden tener acceso los usuarios administrar el modo de ello, vaya a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo) o seleccionando **información de seguridad** de mi perfil. Desde allí, los usuarios pueden agregar métodos, eliminar o cambiar los métodos existentes, cambiar el método predeterminado y mucho más.

## <a name="key-usage-scenarios"></a>Escenarios de uso clave

### <a name="set-up-security-info-during-sign-in"></a>Configurar la información de seguridad durante el inicio de sesión

Un administrador ha registro exigido.

Un usuario no ha configurado toda la información de seguridad requeridos y entra en el portal de Azure. Después de escribir el nombre de usuario y la contraseña, se solicita al usuario para configurar la información de seguridad. El usuario, a continuación, sigue los pasos que se muestra en el Asistente para configurar la información de seguridad necesarios. Si la configuración lo permite, el usuario puede configurar los métodos que no sean las que se muestran de forma predeterminada. Después de completar al asistente, los usuarios revisar los métodos que se configuraron y su método predeterminado para la autenticación multifactor. Para completar el proceso de instalación, el usuario confirma la información y continúa en el portal de Azure.

### <a name="set-up-security-info-from-my-profile"></a>Configurar la información de seguridad desde mi perfil

Un administrador no aplica el registro.

Un usuario que aún no ha configurado toda información de seguridad requiere que se va a [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com). El usuario selecciona **información de seguridad** en el panel izquierdo. Desde allí, el usuario decide agregar un método, selecciona cualquiera de los métodos disponibles y sigue los pasos para configurar ese método. Cuando termine, el usuario ve el método que acaba de configurar en la página de información de seguridad.

### <a name="delete-security-info-from-my-profile"></a>Eliminar información de seguridad de mi perfil

Un usuario que previamente haya configurado al menos un método navega a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). El usuario decide eliminar uno de los métodos previamente registrados. Cuando termine, el usuario ya no ve ese método en la página de información de seguridad.

### <a name="change-the-default-method-from-my-profile"></a>Cambiar el método predeterminado de mi perfil

Un usuario que previamente haya configurado al menos un método que puede usarse para la autenticación multifactor navega a [ https://aka.ms/mysecurityinfo ](https://aka.ms/mysecurityinfo). El usuario cambia el método predeterminado actual a un método distinto. Cuando termine, el usuario ve el nuevo método de forma predeterminada en la página de información de seguridad.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar el registro combinado en el inquilino](howto-registration-mfa-sspr-combined.md)

[Métodos disponibles para la autenticación multifactor y SSPR](concept-authentication-methods.md)

[Configuración del autoservicio de restablecimiento de contraseña](howto-sspr-deployment.md)

[Configuración de Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
