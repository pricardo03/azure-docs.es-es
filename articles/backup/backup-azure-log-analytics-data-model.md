---
title: Modelo de datos de los registros de Azure Monitor para copia de seguridad de Azure
description: Este artículo se habla Azure Monitor registra los detalles del modelo de datos para los datos de copia de seguridad de Azure.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: adigan
ms.openlocfilehash: dd4dad2cc3e541d3b6866c02341161dc1d9e1e6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61234980"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Modelo de datos de Log Analytics para datos de Azure Backup

Utilice el modelo de datos de Log Analytics para crear alertas personalizadas de Log Analytics.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-azure-backup-data-model"></a>Uso del modelo de datos de Azure Backup

Puede usar los siguientes campos que se proporcionan como parte del modelo de datos para crear objetos visuales, consultas personalizadas y paneles, según sus requisitos.

### <a name="alert"></a>Alerta

Esta tabla proporciona detalles acerca de los campos relacionados con la alerta.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| AlertUniqueId_s |Text |Identificador único de la alerta generada |
| AlertType_s |Text |Tipo de alerta, por ejemplo, Copia de seguridad |
| AlertStatus_s |Text |Estado de la alerta, por ejemplo, Activa |
| AlertOccurrenceDateTime_s |Fecha y hora |Fecha y hora en que se creó la alerta |
| AlertSeverity_s |Text |Gravedad de la alerta, por ejemplo, Crítica |
|AlertTimeToResolveInMinutes_s    | Number        |Tiempo necesario para resolver una alerta. Espacio en blanco para las alertas activas.         |
|AlertConsolidationStatus_s   |Text         |Identifica si la alerta es una alerta consolidada o no         |
|CountOfAlertsConsolidated_s     |Number         |Número de alertas consolidadas si se trata de una alerta consolidada          |
|AlertRaisedOn_s     |Text         |Tipo de entidad en que se genera la alerta         |
|AlertCode_s     |Text         |Código para identificar de forma única un tipo de alerta         |
|RecommendedAction_s   |Text         |Acción recomendada para resolver la alerta         |
| EventName_s |Text |Nombre del evento. Siempre AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Identificador único del elemento de copia de seguridad asociado con la alerta |
| SchemaVersion_s |Text |Versión actual del esquema, por ejemplo **V2** |
| State_s |Text |Estado actual del objeto de la alerta por ejemplo, Active o Deleted |
| BackupManagementType_s |Text |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder al que pertenece esta alerta |
| nombreOperación |Text |Nombre de la operación actual, por ejemplo, Alerta |
| Categoría |Text |Categoría de datos de diagnóstico se inserta en los registros de Azure Monitor. Siempre AzureBackupReport |
| Recurso |Text |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Text |Identificador único del servidor protegido asociado a la alerta |
| VaultUniqueId_s |Text |Identificador único del almacén protegido asociado a la alerta |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador único para el recurso sobre el que se recopilan los datos. Por ejemplo, el id. del recurso del almacén de Recovery Services |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="backupitem"></a>BackupItem

