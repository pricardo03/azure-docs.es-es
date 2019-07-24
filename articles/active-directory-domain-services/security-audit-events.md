---
title: Habilitación de auditorías de seguridad para Azure AD Domain Services | Microsoft Docs
description: Habilitación de auditorías de seguridad para Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: 3105296b3c670d3d44789c93878fa1fc6076973b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566707"
---
# <a name="enable-security-audits-for-azure-ad-domain-services-preview"></a>Habilitación de auditorías de seguridad para Azure AD Domain Services (versión preliminar)
La auditoría de seguridad de Azure AD Domain Services permite a los clientes usar el portal de Azure AD Domain Services para transmitir los eventos de auditoría de seguridad a los recursos de destino. Los recursos que pueden recibir estos eventos incluyen Azure Storage, las áreas de trabajo de Azure Log Analytics o Azure Event Hubs. Poco después de habilitar los eventos de auditoría de seguridad, Azure AD Domain Services envía todos los eventos auditados para la categoría seleccionada al recurso de destino. Los eventos de auditoría de seguridad permiten a los clientes archivar eventos auditados en Azure Storage. Además, los clientes pueden transmitir eventos a software de administración de eventos e información de seguridad (SIEM) (o equivalente) mediante centros de eventos, o realizar sus propios análisis y conclusiones con Azure Log Analytics en Azure Portal. 

> [!IMPORTANT]
> La auditoría de seguridad de Azure AD Domain Services está disponible solo en instancias basadas en Azure Resource Manager para Azure AD Domain Services.
>
>

## <a name="auditing-event-categories"></a>Categorías de eventos de auditoría
La auditoría de seguridad de Azure AD Domain Services se alinea con la auditoría tradicional que se incluye para los controladores de dominio de Active Directory Domain Services. La reutilización de los patrones de auditoría existentes garantiza que se pueda usar la misma lógica al analizar los eventos. La auditoría de seguridad de Azure AD Domain Services incluye las siguientes categorías de eventos.

