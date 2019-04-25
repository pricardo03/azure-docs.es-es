---
title: Solución de problemas de protección de contraseña de Azure AD - Azure Active Directory
description: Comprender que Azure AD contraseña protección comunes solución de problemas
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
ms.openlocfilehash: 108ead982529d2ac6549cceffd9d2177ab6456bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414773"
---
# <a name="azure-ad-password-protection-troubleshooting"></a>Solución de problemas de la Protección con contraseña de Azure AD

Después de la implementación de Protección de contraseña de Azure AD, puede ser necesario solucionar problemas. Este artículo entra en detalles para ayudarle a entender algunos pasos de solución de problemas comunes.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>El agente de controlador de dominio no puede localizar a un servidor proxy en el directorio

El principal síntoma de este problema es 30017 eventos en el registro de eventos de administración de agente de DC.

La causa habitual de este problema es que un servidor proxy aún no se ha registrado. Si se ha registrado un servidor proxy, puede haber algún retraso debido a la latencia de replicación de AD hasta que un agente de controlador de dominio determinado se puede ver a ese proxy.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>El agente de controlador de dominio no es capaz de comunicarse con un servidor proxy

El principal síntoma de este problema es 30018 eventos en el registro de eventos de administración de agente de DC. Esto puede deberse a varias causas:

1. El agente de controlador de dominio se encuentra en una parte aislada de la red que no permite la conectividad de red a los servidores registrados. Este problema, por tanto, puede ser expected\benign siempre que otros agentes de DC pueden comunicarse con los servidores con el fin de descargar las directivas de contraseñas de Azure, que, a continuación, se obtendrá el controlador de dominio aislado a través de la replicación de los archivos de directivas en el recurso compartido sysvol.

1. El equipo host de proxy está bloqueando el acceso para el punto de conexión RPC endpoint mapper (puerto 135)

   El instalador de Proxy de la protección de contraseña de Azure AD crea automáticamente una regla de entrada de Firewall de Windows que permita el acceso al puerto 135. Si más adelante se elimina o se deshabilita esta regla, los agentes de DC no se pueden comunicarse con el servicio de Proxy. Si se ha deshabilitado el Firewall de Windows builtin en lugar de otro producto de firewall, debe configurar el firewall para permitir el acceso al puerto 135.

