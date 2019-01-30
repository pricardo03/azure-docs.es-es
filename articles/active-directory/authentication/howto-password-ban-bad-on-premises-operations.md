---
title: Operaciones de versión preliminar de la protección con contraseña de Azure AD e informes
description: Operaciones de versión preliminar de la protección con contraseña posterior a la implementación de Azure AD e informes
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: c19275f096191a2d55c939cbc9b76e7ffc6e1b8b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424445"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>Vista previa: Procedimientos operativos de protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Después de haber completado la [instalación de la protección con contraseña de Azure AD](howto-password-ban-bad-on-premises-deploy.md) local, hay algunos elementos que se deben configurar en Azure Portal.

## <a name="configure-the-custom-banned-password-list"></a>Configuración de la lista personalizada de contraseñas prohibidas

Siga las instrucciones del artículo [Configuración de la lista personalizada de contraseñas prohibidas](howto-password-ban-bad-configure.md) para conocer los pasos para personalizar la lista de contraseñas prohibidas de su organización.

## <a name="enable-password-protection"></a>Habilitación de la protección con contraseña

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory**, **Métodos de autenticación** y **Protección con contraseña (versión preliminar)**.
1. Establezca **Enable password protection on Windows Server Active Directory** (Habilitar protección con contraseña en Windows Server Active Directory) en **Sí**.
1. Como se mencionó en la [Guía de implementación](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy), inicialmente se recomienda establecer el **Modo** en **Auditoría**
   * Cuando se sienta cómodo con la característica, puede cambiar el **Modo** a **Forzado**
1. Haga clic en **Guardar**

![Habilitación de componentes de la protección con contraseña de Azure AD en Azure Portal](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Modo de auditoría

El modo de auditoría está previsto como una forma de ejecutar el software en un modo "what if". Cada servicio de agente de controlador de dominio evalúa una contraseña de entrada según la directiva activa en el momento. Si la directiva activa se ha configurado para estar en modo de auditoría, las contraseñas "incorrectas" se traducen en mensajes del registro de errores pero son aceptadas. Esta es la única diferencia entre el modo de auditoría y el modo forzado; todas las demás operaciones se ejecutan de la misma manera.

> [!NOTE]
> Microsoft recomienda que la implementación y las pruebas iniciales comiencen siempre en el modo auditoría. Los eventos del registro de eventos se deben supervisar después para tantear si alguno de los procesos operativos existentes se vería perturbado una vez que se habilite el modo forzado.

## <a name="enforce-mode"></a>Modo forzado

El modo forzado está previsto como la configuración final. Como en el anterior modo de auditoría, cada servicio de agente de controlador de dominio evalúa las contraseñas de entrada según la directiva activa en el momento. Aunque si el modo forzado está habilitado, una contraseña que se considera no segura según la directiva se rechaza.

Cuando un agente de controlador de dominio de la protección con contraseña de Azure AD DC rechaza una contraseña en modo de aplicación, el impacto visible que percibe un usuario final es idéntico al que percibiría si su contraseña fuese rechazada por la aplicación tradicional de complejidad de la contraseña local. Por ejemplo, un usuario podría ver el siguiente mensaje de error tradicional en la pantalla de inicio de sesión/cambio de contraseña de Windows:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Este mensaje es solo un ejemplo de varios resultados posibles. El mensaje de error específico puede variar según el escenario o software real que intenta establecer una contraseña no segura.

Puede que los usuarios finales afectados tengan que trabajar con su personal de TI para comprender los nuevos requisitos y tener más capacidad de elegir contraseñas seguras.

## <a name="enable-mode"></a>Modo de habilitación

Por lo general, este ajuste debe dejarse en su estado habilitado predeterminado (Sí). Si se deshabilita (No), todos los agentes de controlador de dominio de protección con contraseña de Azure AD entrarán en modo inactivo y todas las contraseñas se aceptarán tal cual. No se ejecutará ninguna actividad de validación (por ejemplo, ni siquiera se emitirán los eventos de auditoría).

## <a name="usage-reporting"></a>Informes de uso

Se puede usar el cmdlet `Get-AzureADPasswordProtectionSummaryReport` para generar una vista resumida de la actividad. A continuación se muestra un ejemplo de salida de este cmdlet:

```PowerShell
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
> Este cmdlet funciona al abrir una sesión de PowerShell en cada controlador de dominio. Para que la operación se realice correctamente, se debe habilitar la compatibilidad con la sesión remota de PowerShell en cada controlador de dominio y el cliente debe tener privilegios suficientes. Para más información sobre los requisitos de la sesión remota de Powershell, ejecute "Get-Help about_Remote_Troubleshooting" en una ventana de PowerShell.

> [!NOTE]
> Este cmdlet funciona consultando remotamente el registro de eventos de administración del cada servicio de agente de controlador de dominio. Si los registros de eventos contienen un gran número de eventos, el cmdlet puede tardar mucho tiempo en completarse. Además, las consultas de red masivas de grandes conjuntos de datos pueden afectar al rendimiento del controlador de dominio. Por lo tanto, este cmdlet debe usarse con cuidado en entornos de producción.

## <a name="dc-agent-discovery"></a>Detección del agente de controlador de dominio

Se puede usar el cmdlet `Get-AzureADPasswordProtectionDCAgent` para mostrar información básica sobre los diversos agentes de controlador de dominio que se están ejecutando en un dominio o bosque. Esta información se recupera de los objetos serviceConnectionPoint registrados por los servicios de agente de controlador de dominio en ejecución. A continuación se muestra un ejemplo de salida de este cmdlet:

```PowerShell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Cada servicio de agente de controlador de dominio actualiza las diversas propiedades una vez cada hora aproximadamente. Los datos siguen estando sujetos a la latencia de replicación de Active Directory.

Se puede influenciar el ámbito de la consulta del cmdlet con los parámetros –Forest o –Domain.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas y supervisión para la protección con contraseña de Azure AD](howto-password-ban-bad-on-premises-troubleshoot.md)
