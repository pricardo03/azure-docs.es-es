---
title: Solución de problemas y registro en la versión preliminar de Protección con contraseña de Azure AD
description: Descripción del registro y la solución de los problemas más habituales de la versión preliminar de Protección con contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 55139d7270d602d73605c484eeace7012ff3c1b8
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076084"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Vista previa: supervisión, informes y solución de problemas de Protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Una vez realizada la implementación de Protección con contraseña de Azure AD, la supervisión y la notificación son tareas fundamentales. En este artículo se detalla dónde registra cada servicio la información y cómo envía notificaciones sobre el uso de Protección con contraseña de Azure AD.

## <a name="on-premises-logs-and-events"></a>Registros y eventos locales

### <a name="dc-agent-admin-log"></a>Registro de administración del agente de controlador de domino

En cada controlador de dominio, el software del servicio del agente de controlador de dominio escribe los resultados de sus validaciones de contraseña (y otros estados) en un registro de eventos local:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

Los diversos componentes del agente de controlador de dominio registran los eventos mediante los siguientes intervalos:

|Componente |Intervalo de identificadores de evento|
| --- | --- |
|DLL de filtro de contraseña del agente de controlador de dominio| 10000-19999|
|Proceso de hospedaje del servicio de agente de controlador de dominio| 20000-29999|
|Lógica de validación de directivas de servicio del agente de controlador de dominio| 30000-39999|

Para una operación de validación de contraseñas correcta se registra, por lo general, un evento desde la DLL de filtro de contraseña del agente de controlador de dominio. Si se producen errores durante la validación, se registrarán, por lo general, dos eventos, uno desde el servicio de agente de controlador de dominio y otro desde la DLL de filtro de contraseña de este agente.

Se registran eventos discretos para capturar estas situaciones en función de los siguientes factores:

* Si se está estableciendo o cambiando una contraseña determinada.
* Si la validación de una contraseña se ha realizado correctamente o se han producido errores.
* Si se produjo un error durante la validación debido al uso de la directiva global de Microsoft en lugar de la directiva de la organización.
* Si el modo de solo auditoría está actualmente activado o desactivado para la directiva de contraseñas actual.

Los principales eventos relacionados con la validación de contraseñas son los siguientes:

|   |Cambio de contraseña |Establecimiento de contraseña|
| --- | :---: | :---: |
|Pass (pasado) |10014 |10015|
|Error (no superó la directiva de contraseñas del cliente)| 10016, 30002| 10017, 30003|
|Error (no superó la directiva de contraseñas de Microsoft)| 10016, 30004| 10017, 30005|
|Superó la fase de solo auditoría (la directiva de contraseñas del cliente habría generado errores)| 10024, 30008| 10025, 30007|
|Superó la fase de solo auditoría (la directiva de contraseñas de Microsoft habría generado errores)| 10024, 30010| 10025, 30009|

> [!TIP]
> Las contraseñas de entrada se validan primero en relación con la lista de contraseñas global de Microsoft. Si ahí se producen errores, no se realiza ningún otro proceso. Este comportamiento es el mismo que se realizó en los cambios de contraseña de Azure.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 10014 (establecimiento correcto de contraseña)

```
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 10017 y 30003 (error en el establecimiento de contraseña)

10017:

```
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 30001 (contraseña aceptada ya que no hay ninguna directiva disponible)

```
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

#### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 30006 (nueva directiva aplicada)

```
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

#### <a name="dc-agent-operational-log"></a>Registro operativo del agente de controlador de dominio

El servicio del agente de controlador de dominio también registrará los eventos relacionados con operaciones en el registro siguiente:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

#### <a name="dc-agent-trace-log"></a>Registro de seguimiento del agente de controlador de dominio

El servicio del agente de controlador de dominio también puede registrar eventos de seguimiento de nivel de depuración detallado en el registro siguiente:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

El registro de seguimiento está deshabilitado de manera predeterminada.

> [!WARNING]
>  Cuando se habilita, el registro de seguimiento recibe un gran volumen de eventos y esto puede afectar al rendimiento del controlador de dominio. Por tanto, este registro mejorado solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

#### <a name="dc-agent-text-logging"></a>Registro de texto del agente de controlador de dominio

El servicio del agente de controlador de dominio puede configurarse para escribir en un registro de texto estableciendo el valor de registro siguiente:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)