1. La máquina host de proxy está bloqueando el acceso al extremo de RPC (dinámico o estático) que escucha en el servicio de Proxy

   El instalador de Proxy de la protección de contraseña de Azure AD crea automáticamente un Firewall de Windows escucha regla de entrada que permite el acceso a los puertos de entrada para el servicio de Proxy de la protección de contraseña de Azure AD. Si más adelante se elimina o se deshabilita esta regla, los agentes de DC no se pueden comunicarse con el servicio de Proxy. Si se ha deshabilitado el Firewall de Windows builtin en lugar de otro producto de firewall, debe configurar firewall para permitir el acceso a los puertos de entrada a los que escucha el servicio de Proxy de la protección de contraseña de Azure AD. Esta configuración se puede realizar más específica si se ha configurado el servicio de Proxy para que escuche en un puerto estático específico de RPC (mediante el `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet).

## <a name="the-proxy-service-can-receive-calls-from-dc-agents-in-the-domain-but-is-unable-to-communicate-with-azure"></a>El servicio de Proxy puede recibir llamadas desde los agentes de DC en el dominio, pero no puede comunicarse con Azure

1. Asegúrese de que la máquina de proxy tiene conectividad a los puntos de conexión aparece en el [los requisitos de implementación](howto-password-ban-bad-on-premises-deploy.md).

1. Asegúrese de que el bosque y el proxy de todos los servidores están registrados en el mismo inquilino de Azure.

   Puede comprobarlo ejecutando el `Get-AzureADPasswordProtectionProxy` y `Get-AzureADPasswordProtectionDCAgent` cmdlets de PowerShell, a continuación, compare el `AzureTenant` propiedad de cada elemento devuelto. Para funcionar correctamente estos deben ser el mismo dentro de un bosque, en todos los agentes de DC y servidores proxy.

   Si existe una condición de error de coincidencia del registro de inquilino de Azure, esto puede repararse ejecutando la `Register-AzureADPasswordProtectionProxy` o `Register-AzureADPasswordProtectionForest` según sea necesario, asegurándose de usar credenciales desde el mismo inquilino de Azure para todos los registros de los cmdlets de PowerShell.

## <a name="the-dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files-and-other-state"></a>El agente de controlador de dominio no puede cifrar o descifrar los archivos de directivas de contraseña y otro estado

Este problema, se puede producir con una variedad de síntomas, pero normalmente tiene una causa común.

Protección con contraseña de AD Azure tiene una dependencia crítica en la funcionalidad de cifrado y descifrado proporcionada por el servicio de distribución de claves de Microsoft, que está disponible en los controladores de dominio que ejecutan Windows Server 2012 y versiones posteriores. El servicio KDS debe estar habilitado y funciona en todos los Windows Server 2012 y versiones posteriores controladores de dominio en un dominio.

De forma predeterminada el KDS modo de inicio del servicio del servicio está configurado como Manual (desencadenador de inicio). Esta configuración significa que la primera vez que un cliente intenta usar el servicio, se inicia a petición. Este modo de inicio del servicio predeterminada es aceptable para la protección de contraseña de Azure AD para que funcione.

Si el modo de inicio del servicio KDS se ha configurado como deshabilitado, esta configuración se corrijan antes de que la protección con contraseña de Azure AD funcione correctamente.

Una prueba sencilla para este problema consiste en iniciar manualmente el servicio KDS, ya sea a través de la consola MMC de administración de servicio, o con otras herramientas de administración de servicio (por ejemplo, ejecute "net start kdssvc" desde una consola de línea de comandos). El servicio KDS se espera que se inician correctamente y permanecen en ejecución.

La causa más común para el servicio KDS que no se pueda iniciar es que el objeto de controlador de dominio de Active Directory se encuentra fuera de la unidad organizativa controladores de dominio predeterminado. Esta configuración no es compatible con el servicio KDS y no es una limitación impuesta por la protección con contraseña de Azure AD. La solución para esta condición consiste en mover el objeto de controlador de dominio a una ubicación en la unidad organizativa controladores de dominio predeterminado.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Las contraseñas se están aceptando pero no deben ser

Este problema puede tener varias causas.

1. Los agentes de DC no pueden descargar una directiva o no puede descifrar las directivas existentes. Busque posibles causas de los temas anteriores.

1. El modo Exigir de la directiva de contraseñas sigue establecido en Auditoría. Si esta configuración está en vigor, volver a configurar para aplicar mediante el portal de protección mediante contraseña de Azure AD. Consulte [protección con contraseña habilitar](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Se ha deshabilitado la directiva de contraseñas. Si esta configuración está en vigor, reconfigurarlo habilitada mediante el portal de protección mediante contraseña de Azure AD. Consulte [protección con contraseña habilitar](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. No ha instalado el software del agente de controlador de dominio en todos los controladores de dominio en el dominio. En esta situación, es difícil garantizar que los clientes remotos de Windows como destino un controlador de dominio en particular durante una operación de cambio de contraseña. Si se piensa ha correctamente el destino de un DC determinado está instalado el software del agente de controlador de dominio, puede comprobar si olvida el registro de eventos de administración de agente DC: independientemente del resultado, habrá al menos un evento en el resultado de la contraseña de un documento validación. Si no hay ningún evento presente para el usuario cuya contraseña se cambia, el cambio de contraseña es probable que se procesó por otro controlador de dominio.

   Como una prueba alternativa, intente setting\changing contraseñas al iniciar sesión directamente en un controlador de dominio donde está instalado el software del agente de controlador de dominio. Esta técnica no se recomienda para los dominios de Active Directory de producción.

   Aunque se admite la implementación incremental del software del agente DC sujetos a estas limitaciones, Microsoft recomienda encarecidamente que el software de controlador de dominio del agente está instalado en todos los controladores de dominio en un dominio tan pronto como sea posible.

1. El algoritmo de validación de contraseña realmente puede estar trabajando según lo previsto. Consulte [cómo se evalúan las contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Modo de reparación de servicios de directorio

Si el controlador de dominio se inicia en modo de reparación de servicios de directorio, el servicio del agente DC detecta esta condición y hará que todos los de validación de contraseña o las actividades de cumplimiento que se deshabilite, independientemente de la configuración de directiva activa actualmente.

## <a name="emergency-remediation"></a>Corrección de emergencia

Si se produce una situación en la que el servicio del agente de controlador de dominio causa problemas, este servicio se puede apagar inmediatamente. La DLL de filtro de contraseña del agente de controlador de dominio sigue intentando llamar al servicio que no está en ejecución, y registrará los eventos de advertencia (10012, 10013), pero durante ese tiempo se aceptan todas las contraseñas entrantes. El servicio de agente de controlador de dominio se puede también configurar mediante el Administrador de control de servicios de Windows con un tipo de inicio "Deshabilitado" según sea necesario.

Otra medida de corrección sería establecer el modo Habilitar en No en el portal de Protección de contraseña de Azure AD. Una vez que se ha descargado la directiva actualizada, cada servicio del agente de controlador de dominio entrará en un modo inactivo donde todas las contraseñas se aceptan tal cual. Para más información, consulte [Modo forzado](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Degradación del controlador de dominio

Se admite la degradación de un controlador de dominio en el que todavía se ejecuta el software del agente de controlador de dominio. No obstante, los administradores deben tener en cuenta que el software del agente de controlador de dominio sigue aplicando la directiva de contraseñas actual durante el procedimiento de degradación. La nueva contraseña de la cuenta de administrador local (que se especifica como parte de la operación de degradación) se valida como cualquier otra contraseña. Microsoft recomienda que se elijan contraseñas seguras para las cuentas de administrador locales como parte de un procedimiento de degradación del controlador de dominio. Sin embargo, la validación de la nueva contraseña de la cuenta de administrador local por parte del software del agente de controlador de dominio puede ser perjudicial para los procedimientos operativos de degradación que existían previamente.

Una vez que se ha realizado correctamente la degradación y el controlador de dominio se ha reiniciado y está de nuevo en ejecución como un servidor miembro normal, el software del agente de controlador de dominio se vuelve a ejecutar en modo pasivo. A partir de ese momento se puede desinstalar en cualquier momento.

## <a name="removal"></a>Eliminación

Si se decide desinstalar el software de protección de contraseña de Azure AD y el Liberador de espacio en todos ellos relacionados con estado de los dominios y bosques, esta tarea puede realizarse mediante los pasos siguientes:

> [!IMPORTANT]
> Es importante que realice estos pasos en orden. Si se deja cualquier instancia del servicio de proxy en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint. Si se deja cualquier instancia del servicio del agente de controlador de dominio en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint y el estado sysvol.

1. Desinstale el software del proxy de todas las máquinas. En este paso **no** es necesario reiniciar.
2. Desinstale el software del agente de controlador de dominio de todos los controladores de dominio. En este paso es **necesario** reiniciar.
3. Quite manualmente todos los puntos de conexión del servicio de proxy en cada contexto de nomenclatura de dominio. La ubicación de estos objetos se puede detectar con el siguiente comando de Active Directory PowerShell:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   No omita el asterisco ("*") al final del valor de variable $keywords.

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente.

4. Quite manualmente todos los puntos de conexión del agente de controlador de dominio de cada contexto de nomenclatura de dominio. Puede haber uno estos objetos por cada controlador de dominio del bosque, según cuán extensamente se implementó el software. La ubicación de ese objeto se puede detectar con el siguiente comando de Active Directory PowerShell:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente.

   No omita el asterisco ("*") al final del valor de variable $keywords.

5. Quite manualmente el estado de configuración en el nivel de bosque. El estado de configuración del bosque se conserva en un contenedor en el contexto de nomenclatura de configuración de Active Directory. Se puede detectar y eliminar de la siguiente manera:

   ```powershell
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