| Nombre de categoría de auditoría | DESCRIPCIÓN |
|:---|:---|
| Inicio de sesión de la cuenta|Audita los intentos para autenticar los datos de la cuenta en un controlador de dominio o en un administrador de cuentas de seguridad (SAM) local.</p>Configuración de la directiva de inicio y cierre de sesión e intentos de seguimiento de eventos para acceder a un equipo determinado. La configuración y los eventos de esta categoría se centran en la base de datos de cuenta que se está utilizando. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar validación de credenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Auditar servicio de autenticación Kerberos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Auditar operaciones de vales de servicio Kerberos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Auditar otros eventos de inicio y cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Administración de cuentas|Audita los cambios en grupos y cuentas de usuarios y equipos. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar administración de grupos de aplicaciones](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Auditar administración de cuentas de equipo](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Auditar administración de grupos de distribución](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Auditar otros eventos de administración de cuentas](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Auditar administración de grupos de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Auditar administración de cuentas de usuario](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| Seguimiento de detalles|Audita las actividades de aplicaciones individuales y usuarios de ese equipo, y ayuda a entender cómo se usa un equipo. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar actividad DPAPI](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[Auditar actividad PNP](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[Auditar creación de procesos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[Auditar finalización de procesos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[Auditar eventos de RPC](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Acceso a Directory Services|Audita los intentos para acceder y modificar objetos en Active Directory Domain Services (AD DS). Estos eventos de auditoría se registran solo en controladores de dominio. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar replicación de servicio de directorio detallada](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Auditar el acceso del servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Auditar cambios de servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Auditar replicación de servicio de directorio](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Cierre de sesión-Inicio de sesión|Audita los intentos de iniciar sesión en un equipo de forma interactiva o a través de una red. Estos eventos son útiles para el seguimiento de la actividad de usuario y la identificación de posibles ataques contra los recursos de red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar bloqueo de cuentas](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Auditar reclamaciones de usuario o dispositivo](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[Auditar modo extendido de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Auditar pertenencia a grupos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[Auditar modo principal de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[Auditar modo rápido de IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Auditar cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Auditar inicio de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Auditar servidor de directivas de redes](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Auditar otros eventos de inicio y cierre de sesión](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Auditar inicio de sesión especial](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Acceso a objetos| Audita los intentos de acceso a determinados objetos o tipos de objetos en un equipo o red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar aplicación generada](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Auditar servicios de certificación](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Auditar recurso compartido de archivos detallado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Auditar recurso compartido de archivos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Auditar sistema de archivos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Auditar conexión de plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Auditar colocación de paquetes de la plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Auditar manipulación de identificadores](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Auditar objeto de kernel](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Auditar otros eventos de acceso a objetos](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Auditar Registro](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Auditar almacenamiento extraíble](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[Auditar SAM](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Auditar almacenamiento provisional de directiva de acceso central](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|Cambio de directiva|Audita los cambios en directivas de seguridad importantes en una red o sistema local. Los administradores suelen ser los responsables de establecer las directivas para ayudar a proteger los recursos de red. La supervisión de los cambios o intentos para cambiar estas directivas puede ser un aspecto importante de la administración de seguridad para una red. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar cambio de directiva de auditoría](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Auditar cambio de directiva de autenticación](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Auditar cambio de directiva de autorización](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Auditar cambio de directiva de la plataforma de filtrado](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[Auditar cambio de directiva del nivel de reglas de MPSSVC](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Auditar otros eventos de cambio de directiva](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Uso de privilegios| Audita el uso de determinados permisos en uno o varios sistemas. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar uso de privilegios no confidenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Auditar uso de privilegios confidenciales](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Auditar otros eventos de uso de privilegios](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|Sistema| Audita los cambios del nivel de sistema en un equipo no incluido en otras categorías y que tienen posibles implicaciones de seguridad. Esta categoría incluye las subcategorías siguientes:<ul><li>[Auditar controlador IPsec](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Auditar otros eventos del sistema](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Auditar cambio de estado de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Auditar extensión del sistema de seguridad](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Auditar integridad del sistema](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Identificadores de eventos por categoría
 La auditoría de seguridad de Azure AD Domain Services registra los siguientes identificadores de eventos cuando la acción específica desencadena un evento que se puede auditar.

| Nombre de categoría de eventos | Id. de evento |
|:---|:---|
|Seguridad de inicio de sesión de la cuenta|4767, 4774, 4775, 4776, 4777|
|Seguridad de administración de la cuenta|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Seguridad de seguimiento de detalles|None|
|Seguridad de acceso de DS|5136, 5137, 5138, 5139, 5141|
|Seguridad de cierre de sesión-inicio de sesión|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Seguridad de acceso de objeto|None|
|Seguridad de cambio de directiva|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Seguridad de uso de privilegios|4985|
|Seguridad del sistema|4612, 4621|

## <a name="enable-security-audit-events"></a>Habilitar eventos de auditoría de seguridad
Las siguientes instrucciones le ayudan a suscribirse correctamente a eventos de auditoría de seguridad de Azure AD Domain Services.

> [!IMPORTANT]
> Las auditorías de seguridad de Azure AD Domain Services no son retroactivas. No es posible recuperar eventos del pasado o reproducir eventos del pasado. El servicio solo puede enviar los eventos que se producen después de habilitarlo.
>

### <a name="choose-the-target-resource"></a>Elección del recurso de destino
Puede usar cualquier combinación de Azure Storage, Azure Event Hubs o áreas de trabajo de Azure Log Analytics como recurso de destino para sus auditorías de seguridad. Tenga en cuenta la tabla siguiente para elegir el mejor recurso para su caso de uso.

> [!IMPORTANT]
> Debe crear el recurso de destino antes de habilitar las auditorías de seguridad de Azure AD Domain Services.
>

| Recurso de destino | Escenario |
|:---|:---|
|Azure Storage|Considere la posibilidad de utilizar este destino cuando el requisito principal sea almacenar eventos de auditoría de seguridad para fines de archivado. Se pueden utilizar otros destinos para fines de archivado; sin embargo, esos destinos proporcionan funcionalidades más allá de la principal necesidad de archivado. Para crear una cuenta de Azure Storage, consulte [Crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal#create-a-storage-account-1).|
|Azure Event Hubs|Considere la posibilidad de utilizar este destino cuando la principal necesidad sea compartir eventos de auditoría de seguridad con software adicional, como software de análisis de datos o software de administración de eventos e información de seguridad (SIEM). Para crear un centro de eventos, consulte [Inicio rápido: Creación de un centro de eventos mediante Azure Portal](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).|
|Área de trabajo de Azure Log Analytics|Considere la posibilidad de utilizar este destino cuando su principal necesidad consiste en analizar y revisar auditorías seguras directamente de Azure Portal.  Para crear un área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).|

## <a name="using-the-azure-portal-to-enable-security-audit-events"></a>Uso de Azure Portal para habilitar eventos de auditoría de seguridad 
1. Inicie sesión en Azure Portal en https://portal.azure.com.  En Azure Portal, haga clic en Todos los servicios. En la lista de recursos, escriba **Dominio**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Haga clic en **Azure AD Domain Services**.
2. Haga clic en la instancia de Azure AD Domain Services de la lista.
3. Haga clic en **Diagnostic settings (preview)** [Configuración de diagnóstico (versión preliminar)] en la lista de acciones de la izquierda.</p>
![acción de configuración de diagnóstico](./media/security-audit-events/diagnostic-settings-action.png)
4. Escriba el nombre de la configuración de diagnóstico (**aadds-auditing**, por ejemplo).</p>
![página de configuración de diagnóstico](./media/security-audit-events/diagnostic-settings-page.png)
5. Seleccione la casilla adecuada situada junto a los recursos de destino que va a utilizar con los eventos de auditoría de seguridad.
    > [!NOTE]
    > No puede crear recursos de destino en esta página.
    >
    
    **Azure Storage**:</p>
    Seleccione **Archivar en una cuenta de almacenamiento**. Haga clic en **Configurar**. Seleccione la **suscripción** y la **cuenta de almacenamiento** que desee usar para archivar eventos de auditoría de seguridad. Haga clic en **OK**.</p>
    
    ![configuración de almacenamiento de diagnósticos](./media/security-audit-events/diag-settings-storage.png)
    
    **Azure Event Hubs**:</p>
    Seleccione **Transmitir a un centro de eventos**. Haga clic en **Configurar**. En la página **Seleccionar centro de eventos**, seleccione la **suscripción** que se ha utilizado para crear el centro de eventos. A continuación, seleccione el **espacio de nombres del centro de eventos**, el **nombre del centro de eventos** y el **nombre de directiva del centro de eventos**. Haga clic en **OK**.</p>
    ![configuración del centro de eventos de diagnóstico](./media/security-audit-events/diag-settings-eventhub.png)
    
    **Áreas de trabajo de Azure Log Analytics**:</p>
    Seleccione **Enviar a Log Analytics**. Seleccione la **suscripción** y el **área de trabajo de Log Analytics** que se han utilizado para almacenar eventos de auditoría de seguridad.</p>
    ![configuración de área de trabajo de diagnóstico](./media/security-audit-events/diag-settings-log-analytics.png)

6. Seleccione las categorías de registro que desee incluir para el recurso de destino determinado. Si usa cuentas de almacenamiento, puede configurar directivas de retención.

    > [!NOTE]
    > Puede seleccionar diferentes categorías de registro para cada recurso de destino dentro de una única configuración. Esto le permite elegir qué categorías de registros desea mantener para Log Analytics y qué categorías de registros desea archivar.
    >

7. Haga clic en **Save** (Guardar) para confirmar los cambios. Los recursos de destino recibirán eventos de auditoría de seguridad de Azure AD Domain Services poco después de guardar la configuración.

## <a name="using-azure-powershell-to-enable-security-audit-events"></a>Uso de Azure PowerShell para habilitar eventos de auditoría de seguridad
 
### <a name="prerequisites"></a>Requisitos previos

Siga las instrucciones que aparecen en el artículo para [instalar el módulo de Azure PowerShell y conectarse a la suscripción de Azure](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="enable-security-audits"></a>Habilitación de Security Center

1. Autentíquese en Azure Resource Manager para el suscriptor y la suscripción adecuados con el cmdlet **Connect-AzAccount** de Azure PowerShell.
2. Cree el recurso de destino para los eventos de auditoría de seguridad.</p>
    **Azure Storage**:</p>
    Para crear una cuenta de almacenamiento, consulte [Crear una cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-powershell).</p>
    **Azure Event Hubs**:</p>
    Consulte [Inicio rápido: Creación de un centro de eventos mediante Azure PowerShell](https://docs.microsoft.com/azure/event-hubs/event-hubs-quickstart-powershell). También es posible que deba usar el cmdlet [New-AzEventHubAuthorizationRule](https://docs.microsoft.com/powershell/module/az.eventhub/new-azeventhubauthorizationrule?view=azps-2.3.2) de Azure PowerShell para crear una regla de autorización que permita los permisos de Active Directory AD Domain Services para el **espacio de nombres** del centro de eventos. La regla de autorización debe incluir los derechos para **Administrar**, **Escuchar** y **Enviar**.
    > [!IMPORTANT]
    > Asegúrese de establecer la regla de autorización en el espacio de nombres del centro de eventos y no en el centro de eventos.
       
    </p>
    
    **Áreas de trabajo de Azure Log Analytics**:</p>
    Consulte [Creación de un área de trabajo de Log Analytics con Azure PowerShell](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace-posh) para crear el área de trabajo.
3. Obtenga el identificador de recurso para la instancia de Azure AD Domain Services. En una consola de Windows PowerShell abierta, autenticada, escriba el siguiente comando. Use la variable **$aadds.ResourceId** como parámetro para el identificador de recurso de Azure AD Domain Services para futuras cmdlets.
    ```powershell
    $aadds = Get-AzResource -name aaddsDomainName
    ``` 
4. Utilice el cmdlet **Set-AzDiagnosticSetting** para definir la configuración de Azure Diagnostics y que utilice el recurso de destino para los eventos de auditoría de seguridad de Azure AD Domain Services. En los ejemplos siguientes, la variable $aadds.ResourceId representa el identificador de recurso de la instancia de Azure AD Domain Services (consulte el paso 3).</p>
    **Azure Storage**:
    ```powershell
    Set-AzDiagnosticSetting `
    -ResourceId $aadds.ResourceId` 
    -StorageAccountId storageAccountId `
    -Enabled $true
    ```
    Reemplace *storageAccountId* por el identificador de su cuenta de almacenamiento.</p>
    
    **Azure Event Hubs**:
    ```powershell
    Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -EventHubName eventHubName `
    -EventHubAuthorizationRuleId eventHubRuleId `
    -Enabled $true
    ```
    Reemplace *eventHubName* por el nombre de su centro de eventos. Reemplace *eventHubRuleId* por su identificador de regla de autorización que creó anteriormente.</p>
    
    **Áreas de trabajo de Azure Log Analytics**:
    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -WorkspaceID workspaceId `
    -Enabled $true
    ```
    Reemplace *workspaceId* por el identificador del área de trabajo de Log Analytics que creó anteriormente. 

## <a name="view-security-audit-events-using-azure-monitor"></a>Vista de eventos de auditoría de seguridad con Azure Monitor
Las áreas de trabajo de Log Analytics permiten ver y analizar los eventos de auditoría de seguridad mediante Azure Monitor y el lenguaje de consulta Kusto. El lenguaje de consulta está diseñado para un uso de solo lectura que ofrece funcionalidades analíticas de energía con una sintaxis fácil de leer.
Estos son algunos recursos para ayudarle a comenzar con los lenguajes de consulta Kusto.
* [Documentación sobre Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)
* [Introducción a los análisis de registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)
* [Introducción a las consultas de registro en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)
* [Creación y uso compartido de paneles de datos de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards)

## <a name="sample-queries"></a>Consultas de ejemplo

### <a name="sample-query-1"></a>Consulta de ejemplo 1
Todos los eventos de bloqueo de cuenta de los últimos siete días.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Consulta de ejemplo 2
Todos los eventos de bloqueo de cuenta (4740) entre el 26 de junio de 2019 a las 9:00. y el 1 de julio de 2019 a medianoche, ordenados en orden ascendente por fecha y hora.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2019-06-26 09:00) and TimeGenerated <= datetime(2019-07-01) 
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Consulta de ejemplo 3
Eventos de inicio de sesión de cuenta de hace siete días (desde ahora) para la cuenta de usuario designada.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Consulta de ejemplo 4
Eventos de inicio de sesión de cuenta de hace siete días desde ahora para el usuario con nombre de la cuenta que intentó iniciar sesión con una contraseña incorrecta (0xC0000006a).
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Consulta de ejemplo 5
Eventos de inicio de sesión de cuenta de hace siete días desde ahora para el usuario con nombre de la cuenta que intentó iniciar sesión mientras la cuenta estaba bloqueada (0xC0000234).
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Consulta de ejemplo 6
El número de eventos de inicio de sesión de cuenta de hace siete días desde ahora para todos los intentos de inicio de sesión que se han producido para todos los usuarios bloqueados.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="related-content"></a>Contenido relacionado
* [Introducción](https://docs.microsoft.com/azure/kusto/query/) al lenguaje de consulta Kusto.
* [Tutorial de Kusto](https://docs.microsoft.com/azure/kusto/query/tutorial) para familiarizarse con los conceptos básicos de consultas.
* [Consultas de ejemplo](https://docs.microsoft.com/azure/kusto/query/samples) que le ayudan a aprender nuevas maneras de ver los datos.
* [Procedimientos recomendados](https://docs.microsoft.com/azure/kusto/query/best-practices) de Kusto: optimice las consultas para un mejor rendimiento.














 
