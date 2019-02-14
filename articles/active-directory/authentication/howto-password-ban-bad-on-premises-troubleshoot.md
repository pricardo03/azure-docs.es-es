---
title: Solución de problemas en la versión preliminar de Protección con contraseña de Azure AD
description: Descripción de la solución de los problemas más habituales de la versión preliminar de Protección con contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5727965373752d40e3ce508c1bc79046c2b3b70b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177758"
---
# <a name="preview-azure-ad-password-protection-troubleshooting"></a>Vista previa: Solución de problemas de la Protección con contraseña de Azure AD

|     |
| --- |
| Protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Después de la implementación de Protección de contraseña de Azure AD, puede ser necesario solucionar problemas. Este artículo entra en detalles para ayudarle a entender algunos pasos de solución de problemas comunes.

## <a name="weak-passwords-are-not-getting-rejected-as-expected"></a>Las contraseñas débiles no se rechazan según lo previsto

Esto puede deberse a varias causas:

1. Los agentes de controlador de dominio aún no han descargado una directiva. El síntoma de esto son eventos 30001 en el registro de eventos de administración del agente de controlador de dominio.

    Entre las causas posibles para este problema se incluyen:

    1. El bosque todavía no está registrado
    2. El proxy todavía no está registrado
    3. Problemas de conectividad de red impiden que el servicio de proxy se comunique con Azure (compruebe los requisitos de proxy HTTP)

2. El modo Exigir de la directiva de contraseñas sigue establecido en Auditoría. Si esto es así, vuelva a configurarla en Exigir con el portal de Protección de contraseña de Azure AD. Consulte [Habilitación de la protección con contraseña](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

3. Se ha deshabilitado la directiva de contraseñas. Si esto es así, vuelva a configurarla en Habilitado con el portal de Protección de contraseña de Azure AD. Consulte [Habilitación de la protección con contraseña](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

4. Puede que el algoritmo de validación de contraseña funcione según lo esperado. Consulte [Cómo se evalúan las contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Modo de reparación de servicios de directorio

Si el controlador de dominio se inicia en el modo de reparación de servicios de directorio, el servicio del agente de controlador de dominio detecta esta situación y hará que se deshabiliten todas las actividades de validación de contraseñas o de aplicación de estas, independientemente de la configuración de directivas activa actualmente.

## <a name="emergency-remediation"></a>Corrección de emergencia

Si se produce una situación en la que el servicio del agente de controlador de dominio causa problemas, este servicio se puede apagar inmediatamente. La DLL de filtro de contraseña del agente de controlador de dominio sigue intentando llamar al servicio que no está en ejecución, y registrará los eventos de advertencia (10012, 10013), pero durante ese tiempo se aceptan todas las contraseñas entrantes. El servicio de agente de controlador de dominio se puede también configurar mediante el Administrador de control de servicios de Windows con un tipo de inicio "Deshabilitado" según sea necesario.

Otra medida de corrección sería establecer el modo Habilitar en No en el portal de Protección de contraseña de Azure AD. Una vez que se ha descargado la directiva actualizada, cada servicio del agente de controlador de dominio entrará en un modo inactivo donde todas las contraseñas se aceptan tal cual. Para más información, consulte [Modo forzado](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Degradación del controlador de dominio

Se admite la degradación de un controlador de dominio en el que todavía se ejecuta el software del agente de controlador de dominio. No obstante, los administradores deben tener en cuenta que el software del agente de controlador de dominio sigue aplicando la directiva de contraseñas actual durante el procedimiento de degradación. La nueva contraseña de la cuenta de administrador local (que se especifica como parte de la operación de degradación) se valida como cualquier otra contraseña. Microsoft recomienda que se elijan contraseñas seguras para las cuentas de administrador locales como parte de un procedimiento de degradación del controlador de dominio. Sin embargo, la validación de la nueva contraseña de la cuenta de administrador local por parte del software del agente de controlador de dominio puede ser perjudicial para los procedimientos operativos de degradación que existían previamente.

Una vez que se ha realizado correctamente la degradación y el controlador de dominio se ha reiniciado y está de nuevo en ejecución como un servidor miembro normal, el software del agente de controlador de dominio se vuelve a ejecutar en modo pasivo. A partir de ese momento se puede desinstalar en cualquier momento.

## <a name="removal"></a>Eliminación

Si decide desinstalar la versión preliminar pública del software y limpiar todos los estados relacionados de los dominios y el bosque, puede hacerlo mediante los pasos siguientes:

> [!IMPORTANT]
> Es importante que realice estos pasos en orden. Si se deja cualquier instancia del servicio de proxy en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint. Si se deja cualquier instancia del servicio del agente de controlador de dominio en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint y el estado sysvol.

1. Desinstale el software del proxy de todas las máquinas. En este paso **no** es necesario reiniciar.
2. Desinstale el software del agente de controlador de dominio de todos los controladores de dominio. En este paso es **necesario** reiniciar.
3. Quite manualmente todos los puntos de conexión del servicio de proxy en cada contexto de nomenclatura de dominio. La ubicación de estos objetos se puede detectar con el siguiente comando de Active Directory PowerShell:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   No omita el asterisco ("*") al final del valor de variable $keywords.

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente. 

4. Quite manualmente todos los puntos de conexión del agente de controlador de dominio de cada contexto de nomenclatura de dominio. Puede haber uno estos objetos por cada controlador de dominio del bosque, dependiendo del grado de extensión de la implementación de la versión preliminar del software. La ubicación de ese objeto se puede detectar con el siguiente comando de Active Directory PowerShell:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente.

   No omita el asterisco ("*") al final del valor de variable $keywords.

5. Quite manualmente el estado de configuración en el nivel de bosque. El estado de configuración del bosque se conserva en un contenedor en el contexto de nomenclatura de configuración de Active Directory. Se puede detectar y eliminar de la siguiente manera:

   ```PowerShell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Quite todos los estados relacionados con Sysvol mediante la eliminación manual de la siguiente carpeta y de todo su contenido:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Si es necesario, también se puede acceder localmente a esta ruta en un determinado controlador de dominio. La ubicación predeterminada debe ser parecida a la siguiente:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Esta ruta de acceso será diferente si se ha configurado el recurso compartido de Sysvol en una ubicación distinta a la predeterminada.

## <a name="next-steps"></a>Pasos siguientes

[Preguntas más frecuentes de la protección con contraseña de Azure AD](howto-password-ban-bad-on-premises-faq.md)

Para más información sobre las listas de contraseñas prohibidas globales y personalizadas, consulte el artículo [Ban bad passwords](concept-password-ban-bad.md) (Prohibición de contraseñas incorrectas).