Esta tabla proporciona detalles acerca de los campos relacionados con el elemento de copia de seguridad.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Text |Nombre del evento. Siempre AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Identificador único del elemento de copia de seguridad |
| BackupItemId_s |Text |Identificador del elemento de copia de seguridad |
| BackupItemName_s |Text |Nombre de elemento de copia de seguridad |
| BackupItemFriendlyName_s |Text |Nombre descriptivo del elemento de copia de seguridad |
| BackupItemType_s |Texto |Tipo de elemento de copia de seguridad, por ejemplo, VM o FileFolder |
| ProtectionState_s |Text |Estado actual de protección del elemento copia de seguridad, por ejemplo, Protected o ProtectionStopped |
| ProtectionGroupName_s |Text | Nombre del grupo de protección del elemento de copia de seguridad está protegido, SC DPM y MABS, si procede|
| SecondaryBackupProtectionState_s |Text |Si está habilitada la protección secundaria para el elemento de copia de seguridad|
| SchemaVersion_s |Text |Versión del esquema, por ejemplo, **V2** |
| State_s |Text |Estado del objeto del elemento de copia de seguridad, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder al que pertenece este elemento de copia de seguridad |
| nombreOperación |Text |Nombre de la operación, por ejemplo, BackupItem |
| Categoría |Text |Categoría de datos de diagnóstico se inserta en los registros de Azure Monitor. Siempre AzureBackupReport |
| Recurso |Text |Recurso para el que se recopilan datos, por ejemplo, nombre de almacén de Recovery Services |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Id. de recurso para el que se recopilan datos, por ejemplo, id. de recurso del almacén de Recovery Services. |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para los datos que se recopilan |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para los datos que se recopilan |
| ResourceProvider |Text |Proveedor de recursos de datos que se recopilan, por ejemplo, Microsoft.RecoveryServices |
| ResourceType |Text |Tipo del recurso de datos que se recopilan, por ejemplo, los almacenes. |

### <a name="backupitemassociation"></a>BackupItemAssociation

Esta tabla proporciona detalles acerca de las asociaciones de elementos de copia de seguridad con varias entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Text |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |Identificador exclusivo del elemento de copia de seguridad |
| SchemaVersion_s |Text |Este campo indica la versión actual del esquema, es **V2** |
| State_s |Text |Estado actual del objeto de la asociación del elemento de copia de seguridad, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| BackupItemSourceSize_s |Text | Tamaño de front-end del elemento de copia de seguridad |
| BackupManagementServerUniqueId_s |Text | Campo para identificar de forma única el servidor de administración de copia de seguridad está protegido el elemento de copia de seguridad a través de, si procede |
| Categoría |Text |Este campo representa la categoría de datos de diagnóstico insertada en Log Analytics; es AzureBackupReport |
| nombreOperación |Text |Este campo representa el nombre de la operación actual: BackupItemAssociation |
| Recurso |Text |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Text |Identificador único para la política asociada al elemento de copia de seguridad. |
| ProtectedServerUniqueId_s |Text |Identificador único del servidor protegido asociado al elemento de copia de seguridad. |
| VaultUniqueId_s |Text |Identificador único del almacén que contiene el elemento de copia de seguridad. |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceProvider |Text |Proveedor de recursos de datos que se recopilan, por ejemplo, Microsoft.RecoveryServices |
| ResourceType |Text |Tipo del recurso de datos que se recopilan, por ejemplo, los almacenes. |

### <a name="backupmanagementserver"></a>BackupManagementServer

Esta tabla proporciona detalles acerca de las asociaciones de elementos de copia de seguridad con varias entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
|BackupManagementServerName_s     |Text         |Nombre del servidor de administración de copia de seguridad        |
|AzureBackupAgentVersion_s     |Text         |Versión de Azure Backup Agent en el servidor de administración de Backup          |
|BackupManagementServerVersion_s     |Text         |Versión del servidor de administración de copia de seguridad|
|BackupManagementServerOSVersion_s    |Text            |Versión del sistema operativo del servidor de administración de copia de seguridad|
|BackupManagementServerType_s     |Text         |Tipo del servidor de administración de copia de seguridad, como MABS, SC DPM|
|BackupManagementServerUniqueId_s     |Text         |Campo para identificar de forma única el servidor de administración de copia de seguridad       |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos. |
| ResourceProvider |Text |Proveedor de recursos de datos que se recopilan, por ejemplo, Microsoft.RecoveryServices |
| ResourceType |Text |Tipo del recurso de datos que se recopilan, por ejemplo, los almacenes. |

### <a name="job"></a>Trabajo

