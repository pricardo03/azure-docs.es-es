---
title: Operaciones de versión preliminar de la protección con contraseña de Azure AD e informes
description: Operaciones de versión preliminar de la protección con contraseña posterior a la implementación de Azure AD e informes
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 14aa52b6d424423f4863efa63f3e2e66b84dac70
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163690"
---
# <a name="preview-azure-ad-password-protection-post-deployment"></a>Versión preliminar: Protección con contraseña de Azure AD posterior a la implementación

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Después de haber completado la [instalación de la protección con contraseña de Azure AD](howto-password-ban-bad-on-premises.md) local, hay algunos elementos que se deben configurar en Azure Portal.

## <a name="configure-the-custom-banned-password-list"></a>Configuración de la lista personalizada de contraseñas prohibidas

Siga las instrucciones del artículo [Configuración de la lista personalizada de contraseñas prohibidas](howto-password-ban-bad.md) para conocer los pasos para personalizar la lista de contraseñas prohibidas de su organización.

## <a name="enable-password-protection"></a>Habilitación de la protección con contraseña

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory**, **Métodos de autenticación** y **Protección con contraseña (versión preliminar)**.
1. Establezca **Enable password protection on Windows Server Active Directory** (Habilitar protección con contraseña en Windows Server Active Directory) en **Sí**.
1. Como se mencionó en la [Guía de implementación](howto-password-ban-bad-on-premises.md#deployment-strategy), inicialmente se recomienda establecer el **Modo** en **Auditoría**
   * Cuando se sienta cómodo con la característica, puede cambiar el **Modo** a **Forzado**
1. Haga clic en **Guardar**

![Habilitación de componentes de la protección con contraseña de Azure AD en Azure Portal](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Modo de auditoría

El modo de auditoría está previsto como una forma de ejecutar el software en un modo "what if". Cada servicio de agente de controlador de dominio evalúa una contraseña de entrada según la directiva activa en el momento. Si la directiva activa se ha configurado para estar en modo de auditoría, las contraseñas "incorrectas" se traducen en mensajes del registro de errores pero son aceptadas. Esta es la única diferencia entre el modo de auditoría y el modo forzado; todas las demás operaciones se ejecutan de la misma manera.

> [!NOTE]
> Microsoft recomienda que la implementación y las pruebas iniciales comiencen siempre en el modo auditoría. Los eventos del registro de eventos se deben supervisar después para tantear si alguno de los procesos operativos existentes se vería perturbado una vez que se habilite el modo forzado.

## <a name="enforce-mode"></a>Modo forzado

El modo forzado está previsto como la configuración final. Como en el anterior modo de auditoría, cada servicio de agente de controlador de dominio evalúa las contraseñas de entrada según la directiva activa en el momento. Aunque si el modo forzado está habilitado, una contraseña que se considera no segura según la directiva se rechaza.

Cuando un agente de controlador de dominio de la protección con contraseña de Azure AD DC rechaza una contraseña en modo de aplicación, el impacto visible que percibe un usuario final es idéntico al que percibiría si su contraseña fuese rechazada por la aplicación tradicional de complejidad de la contraseña local. Por ejemplo, un usuario podría ver el siguiente mensaje de error tradicional en la pantalla de inicio de sesión/cambio de contraseña:

"No se puede actualizar la contraseña. El valor proporcionado para la nueva contraseña no cumple los requisitos de longitud, complejidad o de historial del dominio".

Este mensaje es solo un ejemplo de varios resultados posibles. El mensaje de error específico puede variar según el escenario o software real que intenta establecer una contraseña no segura.

Puede que los usuarios finales afectados tengan que trabajar con su personal de TI para comprender los nuevos requisitos y tener más capacidad de elegir contraseñas seguras.

## <a name="usage-reporting"></a>Informes de uso

Se puede usar el cmdlet `Get-AzureADPasswordProtectionSummaryReport` para generar una vista resumida de la actividad. A continuación se muestra un ejemplo de salida de este cmdlet:

```
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

El ámbito de los informes del cmdlet puede condicionarse con uno de los parámetros –Forest, -Domain o –DomainController. No especificar un parámetro implica –Forest.

> [!NOTE]
> Este cmdlet funciona consultando remotamente el registro de eventos de administración del cada servicio de agente de controlador de dominio. Si los registros de eventos contienen un gran número de eventos, el cmdlet puede tardar mucho tiempo en completarse. Además, las consultas de red masivas de grandes conjuntos de datos pueden afectar al rendimiento del controlador de dominio. Por lo tanto, este cmdlet debe usarse con cuidado en entornos de producción.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas e información de registro en la protección con contraseña de Azure AD](howto-password-ban-bad-on-premises-troubleshoot.md)
