---
title: 'Solución de problemas con Protección de contraseñas de Azure AD: Azure Active Directory'
description: Descripción de la solución de los problemas más frecuentes de Protección de contraseñas de Azure AD
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
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60414773"
---
# <a name="azure-ad-password-protection-troubleshooting"></a>Solución de problemas de la Protección con contraseña de Azure AD

Después de la implementación de Protección de contraseña de Azure AD, puede ser necesario solucionar problemas. Este artículo entra en detalles para ayudarle a entender algunos pasos de solución de problemas comunes.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>El agente de controlador de dominio no puede localizar un proxy en el directorio

El síntoma principal de este problema son eventos 30017 en el registro de eventos de administración del agente de controlador de dominio.

La causa habitual de este problema es que no se ha registrado aún un proxy. Si se ha registrado un proxy, puede haber algún retraso debido a la latencia de replicación de AD hasta que un agente de controlador de dominio determinado pueda ver el proxy.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>El agente de controlador de dominio no es capaz de comunicarse con un proxy

El síntoma principal de este problema son eventos 30018 en el registro de eventos de administración del agente de controlador de dominio. Esto puede deberse a varias causas:

1. El agente de controlador de dominio se encuentra en una parte aislada de la red que no permite la conectividad de red con el proxy registrado. Este problema, por tanto, puede ser esperado/benigno siempre que otros agentes de controlador de dominio puedan comunicarse con el proxy con el fin de descargar las directivas de contraseñas de Azure, que se obtendrán mediante el controlador de dominio aislado a través de la replicación de los archivos de directivas en el recurso compartido sysvol.

1. El equipo host de proxy está bloqueando el acceso para el punto de conexión del mapeador del punto de conexión de RPC (puerto 135)

   El instalador de proxy de protección de contraseña de Azure AD crea automáticamente una regla de entrada de firewall de Windows que permite el acceso al puerto 135. Si más adelante se elimina o se deshabilita esta regla, los agentes de controlador de dominio no pueden comunicarse con el servicio de proxy. Si se ha deshabilitado el firewall de Windows integrado en lugar de otro producto de firewall, debe configurar el firewall para permitir el acceso al puerto 135.

