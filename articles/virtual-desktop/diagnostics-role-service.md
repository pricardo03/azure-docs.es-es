---
title: 'Identificar problemas con la característica de diagnósticos de Windows Vista previa del escritorio Virtual: Azure'
description: Describe la característica de diagnóstico de Windows Vista previa del escritorio Virtual y cómo usarlo.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 6b79a26d63c02dd06b62ea6ad09941f947704dc0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60870496"
---
# <a name="identify-issues-with-the-diagnostics-feature"></a>Identificación de problemas con la característica de diagnóstico

Vista previa de Escritorio Virtual de Windows ofrece una característica de diagnóstico que permite al administrador identificar los problemas a través de una única interfaz. Las funciones de Escritorio Virtual de Windows inicie una actividad de diagnóstico cada vez que un usuario interactúa con el sistema. Cada registro contiene información relevante, como los roles de Escritorio Virtual de Windows implicados en la transacción, los mensajes de error, información del inquilino y la información de usuario. Las actividades de diagnóstico se crean para el usuario final y las acciones administrativas y se pueden clasificar en tres depósitos principales:

* Fuente de actividades de suscripción: el usuario final estas actividades desencadena cada vez que intenten conectarse a su fuente a través de aplicaciones de escritorio remoto de Microsoft.
* Las actividades de conexión: el usuario final estas actividades desencadena cada vez que intenten conectarse a un escritorio o una conexión de RemoteApp a través de aplicaciones de escritorio remoto de Microsoft.
* Las actividades de administración: el Administrador de estas actividades desencadena cada vez que realizan operaciones de administración en el sistema, como la creación de grupos host, asignar usuarios a grupos de aplicaciones y crear asignaciones de roles.
  
Las conexiones que no llegan a Escritorio Virtual de Windows no aparecen en los resultados de diagnóstico porque el propio servicio de rol de diagnóstico es parte del escritorio Virtual de Windows. Problemas de conexión de Escritorio Virtual de Windows pueden ocurrir cuando el usuario final está experimentando problemas de conectividad de red.

Para empezar a trabajar, [descargar e importar el módulo de Windows PowerShell de Escritorio Virtual](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) usar en la sesión de PowerShell si no lo ha hecho ya.

## <a name="diagnose-issues-with-powershell"></a>Diagnosticar problemas con PowerShell

Diagnósticos de Escritorio Virtual de Windows usa un solo cmdlet de PowerShell, pero contiene muchos parámetros opcionales para ayudar a restringir y aislar problemas. Las secciones siguientes enumeran los cmdlets que puede ejecutar para diagnosticar problemas. Mayoría de los filtros se puede aplicar juntas. Valores que se muestran entre corchetes, como `<tenantName>`, se debe reemplazar con los valores que se aplican a su situación.

### <a name="retrieve-diagnostic-activities-in-your-tenant"></a>Recuperar las actividades de diagnóstico en el inquilino

Puede recuperar las actividades de diagnóstico mediante la especificación de la **Get RdsDiagnosticActivities** cmdlet. El siguiente cmdlet de ejemplo devolverá una lista de las actividades de diagnóstico, ordenados de más a menos reciente.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName>
```

Como otros cmdlets de PowerShell de Escritorio Virtual de Windows, debe usar el **TenantName -** parámetro para especificar el nombre del inquilino que desea usar para la consulta. El nombre del inquilino es aplicable para casi todas las consultas de actividad de diagnóstico.

### <a name="retrieve-detailed-diagnostic-activities"></a>Recuperar las actividades de diagnóstico detalladas

El **-Detailed** parámetro proporciona detalles adicionales para cada actividad de diagnóstico que se devuelve. El formato para cada actividad varía según el tipo de actividad. El **-detallada** parámetros pueden agregarse a cualquier **Get RdsDiagnosticActivities** de consulta, como se muestra en el ejemplo siguiente.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Detailed
```

### <a name="retrieve-a-specific-diagnostic-activity-by-activity-id"></a>Recuperar un identificador de actividad mediante la actividad de diagnóstico específico

