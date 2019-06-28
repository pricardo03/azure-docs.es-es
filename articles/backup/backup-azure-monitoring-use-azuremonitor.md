---
title: 'Azure Backup: Supervisión de Azure Backup con Azure Monitor'
description: Supervise las cargas de trabajo de Azure Backup y cree alertas personalizadas mediante Azure Monitor
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure Backup; Alertas; Configuración de diagnóstico; Grupos de acciones
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 1e85b633024b5a3e85874707ae9a1f068e7a328d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808527"
---
# <a name="monitoring-at-scale-using-azure-monitor"></a>Supervisión a escala mediante Azure Monitor

En el [artículo acerca de alertas y supervisión integradas](backup-azure-monitoring-built-in-monitor.md) se enumeraban las funcionalidades de supervisión y generación de alertas en un único almacén de Recovery Services y que se ofrece sin ninguna infraestructura de administración adicional. Sin embargo, el servicio integrado está limitado en los escenarios siguientes.

- Supervisión de datos de varios almacenes de Recovery Services en las suscripciones
- Si el correo electrónico NO es el canal de notificación preferido
- Si los usuarios desean recibir alertas en más escenarios
- Si desea ver la información de un componente local, como System Center DPM (SC DPM) en Azure, que no se muestra en [Trabajos de copia de seguridad](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) o [Alertas de copia de seguridad](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) en el portal.

## <a name="using-log-analytics-workspace"></a>Uso del área de trabajo de Log Analytics

> [!NOTE]
> Los datos de las copias de seguridad de máquinas virtuales de Azure, el agente de MAB, System Center DPM (SC DPM) y las copias de seguridad de SQL de las máquinas virtuales de Azure se bombean al área de trabajo de Log Analytics a través de la configuración del diagnóstico. La compatibilidad con la realización de copias de seguridad de recursos compartido de Azure, Microsoft Azure Backup Server (MABS) estará disponible próximamente.

Vamos a sacar provecho de las funcionalidades de dos servicios de Azure: **Configuración de diagnóstico**  (para enviar datos de varios recursos de Azure Resource Manager a otro recurso) y **Log Analytics** (LA: para generar alertas personalizadas en las que puede definir otros canales de notificación mediante grupos de acción) para la supervisión a escala. En las secciones siguientes se explica cómo usar LA para supervisar Azure Backup a escala.

### <a name="configuring-diagnostic-settings"></a>Configuración de valores de diagnóstico

Un recurso de Azure Resource Manager como un almacén de Azure Recovery Services registra toda la información posible acerca de las operaciones programadas y las operaciones desencadenadas por el usuario como datos de diagnóstico. Haga clic en "Configuración de diagnóstico" en la sección de supervisión y especifique el destino de los datos de diagnóstico del almacén de RS.