Esta tabla proporciona detalles acerca de los campos relacionados con los trabajos.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Text |Nombre del evento. Siempre AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |Identificador único del elemento de copia de seguridad |
| SchemaVersion_s |Text |Versión del esquema, por ejemplo, **V2** |
| State_s |Text |Estado actual del objeto del trabajo, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Text |Este campo representa el nombre de la operación actual: Job |
| Categoría |Text |Este campo representa la categoría de datos de diagnóstico que se insertan en los registros de Azure Monitor; es AzureBackupReport |
| Recurso |Text |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Text |Identificador único del servidor protegido asociado al trabajo. |
| ProtectedContainerUniqueId_s |Text | Identificador único para identificar el trabajo se ejecuta en el contenedor de protegido |
| VaultUniqueId_s |Text |Identificador único del almacén protegido. |
| JobOperation_s |Text |Operación para la que se ejecuta el trabajo, por ejemplo, Backup, Restore o Configure Backup |
| JobStatus_s |Text |Estado del trabajo terminado, por ejemplo, Completed o Failed |
| JobFailureCode_s |Text |Cadena del código de error por el que produjo el error del trabajo |
| JobStartDateTime_s |Fecha y hora |Fecha y hora en las que comenzó la ejecución del trabajo |
| BackupStorageDestination_s |Text |Destino de almacenamiento de almacenamiento de copias de seguridad, por ejemplo, Cloud o Disk  |
| AdHocOrScheduledJob_s |Text | Campo para especificar si el trabajo es Ad Hoc o programada |
| JobDurationInSecs_s | Number |Duración del trabajo total en segundos |
| DataTransferredInMB_s | Number |Datos transferidos en MB para este trabajo|
| JobUniqueId_g |Text |Identificador único que identifica el trabajo |
| RecoveryJobDestination_s |Text | Destino de un trabajo de recuperación, donde se recuperan los datos |
| RecoveryJobRPDateTime_s |DateTime | La fecha, hora cuando se creó el punto de recuperación que se va a recuperar. |
| RecoveryJobRPLocation_s |Text | La ubicación donde se almacenó el punto de recuperación que se va a recuperar.|
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services.|
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="policy"></a>Directiva

Esta tabla proporciona detalles acerca de los campos relacionados con las directivas.