El **- ActivityId** parámetro devuelve una determinada actividad de diagnóstico si existe, como se muestra en el siguiente cmdlet de ejemplo.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityId <ActivityIdGuid>
```

### <a name="filter-diagnostic-activities-by-user"></a>Filtrar las actividades de diagnóstico por usuario

El **- UserName** parámetro devuelve una lista de las actividades de diagnóstico iniciado por el usuario especificado, como se muestra en el siguiente cmdlet de ejemplo.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN>
```

El **- UserName** parámetro también se puede combinar con otros parámetros de filtrado opcionales.

### <a name="filter-diagnostic-activities-by-time"></a>Filtrar las actividades de diagnóstico por hora

Puede filtrar la lista devuelta de actividad de diagnóstico con la **- StartTime** y **- EndTime** parámetros. El **- StartTime** parámetro devolverá una lista de actividad de diagnóstico a partir de una fecha concreta, como se muestra en el ejemplo siguiente.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018"
```

El **- EndTime** parámetros pueden agregarse a un cmdlet con el **- StartTime** parámetro para especificar un período específico de tiempo que desea recibir los resultados de. El siguiente cmdlet de ejemplo devolverá una lista de las actividades de diagnóstico entre el 1 de agosto y el 10 de agosto.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018" -EndTime "08/10/2018"
```

El **- StartTime** y **- EndTime** parámetros también se pueden combinar con otros parámetros de filtrado opcionales.

### <a name="filter-diagnostic-activities-by-activity-type"></a>Filtrar las actividades por tipo de actividad de diagnóstico

También puede filtrar las actividades de diagnóstico por tipo de actividad con el **- ActivityType** parámetro. El cmdlet siguiente devolverá una lista de conexiones del usuario final:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Connection
```

El cmdlet siguiente devolverá una lista de tareas de administración de administrador:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Management
```

El **Get RdsDiagnosticActivities** cmdlet actualmente no admite la especificación de fuente como el ActivityType.

### <a name="filter-diagnostic-activities-by-outcome"></a>Filtrar las actividades de diagnóstico por resultado

Puede filtrar la lista de la actividad de diagnóstico devuelta por resultado con el **-resultado** parámetro. El siguiente cmdlet de ejemplo devolverá una lista de actividades correctas de diagnóstico.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Success
```

El siguiente cmdlet de ejemplo devolverá una lista de las actividades de diagnóstico con errores.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Failure
```

El **-resultado** parámetro también se puede combinar con otros parámetros de filtrado opcionales.

## <a name="common-error-scenarios"></a>Escenarios de error comunes

Escenarios de error se clasifican en interno al servicio y externas al escritorio Virtual de Windows.

* Problema interno: especifica los escenarios que no se pueden mitigar mediante el Administrador de inquilinos y deben resolverse como problema de soporte técnico. Se ha producido al generar un ticket de proporcionar el Id. de actividad, el nombre de inquilino y el período de tiempo aproximado el problema.
* Problema externo: se refieren a escenarios que se pueden mitigar mediante el administrador del sistema. Estos son externos al escritorio Virtual de Windows.

En la tabla siguiente se enumera los errores comunes que pueden surgir los administradores.

>[!NOTE]
>Esta versión preliminar no incluye una completa categorización de errores y se actualizará de forma periódica. Para asegurarse de que tiene la información más actualizada, asegúrese de comprobar en este artículo, al menos una vez al mes.

### <a name="external-management-error-codes"></a>Códigos de error de administración externo