El registro de texto está deshabilitado de manera predeterminada. El reinicio del servicio del agente de controlador de dominio es necesario para que los cambios realizados en este valor surtan efecto. Cuando se habilita, el servicio del agente de controlador de dominio escribirá en un archivo de registro ubicado en:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> El registro de texto recibe las mismas entradas de nivel de depuración que se pueden registrar en el registro de seguimiento, pero suelen tener un formato más fácil para revisar y analizar.

> [!WARNING]
> Cuando se habilita, este registro recibe un gran volumen de eventos y esto puede afectar al rendimiento del controlador de dominio. Por tanto, este registro mejorado solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

### <a name="azure-ad-password-protection-proxy-service"></a>Servicio de proxy de Protección con contraseña de Azure AD

#### <a name="proxy-service-event-logs"></a>Registros de eventos del servicio de proxy

El servicio de proxy emite un conjunto mínimo de eventos para los registros de eventos siguientes:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

El servicio de proxy también puede registrar eventos de seguimiento de nivel de depuración detallado en el registro siguiente:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

El registro de seguimiento está deshabilitado de manera predeterminada.

> [!WARNING]
> Cuando se habilita, el registro de seguimiento recibe un gran volumen de eventos y esto puede afectar al rendimiento del host del proxy. Por tanto, este registro solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

#### <a name="proxy-service-text-logging"></a>Registro de texto del servicio de proxy

El servicio de proxy puede configurarse para escribir en un registro de texto estableciendo el valor de registro siguiente:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters!EnableTextLogging = 1 (REG_DWORD value)

El registro de texto está deshabilitado de manera predeterminada. El reinicio del servicio de proxy es necesario para que los cambios realizados en este valor surtan efecto. Cuando se habilita, el servicio de proxy escribirá en un archivo de registro ubicado en:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> El registro de texto recibe las mismas entradas de nivel de depuración que se pueden registrar en el registro de seguimiento, pero suelen tener un formato más fácil para revisar y analizar.

> [!WARNING]
> Cuando se habilita, este registro recibe un gran volumen de eventos y esto puede afectar al rendimiento del controlador de dominio. Por tanto, este registro mejorado solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

#### <a name="powershell-cmdlet-logging"></a>Registro del cmdlet de PowerShell

La mayoría de los cmdlets de Powershell de la protección de contraseñas de Azure AD escribirán en un registro de texto que se encuentra en:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Si se produce un error de cmdlet y la causa o la solución no son evidentes, también se pueden consultar estos registros de texto.

### <a name="emergency-remediation"></a>Corrección de emergencia

Si se produce una situación en la que el servicio del agente de controlador de dominio causa problemas, este servicio se puede apagar inmediatamente. La DLL de filtro de contraseña del agente de controlador de dominio sigue intentando llamar al servicio que no está en ejecución, y registrará los eventos de advertencia (10012, 10013), pero durante ese tiempo se aceptan todas las contraseñas entrantes. El servicio de agente de controlador de dominio se puede también configurar mediante el Administrador de control de servicios de Windows con un tipo de inicio "Deshabilitado" según sea necesario.

### <a name="performance-monitoring"></a>Supervisión del rendimiento

El software del servicio de agente de controlador de dominio instala un objeto de contador de rendimiento denominado **Protección con contraseña de Azure AD**. Los siguientes contadores de rendimiento están disponibles actualmente:

|Nombre del contador de rendimiento | DESCRIPCIÓN|
| --- | --- |
|Contraseñas procesadas |Este contador muestra el número total de contraseñas que se procesan (aceptadas o rechazadas) desde el último reinicio.|
|Contraseñas aceptadas |Este contador muestra el número total de contraseñas que se aceptaron desde el último reinicio.|
|Contraseñas rechazadas |Este contador muestra el número total de contraseñas que se rechazaron desde el último reinicio.|
|Solicitudes de filtro de contraseñas en curso |Este contador muestra el número de solicitudes de filtro de contraseñas actualmente en curso.|
|Máximo de solicitudes de filtro de contraseñas |Este contador muestra el número máximo de solicitudes de filtro de contraseña simultáneas desde el último reinicio.|
|Errores de solicitudes de filtro de contraseñas |Este contador muestra el número total de solicitudes de filtro de contraseñas en las que se produjo algún error desde el último reinicio. Se pueden producir errores si el servicio de agente de controlador de dominio de Protección con contraseña de Azure AD no se ejecuta.|
|Solicitudes de filtro de contraseñas por segundo |Este contador muestra la velocidad a la que se procesan las contraseñas.|
|Tiempo de procesamiento de la solicitud de filtro de contraseñas |Este contador muestra el tiempo promedio necesario para procesar una solicitud de filtro de contraseñas.|
|Tiempo máximo de procesamiento de la solicitud de filtro de contraseñas |Este contador muestra el tiempo máximo de procesamiento de solicitudes de filtro de contraseñas desde el último reinicio.|
|Contraseñas que se aceptan debido al modo de auditoría |Este contador muestra el número total de contraseñas que normalmente se habrían rechazado, pero que se aceptaron porque la directiva de contraseñas se configuró para estar en modo de auditoría (desde el último reinicio).|