![Configuración de diagnóstico del almacén de RS con LA como destino](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Puede seleccionar un área de trabajo de LA de otra suscripción como el destino. *Si se selecciona el mismo área de trabajo de LA para varios almacenes de RS, es posible supervisar los almacenes de varias suscripciones en un solo lugar.* Seleccione "AzureBackupReport" como el registro parta canalizar toda la información relacionada de Azure Backup al área de trabajo de LA.

> [!IMPORTANT]
> Después de completar la configuración, debe esperar 24 horas para que finalice la inserción de datos inicial. A partir de ese momento todos los eventos se insertan como se indicó la [sección acerca de la frecuencia](#diagnostic-data-update-frequency).

### <a name="deploying-solution-to-log-analytics-workspace"></a>Implementación de la solución en el área de trabajo de Log Analytics

Una vez que los datos estén dentro del área de trabajo de LA [implemente una plantilla de GitHub](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) en LA para visualizar los datos. Asegúrese de asignar el mismo grupo de recursos, nombre de área de trabajo y ubicación de área de trabajo para identificar correctamente el área de trabajo y, después, instale esta plantilla en dicho área.

### <a name="view-azure-backup-data-using-log-analytics-la"></a>Visualización de los datos de Azure Backup mediante Log Analytics (LA)

Una vez implementada la plantilla, la solución para la supervisar Azure Backup se mostrará en la región resumen del área de trabajo. Se puede recorrer a través de

- Azure Monitor -> "Más" en la sección "Insights" y elija el área de trabajo relevante O BIEN
- Áreas de trabajo de Log Analytics -> seleccione el área de trabajo relevante -> "Resumen del área de trabajo" en la sección General.

![AzureBackupLAMonitoringTile](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Al hacer clic en el icono, la plantilla del diseñador abre una serie de gráficos acerca de los datos de supervisión básicos de Azure Backup, como

#### <a name="all-backup-jobs"></a>Todos los trabajos de copia de seguridad

![LABackupJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

#### <a name="restore-jobs"></a>Trabajos de restauración

![LARestoreJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

#### <a name="inbuilt-azure-backup-alerts-for-azure-resources"></a>Alertas de Azure Backup integradas para los recursos de Azure

![LAInbuiltAzureBackupAlertsForAzureResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

#### <a name="inbuilt-azure-backup-alerts-for-on-prem-resources"></a>Alertas de Azure Backup integradas para recursos locales

![LAInbuiltAzureBackupAlertsForOnPremResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

#### <a name="active-datasources"></a>Orígenes de datos activos

![LAActiveBackedUpEntities](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

#### <a name="rs-vault-cloud-storage"></a>Almacenamiento en la nube del almacén de Recovery Services

![LARSVaultCloudStorage](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Los gráficos anteriores se incluyen con la plantilla y el cliente es libre de agregar o editar más gráficos.

> [!IMPORTANT]
> Al implementar la plantilla, básicamente se crea un bloqueo de solo lectura, que debe eliminarse para editar la plantilla y guardarla. Para quitar los bloqueos, vaya al panel "Bloqueos" en la sección "Configuración" del área de trabajo de Log Analytics.

### <a name="create-alerts-using-log-analytics"></a>Creación de alertas mediante Log Analytics

Azure Monitor permite a los usuarios crear sus propias alertas desde el área de trabajo de LA, donde puede *sacar provecho de los grupos de acciones de Azure para seleccionar el mecanismo de notificación que prefiera*. Haga clic en cualquiera de los gráficos anteriores para abrir la sección 'Registros' del área de trabajo de LA ***en la que puede editar las consultas y crear alertas sobre ellas***.

![LACreateAlerts](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Si hace clic en "Nueva regla de alerta" como se indicó anteriormente, se abrirá la pantalla de creación de alertas de Azure Monitor.

Como puede ver a continuación, el recurso ya está marcado como área de trabajo de LA y se ha realizado la integración del grupos de acciones.

![LAAzureBackupCreateAlert](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

> [!IMPORTANT]
> Observe que el impacto en el precio que tiene crear esta consulta se proporciona [aquí](https://azure.microsoft.com/pricing/details/monitor/).

#### <a name="alert-condition"></a>Condición de alerta

El aspecto clave es la condición desencadenadora de la alerta. Al hacer clic en "Condición" se cargará automáticamente la consulta de Kusto en la pantalla "Registros", como se muestra a continuación, que, posteriormente, se puede editar para adaptarla a un escenario concreto. En la [sección siguiente](#sample-kusto-queries) se proporcionan algunas consultas de Kusto de ejemplo.

![LAAzureBackupAlertCondition](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Si fuera necesario, edite la consulta de Kusto, seleccione el umbral adecuado (que decidirá cuando se va a desencadenar la alerta), el período apropiado (periodo de tiempo durante el que se ejecuta la consulta) y la frecuencia. Por ejemplo:  Si el umbral es mayor que 0, el período es de 5 minutos y la frecuencia es de 5 minutos, la regla se traduce como "Ejecutar la consulta cada 5 minutos durante los últimos 5 minutos y si el número de resultados es mayor que 0, enviarme una notificación a través del grupo de acciones seleccionado"

#### <a name="action-group-integration"></a>Integración de los grupos de acciones

Los grupos de acciones especifican los canales de notificación disponibles para el usuario. Al hacer clic en "Crear nuevo" en la sección "Grupos de acciones", se muestra la lista de los mecanismos de notificación disponibles.

![LAAzureBackupNewActionGroup](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Para más información acerca de las [alertas del área de trabajo de LA](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) y de los [grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups), consulte la documentación de Azure Monitor.

Por tanto, puede satisfacer todos los requisitos de generación de alertas y supervisión LA de forma independiente, o bien utilizarlo como una técnica complementaria para los mecanismos de notificación integrados.

### <a name="sample-kusto-queries"></a>Ejemplos de consultas de Kusto

Los gráficos predeterminados le proporcionarían consultas de Kusto para los escenarios básicos en los que se pueden crear alertas. También puede modificarlas para obtener los datos de los que desee recibir alertas. Aquí se proporcionan algunos ejemplos de consultas de Kusto que se pueden pegar en la ventana "Registros" y, después, crear una alerta en la consulta.

#### <a name="all-successful-backup-jobs"></a>Todos los trabajos de copia de seguridad con resultado satisfactorio

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Completed"
````

#### <a name="all-failed-backup-jobs"></a>Todos los trabajos de copia de seguridad con errores

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Failed"
````

#### <a name="all-successful-azure-vm-backup-jobs"></a>Todos los trabajos de copia de seguridad de máquinas virtuales de Azure con resultado satisfactorio

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-sql-log-backup-jobs"></a>Todos los trabajos de copia de seguridad del registro de SQL con resultado satisfactorio

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-mab-agent-backup-jobs"></a>Todos los trabajos de copia de seguridad del agente MAB Azure con resultado satisfactorio

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

### <a name="diagnostic-data-update-frequency"></a>Frecuencia de actualización de los datos de diagnóstico

Los datos de diagnóstico del almacén se bombean al área de trabajo de LA con cierto retraso. Todos los eventos llegan al área de trabajo de LA ***con un retraso que oscila entre 20 y 30 minutos después de se insertan desde el almacén de RS.***

- Las alertas integradas del servicio de copia de seguridad (en todas las soluciones) se insertan en cuanto se crean. Esto significa que normalmente aparecen en el área de trabajo de LA entre 20 y 30 minutos después.
- Tanto los trabajos de copia de seguridad ad hoc como los trabajos de restauración (en todas las soluciones) se insertan en cuando ***se completan***.
- Los trabajos de copia de seguridad programados de todas las soluciones (excepto la copia de seguridad SQL) se insertan en cuanto ***se completan***.
- En el caso de la copia de seguridad de SQL, como no podemos tener copias de seguridad de registros cada 15 minutos, de todos los trabajos de copia de seguridad programados completados, incluidos los registros, la información se une en lotes y se inserta cada 6 horas.
- El resto de información como el elemento de copia de seguridad, la directiva, los puntos de recuperación, el almacenamiento, etc. de todas las soluciones se inserta **al menos una vez al día.**
- Un cambio en la configuración de copia de seguridad como por ejemplo un cambio de directiva, la edición de la directiva, etc. desencadena la inserción de toda la información de copia de seguridad relacionada.

## <a name="using-rs-vaults-activity-logs"></a>Uso de los registros de actividad del almacén de RS

Los registros de actividad también se pueden usar para recibir notificaciones de eventos, como copias de seguridad correctas.

> [!CAUTION]
> **Tenga en cuenta que esto solo se puede aplicar a las copias de seguridad de las máquinas virtuales de Azure.** No se puede utilizar esto para otras soluciones como Azure Backup Agent, las copias de seguridad de SQL dentro de Azure, Azure Files,etc.

### <a name="sign-in-into-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en Azure Portal, vaya al almacén de Azure Recovery Services relevante y haga clic en la sección "Registro de actividad" de las propiedades.

### <a name="identify-appropriate-log-and-create-alert"></a>Identificación del registro adecuado y creación de una alerta

Aplique los filtros que se muestran en la ilustración siguiente para comprobar si va a recibir registros de actividad por las copias de seguridad correctas. Cambie el intervalo de tiempo en consecuencia para ver los registros.

![Registros de actividad de las copias de seguridad de máquinas virtuales de Azure](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

Haga clic en el nombre de la operación. Se mostrará la operación y los detalles relevantes.

![Nueva alerta de reglas](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Haga clic en **Nueva regla de alerta** para abrir la pantalla **Crear regla**, donde puede crear alertas mediante los pasos que se describen en este [artículo](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

Aquí el recurso es el propio almacén de Recovery Services y, por consiguiente, es preciso que repita la misma acción en todos los almacenes en los que desee notificación a través de los registros de actividad. La condición no tendrá ningún umbral, período ni frecuencia, ya que se trata de una alerta basada en evento. En cuanto se genera el registro de actividad relevante, se desencadena la alerta.

## <a name="recommendation"></a>Recomendación

***Todas las alertas creadas a partir de los registros de actividad y las áreas de trabajo de LA se pueden ver en el panel "Alertas" de en Azure Monitor, a la izquierda.***

Aunque se puede usar la notificación a través de los registros de actividad, el ***servicio Azure Backup recomienda encarecidamente utilizar LA para la supervisión a escala, EN LUGAR DE los registros de actividad por los siguientes motivos***.

- **Escenarios limitados:** Solo se puede aplicar a las copias de seguridad de máquinas virtuales de Azure y debe repetirse en cada almacén de RS.
- **Ajuste de definición:** La actividad de copia de seguridad programada no se ajusta a la definición más reciente de los registros de actividad y se alinea con los [registros de diagnóstico](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs), lo que provoca un impacto inesperado cuando los datos que llegan a través del canal del registro de actividad se cambian como se indica a continuación.
- **Problemas con el canal del registro de actividad:** Hemos pasado a un nuevo modelo de suministro de los registros de actividad a partir de Azure Backup en almacenes de Recovery Services. Lamentablemente, el movimiento ha afectado a la generación de registros de actividad en las nubes soberanas de Azure. Si los usuarios de las nubes soberanas de Azure crearan y configuraran alertas de los registros de actividad a través de Azure Monitor, no se desencadenarían. Además, en todas las regiones públicas de Azure, si un usuario está recopilando los registros de actividad de Recovery Services en un área de trabajo de Log Analytics, como se menciona [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), estos registros también podrían no aparecer.

Por consiguiente, se recomienda encarecidamente usar el área de trabajo de Log Analytics para la supervisión y generación de alertas a escala en todas las cargas de trabajo protegidas por Azure Backup.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Modelo de datos de Log Analytics](backup-azure-log-analytics-data-model.md) para crear consultas personalizadas.