| Campo | Tipo de datos | Versiones aplicables | DESCRIPCIÓN |
| --- | --- | --- | --- |
| EventName_s |Text ||Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Text ||Este campo indica la versión actual del esquema, es **V2** |
| State_s |Text ||Estado actual del objeto de la directiva, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text ||Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Text ||Este campo representa el nombre de la operación actual: Policy |
| Categoría |Text ||Este campo representa la categoría de datos de diagnóstico que se insertan en los registros de Azure Monitor; es AzureBackupReport |
| Recurso |Text ||Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Text ||Identificador único que identifica la directiva |
| PolicyName_s |Text ||Nombre de la directiva definida |
| BackupFrequency_s |Text ||Frecuencia con la que se ejecutan las copias de seguridad, por ejemplo, a diario o semanalmente |
| BackupTimes_s |Text ||Fecha y hora en que se programan las copias de seguridad |
| BackupDaysOfTheWeek_s |Text ||Días de la semana en los que se han programado copias de seguridad |
| RetentionDuration_s |Número entero ||Duración de retención de las copias de seguridad configuradas |
| DailyRetentionDuration_s |Número entero ||Duración de retención total de las copias de seguridad configuradas, en días |
| DailyRetentionTimes_s |Text ||Fecha y hora en que se configuró la retención diaria |
| WeeklyRetentionDuration_s |Número decimal ||Duración total de la retención semanal de las copias de seguridad configuradas, en semanas |
| WeeklyRetentionTimes_s |Text ||Fecha y hora en que se ha configurado la retención semanal |
| WeeklyRetentionDaysOfTheWeek_s |Text ||Días de la semana seleccionados para la retención semanal |
| MonthlyRetentionDuration_s |Número decimal ||Duración de retención total de las copias de seguridad configuradas, en meses |
| MonthlyRetentionTimes_s |Text ||Fecha y hora en que se ha configurado la retención mensual |
| MonthlyRetentionFormat_s |Text ||Tipo de configuración para la retención mensual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana |
| MonthlyRetentionDaysOfTheWeek_s |Text ||Días de la semana seleccionados para la retención mensual |
| MonthlyRetentionWeeksOfTheMonth_s |Text ||Semanas del mes en que se ha configurado la retención mensual, por ejemplo, la primera, la última, etc. |
| YearlyRetentionDuration_s |Número decimal ||Duración total de la retención de las copias de seguridad configuradas, en años |
| YearlyRetentionTimes_s |Text ||Fecha y hora en que se ha configurado la retención anual |
| YearlyRetentionMonthsOfTheYear_s |Text ||Meses del año seleccionados para la retención anual |
| YearlyRetentionFormat_s |Text ||Tipo de configuración para la retención anual ,por ejemplo, diariamente si se basa en día, semanalmente si se basa en semana | |
| YearlyRetentionDaysOfTheMonth_s |Text ||Fechas del mes seleccionadas para la retención anual |
| SynchronisationFrequencyPerDay_s |Número entero |v2|Número de veces en un día que se sincroniza una copia de seguridad para SC DPM y MABS |
| DiffBackupFormat_s |Text |v2|Formato para copias de seguridad diferenciales de SQL en la copia de seguridad de máquina virtual de Azure |
| DiffBackupTime_s |Hora |v2|Tiempo para copias de seguridad diferenciales de SQL en la copia de seguridad de máquina virtual de Azure|
| DiffBackupRetentionDuration_s |Número decimal |v2|Duración de retención de copias de seguridad diferenciales de SQL en la copia de seguridad de máquina virtual de Azure|
| LogBackupFrequency_s |Número decimal |v2|Frecuencia de las copias de seguridad para SQL|
| LogBackupRetentionDuration_s |Número decimal |v2|Duración de retención para copias de seguridad del registro de SQL en la copia de seguridad de máquina virtual de Azure|
| DiffBackupDaysofTheWeek_s |Text |v2|Días de la semana para copias de seguridad diferenciales de SQL en la copia de seguridad de máquina virtual de Azure|
| SourceSystem |Text ||Sistema de origen de los datos actuales: Azure |
| ResourceId |Text ||Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text ||Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text ||Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text ||Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text ||Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="policyassociation"></a>PolicyAssociation

Esta tabla proporciona detalles acerca de las asociaciones de directivas con varias entidades.

| Campo | Tipo de datos | Versiones aplicables | DESCRIPCIÓN |
| --- | --- | --- | --- |
| EventName_s |Text ||Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Text ||Este campo indica la versión actual del esquema, es **V2** |
| State_s |Text ||Estado actual del objeto de la directiva, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text ||Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Text ||Este campo representa el nombre de la operación actual: PolicyAssociation |
| Categoría |Text ||Este campo representa la categoría de datos de diagnóstico que se insertan en los registros de Azure Monitor; es AzureBackupReport |
| Recurso |Text ||Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| PolicyUniqueId_g |Text ||Identificador único que identifica la directiva |
| VaultUniqueId_s |Text ||Identificador exclusivo del almacén al que pertenece esta directiva |
| BackupManagementServerUniqueId_s |Text |v2 |Campo para identificar de forma única el servidor de administración de copia de seguridad está protegido el elemento de copia de seguridad a través de, si procede        |
| SourceSystem |Text ||Sistema de origen de los datos actuales: Azure |
| ResourceId |Text ||Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text ||Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text ||Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text ||Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text ||Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="protected-container"></a>Contenedor protegido