1. La máquina host de proxy está bloqueando el acceso al punto de conexión de RPC (dinámico o estático) que escucha el servicio de proxy

   El instalador de proxy de protección de contraseña de Azure AD crea automáticamente una regla de entrada de firewall de Windows que permite el acceso a cualquier puerto de entrada que escuche el servicio de proxy de Protección con contraseña de Azure AD. Si más adelante se elimina o se deshabilita esta regla, los agentes de controlador de dominio no pueden comunicarse con el servicio de proxy. Si se ha deshabilitado el firewall de Windows integrado en lugar de otro producto de firewall, debe configurar el firewall para permitir el acceso a cualquier puerto de entrada que escuche el servicio de proxy de Protección con contraseña de Azure AD. Esta configuración puede ser más específica si se ha configurado el servicio de proxy para que escuche un puerto estático de RPC específico (mediante el cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`).

## <a name="the-proxy-service-can-receive-calls-from-dc-agents-in-the-domain-but-is-unable-to-communicate-with-azure"></a>El servicio de proxy puede recibir llamadas de los agentes de controlador de dominio en el dominio, pero no puede comunicarse con Azure

1. Asegúrese de que la máquina de proxy dispone de conectividad con los puntos de conexión que aparecen en los [requisitos de implementación](howto-password-ban-bad-on-premises-deploy.md).

1. Asegúrese de que el bosque y todos los servidores proxy estén registrados en el mismo inquilino de Azure.

   Puede comprobarlo ejecutando los cmdlets de PowerShell `Get-AzureADPasswordProtectionProxy` u `Get-AzureADPasswordProtectionDCAgent` y, a continuación, comparando la propiedad `AzureTenant` de cada elemento devuelto. Para un funcionamiento correcto, deben ser los mismos dentro del bosque, en todos los agentes de controlador de dominio y servidores proxy.

   Si existe una condición de error de coincidencia del registro de inquilino de Azure, puede repararse ejecutando los cmdlets de PowerShell `Register-AzureADPasswordProtectionProxy` o `Register-AzureADPasswordProtectionForest` según sea necesario y asegurándose de usar credenciales desde el mismo inquilino de Azure para todos los registros.

## <a name="the-dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files-and-other-state"></a>El agente de controlador de dominio no puede cifrar o descifrar los archivos de directivas de contraseña y otro estado

Este problema se puede producir con una variedad de síntomas, pero normalmente tiene una causa común.

Protección con contraseña de AD Azure tiene una dependencia crítica en la funcionalidad de cifrado y descifrado proporcionada por el servicio de distribución de claves de Microsoft, que está disponible en los controladores de dominio que ejecutan Windows Server 2012 y versiones posteriores. El servicio KDS debe estar habilitado y funcionar en todos controladores de dominio de Windows Server 2012 y versiones posteriores en un dominio.

De forma predeterminada, el modo de inicio de servicio del servicio KDS está configurado como Manual (inicio de desencadenador). Esta configuración significa que la primera vez que un cliente intenta usar el servicio, se inicia a petición. Este modo de inicio del servicio predeterminado es aceptable para que Protección de contraseñas de Azure AD funcione.

Si el modo de inicio del servicio KDS se ha configurado como Deshabilitado, esta configuración debe corregirse para que Protección de contraseñas de Azure AD funcione correctamente.

Una prueba sencilla para este problema consiste en iniciar manualmente el servicio KDS, ya sea a través de la consola MMC de administración de servicio, o con otras herramientas de administración de servicio (por ejemplo, ejecute "net start kdssvc" desde una consola de línea de comandos). Se espera que el servicio KDS se inicie correctamente y permanezca en ejecución.

La causa más común de que el servicio KDS no se pueda iniciar es que el objeto de controlador de dominio de Active Directory se encuentra fuera de la unidad organizativa controladores de dominio predeterminada. Esta configuración no es compatible con el servicio KDS y no es una limitación impuesta por Protección con contraseña de Azure AD. La solución para esta condición consiste en mover el objeto del controlador de dominio a una ubicación en la unidad organizativa de controladores de dominio predeterminada.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Las contraseñas débiles se aceptan, pero no debería ser así

Este problema puede tener varias causas.

1. Sus agentes de controlador de dominio no pueden descargar una directiva o no pueden descifrar las directivas existentes. Busque posibles causas en los temas anteriores.

1. El modo Exigir de la directiva de contraseñas sigue establecido en Auditoría. Si esta configuración está aplicada, vuelva a configurarla en Exigir con el portal de Protección de contraseña de Azure AD. Consulte [Habilitación de la protección con contraseña](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Se ha deshabilitado la directiva de contraseñas. Si esta configuración está aplicada, vuelva a configurarla en Habilitada con el portal de Protección de contraseña de Azure AD. Consulte [Habilitación de la protección con contraseña](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. No ha instalado el software del agente de controlador de dominio en todos los controladores de dominio en el dominio. En esta situación, es difícil garantizar que los clientes remotos de Windows se dirijan a un controlador de dominio determinado durante una operación de cambio de contraseña. Si cree que se ha dirigido correctamente a un controlador de dominio determinado en el que está instalado el software del agente de controlador de dominio, puede asegurarse de ello volviendo a comprobar el registro de eventos de administración del agente de controlador de dominio: independientemente del resultado, habrá al menos un evento para documentar el resultado de la validación de contraseña. Si no hay ningún evento presente para el usuario para el que se ha cambiado la contraseña, es probable que el cambio de contraseña lo procese otro controlador de dominio.

   Como una prueba alternativa, intente configurar/cambiar contraseñas al iniciar sesión directamente en un controlador de dominio donde esté instalado el software del agente de controlador de dominio. Esta técnica no es recomendable para los dominios de Active Directory de producción.

   Aunque se admite la implementación incremental del software del agente de controlador de dominio sujeta a estas limitaciones, Microsoft recomienda encarecidamente que se instale el software del agente de controlador de dominio en todos los controladores de dominio en un dominio tan pronto como sea posible.

1. Puede que el algoritmo de validación de contraseña funcione realmente según lo esperado. Consulte [Cómo se evalúan las contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Modo de reparación de servicios de directorio

Si el controlador de dominio se inicia en el modo de reparación de servicios de directorio, el servicio del agente de controlador de dominio detecta esta anomalía y hará que se deshabiliten todas las actividades de validación de contraseñas o de aplicación de estas, independientemente de la configuración de directivas activa actualmente.

## <a name="emergency-remediation"></a>Corrección de emergencia

Si se produce una situación en la que el servicio del agente de controlador de dominio causa problemas, este servicio se puede apagar inmediatamente. La DLL de filtro de contraseña del agente de controlador de dominio sigue intentando llamar al servicio que no está en ejecución, y registrará los eventos de advertencia (10012, 10013), pero durante ese tiempo se aceptan todas las contraseñas entrantes. El servicio de agente de controlador de dominio se puede también configurar mediante el Administrador de control de servicios de Windows con un tipo de inicio "Deshabilitado" según sea necesario.

Otra medida de corrección sería establecer el modo Habilitar en No en el portal de Protección de contraseña de Azure AD. Una vez que se ha descargado la directiva actualizada, cada servicio del agente de controlador de dominio entrará en un modo inactivo donde todas las contraseñas se aceptan tal cual. Para más información, consulte [Modo forzado](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Degradación del controlador de dominio

Se admite la degradación de un controlador de dominio en el que todavía se ejecuta el software del agente de controlador de dominio. No obstante, los administradores deben tener en cuenta que el software del agente de controlador de dominio sigue aplicando la directiva de contraseñas actual durante el procedimiento de degradación. La nueva contraseña de la cuenta de administrador local (que se especifica como parte de la operación de degradación) se valida como cualquier otra contraseña. Microsoft recomienda que se elijan contraseñas seguras para las cuentas de administrador locales como parte de un procedimiento de degradación del controlador de dominio. Sin embargo, la validación de la nueva contraseña de la cuenta de administrador local por parte del software del agente de controlador de dominio puede ser perjudicial para los procedimientos operativos de degradación que existían previamente.

Una vez que se ha realizado correctamente la degradación y el controlador de dominio se ha reiniciado y está de nuevo en ejecución como un servidor miembro normal, el software del agente de controlador de dominio se vuelve a ejecutar en modo pasivo. A partir de ese momento se puede desinstalar en cualquier momento.

## <a name="removal"></a>Eliminación

Realice los pasos siguientes si decide desinstalar el software de Protección de contraseñas de Azure AD y borrar todos los estados relacionados de los dominios y el bosque:

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

4. Quite manualmente todos los puntos de conexión del agente de controlador de dominio de cada contexto de nomenclatura de dominio. Puede haber uno estos objetos por cada controlador de dominio del bosque, dependiendo del grado de extensión de la implementación del software. La ubicación de ese objeto se puede detectar con el siguiente comando de Active Directory PowerShell:

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
