---
title: Introducción al registro combinado para SSPR de Azure AD y MFA (versión preliminar)
description: Habilitar combina autenticación multifactor de Azure AD y restablecimiento de contraseña de registro (versión preliminar)
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
ms.openlocfilehash: 2a6896e2b9633b8de679e8d14a7957dc0e3229e7
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226732"
---
# <a name="enable-combined-security-information-registration-preview"></a>Información de seguridad permiten combinar del registro (versión preliminar)

Antes de habilitar la nueva experiencia, revise el artículo [combinar información de registro de seguridad (versión preliminar)](concept-registration-mfa-sspr-combined.md) para asegurarse de que entiende la funcionalidad y el impacto de esta característica.

![Información del registro de seguridad combinada mejorada experiencia solicitar más información en el inicio de sesión. Registrar la aplicación Microsoft Authenticator como primer método muestra de ejemplo.](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Información del registro de seguridad combinada para el restablecimiento de contraseña de autoservicio de Azure Multi-factor Authentication y Azure AD es una característica de versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="enable-combined-registration"></a>Habilitar registro combinado

Complete los pasos siguientes para habilitar el registro combinado:

1. Inicie sesión en el portal de Azure como un usuario administrador o administrador global.
2. Vaya a **Azure Active Directory** > **Configuración de usuario** > **Administrar la configuración de las características en versión preliminar del panel de acceso**.
3. En **los usuarios pueden usar características para registrar y administrar información de seguridad de vista previa: actualiza**, optar por habilitar para una **seleccionados** grupo de usuarios o para **todas** a los usuarios.

![Habilitar la experiencia de versión preliminar de información de seguridad combinada para todos los usuarios en el portal de Azure AD](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> A partir de marzo de 2019 las opciones de llamada de teléfono no estará disponible para los usuarios MFA y SSPR en inquilinos de Azure AD gratuito de prueba. Mensajes SMS no se ven afectados por este cambio. Llamada de teléfono seguirá estando disponible para los usuarios de inquilinos de Azure AD de pago. Este cambio solo afecta a los inquilinos de Azure AD gratuito de prueba.

> [!NOTE]
> Una vez se habilite el registro combinado, los usuarios que registran o confirmación que su número de teléfono o aplicación móvil a través de la nueva experiencia puede usarlas para MFA y SSPR, si estos métodos están habilitados en las directivas de MFA o SSPR. Si, a continuación, se deshabilita esta experiencia, los usuarios que vaya al registro SSPR anterior página en `https:/aka.ms/ssprsetup` le pedirá que realicen autenticación multifactor para poder tener acceso a la página.

Si ha configurado el sitio de la lista de asignaciones de zona en los sitios siguientes deben estar en la misma zona de Internet Explorer:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Pasos siguientes

[Métodos disponibles para MFA y SSPR](concept-authentication-methods.md)

[Configuración del restablecimiento de contraseña de autoservicio](howto-sspr-deployment.md)

[Configurar Azure Multi-factor Authentication](howto-mfa-getstarted.md)

[Solución de problemas de combina el registro de información de seguridad](howto-registration-mfa-sspr-combined-troubleshoot.md)