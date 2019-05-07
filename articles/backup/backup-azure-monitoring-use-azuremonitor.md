---
title: 'Azure Backup: Supervisar copias de seguridad de Azure con Azure Monitor'
description: Supervisar las cargas de trabajo de copia de seguridad de Azure y crear alertas personalizadas con Azure Monitor
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Copia de seguridad de Azure; Alertas; Configuración de diagnóstico; Grupos de acciones
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 94fde7714f3efe0a460983966923071bce1afcc6
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190501"
---
# <a name="monitoring-at-scale-using-azure-monitor"></a>Supervisión a escala mediante Azure Monitor

El [integradas de supervisión y alerta artículo](backup-azure-monitoring-built-in-monitor.md) muestran la supervisión y generación de alertas en un único almacén de Recovery services y que se ofrece sin ninguna infraestructura de administración adicionales. Sin embargo, el servicio integrado está limitado en los escenarios siguientes.

- Datos de supervisión de varios almacenes de RS en las suscripciones
- Si el correo electrónico no es el canal de notificación preferido
- Si los usuarios desean recibir un aviso para los escenarios más
- Si desea ver la información del componente en el entorno local, como System Center DPM (SC DPM) en Azure, que no se muestra en [trabajos de copia de seguridad](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) o [alertas de copia de seguridad](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) en el portal.

## <a name="using-log-analytics-workspace"></a>Usar el área de trabajo de Log Analytics

> [!NOTE]
> Datos de la máquina virtual de Azure las copias de seguridad, el agente de MAB, System Center DPM (SC DPM), las copias de seguridad SQL en Azure Virtual Machines es que se bombean al área de trabajo de Log Analytics a través de la configuración de diagnóstico. Compatibilidad con copias de seguridad del recurso compartido de archivos de Azure, Microsoft Azure Backup Server (MABS) estará disponible próximamente.

Estamos aprovechando las capacidades de dos servicios de Azure - **configuración de diagnóstico** (para enviar datos de varios recursos de Azure Resource Manager a otro recurso) y **Log Analytics** (LA - generar alertas personalizadas donde puede definir otros canales de notificación mediante grupos de acción) para la supervisión a escala. La secciones siguientes se explica en cómo usar LA para supervisar la copia de seguridad de Azure a escala.

### <a name="configuring-diagnostic-settings"></a>Configuración de diagnóstico

Un recurso de Azure Resource Manager como almacén de Azure Recovery services registra toda la información posible acerca de las operaciones programadas y las operaciones de usuario que se desencadena como datos de diagnóstico. Haga clic en "configuración de diagnóstico" en la sección de supervisión y especifique el destino de datos de diagnóstico del almacén RS.