|Código numérico|Código de error|Solución sugerida|
|---|---|---|
|3|UnauthorizedAccess|El usuario que intentó ejecutar el cmdlet de PowerShell administrativo no tiene permisos para hacerlo o escrito correctamente su nombre de usuario.|
|1000|TenantNotFound|El nombre del inquilino que escribió no coincide con los inquilinos existentes. Revise el nombre del inquilino errores tipográficos y vuelva a intentarlo.|
|1006|TenantCannotBeRemovedHasSessionHostPools|No se puede eliminar a un inquilino como long que contiene los objetos. Elimine primero los grupos de host de sesión y vuelva a intentarlo.|
|2000|HostPoolNotFound|El nombre del grupo host que especificó no coincide con los grupos de host existente. Revise el nombre del grupo host para los errores tipográficos y vuelva a intentarlo.|
|2005|HostPoolCannotBeRemovedHasApplicationGroups|No se puede eliminar un grupo host siempre que lo contiene objetos. Quite primero todos los grupos de aplicaciones en el grupo host.|
|2004|HostPoolCannotBeRemovedHasSessionHosts|Quitar todos los hosts de las sesiones antes eliminando primero el grupo de host de sesión.|
|5001|SessionHostNotFound|Ha consultado el host de sesión puede estar sin conexión. Compruebe el estado del grupo host.|
|5008|SessionHostUserSessionsExist |Debe iniciar sesión de todos los usuarios en el host de sesión antes de ejecutar la actividad de administración deseados.|
|6000|AppGroupNotFound|El nombre del grupo de aplicación que escribió no coincide con los grupos de aplicaciones existente. Revise el nombre del grupo de aplicación si hay errores ortográficos y vuelva a intentarlo.|
|6022|RemoteAppNotFound|El nombre de conexión de RemoteApp que escribió no coincide con cualquier RemoteApps. Revise el nombre de conexión de RemoteApp para errores tipográficos y vuelva a intentarlo.|
|6010|PublishedItemsExist|El nombre del recurso que está intentando publicar es igual a un recurso que ya existe. Cambiar el nombre del recurso y vuelva a intentarlo.|
|7002|NameNotValidWhiteSpace|No utilice espacios en blanco en el nombre.|
|8000|InvalidAuthorizationRoleScope|El nombre de rol especificado no coincide con los nombres de función existente. Revise el nombre del rol si hay errores ortográficos y vuelva a intentarlo. |
|8001|UserNotFound |El nombre de usuario que escribió no coincide con los nombres de usuario existente. Revise el nombre si hay errores ortográficos y vuelva a intentarlo.|
|8005|UserNotFoundInAAD |El nombre de usuario que escribió no coincide con los nombres de usuario existente. Revise el nombre si hay errores ortográficos y vuelva a intentarlo.|
|8008|TenantConsentRequired|Siga las instrucciones [aquí](tenant-setup-azure-active-directory.md#grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service) para dar su consentimiento para el inquilino.|

### <a name="external-connection-error-codes"></a>Códigos de error de conexión externa

|Código numérico|Código de error|Solución sugerida|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|El host de sesión no está unido correctamente a Active Directory.|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|Las conexiones no pudieron porque el host de sesión no está disponible. Compruebe el estado del host de sesión.|
|-2146233088|ConnectionFailedClientDisconnect|Si ve este error con frecuencia, asegúrese de que el equipo del usuario está conectado a la red.|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|La sesión a que del usuario de host intentó conectarse no es correcta. Depuración de la máquina virtual.|
|-2146233088|ConnectionFailedUserNotAuthorized|El usuario no tiene permiso para acceder a la aplicación publicada o el escritorio. El error puede aparecer después de que el administrador quita los recursos publicados. Pida al usuario que actualice la fuente en la aplicación de escritorio remoto.|
|2|FileNotFound|La aplicación que el usuario intentó tener acceso incorrectamente está instalada o está establecida en una ruta de acceso incorrecta.|
|3|InvalidCredentials|El nombre de usuario o contraseña especificados por el usuario no coincide con los nombres de usuario o contraseñas existente. Revise las credenciales si hay errores ortográficos y vuelva a intentarlo.|
|8|ConnectionBroken|Interrumpe la conexión entre el cliente y la puerta de enlace o servidor. Ninguna acción necesaria a menos que ocurre de forma inesperada.|
|14|UnexpectedNetworkDisconnect|Interrumpe la conexión a la red. Pida al usuario conectarse de nuevo.|
|24|ReverseConnectFailed|La máquina virtual host no tiene ninguna línea de visión directa a la puerta de enlace de escritorio remoto. Asegúrese de que se puede resolver la dirección IP de puerta de enlace.|

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los roles dentro de Escritorio Virtual de Windows, consulte [entorno de Windows Vista previa del escritorio Virtual](environment-setup.md).

Para ver una lista de cmdlets de PowerShell disponibles para el escritorio Virtual de Windows, consulte el [referencia de PowerShell](/powershell/windows-virtual-desktop/overview).