Esta tabla proporciona campos básicos acerca de los contenedores protegidos. (Era ProtectedServer en v1)

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Text | Campo para identificar de forma única un contenedor protegido |
| ProtectedContainerOSType_s |Text |Tipo de sistema operativo del contenedor protegido |
| ProtectedContainerOSVersion_s |Text |Versión del sistema operativo del contenedor protegido |
| AgentVersion_s |Text |Número de versión de agente de copia de seguridad o el agente de protección (en el caso de SC DPM y MABS) |
| BackupManagementType_s |Text |Tipo de proveedor para realizar la copia de seguridad, por ejemplo, IaaSVM o FileFolder |
| EntityState_s |Text |Estado actual del objeto de servidor protegido, por ejemplo, Active o Deleted |
| ProtectedContainerFriendlyName_s |Text |Nombre descriptivo del servidor protegido |
| ProtectedContainerName_s |Text |Nombre del contenedor protegido |
| ProtectedContainerWorkloadType_s |Text |Tipo del contenedor protegido de copia de seguridad, por ejemplo, IaaSVMContainer |
| ProtectedContainerLocation_s |Text |Si el contenedor protegido está ubicado en el entorno local o en Azure |
| ProtectedContainerType_s |Text |Si el contenedor protegido es un servidor o un contenedor |

### <a name="storage"></a>Almacenamiento

Esta tabla proporciona detalles acerca de los campos relacionados con el almacenamiento.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| CloudStorageInBytes_s |Número decimal |Almacenamiento de copia de seguridad en la nube utilizado por las copias de seguridad, calculados basándose en el último valor |
| ProtectedInstances_s |Número decimal |Número de instancias protegidas que se utilizan para calcular el almacenamiento de front-end en la facturación; se calcula en función del valor más reciente |
| EventName_s |Text |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Text |Este campo indica la versión actual del esquema, es **V2** |
| State_s |Text |Estado actual del objeto de almacenamiento, por ejemplo, Active o Deleted |
| BackupManagementType_s |Text |Tipo de proveedor para el servidor que realiza el trabajo de copia de seguridad, por ejemplo, IaaSVM o FileFolder. |
| nombreOperación |Text |Este campo representa el nombre de la operación actual: Storage |
| Categoría |Text |Este campo representa la categoría de datos de diagnóstico que se insertan en los registros de Azure Monitor; es AzureBackupReport |
| Recurso |Text |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| ProtectedServerUniqueId_s |Text |Id. exclusivo del servidor protegido para el que se calcula el almacenamiento |
| VaultUniqueId_s |Text |Se calcula el identificador único del almacén para el almacenamiento |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="storageassociation"></a>StorageAssociation

Esta tabla proporciona campos básicos relacionados con el almacenamiento de conexión de almacenamiento a otras entidades.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- |  --- |
| StorageUniqueId_s |Text |Identificador único usado para identificar la entidad de almacenamiento |
| SchemaVersion_s |Text |Este campo indica la versión actual del esquema, es **V2** |
| BackupItemUniqueId_s |Text |Identificador único usado para identificar el elemento de copia de seguridad relacionadas con la entidad de almacenamiento |
| BackupManagementServerUniqueId_s |Text |Identificador único usado para identificar el servidor de administración de copia de seguridad relacionadas con la entidad de almacenamiento|
| VaultUniqueId_s |Text |Identificador único usado para identificar el almacén relacionada con la entidad de almacenamiento|
| StorageConsumedInMBs_s |Number|Tamaño de almacenamiento usado por el elemento de copia de seguridad correspondiente en el almacenamiento correspondiente |
| StorageAllocatedInMBs_s |Number |Tamaño de almacenamiento asignado por el elemento de copia de seguridad correspondiente en el almacenamiento correspondiente del tipo de disco|

### <a name="vault"></a>Almacén

