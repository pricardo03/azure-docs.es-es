---
title: Solución de problemas y registro en la versión preliminar de Protección con contraseña de Azure AD
description: Descripción del registro y la solución de los problemas más habituales de la versión preliminar de Protección con contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 1eea6380d4276644db0c7681f23a4b0c5e79ff09
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187356"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Versión preliminar: Supervisión, informes y solución de problemas de Protección con contraseña de Azure AD

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Después de la implementación de Protección con contraseña de Azure AD, la supervisión y la notificación son tareas fundamentales. En este artículo se detalla dónde registra cada servicio la información y cómo envía notificaciones sobre el uso de Protección con contraseña de Azure AD.

## <a name="on-premises-logs-and-events"></a>Registros y eventos locales

### <a name="dc-agent-service"></a>Servicio de agente de controlador de dominio

En cada controlador de dominio, el software del servicio de agente de controlador de dominio escribe los resultados de sus validaciones de contraseña (y otros estados) en un registro de eventos local: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

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

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 10014 de establecimiento correcto de contraseña

La contraseña modificada del usuario especificado se validó como compatible con la directiva de contraseñas de Azure actual.

 UserName: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 10017 y 30003 de error en el establecimiento de contraseña

10017:

Se rechazó la contraseña restablecida para el usuario especificado porque no cumplía con la directiva de contraseñas actual de Azure. Consulte el mensaje de registro de evento relacionado para más información.

 UserName: BPL_03283841185 FullName:

30003:

Se rechazó la contraseña restablecida para el usuario especificado porque coincidía con al menos uno de los tokens presentes en la lista de contraseñas prohibidas por inquilino de la directiva de contraseñas actual de Azure.

 UserName: BPL_03283841185 FullName:

Algunos otros mensajes importantes de registro de eventos a tener en cuenta son:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 30001

Se aceptó la contraseña para el usuario especificado porque no hay disponible aún una directiva de contraseñas de Azure

UserName: <user> FullName: <user>

Esta condición se puede deber a uno o varios de los siguientes motivos: %n

1. El bosque aún no se ha registrado con Azure.

   Pasos de la solución: un administrador debe registrar el bosque con el cmdlet Register-AzureADPasswordProtectionForest.

2. No hay todavía un proxy de Protección con contraseña de Azure AD disponible en al menos una máquina del bosque actual.

   Pasos de la solución: un administrador debe instalar y registrar un proxy con el cmdlet Register-AzureADPasswordProtectionProxy.

3. Este controlador de dominio no tiene conectividad de red con ninguna instancia de proxy de Protección con contraseña de Azure AD.

   Pasos de la solución: asegúrese de que haya conectividad de red con al menos una instancia de proxy de Protección con contraseña de Azure AD.

4. Este controlador de dominio no tiene conectividad con otros controladores del dominio.

   Pasos de la solución: asegúrese de que existe conectividad de red con el dominio.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Ejemplo de mensaje de registro de evento para el evento con identificador 30006

El servicio aplica ahora la siguiente directiva de contraseñas de Azure.

 AuditOnly: 1

 Fecha de la directiva global: 2018-05-15T00:00:00.000000000Z

 Fecha de la directiva de inquilino: 2018-06-10T20:15:24.432457600Z

 Aplicar directiva de inquilino: 1

#### <a name="dc-agent-log-locations"></a>Ubicaciones de registro de eventos del agente de controlador de dominio

El servicio de agente de controlador de dominio también registrará eventos relacionados con operaciones en el registro siguiente: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

El servicio de agente de controlador de dominio también puede registrar eventos detallados de seguimiento en el nivel de depuración en el siguiente registro: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

> [!WARNING]
> El registro de seguimiento está deshabilitado de manera predeterminada. Cuando se habilita, este registro recibe un gran volumen de eventos y esto puede afectar al rendimiento del controlador de dominio. Por tanto, este registro mejorado solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

### <a name="azure-ad-password-protection-proxy-service"></a>Servicio de proxy de Protección con contraseña de Azure AD

El servicio de proxy de Protección con contraseña emite un conjunto mínimo de eventos en el siguiente registro de eventos: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