## <a name="directory-services-repair-mode"></a>Modo de reparación de servicios de directorio

Si el controlador de dominio se inicia en el modo de reparación de servicios de directorio, el servicio del agente de controlador de dominio detecta esta situación y hace que se deshabiliten todas las actividades de validación de contraseñas o de aplicación de estas, independientemente de la configuración de directivas activa actualmente.

## <a name="domain-controller-demotion"></a>Degradación del controlador de dominio

Se admite la degradación de un controlador de dominio en el que todavía se ejecuta el software del agente de controlador de dominio. Los administradores deben tener en cuenta sin embargo que el software del agente de controlador de dominio sigue ejecutándose y continúa aplicando la directiva de contraseñas actual durante el proceso de degradación. La nueva contraseña de la cuenta de administrador local (que se especifica como parte de la operación de degradación) se valida como cualquier otra contraseña. Microsoft recomienda que se elijan contraseñas seguras para las cuentas de administrador locales como parte de un procedimiento de degradación del controlador de dominio. Sin embargo, la validación de la nueva contraseña de la cuenta de administrador local por parte del software del agente de controlador de dominio puede ser perjudicial para los procedimientos operativos de degradación que existían previamente.

Una vez que se ha realizado correctamente la degradación y el controlador de dominio se ha reiniciado y está de nuevo en ejecución como un servidor miembro normal, el software del agente de controlador de dominio se vuelve a ejecutar en modo pasivo. A partir de ese momento se puede desinstalar en cualquier momento.

## <a name="removal"></a>Eliminación

Si decide desinstalar la versión preliminar pública del software y limpiar todos los estados relacionados de los dominios y el bosque, puede hacerlo mediante los pasos siguientes:

> [!IMPORTANT]
> Es importante que realice estos pasos en orden. Si se deja cualquier instancia del servicio de proxy en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint. Si se deja cualquier instancia del servicio del agente de controlador de dominio en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint y el estado sysvol.

1. Desinstale el software del proxy de Protección con contraseña de todas las máquinas. En este paso **no** es necesario reiniciar.
2. Desinstale el software del agente de controlador de dominio de todos los controladores de dominio. En este paso es **necesario** reiniciar.
3. Quite manualmente todos los puntos de conexión del servicio de proxy en cada contexto de nomenclatura de dominio. La ubicación de estos objetos se puede detectar con el siguiente comando de Active Directory Powershell:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   No omita el asterisco ("*") al final del valor de variable $keywords.

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente. 

4. Quite manualmente todos los puntos de conexión del agente de controlador de dominio de cada contexto de nomenclatura de dominio. Puede haber uno estos objetos por cada controlador de dominio del bosque, dependiendo del grado de extensión de la implementación de la versión preliminar del software. La ubicación de ese objeto se puede detectar con el siguiente comando de Active Directory Powershell:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   El objeto u objetos resultantes que se encuentran a través del comando `Get-ADObject`, se pueden canalizar hacia `Remove-ADObject`, o eliminar manualmente.

5. Quite manualmente el estado de configuración en el nivel de bosque. El estado de configuración del bosque se conserva en un contenedor en el contexto de nomenclatura de configuración de Active Directory. Se puede detectar y eliminar de la siguiente manera:

   ```Powershell
   $passwordProtectonConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectonConfigContainer
   ```

6. Quite todos los estados relacionados con Sysvol mediante la eliminación manual de la siguiente carpeta y de todo su contenido:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Si es necesario, también se puede acceder localmente a esta ruta en un determinado controlador de dominio. La ubicación predeterminada debe ser parecida a la siguiente:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Esta ruta de acceso será diferente si se ha configurado el recurso compartido de Sysvol en una ubicación distinta a la predeterminada.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las listas de contraseñas prohibidas globales y personalizadas, consulte el artículo [Ban bad passwords](concept-password-ban-bad.md) (Prohibición de contraseñas incorrectas).
