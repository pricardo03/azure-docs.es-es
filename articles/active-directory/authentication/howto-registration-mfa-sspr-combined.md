---
title: 'Introducción al registro combinado para SSPR de Azure AD y Multi-factor Authentication (versión preliminar): Azure Active Directory'
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
ms.openlocfilehash: 3baf2690ae07b87bb4d5dba30fcd20f62a1a4506
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280577"
---
# <a name="enable-combined-security-information-registration-preview"></a>Información de seguridad permiten combinar del registro (versión preliminar)

Antes de habilitar la nueva experiencia, revise el artículo [combinar información de registro de seguridad (versión preliminar)](concept-registration-mfa-sspr-combined.md) para asegurarse de que entiende la funcionalidad y los efectos de esta característica.

![Información del registro de seguridad combinada una experiencia mejorada](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| Información del registro de seguridad combinada para Azure Multi-factor Authentication y restablecimiento de contraseña de autoservicio de Azure Active Directory (Azure AD) es una característica de versión preliminar pública de Azure AD. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="enable-combined-registration"></a>Habilitar registro combinado

Siga estos pasos para habilitar el registro combinado:

1. Inicie sesión en el portal de Azure como un usuario administrador o administrador global.
2. Vaya a **Azure Active Directory** > **configuración de usuario** > **administrar la configuración de características de vista previa del panel de acceso**.
3. En **los usuarios pueden usar características para registrar y administrar información de seguridad de vista previa: actualiza**, optar por habilitar para una **seleccionados** grupo de usuarios o para **todas** a los usuarios.

   ![Habilitar la experiencia de versión preliminar de información de seguridad combinada para todos los usuarios](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> A partir de marzo de 2019, las opciones de llamada de teléfono no estarán disponibles para la autenticación multifactor y los usuarios SSPR en inquilinos de Azure AD gratuito de prueba. Mensajes SMS no se ven afectados por este cambio. Las opciones de llamada de teléfono aún estará disponibles para los usuarios de inquilinos de Azure AD de pago.

> [!NOTE]
> Después de habilitar el registro combinado, los usuarios que registran o confirme el número de teléfono o aplicación móvil a través de la nueva experiencia puede usarlas para autenticación multifactor y SSPR, si estos métodos están habilitados en la autenticación multifactor y SSPR directivas. Si, a continuación, se deshabilita esta experiencia, los usuarios que vaya al registro SSPR anterior página en `https:/aka.ms/ssprsetup` le pedirá que realicen autenticación multifactor para poder tener acceso a la página.

Si ha configurado el sitio de la lista de asignación de zona de Internet Explorer, visite los sitios siguientes deben estar en la misma zona:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="next-steps"></a>Pasos siguientes

[Métodos disponibles para la autenticación multifactor y SSPR](concept-authentication-methods.md)

[Configuración del autoservicio de restablecimiento de contraseña](howto-sspr-deployment.md)

[Configuración de Azure Multi-Factor Authentication](howto-mfa-getstarted.md)

[Solución de problemas de combina el registro de información de seguridad](howto-registration-mfa-sspr-combined-troubleshoot.md)