El servicio de proxy de Protección con contraseña puede también registrar eventos detallados de seguimiento en el nivel de depuración en el siguiente registro: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> El registro de seguimiento está deshabilitado de manera predeterminada. Cuando se habilita, este registro recibe un gran volumen de eventos y esto puede afectar al rendimiento del host del proxy. Por tanto, este registro solo se debe habilitar si un problema requiere una investigación más profunda y solo durante un período mínimo de tiempo.

### <a name="dc-agent-discovery"></a>Detección del agente de controlador de dominio

Se puede usar el cmdlet `Get-AzureADPasswordProtectionDCAgent` para mostrar información básica sobre los diversos agentes de controlador de dominio que se están ejecutando en un dominio o bosque. Esta información se recupera de los objetos serviceConnectionPoint registrados por los servicios de agente de controlador de dominio en ejecución. A continuación se muestra un ejemplo de salida de este cmdlet:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Cada servicio de agente de controlador de dominio actualiza las diversas propiedades una vez cada hora aproximadamente. Los datos siguen estando sujetos a la latencia de replicación de Active Directory.

Se puede influenciar el ámbito de la consulta del cmdlet con los parámetros –Forest o –Domain.

### <a name="emergency-remediation"></a>Corrección de emergencia

Si se produce una situación desafortunada en la que el servicio del agente de controlador de dominio provoca problemas, este servicio se puede apagar inmediatamente. La DLL de filtro de contraseña del agente de controlador de dominio intenta llamar al servicio que no está en ejecución y registrará los eventos de advertencia (10012, 10013), pero se aceptan todas las contraseñas entrantes durante ese tiempo. El servicio de agente de controlador de dominio se puede también configurar mediante el Administrador de control de servicios de Windows con un tipo de inicio "Deshabilitado" según sea necesario.

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
> Es importante que realice estos pasos en orden. Si deja cualquier instancia del servicio de proxy de Protección con contraseña en ejecución, esta volverá a crear periódicamente su objeto serviceConnectionPoint así como el estado de Sysvol.

1. Desinstale el software del proxy de Protección con contraseña de todas las máquinas. En este paso **no** es necesario reiniciar.
2. Desinstale el software del agente de controlador de dominio de todos los controladores de dominio. En este paso es **necesario** reiniciar.
3. Quite manualmente todos los puntos de conexión del servicio de proxy en cada contexto de nomenclatura de dominio. La ubicación de estos objetos se puede detectar con el siguiente comando de Active Directory Powershell:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   No omita el asterisco ("*") al final del valor de variable $keywords.

   El objeto resultante que se encuentra a través del comando `Get-ADObject`, se puede canalizar hacia `Remove-ADObject`, o eliminar manualmente. 

4. Quite manualmente todos los puntos de conexión del agente de controlador de dominio de cada contexto de nomenclatura de dominio. Puede haber uno estos objetos por cada controlador de dominio del bosque, dependiendo del grado de extensión de la implementación de la versión preliminar del software. La ubicación de ese objeto se puede detectar con el siguiente comando de Active Directory Powershell:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   El objeto resultante que se encuentra a través del comando `Get-ADObject`, se puede canalizar hacia `Remove-ADObject`, o eliminar manualmente.

5. Quite manualmente el estado de configuración en el nivel de bosque. El estado de configuración del bosque se conserva en un contenedor en el contexto de nomenclatura de configuración de Active Directory. Se puede detectar y eliminar de la siguiente manera:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Quite todos los estados relacionados con Sysvol mediante la eliminación manual de la siguiente carpeta y de todo su contenido:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Si es necesario, también se puede acceder localmente a esta ruta en un determinado controlador de dominio. La ubicación predeterminada debe ser parecida a la siguiente:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Esta ruta de acceso será diferente si se ha configurado el recurso compartido de Sysvol en una ubicación distinta a la predeterminada.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las listas de contraseñas prohibidas globales y personalizadas, consulte el artículo [Ban bad passwords](concept-password-ban-bad.md) (Prohibición de contraseñas incorrectas).