![Configuración de diagnóstico de almacén RS co como destino](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Puede seleccionar un área de trabajo de LA de otra suscripción como el destino. *Al seleccionar el área de trabajo de LA misma para varios almacenes de RS, puede supervisar los almacenes en las suscripciones en un solo lugar.* Seleccione 'AzureBackupReport' como el registro de todos los canales información al área de trabajo LA relacionada con la copia de seguridad de Azure.

> [!IMPORTANT]
> Después de completar la configuración, debe esperar 24 horas para que finalice la inserción de datos inicial. A partir de entonces todos los eventos se insertan como se mencionó en el [sección frecuencia](#diagnostic-data-update-frequency).

### <a name="deploying-solution-to-log-analytics-workspace"></a>Implementar la solución al área de trabajo de Log Analytics

Una vez que los datos se están dentro del área de trabajo de LA [implementar una plantilla de github](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) en LA para visualizar los datos. Asegúrese de que asignar el mismo grupo de recursos, el nombre de área de trabajo y ubicación del área de trabajo para identificar correctamente el área de trabajo y, a continuación, instalar esta plantilla en ella.

### <a name="view-azure-backup-data-using-log-analytics-la"></a>Ver los datos de copia de seguridad de Azure mediante Log Analytics (LA)

Una vez implementada la plantilla, la solución para la supervisión de Azure Backup se mostrará en la región de resumen del área de trabajo. Puede recorrer a través de

- Azure Monitor -> "Más" en la sección "Insights" y elija el área de trabajo relevante o
- Áreas de trabajo de log Analytics -> seleccione el área de trabajo relevante -> 'Del área de trabajo resumen' en la sección General.

![AzureBackupLAMonitoringTile](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Al hacer clic en el icono, la plantilla de diseñador se abre una serie de gráficos sobre los datos de supervisión básicos de copia de seguridad de Azure, como

#### <a name="all-backup-jobs"></a>Todos los trabajos de copia de seguridad

![LABackupJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

#### <a name="restore-jobs"></a>Trabajos de restauración

![LARestoreJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

#### <a name="inbuilt-azure-backup-alerts-for-azure-resources"></a>Alertas de copia de seguridad de Azure integradas para los recursos de Azure

![LAInbuiltAzureBackupAlertsForAzureResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

#### <a name="inbuilt-azure-backup-alerts-for-on-prem-resources"></a>Alertas de copia de seguridad de Azure integradas para recursos locales

![LAInbuiltAzureBackupAlertsForOnPremResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

#### <a name="active-datasources"></a>Orígenes de datos activos

![LAActiveBackedUpEntities](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

#### <a name="rs-vault-cloud-storage"></a>Almacenamiento de almacén RS en la nube

![LARSVaultCloudStorage](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Los gráficos anteriores se proporcionan con la plantilla y el cliente es libre para agregar y editar gráficos más.

> [!IMPORTANT]
> Al implementar la plantilla, básicamente se crea un bloqueo de solo lectura y deberá quitarlo para editar la plantilla y guardar. Para quitar los bloqueos, busque en el panel "Bloqueos" en la sección "Configuración" del área de trabajo de Log Analytics.

### <a name="create-alerts-using-log-analytics"></a>Creación de alertas mediante Log Analytics

Azure Monitor permite a los usuarios crear sus propias alertas del área de trabajo de LA que puede *aprovechar los grupos de acciones de Azure para seleccionar el mecanismo de notificación preferido*. Haga clic en cualquiera de los gráficos anteriores para abrir la sección 'Registra' del área de trabajo de LA ***donde puede editar las consultas y crear alertas sobre ellos***.

![LACreateAlerts](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Al hacer clic en "Nueva regla de alerta" como se indicó anteriormente, se abrirá la pantalla de creación de alertas de Azure Monitor.

Como puede observar a continuación, el recurso ya está marcado como el área de trabajo de LA y se proporciona integración de grupos de acción.

![LAAzureBackupCreateAlert](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

> [!IMPORTANT]
> Tenga en cuenta que se proporciona el impacto de precios correspondiente de la creación de esta consulta [aquí](https://azure.microsoft.com/pricing/details/monitor/).

#### <a name="alert-condition"></a>Condición de alerta

El aspecto clave es la condición desencadenadora de la alerta. Al hacer clic en "Condiciones" cargará automáticamente la consulta de Kusto en la pantalla "Logs", tal como se muestra a continuación y puede modificarlo para adaptarlo a su escenario. En el que se proporcionan algunos ejemplos de consultas de Kusto el [siguiente sección](#sample-kusto-queries).

![LAAzureBackupAlertCondition](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Edite la consulta Kusto, si es necesario, seleccione el umbral adecuado (que decidirá cuando se desencadena la alerta), el período de la derecha (ventana de tiempo para el que se ejecuta la consulta) y la frecuencia. Por ejemplo:  Si el umbral es mayor que 0, el período es de 5 minutos y la frecuencia es de 5 minutos, a continuación, la regla se traduce como "Ejecutar la consulta cada 5 minutos durante los últimos 5 minutos y si el número de resultados es mayor que 0, notificarme vía el grupo de acciones seleccionado"

#### <a name="action-group-integration"></a>Integración de grupos de acción

Grupos de acciones especifican los canales de notificación disponibles para el usuario. Al hacer clic en "Crear nuevo" en "Grupos de acciones" sección muestra la lista de los mecanismos de notificación disponibles.

![LAAzureBackupNewActionGroup](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Obtenga más información sobre la [alertas del área de trabajo de LA](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) y aproximadamente [grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) de documentación de Azure Monitor.

Por lo tanto, puede satisfacer todas las alertas y supervisión de los requisitos de por sí solo LA o utilizarlo como una técnica con mecanismos de notificación integrados.

### <a name="sample-kusto-queries"></a>Ejemplos de consultas de Kusto

Los gráficos predeterminado daría a las consultas de Kusto para los escenarios básicos en el que puede generar alertas. También puede modificarlas para obtener los datos que desea recibir alertas sobre. Aquí se proporcionan algunos ejemplos de consultas de Kusto que se pueden pegar en la ventana "Logs" y, a continuación, creación una alerta en la consulta.

#### <a name="all-successful-backup-jobs"></a>Todos los trabajos de copia de seguridad correcta

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Completed"
````

#### <a name="all-failed-backup-jobs"></a>Error de todos los trabajos de copia de seguridad

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Failed"
````

#### <a name="all-successful-azure-vm-backup-jobs"></a>Todos los trabajos de copia de seguridad de máquina virtual de Azure correcta

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

#### <a name="all-successful-sql-log-backup-jobs"></a>Todos los trabajos de copia de seguridad de registro de SQL correcta

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

#### <a name="all-successful-mab-agent-backup-jobs"></a>Todos los trabajos de copia de seguridad del agente de MAB correcta

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

### <a name="diagnostic-data-update-frequency"></a>Frecuencia de actualización de datos de diagnóstico

Los datos de diagnóstico desde el almacén se bombean al área de trabajo LA con algún retraso. Cada evento llega al área de trabajo LA ***con un retraso de 20 a 30 minutos después de se inserta desde el almacén RS.***

- Las alertas de servicio de copia de seguridad integradas (a través de todas las soluciones) se insertan en cuanto se crean. Lo que significa que normalmente aparecen en el área de trabajo de LA después de un retraso de 20 a 30 minutos.
- Los trabajos de copia de seguridad ad hoc y restauración (a través de todas las soluciones) se insertan tan pronto como ***se completan***.
- Los trabajos programados de copia de seguridad de todas las soluciones (excepto la copia de seguridad SQL) se insertan tan pronto como ***se completan***.
- Para copia de seguridad SQL, ya que podemos hacer copias de seguridad de registros cada 15 minutos, para todos los el trabajo programado completado copia de seguridad, incluidos registros, la información es por lotes y se inserta cada 6 horas.
- Toda la demás información como el elemento de copia de seguridad, directiva, los puntos de recuperación, etc. a través de todas las soluciones de almacenamiento se insertan **al menos una vez al día.**
- Un cambio en la configuración de copia de seguridad como el cambio de edición de directivas, etc. de la directiva desencadena una inserción de todos ellos relacionados con información de copia de seguridad.

## <a name="using-rs-vaults-activity-logs"></a>Uso del almacén RS los registros de actividad

También puede usar los registros de actividad para recibir notificación de eventos, como la copia de seguridad correcta.

> [!CAUTION]
> **Tenga en cuenta que esto solo es aplicable para las copias de seguridad de máquina virtual de Azure.** No se puede utilizar esto para otras soluciones como el agente de copia de seguridad de Azure, las copias de seguridad SQL dentro de Azure, etcetera de archivos de Azure.

### <a name="sign-in-into-azure-portal"></a>Inicie sesión en Azure portal

Inicie sesión en el portal de Azure y continuar con el almacén de Recovery Services pertinente y haga clic en la sección "Registro de actividad" en las propiedades.

### <a name="identify-appropriate-log-and-create-alert"></a>Identificar el registro adecuado y crear alerta

Aplique los filtros que se muestran en la ilustración siguiente para comprobar si va a recibir registros de actividad por las copias de seguridad correctas. Cambie el intervalo de tiempo en consecuencia para ver los registros.

![Registros de actividad de las copias de seguridad de máquina virtual de Azure](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

Puede hacer clic en el segmento "JSON" para obtener más detalles; para verlo, cópielo y péguelo en un editor de texto. Deben mostrar los detalles del almacén y el elemento que desencadenó la actividad de registro, es decir, el elemento de copia de seguridad.

A continuación, haga clic en "Agregar alerta de registro de actividad" para generar alertas para todos los registros de este tipo.

Puede hacer clic en "Alerta de registro de actividad agregar" anterior y se abrirá la pantalla de creación de la alerta es similar a la pantalla de creación de la alerta [como se describió anteriormente](#create-alerts-using-log-analytics).

Aquí el recurso es el propio almacén RS y, por tanto, deberá repetir la misma acción para todos los almacenes en el que desea que la notificación a través de los registros de actividad. La condición no tendrán ningún umbral, el período, la frecuencia porque se trata de una alerta basada en eventos. Tan pronto como se genera el registro de actividad correspondiente, se desencadena la alerta.

## <a name="recommendation"></a>Recomendación

***Crear todas las alertas de registros de actividad y las áreas de trabajo de LA que pueden verse en Azure Monitor en el panel "Alertas" a la izquierda.***

Aunque se puede usar la notificación a través de los registros de actividad, ***servicio Azure Backup se recomienda encarecidamente utilizar LA para la supervisión a escala y no registros de actividad por las razones siguientes***.

- **Escenarios limitados:** Aplicable sólo a las copias de seguridad de máquina virtual de Azure y debe repetirse para cada almacén RS.
- **Ajustar la definición:** No se ajusta con la definición más reciente de los registros de actividad y se alinea con la actividad de copia de seguridad programada [los registros de diagnóstico](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-are-azure-monitor-diagnostic-logs). Esto ocasionaba un impacto inesperado cuando se cambian el bombeo a través de canal de registro de actividad de datos como se indicó a continuación.
- **Problemas con el canal de registro de actividad:** Hemos pasado a un nuevo modelo de suministro de los registros de actividad a partir de Azure Backup en almacenes de Recovery Services. Lamentablemente, el movimiento ha afectado a la generación de registros de actividad en las nubes soberanas de Azure. Si los usuarios en la nube Soberana de Azure crean y habían configurado las alertas de registros de actividad a través de Azure Monitor, no se desencadenaría. Además, en todas las regiones públicas de Azure, si un usuario está recopilando los registros de actividad de Recovery Services en un área de trabajo de Log Analytics, como se menciona [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), estos registros también podrían no aparecer.

Por lo tanto, se recomienda encarecidamente usar el área de trabajo de Log Analytics para la supervisión y las alertas de escalado para la copia de seguridad de Azure de cargas de trabajo protegidas.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [modelo de datos de análisis de registro](backup-azure-log-analytics-data-model.md) para crear consultas personalizadas.