Esta tabla proporciona detalles acerca de los campos relacionados con el almacén.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| EventName_s |Text |Este campo representa el nombre de este evento; es siempre AzureBackupCentralReport |
| SchemaVersion_s |Text |Este campo indica la versión actual del esquema, es **V2** |
| State_s |Text |Estado actual del objeto del almacén por ejemplo, Active o Deleted |
| nombreOperación |Text |Este campo representa el nombre de la operación actual: Vault |
| Categoría |Text |Este campo representa la categoría de datos de diagnóstico que se insertan en los registros de Azure Monitor; es AzureBackupReport |
| Recurso |Text |Este es el recurso para el que se están recopilando datos; muestra el nombre del almacén de Recovery Services |
| VaultUniqueId_s |Text |Id. exclusivo del almacén |
| VaultName_s |Text |Nombre del almacén |
| AzureDataCenter_s |Text |Centro de datos donde se encuentra el almacén |
| StorageReplicationType_s |Text |Tipo de replicación de almacenamiento para el almacén, por ejemplo, GeoRedundant |
| SourceSystem |Text |Sistema de origen de los datos actuales: Azure |
| ResourceId |Text |Identificador de recursos de datos que se recopilan. Por ejemplo, el id. del recurso del almacén de Recovery Services. |
| SubscriptionId |Text |Identificador de la suscripción del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceGroup |Text |Grupo de recursos del recurso (p. ej., Almacén de Recovery Services) para el que se recopilan los datos |
| ResourceProvider |Text |Proveedor de recursos para el que se recopilan los datos. Por ejemplo, Microsoft.RecoveryServices. |
| ResourceType |Text |Tipo de recurso para el que se recopilan los datos. Por ejemplo: almacenes |

### <a name="backup-management-server"></a>Servidor de administración de copia de seguridad

Esta tabla proporciona campos básicos acerca de los servidores de administración de copia de seguridad.

|Campo  |Tipo de datos  | DESCRIPCIÓN  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Text         |Nombre del servidor de administración de copia de seguridad        |
|AzureBackupAgentVersion_s     |Text         |Versión de Azure Backup Agent en el servidor de administración de Backup          |
|BackupManagmentServerVersion_s     |Text         |Versión del servidor de administración de copia de seguridad|
|BackupManagmentServerOSVersion_s     |Text            |Versión del sistema operativo del servidor de administración de copia de seguridad|
|BackupManagementServerType_s     |Text         |Tipo del servidor de administración de copia de seguridad, como MABS, SC DPM|
|BackupManagmentServerUniqueId_s     |Text         |Campo para identificar de forma única el servidor de administración de copia de seguridad       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Esta tabla especifican las cargas de trabajo que está asociado a un volumen.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| StorageUniqueId_s |Text |Identificador único usado para identificar la entidad de almacenamiento |
| BackupItemType_s |Text |Las cargas de trabajo para el que este volumen es el almacenamiento preferido|

### <a name="protectedinstance"></a>ProtectedInstance

Esta tabla proporciona campos relacionados básico de instancias protegidas.

| Campo | Tipo de datos |Versiones aplicables | DESCRIPCIÓN |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Text |v2|Id. exclusivo usado para identificar el elemento de copia de seguridad de máquinas virtuales de una copia de seguridad con DPM, MABS|
| ProtectedContainerUniqueId_s |Text |v2|Identificador único usado para identificar el contenedor protegido para todo excepto las máquinas virtuales de una copia de seguridad con DPM, MABS|
| ProtectedInstanceCount_s |Text |v2|Recuento de instancias protegidas para el elemento de copia de seguridad asociado o un contenedor protegido en esa fecha y hora|

### <a name="recoverypoint"></a>RecoveryPoint

Esta tabla proporciona recuperación básica elija los campos relacionados.

| Campo | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| BackupItemUniqueId_s |Text |Id. exclusivo usado para identificar el elemento de copia de seguridad de máquinas virtuales de una copia de seguridad con DPM, MABS|
| OldestRecoveryPointTime_s |Text |Hora de fecha del punto de recuperación más antiguo para el elemento de copia de seguridad|
| OldestRecoveryPointLocation_s |Text |Ubicación del punto de recuperación más antiguo para el elemento de copia de seguridad|
| LatestRecoveryPointTime_s |Text |Hora de fecha del último punto de recuperación para el elemento de copia de seguridad|
| LatestRecoveryPointLocation_s |Text |Ubicación del punto de recuperación más reciente para el elemento de copia de seguridad|

## <a name="next-steps"></a>Pasos siguientes

Después de revisar el modelo de datos, puede iniciar [crear consultas personalizadas](../azure-monitor/learn/tutorial-logs-dashboards.md) en registros de Azure Monitor para crear su propio panel.
